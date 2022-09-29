---
title: CAPEC 100 - Overflow Buffers
categories: [Ethical Hacking, CAPEC]
tag: [buffer overflow]
comments: true
---
# Overview

Buffer overflow vulnerabilities are commonly targeted by exploiting buffer sizes. For example, if a buffer is set to allow 8 bytes however 10 are pushed to the buffer, the bytes can overflow into the next buffer. Below is a simple example depicting two buffers with a size of 8 bytes each. The second step depicts the push of 10 bytes to buffer 1, followed by the resulting buffer overflow of the extra two bytes that are ultimately pushed into buffer 2. Although the example depicts only 10 bytes being pushed to the buffer, this number can be increased and cause an overflow to multiple areas of memory that are located after the buffer. Buffer overflow will often result in an application crashing as integral data is overwritten by the overflow data.

    ```C
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

      - Using the `generic_send_tcp`, each buffer can be sent a large input in an attempt to identify buffer overflow vulnerabilities. The tool requires a `spike_script` to be compiled before running against an application. The generic command parameter to run the tool is `generic_send_tcp host port spike_script SKIPVAR SKIPSTR`. The spike_script should contain the following information, the %INPUT% field should be replaced with the application input being tested.
        
        ```C
        s_readline();
        s_string("%INPUT% ");
        s_string_variable("0");
        ```

      - Running the test on `TRUN` identified a buffer overflow vulnerability. The below screenshot shows the evidence via immunity debugger. Note that the value `A` is repeated in register `EAX` and has overflowed into additional registers `ESP`, `EBP`, and `EIP`. The values of `EBP` and `EIP` are expressed in Hex equivalent (A = 41).

        ![Immunity Debugger - Buffer Overflow Example](/assets/img/posts/ETH/CAPEC/100_Experiment_ImmunityDBG.png "Immunity Debugger - Buffer Overflow Example")

    - **Craft overflow content:**
      - Having identified that `TRUN` is vulnerable, the next step is to identify the exact byte count at which the the application crash occurs. A fuzzing script can be written and used against `TRUN` to step the fuzzing process, the below python script can be used to achieve this.

        ```python
        #!/usr/bin/python
        import sys, socket
        from time import sleep

        ############### fuzzing script ##################

        buffer = "A" * 100

        while True:
            try:

                s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
                s.connect(('172.16.70.134',9999))

                s.send(('TRUN /.:/' + buffer))
                s.close
                sleep(1)
                buffer = buffer + "A"*100

            except:
                print "Fuzzing crashed at %s bytes" % str(len(buffer))
                sys.exit()
        ```
        
        Once the script has completed, the byte count will be printed to the terminal.

# Sources
- [TCM Academy - Practical Ethical Hacking](https://academy.tcm-sec.com/)
- [Hacking - The Art of Exploitation 2nd Edition](https://www.amazon.com.au/Hacking-Art-Exploitation-Jon-Erickson/dp/1593271441)
- [OWASP - Buffer Overflow Attack](https://owasp.org/www-community/attacks/Buffer_overflow_attack)
- [Catharsis - Basic Buffer Overflow Guide](https://catharsis.net.au/blog/basic-buffer-overflow-guide/)
---
title: Evidence of External Device Activity
categories: [DFIR, Evidence Artifacts]
tags: []
comments: true
---
Techniques that can be used to discover evidence in support of incidents where removeable devices were involved.

# Windows

## USB Key Identification
Track USB devices plugged into a machine.

**WIN:** XP+ <br>
**SRV:** NULL

### Location
```plaintext
HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR

HKLM\SYSTEM\CurrentControlSet\Enum\USB
```

### Interpretation and Investigative Notes
- Identify vendor, product, and version of a USB device plugged into a machine.
- Identify a unique USB device plugged into the machine
- Determine the time a device was plugged into the machine
- Devices that do not have a unique serial number will have an "&" in the second character of the serial number.
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)

### Sources
- [Magnet Forensics - Artifact Profile USB Devices](https://www.magnetforensics.com/blog/artifact-profile-usb-devices/)

## Plug and Play (PnP) Events
When a PnP driver is initiated, the service will log an event and provide status details. It is important to note that this event will trigger for any PnP device, USB, Firewire, PCMIA, etc.

**WIN:** XP+ <br>
**SRV:** NULL

### Location
```plaintext
# WINDOWS XP
C:\Windows\setupapi.log

# WINDOWS 7+
C:\Windows\setupapi.dev.log

HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR\Ven_Prod_Ver\USB_Serial_#\Properties{83da6326-####-####-####-############}####

%SYSTEM ROOT%\System32\winevt\logs\System.evtx
```

### Interpretation and Investigative Notes
- `HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR\Ven_Prod_Ver\USB_Serial_#\Properties{83da6326-####-####-####-############}####`
  - Event IDs (Last portion of Registry Key entry as represented by `\####`)
    - **0064:** First Install (Win7+)
    - **0066:** Last Connected (Win8+)
    - **0067:** Last Removal (Win8+)
- `%SYSTEM ROOT%\System32\winevt\logs\System.evtx`
  - Event IDs
    - **200001:** PnP Driver Install Attempted
      - Includes following information
        - Timestamp
        - Device Information
        - Device Serial Number
        - Status (0 = No Errors)
- Search for device serial number.
- Log files are set to local times
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- [Event Log Explorer](https://www.eventlogxp.com/)
- [Event Log Parser (EvtxECmd)](https://github.com/EricZimmerman/evtx)
- Native Event Viewer

### Sources
- [Magnet Forensics - Artifact Profile USB Devices](https://www.magnetforensics.com/blog/artifact-profile-usb-devices/)

## Account Responsible
Identify the user that used the USB device

**WIN:** XP+ <br>
**SRV:** NULL

### Location
```plaintext
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2
```

### Interpretation and Investigative Notes
- Look for the GUID from the registry key below
  - `HKLM\SYSTEM\MountedDevices`
- The above GUID will be used to identify the user that plugged in the device. The last write time of this key also corresponds to the last write time the device was plugged into the machine by that user. The number will be references in the number in the MountPoint registry key in the users `NTUSER.DAT` file.
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)

### Sources
- [Hats Off Security - USB Forensics Pt6 Which User Account Used the USB Device?](https://hatsoffsecurity.com/2014/06/17/usb-forensics-pt-6-which-user-account-used-the-usb-device/)

## Volume and Device Serial Number
Discover the Volume Serial Number of the Filesystem Partition on the USB and the Unique Device Serial Number and Model.

**WIN:** XP+ <br>
**SRV:** NULL

### Location
```plaintext
Software\Microsoft\WindowsNT\CurrentVersion\ENDMgmt
```

### Interpretation and Investigative Notes
- Use the Volume Name and USB Unique Serial Number to:
  - Find last integer number in line
  - Convert Decimal Serial Number into the Hex Serial Number
- Knowing both the Volume Serial Number and the Volume Name, you can correlate the data across SHORTCUT File (.LNK) analysis and the RECENTDOCs key.
- The .LNK file contains the Volume Serial Number and Name.
- The RECENTDOCs key will contain the Volume Name when the device was opened by Windows Explorer.
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- PowerShell `vol %Drive Letter%:`
- WMI `wmic diskdrive get Model, Name, InterfaceType, SerialNumber`

### Sources
- [Get USB - How to Get USB Volume Serial Number and USB Device Serial Number](https://www.getusb.info/how-to-get-usb-volume-serial-number-and-usb-device-serial-number/#)

## Drive Letter and Volume Name Identification
Discover the last drive letter of the USB Device when it was pluigged into the machine.

**WIN:** XP+ <br>
**SRV:** Not Tested

### Location
```plaintext
# WINDOWS XP
# Identify ParentIdPrefix
HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR
# Use ParentIdPrefix to discover the last Mount Point
HKLM\SYSTEM\MountedDevices

# WINDOWS 7+
Software\Microsoft\Windows Portable Devices\Devices

HKLM\SYSTEM\MountedDevices
```

### Interpretation and Investigative Notes
- Identify the USB device that was last mapped to a specific drive letter. This technique will only work for the last drive mapped. It does not contain historical records of every drive letter mapped to a removable drive.
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)

### Sources

## Shortcut Files (.LNK)
Shortcut files automatically created by windows when accessing recent items and opening local and remote data files and documents. Windows 11 contains a shortcut (.LNK) files that direct to the application, file, or directory.

[Darkcybe - Evidence of File and Folder Interaction](https://darkcybe.github.io/posts/DFIR_Evidence_of_File_and_Folder_Interaction/)
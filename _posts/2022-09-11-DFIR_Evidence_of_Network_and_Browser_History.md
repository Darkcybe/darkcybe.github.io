---
title: Evidence of Network and Browser History
categories: [DFIR, Evidence Artifacts]
tags: [cookies, evtx:wlan, srum]
comments: true
---
Techniques that can be used to discover evidence in support of an assets physical location, network connectivity and web browser history post-breach. More useful in investigation relating to insider threat or more commonly during the COVID Pandemic, attacks originating from employees working away from the office.

# Windows

## Timezone
Identification of the systems timezone can grant information that could indicate the an assets phyiscal locale.

**WIN:** XP+ <br>
**SRV:** 2003+

### Location
```plaintext
HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation
```

### Interpretation and Investigative Notes
- Internal logs and DTG stamps will be based on the control set saved in the registry key.
- Other network sourced logs will need to be correlated for any time difference/skew.
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)

### Sources
- [Microsoft - HKLM\System\CurrentControlSet\Control Registry Tree](https://docs.microsoft.com/en-us/windows-hardware/drivers/install/hklm-system-currentcontrolset-control-registry-tree)

## Browser Cookies
Cookies give insight into which sites have been visited and the activities that occurred on the site.

**WIN:** XP+ <br>
**SRV:** 2003+

### Location
```plaintext
# INTERNET EXPLORER
# Versions 6-10
%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Cookies

# Version 11
%USERPROFILE%\AppData\Local\Microsoft\Windows\INetCookies

# MOZILLA FIREFOX
# WINDOWS XP
%USERPROFILE%\Application Data\Mozilla\Firefox\Profiles<RANDOM-TEXT>.default\cookies.sqlite

# WINDOWS 7+
%USERPROFILE%\AppData\Mozilla\Firefox\Profiles<RANDOM-TEXT>.default\cookies.sqlite

# GOOGLE CHROME
# WINDOWS XP
%USERPROFILE%\Local Settings\ApplicationData\Google\Chrome\User Data\Default\Local Storage

# WINDOWS 7+
%USERPROFILE%\AppData\Local\Google\Chrome\User Data\Default\Local Storage
```

### Interpretation and Investigative Notes
- Google Analytics (GA) has developed an extremely sophisticated methodology for tracking site visits, user activity, and paid search. Since GA is largely free, it has a commanding share of the market, estimated at over 80% of sites using traffic analysis and over 50% of all sites.
  - _utma (Unique Vistors)
    - Domain Hash
    - Vistor ID
    - Cookie Creation Time
    - Time of 2nd most recent visit
    - Time of most recent visit
    - Number of visits
  - _utmb (Session Tracking)
    - Domain Hash
    - Page views in current session
    - Outbound link clicks
    - Time current session started
  - _utmz (Traffic Sources)
    - Domain Hash
    - last Update Time
    - Number of vists
    - Number of different types of vists
    - Source used to access site
    - Google AdWords campaign name
    - Access Method (organic, referral, cpc, email, direct)
    - Keyword used to find site (non-SSL only)
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)
- [Google Analytic Cookie Cruncher](https://github.com/mdegrazia/Google-Analytic-Cookie-Cruncher)
- [AZ4n6 - Google Analytic Cookie Parser](https://az4n6.blogspot.com/2012/11/google-analytics-cookie-parser_23.html)

### Sources
- [Hacking Articles - Beginner Guide to Understanding Cookies Session Management](https://www.hackingarticles.in/beginner-guide-understand-cookies-session-management/)
- [Acquire Forensics - Google Chrome Browser Forensics](https://www.acquireforensics.com/blog/google-chrome-browser-forensics.html)
- [Google - Analytics](https://analytics.withgoogle.com/)
- [Hats Off Security - Google Analytic Cookies](https://hatsoffsecurity.com/2014/10/03/google-analytic-cookies/)

## WLAN Event Log
Determine what wireless connections have been established, displays SSID.

**WIN:** 7+ <br>
**SRV:** Not Tested

### Location
```plaintext
Microsoft-Windows-WLAN-AutoConfig Operational.evtx
```

### Interpretation and Investigative Notes
- Event IDs
  - **11000:** Wireless network association started
  - **8001:** Successful connection to wireless network
  - **8002:** Failed connection to wireless network
  - **8003:** Disconnect from wireless network
  - **6100:** Network diagnostics (`System.evtx`)
  
### Tools
- [Event Log Explorer](https://www.eventlogxp.com/)
- [Event Log Parser (EvtxECmd)](https://github.com/EricZimmerman/evtx)
- Native Event Viewer

### Sources
- [SANS - Making the out of WLAN Event Log](https://www.sans.org/blog/making-the-most-out-of-wlan-event-log-artifacts/)

## Browser Search Times
Records websites visited by date and time. Details are stored for each local user account. Records the number of times visited (frequency) and also tracks access of local system files. Includes the website history of search terms in search engines.

**WIN:** XP+ <br>
**SRV:** Not Tested

### Location
```plaintext
# INTERNET EXPLORER
# Versions 6-7
%USERPROFILE%\Local Settings\History\History.IE5

# Versions 8-9
%USERPROFILE%\AppData\Local\Microsoft\Windows\History\History.IE5

# Versions 10-11
%USERPROFILE%\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat

# MOZILLA FIREFOX
# WINDOWS XP
%USERPROFILE%\Application Data\Mozilla\Firefox\Profiles<RANDOM-TEXT>.default\places.sqlite

# WINDOWS 7/8/10
%USERPROFILE%\AppData\Mozilla\Firefox\Profiles<RANDOM-TEXT>.default\places.sqlite
```

### Interpretation and Investigative Notes
  
### Tools
- [SQLECmd](https://github.com/EricZimmerman/SQLECmd)
- [Chromium Parser](https://tzworks.com/prototype_page.php?proto_id=45)
- [Mozilla Cache Parser](https://tzworks.com/prototype_page.php?proto_id=50)

### Sources
- [Nasbench - Web Browser Forensics](https://nasbench.medium.com/web-browsers-forensics-7e99940c579a)

## System Resource Usage Monitor (SRUM)
Records 30 to 60 days of historical system performance. Applications run, user account responsible for each, and application and bytes sent/received per application per hour.

**WIN:** 8+ <br>
**SRV:** Not Tested

### Location
```plaintext
SOFTWARE\Microsoft\WindowsNT\CurrentVersion\SRUM\Extensions

SOFTWARE\Microsoft\WlanSvc\Interfaces

C:\Windows\System32\SRU\
```

### Interpretation and Investigative Notes
- SOFTWARE\Microsoft\WindowsNT\CurrentVersion\SRUM\Extensions
  - Windows Network Data Usage Monitor
    - `{973F5D5C-xxxx-xxxx-xxxx-xxxxxxxxxxxx}`
  - Windows Network Connectivity Usage Monitor
    - `{DD6636C4-xxxx-xxxx-xxxx-xxxxxxxxxxxx}`
  
### Tools
- [Forensafe](https://www.forensafe.com/free.html)
- [SRUM Parser (SrumECmd)](https://github.com/EricZimmerman/Srum)
- [SRUM Dump](https://github.com/MarkBaggett/srum-dump)

### Sources
- [Velociraptor - Digging Into the System Resource Usage Monitor (SRUM)](https://velociraptor.velocidex.com/digging-into-the-system-resource-usage-monitor-srum-afbadb1a375)

## Browser Cache
The Broswer cache is where web page components can be stored locally to speed up subsequent visits. It can be used to glean further information on what a user was actively looking at online. Providing the following information:
- Websites visited
- Files viewed on a website visited (caches files are linked to specific local accounts)
- Timestamps indicate when site was first saved and last accessed.

**WIN:** XP+ <br>
**SRV:** Not Tested

### Location
```plaintext
# INTERNET EXPLORER
# Versions 8-10
%USERPROFILE%\AppData\Local\Microsoft\Windows\Temporary Internet Files\Content.IE5

# Version 11
%USERPROFILE%\AppData\Local\Microsoft\Windows\INetCache\IE

# Edge
%USERPROFILE%\AppData\Local\Packages\microsoft.micosoftedge_<APP ID>\AC\MicrosoftEdge\Cache

# MOZILLA FIREFOX
# WINDOWS XP
%USERPROFILE%\Application Data\Mozilla\Firefox\Profiles<RANDOM-TEXT>.default\cache

# WINDOWS 7+
%USERPROFILE%\AppData\Mozilla\Firefox\Profiles<RANDOM-TEXT>.default\cache

# GOOGLE CHROME
# WINDOWS XP
%USERPROFILE%\Local Settings\Application Data\Google\Chrome\User Data\Default\Cache\ - data_# and f_######

# WINDOWS 7+
%USERPROFILE%\AppData\Local\Google\Chrome\User Data\Default\Cache- data_# and f_######
```

### Interpretation and Investigative Notes
  
### Tools
- [SQLECmd](https://github.com/EricZimmerman/SQLECmd)
- [Chromium Parser](https://tzworks.com/prototype_page.php?proto_id=45)
- [Mozilla Cache Parser](https://tzworks.com/prototype_page.php?proto_id=50)
- [Microsoft Edge Dev Tools](https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/storage/cache)

### Sources
- [Nasbench - Web Browser Forensics](https://nasbench.medium.com/web-browsers-forensics-7e99940c579a)
- [Acquire Forensics - Google Chrome Browser Forensics](https://www.acquireforensics.com/blog/google-chrome-browser-forensics.html)

## Flash and Super Cookies
Local Stored Objects (LSO's), or Flash Cookies, have become ubiquitous on most systems due to the extremely high penetration of Flash applications across the internet. They tend to be much more persistent because they do not expire, and there is no built-in mechanisms within the browser to remove them. In fact, many sites have begun using LSOs for their tracking mechanisms because they rarely get cleared like traditional cookies. 

Provides the following information:
- Websites visited
- User account used to visit the site
- When cookie was created and last accessed

**WIN:** 7+<br>
**SRV:** Not Tested

### Location
```plaintext
%APPDATA%\Roaming\Macromedia\FlashPlayer#SharedObjects<random_profile_id>
```

### Interpretation and Investigative Notes
  
### Tools
- [SQLECmd](https://github.com/EricZimmerman/SQLECmd)
- [Chromium Parser](https://tzworks.com/prototype_page.php?proto_id=45)
- [Mozilla Cache Parser](https://tzworks.com/prototype_page.php?proto_id=50)
- [Microsoft Edge Dev Tools](https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/storage/cache)

### Sources
- [Nasbench - Web Browser Forensics](https://nasbench.medium.com/web-browsers-forensics-7e99940c579a)
- [Forensics From the Sausage Factory - Adobe Flash Player Local Shared Objects](https://forensicsfromthesausagefactory.blogspot.com/2009/04/adobe-flash-player-local-shared-objects.html)

## Session Restore
Automatic Crash Recovery features built into the browser.

**WIN:** 7+<br>
**SRV:** Not Tested

### Location
```plaintext
# INTERNET EXPLORER
%USERPROFILE%\AppData\Local\Microsoft\Internet Explorer\Recovery

# MOZILLA FIREFOX
%USERPROFILE%\AppData\Mozilla\Firefox\Profiles<RANDOM-TEXT>.default\sessionstore.js

# GOOGLE CHROME
%USERPROFILE%\AppData\Local\Google\Chrome\User Data\Default\
```

### Interpretation and Investigative Notes
- Historical Websites viewed in each tab
- Referring Websites
- Time session ended
- Modified time of .dat files in LastActive folder
- Time each tab opened (only when crash occured)
- Creation time of .dat files in Active Folder

### Tools
- [SQLECmd](https://github.com/EricZimmerman/SQLECmd)
- [Chromium Parser](https://tzworks.com/prototype_page.php?proto_id=45)
- [Mozilla Cache Parser](https://tzworks.com/prototype_page.php?proto_id=50)
- [Huge JSON Viewer](https://github.com/WelliSolutions/HugeJsonViewer)

### Sources
- [Computer Forensics Parsonage - Web Browser Session Restore Forensics](http://computerforensics.parsonage.co.uk/downloads/WebBrowserSessionRestoreForensics.pdf)
- [Acquire Forensics - Google Chrome Browser Forensics](https://www.acquireforensics.com/blog/google-chrome-browser-forensics.html)
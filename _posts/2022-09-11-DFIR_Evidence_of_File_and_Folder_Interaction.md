---
title: Evidence of File and Folder Interaction
categories: [DFIR, Evidence Artifacts]
tags: [acmru, thumbcache, wordwheelquery, recycle bin, lastvisitedmru, opensavemru, jump list, shell bag, .lnk, prefetch, activitiescache]
comments: true
---
Techniques that can be used to discover evidence in support of an attackers interaction with files and folders such as search, deletion and opening post-breach.

# Windows

## XP Search (ACMRU)
A wide variety of information can be searched for through the search assistant on a Windows XP Machine. The search assistant will remember a user's search terms for filenames, computers, or words that are inside a file. This is an example of where you can find the "Search History" on the Windows system.

**WIN:** XP <br>
**SRV:** NULL

### Location
```plaintext
NTUSER.DAT\Software\Microsoft\Search Assistant\ACMru\####
```

### Interpretation and Investigative Notes
- Search the Internet
  - ####-5001
- All or part of a document name
  - ####-5603
- A word or phrase within a document
  - ####-5604
- Printers, computers, or people
  - ####-5647
  
### Tools
- [Registry Explorer (RECmd)](https://www.sans.org/tools/registry-explorer/)

### Sources
- [Forensics Wiki - List of Windows MRU Locations](https://forensicswiki.xyz/wiki/index.php?title=List_of_Windows_MRU_Locations)

## ThumbCache.db
Thumbnails of pictures, office documents, and folders exist in a database called the thumbcache. Each user will have their own database  based on the thumbnail sizes viewed by the user (small, medium, large, and extra large)

**WIN:** XP+ <br>
**SRV:** 2003+

### Location
```plaintext
C:%USERPROFILE%\AppData\Local\Microsoft\Windows\Explorer
```

### Interpretation and Investigative Notes
- Created when a user switches a folder to thumbnail mode or views pictures via a slide show. As it were, our thumbs are now stored in separate database files.
- The thumbnail will store the thumbnail copy of the picture based on the thumbnail size in the content of the equivalent database file (Small, Medium, Large, and Extra Large)
  
### Tools
- [ThumbCache Viewer)](http://thumbcacheviewer.github.io/)

### Sources
- [GHacks - Thumbnail Cache Files Windows](https://www.ghacks.net/2014/03/12/thumbnail-cache-files-windows/)

## Thumbs.db
Hidden file in directory where images on a machine exist stored in a smaller thumbnail graphic. Thumbs.db catalogs pictures in a folder and stores a copy of the thumbnail even if the pictures were deleted.

**WIN:** XP+ <br>
**SRV:** 2003+

### Location
```plaintext
# WINDOWS XP-8
Automatically created anywhere with homegroup enabled
# WINDOWS 7+
Automatically created anywhere and accessed via a UNC Path (local or remote)
```

### Interpretation and Investigative Notes
- The database includes information such as:
  - Thumbnail Picture of Origin Picture
  - Document Thumbnail - Even if Deleted
  - Last Modification Time (**XP Only**)
  - Original Filename (**XP Only**)
  
### Tools
- [MiTeC](http://www.mitec.cz/wfa.html)
- [FTK Toolkit](https://accessdata.com/products-services/forensic-toolkit-ftk)

### Sources
- [PCWorld - Manage Thumbs DB Files in Windows and on the Network](https://www.pcworld.com/article/2999243/manage-thumbs-db-files-in-windows-and-on-the-network.html)
- [Discovery Forensics - Caught You Looking! ThumbCache Databases](https://www.discoveryforensics.ca/2022/07/13/thumbs-db-and-thumbcache-db/)

## Internet Explorer (IE) and Edge File History
A little-known fact about the IE and Edge History is that the information stored is not just related to Internet browsing. The history also contains records of local and remote network share file access, giving us an excellent means for determining which files and applications were accessed on the system, day by day.

**WIN:** XP+ <br>
**SRV:** 2003+

### Location
```plaintext
# INTERNET EXPLORER
# Version 6/7
%USERPROFILE%\LocalSettings\History\History.IE5

# Version 8/9
%USERPROFILE%\AppData\Local\Microsoft\WindowsHistory\History.IE5

# Version 10/11
%USERPROFILE%\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat

# EDGE
%USERPROFILE%\AppData\Local\Microsoft\Windows\History\Low\History.IE5>
```

### Interpretation and Investigative Notes
- Stored in `index.dat` as:
  - `file:///C:/directory/filename.ext`
- Does not prove that the file was opened by the browser
  
### Tools
- [Pasco2](https://sourceforge.net/projects/pasco2/)
- [Windows Index.dat Parser](https://tzworks.com/prototype_page.php?proto_id=6)

### Sources
- [Defend The Web - Internet Explorer Forensic Investigation](https://defendtheweb.net/article/internet-explorer-forensic-investigation)
- [Forensics Wiki - Internet Explorer History File Format](https://forensicswiki.xyz/wiki/index.php?title=Internet_Explorer_History_File_Format)

## Search WordWheelQuery
Keywords search for from the START menu bar.

**WIN:** 7+ <br>
**SRV:** 2003+

### Location
```plaintext
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery
```

### Interpretation and Investigative Notes
- Keywords are added in Unicode and listed in temporal order in an MRUlist.
  
### Tools
- [Registry Explorer](https://www.sans.org/tools/registry-explorer/)

### Sources
- [Forensafe - Investigating Searched Strings](https://forensafe.com/blogs/searchedstrings.html)

## Recycle Bin
The recycle bin is a very important location on a Windows file system system to understand. It can help you when accomplishing a forensics investigation, as every file that is deleted from a Windows Recycle Bin aware program is generally first put in the Recycle Bin.

**WIN:** XP+ <br>
**SRV:** 2003+

### Location
```plaintext
# WINDOWS XP
C:\Recycler" 2000/NT/XP/2003

# WINDOWS 7+
C:$Recycle.bin

Deleted Time and Original Filename contained in separate files for each deleted recovery file
```

### Interpretation and Investigative Notes
- WINDOWS XP
  - Subfolder is created with user's SID and can be mapped to user
  - Maps file name to the actual name and path it was deleted from
  - Hidden file in directory called `INFO2` contains Deleted Time and Original Filename
  - Filename in both ASCII and UNICODE
- WINDOWS 7+
  - Subfolder is created with user's SID and can be mapped to user
  - Deleted Time and Original Filename contained in separate files for each deleted recovery file
  - Filenames proceeded by `$I######`, contain:
    - Original PATH and name
    - Deletion Date/Time
  - Filenames procceded by `$R######`, contain:
    - Recovery Data
  
### Tools
- [Recycle Bin Artifact Parser (RBCmd)](https://github.com/EricZimmerman/RBCmd)
- [Trash Inspection and Analysis (TIA)](https://tzworks.com/prototype_page.php?proto_id=38)

### Sources
- [Forensically Fit - Windows Recycle Bin Forensics](https://forensicallyfit.wordpress.com/2019/01/28/windows-recycle-bin-forensics/)

## LastVisitedMRU
Tracks the specific executable used by an application to open files documented in the OpenSaveMRU key. In addition, each value also tracks the directory location for the last file that was access by that application.

[Darkcybe - Evidence of Execution](https://darkcybe.github.io/posts/DFIR_Evidence_of_Execution/#lastvisitedmru)

## OpenSaveMRU
Tracks files that have been opened or saved within a Windows shell dialog box. This happens to be a big data set, not only including web browsers like Internet Explorer and Firefox, but also a majority of commonly used applications.

[Darkcybe - Evidence of Download](https://darkcybe.github.io/posts/DFIR_Evidence_of_Download/#opensavemru)

## Recent Files
Registry Key that will track the last files and folder opened and is used to populate data in "recent" menus of the Start Menu.

**WIN:** XP+ <br>
**SRV:** Not Tested

### Location
```plaintext
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs
```

### Interpretation and Investigative Notes
- RecentDocs
  - Overall key will track the overall order of the last 150 files or folders opened. MRU list will keep track of the temporal order in which each file/folder was opened.
  - Includes last entry time which mirrors the last opening time.
- The `.%%%` key (Three Letter Extension)
  - Stores file opening operations based of a specific extension in temporal order.
- Folder
  - Stores folder access based on opening in temporal order.
  
### Tools
- [Registry Explorer](https://www.sans.org/tools/registry-explorer/)

### Sources
- [Forensic4cast - The Recentdocs Key in Windows 10](https://forensic4cast.com/2019/03/the-recentdocs-key-in-windows-10/)

## Jump Lists
The Windows task bar (Jump List) is engineered to allow users to "jump" or access items they have frequently or recently used quickly and easily. This funcationality cannot only include recent media files; it must also include recent tasks. 

The data stored in the AutomaticDestinations folder will each have a unique file prepended with the AppID of the associated application on Windows 7 through 10 machines. Windows 11 contains a shortcut (.LNK) files that direct to the application, file, or directory.

[Darkcybe - Evidence of Execution](https://darkcybe.github.io/posts/DFIR_Evidence_of_Execution/#jump-lists)

## Shell Bags
Which folders were accessed on the local machine, the network and/or removable devices. Evidence of previously existing folders after deletion/overwrite. When certain folders are created.

**WIN:** XP+ <br>
**SRV:** Not Tested

### Location
```plaintext
# Access via Explorer
USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags
USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagsMRU

# Access via Desktop
NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags
NTUSER.DAT\Software\Microsoft\Windows\Shell\BagsMRU
```

### Interpretation and Investigative Notes

  
### Tools
- [Registry Explorer](https://www.sans.org/tools/registry-explorer/)
- [Shell Bags Explorer](https://ericzimmerman.github.io/#!index.md)
- [Windows Shell Bag Parser](https://tzworks.com/prototype_page.php?proto_id=14)

### Sources
- [CE Digital Forensics - Shellbag Analysis](https://medium.com/ce-digital-forensics/shellbag-analysis-18c9b2e87ac7)

## Shortcut Files (.LNK)
Shortcut files automatically created by windows when accessing recent items and opening local and remote data files and documents. Windows 11 contains a shortcut (.LNK) files that direct to the application, file, or directory.

**WIN:** XP+ <br>
**SRV:** Not Tested

### Location
```plaintext
# WINDOWS XP
C:%USERPROFILE%\Recent

# WINDOWS 7+
C:%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\

C:%USERPROFILE%\AppData\Roaming\Microsoft\Office\Recent\
```

### Interpretation and Investigative Notes
- Although the locations listed are the default, they can be created anywhere.
- Date/Time file of that name was first opened
  - Creation Date of .LNK file
- Date/Time file of that name was last opened
  - Last Modification Date of .LNK file
- LNKTarget File (Internal LNK file details) Details:
  - Modified, Accessed and creation times of target file
  - Volume information
  - Network Share information
  - Original location
  - Name of system
  
### Tools
- [LNK Explorer (LECmd)](https://github.com/EricZimmerman/LECmd)
- [Windows LNK Parsing Utility](https://tzworks.com/prototype_page.php?proto_id=11)

### Sources
- [Magnet Forensics - Forensic Analysis of LNK Files](https://www.magnetforensics.com/blog/forensic-analysis-of-lnk-files/)

## Prefetch
Increases performance of a system by pre-loading code pages of commonly used applications. Cache Manager monitors all files and directories referenced for each application or process and maps them into a .pf file. Utilized to know an application was executed on a system.
- Limited to 128 files on XP and Windows 7
- Limited to 1024 files on Windows 8
- `<EXE_NAME>-<HASH>.pf`

[Darkcybe - Evidence of Execution](https://darkcybe.github.io/posts/DFIR_Evidence_of_Execution/#prefetch)

## Microsoft Office Recent Files
Microsoft Office programs will track their own recent files list to make it easier for users to remember the last file they were editing.

**WIN:** XP+ <br>
**SRV:** Not Tested

### Location
```plaintext
# MICROSOFT OFFICE
# Versions 10-14 (XP - 2010)
NTUSER.DAT\Software\Microsoft\Office\VERSION

# Version 15 (Office365)
NTUSER.DAT\Software\Microsoft\Office\VERSION\UserMRU\LiveID_####\FileMRU
```

### Interpretation and Investigative Notes
Similar to recent files, this will track the last files that were opened by each Microsoft Office application. The last entry added, per the MRU, will be the time the last filewas opened by a specific application.
  
### Tools
- [Registry Explorer](https://www.sans.org/tools/registry-explorer/)

### Sources
- [DF Stream - Microsoft Office Forensics](https://df-stream.com/category/microsoft-office-forensics/)

## Windows Timeline (ActivitiesCache.db)
Windows 10 introduced a background feature that records recently used applications and accessed files over a 30 day duration in a "timeline" accessible via the "WIN+TAB" key. The data is recorded in a SQLite database. Windows 11 removed the "WIN+TAB" functionality, however the ActivitiesCache.db still remains.

Research identified that Windows Server 2016 also maintains an ActivitiesCache.db file, however `ActivityOperation`, `Activity_PackageId`, and `Activity` entries were not recorded.

[Darkcybe - Evidence of Execution](https://darkcybe.github.io/posts/DFIR_Evidence_of_Execution/#windows-timeline-activitiescachedb)
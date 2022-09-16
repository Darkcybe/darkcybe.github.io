---
title: Evidence of File Interaction
categories: [DFIR]
order: 3
tags: [evidence, acmru, thumbcache, wordwheelquery, recycle bin, lastvisitedmru, internet explorer, microsoft edge]
comments: true
---
Techniques that can be used to discover evidence in support of an attackers interaction with files such as search, deletion and opening post-breach.

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

[Darkcybe - Evidence of Execution](https://darkcybe.github.io/posts/DFIR_Evidence_of_Execution/)
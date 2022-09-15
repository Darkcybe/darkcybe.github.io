---
title: Evidence of File Interaction
categories: [DFIR]
tags: [evidence, acmru, thumbcache]
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

## ThumbCache
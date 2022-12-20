---
title: Incident Timelines
categories: [DFIR Tools, Toolkits]
tags: [log2timeline, plaso]
comments: true
---

## Overview

Timelines are an important tool in incident response and digital forensics for understanding the sequence of events that have occurred on a computer system. They can be used to:

1. **Identify the root cause of an incident:** By understanding the sequence of events that led up to an incident, it is possible to identify the root cause and take appropriate corrective action.
2. **Investigate attacks:** Timelines can be used to identify suspicious activity and track the actions of an attacker or perpetrator.
3. **Understand the history of a computer's usage:** Timelines can provide a comprehensive record of all the events that have occurred on a computer system, which can be useful for understanding its history and how it has been used.

To create a timeline, forensic analysts typically gather data from multiple sources, such as system logs, browser history, and file metadata. Tools can then be used to create a timeline from this data and visualize it for easier analysis.

Here is a list of forensic analysis tools that can create a timeline:

1. **[Plaso](https://github.com/log2timeline/plaso)** A tool for generating a supertimeline from multiple data sources, such as system logs, browser history, and file metadata.
2. **Autopsy:** A digital forensics platform that includes a timeline feature for creating a timeline from data sources such as system logs and file metadata.
3. **EnCase:** A digital forensics platform that includes a timeline feature for creating a timeline from data sources such as system logs and file metadata.

Timelines can be a valuable resource in incident response and digital forensics, as they provide a clear and concise overview of the events that have occurred on a system and can help investigators understand the context and circumstances surrounding an incident.

## Plaso

Plaso (Plaso Langar Að Safna Öllu) is a digital forensics tool that extracts, analyzes, and stores digital artifacts from a computer system. It works by scanning a disk or disk image and extracting data from various sources, such as system logs, browser history, and file metadata. The extracted data is then processed and stored in a SQLite database, which can be queried and analyzed to identify suspicious activity and understand the sequence of events that have occurred on the system.

Plaso uses a plug-in based architecture, which means that it can be easily extended to support new data sources and analysis techniques. It comes with a number of built-in plugins that can extract data from common sources, such as Windows event logs, MacOS system logs, and Android device logs.

Plaso is composed of a number of modular components, which work together to extract, analyze, and store digital artifacts from a computer system.

Here is a brief overview of some of the main modules in Plaso:

1. **image_export:** This module extracts data from a disk image and save it to a file or directory.
2. **log2timeline:** This module is used to extract data from various sources and create a supertimeline, which is a single timeline that combines data from multiple sources. It is the main module of Plaso and is responsible for coordinating the extraction, processing, and storage of data.
3. **pinfo:** This module is used to display information about a Plaso storage file or the events contained within it.
4. **psort:** This module is used to sort and filter events in a Plaso storage file.
5. **psteal:** This module extracts specific types of data from a Plaso storage file and saves it to a file or directory.

Note that each tool can be invoked with the `-h` or `--help` command line flag to display basic usage and command line option information.

### Installing Plaso on Ubuntu 22.04 LTS

To install Plaso from the GIFT Personal Package Archive (PPA) add the GIFT PPA:

```bash
sudo add-apt-repository ppa:gift/stable
```

Update and install Plaso:

```bash
sudo apt-get update
sudo apt-get install plaso-tools
```

Additionally, the SANS [SIFT](https://github.com/teamdfir/sift) workstation is an independent project that provides Plaso releases pre-built within a forensic focused Linux distribution.

If you are using SIFT and you have a deployment problem please report that directory to the SIFT project.

### Generating a Supertimeline with log2timeline

log2timeline is a tool that can be used to generate a supertimeline, which is a single timeline that combines data from multiple sources, such as system logs, browser history, and file metadata. This can be useful for quickly identifying suspicious activity or understanding the sequence of events that led to a particular outcome.

1. Install log2timeline using your preferred method (e.g. pip install log2timeline).
2. Run log2timeline with the appropriate parameters to specify the data sources you want to include in the supertimeline. For example:
Copy code

```bash
log2timeline.py --storage-file timeline.plaso image.raw
```

3. log2timeline will process the data sources and generate a plaso file with the supertimeline data.
4. The output Plaso file in SQLite database, psort can be used to filter, sort and run automatic analysis on the contents of Plaso storage files. In the example below, psort is used to parse the `timeline.plaso` file and output a dynamic .csv file. Psort is capable of a lot more, see the "Using Psort" link below.

```bash
psort.py -o dynamic -w timeline.csv timeline.plaso
```

## Resources
- [Plaso Documentation](https://plaso.readthedocs.io/en/latest/)
- [Log2timeline/plaso - GitHub](https://github.com/log2timeline/plaso)
- [Using Psort](https://plaso.readthedocs.io/en/latest/sources/user/Using-psort.html)
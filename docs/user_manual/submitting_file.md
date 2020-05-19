---
layout: default
title: Submitting file
nav_order: 1
parent: User's manual
has_children: false
---

# Submitting a file for analysis

## Submission
Submitting a file for analysis is really easy; it can be done directly using the AssemblyLine WebUI. For automation and integration you can use the [restapi](assemblyline_client.html#submit-a-file).

<img src="./images/submit.png" width="725">

### Sharing and classification

If your system is configured with a sharing control (TLP) or Classification configuration; available restriction can be selected by clicking on the banner.

### Selecting a file to scan

You can click the "Select a file to scan" or drag and drop a file to the area to add a file to be analyzed.

## Options
Additional submission options are available to:

- select which service categories or specific services to use for the analysis. 
- specify service configuration options (such as providing a password, or dynamic analysis timeout).

| Ignore filtering services | Bypass whitelisting services |
| Ignore result cache | Force re-analysis even if the same file had been scanned recently with the same services version |
| Ignore dynamic recursion prevention | Disable iteration limit on a file |
| Profile current scan | |
| Perform deep analysis | Provide maximum deobfuscation - **Highly recommanded for known malicious or highly suspicious file to detect highly obfuscated content** |
| Time to live | Time (in days) before the file is purged from the system |

<img src="./images/submit_options.png" width="725">

## File analysis

Once a file is submitted to AssemblyLine, the system will automatically perform multiple check to determine how to best process the file. One of AssemblyLine most powerful functionality is its recursive analysis model. Malware and malicous documents often user multiple layers of obfuscation; resursive analysis allow the system to remove these layers and keep analyzing the file. The end results is often a cleartext script or unpacked malware which tradional anti-virus are very effective at detecting.

<img src="./images/processing.png" width="725">

[Ok! Let's look at results](./results.html){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

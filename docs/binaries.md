# Binaries
Each [release](https://github.com/mjordan/islandora_workbench/releases) provides binaries for popular operating systems.
  
 <br/>
 <br/>
  
## Additional steps for running binary files

### macOS

workbench-macos binary is not a "signed application" also from an "unidentified developer" according to Apple. Therefore you will need to bypass Apple's check for malicious software with the following command:

`xattr -d com.apple.quarantine /path/to/workbench-macos`

NOTE: If you do not want to bypass the Apple's malware protection or are unable to bypass it, you can still run workbench by following the standard [installation instructions](/islandora_workbench_docs/installation). 

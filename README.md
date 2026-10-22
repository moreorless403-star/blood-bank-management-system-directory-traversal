# The Blood-Bank-Management-System has Directory traversal
## NAME OF AFFECTED PRODUCT(S)
Blood-Bank-Management-System
## Assessment Management
[Blood-Bank-Management-System](https://github.com/anshikasharma09/Blood-Bank-Management-System)
## AFFECTED AND/OR FIXED VERSION(S)
### VERSION(S)
V1.0
## Software Link
[Blood-Bank-Management-System](https://github.com/anshikasharma09/Blood-Bank-Management-System)
## PROBLEM TYPE
### Vulnerability Type
Directory traversal
## Root Cause
A directory traversal vulnerability has been identified in the "Blood-Bank-Management-System" project. By injecting `../../` sequences into parameters, an attacker can bypass web directory restrictions and access the contents of arbitrary files within the web directory, effectively allowing them to traverse the entire web directory structure.
## Impact
Attackers can exploit this directory traversal vulnerability to gain unauthorized access to web directories, leak sensitive data, tamper with data, achieve full system control, or even cause service disruptions—posing a serious threat to both system security and business continuity.
## DESCRIPTION
During a security review of the "Blood Bank Management System," a critical directory traversal vulnerability was discovered in the web server. The vulnerability stems from inadequate web server configuration, which allows an attacker to inject malicious commands. Because the server lacks restrictions against directory traversal, an attacker can navigate throughout the entire web directory—for instance, by using sequences like `../..` or by accessing directories directly—thereby gaining unauthorized access to the database, modifying or deleting data, and even compromising sensitive information. Remedial measures must be taken immediately to ensure system security and protect data integrity.
## No login or authorization is required to exploit this vulnerability
## Vulnerability details and POC
### Vulnerability type:
Directory Traversal
## Vulnerability location:
```
Blood-Bank-Management-System/file
Blood-Bank-Management-System/sql
Blood-Bank-Management-System/image
```
## Attack results

![9541c6e3-42e1-45ba-b2ee-77c1c80ea91e](/Users/aipao/Library/Containers/com.tencent.xinWeChat/Data/Documents/xwechat_files/wxid_zx6fxs8godd422_0b5c/temp/InputTemp/9541c6e3-42e1-45ba-b2ee-77c1c80ea91e.png)

![715572c2-4fd8-44da-862d-b068661d0194](/Users/aipao/Library/Containers/com.tencent.xinWeChat/Data/Documents/xwechat_files/wxid_zx6fxs8godd422_0b5c/temp/InputTemp/715572c2-4fd8-44da-862d-b068661d0194.png)

![42a0e793-456d-4a63-b7e3-3c5b55d23e16](/Users/aipao/Library/Containers/com.tencent.xinWeChat/Data/Documents/xwechat_files/wxid_zx6fxs8godd422_0b5c/temp/InputTemp/42a0e793-456d-4a63-b7e3-3c5b55d23e16.png)

## Suggested repair

1. Never build a file path by concatenating user input. Resolve the requested name against a fixed base directory with basename() so that any directory components supplied by the client are discarded.
2. Verify the resolved path is still inside the base directory. Canonicalise the path (resolving symlinks and ..) and confirm it still starts with the expected base path before opening, reading, or deleting the file.

3. Reject traversal sequences in the input. Treat .., /, \, and null bytes as invalid input and return an error, rather than silently stripping or normalising them away.

4. Map opaque identifiers to server-side paths. Let the client submit a record ID and resolve it to a file path through a server-side lookup, so no path information is ever supplied by the client.
5. Validate against an allow-list where the permitted set is known. Check the requested name against a fixed list of valid entries instead of trying to blacklist dangerous characters.
6. Do not follow symbolic links when the target must stay inside the base directory. Where the platform supports it, open the file with an API or flag set that refuses to traverse symlinks.
7. Apply the same rules to every file operation. Reading, writing, uploading, renaming, and unlink() must all go through one shared path-resolution helper, so the validation cannot be bypassed by using a different code path.
8. Never let user input decide where a file is stored. For uploads, generate the stored filename server-side and keep the client-supplied name only as a display attribute.

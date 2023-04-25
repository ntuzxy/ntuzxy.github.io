---
title: OneDrive Client for Linux
date: 2023-04-25 11:33:00 +0800
categories: [Tools, Linux]
tags: [linux, onedrive, client, sync]
math: true
mermaid: true
---

# OneDrive Client for Linux

In this blog, I will list some key steps so you can use OneDrive client in Linux quickly. 

## 1. Install
For CentOS 7 user, follow this:

>Dependencies: Fedora < Version 18 / CentOS 7.x / RHEL 7.x
>>sudo yum groupinstall 'Development Tools'  
>>sudo yum install libcurl-devel sqlite-devel  
>>curl -fsS https://dlang.org/install.sh | bash -s dmd-2.099.0
>>
>For notifications the following is also necessary:
>>
>>sudo yum install libnotify-devel

For other Linux distributions, refer to [this](https://github.com/abraunegg/onedrive/blob/master/docs/INSTALL.md)

## 2. Configuration and Usage



# Reference
<https://github.com/abraunegg/onedrive/blob/master/README.md>

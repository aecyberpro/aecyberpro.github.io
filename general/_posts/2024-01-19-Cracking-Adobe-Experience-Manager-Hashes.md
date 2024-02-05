---
layout: post
title: Cracking Adobe Experience Manager Hashes
description: >
  Adobe Experience Manager (AEM) password hashes are not supported by Hashcat. Follow along as I show how to install a compatible version of John, convert, and finally crack the hash.
sitemap: true
---

On a recent pentest I discovered Adobe Experience Manager (AEM) password hashes from a Nuclei scan. I found that Hashcat doesn't support cracking these hashes, although there is a forum [post](https://hashcat.net/forum/thread-7471.html) about it which is what ultimately led me to John.

Note: This post is now outdated. Since originally posting this, I've found that although the latest Hashcat release doesn't support AEM hashes, you can build the latest source and it supports AEM hashes. Hashcat also provides better support for GPU cracking. I found that I had an error I was unable to resolve when attempting to compile John source with GPU support.

The version of John installed from the package manager isn't compatible with AEM hashes. Follow these steps to build a compatible version and get cracking:

1. `sudo apt-get install build-essential libssl-dev`
2. `sudo apt-get install yasm libgmp-dev libpcap-dev libnss3-dev libkrb5-dev pkg-config libbz2-dev zlib1g-dev`
3. For an Nvidia GPU: `sudo apt-get install nvidia-cuda-toolkit nvidia-opencl-dev`
4. `git clone git://github.com/magnumripper/JohnTheRipper -b bleeding-jumbo john`
5. `./configure && make -s clean && sudo make -sj4`
6. Convert the hash: `john/run/aem2john.py <path to file with hash> > aem2john.hash`
7. Crack the hash: `john/run/john aem2john.hash`
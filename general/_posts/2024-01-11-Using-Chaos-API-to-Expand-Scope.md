---
layout: post
title: Using the Chaos DNS API to Enumerate Pentest Subdomains
description: >
  For your pentest, you're given a list of IP addresses and some domains in scope. Using this methodology you can use the Chaos API to enumerate subdomains.
sitemap: true
---

I was recently preparing for an upcoming external pentest which included a very large scope. I was given a list of thousands of IP addresses, and a few hundred domain names. If you've ever hacked on Hack The Box you may have seen boxes where the exposed web app is different based on the hostname or vhost. An important part of performing external network pentests is enumerating domains and subdomains to help you find more web app content to hack.

First, you need to sign up for a [Chaos](https://chaos.projectdiscovery.io/#/) account and get an API key, then install the [Chaos client](https://github.com/projectdiscovery/chaos-client).

I started out with a file containing in scope IP addresses, and another file containing hostsnames provided by the client. Then I enumerated for top-level domains (TLD's) using the following Bash command: `rev hostnames.txt | cut -d '.' -f 1,2 | rev | sort -u > tlds.txt`. This reverses each line, grabs the TLD, then reverses it back and saves the TLD's to a file.

Next, run `while read -r line;do chaos -silent -d "$line";done<tlds.txt | tee alldomains.txt` to get a list of all known subdomains. Finally, run the following script to check to see if each subdomain's resolved IP address is in scope and if yes then print it. This just helped me to double the list of subdomains in scope.

```bash
#!/bin/bash

# Check if alldomains.txt and scope.txt exist
if [ ! -f alldomains.txt ]; then
    echo "Error: alldomains.txt not found."
    exit 1
fi

if [ ! -f scope.txt ]; then
    echo "Error: scope.txt not found."
    exit 1
fi

# Read each line in alldomains.txt
while read -r domain; do
    # Use the host command to find the IP address
    ip_addresses=$(host "$domain" | grep "has address" | awk '{ print $4 }')

    # Check each IP address
    for ip in $ip_addresses; do
        # Check if the IP address is in scope.txt
        if grep -q "$ip" scope.txt; then
            # If the IP is in scope.txt, print the domain
            echo "$domain"
            # Break the loop after finding the first match
            break
        fi
    done
done < alldomains.txt
```
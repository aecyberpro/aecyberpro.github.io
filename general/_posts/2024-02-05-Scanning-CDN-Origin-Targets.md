---
layout: post
title: Scanning CDN Origin Targets
description: >
  This article discusses the importance of using hostnames instead of IP addresses when conducting vulnerability scans or penetration tests. It explains how protocols like HTTP behave differently depending on how they are addressed, and how this can lead to missed vulnerabilities when only IP addresses are used.
sitemap: true
---

When performing a penetration test you're likely to start off with a list, range, or network of IP addresses. While it's a logical first step to scan based on IP addresses, you should always follow up by scanning and enumerating based on hostnames. A webserver may respond with a different web application depending on the value sent in request "Host" header. If you scan by IP address, the IP address is what is submitted in the Host header and the server will respond with the default web application. The default may be the default IIS page which looks like a base installation of IIS with no other content, or it may be the default one of may hosted web applications. Enumerating DNS PTR records and TLS certificate Subject Alternative Names (SAN) are likely to provde you with a list of hostnames that an IP address hosts. If the DNS record for any particular IP address doesn't match the SAN, you may need to add the hostname to your `/etc/hosts` file before continuing your scanning and enumeration. The Nuclei scanner will output any discovered DNS PTR hostnames as well as TLS certificate SANs. You should run additional scanning and enumeration targeting these hostnames.

Your target company may be using a Content Delivery Network (CDN) and the DNS record may be pointing to the CDN IP address. Scanning this address will likely result in "false negatives" because the CDN is using a WAF. If you can find the CDN origin IP in your pentest scope, you can scan the host directly, bypassing the CDN WAF. The first thing you need to do is enumerate the TLS certificate Subject Alternative Names (SAN) to find the hostnames that each IP address is routing to backend servers. Once that is complete, if you run `nslookup`, `host`, or `dig` on each hostname, you may find that it's resolving to `akamaiedge.net` or other Content Delivery Network (CDN) provider. You don't want to be scanning through the CDN provider because it will be using a WAF. This would likely result in not only missing vulnerabilities, this could be a disaster if you're scanning from your home/office IP address which could get you blocked across a number of other popular websites using the same CDN.

Use the following steps to enumerate the hostnames from the TLS SANS, bulk add the hostnames to your /etc/hosts and scan them with Nuclei or Nessus.

Enum ssl-dns-names:

```bash
nuclei -l liveips.txt -t ~/nuclei-templates/ssl/ssl-dns-names.yaml -o scans/nuclei-ssl-dns-names.txt
```

Sort unique: To test for the uniqueness of all lines in a file based on the fifth column of a space-separated list, you can use the following Bash one-liner: `awk '!seen[$5]++' filename`. This one-liner will print each line from the file where the fifth column has a unique value, according to the first occurrence of each unique value. If a value in the fourth column repeats, subsequent lines with that value will not be printed.

Reformat output for use in `/etc/hosts`. 

```bash
cat input.txt | cut -d ' ' -f 4- | sed 's/:443//' | sed 's/\[//g' | sed 's/\]//g' | sed 's/"//g' | sed 's/,/ /g' | sed 's/ \*\.[^ ]*//g' > newhostsfile.txt
```

Remove wildcards: `sed -i 's/\*//g'`

If any line has no hostnames after removing the wildcard, remove the line. 

Paste the output into `/etc/hosts`

Scan with Nuclei: `nuclei -l unique-hosts.txt -t http -o nuclei2.txt -H "User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"`

Scan with Nessus: Format the previous output for the hosts file into a target format that Nessus understands: `www.target.com[1.2.3.4]`:

```bash
while read -r hostname; do ip=$(awk -v host="$hostname" '$0 ~ host {print $1; exit}' hostsfile.txt); if [[ -n $ip ]]; then echo "${hostname}[${ip}]"; fi; done <newhostsfile.txt > nessusformattedtargets.txt
```

Take the output from this command and paste it into the Nessus scan targets.
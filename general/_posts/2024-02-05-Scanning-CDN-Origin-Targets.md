---
layout: post
title: Scanning CDN Origin Targets
description: >
  How to work around DNS resolving your external pentest targets to CDN (Akamai, Cloudflare) to run targeted scans based on TLS certificate names.
sitemap: true
---

You're on an external penetration test. Your target company has an IP range or CIDR network address in scope and they use reverse proxies, meaning any particular IP address may be hosting multiple websites and routing traffic to backend web server targets based on domain name in the Host header. 

The first thing you need to do is enumerate the TLS certificate Subject Alternative Names (SAN) to find the hostnames that each IP address is routing to backend servers. Once that is complete, if you run `nslookup`, `host`, or `dig` on each hostname, you may find that it's resolving to `akamaiedge.net` or other CDN provider.

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
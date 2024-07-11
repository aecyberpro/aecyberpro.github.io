---
layout: post
title: Finding CDN Origin Targets
description: >
  This article demonstrates how to find CDN (Akamai & Cloudflare) web application origin servers. If these origin servers aren't properly limiting source IP addresses to the CDN, you can hard-code the name to IP address mapping in your hosts file to bypass the WAF.
sitemap: true
---

This works to bypass WAF's when a company hosts a website through a CDN such as Cloudflare or Akamai. There are two approaches:

- You have a hostname which resolves to Cloudflare or Akamai. In this case, go to the "Use Censys" header.
    
- You have an IP address or network address. Go to the "Use Nuclei" header to discover applications hosted on these IP addresses, then hard-code the hostname to IP address mapping in your `/etc/hosts` file, or in Burp settings.
    

Note: If you’re able to find an origin server and test it directly without being filtered by the CDN WAF, this should be a finding. See “Remediation” below.

# Use Censys

You should be able to create a free [Census.io](http://census.io/ "http://Census.io") account.

To search for a specific SAN, you can use the following query: (Select Certificates in search dropdown menu) `parsed.extensions.subject_alt_name.dns_names: example.com`

You can also use wildcards to search for SANs that match a specific pattern:  
`parsed.extensions.subject_alt_name.dns_names: *.example.com`

To find the IP addresses using the certificates found in the previous searches, use the following search: (Select Hosts in the search dropdown menu)  
`services.tls.certificate.fingerprint_sha256: [sha-256 hash]`

# Use Nuclei

`nuclei -t /Users/lpha3ch0/nuclei-templates/ssl/ssl-dns-names.yaml -l [file with iP or network addresses] | -u [IP address]`

# Remediation

**Configuring Akamai**

For Akamai, you can use the Origin IP Access Control List (ACL) to restrict access to your origin server to only Akamai-controlled IP addresses. Akamai maintains a small and stable list of IP addresses that you use in policy rules in your origin server’s firewall. Here are the steps:

1. **Get the IP addresses**: You need to add the IP addresses from the Akamai list to your firewall. These addresses can be found and updated periodically through Akamai’s Site Shield maps.
    
2. **Set it up**: Add the Origin IP ACL to your delivery configuration.
    
3. **Review and maintain**: Regularly update your firewall rules to reflect any changes in the IP addresses listed by Akamai.
    

For detailed instructions, you can refer to Akamai’s documentation on [Origin IP Access Control List](https://techdocs.akamai.com/origin-ip-acl/docs/welcome "https://techdocs.akamai.com/origin-ip-acl/docs/welcome") and [updating your firewall](https://techdocs.akamai.com/site-shield/docs/update-your-firewall-with-the-latest-ip-addresses "https://techdocs.akamai.com/site-shield/docs/update-your-firewall-with-the-latest-ip-addresses")  .

**Configuring Cloudflare**

For Cloudflare, you can allow traffic only from Cloudflare’s IP ranges to your origin server. Cloudflare provides a list of IP ranges that their edge servers use, which you can add to your firewall rules. Here’s how:

1. **Get the IP addresses**: Obtain the list of Cloudflare IP ranges from the [Cloudflare IP Ranges page](https://www.cloudflare.com/ips/ "https://www.cloudflare.com/ips/").
    
2. **Update your firewall**: Configure your firewall to allow incoming connections only from these IP addresses. This can be done using various firewall software, including iptables, firewalld, or the firewall settings in your cloud provider’s control panel.
    

**Linux (iptables)**:
```
# Example for Cloudflare IP ranges
for ip in $(curl https://www.cloudflare.com/ips-v4); do
  iptables -A INPUT -p tcp -s $ip --dport 80 -j ACCEPT
  iptables -A INPUT -p tcp -s $ip --dport 443 -j ACCEPT
done

for ip in $(curl https://www.cloudflare.com/ips-v6); do
  ip6tables -A INPUT -p tcp -s $ip --dport 80 -j ACCEPT
  ip6tables -A INPUT -p tcp -s $ip --dport 443 -j ACCEPT
done

# Default rule to drop other traffic
iptables -A INPUT -p tcp --dport 80 -j DROP
iptables -A INPUT -p tcp --dport 443 -j DROP
```

**Nginx Configuration**:
```
server {
    listen 80;
    listen 443 ssl;
    
    ssl_certificate /path/to/ssl_certificate.crt;
    ssl_certificate_key /path/to/private_key.key;

    # Allow Cloudflare IP ranges
    include /etc/nginx/cloudflare-ips.conf;

    location / {
        # Your existing configuration
    }
}
```

Where /etc/nginx/cloudflare-ips.conf contains:
```
allow 173.245.48.0/20;
allow 103.21.244.0/22;
allow 103.22.200.0/22;
allow 103.31.4.0/22;
allow 141.101.64.0/18;
allow 108.162.192.0/18;
allow 190.93.240.0/20;
allow 188.114.96.0/20;
allow 197.234.240.0/22;
allow 198.41.128.0/17;
allow 162.158.0.0/15;
allow 104.16.0.0/12;
allow 172.64.0.0/13;
allow 131.0.72.0/22;
deny all;
```
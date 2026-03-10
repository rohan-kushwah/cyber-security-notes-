# Reverse Name Server (NS) Lookup

## What Is It?

Reverse NS Lookup helps identify all domains using a specific Name Server (NS). It's a power move in:

• Reconnaissance  
• OSINT investigations  
• Infrastructure mapping  
• Bug bounty pivoting

## What It Can Reveal

• Other domains hosted with the same provider  
• Hidden sub-brands, dev/staging environments  
• Related assets tied to the same DNS infrastructure  
• Pivot paths in an organization's digital footprint

## How to Find NS Records

### Using `dig`

```
dig armourinfosec.com ns
```

### Using `nslookup`

```
nslookup -type=ns armourinfosec.com
```

### Example Output

```
ns1.hostingprovider.com
ns2.hostingprovider.com
```

## Online Reverse NS Lookup Tools

### [ViewDNS Reverse NS Lookup](https://viewdns.info/reversens/)

• Input a name server hostname (e.g., `ns1.hostingprovider.com`)  
• Get a list of all domains using that NS  
• Great for shared hosting recon or mapping digital infrastructure

### [SecurityTrails](https://securitytrails.com/)

• Powerful passive DNS database  
• Shows historical domain – NS relationships  
• Also includes subdomains, WHOIS, DNS, and tech stack info

### [DNSDumpster](https://dnsdumpster.com/)

• Free passive DNS tool for domain mapping  
• Can indirectly reveal NS and A/CNAME relationships

## Example Use Case

If `armourinfosec.com` uses:

```
NS: ns1.examplehost.com
```

A reverse NS lookup may reveal domains like:

• `site1.com`  
• `companybeta.org`  
• `dev-clientproject.net`

You've just uncovered sibling infrastructure or hidden environments!

## Blue Team Defense Tips

• Use private or dedicated name servers for sensitive domains

• Avoid generic/shared DNS for critical infrastructure

• Use DNS providers that support obfuscation (e.g., [Cloudflare](https://www.cloudflare.com/))

• Monitor reverse DNS lookups and NS leaks via tools like:
  - [SecurityTrails Monitor](https://securitytrails.com/)
  - [Spyse](https://spyse.com/)
  - [GreyNoise](https://www.greynoise.io/)

----
----
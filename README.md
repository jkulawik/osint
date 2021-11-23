# OSINT cheat sheet

Recipes for open source intelligence.

## Subdomain enumeration

Find top level domains:
* Company websites
* RIPE
* Spyse.com

Store TLDs in a file (`subs` will be used in command examples).

Gather subdomains for one TLD:
* `sublist3r -d example.org -o subl.txt`
* `subfinder -d example.org -o subf.txt`
* `amass enum -d example.org -o amass.txt`

Bulk TLD subdomain gathering:
* `subfinder -dL subs -o subf.txt`
* `amass enum -df subs -o amass.txt`

Gather all domains in one file: `*.txt >> all.subs`

Get unique subs: `sort -u all.subs > unique.subs`

Passive nmap pseudoscan to check which domains point to IPs (+grep to only save resolved ones): `nmap -sL -iL unique.subs | grep Nmap > resolved.nmap`

Use a text editor or smth else to remove unneeded text via regex. This can also be used to replace that text with commas and make the file work as CSV with subdomain names and IPs.

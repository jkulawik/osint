# OSINT cheat sheet

Recipes for open source intelligence.

## Leaks

Check leaks on these websites:
* [IntelX](https://intelx.io)
* [Dehashed](https://dehashed.com)
* [haveibeenpwned.com](https://haveibeenpwned.com/) (desperate)

## Subdomain enumeration

Find domains:
* Company websites
* RIPE (also IP blocks - always validate whether they belong to client though)
* Spyse.com
* crt.sh

Gather subdomains for a single domain:
* `sublist3r -d example.org -o subl.txt`
* `subfinder -d example.org -o subf.txt`
* `amass enum -d example.org -o amass.txt`

Bulk subdomain gathering from a list file:
* `subfinder -dL domains -o subf.txt`
* `amass enum -df domains -o amass.txt`

Gather all domains in one file: `cat *.txt >> all.subs`

Get unique subs: `sort -u all.subs > unique.subs`

Passive nmap pseudoscan to check which domains point to IPs: `nmap -sL -iL unique.subs -oN resolved.nmap`

Use a text editor or smth else to remove unneeded text via regex. This can also be used to replace that text with commas and make the file work as CSV with subdomain names and IPs.

## Shodan

Look up open ports, tech and potential vulns on Shodan. Extract unique IPs from subdomain recon and then use https://github.com/emresaglam/shodan-bulk-ip-query. The code needs tailoring and writing a parser though.

Alternatively, [nrich](https://gitlab.com/shodan-public/nrich) can be used. Unfortunately it lists ports and CVEs on a per IP basis, making it harder to do meaningful analysis of the results.

## Manual testing

Check for Wordpress installations:
* `/wp-admin/`
* `/wp-json/wp/v2/users/` - passive user enumeration (if not disabled)

Potentially interesting info:
* `/robots.txt`
* `/sitemap.xml`

## E-mail gathering

Get e-mails from:
* Leaks
* Company LinkedIn
* https://hunter.io

Additionally, [e-mails can be verified on an SMTP server using `mail to` messages](https://mailtrap.io/blog/verify-email-address-without-sending/). Obviously this isn't pure OSINT anymore though.

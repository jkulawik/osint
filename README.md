# OSINT cheat sheet

Recipes for open source intelligence.

## Leaks

Check leaks on these websites:
* [IntelX](https://intelx.io)
* [Dehashed](https://dehashed.com)
* [haveibeenpwned.com](https://haveibeenpwned.com/) (desperate)

## Subdomain enumeration

Find top level domains:
* Company websites
* RIPE (also IP blocks - always validate whether they belong to client though)
* Spyse.com

Store TLDs in a file (`tlds` will be used in command examples).

Gather subdomains for one TLD:
* `sublist3r -d example.org -o subl.txt`
* `subfinder -d example.org -o subf.txt`
* `amass enum -d example.org -o amass.txt`

Bulk TLD subdomain gathering:
* `subfinder -dL tlds -o subf.txt`
* `amass enum -df tlds -o amass.txt`

Gather all domains in one file: `cat *.txt >> all.subs`

Get unique subs: `sort -u all.subs > unique.subs`

Passive nmap pseudoscan to check which domains point to IPs (+grep to only save resolved ones): `nmap -sL -iL unique.subs | grep Nmap > resolved.nmap`

Use a text editor or smth else to remove unneeded text via regex. This can also be used to replace that text with commas and make the file work as CSV with subdomain names and IPs.

## Shodan

Look up open ports, tech and potential vulns on Shodan. Extract unique IPs from subdomain recon and the use https://github.com/emresaglam/shodan-bulk-ip-query. The code needs tailoring and writing a parser though.

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

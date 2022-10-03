# OSINT cheat sheet

Recipes for open source intelligence.

## Leaks
* [IntelX](https://intelx.io)
* [Dehashed](https://dehashed.com)

## Subdomain enumeration

Find domains:
* Company websites
* RIPE (also IP blocks - always validate whether they belong to client though)
* Spyse.com
* crt.sh
* whois, perhaps

Gather subdomains for a single domain:
* `sublist3r -d example.org -o subl.txt`
* `subfinder -d example.org -o subf.txt`
* `amass enum -d example.org -o amass.txt`

Bulk subdomain gathering from a list file:
* `subfinder -dL domains -o subf.txt`
* `amass enum -df domains -o amass.txt`

Use `dnsenum` or `dnsrecon` with amass wordlists (`wordlists` prints their location) to brute force subdomains.

Gather all domains in one file: `cat *.txt >> all.subs`

Get unique subs: `sort -u all.subs > unique.subs`

Try to resolve addresses with nmap to check which sites are still up: `nmap -sL -iL unique.subs -oN resolved.nmap`

Use a text editor or smth else to convert to CSV.

## Shodan

Look up open ports, tech and potential vulns on Shodan. Extract unique IPs from subdomain recon and then use https://github.com/emresaglam/shodan-bulk-ip-query. The code needs tailoring and writing a parser though.

Alternatively, [nrich](https://gitlab.com/shodan-public/nrich) can be used. User the `ndjson` format to easily convert it to CSV with `sed`:
```
nrich -o ndjson ip_file > nrich.json
```

```
sed 's/{"cpes"://g; s/"hostnames"://g; s/"ip"://g; s/"ports"://g; s/"tags"://g; s/"vulns"://g; s/}//g' nrich.json > nrich.csv
```

## Semi-active website recon

* wafw00f
* whatweb -- identifies web technologies, including CMS and a bunch of other stuff
* wpscan in passive mode
* eyewitness: `eyewitness --jitter 3 --delay 2 --user-agent "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:104.0) Gecko/20100101 Firefox/104.0" -d output_dir -f urls.txt`. Accepts nmap input!
* URL wordlist attack with `gobuster dir` or `dirb` (requires scripting to do en masse because they normally only accepts single domains). Check out dirb and dirbuster wordlists for this
* If the server software can be determined, `/wordlists/dirb/vulns/` has vuln-related URLs to scan for Apache, IIS etc (also use `gobuster`).

## Website scanners

* skipfish? Creates interactive reports and does crawling
* nikto (slow and results are rarely good)

## Manual website recon

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

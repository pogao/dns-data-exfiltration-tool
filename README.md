# dns-data-exfiltration-tool
DNS Exfiltration tool is a very simple shell-script which has the goal of
helping you exfiltrate the data of a host using DNS.

For this to work you'll need to control a public and Internet accessible DNS
server on the Internet.

This script will basically do invalid DNS queries which you should log. Later
you'll check the log file from the DNS service and parse all these invalid
queries looking for the text in the "TAG" variable so you can pick up the
"hostname" part of it - which is where the exfiltrated data will be.

This script contains the part which should be executed in the compromised
host, from which you'll want to exfiltrate information. On the DNS server logs
(which will depend on which one you use) look for the TAG string using egrep
and extract those to another file, like so:
egrep -o "[0-9a-f]*.exfiltrated.domain.com" /var/log/query.log | cut -d. -f1 | uniq >> /tmp/info.txt

Now you just need to reassemble the information:
xxd -r -p < /tmp/info.txt > exfiltrated-information.txt

Pretty simple, huh? :)

developed by @pogao

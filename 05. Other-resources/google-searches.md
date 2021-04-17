
# Google Search Commands

## https://github.com/six2dez/degoogle_hunter
degoogle_hunter.sh company.com

## Google dorks helper
https://dorks.faisalahmed.me/

## Code share sites
site:http://ideone.com | site:http://codebeautify.org | site:http://codeshare.io | site:http://codepen.io | site:http://repl.it | site:http://jsfiddle.net "company"
## GitLab/GitHub/Bitbucket
site:github.com | site:gitlab.com | site:bitbucket.org "company"
## Stackoverflow
site:stackoverflow.com "target.com"
## Project management sites
site:http://trello.com | site:*.atlassian.net "company"
## Pastebin-like sites
site:http://justpaste.it | site:http://pastebin.com "company"
## Config files
site:target.com ext:xml | ext:conf | ext:cnf | ext:reg | ext:inf | ext:rdp | ext:cfg | ext:txt | ext:ora | ext:env | ext:ini
## Database files
site:target.com ext:sql | ext:dbf | ext:mdb
## Backup files
site:target.com ext:bkf | ext:bkp | ext:bak | ext:old | ext:backup
## .git folder
inurl:"/.git" target.com -github
## Exposed documents
site:target.com ext:doc | ext:docx | ext:odt | ext:pdf | ext:rtf | ext:sxw | ext:psw | ext:ppt | ext:pptx | ext:pps | ext:csv
## Other files
site:target.com intitle:index.of | ext:log | ext:php intitle:phpinfo "published by the PHP Group" | inurl:shell | inurl:backdoor | inurl:wso | inurl:cmd | shadow | passwd | boot.ini | inurl:backdoor | inurl:readme | inurl:license | inurl:install | inurl:setup | inurl:config | inurl:"/phpinfo.php" | inurl:".htaccess" | ext:swf
## SQL errors
site:target.com intext:"sql syntax near" | intext:"syntax error has occurred" | intext:"incorrect syntax near" | intext:"unexpected end of SQL command" | intext:"Warning: mysql_connect()" | intext:"Warning: mysql_query()" | intext:"Warning: pg_connect()"
## PHP errors
site:target.com "PHP Parse error" | "PHP Warning" | "PHP Error"
## Login pages
site:target.com inurl:signup | inurl:register | intitle:Signup
## Open redirects
site:target.com inurl:redir | inurl:url | inurl:redirect | inurl:return | inurl:src=http | inurl:r=http
## Apache Struts RCE
site:target.com ext:action | ext:struts | ext:do
## Search in pastebin
site:pastebin.com target.com
## Linkedin employees
site:linkedin.com employees target.com
## Wordpress files
site:target.com inurl:wp-content | inurl:wp-includes
## Subdomains
site:*.target.com
## Sub-subdomains
site:*.*.target.com
#Find S3 Buckets
site:.s3.amazonaws.com | site:http://storage.googleapis.com | site:http://amazonaws.com "target"
## Traefik
intitle:traefik inurl:8080/dashboard "target"
## Jenkins
intitle:"Dashboard [Jenkins]"
## Use shodan CLI tool for fast searching
pip install shodan\
shodan search "query"

### Find API Keys in HTML Body of publicly exposed pages. xoxb- for slack.
http.html:"xoxb-"

### Search for public Jenkins server using favicon hash

### https://github.com/sansatart/scrapts/blob/master/shodan-favicon-hashes.csv 
http.favicon.hash:81586312

### Find all Grafana Dashboards using website's title
http.title:"Grafana"

### Search for default passwords for a particular organisation
"default password" org:"orgName"

### Open and Login Success FTP Ports
"230 Login Successful" port:21 org:orgName 

### FTP Anonymous Login
230 'anonymous@' login ok org:orgName

### Finding wp-config.php in public
http.html:"The wp-config.php creation script uses this file"

### Finding targets using ASN
asn: AS714

### Increasing attack surface with ssl certificate
ssl.cert.subject.cn:apple.com
ssl.cert.issuer.cn: apple.com

### Finding particular backend component
http.component:AngularJS org:orgName

### Directory Listing
http.title:"Index Of /"\
http.title:"Index Of /admin"\
http.title:"Directory Listing" org:orgName

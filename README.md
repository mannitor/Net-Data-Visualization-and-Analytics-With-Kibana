# Net-Data-Visualization-and-Analytics-With-Kibana
To create Visualization and analytics from Rapid 7 Open Data using KIbana

About Opendata Rapid7 Project Sonar:
Security research project to find and expose common vulnerabilities.
Data available for security research.
About the Data:
Sonar collects SSL certificates visible on public IPv4 HTTPS web servers.
Data can be used to detect changes such as malicious replacement of certificates or reveal compromised previous certificates.
Other purposes include detection of insecurely reused or still actively used revoked certificates.
You can see all IP addresses / services that claim to represent a particular domain
Can be used for asset identification and detection of malicious certificate usage.
Note: certificate fields can be used for soft and hardware identification in specific situations
Also encompasses non-HTTP services such as SSL and STARTTLS-enabled email services like SMTP, IMAP, and POP.


Additional Use Cases:
Sonar performs HTTP studies to collect HTML content. It requests index page ("/") on port 80 and other ports.
Note: Sonar does not crawl the servers beyond initial requested page.
Potential use: Identification of compromised web servers and injected malicious HTML snippets such as "iframes" to non-advertisement web servers.
Can also use the data to id vulnerable embedded devices through fingerprinting the content and headers of HTTP responses.


Reverse DNS:
Data enables organizational asset discovery and can help id misconfigurations and possibly DNS hijacking attempts

Setup (Ubuntu 18.04):
Note: Everything is installed locally. Kibana configuration will allow over the network access later. Skip any mention of nginx. All important config files is going to be under "/etc/[APPS]"
YAML type files are extremely dependent on spacing and tab. Not recommended to shift data around.
Most of the setup is followed using the link below. The link will provide basic understanding of ELK stack.
URL: https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elastic-stack-on-ubuntu-18-04#step-1-%E2%80%94-installing-and-configuring-elasticsearch

Problems that have to be dealt with include installing java jdk.
Solution: Install Latest Java Jdk.

Once you have installed, elasticsearch, logstash, kibana, and filebeat, check each configuration.
For elasticsearch, make sure that the follow is exactly implemented, while keeping everything else the same:
"network.host: localhost"
 Do not adjust any config settings for logstash. If you followed the URL, then you would have created three config files for logstash. Edit the following file "10-syslog-filter.conf"
Edit Kibana config file to match the following:
"server.port: 5601"
"server.host: "0.0.0.0"
This will allow you to access kibana from over the network. Simply find the IP by typing "ifconfig" and using that IP in the following link in your browser
"[IPFOUND]:5601" in the URL
 Edit filebeat config file to match the following:
This will allow filebeat to read through all text files inside multiple sub-directories
Scroll down to Elasticsearch output and make sure to comment out as follows
"#output.elasticsearch:"
#hosts:["localhost:9200]"
Scroll down to logstash output and make the following changes:
"output.logstash:"
hosts: ["localhost:5044"]
If you used the following URL provided above, then you may have enabled system module for filebeat
Edit the following to ensure continuous reading from same folder.

Var.paths should have the same path as your filebeat config path.
Now you are done with Configuration. To start up everything, use the following commands (substitute {APPS} for the following names, elasticsearch, logstash, kibana, filebeat):
"sudo systemctl start {APPS}"
"sudo systemctl enable {APPS}"
To check if its running:
"sudo service {APPS} status"
To stop service
"sudo systemctl stop {APPS}

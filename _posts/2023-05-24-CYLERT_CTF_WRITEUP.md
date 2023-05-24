
Cylert CTF Challenge Writeup
Hello everyone, it was my pleasure to meet you all at the CDIS Conference last week.
I had the opportunity to make a simple network challenge and I was delighted to have good feedback about the challenge. The challenge is aimed for beginner to early intermediate CTF players. Let’s dive to the writeup right away.
The challenge was hosted on the local network on the IP 111.111.10.209 but I wrote this writeup on my local network so the IP will be different in screenshots.
We start with the basic Nmap scan using the -p- argument to scan All ports.
![image](https://github.com/offalltn/blog/assets/110370549/da199260-ed15-4137-9d8f-434a7c06c95c)

We have 2 open ports 80 and 5374. Scanning the strange port with Nmap scripting engine we had the following results.
![image](https://github.com/offalltn/blog/assets/110370549/995b2f47-a4b1-4936-8da4-a47380daaaaa)


It has ftp service running and anonymous login is allowed. Lets try logging in.
![image](https://github.com/offalltn/blog/assets/110370549/9eb00f91-ad81-4aa6-966f-431f5dcc0d65)


We tried to execute simple commands but as you can see in the above screenshot it says it is entering passive mode then nothing happens.
So, we disabled passive mode as you can see below.
![image](https://github.com/offalltn/blog/assets/110370549/5ba0703b-8caf-48d0-b63a-1d350d125d40)


Now commands are executed normally and we managed to list files on the box. We finally got our first flag.
![image](https://github.com/offalltn/blog/assets/110370549/be8b6fa9-94ca-4b3c-a9cf-46f8089ea01a)


Then we tried to list hidden files in the “files” directory and we got a file called keys.
![image](https://github.com/offalltn/blog/assets/110370549/c2a0c824-4a38-4d4c-868d-992df3d6d08e)


This file contained what seemed like ports.
![image](https://github.com/offalltn/blog/assets/110370549/650eee75-1f27-42c8-9649-37fa70e66ab7)

We will leave this for now and move to port 80. After visiting site we were faced with the below response.
![image](https://github.com/offalltn/blog/assets/110370549/9d0dead2-b069-4f3b-a008-c15490d8ff97)


Which means that we need to modify our /etc/hosts to point to this virtual host name.
![image](https://github.com/offalltn/blog/assets/110370549/298ea19e-0f31-4a1d-87ea-6854baeb8fd6)



 After that we can navigate to this host where we find a company page called “Safe Cam”.
 ![image](https://github.com/offalltn/blog/assets/110370549/44b1263f-0e6c-4478-8f0d-63394aa1b1c1)


Directory busting on the web servers may be essentials to reveal hidden files or directories on the web server so we ran “dirsearch” tool and we found an interesting test.txt file.
![image](https://github.com/offalltn/blog/assets/110370549/9cfd7b9f-e535-459d-ae45-d52c58da0493)

Navigating to this file revealed another virtual host which is Cylert.host.com.
![image](https://github.com/offalltn/blog/assets/110370549/6d9413fa-2fd9-4054-8d2a-ece194dbc499)

Adding this host to the /etc/hosts made this host available to us.
Navigating to this virtual host reveals another website called greenhouse for hosting services.
Also using “dirsearch” tool on this virtual host reveal the second flag. As you can see below.
![image](https://github.com/offalltn/blog/assets/110370549/efbb621f-e22b-4759-af3c-c32082694254)
![image](https://github.com/offalltn/blog/assets/110370549/4261780d-4f3e-4aee-beaa-9867319dd2fd)


The source code of this web site page reveals an interesting info.
![image](https://github.com/offalltn/blog/assets/110370549/cf88d28e-92ad-4e38-931d-c289f7cc5277)


With this info and the hidden “keys” file we were sure that this box has port knocking technique enabled. And the sequence of ports that we had earlier is the key to open the SSH service.
We used NMAP to do the knocking part as you can see below.
![image](https://github.com/offalltn/blog/assets/110370549/5bca618d-bd73-4eb6-9d31-c6be448c1fd3)


When we do NMAP scan again we find that SSH port is now open.
![image](https://github.com/offalltn/blog/assets/110370549/7006b571-adc5-44d0-bd2f-d2308d11a404)


But we don’t have credentials to access the SSH service. We must have missed something. 
So Going through the Safe Cam website we find 2 pieces of info. The box uses virtual machines form OSBOXES website.
![image](https://github.com/offalltn/blog/assets/110370549/782cabe1-2bab-4e33-b314-6ae8b8b595c8)


This info was also mentioned at the footer of the web page.
![image](https://github.com/offalltn/blog/assets/110370549/7e9f042b-b41d-4d97-89ad-4b00e6e187f6)


Using this info, we head to google to search about OSBOXES and we found that the default credentials for the virtual machines is “osboxes.org” with the username OSBOXES. 
![image](https://github.com/offalltn/blog/assets/110370549/98da900a-1f57-4546-b5fa-e7e90cfc1b08)

So eventually we try this combination of username and password on the newly found SSH service and we are in! And we got the last Flag.
![image](https://github.com/offalltn/blog/assets/110370549/544c1ae8-246c-403b-a270-94a339138e0d)


And now we come to the end of this writeup. I really enjoyed making this challenge and I hope you all enjoyed solving it at the conference.

---
published: true
---
Hi there!  
Nowadays, everyone needs a **VPN**... Bolow I explain and show how to do it from **scratc**.

## Getting a server (run)
The easiest way to get it is to [register a free 1 year Amazon account](https://portal.aws.amazon.com/billing/signup). It's a very generous offer from amazon, 1 year of free server using, isn't it?

Well, [here is step by step instruction](https://aws.amazon.com/getting-started/tutorials/launch-a-virtual-machine/) of how to do it. Except a two moments:
1. Choose **Ubuntu 16.04**  not Amazon Machine Image (AMI)
2. While configuring you should set security group like here I have:
![security group of instance inbound all]({{site.baseurl}}/imgs/screenshot-us-east-2.console.aws.amazon.com-2018.05.12-08-42-31.png)
![security group of instance outbound all]({{site.baseurl}}/imgs/screenshot-us-east-2.console.aws.amazon.com-2018.05.12-08-44-01.png)

Ususally, it takes 5 minutes to run and connect to a server.

## Installation proxy enviroment
To continue you should be connected to your server like this:
![running ssh remote connection]({{site.baseurl}}/imgs/Screenshot_2018-05-12_08-54-05.png)
If so, just follow this steps:
1. type `sudo apt-get update`
2. type `sudo apt-get upgrade`
3. type `sudo apt-get install squid`

## Configuration of a basic proxy server
The configuration of the **Squid Proxy Server** is handled in the _/etc/squid/squid.conf_. I will show you how to configure a very basic proxy server:
1. type `nano /etc/squid/squid.conf`
2. press Ctrl/Cmd + W and type `http_access deny all` 
3. change `http_access deny all` to `http_access allow` 
4. press Ctrl/Cmd + O to save and Ctrl/Cmd + W to exit
5. And type `sudo service squid restart` to restart server with new configurations

> Remark: Ctrl/Cmd means key Ctrl for Windows and Cmd for Mac

## Connecting and Testing our VPN
Before closing ssh connection type in it `curl 'https://ipinfo.io/ip'`.
You will recieve public ip addres of testable server.  Don't forget to copy the value.

Now go to proxy settings of your browser or of operating system and input recently received IP + port: _3128_.

## Conclusion
That's it. Hope you like your result and it took not more time than I have the article writing =).
Also, I will be happy if you leave a comment about the article. Piece!

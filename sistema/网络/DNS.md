what happens when you type a website address in your browser?

## DNS

The domain name system



### what?

- Computers and other devices communicate using IP addresses to identify each other on the internet. (e.g., 10.0.0.1、192.168.1.0)；
- But humans can't remember IP addresses, so they use words.（e.g., wikipedia.org）；
- The domain name system (DNS) brings the two together and gets you to your destination.



### how?

![DNS](/Immagine/DNS.svg)



步骤：

1. 用户输入网址，比如：http://dnsimple.com
2. 浏览器查询缓存，没有=>步骤3，有=> 步骤n
3. 浏览器询问操作系统，操作系统查询缓存，没有=>步骤4，有=>步骤11
4. 操作系统询问Resolver，Resolver查询缓存，没有=>步骤5，有=>步骤10
5. Resolver定位到Root
6. Root帮助Resolver定位到TLDs，比如对应这个网址的：.COM
7. TLDs给出 the authoritative name servers 列表，比如：
   1. ns1.dnsimple.com:[198.241.10.51] 
   2. ns2.dnsimple.com:[162.159.25.4]
   3. ns3.dnsimple.com: [50.31.242.53]
   4. ns4.dnsimple.com: [162.159.27.4]

8. Resolver来到其中一个the authoritative name server，得到IP
9. Resolver告知操作系统网址对应的IP，操作系统存储该值
10. 浏览器缓存网址对应的IP
11. 浏览器访问网址对应的服务器
12. 打开网页



### 名词解释

- Resolver：即**互联网服务供应商**

	> The resolver server is usually your ISP (Internet Service Provider).

- Root：**Root位于DNS架构的顶部**

  > 1. This root server is just one of the 13 root name servers that exist today. They are named [letter].root-servers.net where [letter] ranges from A to M.
  > 2. They are scattered around the globe and operated by 12 independent organisations.
  > 3. Root servers sit at the top of the DNS hierarchy.

- TLDs(Top-Level Domains)：

  > The coordination of most top-level domains (TLDs) belong to the Internet Corporation for Assigned Names and Numbers (**ICANN**)

- Domain Registrar：负责把新出生的域名同步给对应的TLDs

  > When a domain is purchased,  the domain registrar reserves the name and communicates to the TLD registry the authoritative name servers.

- The Authoritative name servers：

  > No cached values. Not asking someone else. Only the real deal.



## 参考链接

[how dns works](https://howdns.works/)

[DNS in One Picture](https://roadmap.sh/guides/dns-in-one-picture)



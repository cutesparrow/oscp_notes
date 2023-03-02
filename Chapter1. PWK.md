# OSCP 网络环境配置经验
Best practice for chinese students：
1. buy a shadowsocks service like bywave
2. buy one vps in USA
3. setup openvpn service in vps
4. connect to the vps by openvpn with shadowsocks as the proxy（that openvpn should use tcp protocol rather than udp）
5. connect to lib or exam environment with the ovpn file provided by offensive security
之所以要先用tcp 协议的openvpn链接是因为 udp协议的openvpn不能挂代理。
优点：网络稳定性较好，几乎不存在掉线或者网络抖动的情况
缺点：需要购买两个服务，成本高一点，大约100rmb每月


MefaCorpone.com 是一个靶场域名
Sandbox.local 是一个虚拟的公司内网域

靶机的flag文件为：proof.txt/network-secret.txt
通过Control Panel输入network-secret.txt 可以解锁新的子网，继续渗透

攻击者掌握三个客户机：Windows 10/ Debian Linux/ Windows Server 2016 DC

# 报告
[word模板](https://www.offensive-security.com/pwk-online/OSCP-Exam-Report.docx)

# 笔记策略
1. 使用kali自带的截图软件在关键点进行截图保存，注意进行适当的命名，并按照靶机区分
2. 对于渗透过程，可以使用obsidian/typora 等markdown进行记录。同时，对于payload/shellcode 等关键代码需要保存或记录链接。
3. 如果渗透整体过程没有特殊步骤，可以在报告编写时再补充方法论相关内容。
4. 对于域渗透，一定要对整体的渗透步骤进行详细记录。
5. 重点是：关键点截图 代码 proof文件和截图。
6. 使用[terminal logger](https://github.com/tmux-plugins/tmux-logging)记录所有的命令
7. 每个环境单独启动一个Terminal窗口，其中使用tmux做分割，vpn单独启动窗口

# 考前准备
Make sure you have all your tools and links organized.
Make sure your methodology for enumeration is ready to go.
Have your report outlined and ready to fill in.
Make a snapshot of the VM you are planning on using

# 考试经验
Personally (and perhaps I overdid it but rather it so than failing), this was what I did prior to getting to the point where I had enough to pass plus some padding (80 pts):

1.  Logged all terminal output
2.  Divided my notes by machine into Enumeration, Exploitation and Privilege Escalation (and Lateral Movement for the AD set), and Command List (the exact successful commands needed to identify and compromise vulnerabilites)
3.  Copied all output that I felt successfully progressed the categories above to my notes, removing the extraneous ones if I ultimately hit a dead end
4.  Took a screenshot of only things I felt I'd need to remind myself of when re-compromising the boxes, as well as the local.txt and proof.txt shots with the user/ group and IP info as required by the exam doc.

When I hit 80 points, I:

1) Reverted all boxes

2) Used my command list to re-enumerate and re-compromise each machine, correcting the list if required. (I did not demonstrate any extraneous enumeration or dead ends)

3) Took screenshots of EVERY command and output

4) Provided links to all required exploit code and critical research results (e.g. if GTFO bins was used, I linked to the page)

5) Took screenshots of all modified exploit code snippets

6) Re-took proof screenshots

7) Built a single notes page of proofs organized by IP and made sure I had all the correct info and it matched submitted hashes

Then I went back to try for more points until I had enough. This all made the reporting very easy for me and mostly cut and paste. The only big addition for the report was vulnerability and mitigation descriptions, and for this I re-iterated and linked to any CVE advisory recommendations provided. For configuration type issues or generally bad practices I linked to associated CWEs and OSWAP language.


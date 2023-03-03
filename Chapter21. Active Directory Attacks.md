
IP - NTLM
域名 - Kerberos

TGT只能在本机使用，因为里面有IP信息，
ST可以在任何机器上使用，用于Pass the ticket 攻击

如果有了一个service account的hash，可以制造silver ticket获得服务内的高权限，并且这个ticket只和服务的SPN绑定（不像TGT，和请求者IP绑定），如果一个服务存在于多个主机（SPN相同），则利用silver ticket 可以在他们之间横向移动。

golden ticket其实是一种Persistence 手段，而不是攻击手段。
有了krbtgt之后，可以在任意机器上生成golden ticket，甚至是不在域内的主机

在有了golden ticket的上下文中，可以使用psexec 直接rce，其实就是一种over pass the hash，只不过这个hash 是krbtgt的hash

NTDS.dit 在域控上，保存了域内所有账号的信息，包括密码hash
# övning med nat

I den här övningen ska ni lära er hur nat (network address translation) fungerar och lite git, github.
Mer om nat [här](https://en.wikipedia.org/wiki/Network_address_translation#One-to-many_NAT).


börja med att clona det här projektet, öppna en terminal och gå till er *kurskatalog*

`git clone https://github.com/bojohan/nat.git`

nu har ni laddat ner övningen, gå in i katalogen


`cd nat`

Vagrant uppsättningen består av två maskiner, en client och en gateway, trafiken från clienten går via gatewayen ut på internet.

Ni kan starta bägge maskiner med

`vagrant up`

logga in på clienten med

`vagrant ssh client`

öppna en ny terminal och logga in på gatewayen

`vagrant ssh gateway`

Ni ska läsa logfilen på gatewayen och se hur trafiken vidarebefodras, det kan ni göra med följande kommando

`sudo tail -f /var/log/syslog|grep gateway`


gå nu tillbaka till den terminal där ni loggade in på clienten, hämta hem en data från valfri webbplats ex.

`curl sr.se`


tillbaka till terninal för gatewayen, ser ni något sådant här.

```bash
vagrant@gateway:~$ sudo tail -f /var/log/syslog|grep gateway Sep  4 21:46:26 vagrant-ubuntu-trusty-64 kernel: [   97.019845]
[gateway] IN=eth1 OUT=eth0 MAC=08:00:27:9d:1d:41:08:00:27:78:22:8c:08:00 SRC=192.168.0.2 DST=134.25.4.140 LEN=40 TOS=0x00
PREC=0x00 TTL=63 ID=18096 DF PROTO=TCP SPT=44269 DPT=80 WINDOW=29200 RES=0x00 ACK URGP=0
```

Nu är eran uppgift att förklara vad som händer, beskriv utifrån

- SRC, source
- DST, destination
- SPT, source port
- DPT, destination port


När ni är klar med uppgiften kan ni göra


```
exit
$ vagrant destroy -f
```

skriptet i vagrantfilen som bygger upp gatewayen, där har jag inspirerats av den  här  [sidan](https://help.ubuntu.com/community/Internet/ConnectionSharing)

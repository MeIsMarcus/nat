
$gateway_setup = <<SCRIPT
iptables -A FORWARD -o eth0 -i eth1 -s 192.168.0.0/24 -m conntrack --ctstate NEW -j ACCEPT
iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -t nat -F POSTROUTING
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE


iptables -I FORWARD 1 -j LOG --log-prefix='[gateway] '



iptables-save | sudo tee /etc/iptables.sav
awk  '/^exit 0/{print "iptables-restore < /etc/iptables.sav"}1' /etc/rc.local > tmp && mv tmp /etc/rc.local

sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
sed -i '/net.ipv4.ip_forward=1/s/^#//g' /etc/sysctl.conf







SCRIPT

$client_setup = <<SCRIPT

route del default
route add default gw 192.168.0.1

SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: "echo Hello"

  config.vm.define "gateway" do |gateway|
    gateway.vm.hostname = "gateway"
    gateway.vm.box = "ubuntu/trusty64"
    gateway.vm.network "private_network", ip: "192.168.0.1",
        virtualbox__intnet: "happynetwork"
    gateway.vm.provision "shell", inline: $gateway_setup
  end

  config.vm.define "client" do |client|
    client.vm.hostname = "client"
    client.vm.box = "ubuntu/trusty64"
    client.vm.network "private_network", ip: "192.168.0.2",
        virtualbox__intnet: "happynetwork"
    client.vm.provision "shell", inline: $client_setup
  end
end

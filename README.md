# Beschreibung
Mit Hilfe des folgenden Skripts wird eine virtuelle Maschine mit einem DHCP-Dienst aufgesetzt.

# Spezifikationen
IP = 192.168.50.10  
Hostname = M300_dhcp  
RAM = 1024 MB  
Dienst = DHCP  
Box = Debian  
```
Vagrant.configure(2) do |config|
config.vm.define "dhcp" do |dhcp|
    dhcp.vm.box = "debian/jessie64"
    dhcp.vm.hostname = "M300_dhcp"
    dhcp.vm.network "private_network", ip:"192.168.50.10" 
	dhcp.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
	dhcp.vm.provider "virtualbox" do |vb|
	  vb.memory = "1024"    
end
```
# Konfiguration
Im ersten Schritt muss das Paketverzeichnis installiert werden. Dazu führt man zuerst ein Update aus. Sobald das fertig ist kann man mit der Installation von DHCP beginnen:
```
sudo apt-get update
sudo apt-get -y install isc-dhcp-server
```
Danach müssen die Einstellungen für den DHCP-Dienst gesetzt werden. Diese sehen bei mir wie folgt aus:
```
sudo sed -i 's/example.org/test.local/g' /etc/dhcp/dhcpd.conf
sudo sed -i 's/ns2.test.local/8.8.8.8/g' /etc/dhcp/dhcpd.conf
sudo sed -i 's/#authoritative/authoritative/g' /etc/dhcp/dhcpd.conf
sudo sed -i '$asubnet 192.168.50.0 netmask 255.255.255.0 {' /etc/dhcp/dhcpd.conf
sudo sed -i '$arange 192.168.50.30 192.168.50.100 {' /etc/dhcp/dhcpd.conf
sudo sed -i '$aoption routers 192.168.50.1;' /etc/dhcp/dhcpd.conf
```
Der Server befindet sich in der Domain **"test.local"**  
Als DNS wird der Google DNS Server mit der IP Adresse **8.8.8.8** verwendet  
Er befindet sich im Netz **192.168.50.0** mit dem Suffix **/24**  
Sein Range erstreckt sich von **192.168.50.30 bis 192.168.50.100**  
Als Gateway wird **192.168.50.1** verwendet  

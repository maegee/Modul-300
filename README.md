# Beschreibung
Mit Hilfe des folgenden Skripts wird eine virtuelle Maschine mit einem DHCP-Dienst aufgesetzt.

# Server
IP = 192.168.50.10
Hostname = M300_dhcp
RAM = 1024 MB
Dienst = DHCP
Box = Debian
'''
config.vm.define "dhcp" do |dhcp|
    dhcp.vm.box = "debian/jessie64"
    dhcp.vm.hostname = "M300_dhcp"
    dhcp.vm.network "private_network", ip:"192.168.50.10" 
	dhcp.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
	dhcp.vm.provider "virtualbox" do |vb|
	  vb.memory = "1024"  
'''

# Aufsetzen
'''



```

```
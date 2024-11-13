#!/bin/bash


# Statik IP adresi ve diğer ağ yapılandırma bilgilerini burada tanımlayın
IP_ADDRESS="192.168.203.10"
NETMASK="24"
GATEWAY="192.168.203.255"
DNS_SERVERS="[8.8.8.8, 8.8.4.4]"


# Netplan YAML dosyasını düzenleme
sudo tee /etc/netplan/01-network-manager-all.yaml > /dev/null <<EOF
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:
      dhcp4: no
      addresses:
        - $IP_ADDRESS/$NETMASK
      gateway4: $GATEWAY
      nameservers:
        addresses: $DNS_SERVERS
EOF



# 5 saniye bekle
echo "Ağ yapılandırması tamamlandı. 5 saniye bekleniyor..."
sleep 5s

# Sistem güncelleme
sudo apt update && sudo apt upgrade -y


echo "Ansible yükleniyor..."



# Ansible'ı yükle
sudo apt install -y ansible

echo "Ansible başarıyla yüklendi."

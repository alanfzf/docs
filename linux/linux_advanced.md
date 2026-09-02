# Generation commands

## Generate a private and public SSH key

```bash
ssh-keygen -t ed25519 -C "example@gmail.com"
```

## Generate a random base64 string

```bash
cat /dev/urandom | head -c 64 | base64
```

# Misc commands

```bash
# Get free disk space
df -h
# Get RAM info
free -h
```

# Network commands

## Connect to WiFi network using nmcli

```bash
nmcli device wifi list
nmcli device wifi connect SSID_NAME password PASSWORD
```

## Set a static IP

```bash
cd /etc/netplan/
sudo vi 00-installer-config.yaml
# apply changes
sudo netplan apply
```

```yml
# change to this
network:
  renderer: networkd
  ethernets:
    ens33:
      addresses:
        - 192.168.1.247/24
      nameservers:
        addresses: [4.2.2.2, 8.8.8.8]
      routes:
        - to: default
          via: 192.168.1.1
  version: 2
```

## Reverse SSH Tunnel for MySQL Access

Opens port 3366 locally and forwards it to port 3306 on the remote host.

```bash
ssh -N -L 3366:127.0.0.1:3306 host
```

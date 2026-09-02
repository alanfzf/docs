# Tailscale setup

## AWS Node setup

```bash
sudo tee /etc/sysctl.d/99-tailscale.conf >/dev/null <<'EOF'
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding = 1
EOF
```

```bash
sudo tee /etc/systemd/system/tailscale-ethtool.service >/dev/null <<'EOF'
[Unit]
Description=Enable UDP GRO forwarding for Tailscale
After=network-online.target
[Service]
Type=oneshot
ExecStart=/sbin/ethtool -K ens5 rx-udp-gro-forwarding on
ExecStart=/sbin/ethtool -K ens5 rx-gro-list off

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable --now tailscale-ethtool.service
```

```bash
sudo tailscale up --advertise-routes=172.31.0.0/16
```

# OT-DNS-Playground

A minimal Docker Compose lab for on-prem OT DNS.

---

## 🚀 Quick Start

1. **Clone**

   ```bash
   git clone https://github.com/dirnberg/ot-dns-playground.git
   cd ot-dns-playground
   ```

2. **Create directories**

   ```bash
   mkdir logs pcaps
   ```

3. **Start**

   ```bash
   docker-compose up -d
   ```

   * **ot-dns**: DNS server on port 53 (UDP/TCP)
   * **dns-capture**: writes `pcaps/dns-queries.pcap` then exits

4. **Check status**

   ```bash
   docker-compose ps
   ```

---

## 📋 Logs & PCAP

* **DNS server logs**

  ```bash
  docker logs ot-dns
  tail -f logs/dnsmasq.log
  ```

* **View PCAP**

  ```bash
  tcpdump -r pcaps/dns-queries.pcap -n
  # or open in Wireshark
  ```

---

## ✏️ Host Entries

1. **Edit**

   ```text
   # e.g. in hosts:
   10.1.123.45  new-device.sb110.ele.at  new-device
   ```

2. **Reload without downtime**

   ```bash
   docker kill --signal=HUP ot-dns
   ```

3. **Verify**

   ```bash
   docker logs ot-dns | grep new-device
   ```

---

## 🔄 Restart

For changes to `dnsmasq.conf` or `cname.conf`:

```bash
docker-compose restart ot-dns
```

---

## 🔍 DNS Tests

```bash
# Forward lookup
nslookup bay1-controller1.sb110.ele.at 127.0.0.1

# Short name
nslookup new-device 127.0.0.1

# Reverse lookup
nslookup 10.1.110.1 127.0.0.1

# PowerShell
Resolve-DnsName -Name bay1-controller1.sb110.ele.at -Server 127.0.0.1
```

---

## 🛑 Stop & Cleanup

```bash
docker-compose down
```

---

Enjoy your on-prem OT DNS playground! 🚧🛠️

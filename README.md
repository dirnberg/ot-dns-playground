````markdown
# OT-DNS-Playground

A fully self-contained Docker Compose lab for on-prem OT DNS, featuring:

- **`ot-dns`**: a `dnsmasq` server serving your OT zone (`sb110.ele.at`)
- **`dns-capture`**: a one-shot `tcpdump` container capturing all DNS traffic into a PCAP

---

## 🚀 Quick Start

1. **Clone the repository**  
   ```bash
   git clone https://github.com/dirnberg/ot-dns-playground.git
   cd ot-dns-playground
````

2. **Create host directories**

   ```bash
   mkdir logs pcaps
   ```

3. **Launch the lab**

   ```bash
   docker-compose up -d
   ```

   * `ot-dns` listens on UDP/TCP port 53
   * `dns-capture` writes `pcaps/dns-queries.pcap` then exits

4. **Verify running containers**

   ```bash
   docker-compose ps
   ```

---

## 📋 Viewing Logs & Captures

* **DNS server logs**

  ```bash
  docker logs ot-dns
  tail -f logs/dnsmasq.log
  ```

* **Inspect captured DNS traffic**

  ```bash
  tcpdump -r pcaps/dns-queries.pcap -n
  # Or open `pcaps/dns-queries.pcap` in Wireshark
  ```

---

## ✏️ Adding or Updating Host Records

1. **Edit** the `hosts` file (or `dnsmasq.conf` for `host-record` lines):

   ```text
   # e.g. in hosts:
   10.1.123.45   new-device.sb110.ele.at   new-device
   ```

2. **Reload** dnsmasq without downtime:

   ```bash
   docker kill --signal=HUP ot-dns
   ```

3. **Confirm** the update:

   ```bash
   docker logs ot-dns | grep new-device
   ```

---

## 🔄 Restarting the DNS Service

If you change core config files (`dnsmasq.conf`, `cname.conf`), perform a full restart:

```bash
docker-compose restart ot-dns
```

---

## 🔍 Live DNS Queries

Test your new records or existing entries:

```bash
# Forward lookup
nslookup bay1-controller1.sb110.ele.at 127.0.0.1

# Short-name lookup
nslookup new-device 127.0.0.1

# Reverse lookup
nslookup 10.1.110.1 127.0.0.1

# PowerShell alternative
Resolve-DnsName -Name bay1-controller1.sb110.ele.at -Server 127.0.0.1
```

---

## 🛑 Teardown

Stop and remove all containers, networks and volumes:

```bash
docker-compose down
```

---

Enjoy your on-prem OT DNS playground! 🚧🛠️

```
```

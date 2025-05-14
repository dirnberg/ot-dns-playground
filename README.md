# OT-DNS-Playground

A self-contained Docker Compose lab that runs:
- **`ot-dns`**: a dnsmasq server serving your OT zone (`sb110.ele.at`)  
- **`dns-capture`**: a one-shot tcpdump container capturing all DNS traffic to a PCAP  

## 🚀 Quick Start

1. **Clone the repo**  
   ```bash
   git clone https://github.com/dirnberg/ot-dns-playground.git
   cd ot-dns-playground
````

2. **Prepare directories**

   ```bash
   mkdir logs pcaps
   ```

3. **Launch services**

   ```bash
   docker-compose up -d
   ```

   * `ot-dns` listens on UDP & TCP port 53
   * `dns-capture` writes `pcaps/dns-queries.pcap` then exits

4. **Verify containers are running**

   ```bash
   docker-compose ps
   ```

---

## 📋 Checking Logs

* **dnsmasq startup & query logs**

  ```bash
  docker logs ot-dns
  tail -f logs/dnsmasq.log
  ```
* **Capture summary**

  ```bash
  tcpdump -r pcaps/dns-queries.pcap -n
  # or open in Wireshark
  ```

---

## ✏️ Adding a New Host Record

1. **Edit** `hosts` (or `dnsmasq.conf` if using `host-record` lines):

   ```text
   # example in hosts:
   10.1.123.45  new-device.sb110.ele.at  new-device
   ```
2. **Reload dnsmasq** (no downtime):

   ```bash
   docker kill --signal=HUP ot-dns
   ```
3. **Confirm** in logs:

   ```bash
   docker logs ot-dns | grep new-device
   ```

---

## 🔄 Restarting the DNS Service

If you’ve changed core config (`dnsmasq.conf`, `cname.conf`), a full restart is easiest:

```bash
docker-compose restart ot-dns
```

---

## 🔍 Feature: Live DNS Queries

Send queries against your local DNS:

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

## 🛑 Stopping the Lab

```bash
docker-compose down
```

This stops and removes all containers, networks and the one-shot capture service.

---

Enjoy your on-prem OT DNS playground! 🚧🛠️

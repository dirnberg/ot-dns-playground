````markdown
# ot-dns-playground

A hands-on Docker playground for spinning up lightweight, authoritative **dnsmasq** servers in real-world OT scenarios: Substation, Production Plant, Hospital Devices, Building Automation, and CCTV.

## 🔧 Prerequisites

- Docker & Docker Compose installed  
- Basic familiarity with environment variables

## 🚀 Quick Start

1. **Clone the repo**  
   ```bash
   git clone https://github.com/<your-org>/ot-dns-playground.git
   cd ot-dns-playground
````

2. **Choose a scenario** by setting the `SCENARIO` environment variable:

   ```bash
   export SCENARIO=substation
   ```

3. **Launch the DNS server**

   ```bash
   docker-compose up -d --build
   ```

4. **Test** example (Substation PLC):

   ```bash
   dig @127.0.0.1 plc1.substation.energy.local +short
   ```

## 📂 Directory Structure

```text
ot-dns-playground/
├── LICENSE
├── README.md
├── docker-compose.yml
├── templates/
│   ├── Dockerfile.template
│   ├── dnsmasq.conf.template
│   └── hosts.template
└── scenarios/
    ├── substation/
    │   └── config/
    │       ├── dnsmasq.conf
    │       └── hosts
    ├── production-plant/
    ├── hospital-devices/
    ├── building-automation/
    └── cctv/
```

## 🎯 Available Scenarios

* **substation**: Electrical substation network
* **production-plant**: Manufacturing OT environment
* **hospital-devices**: Medical devices network
* **building-automation**: Smart building control
* **cctv**: IP cameras and video recorders

To run any scenario:

```bash
export SCENARIO=<scenario-name>
docker-compose up -d --build
```

## ✨ Adding a New Scenario

1. Copy an existing folder under `scenarios/`:

   ```bash
   cp -r scenarios/substation scenarios/my-new-scenario
   ```

2. Edit `scenarios/my-new-scenario/config/dnsmasq.conf` and `scenarios/my-new-scenario/config/hosts` with your domain and IPs.

3. Start it:

   ```bash
   export SCENARIO=my-new-scenario
   docker-compose up -d --build
   ```

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

```
```

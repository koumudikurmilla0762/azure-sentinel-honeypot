# Azure Sentinel RDP Honeypot — Attack Telemetry & Threat Analytics

Hands-on lab (inspired by Josh Madakor’s video): I exposed a Windows VM to the internet to attract RDP brute-force attempts and analyzed the telemetry in **Microsoft Sentinel** using **KQL**. This repo contains queries, notes, and sample outputs.

## What I built
- **Windows Server VM** in Azure with a permissive **NSG** (“allow any”) to act as an RDP honeypot (TCP/3389).
- **Microsoft Sentinel** enabled on a **Log Analytics workspace**.
- **Azure Monitor Agent (AMA)** installed via a **Data Collection Rule**; connected the **Windows Security Events via AMA** connector.
- **KQL** to surface failed logons (**SecurityEvent 4625**), top attacking IPs, timelines, and host/user targets.
- Geo-enrichment of attacker IPs using KQL `iplocation()` (or a GeoIP CSV) and simple charts/tables exported to CSV/Excel.

## Repo structure
azure-sentinel-honeypot/
├─ README.md
├─ .gitignore
├─ kql/
│ └─ rdp_failed_logons.kql
├─ images/ # screenshots (redact IDs/secrets)
├─ exports/ # sample CSVs 

## KQL highlights
- Agent health with `Heartbeat`
- Failed RDP logons (EventID **4625**)
- Top attacking IPs
- Geo-enrichment with `iplocation()`

## How to reproduce (short)
1. Create a Windows VM + Log Analytics workspace; enable **Microsoft Sentinel** on the workspace.  
2. Install **AMA** via a **Data Collection Rule**; connect **Windows Security Events via AMA**.  
3. Run the queries in **Sentinel → Logs** (see `kql/rdp_failed_logons.kql`).  
4. Export results to CSV/Excel; optionally geo-enrich with `iplocation()` or a GeoIP CSV.  
5. Control costs: **Stop (deallocate)** the VM off-hours, set a **workspace daily cap**, and pause data collection when idle.

## Attack map
<img width="1482" height="707" alt="Attack_Image" src="https://github.com/user-attachments/assets/9258bf2a-d270-47a7-8fa3-745ea257b00e" />


## Findings (examples)
- Frequent failed RDP attempts from multiple countries (US, BG, JP, KR, SG).  
- Brute-force bursts during evening hours (UTC).  
- Top ASNs/providers observed in telemetry.

## License
MIT


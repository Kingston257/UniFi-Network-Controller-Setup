# UniFi Network Controller Setup — Remote Device Management

**Project type:** Network / Systems Administration  
**Owner:** Chisom Onyeka — ICT Coordinator / Systems & Networks  
**Environment:** Windows 10 host, UniFi Network application, UniFi AC LR access points  
**Status:** Implemented — remote management enabled, monitoring efficiency improved

---

## Overview
This project documents the setup and configuration of a UniFi Network Controller to re-adopt and centrally manage UniFi access points at a remote Fedoz client's off-site location. The goal was to restore centralized control, improve monitoring, and enable secure remote management for ongoing operations and troubleshooting.

_Screenshots:_ see `assets/dashboard.jpg` and `assets/topology.jpg`.

---

## Problem
An off-field site had UniFi devices that were no longer managed by a central controller. Issues included inconsistent configurations, limited visibility of device health, and difficulty troubleshooting without physical site visits.

---

## Objective
- Re-adopt the UniFi APs into a centrally managed controller.  
- Enable remote access so the on-call admin can monitor and manage devices off-site.  
- Configure SSIDs, basic VLAN/guest settings, and monitoring to reduce on-site support calls.

---

## Environment & Hardware
- **Controller host:** Windows 10 (local box)  
- **UniFi application:** UniFi Network (UniFi Controller application)  
- **Access Points:** 2 × UniFi AC LR  
- **Network core:** UniFi USG / UniFi switch (topology shown in screenshots)  
- **Remote access:** Controller configured for remote administration (tested via UniFi mobile app / remote management)

---

## Tools & Technologies
- UniFi Network application (controller)  
- UniFi mobile app (remote testing)  
- SSH (for manual adopt/set-inform when required)  
- Basic tools: web browser (controller UI), file export for backups

---

## Implementation — Steps Taken
> **Note:** adapt commands and IPs to your environment. Replace `<CONTROLLER_IP>` and `<AP_IP>` with actual values.

1. **Controller install & initial config**
   - Installed the UniFi Network application on a Windows 10 host and completed the first-time setup (site name, admin account).
   - Secured the admin account with a strong password and enabled two-factor auth on the UniFi account where applicable.

2. **Device discovery & re-adoption**
   - Ensured the controller and APs were on the same Layer 2 network (or reachable via SSH for set-inform).
   - From the UniFi UI, located unmanaged devices and used the **Adopt** action to add the APs to the site.
   - When GUI adopt failed or APs were remote, performed SSH adopt:
     ```bash
     ssh ubnt@<AP_IP>
     # then on the AP shell
     set-inform http://<CONTROLLER_IP>:8080/inform
     ```
     (Repeat `set-inform` twice if necessary.)

3. **Network configuration**
   - Created SSIDs (production & guest), applied basic security settings and WPA2/WPA3 where supported.
   - Configured VLANs and user-group rules as required by the site policy.
   - Adjusted radio/channel width and power to balance coverage and interference.

4. **Remote management**
   - Enabled remote management so the controller is accessible offsite (verified via UniFi mobile app).
   - Verified authentication and remote adoption workflows worked from outside the network.

5. **Monitoring & verification**
   - Used the UniFi dashboard and topology view to verify device health, client connections, and TX/RX statistics.
   - Took topology and dashboard screenshots for documentation (see `/assets`).

6. **Backup & documentation**
   - Exported site configuration and backups from the controller UI and saved in `configuration-files/`.
   - Documented the adoption steps, credentials policy, and alerting recommendations.

---

## Results & Impact
- **Centralized visibility** of all UniFi devices at the off-field site.  
- **Remote management** capability verified (controller accessible offsite via mobile app / remote admin).  
- **Improved monitoring efficiency**, reducing the need for on-site visits and enabling faster incident response.  
- Ongoing work: implement automated alerts and reporting to proactively detect and resolve issues.

---

## Lessons Learned & Next Steps
- Keep controller and device firmware up to date to avoid adoption/version conflicts.  
- Consider automating monitoring using the UniFi API and a small Python script to push alerts to email or Slack.  
- Plan to harden remote access (VPN or UniFi cloud access) and document a clear recovery procedure for controller host failures.

---

## Files & Assets
- `assets/dashboard.png` — Controller dashboard screenshot  
- `assets/topology.png` — Network topology screenshot  
- `configuration-files/network-config-backup.json` — (to be reuploaded) exported controller/site backup

---

## How to reproduce (high level)
1. Install UniFi Network application on a host or use UniFi Cloud Key/UniFi OS.  
2. Configure host network, ensure reachability to APs or open SSH for set-inform.  
3. Adopt devices via UI or SSH `set-inform`.  
4. Configure SSIDs, VLANs and policies.  
5. Enable remote access and test adoption/monitoring from offsite.

---

## Author
**Chisom Onyeka** — Acting ICT Coordinator | Data & Systems  
LinkedIn: [https://linkedin.com/in/chisom-onyeka] • 

---


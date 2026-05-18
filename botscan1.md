# Cybersecurity Incident Report

* [cite_start]**Date of Incident:** 5/17/2026 [cite: 1]
* [cite_start]**Date of Report:** 5/18/2026 [cite: 1]
* [cite_start]**Date of Resolution:** [cite: 1]
* [cite_start]**Affected Systems:** STAR Micronics Thermal Receipt Printer [cite: 1]
* [cite_start]**Business Impact:** Low (No data breach. Temporary disruption of service, small waste of physical assets (printer paper)) [cite: 1]

---

## Summary

[cite_start]On 5/17/2026, a STAR Thermal Printer at **[Business Redacted]** printed unauthorized pages containing automated internet scan logs, raw command syntax, and a *"I hacked you!"* message[cite: 1, 2]. [cite_start]As of current report, no data has been compromised as the business operates through an isolated configuration and uses SQUARE POS[cite: 2].

---

## Most Likely Occurrence

[cite_start]Preliminary findings after research and talking with the owner suggest that the printer is using factory default settings (UN: `root` / PW: `public`)[cite: 3]. [cite_start]The printer was most likely exposed to the web via an open port on the router, allowing either automated bots or a script kiddie to send raw text data to the printer's hardware[cite: 4].

---

## Technical Details and Evidence

### Most Likely Root Cause
[cite_start]The source of the exposure is the router or location of the printer[cite: 5]. 
* [cite_start]The router may have port forwarding enabled for **TCP Port 9100 (RAW Printing)**[cite: 6].
* [cite_start]Otherwise, the printer was placed in the **DMZ** and was exposed to the internet[cite: 7].

[cite_start]**IP/Host Involved:** `[Redacted]` [cite: 8]

### Artifacts
[cite_start]Automated Web Scans were observed as the printer processed standard HTTP GET requests (`GET /remote/login`, `GET /api/v2/about`) originating from automated global scanners[cite: 9]. 

[cite_start]The commands were sent via **Printer Job Language (PJL)** as shown by queries[cite: 9]:
* [cite_start]`@PJL INFO STATUS` *(essentially "Do you have paper, are you on?")* [cite: 9]
* [cite_start]`@PJL RDYMSG DISPLAY = rdymsgarg` *(This tells the screen to print rdymsgarg)* [cite: 9]

[cite_start]The hack was perpetrated by **t.me/CAMERAPW**, a Telegram account that attacks IoT devices. [cite_start]This attacker printed `t.me/CAMERAPW HACKED YOU!`.

[cite_start]*Images of printed documents can be found at the end of the report.* [cite: 11]

---

## Suggested Containment and Root Cause

[cite_start]The root cause is most likely a network misconfiguration rather than a targeted attack[cite: 12]. [cite_start]This is probably the result of a bot running automated scripts on the internet[cite: 13]. [cite_start]The RAW printing protocol requires no authentication, so the printer treated this hack as a valid job[cite: 14].

---

## Containment and Resolution

### Step 1: Identify Printer IP
[cite_start]Identify the internal IP address by powering off the device, holding the **FEED** button, and powering it back on[cite: 15]. [cite_start]This should print a configuration including the IP address[cite: 15].

### Step 2: Access Router Admin Portal
1. [cite_start]Open a web browser and input the **Gateway IP address**[cite: 16]. 
2. [cite_start]Input the admin credentials[cite: 17]. [cite_start]If not found on the router, the default may be searchable online[cite: 17].

### Step 3: Remove Port Forwarding Rule
1. [cite_start]Once inside the admin portal, navigate to the security or advanced network section[cite: 18].
2. [cite_start]Find a tab named **Port Forwarding**, **Port Triggering**, **Virtual Server**, or **Pinholes**[cite: 18].
3. [cite_start]Scan the routing table for any rules mentioning **Port 9100** or **RAW/JetDirect**[cite: 19]. [cite_start]The rule will look like it is directing traffic from the internet to the Printer's private LAN IP address[cite: 20].
4. [cite_start]Select the rule and click **Delete**, **Remove**, or **Disable**[cite: 21].
5. [cite_start]Apply Changes and Save[cite: 22].

### Step 4: Disable UPnP
1. [cite_start]Inside the router's admin panel, locate the **LAN Settings**, **Advanced Network**, or **App and Gaming** tab[cite: 23].
2. [cite_start]Find a setting called **UPnP** or **Universal Plug and Play**[cite: 24].
3. [cite_start]Change the setting to **Disabled** or **Off**[cite: 24].
4. [cite_start]Apply and Save[cite: 24].

### Step 5: Verification
[cite_start]To verify the fix, use a diagnostic app such as **FING** on a smartphone to run a port scan and see if 9100 pops up[cite: 25].

---

## Artifact Descriptions

### [cite_start]Figure 1: Printed receipt showing the PJL commands [cite: 26]
### [cite_start]Figure 2: Bot Signature [cite: 26]

* [cite_start]**Software Probing:** The bot is checking to see if the IP address runs outdated and flawed chat software[cite: 26]. [cite_start]It scans for the brand of software or hardware running on the device, looking for language files such as Printer Job Language[cite: 27, 28].
* [cite_start]**Brute Force Scans:** The bot looks for a remote admin login portal to attempt a brute force attack[cite: 28].
* [cite_start]**HVAC / Automation Targeting:** The bot is looking for *Johnson Controls Metasys*, a building automation system used by HVAC to control heating, cooling, and ventilation[cite: 29]. [cite_start]Basically, it is looking to see if you have an unsecured smart HVAC system it can exploit[cite: 30].
* [cite_start]**Router Vulnerabilities:** The scanner is trying to find a family of routers that have a known exploit[cite: 31].
* [cite_start]**Identity & Credential Harvesting:** The bot is looking for identity management software (access control) [cite: 32][cite_start], as well as backup/restore modules found on some IoT devices and routers[cite: 33]. [cite_start]If found, it would download the system's backup files, which could possibly include network login credentials[cite: 34].
* [cite_start]**Binary Interpretation:** The bot tries to use binary code injection, but the printer didn't recognize it, so it spat out the symbolic equivalent of the binary[cite: 35]. [cite_start]Then it switched to PJL and hit paydirt, where the printer responded with a system response to `@PJL INFO STATUS`[cite: 35].
* [cite_start]**Corporate Gateway Cycling:** The rest of this is the bot cycling through common corporate gateways[cite: 36]. [cite_start]These include VPN systems for government employees, Enterprise level File Transfer, and large network-attached storage units to see if it can get admin creds straight from the device[cite: 37].
* [cite_start]**User Agent Shift:** Here, the browser shifts from Mozilla-based to *YaBrowser*, a popular Russian/Eastern European browser[cite: 38].
* [cite_start]**Conclusion:** The open Port 9100 allowed for a text injection[cite: 39]. [cite_start]Essentially, it tried every key it had and eventually just yelled really loud before storming away[cite: 39].
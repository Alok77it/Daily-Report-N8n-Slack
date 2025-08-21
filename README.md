# DailyReport n8n Workflow

This repository contains the **DailyReport** workflow for [n8n](https://n8n.io/), designed to automate the daily health check and reporting of a server. The workflow executes a server-side Python script, parses its output, and sends a formatted daily status message to a Slack channel.

---

## 🛠️ What the Workflow Does

1. **Schedule Trigger**  
   Runs automatically every day at 9:00 AM server time.

2. **SSH Command Execution**  
   Connects via SSH to a target server and runs the command:
   ```
   python3 /report.py
   ```
   The script should output a JSON-formatted report (with possible plain text backup info at the start).

3. **Code Node - Data Parsing & Formatting**  
   - Parses the SSH command output.
   - Extracts system, backup, service, cron job, network, and uptime data.
   - Builds a readable and emoji-enhanced Slack report.

4. **Slack Message**  
   Sends the formatted report to the specified Slack channel (e.g., `#ys-alerts`).

---

## 📑 File Structure

- `DailyReport.json` (your exported n8n workflow)
- `README.md` (this file)

---

## 📝 Requirements

- [n8n](https://n8n.io/) instance
- Access credentials for:
  - SSH to the server where `/report.py` runs
  - Slack bot with permission to post to the target channel

- The server must have a Python script `/report.py` that outputs a JSON with at least these keys:
  - `system` (with `cpu_percent`, `ram_used_mb`, `ram_total_mb`, `swap_used_mb`, `swap_total_mb`)
  - `services` (`{serviceName: true/false}`)
  - `cron_jobs` (`{jobName: true/false}`)
  - `backup_exists` (`true`/`false`)
  - `network` (`bytes_sent_mb`, `sent_mbps`, `bytes_recv_mb`, `recv_mbps`)
  - `uptime` (string)

---

## ⚡ Setup Steps

1. **Import Workflow into n8n**
   - From your n8n dashboard, click "Import" and select the `DailyReport.json` file.

2. **Configure Credentials**
   - Set up SSH credentials for the server.
   - Set up your Slack API credentials.

3. **Edit the Workflow (if needed)**
   - Change the schedule hour in the Schedule Trigger node.
   - Adjust the SSH command or Slack channel as needed.

4. **Activate the Workflow**
   - Toggle the workflow to `Active` in n8n.

---

## 💬 Example Slack Output

```
📊 Daily Server Report

✅ Backup exists for today.
*System Stats:*
- CPU Usage: 5%
- RAM Usage: 1024 MB / 4096 MB
- Swap Usage: 0 MB / 1024 MB
*Services:*
- ✅ nginx running
- ✅ redis running
- ⚠️ postgres down
*Cron Jobs:*
- ✅ backup succeeded
- ⚠️ cleanup failed
*Network Stats:*
- Sent: 12 MB (1.2 Mbps)
- Received: 24 MB (2.4 Mbps)
*Uptime:* 3 days, 4:12:08
```

---

## 🔒 Security

- **Credentials:** Never commit sensitive credentials to version control.
- **SSH:** The workflow assumes secure network and credential management in n8n.

---

## 📬 Support

For questions or issues, please open an issue in this repository or contact your DevOps team.

---

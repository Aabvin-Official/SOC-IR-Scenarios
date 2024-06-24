# Create a GitHub markdown file with the content
content = """
# Detailed Implementation Process for Triaging SOC Alerts

## 1. Initial Alert Generation

### QRadar: Configure QRadar to detect anomalous behavior
- 🔐 **Step 1**: Log in to QRadar and navigate to the "Offenses" tab.
- 🛠️ **Step 2**: Go to "Rules" and click "Add Rule" to create a new detection rule.
- 📝 **Step 3**: Define the rule to detect unexpected connections to external IPs:
  - 📍 Source IP within internal network range.
  - 🌐 Destination IP outside the internal network.
  - 🚨 Known malicious IPs (using threat intelligence feeds).
- 📊 **Step 4**: Set thresholds for the number of connections that will trigger the alert.

### QRadar: Set up rules to flag excessive failed login attempts followed by a successful one
- 🛠️ **Step 1**: Create a new rule in QRadar by going to "Rules" and selecting "Add Rule."
- 📝 **Step 2**: Define conditions:
  - 🔑 Event name contains "failed login."
  - ⏱️ Track the number of failed login attempts within a specified time frame (e.g., 5 attempts in 10 minutes).
  - ✅ Followed by a successful login attempt from the same source IP.
- 🚨 **Step 3**: Configure the rule action to generate an offense if the conditions are met.

- 💾 Step 3: Save this search as an alert by clicking "Save As" and selecting "Alert."
⚙️ Step 4: Configure alert conditions (e.g., trigger when the search returns results) and actions (e.g., send email, create incident).
Splunk: Use Splunk's correlation search to identify phishing patterns
🛠️ Step 1: Create a correlation search that identifies multiple users receiving the same suspicious email.
index=email_logs sourcetype=email | stats count by subject, sender | where count > 1


### Splunk: Set up alerts to monitor email gateway logs
- 📧 **Step 1**: Access Splunk and go to the "Search & Reporting" app.
- 🔍 **Step 2**: Create a search query to identify emails with attachments or links:
  ```spl
  index=email_logs sourcetype=email | search attachment=* OR link=*

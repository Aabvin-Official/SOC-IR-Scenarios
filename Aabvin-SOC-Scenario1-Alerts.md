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
- ⚙️ Step 4: Configure alert conditions (e.g., trigger when the search returns results) and actions (e.g., send email, create incident).

**Splunk: Use Splunk's correlation search to identify phishing patterns**
- 🛠️ Step 1: Create a correlation search that identifies multiple users receiving the same suspicious email.  
   ```index=email_logs sourcetype=email | stats count by subject, sender | where count > 1```
- 💾 Step 2: Save this search as an alert and set the alert conditions and actions.

2. Alert Prioritization
QRadar Risk Manager: Utilize risk scoring to prioritize alerts
- 🛠️ Step 1: Access QRadar Risk Manager and navigate to the risk scoring configuration.
- 📊 Step 2: Customize risk scoring by integrating asset value data, threat intelligence feeds, and historical incident data.
- ⚖️ Step 3: Adjust the risk score calculation based on the severity and potential impact of the detected events.
- 📈 Step 4: Use risk scores to prioritize alerts in the QRadar dashboard.

  Splunk: Use Splunk's adaptive response framework
- ⚙️ Step 1: Enable adaptive response actions in Splunk Enterprise Security.
- 🛠️ Step 2: Configure adaptive response actions for specific alerts to dynamically adjust their priority based on context.
- 📈 Step 3: Set up workflows to automatically escalate high-priority alerts and suppress low-priority ones.

3. Contextual Analysis
Splunk: Analyze logs from various sources
- 🖥️ Step 1: In Splunk, create a dashboard that aggregates logs from network, application, and endpoint sources.
- 🔍 Step 2: Use search queries to correlate these logs and identify the context of the alert:  
   ``` index=network_logs OR index=app_logs OR index=endpoint_logs | stats count by source, dest, action ```
- 📊 Step 3: Visualize user behavior using Splunk’s data models to detect anomalies.

Wireshark: Capture and analyze network traffic
- 🛠️ Step 1: Open Wireshark and start a network capture on the relevant interface.
- 🔍 Step 2: Apply filters to focus on suspicious traffic (e.g., DNS queries, large data transfers):
   ``` dns or tcp.len > 1000 ```
- 📈 Step 3: Analyze the captured packets to identify any unusual patterns or indicators of compromise.

4. Historical Data Comparison
QRadar: Query historical data
- 🔍 Step 1: In QRadar, use the "Log Activity" tab to search historical logs.
- 📝 Step 2: Create a query to find similar alerts from the past:  
   ``` (eventName="failed login" AND eventName="successful login") AND sourceIP=<source IP> ```
- 📊 Step 3: Analyze the outcomes of these historical alerts to determine if they were genuine threats or false positives.

Splunk: Utilize Splunk's machine learning toolkit
🛠️ Step 1: Use the machine learning toolkit to build models that predict normal behavior.
📈 Step 2: Create a model using historical data and apply it to current data to detect deviations:  
   ``` fit DensityFunction normal_behavior from historical_data into model_behavior ```
  ```   apply model_behavior ```

5. Threat Intelligence Application
QRadar and Splunk: Integrate threat intelligence feeds
🌐 Step 1: Subscribe to threat intelligence feeds and integrate them into QRadar and Splunk.
⚙️ Step 2: Configure QRadar to enrich alerts with threat intelligence data (e.g., IP reputation, malware hashes).
🔍 Step 3: In Splunk, use threat intelligence lookups to validate indicators within alerts:  
   ``` inputlookup threat_intel.csv | search [| inputlookup current_alerts.csv] ```

Threat Intelligence Platforms: Use platforms like ThreatConnect or MISP  
🌐 Step 1: Access ThreatConnect or MISP and gather additional threat intelligence relevant to the alerts.  
🔍 Step 2: Correlate this intelligence with current alerts in QRadar and Splunk to validate and enrich the context of the alerts.


 Assessment Questions
Questions:
🛠️ What steps are involved in configuring QRadar to detect anomalous behavior?  
⚙️ How do you set up rules in QRadar to flag excessive failed login attempts followed by a successful one?  
📧 Describe the process of setting up alerts to monitor email gateway logs in Splunk.  
⚖️ How can QRadar Risk Manager be used to prioritize alerts?  
📈 Explain the use of Splunk’s adaptive response framework in alert prioritization.  
🖥️ What are the steps for analyzing logs from various sources in Splunk?  
🔍 How can Wireshark be used to capture and analyze network traffic?  
🔍 Describe the process of querying historical data in QRadar.  
🛠️ How does Splunk’s machine learning toolkit help in detecting deviations from normal behavior?  
🌐 Explain how threat intelligence feeds are integrated into QRadar and Splunk.  

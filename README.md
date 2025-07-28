# Integrated Security System with ELK, Wazuh, Suricata & Discord Alerts
A thesis project on the development of an integrated security system using the ELK Stack, Wazuh, and Suricata. This system is designed to improve the effectiveness and efficiency of incident handling by automatically detecting cyberattacks and sending real-time alert notifications to Discord.

---

An open-source project designed to build a comprehensive and affordable security monitoring solution. This system integrates leading open-source tools to detect, analyze, and respond to cyber threats in real-time within a virtualized environment.

## ✨ Key Features

* **Integrated Security Analytics:** Centralizes log collection and analysis from various sources using the ELK Stack.
* **Host-Based Intrusion Detection (HIDS):** Utilizes **Wazuh** for monitoring endpoint activities, file integrity, and configuration assessment.
* **Network Intrusion Detection (NIDS):** Employs **Suricata** to monitor network traffic for malicious patterns and threats like DDoS or scanning.
* **Real-Time Alerting:** Automates incident notifications to a **Discord channel** using custom webhooks, enabling rapid response from the security team.
* **Comprehensive Visualization:** Uses **Kibana** to create intuitive dashboards for analyzing security events and identifying threat patterns.

## 🛠️ Tech Stack

* **SIEM & Log Management:** ELK Stack (Elasticsearch, Logstash, Kibana) v7.17.12 
* **HIDS/Security Platform:** Wazuh v4.9
* **NIDS/IPS Engine:** Suricata
* **Notification:** Discord Webhooks
* **Virtualization:** VirtualBox 7.1.4
* **Operating Systems:** Ubuntu Server 24.04, Kali Linux 2024.2

## 🚀 Getting Started

### 1. System Architecture

The system operates by having Suricata and the Wazuh Agent monitor network traffic and endpoint logs. These logs are forwarded to the Wazuh Server for analysis. If a threat is detected, Wazuh triggers two actions simultaneously:
1.  The log is sent to the ELK Stack for storage and visualization.
2.  An integration module runs a custom Python script to send a formatted alert to a specified Discord webhook.

### 2. Installation & Configuration

1.  **Install ELK Stack and Wazuh Server**
2.  **Install Suricata:** Set up Suricata on the Wazuh server and configure it to monitor the desired network interface.
3.  **Deploy Wazuh Agent:** Install the Wazuh agent on the target servers that need to be monitored.
4.  **Configure Discord Integration:**
    * Create a webhook in your Discord server.
    * Add the integration block to `/var/ossec/etc/ossec.conf` on the Wazuh server. See `config-examples/ossec.conf.example` for a template.
      ```conf
      <ossec_config>
        <global>
          <jsonout_output>yes</jsonout_output>
          <alerts_log>yes</alerts_log>
          <logall>no</logall>
          <logall_json>no</logall_json>
          <email_notification>yes</email_notification>
          <smtp_server>localhost</smtp_server>
          <email_from>{origin email}</email_from>
          <email_to>{destination email}</email_to>
          <email_maxperhour>12</email_maxperhour>
          <email_log_source>alerts.log</email_log_source>
          <agents_disconnection_time>10m</agents_disconnection_time>
          <agents_disconnection_alert_time>0</agents_disconnection_alert_time>
         </global>
         <alerts>
          <log_alert_level>3</log_alert_level>
          <email_alert_level>8</email_alert_level>
         </alerts>

      <!--Custom discord Integration --> 
      <integration>
          <name>custom-discord</name>
          <hook_url>{insert your discord webhook here}</hook_url>
          <level>3</level>
          <group>ids,suricata,web,accesslog,sshd,bfdvwa</group>
          <alert_format>json</alert_format>
        </integration>
      ```
      > [!IMPORTANT]
      > Be sure to replace the placeholder values in the configuration above:
      > * `<email_from>`: The email address alerts will be sent from.
      > * `<email_to>`: The email address that will receive the backup alerts.
      > * `<hook_url>`: Paste the webhook URL you created in Discord.
    * Place the `custom-discord.py` and the corresponding bash script into `/var/ossec/integrations/`. The Python script is available in the `integrations/` folder of this repository.

## ✍️ Authors

* **Matthew Kurniawan** - *Bina Nusantara University*
* **Benedicto Marvelous Alidajaya** - *Bina Nusantara University*

## 🙏 Acknowledgements

* Dr. Aditya Kurniawan, S.Kom., MMSI., CND, CEHmaster - Thesis Advisor.
* The entire faculty at the School of Computer Science, Bina Nusantara University.
* The custom Discord integration script was heavily inspired by the excellent guide from **Eugenio Chaves**. His blog post was invaluable in developing the webhook functionality. [Creating a custom Wazuh integration](https://eugenio-chaves.github.io/blog/2022/creating-a-custom-wazuh-integration).
    

## 📚 How to Cite

If you use this project for your research, please cite the original thesis:

Kurniawan, M., & Alidajaya, B. M. (2025). *Development of an Integrated Security System with ELK Stack, Wazuh, Suricata, and Response Automation and Alert Notifications to Discord*. Undergraduate Thesis, Bina Nusantara University, Jakarta.

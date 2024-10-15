# 30-Day-On-Prem

## Objective

Implement a comprehensive Security Information and Event Management (SIEM) system on-premises. The project will focus on, deploying and configuring an ELK stack (Elasticsearch, Logstash, Kibana) to collect, process, and visualize security-relevant data, setting up a Fleet Server to manage and monitor Elastic Agents deployed on various systems and keep it all centralized, provisioning Windows and Linux server environments for testing and demonstration purposes, integrating a Mythic C2 server to simulate cyberattacks and assess the SIEM's detection capabilities, ans deploying an OSTicket server for incident management and ticketing. The overall goal is to establish a robust SIEM solution capable of detecting, analyzing, and responding to security threats within the organization.

### Skills Learned

ELK Stack: Elasticsearch, Logstash, and Kibana for data ingestion, processing, and visualization.
Security Information and Event Management (SIEM): Understanding of SIEM concepts, best practices, and use cases.
Network and System Administration: Knowledge of system configurations and troubleshooting techniques.
Security Tools and Technologies: Familiarity with tools like Kali, osTicket, and other security assessment tools.
Problem-solving: Ability to identify and troubleshoot issues related to SIEM deployments and data analysis.
Analytical Thinking: Skill in analyzing large datasets to identify security threats and patterns.
Attention to Detail: Meticulous approach to ensure accurate data collection and analysis.
Continuous Learning: A mindset of staying updated with the latest security threats and technologies.

### Tools Used

Elasticsearch: For storing and analyzing large volumes of data.
Logstash: Collects, processes, and transforms data from various sources.
Kibana: A powerful visualization tool for exploring and analyzing data stored in Elasticsearch.
Beats: Lightweight data shippers (e.g., Filebeat, Auditbeat, Metricbeat) to collect data from various sources.
Threat Intelligence Platforms: Tools to gather and analyze threat intelligence data.
Configuration Management Tools: To automate the deployment and configuration of the SIEM components.


## Steps

1. In Virtual box get your 6 machines ready. 4 ubuntu Libux m 2 Windows servers.<br>
![VMs](https://github.com/user-attachments/assets/a24c743d-907d-4d7e-b02f-e08b3b694833)<br>
*Ref 1: VMs*<br>
2. Set up your NAT network, enter your prefered IPv4 prefic and group your VMs into your newly created NAT network.<br>
![NAT Network](https://github.com/user-attachments/assets/735129b7-a24b-4b65-b076-2e3d48932b45)<br>
*Ref 2: NAT Network*<br>
3. Spin up your machines and update them.
4. Configure SSH port forwarding in your VM software, create one for each VM.<br>
![port-forwarding](https://github.com/user-attachments/assets/ee3278a5-30eb-4ba2-9df2-6be36221ef7f)<br>
*Ref 3: Port Forwarding*<br>
5. Install Elastic and Kibana on your ELK server and Sysmon on your Windows Server. Sudo nano /etc/elasticsearch/elasticsearh.yml > uncomment "network.host" and change it to the VMs ip and uncomment port. Start up elastic.<br>
![start elasticsearch](https://github.com/user-attachments/assets/7c4277af-cca8-466c-a77e-76c862fe3956)<br>
*Ref 4: Start elastic*<br>
6. Install Kibana, update config file > uncomment "server.port" and "server.host" and change it to ip address, same with "server.publicBaseUrl" with <ip address>:5601.<br>
![kibana config](https://github.com/user-attachments/assets/fc12b7e2-62a3-41fa-93bb-f59a48597e37)<br>
*Ref 5: Kibana config*<br>
7. Create encryption keys > cd /usr/share/kibana/bin > sudo ./kibana-encryption-keys generate. Next sudo ./kibana-keystore add <encryption key filename> > encryption key. Start up Kibana.<br>
![kibana generate encrypt keys](https://github.com/user-attachments/assets/0c2e2889-fd62-4d47-bf0a-9859d13167db)<br>
*Ref 6: Generate enctyption keys*<br>
![kibana encryption keys](https://github.com/user-attachments/assets/c8bc5e16-0f93-482e-b8b4-9a5245ef4fba)<br>
*Ref 7: Encryption keys*<br>
![start kibana](https://github.com/user-attachments/assets/81dff932-ce13-4ef2-847c-dc21945a767d)<br>
*Ref 8: Start Kibana*
8. Create a port forward rule on virtualbox(or VMware) to ELK. Create enrollment token > cd/usr/share/elasticsearch/bin. For verification code: cd /usr/share/kibana/bin > sudo ./kibana-verification-code. To login use elastic as username and password from security auto-configuration information("elastic built-in superuser passwrod").<br>
![ELK port forward](https://github.com/user-attachments/assets/5894648f-87b3-4a15-8078-48770b7941ca)<br>
*Ref 9: ELK port forward*<br>
![kibana enrollment key](https://github.com/user-attachments/assets/c175773b-674c-4425-8287-c14e76ed298c)<br>
*Ref 10: Enrollment key*<br>
![kibana verification code](https://github.com/user-attachments/assets/57e5270d-d35d-4cb2-831d-b7988c725d4e)<br>
*Ref 11: Verification code*<br>
9. Setup Fleet Server, Hamburger icon > fleet > add fleet server. Specify name, URL is the ip address of the fleet VM > generate server policy > fleet settings to change port from 443 to 8220 in Edit Fleet Server. Click agents > add fleet server > continue; copy Linux Tar and paste into fleet VM. Continue enrolling elastic agent > enter a policy name > create policy > click Windows tab and copy agent install text > on Windows server VM paste into an admin PowerShell instance. use "--insecure" flag.<br>
![elastic screen](https://github.com/user-attachments/assets/dce33c5a-0715-41f5-bd91-44490badbd7e)<br>
*Ref 12: elastic screen*<br>
![PS insecure agent](https://github.com/user-attachments/assets/233783f1-cc9b-497e-978d-979c0712b4cc)<br>
*Ref 13: insecure flag*<br>
![windows add agent](https://github.com/user-attachments/assets/42dd1a73-a36b-45c9-a653-184ef7baf22f)<br>
*Ref 14: Windows add agent*<br>
![Add Agent success](https://github.com/user-attachments/assets/f65fdb07-289a-42c2-a5be-026e9716dd3c)<br>
*Ref 15: Add Windows agent success*<br>
10. Add and Agent for Linux(Ubuntu) Server > create new agent policy > name it > enroll in fleet > create policy > copy Linux Tar and paste into Linux Server.<br>
![elastic screen](https://github.com/user-attachments/assets/9d84d1dd-1e1d-4594-be5d-4d0fa940a537)<br>
*Ref 16: Elastic screen*<br>
![Fleet](https://github.com/user-attachments/assets/fdbd22f3-b0a0-4bbf-b236-4957421d3883)<br>
*Ref 17: Fleet*<br>
11. Click the Haburger Icon > Integrations > Search "custom windows event logs" > add custom winds event logs > name it, for channel name go to windows server and open event viewer to find sysmon then operational: right-click, properties, "full name" is the channel name > exisiting host under "where to add", ...; win-policy > save and continue.<br>
![Custom windows event logs](https://github.com/user-attachments/assets/03598f24-eb91-4029-8187-3dbb992208cf)<br>
*Ref 18: Custom Windows event logs*<br>
12. On your Windows Server, Event viewer find Windows Defender > operational, properties, copy "full name" and add another custom Windows event logs like above. Enable RDP on Windows Server under remote desktop settings<br>
13. Install docker-compose and make on your mythic vm. Install <a href="https://docs.mythic-c2.net/installation">Mythic</a> then run sudo make. > sudo ./mythic-cli start > cat env, and create another port forwarding rule for Mythic, then install a Mythic agent and C2 profile.<br>
14. 

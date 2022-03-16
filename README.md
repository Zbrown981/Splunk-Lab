# Splunk-Lab
## Vandalay Industries Monitoring Activity Instructions 

Step 1: The Need for Speed 

Background: As the worldwide leader of importing and exporting, Vandalay Industries has been the target of many adversaries attempting to disrupt their online business. Recently, Vandaly has been experiencing DDOS attacks against their web servers. 

  

Not only were web servers taken offline by a DDOS attack, but upload and download speed were also significantly impacted after the outage. Your networking team provided results of a network speed run around the time of the latest DDOS attack. 

  

Task: Create a report to determine the impact that the DDOS attack had on download and upload speed. Additionally, create an additional field to calculate the ratio of the upload speed to the download speed. 

  

Upload the following file of the system speeds around the time of the attack. 

  

Speed Test File 

Using the eval command, create a field called ratio that shows the ratio between the upload and download speeds. 

source="speedtestfile.csv" host="speedtestfileA" sourcetype="csv" | eval ratio='UPLOAD_MEGABITS'/'DOWNLOAD_MEGABITS' 
![splunk1](https://user-images.githubusercontent.com/89311706/158698181-8ae3a7e5-946b-4025-97ba-45dd67ff4a41.png)
 

  

Hint: The format for creating a ratio is: | eval new_field_name = 'fieldA' / 'fieldB' 

Create a report using the Splunk's table command to display the following fields in a statistics report: 

  

- _time 

- IP_ADDRESS 

- DOWNLOAD_MEGABITS 

- UPLOAD_MEGABITS 

- ratio 


source="speedtestfile.csv" host="speedtestfileA" sourcetype="csv" | eval ratio='UPLOAD_MEGABITS'/'DOWNLOAD_MEGABITS' | table _time IP_ADDRESS DOWNLOAD_MEGABITS UPLOAD_MEGABITS ratio 

Based on the report created, what is the approximate date and time of the attack? 

February 23rd 2020, at 2:30 pm. 

How long did it take your systems to recover? 

About 9 hours. 

Submit a screen shot of your report and the answer to the questions above. 
![splunk2](https://user-images.githubusercontent.com/89311706/158698467-4044b4e2-2cf2-4ad1-953b-3c0727bf4ee8.png)
 

  

Step 2: Are We Vulnerable? 

Background: Due to the frequency of attacks, your manager needs to be sure that sensitive customer data on their servers is not vulnerable. Since Vandalay uses Nessus vulnerability scanners, you have pulled the last 24 hours of scans to see if there are any critical vulnerabilities. 

  

For more information on Nessus, read the following link: https://www.tenable.com/products/nessus 

Task: Create a report determining how many critical vulnerabilities exist on the customer data server. Then, build an alert to notify your team if a critical vulnerability reappears on this server. 

  

Upload the following file from the Nessus vulnerability scan. 

  

Nessus Scan Results 

Create a report that shows the count of critical vulnerabilities from the customer database server. 

 The database server IP is 10.11.36.23. 

The field that identifies the level of vulnerabilities is severity. 

Build an alert that monitors every day to see if this server has any critical vulnerabilities. If a vulnerability exists, have an alert emailed to soc@vandalay.com. 

  

Submit a screenshot of your report and a screenshot of proof that the alert has been created. 

Report:  source="nessus_logs.csv" host="nessuslogs" sourcetype="csv" dest_ip="10.11.36.23" severity=critical | stats count by severity  
![splunk3](https://user-images.githubusercontent.com/89311706/158698601-fc94deab-d882-4c30-b56a-b67110390f27.png)
Alert:    source="nessus_logs.csv" host="nessuslogs" sourcetype="csv" dest_ip="10.11.36.23" severity=critical 

 

 

  

Step 3: Drawing the (base)line 

Background: A Vandaly server is also experiencing brute force attacks into their administrator account. Management would like you to set up monitoring to notify the SOC team if a brute force attack occurs again. 

  

Task: Analyze administrator logs that document a brute force attack. Then, create a baseline of the ordinary amount of administrator bad logins and determine a threshold to indicate if a brute force attack is occurring. 

  

Upload the administrator login logs. 

  

Admin Logins 

When did the brute force attack occur? 

 The brute force attack started at 8 am on February 21st 2020 and ended at 2pm on the same day. 

 

Hints: 

Look for the name field to find failed logins. 

Note the attack lasted several hours. 

Determine a baseline of normal activity and a threshold that would alert if a brute force attack is occurring.  After examining the data, 20+ failed log-ons only happened a couple times outside of the attack. I think a good baseline for normal activity is 10 and 25 failed attempts would be my threshold. I set it at 25 because 23 failed attempts occurred at 4 am before the attack and I’d like to limit false positives. 

  

Design an alert to check the threshold every hour and email the SOC team at SOC@vandalay.com if triggered. 

Alert: source="Administrator_logs.csv" host="AdminLogs" sourcetype="csv" name="An account failed to log on" | stats count by name 

 

 

  

Submit the answers to the questions about the brute force timing, baseline and threshold. Additionally, provide a screenshot as proof that the alert has been created. 

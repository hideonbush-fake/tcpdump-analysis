# tcpdump-analysis

## Objective

link: https://www.malware-traffic-analysis.net/index.html

I will be analyzing and parsing PCAPs using only tcpdump. yes... I know that using both tcpdump and Wireshark is the most efficient; however, I'll be creating another project that incorporates tcpdump, Wireshark, and Snort IDS/IPS together :))))))))))

### Skills Learned

- Parse through network packets using tcpdump to detect anomalies and security threats
- Use packet analysis to detect reconnaissance scans, suspicious traffic patterns, and protocol abuse
- Capture malicious traffic indicators such as unauthorized logins and exfiltration attempts
- Identify protocol anomalies, suspicious DNS queries, and HTTP/S traffic patterns linked to cyber threats
- Use tcpdump filters focus on specific traffic types

## Gathering Information

To optimize analysis efficiency, I will filter high-volume network traffic by identifying the most frequently used IP addresses and ports.

![Screenshot 2025-02-19 191724](https://github.com/user-attachments/assets/bbd71f31-8289-4e78-853e-e2a3e82d29ca)

![Screenshot 2025-02-19 191839](https://github.com/user-attachments/assets/208427fe-dfb7-4fea-811a-cfd001cb9d29)

Just change the (.) delimiter field to 5 to get the source port

![Screenshot 2025-02-19 192145](https://github.com/user-attachments/assets/e97f0288-9baf-437d-8332-c865accf1b7e)

Change the space delimiter field to 5 to get the destination address

![Screenshot 2025-02-19 191830](https://github.com/user-attachments/assets/5695900f-db14-465e-b1f1-b7ce7dbddf57)

Change the (.) delimiter field to 5 to get the destination port

![Screenshot 2025-02-19 192214](https://github.com/user-attachments/assets/d15c27d5-263f-4e5f-8987-63dd346f7be1)

The infected endpoint IP address is 10.0.2.10

Most used port protocol in this pcap --> 80/53/21

## Investigate HTTP Traffic

There is a high volume of traffic to port 80, indicating potential web-based communication. To investigate further, I will analyze HTTP GET and POST requests to identify any suspicious activity.

![Screenshot 2025-02-19 192826](https://github.com/user-attachments/assets/5ac5c768-c9e8-44f0-85e3-d4aa8fae0cff)

These 2 GET packets and 1 POST packets caught my attention.

![Screenshot 2025-02-19 192905](https://github.com/user-attachments/assets/4fe35ca1-1b04-445f-a829-ef1240e9be87)
![Screenshot 2025-02-19 192930](https://github.com/user-attachments/assets/e5cfedae-52cd-439f-8c98-8f7efa008701)
![Screenshot 2025-02-19 195202](https://github.com/user-attachments/assets/2d90f819-6dae-41f8-85cc-96ffe5b53a6b)
- on the POST packet, I wanted to see if I can get any sort of credentials posted within the payloads

![Screenshot 2025-02-19 194645](https://github.com/user-attachments/assets/c8771810-ecd9-4774-80ef-7987cbdbf0da)
- username: bsmith   password: ilovecats9182

## Investigate FTP Traffic
Investigated FTP traffic as we saw a lot of activity in port 21.

![Screenshot 2025-02-19 200259](https://github.com/user-attachments/assets/9bbb798a-e0f3-4b14-9fcf-b4775ff7130a)
![Screenshot 2025-02-19 200410](https://github.com/user-attachments/assets/aa8e079f-2d00-4f2b-94f9-88b8edb91ee2)

Infected endpoint has successfully logged into the FTP server using these credentials
- demo:password
  
Also successfully transferred readme.txt file from the FTP server to the endpoint.

- the attacker can now gather more information on us (RECON) and exploit our misconfigurations

## Investigate User-Agent String

![Screenshot 2025-02-19 200821](https://github.com/user-attachments/assets/2eaa41dd-6551-4aa3-b91d-72eb14ab8640)

TeslaBrowser/5.5 User-Agent is used by Lumma Stealer (malware family) 
- targets mostly 2FA browser extensions

![Screenshot 2025-02-19 201235](https://github.com/user-attachments/assets/c0a5f8d6-1a4f-425e-bc5a-ee3a49211d0b)

The endpoint tried to connect to this Telegram URL which most likey is the command and control server that the adversary is using




















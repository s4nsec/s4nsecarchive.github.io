# IRZ RUH2 GSM Router XSS vulnerability

### Affected Product
IRZ RUH2 GSM ROUTER

### Affected version
RUH2

### CVE ID
CVE-2021-32302

### Vulnerability Type
Cross Site Scripting (XSS)

### Description
There exists Cross Site Scripting vulnerability in IRZ Electronics RUH2 GSM router that allows attacker to obtain sensitive information via the Upload File parameter. To trigger XSS, an adversary just needs to upload a file with a name like "<script\>alert('XSS')</script>.png"



2 years ago I was performing penetration tests on IRZ's RUH2 GSM Router's web interface.
I was able to find 2 XSS vulnerabilities while looking at Send SMS and file upload sections of the inteface.

First, I tried "Send SMS" functionality and found out that you can trigger XSS if you send an SMS through the interface:
![image](https://user-images.githubusercontent.com/99656904/220202825-22385a64-37ee-45bd-9357-0a4aa6850a34.png)

Secondly, there was a file upload functionality on the web interface.
If you uploaded a file with a name like "<script\>alert('XSS')</script>.png", then when uploaded, the interface would try to display the file's name and trigger XSS:  
![image](https://user-images.githubusercontent.com/99656904/220203428-d6199044-4028-4285-acf9-add0dc3d08ae.png)

I emailed the vendor regarding the vulnerabilities, however, could not get a response...

### References
https://irz.net/en/products/routers/ruh-series/ruh2

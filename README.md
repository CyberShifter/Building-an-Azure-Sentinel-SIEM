# Building-an-Azure-Sentinel-SIEM
---
---
Overview
---
---
I implemented an efficient security strategy using Azure Sentinel, Microsoft's SIEM solution. By deploying a virtual machine as a honeypot, I attracted potential attackers to monitor their activities. Integration of audit logs, specifically login successes and failures, into Azure Sentinel provided a centralized effective SIEM environment. This allowed precise tracking of intrusion attempts, revealing usernames, passwords, and attack frequency.
---
---
[Microsft Azure Student Sign Up](https://azure.microsoft.com/en-us/free/students)

I created a free Microsoft Azure account with my student email. If you are a student at any educational institutiion, Microsoft provides free credits that you can use in Azure for a subscription to the Azure Portal. It lasts about 12 months students should take advantage of the opportunity. Now that I created my account, I will head over to the Azure Portal. I placed the link above. 


1. After creating my account I am redirected to the Azure Portal home page:

![27](https://imgur.com/noq7d1z.png)

---
2. The very next thing that I do is navigate the virtual machine in Azure. I proceed by choosing the icon with a computer on it labeled "Virtual Machine" or just type it into the Azure Portal seachbar. After doing that I am redirected to this page:

![28](https://imgur.com/4CJheM5.png)
---
3. I click on "Create" and a selection of the different types of Azure VMs pop up. I click on the option that says "Azure Virtual Machine"

   **Note to self**: This virtual machine will be exposed to the internet. Different people from different countries      will attack this VM. They'll start trying to log into it as soon as they discover it. This is basically going to be    my Honeypot.

   ![29](https://imgur.com/vs5hI94.png)
---
4. I am redirected to a page where I am able to pre-configure my honeypot VM. Down below I have taken note of the pre-configurations that I made.

*Resource Group* : Under the drop down entry space, I clicked on "Create New" and named the resource group, "honeypotlab".
      **Note to self**: Resource group is the logical grouping of resources in Azure that usually shares the same life span. Everything I do in this project will be placed in the same resource group.

*Virtual Machine Name* : Decided to name my VM "honeypot"

*Region* : I put everything in the "(US) West US 3" region. This is the region where the data will exist in.

*Image* : I am using the "Windows 10 Pro Version 22H2 - x64 Gen 2"

![30](https://imgur.com/jCb2P6A.png)

---
5. *Size* : I used the standard size that was given to me by default. No need for me to change it.

  I also created a username and password that I would later need to log in to the virtual machine. 


![31](https://imgur.com/bNZztGt.png)

---
6. I then clicked on the box in the **Licencing** to confirm that I have a Windows device . Then clicked the "Next: Disks>"


![32](https://imgur.com/XVPpr7A.png)

---
7. Everything has been configured for me in the disks section by default, so I click on the "Next: Networking>" button.

![33](https://imgur.com/jOIUZfw.png)

---
8. On the networking section, I look for the "NIC Network Security Group".
   
   **Note to self** : This is like my firewall. My protective barrier between my network and the external threats.

   I change it to "Advanced".

   ![34](https://imgur.com/8xVam3f.png)

---
9. Then navigating below to "Configure Network Security Groups", I click on "Create New" highlighted in the blue hyperlink. Now I'm redirected to this page

    ![35](https://imgur.com/VHruyU9.png)

---
10. I now delete the current imbound traffic rule that I have there by default. To do this, I hover over the imbound rules with my mouse ad find the trash can icon. I click and delete the default imbound rule to create a new pair of rules. My imbound rules for my firewall are now empty.

![36](https://imgur.com/I2RwfLv.png)

---
11. I navigate to the blue hyperlink that says "+ Add an Imbound Rule". I click on it and a setting pane appears on the right side of my screen with different options as to how to configure rules regarding Imbound traffic.

I will change my:

* **Destination Port Ranges** : changed this entry to a "*" that represents "ANY" port. So I can recieve traffic from all types of ports.
* **Actions** : filled in the "Allow" bubble
* **Name** : I just made a name a name up called "DANGER_ANY_IN"

Then when I completed that, I clicked "Add" at the bottom of the pane and I have finally configured my imbound rules. This will allow all traffic from the Internet to enter my virtual machine.

I will then click "OK" at the bottom of the screen and I am redirected back to the Networking Section

![37](https://imgur.com/ABQ7PlV.png)

---
12. I navigate to the "Review + Create" pane at the top and click on it. I am now done with configuring my honeypot VM. This part gives me an overview of how I configured my VM. So now that I am satisfied with it, I just click on "Create".

    **Note to self** : The point of this configuration for this lab is that I want to make the VM very discoverable by any means necessary. Whether the attackers that I am trying to lure     are doing TCP pings, SYN scans, or even an ICMP ping. I want the VM to be discoverable quickly so that people know that my VM is online and will subsequently start to attack it.

    
    ![38](https://imgur.com/u7o1WZi.png)

---
13. My VM deployment is in progress of completion

![39](https://imgur.com/V7lh32B.png)

---
14. It took a few mins to create but within those few minutes, I created a Log Analytic Workspace. I type in "Log Analytic Workspace" in the Azure Portal Seach bar and click on the icon.

![40](https://imgur.com/3ZsXe9a.png)


    **Note to self** : The purpose of this is to ingest the logs coming from my Honeypot VM. In this project, I will ingest the "Windows Event Logs".

I will create my own custom logs that contain geographic info that will allow me to discover where attackers are coming from. So to do that I click on the three dots towards the bottom of the screen to create the log analysis workspace

My SIEM in Azure Sentinel will connect to the log analysis workspace that I am creating to be able to display that geo-data on the map

---
15. I am now redirected to the Log Analysis Workspace section and I navigate to the "+ Create" button on the top left of the screen.

    ![41](https://imgur.com/3ZsXe9a.png)

---
16. Redirected to the creation part of this section, I choose the same resource group that I created in the beginning called, "Honeypotlab"


 ![42](https://imgur.com/ck4VVaT.png)

 I named my Log Analytic Workspace "law-honepot1" and changed my region to "(US)West US-3".

---
 17. After completing that, I click on "Review + Create" at the bottom of the screen and review my configurations. Then click "Create"

![43](https://imgur.com/GzCpeR8.png)

---

18. While my Log Analysis Workspace is being prepared for deployment....


![44](https://imgur.com/jfdJQjg.png)


I will navigate the search bar in Azure and type in "Defender for Cloud" and click on it to be redirected to this page


![45](https://imgur.com/jBmSYN0.png)


**Note to Self** : I'm doing this because I want to enable the ability to gather logs from the VM into the Log Analysis Workspaces.

---
19. Navigate to the left task pane and under the "Management" subheading, click on "Environment Settings"


![46](https://imgur.com/n191DqK.png)

---
20. Turn "Defender CPSM" on and save it.


![47](https://imgur.com/p6uYpz7.png)

---
21. Once I save that, I go back to the Log Analytic Workspace to connect it to the VM. Click on "law-honeypot1"


![48](https://imgur.com/pQJ85Zj.png)

---
22. I navigate the  "Virtual Machines" section on the left pane and click it. I now see the VM that I named "honeypot-vm" and proceed to click on it.

![49](https://imgur.com/b8I2VXV.png)

---
23. Click on the "Connect" icon

![50](https://imgur.com/K5IzvOj.png)

---
24. Now I will set up Microsoft Sentinel. So I navigate to the Azure Portal search bar and type in "Sentinel" and click on it.

    **Note to Self** : This is a SIEM that I am going to use to visualize the attack data.

    ![51](https://imgur.com/OsW1IFY.png)

---
25. Once selected, I click on "+ Create"

    
![52](https://imgur.com/IcZH87H.png)

---
26. Then I click on the Log Analysis Workspace that I want to work with which is "law-honeypot1"

    
   ![53](https://imgur.com/CtgC8sb.png)

---
27. I click on "Add" at the bottom left of the page and the deployment will take place as I wait for it to finish

    ![54](https://imgur.com/Q69i2dH.png)


---
28. As I wait for it to finish, I will go back to look and see if my honeypot VM is done. As I was documenting, it finished quicker than I thought so  I click on it and take a look at the overview. I copy the public IP Address on the right side of the screen
   

![55](https://imgur.com/Q69i2dH.png)

---
29. After copying I am going to log in to my Virtual machine via Remote Desktop Connectionand begin performing activities in there. So I click on the Windows logo (bottom left of my screen) and type "Remote Desktop Connection" and click on the application.

![56](https://imgur.com/yJOGrvM.png)

---
30. I then paste the IP Address that i copied into the Remote Desktop Connection API white space entry. Then I click "Connect" button.

![57](https://imgur.com/GpPGDmS.png)

---
31. After clicking that, Admin credentials will need to be entered in order to connect to the IP Address/ VM remotely. I will not be using the credentials that I created in Azure earlier, so I click on the blue hyperlink that says "More choices" and enter those credentials and click "Ok". Then "Accept" the certificates.

![59](https://imgur.com/SB6lv1t.png)

---
32. I am now redirected to the Remote desktop Virtual machine Interface (Windows setup)

![60](https://imgur.com/xnX4IGH.png)

---
33.In this process, I configured my VM setup slightly. I -

    * Turned the *location* off
    * Turned *advertising* off
    * Turned *Tailored Experiences* off 
    * Turned *Inking & Typing* off

Then I clicked "Accept" on the bottom right of the page.

![61](https://imgur.com/CxvaxAl.png)

--
34. I am now inside of my VM. I see a Notification pane on my right hand side of the screen asking if I want to be "discoverable by other devices" on the network. I click, "yes" because it's my objective of this project is to be seen by other devices so that I can bait them and capture them attempting to try and log into my VM.

![62](https://imgur.com/3UoRLvM.png)


---
35. I proceed to navigating to the Microsoft Edge Application to set it up because I will be using it for IP Addresses that i capture and just so curiously happen to want to know their location.

![63](https://imgur.com/qT7KnnB.png)

---
36. Click on start and type in "Event Viewer". I will be taking a look at the some logs.

![64](https://imgur.com/VirL3M4.png)

---
37. I am now in the *Event Viewer* after taking a wgile to load.

![65](https://imgur.com/r5pL1An.png)

---
38. I will then navigate to the "*Windows Log*" >> "*Security*".


![66](https://imgur.com/GYx7wk0.png)

It will display all the security events on this vM that I and other people have done. So if I attempted to log in and I mistakenly fail my password attempt, I will see that mistake in this Windows Log and it is termed "*Audit Failure*"

---
39. I was able to observe really closely during this process, and under keywords column, it says "Audit Success". This shows the successes of all the times that me and other people (attackers) that were able to log into this VM via RDP connection.

    **Note to Self** : I was attempting to catch an Audit Failure to gather it but there are no failures because I forgot to fail the Remote Desktop Connection login. I'll just click on
    one of the *Audit Success* logs and analyze what's in it down below:

    
  ![67](https://imgur.com/zrAUNb8.png)

  As seen above in the screenshot, I have an account that shows the username and domain I used to log on to the VM.

  Assuming that I did attempt to log in with the wrong password, I would've seen an "*Audit Failure*" in my keyword column but that isn't the case. 
  
---
40. I will attempt to log on to the VM with a horrible password attempt just to capture an Audit Failure. So i'll open Remote Desktop on my VM and attempt a log in failure.

    Then I come back and log on to RDC in my actual PC abd attempt a horrible password attempt as well.


![68](https://imgur.com/uLyqEhZ.png)

---
42. After a failed login attempt, I go back into the "Event Viewer" >> "Windows Logs" >> "Security" and notice that I have some "Audit Failures"
 

![70](https://imgur.com/bybpTLt.png)


---
41. Once I click on audit failure, I can see all types of info regarding what led to the failure and in this current screenshot, I can see that the "failure reason" is caused by an "error occurring during log in". 

![69](https://imgur.com/Rpo1uir.png)

After attackers have discover this VM on the internet, I will be able to see the IP Addresses from the devices they attempted to log in from. This will be called the "*Source Network Address*" and I should be able to see their Workstation Name" as well.


---
43. When scrolling down, it provides more info about the failure.

    ![71](https://imgur.com/0kUOURT.png)


---
44. I want to be able to tell the hackers location since the *Event Viewer* limits me from that but I can always use a handy tool called "ipgeolocation.io" and will open this in Microsoft Edge in my VM to type it in and paste the attackers IP Address in the application interface and the API will give details of the attackers location.

    **Note to self** : I plan on using the info that I get from the API to create my own custom log and I will send that log into my Log Analytic Workspace in Azure then I will use the       Sentinel SIEM to read the longitude/latitude OR plot out the different attackerson the map.

![72](https://imgur.com/WxN9DIA.png)


---
45. Before I do this, I will turn off my firewall in the VM so that it can respond to ICMP echo requests so that attackers can discover it on the Internet faster. So I go to my actual PC and open my command line.
    

![73](https://imgur.com/p2NyW3y.png)

---
46. I will enter my VM honeypot's public IP Address into the command line with the ping command as well as the "-t" at the end so it should look like


            ping 20.38.32.175 -t

![74](https://imgur.com/95kEqKH.png)

    
    
And this will just ping my VM.

---
47. My ping request to my VM timed out because of the firewall being on.

![75](https://imgur.com/gBmbFno.png)


---
48. I exit the command line and find the Windows logo at the bottom left of my screen and search for the Windows Firewall by typing: "wf.msc" in the Windows search API. I click on it.

![76](https://imgur.com/qu4JFYt.png)


---
49. Once I am redirected to the firewall page, I navigate abd look for "Windows Defender Firewall Properties" in the blue hyperlink and click on it.

![77](https://imgur.com/WKtfkj2.png)


---
50. As seen in the "Domain Profile" pane, I turn off the firewall state

![78](https://imgur.com/i4CVml8.png)


---
51. I then navigate to the "Private Profile" pane and turn off the firewall state

![79](https://imgur.com/1EsTOJb.png)


---
52. I then navigate to the "Public Profile" pane and turn off the firewall state. Then click "Apply" at the bottom when I'm done turning off the firewall.


![80](https://imgur.com/JefChNv.png)


---
53. I open the command line again and attempt to ping my VM once again. This time it is successful. I recieved a ping response from my VM. So I close the CLI and go back to my VM.


![81](https://imgur.com/p9wGA5d.png)


---
54. Now I will downlaod a Powershell script for this SIEM off of a github repository and paste it in my notepad.


![82](https://imgur.com/vzk2g9u.png)

---
55. I open up Powershell ISE on my VM

![83](https://imgur.com/tocTjau.png)


---
56. I click on "New" to operate on 2 seperate workspaces on Powershell. One will be for my input and the other will be for output.


![84](https://imgur.com/Ia7akdw.png)


---
57. I will then save the script on the desktop as "Sentinel Script"

**Note to myself** : Basically this script runs in perpetuality. It carries out a loop and essentially what it does is tht it looks through the event log  that I was looking at earlier, mainly the security log in particular, grabs all the events of people who failed to log into my virtual machine, grabs their IP address and collects their geo-data from them and creates a new log file for them. The log file gets output to the "C:\Program\Data" in line 4 of my script which is my program data file.


![85](https://imgur.com/sxTXo81.png)

---
58. After I get the API key from the "ipgeolocation.io", then I will paste it in my script on line 2 and see if it runs. Looking down at the screen, the powershell script does run.

![86](https://imgur.com/xypexRO.png)


The purple output seen on the output part of my Powershell API appears whenever there is a failed log on in the event viewer for the "Failed Audits" and takes them all out just to send them to the API of ipgeolocation. It will then get the country in geo data and creates a "failed_rdp" log text file full of multiple locations from attackers who have tried to log on from different parts of the world.


---
59. I leave the VM for my actual PC and go back to azure to create a custom log inside my log analytics workspace, that will allow me to bring that custom log with the geo-data into my *Log Analytic Workspace*. So I type in "Log Analytics Workspace" and click on it

![87](https://imgur.com/QfOBOxO.png)



---
60. I click on the workspace named "law-honeypot1"


![88](https://imgur.com/mdAxW6D.png) 




---
61. Then I go to "Custom Logs".


![89](https://imgur.com/ZdCECFW.png) 


---
62. I add the log file from my VM, the textfile that I named, "failed_RDP". I copy everything in that text file and then paste it into a text file in my actual PC and use it as the file to create a custom log.


![90](https://imgur.com/RuaMVnr.png)


---
63. After clicking "Next", it revealed a preview of the text


![91](https://imgur.com/Jx3k4mU.png)


---
64. I'll then change the collection path to "Windows" and ckick "Next".

![92](https://imgur.com/RKlC1Qw.png)


---
65. After clicking next, I give my log a custom name, which I called "FAILED_RDP_WITH_GEO_CL" then click "Next"


![93](https://imgur.com/1pd1hbT.png)


---
66. I click on "Create" after viewing over the custom log. It takes a while for the *Log Analytic Workspace*" and the VM to sync up and bring the log into the Log Analytic Workspace, but to see the log, I will take a screenshot of the text I see in the custom logs.


![94](https://imgur.com/u4A1VEi.png)

---
67. I go to "Logs" on the left pane and I type in "FAILED_RDP_GEO_CL" into the table as seen below:

![95](https://imgur.com/seGXgLB.png)

---
68. I click on "Run" and I can see some log entries here at the bottom but I don't have any yet.

![96](https://imgur.com/l96e6qt.png)

---
69. Now I can type in "Security Event" after erasing "FAILED_RDP_GEO_CL" and run it. [This is the Windows Event Log]

![98](https://imgur.com/dDQaTb4.png)

---
70. I am now able to see all the entries that I am looking at that correlate to the Audit Failures in my Event Viewer in my Virtual Machine. It is just being brought into Log Analysis Workspace.


![99](https://imgur.com/gGD3buz.png)


---
71. If I go back to my table and enter "Security Event | where Event ID == 4.625" and run it, it will return all of the failed RDC logs and when I expand it, I can see the IP Address, account log-in failure, the username of the account that I failed to log in with, and so mcuhh more.

![100](https://imgur.com/rGYR0QU.png)

---
72. Now I go back in the tables and type in "FAILED_RDP_WITH_GEO_CL" and run it. I now see a whole bunch of logs come through at the bottom of my "Tables".


![101](https://imgur.com/a0hJzCe.png)


---
73. If I go back to my VM, I can match the "Raw Data" with the "FAILED_RDP_WITH_GEO_CL.txt" like inside of my VM. [This is my custom log entries]


![102](https://imgur.com/gZQ7i53.png)

---
74. If I go back to my Azure portal and look at the "Raw Data" column, I can see that in contains that whole line in the previous screenshot.

![103](https://imgur.com/2H7SN7V.png)


---
75. The next thing that must be done is taking the "Raw Data" that I have and extractingcertain fields from it to make their own fields. So I to have like a *Latitude* field, as well as a *Longitude* field, and a *username* field, and a *password* field, and  a *country* field, and so on and so forth.  I'll expand the logs at random and click on the 3 dots. I see an option that says "Extract fields from FAILED_RDP_WITH_GEO_CL".


![104](https://imgur.com/EluSTaN.png)


---
76. I click on it and I can see the raw data logging, next to it, I can see the latitude and longitude, the destination host and username and much more.


![105](https://imgur.com/EluSTaN.png)


---
77. I want to extract all of these and turn them into their own fields. What I'll do is highlight the field of latutude.


![106](https://imgur.com/73sN5Hb.png)


---
78. I will then automatically recieve a pop up asking for a field title and field type.I name my field title "Latutude and my field type as a "numeric". Then I click "Extract".


![107](https://imgur.com/Cj7uXj8.png)


---
79. I recieve another pop up on the middle of the screen and I make sure that all the fields look proper. Eveything does look accurate so i'll save the extraction by clicking on "Save" at the bottom of the screen



**Note to Self** : I repeat the smae processes for *Longitude* , I*Country* (but changing the field type to text), *State* (change the field type to text), and *Sourcehost* (change the field type to numeric)


![108](https://imgur.com/TarGxEe.png)


---
80. While creating a field for *longitude*, I noticed that the second search result on my gpop up overview is highlighted (meaning it is screwed). So I click on the compass logo on the blue header of it, and I "modify the highlight".




![109](https://imgur.com/undefined.png)



---
80. So I'll highlight the longitude and a pop up will appear and I end up editing it by changing the field type to metric.

![110](https://imgur.com/FKPkym7.png)



---
81. At the end of the extraction process, I end up with this end result. There is no data in any of the columns yet. There is no data in any of the columns yet. The data will come in on subsequent logs, so the next time an attacker fails to log in, the data will be passed out and put in those fieds properly.

![111](https://imgur.com/AFocDUc.png)


---
82. I was able to navigate to my *Custom logs* and into my *Custom Fields* just to see all the fields that i extracted, which was everything in the *Raw Data*. Whenever I make an extract, it creates a *custom field* here and in the future, when I get more data coming in, like when more people fail to log in, and I find that the latitude or any of the custom field names, I can always delete the field and redo the extract for it.

![113](https://imgur.com/ueWso5E.png)



---
83. I will now finish setting up my map in Sentinel. I navigate to *Microsoft Sentinel* on Azure Portal and click on "law-honeypot1".


![114](https://imgur.com/Mt9D3of.png)


---
83. When I click on *Overview*, I am able to see the general Sentinel dashboard. Iam able to see the logs coming in, I can see the "FAILED_RDP_WITH_GEO_CL" and "Security Event" which is my event log from the actual VM.

![116](https://imgur.com/MtFKBph.png)

---
84. I am going to set up my actual geo map, so I click on "*Workbooks*" and click on "*Add Workbook*"

![117](https://imgur.com/6Udn8qf.png)


---
85. Then I click on "*Edit*"

![118](https://imgur.com/gf7vzCH.png)


---
86. I'll then remove the two default widgets given to me by the Workbook.

![119](https://imgur.com/74ab2LB.png)


---
87. Then I'll add the query in the space.

![120](https://imgur.com/3BVjIM4.png)


---
88. Then I'll paste the query in the space.

![121](https://imgur.com/gXj3qIB.png)


---
89. I modify this query to be tailored to the one in my VM so it looks like this (I ended up removing the *destinationhost* because of the problems it gave me but I was able to figure out where i went went wrong)



![121](https://imgur.com/6tAs3sW.png)


---
90. So I then run the query and it gives me a result at the bottom of the screen.

![123](https://imgur.com/8v0RqCo.png)


---
91. I want to visualize this on a world map, so I click on "*Visualization*" then navigate and look for "*Map*"

![124](https://imgur.com/vsgIDaW.png)


---
92. Now I run the query.


![125](https://imgur.com/RF7Jzfr.png)


---
93. On the right, I have a pane for my Map Settings. I modified the *Longitude* , *Latitude*, and other fields to the proper field names I designated them to when I created them and click on "*Apply*".

![127](https://imgur.com/pcSiDDw.png)

---
94. On the map, I can see my current location pin pointed on the map. This is where my failed log in attempt came from.


![127](https://imgur.com/mJxHkQN.png)


---
95. I can also configure the map settings in the right pane. I was able to configure/change the *metric value*, *metric label*, it allowed me to view metrics off the map, such as the IP Address,and the ammount of people who attempted to log in.

![129](https://imgur.com/Mkd1qdb.png)

---
96. This is a closer view.

![130](https://imgur.com/nySNTGO.png)


---
97. Now I save and close the *Map Settings*. I will save it as "Failed RDP World Map" and click save when done.


![131](https://imgur.com/AbgjYkZ.png)

---
98. Clock on "*Done Editing*"


![132](https://imgur.com/d1A1NM7.png)


I now have my map so I'll be back in a 3 hours to see if other people have discovered this honeypot and see if other people are trying to log in to my VM.

*Note to Self* : I realized that the Powershell scriptnhas to be running in my VM as it is just like a proof of concept implementation. I made sure it runs so that when subsequent logins fail, it'll be recorded. If the script in Powershell isn't running all the failed logins go to the *Windows Event Log* but they won't go into my custom logs which is what is needed for the extraction of the geo data.


---
99. Okay, so it has been about 5 hours and I couldn't be bothered to wait, I did notice alot of activity regarding failed logins down below:


![133](https://imgur.com/nbzyLh9.png)


As seen on my SIEM, someone in China attempted to get into my Virtual Machine. I did mis-edit  this because the U.S. bubble is massive and the China bubble is small so I edit it.

---
100. As you can see in my VM, they attempted to log in with Mandarin letters in the username section of my Raw Data.


![134](https://imgur.com/kcKweXS.png)

---
101. I went to Google Translate to curiously discover what the attacker even put into the VM username login Interface. The translation of the letters was finally translated and it meant, "*User Account*" in English which I found interesting.

![135](https://imgur.com/4bQhrNo.png)


---
102. Fast forward to another 4 hours and I see attackers from Taiwan trying to log into my honey pot VM with 312 failed attempts from my location on the map.

![136](https://imgur.com/xNNXseH.png)

---
103. Fast forward to the following day (8 hours later) and I have 249 log in attempts from the Phillipines, 530 attempts from China, and 836 attempts from Taiwan

![137](https://imgur.com/tk7EzsX.png)


---
104. In my VM, located in my Poweshell Script that I am still running, I see my bottom hal of the script execution and it shows the region of the Phillipines. And the attacker trying to login with the username "*Admin*".

![138](https://imgur.com/mod5uTM.png)



---
105. I view my SIEM again in about another 12 hours since I am at work during the time, and I am viewing attacks happening to my VM from all over the world. I have attackers from Russia, Hong Kong, India, China, Taiwan, Phillipines, Chile and other countries. What really stood out to me was the 3,000 attempts from the Phillipines. I'm pretty sure it was the same attacker just trying to brute force his way into my VM and I admire his/her dedication haha.

![139](https://imgur.com/zPaqoQY.png)


---
---



---
Final Thoughts/Conclusion
---
In thi lab, L learned alot. I learned that as soon as any device gets put on the Internet, people will attempt to get into your device. It doesn't matter if you are the US Department of Defense, the U.S. President, or any type of big business. Attackers do not care. These attackers scavenge on the Internet for hosts to get in. You are going to be a target of opportunity if your device is online and ports are open.

Another thought is that digital civilians/people should avoid using "Admin" as username and passwords. In my Powershell Script that I have running, the amount of times that attackers attempted to log in to my honeypot using "Admin" as the Username was astonishing to me because I know a lot of companies and people who use "Admin" as credentials for logging into devices. People need to learn to exercise strong passwords and enabling MFA to mitigate attacks. 

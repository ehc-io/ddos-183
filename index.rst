Agility 2018 Hands-on Lab Guide

Mitigating Real World DDoS Attacks

Presented by: Nilesh Mistry, Barrymore Simon and Jack Fenimore

What’s inside

`Lab Diagram 1 <#lab-diagram>`__

`Access and Credential Summary 2 <#access-and-credential-summary>`__

`Helpful Tips and Tricks 2 <#helpful-tips-and-tricks>`__

`Introduction to DDoS Hybrid Defender
4 <#introduction-to-ddos-hybrid-defender>`__

`Accessing the Lab Environment 5 <#accessing-the-lab-environment>`__

`Task - Use RDP client and connect to your Windows Jumpbox IP and the
Win-ToolsServer IP
5 <#task---use-rdp-client-and-connect-to-your-windows-jumpbox-ip-and-the-win-toolsserver-ip>`__

`Exercise 1– Environment Review 7 <#exercise-1-environment-review>`__

`Lab 1.1 – Review the tools and understand your environment
7 <#lab-1.1-review-the-tools-and-understand-your-environment>`__

`Lab 1.2 – Launch an attack and view traffic
8 <#lab-1.2-launch-an-attack-and-view-traffic>`__

`Exercise 2 – Manual Mitigations 10 <#exercise-2-manual-mitigations>`__

`Lab 2.1 – Device Level Protection for Mitigating Attacks.
10 <#lab-2.1-device-level-protection-for-mitigating-attacks.>`__

`Lab 2.2 – Device Level Protections for Mitigating Attacks
11 <#lab-2.2-device-level-protections-for-mitigating-attacks>`__

`Lab 2.3 – Device Level Protections for Mitigating Attacks
12 <#lab-2.3-device-level-protections-for-mitigating-attacks>`__

`Lab 2.4 – Device Level Protections for Mitigating Attacks
13 <#lab-2.4-device-level-protections-for-mitigating-attacks>`__

`Lab 2.5 – Device Level Protections for Mitigating Attacks
13 <#lab-2.5-device-level-protections-for-mitigating-attacks>`__

`Lab 2.6 – Protected Object Level Protections for Mitigating Attacks
15 <#lab-2.6-protected-object-level-protections-for-mitigating-attacks>`__

`Lab 2.7 – Protected Object Level Protections for Mitigating Attacks
17 <#lab-2.7-protected-object-level-protections-for-mitigating-attacks>`__

`Lab 2.8 – Whitelisting 18 <#lab-2.8-whitelisting>`__

`Lab 2.9 – BOT Defense for Application Attacks. 19 <#_Toc520145141>`__

`Exercise 3 – Automatic Mitigations
22 <#exercise-3-automatic-mitigations>`__

`Lab 3.1 – Auto Thresholding for Mitigating Attacks.
22 <#lab-3.1-auto-thresholding-for-mitigating-attacks.>`__

`Lab 3.2 – Behavioral L4 for Mitigating Attacks
25 <#lab-3.2-behavioral-l4-for-mitigating-attacks>`__

`Lab 3.3 – Behavioral L7 for Mitigating Attacks
28 <#lab-3.3-behavioral-l7-for-mitigating-attacks>`__

Lab Diagram
===========

|image10|

Access and Credential Summary
=============================

You will be using the Win7 JUMPBOX to access other systems for all labs.
You will use Putty that has been preconfigured with appropriate keys to
access the Good Client and the Attacker systems. To run scripts, you
will need to have root access, requiring you to **‘sudo bash’** before
running attacks, baselines, etc.

+--------------------------------+------------------+-------------+
| System                         | Username         | Password    |
+================================+==================+=============+
| Jumpbox                        | external\_user   | f5DEMOs4u   |
+--------------------------------+------------------+-------------+
| Hybrid Defender – WebUI/TMUI   | admin            | f5DEMOs4u   |
+--------------------------------+------------------+-------------+
| Hybrid Defender – CLI          | root             | f5DEMOs4u   |
+--------------------------------+------------------+-------------+
| Passive BIG-IP – WebUI/TMUI    | admin            | f5DEMOs4u   |
+--------------------------------+------------------+-------------+
| Good Client                    | ubuntu           | Use key     |
+--------------------------------+------------------+-------------+
| Attacker                       | ubuntu           | Use key     |
+--------------------------------+------------------+-------------+
| Win-ToolsServer                | external\_user   | f5DEMOs4u   |
+--------------------------------+------------------+-------------+
| WebServer / Auction Server     | root             | default     |
+--------------------------------+------------------+-------------+

Helpful Tips and Tricks
=======================

Here are a few tips that you can use during the labs. Since the
environment and all its components are running in a virtualized
environment with limited shared resources you may encounter some slow
performance.

1 > When using the Wireshark tool, it will capture a lot of packets.
During DDoS attacks the tool will be overwhelmed. Its recommended that
you start the capture and then stop it soon so that you can view the
data captured easily.

2 > If you find that you are not seeing any attacks then go back and
check if the Attack you launched is still running. If it has stopped,
kindly relaunch it.

3 > If an attack is not being detected on the DHD check the value of
your detection threshold EPS. For an attack to be detected this value
must be lower than the attack being launched. Similarly, the rate/leak
limit value sets the threshold for dropping the packets.

4 > During automatic/behavioral mitigations labs there is about 10-15
minutes of baseline traffic learning time for the Hybrid Defender. Use
that time to ask questions, chat with F5 Engineers and/or your peers
about DDoS mitigations, security and what they are doing in their
organization. Additionally, browse around the DoS Visibility tool to see
some cool graphical reports that were generated.

5 > Make sure the name of the Protected Objects you create in various
labs matches exactly to what is provided in this guide otherwise the
scripts/commands for monitoring learning status will not work as they
are tied to specific profile names that get created.

6 > You will notice that the commands “\ **sudo bash**\ ” “\ **cd
f5agility**\ ” are included in each step. If you are already logged in
and have root privileges and in the f5agility folder then kindly ignore
those steps. If not, then use them. Basically, you need root level
access to execute the scripts and be in the f5agility folder/directory.

7 > Since the WebUI/TMUI will look the same for the BIG-IP Passive and
the Hybrid Defender device make sure that all mitigation/changes are
being made to the Hybrid Defender only and the Passive device is used
only for visibility.

8 > Don’t forget to use CTRL+C to break and stop the attacks so that you
get better responses from various tools once you have enough data.

9 > When starting a new capture in WireShark always select continue
without saving when prompted.

10 > Use Right click and “Open in new tab” to browse various DHD menus
(Overview, Event Logs,etc) so you don’t have to go back and forth.

11 > **STOP** all attacks, good traffic baseline scripts after end of
each lab before proceeding to the next lab.

12 > Use the PuTTY shortcuts on the desktops to access various shells.
The PuTTY window has a title on top so that you know which shell you are
in. If you get a Security Alert for the Servers Host Key just click YES
to proceed to connect to the shell.

Introduction to DDoS Hybrid Defender
====================================

F5® DDoS Hybrid Defender™ (DHD) protects your organization against a
wide range of DDoS attacks using a multi-pronged approach. By combining
on-premises and cloud technologies, analytics, and advanced methods,
DDoS Hybrid Defender is a hybrid solution that detects network and
application layer attacks and is easy to deploy and manage.

DDoS Hybrid Defender mitigates against the full spectrum of DDoS attacks
including:

• Network capacity attacks

• DNS and SIP protocol volumetric attacks

• HTTP and HTTPS volumetric attacks

• HTTP and HTTPS CPU-based (heavy URL) attacks

You can specify which objects to protect on the network, assigning the
appropriate protections to network devices and application servers, and
prevent attackers from exhausting network resources and impacting
application availability.

**Deployments:**

The deployment you use for DDoS Hybrid Defender™ depends on the needs of
your organization. For maximum DDoS protection, it is recommended that
you deploy DDoS Hybrid Defender inline. However, it can also be deployed
out of band, or in locations where symmetric data flows are not
guaranteed.

Typical locations for the placement of DDoS Hybrid Defender are at the
edge of the network or at the edge of the data center

**Inline deployment**

DDoS Hybrid Defender provides maximum protection when deployed inline in
one of two ways:

• Bridged mode with VLAN groups (This is default and we will use in our
labs)

• Routed mode

**Out of band deployment**

You can deploy DDoS Hybrid Defender out of band in two ways:

• Set up a Layer 2 switch with span ports so that it mirrors traffic
onto DDoS Hybrid Defender. (Our passive device is setup this way in our
labs)

• Configure network devices so that they send NetFlow data to DDoS
Hybrid Defender.

Accessing the Lab Environment
=============================

Task - Use RDP client and connect to your Windows Jumpbox IP and the Win-ToolsServer IP
---------------------------------------------------------------------------------------

**Note: Use the show options to provide **

**User name: external\_user. Password: f5DEMOs4u**

|image0|

 Click YES at the warning

|image1|

**All Exercises/Tasks are to be completed from the Windows Jumpbox.
There are various shortcuts -- Chrome Incognito, Putty shortcuts, on the
Jumpbox that you will use through the exercises.**

Exercise 1– Environment Review
==============================

Lab 1.1 – Review the tools and understand your environment
----------------------------------------------------------

You are the security engineer for Acme corporation. Your organization
has recently seen a lot of outages in your network and applications.
Some of these have been due to DDoS attacks and the outages have caused
a significant loss of revenue as well as reputational impact. You have
made the wise decision to invest in a world class leading edge DDoS
mitigation solution and have the F5 DHD installed in your environment.
It’s been configured in the Layer 2 inline mode and is now available to
you to enforce DDoS mitigations.

*Tools:*

1 > In our lab we have an additional DHD available to you in a passive
mode. It’s basically setup on SPAN ports (out of band deployment) to
provide you visibility.

2 > The Win-ToolsServer is also installed to listen on SPAN port and has
Wireshark available for visibility.

Let’s get familiar on how to use these tools.

Note: Not all attacks will be visible in both tools. So, use the tools
accordingly. This is done purposefully so that you get into the habit of
troubleshooting/fighting attacks in the real world.

Use a web browser (Chrome in incognito mode) to log into the WebUI of
the Passive DHD at https://10.1.1.246 or use the bookmarked shortcut.
Accept the SSL warning and proceed to connect.

Username: admin

Password: f5DEMOs4u

-  Click **Security>>Event Logs>>DoS>>Network>>Events**

-  Click **Security>>DoS Protection>>DoS Overview (**\ Tip: Right Click
   and open link in new tab/window)

-  You will use the above two screens on the Passive DHD for visibility
   of traffic/attacks.

-  On the Win-Tools Server launch Wireshark by using the shortcut link
   on desktop and then click on the blue shark fin on top left corner to
   start capturing data. **(**\ Tip: Use the Red Square button to stop
   captures when needed)

Lab 1.2 – Launch an attack and view traffic
-------------------------------------------

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

# sudo bash

# cd f5agility

# ./lab1-2.sh

-  View Wireshark and notice the ongoing captures.

-  What type of traffic do you notice? As you can see these are all ICMP
   requests/responses and a lot of them. What are the IP addresses
   involved? Can you identify the attacking IP? **(**\ Tip: Did you
   review the lab network diagram?)

|image2|

In the Passive DHD Windows what do you notice? **(**\ Tip: You may need
to click Search button/Refresh button or set Auto Refresh)

|image3|

|image4|

**As you can see the visibility is better in terms of the Attack Vector
and number of packets in/sec on the passive DHD.**

It’s up to you on which tool you may want to use for the remaining labs.
If you are comfortable with WireShark then use that or use the Passive
DHD or both. As noted previously you will have to visit both tools to
see where you can gather some visibility to fight a real-world DDoS
attack.

Use CTRL+C in the attacker shell to stop the attack.

Exercise 2 – Manual Mitigations
===============================

Lab 2.1 – Device Level Protection for Mitigating Attacks.
---------------------------------------------------------

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

# sudo bash

# cd f5agility

# ./lab2-1.sh

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  On the Passive DHD identify the ongoing attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved?

-  Let’s mitigate this attack using Device Level mitigation.

Log into the DHD https://10.1.1.245 accept the SSL warning and proceed
to connect with credentials provided.

-  In the Configuration Utility, go to **DoS Protection>>Quick
   Configuration.**

-  In the **Device Protection** section click **Device Configuration.**

-  In the **Flood** row click the + icon, and then click **ICMPv4**
   flood.

-  On the right-side of the page select the drop-down to **"Mitigate"**

+-------------------------------+----------------+
| Mitigation                    | Fully Manual   |
+===============================+================+
| Detection Threshold EPS       | 100            |
+-------------------------------+----------------+
| Detection Threshold Percent   | 500            |
+-------------------------------+----------------+
| Rate/Leak Limit               | 500            |
+-------------------------------+----------------+

-  On the Hybrid Defender you will now see the attack is being mitigated
   (Where will you check this? Tip: It’s the same places that you are
   looking on the Passive device). You have successfully mitigated a
   network flood single vector attack. Use CTRL+C in the attacker window
   to stop the attack.

Lab 2.2 – Device Level Protections for Mitigating Attacks
---------------------------------------------------------

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

# sudo bash

# cd f5agility

# ./lab2-2.sh

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  On the Passive DHD identify the ongoing attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved?

Mitigate this attack using Device Level mitigation steps like those that
you did in Lab 2.1 above.

Lab 2.3 – Device Level Protections for Mitigating Attacks
---------------------------------------------------------

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

# sudo bash

# cd f5agility

# ./lab2-3.sh

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved? Look closely and you will
   notice that there is a range of destination IPs that are being
   targeted and a lot of SYN, Retransmit, Out of Sequence, RST packets.
   This looks like someone is trying to run a scan against your network.
   How will you mitigate against this? They are “Sweep”ing your network.

-  In the Configuration Utility, in the **Device Protection** section
   click **Device Configuration.**

-  In the **Single Endpoint** row click the + icon, and then click
   **Single Endpoint Sweep**.

-  On the right-side of the page select the drop-down to **"Mitigate"**

+---------------------------+------------+
| Detection Threshold EPS   | 100        |
+===========================+============+
| Rate/Leak Limit           | 500        |
+---------------------------+------------+
| Packet Types (Selected)   | All IPv4   |
+---------------------------+------------+

-  On the Hybrid Defender you will now see the attack is being
   mitigated. This attack is short lived so make sure you launch it
   again if it has stopped to see the mitigation. You have successfully
   mitigated a sweep flood attack. Use CTRL+C in the attacker window to
   stop the attack.

Lab 2.4 – Device Level Protections for Mitigating Attacks
---------------------------------------------------------

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

# sudo bash

# cd f5agility

# ./lab2-4.sh

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  On the Passive DHD identify the ongoing attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved?

-  Use the manual mitigations steps you learned in previous tasks to
   mitigate against all the attack vectors that you have identified.

-  Use CTRL+C in the attacker window to stop the attack.

Lab 2.5 – Device Level Protections for Mitigating Attacks
---------------------------------------------------------

You received a call that a lot of users are intermittently getting a
page cannot be displayed for various applications. Your Network
Operations Center has stated that none of their monitoring systems for
those applications are reporting any outages. The NOC tools monitor
application health using the application URLs like
http://10.1.20.12/index.php and so on. Your users are using the
application using the FQDNs. You suspect that there is an ongoing DDoS
attack and you need to identify it and mitigate against it.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

# sudo bash

# cd f5agility

# ./lab2-5.sh

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  Let’s look at an alternate way to see which vector is being triggered
   so that you can identify the attack. If in your environment you had
   no tools like the Wireshark or the Passive DHD device, you can still
   identify the attack. While the event logs, DoS Overview screens are
   populated only when an attack is detected based on the threshold
   values set, if the attack doesn’t trigger the detection threshold you
   will not see it in the Overview and Event Logs.

-  In the Configuration Utility of the Hybrid Defender, go to **DoS
   Protection>>Quick Configuration.**

-  In the **Device Protection** section click **Device Configuration.**

-  In the **DNS** row click the + icon, and then view the Current Device
   Statistics Section. You can see that we are triggering a vector and
   registering the packets for that vector even though we have the
   default detection/mitigation configured for it.

-  Alternately there is a CLI command also available to view the attack
   vector that is being triggered. Open a putty shell to the Hybrid
   Defender (use shortcut on desktop), login with the credentials:
   root/f5DEMOs4u and then :

# cd f5agility

# ./show\_attackvector\_stats.sh

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved? Hint: (Wireshark) Destination
   IP, Targeted Port and Protocol used.

-  Use the manual mitigations steps you learned in previous tasks to
   mitigate against the attack vector that you have identified.

-  Use CTRL+C in the attacker window to stop the attack.

Lab 2.6 – Protected Object Level Protections for Mitigating Attacks
-------------------------------------------------------------------

You mitigated a DNS vector attack above at device level. You have again
received a call that a lot of users are intermittently getting a page
cannot be displayed for various applications. Your Network Operations
Center has stated that none of their monitoring systems for those
applications are reporting any outages. The NOC tools monitor
application health using the application URLs like
http://10.1.20.12/index.php and so on. Your users are using the
application using the FQDNs. You suspect that there is an ongoing DDoS
attack and you need to identify it and mitigate against it. You don’t
want to implement a mitigation for a vector device wide and want to
specifically mitigate the suspected victim server.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

# sudo bash

# cd f5agility

# ./lab2-6.sh

-  On the WireShark start a capture/stop and identify the ongoing
   attack.

-  On the Passive DHD identify the ongoing attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved?

-  In the BIG-IP Configuration Utility, open the **DoS Protection >
   Quick Configuration** page.

-  In the **Protected Objects** section click **Create**.

-  Configure a protected object using the following information, and
   then click **Create.**

+------------------------+--------------------+
| Name                   | DNSServer          |
+========================+====================+
| IP Address             | 10.1.20.14         |
+------------------------+--------------------+
| Port                   | 53                 |
+------------------------+--------------------+
| Protocol               | UDP                |
+------------------------+--------------------+
| Protection Settings:   | Log and Mitigate   |
| Action                 |                    |
+------------------------+--------------------+
| Protection Settings:   | DNS                |
| DDoS Settings          |                    |
+------------------------+--------------------+

-  In the **DNS** row click the **+** icon, and then click **DNS A
   Query**.

-  On the right-side of the page configure using the following
   information, and then click **Create**.

+-------------------------------+----------------+
| Detection Threshold EPS       | Specify: 10    |
+===============================+================+
| Detection Threshold Percent   | Specify: 500   |
+-------------------------------+----------------+
| Mitigation Threshold EPS      | Specify: 100   |
+-------------------------------+----------------+

-  On the Hybrid Defender you will now see the attack is being
   detected/mitigated. You have successfully mitigated a DNS A Query
   flood. Use CTRL+C in the attacker window to stop the attack.

Lab 2.7 – Protected Object Level Protections for Mitigating Attacks
-------------------------------------------------------------------

There has been a high-profile DDoS attack and you must provide Law
Enforcement some details on the offending IP addresses. In your
environment at any given time you have a few hundred thousands of IP
addresses observed on your network. You want to identify a few offending
IP addresses and blacklist them so that you can provide the details to
Law Enforcement.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

# sudo bash

# cd f5agility

# ./lab2-7.sh

-  On the WireShark start a capture and identify the ongoing attack.

-  Did you identify the attack? What type of attack is it? What Source
   IPs and Destinations IPs are involved? Make a note of the protocol of
   attack and the destination IP (target).

-  We will build a protected object and use Bad Actor Detection and
   Black Listing.

-  In the BIG-IP Configuration Utility, open the **DoS Protection >
   Quick Configuration** page and in the

-  In the **Protected Objects** section click **Create**.

-  Configure a protected object using the following information, and
   then click **Create.**

+------------------------+--------------------+
| Name                   | BadActorServer     |
+========================+====================+
| IP Address             | 10.1.20.12         |
+------------------------+--------------------+
| Port                   | \*                 |
+------------------------+--------------------+
| Protocol               | All                |
+------------------------+--------------------+
| Protection Settings:   | Log and Mitigate   |
| Action                 |                    |
+------------------------+--------------------+
| Protection Settings:   | UDP                |
| DDoS Settings          |                    |
+------------------------+--------------------+

-  In the **UDP** row click the **+** icon, and then click **UDP
   Flood**.

-  On the right-side of the page configure using the following
   information, and then click **Create**.

+--------------------------------------+----------------+
| Detection Threshold PPS              | Specify: 100   |
+======================================+================+
| Detection Threshold Percent          | Specify: 500   |
+--------------------------------------+----------------+
| Mitigation Threshold EPS             | Specify: 200   |
+--------------------------------------+----------------+
| Bad Actor Detection                  | Checked        |
+--------------------------------------+----------------+
| Per Source IP Detection Threshold    | 100            |
+--------------------------------------+----------------+
| Per Source IP Mitigation Threshold   | 30             |
+--------------------------------------+----------------+
| Blacklist Attacking Address          | Checked        |
+--------------------------------------+----------------+
| Sustained Attack Detection Time      | 15             |
+--------------------------------------+----------------+
| Category Duration Time               | 120            |
+--------------------------------------+----------------+

-  On the Hybrid Defender you will now see the attack is being
   detected/mitigated.

-  View the offending IP addresses at **Security>>Event
   Logs>>Network->IP Intelligence **

-  View the Shun list / Blacklist at **Security>>Event
   Logs>>Network>>Shun**

-  You have successfully identified the Bad Actors and put them in a
   Blacklist. Use CTRL+C in the attacker window to stop the attack.

Lab 2.8 – Whitelisting
----------------------

You get a call from your QA team that is running load runner scripts
against your application server 10.1.20.12 that they are seeing packets
being dropped. You ask them what the source IP address of the server is
they are running the load runner script from and they provide you with
10.1.17.225.

-  Why do you think their packets are being dropped? Hint: Check the
   blacklist (**Event Logs>>Network>>Shun**). They have been added to
   that list. You will now need to maintain the mitigations in place and
   only allow 10.1.17.225 to not be enforced with any DDoS mitigations
   going to 10.1.20.12.

-  Go to the protected object 10.1.20.12 and add the IP to the
   whitelist.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

# sudo bash

# cd f5agility

# ./lab2-7.sh

-  View the offending IP addresses at **Security>>Event
   Logs>>Network->IP Intelligence** and **Security>>Event
   Logs>>Network>>Shun** and confirm that 10.1.17.225 is not being added
   to the list\ **.**

-  You have successfully whitelisted an IP to bypass DDoS mitigations.
   Use CTRL+C in the attacker window to stop the attack.

Lab 2.9 – BOT Defense for Application Attacks.
----------------------------------------------

HTTP DoS attacks are very popular. Some can be in form of HTTP Floods
and some can be low and slow attacks (slow loris, slow post, slow read).
They have been used by BOTS to bring down a site. Sometimes even though
the BOTS don’t bring the site down they demand for you to stand up
additional infrastructure to support the traffic they are generating
costing your organization a significant spend when it can be mitigated
and avoided. Your organization just published a brand-new web
application. As soon as it was available to public you started getting
calls that the site is sometimes unavailable and slow to respond. Based
on the predicted traffic patterns one server was enough to handle the
valid user load. The application team viewed the web server logs and
noticed that there is 30% additional traffic then predicted from what
seems like automated tools. Your IT management has asked you to provide
a solution on what’s driving up the traffic to the server and
potentially mitigate it. You will now learn how to manually mitigate BOT
traffic.

-  Open a PuTTY shell to the WebServer (use the shortcut on the
   desktop). Login with credentials: root/default. You will use the
   webservers log to monitor the requests coming to the server. Once
   logged into the WebServer shell:

# cd /usr/local/apache/logs

# tail -f access\_log

-  Hit the Enter key a few times so that you can see incoming requests
   clearly in the blank space.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack to simulate BOT traffic:

# sudo bash

# cd f5agility

# ./lab2-9.sh

-  We are just simulating 25 requests so that it’s a controlled
   environment and you can view the requests/logs.

-  View the WebServer shell where you have the tail -f access\_log
   running. Do you see the requests come in? What’s the source IP
   address of the requests?

-  As you can see the site is available to everyone including BOTS. You
   have not set this up on the DHD and hence no BOT protection is
   applied.

-  You will now publish the website through the DHD with needed
   protections.

-  In the BIG-IP Configuration Utility, open the **DoS Protection >
   Quick Configuration** page and in the Protected Objects section click
   **Create**.

-  Configure a protected object using the following information, and
   then click **Create**.

+------------------------+---------------------------------+
| Name                   | WebServer                       |
+========================+=================================+
| IP Address             | 10.1.20.101                     |
+------------------------+---------------------------------+
| Port                   | 80                              |
+------------------------+---------------------------------+
| VLAN (Selected)        | **defaultVLAN (uncheck ANY)**   |
+------------------------+---------------------------------+
| Protection Settings:   | Log and Mitigate                |
| Action                 |                                 |
+------------------------+---------------------------------+
| Protection Settings:   | IPv4, TCP, HTTP                 |
| DDoS Settings          |                                 |
+------------------------+---------------------------------+

-  By simply creating the Protected Object and applying HTTP protections
   the BOT protections are automatically turned on. Everyone will now
   access the web application through the DHD with mitigations enforced.

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack to simulate BOT traffic:

# sudo bash

# cd f5agility

# ./lab2-9.sh

-  View the WebServer log (tail -f access\_log) in the shell. You will
   see not requests come through this time from the attacker.

-  View the mitigation in **Security>>Event Logs>>Bot
   Defense>>Requests.** All the requests from the BOT are blocked.

-  Open a firefox browser on the Jumpbox and go to http://10.1.20.101.
   This request will open your web application and its not blocked as
   it’s not a BOT. You will also see the request in the WebServer log
   shell.

-  View the valid request from your browser in the DHD in
   **Security>>Event Logs>>Bot Defense>>Requests.** You will notice that
   valid requests are being challenged and allowed only after a valid
   response. Note: There is a default grace period of 300s when the
   mitigation is implemented so some requests are allowed as grace. This
   is Proactive BOT defense in action.

-  View the BOT Defense in **Security>>Reporting>>DoS>>Analysis** and
   look at the graph under HTTP -> Transaction Outcomes. **Please be
   patient as these graphs are usually populated with a delay.**

   You have successfully mitigated BOT traffic to your application.
   CTRL+C in all shell windows and close them all.

Exercise 3 – Automatic Mitigations
==================================

Lab 3.1 – Auto Thresholding for Mitigating Attacks.
---------------------------------------------------

Your organization is about to launch a new marketing campaign and there
is a website that will host the content. You want to make sure that the
application is protected against DDoS attacks but are not sure what
traffic patterns are or what values to set for detections/rate
limits/mitigations. You will create a Protected Object for the marketing
website and use automatic mitigations.

-  In the BIG-IP Configuration Utility, open the **DoS Protection>>Quick
   Configuration** page and in the **Protected Objects** section click
   **Create**.

-  Configure a protected object using the following information, and
   then click **Create**.

+-------------------------+--------------------+
| Name                    | MarketingServer    |
+=========================+====================+
| IP Address              | 10.1.20.15         |
+-------------------------+--------------------+
| Port                    | \*                 |
+-------------------------+--------------------+
| Protocol                | All Protocols      |
+-------------------------+--------------------+
| Protection Settings:    | Log and Mitigate   |
| Action                  |                    |
+-------------------------+--------------------+
| Threshold Sensitivity   | High               |
+-------------------------+--------------------+
| Protection Settings:    | IPv4, TCP,         |
| DDoS Settings           |                    |
+-------------------------+--------------------+

Generate some good traffic to the marketing server.

-  Putty SSH (use the shortcut) to open a shell to the good client
   system.

-  Login as user: ubuntu. The session is preconfigured to authenticate
   with a certificate.

-  Start the auto-threshold baselining script with:

# sudo bash

# cd f5agility

# ./auto\_baseline.sh

Let this baseline traffic run for at least 10 minutes before proceeding
to the below step.

In our lab we need to roll back the device level protection so that it
doesn’t mitigate the stress we are generating for the auto-threshold on
the MarketingServer.

-  In the Configuration Utility, in the **Device Protection** section
   click **Device Configuration.**

-  In the **Flood** row click the + icon, and then click **ICMPv4**
   flood.

-  On the right-side of the page select the drop-down to
   **"Detect-Only"**

+-------------------------------+----------------+
| Mitigation                    | Fully Manual   |
+===============================+================+
| Detection Threshold EPS       | Infinite       |
+-------------------------------+----------------+
| Detection Threshold Percent   | 500            |
+-------------------------------+----------------+
| Rate/Leak Limit               | Infinite       |
+-------------------------------+----------------+

click **Update** at the bottom of the screen. This will allow our attack
to pass through to the automatic mitigation profile of the
MarketingServer that we are configuring below.

In the Hybrid Defender WebUI, for the **MarketingServer** Protected
Object configuration, enable auto-thresholding for the following
vectors: **ICMPv4 Flood, TCP SYN Flood, TCP Push Flood, TCP RST Flood,
TCP SYN ACK Flood** by selecting each vector and **clicking the “Fully
Automatic” Configuration radio button**. When all vectors are
configured, click **Update** at the bottom of the screen.

-  In the Hybrid Defender WebUI, view the Auto Threshold event log by
   navigation to **Security>>Event Logs>>DoS>>Network>>Auto Threshold**.

   The system is updating the detection thresholds. With
   auto-thresholding, the system adjusts the detection thresholds based
   on observed traffic patterns. However, mitigation rate limits are
   always dynamic based on detected system or protected object stress.
   If anomalous levels of traffic are running, but there is no stress,
   the Hybrid Defender will generate alerts but will not block traffic.
   Under stress, the rate limits are automatically created and adjusted
   dynamically.

   Generate some stress by launching an attack.

   Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack:

# sudo bash

# cd f5agility

# ./lab3-1.sh

Keep on refreshing the Auto Threshold event log (**Security>>Event
Logs>>DoS>>Network>>Auto Threshold)** and observe how the values are
changing dynamically. Even though our attack is ICMPv4 flood the other
vectors that are set to Fully Automatically are also being adjusted
dynamically.

View **Security>>DoS Protection>>DoS Overview.** Notice how automatic
detection and mitigation is happening as stress varies.

Stop all scripts and attacks using CTRL + C.

Lab 3.2 – Behavioral L4 for Mitigating Attacks
----------------------------------------------

In this lab you will use the Hybrid Defender’s network behavioral DoS
analysis capabilities and its ability to interpret behavioral history
and stress to automatically generate and enforce a precise, dynamic
signature. This capability allows the granular filtering of the good
from the bad, which is a major challenge in DoS mitigation. The bad must
be accurately identified to mitigate the DoS attack, particularly if the
attack changes over time. Enforcement of a very precise signature, with
enforcement thresholds based on system or network stress signals,
dramatically reduces false positives—increasing network and application
availability.

-  In the BIG-IP Configuration Utility, open the **DoS Protection >
   Quick Configuration** page and in the

-  In the **Protected Objects** section click **Create**.

-  Configure a protected object using the following information, and
   then click **Create.**

+------------------------+----------------------------+
| Name                   | BaDoSL4Server              |
+========================+============================+
| IP Address             | 10.1.20.13                 |
+------------------------+----------------------------+
| Port                   | \*                         |
+------------------------+----------------------------+
| Protocol               | All Protocols              |
+------------------------+----------------------------+
| Protection Settings:   | Log and Mitigate           |
| Action                 |                            |
+------------------------+----------------------------+
| Protection Settings:   | IPv4, TCP, L4 Behavioral   |
| DDoS Settings          |                            |
+------------------------+----------------------------+

-  In the **L4 Behavioral** row click the **+** icon.

-  Configure under Dynamic Signatures using the following information,
   and then click **Create**.

+--------------------------+-------------+
| Learn Only               | Unchecked   |
+==========================+=============+
| Mitigation Sensitivity   | High        |
+--------------------------+-------------+

-  Putty SSH (use the shortcut) to open a shell to the good client
   system.

-  Login as user: ubuntu. The session is preconfigured to authenticate
   with a certificate.

-  Start the behavioral L4 baselining script with:

# sudo bash

# cd f5agility

# ./baseline\_L4.sh

You can monitor the learning progress on the DHD.

-  Putty SSH (use the shortcut) to open **two shells** to the
   HybridDefender.

-  Login as user: root and password provided.

-  View the behavioral L4 baselining learning with following in
   1\ :sup:`st` shell. Notice the learning phase In Progress.

# cd f5agility

# ./show\_baseline\_L4\_status.sh

-  View the behavioral L4 baselining bins populating in 2nd shell.

# cd f5agility

# ./show\_baseline\_L4\_bins.sh

-  While the learning is happening, we need to turn off some manual
   mitigations at Device Level as they will block our attack that is
   going to create stress to trigger dynamic signatures.

-  In the Configuration Utility, in the **Device Protection** section
   click **Device Configuration.**

-  In the **Flood** row click the + icon, and then change click **TCP
   SYN Flood, TCP SYN Oversize** and change the attack vector to
   **“Detect-Only”**.

-  In the **Single Endpoint** row click the + icon, and then change
   click **Single Endpoint Sweep** and change the attack vector to
   **“Detect-Only”**.

Make sure the status is changed from “In Progress” to “Finished” for the
learning phase on the DHD before proceeding to the next steps below
(about 15 minutes)

-  Access the Attacker System CLI/shell and launch the attack:

# sudo bash

# cd f5agility

# ./lab3-2.sh

On the Hybrid Defender you will now see the attack is being
detected/mitigated. . Did you notice the dynamic signatures in DoS
Overview window? Give it a couple of minutes and it will show up. You
can view the signature **Security>>DoS Protection>>Signatures** under
Dynamic Signature section. Click on the “Network” (not the signature
hyperlink) to view details of the signature.

|image5|

Use CTRL+C in all shells - attacker, good traffic, DHD to stop all
scripts.

Lab 3.3 – Behavioral L7 for Mitigating Attacks
----------------------------------------------

In this lab you will use the Hybrid Defender’s application behavioral
DoS analysis capabilities and its ability to interpret behavioral
history and stress to automatically generate and enforce a precise,
dynamic signature. This capability allows the granular filtering of the
good from the bad, which is a major challenge in DoS mitigation. The bad
must be accurately identified to mitigate the DoS attack, particularly
if the attack changes over time. Enforcement of a very precise
signature, with enforcement thresholds based on system, network or
application stress signals, dramatically reduces false
positives—increasing network and application availability.

-  In the BIG-IP Configuration Utility, open the **DoS Protection >
   Quick Configuration** page and in the

-  In the **Protected Objects** section click **Create**.

-  Configure a protected object using the following information, and
   then click **Create.**

+------------------------+--------------------+
| Name                   | BaDoSL7Server      |
+========================+====================+
| IP Address             | 10.1.20.20         |
+------------------------+--------------------+
| Port                   | 80                 |
+------------------------+--------------------+
| Protocol               | TCP                |
+------------------------+--------------------+
| Protection Settings:   | Log and Mitigate   |
| Action                 |                    |
+------------------------+--------------------+
| Protection Settings:   | IPv4, TCP, HTTP    |
| DDoS Settings          |                    |
+------------------------+--------------------+

-  In the **HTTP** row click the **+** icon.

-  Click **Behavioral** and in the right pane configure using the
   following information.

+-------------------------------+-----------------------+
| Mitigation                    | Standard Protection   |
+===============================+=======================+
| Request Signature Detection   | Checked               |
+-------------------------------+-----------------------+

-  Click **Proactive Bot Defense** and in the right pane configure using
   the following information.

+-------------------+------------+
| Mitigate Action   | Disabled   |
+===================+============+
+-------------------+------------+

-  Click **DOS Tool** and in the right pane configure using the
   following information, and then click **Create**.

+-------------------+----------+
| Mitigate Action   | Report   |
+===================+==========+
+-------------------+----------+

Putty SSH (use the shortcut) to open **two shells** to the good client
system.

-  Login as user: ubuntu. The session is preconfigured to authenticate
   with a key.

-  Start the behavioral L7 baselining script in both shells with:

# sudo bash

# cd f5agility

# ./baseline\_L7.sh

Select 1) Increasing in first shell and 2) Alternate in the second
shell.

You will see a few 0000 statuses as there are certain bad requests in
the script. But majority of status is 200s.

You can monitor the learning progress on the DHD.

-  Putty SSH (use the shortcut) to open a shell to the HybridDefender.

-  Login as user: root and password provided.

-  View the behavioral L7 baseline learning with following. Notice the
   learning phase In Progress.

# cd f5agility

# ./show\_L7BaDoS\_learning.sh

-  The output is like this:

   *vs./Common/BaDoSL7Server+/Common/BaDoSL7Server.info.learning:[\ **62.0614**,
   6, 7061, 100] *

-  It will be 0.00 for a while (in above example output 62.0614 is the
   average approximation to the learned baselines)

-  For this demo, wait until you have reached at least 80.00-90.00
   (**the first number in the output**). This should happen after about
   8-10 minutes. Once you see 80.00 and above you can move to next
   steps.

-  The longer it runs, the better it is, because the system is
   self-adjusting permanently.

Make sure the status is “80.00-90.00” range (the first number in the
output) for the learning phase on the DHD before proceeding to the next
steps (about 10 minutes). Once you see 80.00 and above you can move on.

-  Hit CTRL+C in the DHD Shell and stop this learning status. We will
   now use this Shell window to see the dynamic signature that is
   generated.

-  Keep this shell window easily viewable. Behavioral L7 mitigation is
   very dynamic and hence based on the environmental conditions,
   underlying infrastructure for your lab instance some of you may see
   the Signature quickly appear and vanish, some may not see it and some
   will see it longer. Basically, the Signature mitigation is triggered
   and then by default the offending IP is added to Bad Actor/Shun list
   and the signature disappears if the system identifies it’s no longer
   needed for mitigation.

# ./show\_dos\_signature.sh

-  Access the Attacker System CLI/shell (use putty shortcut on Jumpbox)
   and launch the attack. Open **TWO** shells\ **.** In first
   shell\ **:**

# sudo bash

# cd f5agility

# ./lab3-3.sh

Choose 1) Attack Start – Similarity.

-  In Second shell\ **:**

# sudo bash

# cd f5agility

# ./lab3-3.sh

Choose 2) Attack Start – Score.

As soon as the attack is started you will see that your baseline traffic
status of 200s in the good client is now suddenly going to 0000. Wait
for a couple of minutes till it returns to a lot more 200s. (Keep the
eye on the DHD Shell for Signature)

On the Hybrid Defender Shell you will now see the attack is being
mitigated and a signature may appear (see note above).

View Bot Defense logs. **Security>>Event Logs>>Bot Defense>>Requests. **

View Bad Actor Log/Blacklist and notice the offending IP is added to the
list. **Security>>Event Logs>>Network>>Shun. **

Use CTRL+C in all open shell windows (Attacker, Good Client, Hybrid
Defender) to STOP all traffic and scripts. Close out all Windows.

    END OF LABS!


.. |image0| image:: media/image3.png
   :width: 2.99000in
   :height: 3.46000in
.. |image1| image:: media/image4.png
   :width: 2.92708in
   :height: 2.92708in
.. |image2| image:: media/image5.png
   :width: 5.30972in
   :height: 2.39444in
.. |image3| image:: media/image6.png
   :width: 5.26736in
   :height: 0.69235in
.. |image4| image:: media/image7.png
   :width: 5.30972in
   :height: 1.70139in
.. |image5| image:: media/image8.png
   :width: 5.11111in
   :height: 1.85169in
.. |image10| image:: media/image10.png
   :width: 5.11111in
   :height: 5.85169in

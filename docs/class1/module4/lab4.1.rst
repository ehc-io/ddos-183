Lab – Configure Application Layer DoS Defenses
----------------------------------------------

    Use a protected object to mitigate L7 DoS attacks.

Create a Protected Object
~~~~~~~~~~~~~~~~~~~~~~~~~

    #. In the BIG-IP Configuration Utility, open the DoS Protection > Quick Configuration page and in the **Protected Objects** section click **Create**.

    #. Configure a protected object using the following information, and then click **Create**.

        =============================   ======================
        Name:                           Server1-https           
        IP Address:                     10.1.20.11/32   
        Port:                           443
        VLAN:                           defaultVLAN 
        SSL:                            Enabled
        SSL Certificate                 default
        Key                             default
        Encrypt Connection to Server    Yes (selected)
        Protec. Settings Action:        Log and Mitigate 
        Protec. Settings Silverline:    Yes (selected) 
        Protec. Settings DDoS:          IPv4, TCP, HTTPS
        =============================   ======================

    #. In the **Protected Objects** section click **Create**.

    #. Configure another **Protected Object** using the following information, and then click **Create**.

        =============================   ======================
        Name:                           Server2-http          
        IP Address:                     10.1.20.12/32   
        Port:                           *
        VLAN:                           defaultVLAN            
        Protec. Settings Action:        Log and Mitigate 
        Protec. Settings Silverline:    Yes (selected) 
        Protec. Settings DDoS:          IPv4, TCP
        =============================   ======================

        .. NOTE:: 
            This protected object will be used for the L7 behavioral DoS exercise. You’ll be using this protected object to create a traffic baseline which takes a minimum of 15 minutes.

    #. In the HTTP row click the **+** icon, and then click **Behavioral**, then from the **Mitigation** list select **Standard Protection**.

    #. In the HTTP section click **Proactive Bot Defense**, then from the **Mitigate Action** list select **Disabled**.

        .. NOTE:: 
            Both the good traffic and the attack traffic are generated with simple tools that would be blocked by Proactive Bot Defense. Please note that, by default, the Hybrid Defender will set Proactive Bot Defense to **always**'.

    #. In the HTTPS section click **DoS Tool**, then from the **Mitigate Action** list select **Report**, and then click **Create**.

        .. NOTE:: 
            The lab uses apache bench for the attack traffic which is categorized as a malicious bot tool. If left at a 'block' posture the system will mitigate the attack with the known signature of this tool rather than the behavioral mitigations.

    #. Open the Security > DoS Protection > DoS Profiles page and click **Server2-http**.

        |image34|

    #. Open the **Application Security** page.

        |image35|
    
    #. Click **Behavioral & Stress-based Detection**, and then for **Behavioral Detection and Mitigation** click **Edit**.
    
    #. Select the **Request signatures detection** checkbox, and then click **Update**.

        |image36|

Generate L7 Behavioral baseline for Server2-http
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    Use a script to generate an L7 behavioral DoS baseline on the BIG-IP DHD..

    #. On the goodclient terminal session, type (or copy and paste) the following command:

        ``cd ~/tools_agility_183/``  

        ``sudo ./generate_clean_traffic.sh``      

    .. NOTE:: 
        This will generate traffic. Please note that it will take at least 15 minutes before you will see the
        **Baseline** in Process message change to **Baseline Established*. This defines the minimum amount of time for the behavioral engine to baseline the good traffic.

    .. code::

        f5student@goodclient:~/tools_agility_183$ ./generate_clean_traffic.sh
        welcome.php	status: 200	bytes: 1045	time: 0.017
        welcome.php	status: 200	bytes: 1045	time: 0.014
        welcome.php	status: 200	bytes: 1045	time: 0.014
        welcome.php	status: 200	bytes: 1045	time: 0.015
        headers.php	status: 200	bytes: 1847	time: 0.015
        headers.php	status: 200	bytes: 1847	time: 0.014
        headers.php	status: 200	bytes: 1847	time: 0.014
        headers.php	status: 200	bytes: 1847	time: 0.014
        bigtext.html	status: 200	bytes: 634965	time: 0.081
        bigtext.html	status: 200	bytes: 634965	time: 0.084
        bigtext.html	status: 200	bytes: 634965	time: 0.076
        bigtext.html	status: 200	bytes: 634965	time: 0.134
        httprequest.php	status: 200	bytes: 710	time: 0.013
        httprequest.php	status: 200	bytes: 710	time: 0.014
        httprequest.php	status: 200	bytes: 710	time: 0.014
        httprequest.php	status: 200	bytes: 710	time: 0.013
        badlinks.html	status: 200	bytes: 1270	time: 0.014
        badlinks.html	status: 200	bytes: 1270	time: 0.013
        badlinks.html	status: 200	bytes: 1270	time: 0.014
        badlinks.html	status: 200	bytes: 1270	time: 0.014
        F5_building.jpg	status: 200	bytes: 33447	time: 0.019
        F5_building.jpg	status: 200	bytes: 33447	time: 0.021
        bigip4200.jpg	status: 200	bytes: 9753	time: 0.016
        bigip4200.jpg	status: 200	bytes: 9753	time: 0.017
        viprion2400.jpg	status: 200	bytes: 13009	time: 0.016
        viprion2400.jpg	status: 200	bytes: 13009	time: 0.016
        viprion4800.jpg	status: 200	bytes: 10078	time: 0.018
        viprion4800.jpg	status: 200	bytes: 10078	time: 0.017

    .. NOTE:: 
        When the sytem is healthy, the web server must return **HTTP Status Code** must be **200** for every request.

    #. Move on in the exercises while the baseline is being generated.

    #. Open a terminal session with the DHD and run the following command:

        ``admd -s vs./Common/Server2-http.sig.health`` 

    .. code::

        [root@dhd-ehc:Active:Standalone] scripts # admd -s vs./Common/Server2-http.sig.health
        vs./Common/Server2-http.sig.health:[0.452373]
        vs./Common/Server2-http.sig.health:[0.453407]
        vs./Common/Server2-http.sig.health:[0.451726]
        vs./Common/Server2-http.sig.health:[0.45372]
        vs./Common/Server2-http.sig.health:[0.452021]
        vs./Common/Server2-http.sig.health:[0.45349]

    .. IMPORTANT::
            The results for each health check should **not** be 0.5. Let those terminal sessions opened for the rest of this lab.
        
Configure DoS Protection for L7 Encrypted Traffic
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    Launch a Slowloris attack to the back-end web server and view the results, and then add mitigation on the BIG-IP DHD.

    #. First run the test script on server2.f5demo.com as follows:

         `` cd ~/tools_agility_183/ ; ./server2_http_test.sh``    

    #. Before launching the application layer attack, observe the server2 is currently healthy.

        .. code::

            welcome.php     status: 200	bytes: 1045     time: 0.018
            bigtext.html    status: 200	bytes: 634965   time: 0.136
            httprequest.php status: 200	bytes: 710      time: 0.017

        .. NOTE:: 
            The system is healthy since the web server returns **HTTP Status Code 200** for every request.

    #. Now from the attacker terminal session run the following command:

        ``./slowloris.sh`` 

        .. code::

            Mon Aug 13 11:26:54 2018:
            slowhttptest version 1.6
            - https://code.google.com/p/slowhttptest/ -
            test type:                        SLOW HEADERS
            number of connections:            4090
            URL:                              https://server2.f5demo.com/
            verb:                             GET
            Content-Length header value:      4096
            follow up data max size:          68
            interval between follow up data:  10 seconds
            connections per seconds:          200
            probe connection timeout:         5 seconds
            test duration:                    240 seconds
            using proxy:                      no proxy

            Mon Aug 13 11:26:54 2018:
            slow HTTP test status on 30th second:
            initializing:        0
            pending:             1790
            connected:           150
            error:               0
            closed:              2092
            service available:   **NO**

    #. Now observe how the service is impacted as the slowloris attack hits the server2.f5demo.com.

        .. code::

            welcome.php     status: 000	bytes: 0    time: 1.002
            bigtext.html    status: 000	bytes: 0    time: 1.002
            httprequest.php status: 000	bytes: 0    time: 1.002

        .. NOTE:: 
            Since the slowloris attack is being encrypted (https://server2.f5demo.com) we need to setup the certificate and private keys so the traffic can be inspected then mitigated by the Hybrid Defender..

    #. Configure SSL on the protected object to in order to inspect HTTPS traffic.
    
    #. Go to DDoS Protection > Quick Configuration > Protected Objects, then click **Server2-http**. Configure the SSL as follows:

        =============================    ===============
        Port:                            443
        SSL:                             Enabled
        SSL Certificate:                 default
        Key:                             default
        Encrypt Connection to Server:    Yes (selected)   
        =============================    ===============
    
    #.  Disable bot protections so the scripts can be used for testing the server health.

    #. On the Server2-http Protected Object section go to the HTTP ro, click the **+** icon, and then click **Behavioral**, then from the **Mitigation** list select **Standard Protection**.

    #. In the HTTPS section click **Proactive Bot Defense**, then from the **Mitigate Action** list select **Disabled**.

    #. Now that SSL is also being inspected for this Protected Object, let's run the slowloris script once again and verify if the attack still works.


.. |image34| image:: /_static/image034.png
.. |image35| image:: /_static/image035.png
.. |image36| image:: /_static/image036.png
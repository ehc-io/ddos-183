Hybrid Defender Setup
------------------------

Getting Started
================

**Accessing the lab environment.**

#. Open a browser and go to http://training.f5agility.com.  Enter your Class# and Student# as provided by your instructor.

#. Look for the jumphost virtual machine. You will use the xubuntu jumphost for all the labs. (see below)

   .. HINT::

      You will be using the Ubuntu jumpbox to access other systems for all labs. You will use either the PUTTY client or native bash shell prompt which has been preconfigured with appropriate keys to access the Good Client and the Attacker systems. A few scripts require root access. Don't forget to ‘sudo bash’ before running attacks, baselines, etc.

  |image1| 

Lab Components
~~~~~~~~~~~~~~

**Lab Diagram**

      |image2|

.. NOTE::
   You may have noticed that although clients (goodclient, Kali) and server (LAMP) are siting at the same network subnet [10.1.20.0/24], they're in different VLANs actually (internal - ID 20 vs external - ID 10). Those two VLANs will be grouped toghether (VLAN Group) and act like a single Layer-2 broadcast domain.

The lab environment provides the following resources:

- 1 x F5 BIG-IP DHD (v13.1.2)
- 1 x Linux Kali **[Attacker]** 
- 3 x Linux Ubuntu 14.04.5 LTS [**Jumphost**, **LAMP server**, **goodclient**]

Networking Info
~~~~~~~~~~~~~~~

IP addressing, Out of Band management, and credentials for all components:

  .. list-table::
      :widths: 20 40 40
      :header-rows: 1
      :stub-columns: 1

      * - **Component**
        - **VLAN/IP Address(es)**
        - **Credentials**
      * - Jumphost
        - - **Management:** 10.1.0.51
          - **internal:** 10.1.20.51
        - ``f5student``/``f5student``

      * - kali
        - - **Management:** 10.1.0.52
          - **internal:** 10.1.20.52
        - ``f5student``/``f5student``

      * - goodclient
        - - **Management:** 10.1.0.53
          - **internal:** 10.1.20.53
        - ``f5student``/``f5student``

      * - Lamp
        - - **Management:** 10.1.0.252
          - **internal:** 10.1.20.252
        - ``f5``/``f5student``

      * - F5-DHD
        - - **Management:** 10.1.0.244
          - **internal:** 10.1.20.244
        - ``root``/``root``

Re License your DHD Device
==========================

.. IMPORTANT::
  For Silverline device registration to function properly the Hybrid Defender device must have a unique device ID, which is comprised of unique attributes like Base MAC and registration key.

For the following steps please use the registration key provided by your instructor.

  #. Go to System->License and then click on Re-activate.  

      |image3|

  #. Edit the Base Registration key, replacing it by the new license key. The Activation method option must be manual. Then click **Next**.  

      |image4|

  #. Select all in the Dossier frame and copy it.  Click on “Click here to access F5 Licensing Server”.  

     |image5|

  #. Agreee with the contract terms, copy the contents in the license frame, then click **Next** ”.  

     |image6|

  #. Go back to your F5 DHD and paste the contents copied from the above. License and click **Next**.  

     |image7|

  .. Hint::
    The BIG-IP will restart daemons and a window will pop up indicating system configuration has changed.  Please wait for it to reconnect and click **Continue**. Your device is now licensed.  Click **Next**


Register DHD Device with Silverline
====================================

For Silverline signaling we will be leveraging both the DHD built-in signaling, as well as bandwidth utilization reporting for Hybrid DDoS protection.  

  #. Go to System->Platform menu and change the hostname replacing the digits part with your student number, as the example bellow. This will make easier to identify alerts from your particular device in the Silvrline Portal. When finished, click **Update**.  

      |image8|

  #. In Device Management->Devices select the device and then click “Change Device Name”.  

      |image9|

  #. Update the device name to match the hostname you have chosen. **Important**: Use your student number.

      |image11|

  #. From the Hybrid Defender shell, restart services with:  
 
      ``# bigstart restart``  

  #. Now proceed with the Silverline registration going to the DoS Protection->Quick Configuration->Silverline menu as follows:  

    - Provide proper credentials:  

      ===========   =============================
      username      dhdadmin@signaling.com        
      password      [will be provided]               
      Service URL   https://api.f5silverline.com  
      ===========   =============================

      |image12|

    - Access the Silverline Portal https://portal.f5silverline.com  using same DHD admin account  

    - Navigate to Config->Hybrid Config->Hybrid Device Management  

    - Enter your email prefix in the Search field  

    - Verify that you have both registrations. Approve both  

      |image13|

.. |image1| image:: /_static/dashboard_student.png
.. |image2| image:: /_static/lab_network_topology.png
.. |image3| image:: /_static/image003.png
.. |image4| image:: /_static/image004.png
.. |image5| image:: /_static/image005.png
.. |image6| image:: /_static/image006.png
.. |image7| image:: /_static/image007.png
.. |image8| image:: /_static/image008.png
.. |image9| image:: /_static/image009.png
.. |image10| image:: /_static/image010.png
.. |image11| image:: /_static/image011.png
.. |image12| image:: /_static/image012.png
.. |image13| image:: /_static/image013.png
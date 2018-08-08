Lab Setup
---------

Getting Started
================

**Accessing the lab environment.**

#. Open a browser and go to http://training.f5agility.com.  Enter your Class# and Student# as provided by your instructor.

#. Look for the jumphost virtual machine. You will use the xubuntu jumphost for all the labs. (see below)

   .. HINT::

      You will be using the Ubuntu jumpbox to access other systems for all labs. You will use either the PUTTY client or native bash shell prompt which has been preconfigured with appropriate keys to access the Good Client and the Attacker systems. A few scripts require root access. Don't forget to ‘sudo bash’ before running attacks, baselines, etc.

  |dashboard| 

Lab Components
===============

**Lab Diagram**

      |topology|

.. NOTE::
   You may have noticed that although clients (goodclient, Kali) and server (LAMP) are siting at the same network subnet [10.1.20.0/24], they're in different VLANs actually (internal - ID 20 vs external - ID 10). Those two VLANs will be grouped toghether (VLAN Group) and act like a single Layer-2 broadcast domain.

The lab environment provides the following resources:

- 1 x F5 BIG-IP DHD (v13.1.2)
- 1 x Linux Kali **[Attacker]** 
- 3 x Linux Ubuntu 14.04.5 LTS [**Jumphost**, **LAMP server**, **goodclient**]

Networking Info
===============

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

Register DHD Device with Silverline
====================================

For Silverline signaling we will be leveraging both the DHD built-in signaling,
as well as bandwidth utilization reporting for Hybrid DDoS protection.  

#. Go to System->Platform menu and change the hostname replacing the digits part with your student number, as the example bellow. This will make easier to identify alerts from your particular device in the Silvrline Portal.

    **dhd-77.latam.f5demo.com**

#. In Device Management->Devices select the device and then click “Change Device Name”. 

#. Update the device name to match the hostname you have chosen.  

#. From the Hybrid Defender shell, restart services with:

    ``# bigstart restart``

#. Register the device with Silverline in DoS Protection->Quick Configuration->Silverline as follows:

    ===========   =============================
    Username      dhdadmin@signaling.com        
    Password      [will be provided]               
    Service URL   https://api.f5silverline.com  
    ===========   =============================

#. Register the device with the Silverline iApp, to provide bandwith utilization updates in iApps->Application Services->Applications->silverline_connector. In the iApp, select Reconfigure and then click Finished. This will cause the iApp to register under the new device name.  

#. Approve the device registrations  

#. Access the Silverline Portal DHD Admin Account:  
    - Navigate to Config->Hybrid Config->Hybrid Device Management  
    - Enter your email prefix in the Search field  
    - Verify that you have both registrations. Approve both  

.. |dashboard| image:: /_static/dashboard_student.png
.. |topology| image:: /_static/lab_network_topology.png


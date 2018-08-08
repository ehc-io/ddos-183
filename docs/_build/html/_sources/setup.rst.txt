Lab Setup
---------

Getting Started
================

**Accessing the lab environment.**

#. Open a browser and go to http://training.f5agility.com.  Enter your Class# and Student# as provided by your instructor.

#. Look for the xubuntu-jumpbox.  You will use the xubuntu jumpbox for all the labs. (see below)

   .. HINT::

      You will be using the Ubuntu xjumpbox to access other systems for all labs. You will use Putty that has been preconfigured with appropriate keys to access the Good Client and the Attacker systems. To run scripts, you will need to have root access, requiring you to ‘sudo bash’ before running attacks, baselines, etc
      
   .. image:: /_static/dashboard_student.png

Lab Components
===============

**Lab Diagram**

   .. image:: /_static/diagram.png

The following components have been included in your lab environment:

- 1 x F5 BIG-IP DHD (v13.1.2)
- 1 x Linux kali 
- 3 x Linux Ubuntu 14.04.5 LTS [Jumphost, LAMP server, goodlclient]

The following table lists VLANS, IP Addresses and Credentials for all
components:

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

    * - kali[Hacker]
      - - **Management:** 10.1.0.52
        - **internal:** 10.1.20.52
      - ``f5student``/``f5student``

    * - goodclient
      - - **Management:** 10.1.0.53
        - **internal:** 10.1.20.53
      - ``f5student``/``f5student``
  
    * - Lamp[victim]
      - - **Management:** 10.1.0.252
        - **internal:** 10.1.20.252
      - ``f5``/``f5student``
  
    * - F5-DHD[defense]
      - - **Management:** 10.1.0.244
        - **internal:** 10.1.20.244
      - ``root``/``root``

Register DHD Device with Silverline
====================================

For Silverline signaling we will be leveraging both the DHD built-in signaling,  
as well as bandwidth utilization reporting for Hybrid DDoS protection.


#. In System->Platform change the hostname to <yourf5emailprefix>.f5demo.com. For example j.smith@f5.com would register as j.smith.f5demo.com. If you have more than one DHD environment registered to Silverline, add a number to make each DHD unique.  

    **dhd-[student#].latam.f5demo.com**

#. In Device Management->Devices select the device and then click “Change Device Name…”. 

#. Update the device name to match the hostname you have chosen.  

    **image-02**

#. From the Hybrid Defender shell, restart services with:

    ``# bigstart restart``

#. Register the device with Silverline in DoS Protection->Quick Configuration->Silverline:

#. Register the device with the Silverline iApp, to provide bandwith utilization updates in iApps->Application Services->Applications->silverline_connector. In the iApp, select Reconfigure and then click Finished. This will cause the iApp to register under the new device name.  

#. Approve the device registrations  

#. Access the Silverline Portal DHD Admin Account:  
    - Navigate to Config->Hybrid Config->Hybrid Device Management  
    - Enter your email prefix in the Search field  
    - Verify that you have both registrations. Approve both  

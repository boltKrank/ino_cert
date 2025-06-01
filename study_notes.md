# INO Study

## Sections

1. NIOS DDI Introduction
2. NIOS DDI DHCP Operations
3. NIOS DDI DNS Operations
4. NIOS DDI IPAM Operations


### 1.NIOS DDI Introduction

#### Introducting NIOS


**TEXT:**

NIOS is the operating system that powers many Infoblox appliances.
NIOS stands for Network Identity Operating System.
NIOS is a security-hardened operating system.
It operates on a heavily customized Linux-based OS.
Depending on the version of NIOS in use, it could function as a container within a hardened operating system.
Regardless of the version, security is a fundamental component of NIOS, integrated at the operating system level from its inception.
In addition to its security features, NIOS also delivers core DDI services by providing dedicated software modules, such as DHCP and DNS servers.
It also allows each NIOS-enabled device to link to form a centrally managed Grid.
With a strong focus on security, NIOS imposes many restrictions.
It does not expose extraneous network ports, run unnecessary system services, provide superuser or root access, or allow users to install additional software or execute custom scripts.
Despite the restrictions, NIOS provides industry-standard extension capabilities, such as logging system events via syslog, monitoring via the SNMP protocol, and API access for customization and further integration.

**POINTS:**

- NIOS is "Network Identity Operating System"
- It is based on a highly customised version of Ubuntu
- Depening on NIOS version, some functions run as containers
- Security is the number one priority, and is enforced from the OS up
- Other point



#### Lab notes:

#### Defining a NIOS Grid

**TEXT:**

Defining a NIOS Grid.
A NIOS Grid is created when you link two or more NIOS appliances to be managed from a single, secure access point.

The NIOS Grid is not an additional tool for managing and reporting on these devices.

Instead, it's an essential part of every NIOS device, allowing secure control of each device on the Grid.
The NIOS Grid addresses a common issue in enterprises where servers or devices are set up and maintained separately.

These standalone devices cannot ensure data accuracy or availability, and they also increase management overhead.
By integrating these devices together, the NIOS Grid improves service reliability and efficiency, and reduces operating costs.

In other words, the Grid allows you to make configuration changes, view detailed reports, and perform system operations such as code upgrades.

From a single location, you can access all of these on every device connected to the Grid.

You may be familiar with other management technologies that take a while to reflect changes on the devices they manage.

However, that's not the case with the Grid!

The NIOS Grid uses a proprietary database that sends the right data to the right device, known as a member, in near real-time.

With the NIOS Grid, you can even apply patches and perform system upgrades with zero down-time!

Let's look at some of the components of a NIOS Grid.
A typical NIOS Grid has a Grid Master, an optional Grid Master Candidate, and Grid Members.

They communicate with each other using Grid Communication, which is an encrypted VPN connection.
We will learn more about Grid Members and Grid Communication in other lessons.

**POINTS:**

- A
- B
- C

#### Outlining the Roles of NIOS Grid Members

**TEXT:**

Outlining the Roles of NIOS Grid Members.

A Grid member is a single logical component of the Grid.
It can be a physical appliance, a virtual machine, or a cloud instance.

Furthermore, a member can be made highly available by combining two devices or two virtual machines to form a pair or cluster.

This is known as a high-availability pair, or HA pair.
When integrated into the Grid, the pair appears as a single member.

In order to join the Grid, the device must operate on a supported version of NIOS and have the appropriate licenses and access information.

Upon joining a Grid, the device will automatically adjust its NIOS version to align with the other grid members.
We will explore this topic further in upcoming lessons at the 3000 level.

The course 'NIOS DDI Grid Deployment' provides information on deploying a Grid.

On a NIOS Grid, a Grid Master is always present.
In fact, every NIOS device initially operates as its own Master, creating a single-member Grid until it connects with another device.

Afterwards, one device is configured as the Grid Master.
Within the user interface, a 4-green diamond icon is displayed next to the member acting as the Grid Master.
In pictures and diagrams, the Grid Master, or GM, is depicted with a gold crown.

The Grid Master holds the entire Grid database and continuously synchronizes data with the Grid Master Candidate.

A Grid Master Candidate, or GMC, is optional; it may not exist on every NIOS Grid.

If a Grid Master Candidate is on the Grid, it will be signified by the half-white-half-green diamond beside it.
In pictures and diagrams, the GMC is portrayed with a silver crown.

In a single Grid, it's possible to have several Grid Master Candidates.

It is designed to be an emergency or disaster recovery component.

For example, if the Grid Master were to fail, one of the Grid Master Candidates could be manually selected to replace the failed Grid Master.

Any device that is not a Grid Master or a Master Candidate is a regular Grid Member or simply a Member.
This is the most common component seen on a Grid, represented by an icon of four blue diamonds.

In diagrams and pictures, Grid Members do not have distinct markings.

Remember that a Member may be represented by a single device or a double-stacked device, indicating that it is an HA pair.

Consider the Grid Master as the manager, and the Grid Members are the workers.

The configuration changes are set on the Grid Master, while the Grid Members provide DNS, DHCP, and other core network services to the users.

Typically, the Grid Master and Grid Master Candidates only perform management functions, and all other services are disabled.

However, designs can vary.

There are other specialized Grid Member roles that may require specific hardware models or software licenses, such as Reporting, Network Insight, and Cloud Network Automation.

The Reporting and Analytics Member, or the Reporting Member, collects data from every member of the Grid, stores it in a separate reporting database, and generates reports.

With the Reporting Member, users will get additional statistical information, such as performance metrics and trending data.

Network Insight provides network management functionality.

The member with this feature is known as the Discovery Member.

It allows administrators to discover and control network devices from the Grid, allowing them to have better visibility of the network infrastructure, including virtual network infrastructure (VRF) and switch port control.

Cloud Network Automation incorporates data from the cloud management platform into the Grid's IPAM data.

For example, if a user has many devices in their Amazon and Google cloud environment, with a Cloud Member, they can synchronize that information to the Grid's database to get a complete picture of their IP address usage in the cloud.

**POINTS:**

- A
- B
- C

#### Navigating Grid Communication Between Grid Members

**TEXT:**

Navigating Grid Communication Between Grid Members.
NIOS Grid communication is an encrypted VPN tunnel among the members.

Each member of the Grid establishes a VPN tunnel with the Grid Master, and all of those VPN tunnels are known as Grid Communication.

Here is a simplified setup on screen, just between the Grid Master and a Grid Member.

They connect via a network switch.

The VPN tunnel, which has two network ports, serves as the communication channel between these two devices.
The first network port is UDP 2114, or two-one-one-four, a fixed, non-configurable setting.

When a device wants to join the Grid, it initiates communication over port 2114 to perform a key exchange.
Essentially, the device is saying to the Grid Master, "Hey, I want to join you and become a Member of your Grid. Here's some access information; please let me join."

Once the Grid Master has validated the access information provided, it will generate a unique strong encryption key and notify the joining device to switch communication to the second network port to continue the conversation.
After an encryption key has been generated, both the Grid Member and the Grid Master switch to a second, configurable network port.

By default, this is UDP 1194, or one-one-nine-four.
You may choose a different port number for this communication, if you wish.

Over this fully secure channel, the Grid Master can now push software updates or configuration changes to the newly joined Grid Member.

These two network ports are crucial for the NIOS Grid to function.

Both ports must be allowed bi-directionally between the Grid Master and every other member of the Grid.
If there is a Grid Master Candidate in the design, it will be necessary to ensure that these ports are also allowed from your Grid Master Candidate to other members.
In case the Grid Master is unavailable, the Grid Master Candidate can still communicate with all the members.
As a natural follow up question to this, you may ask:
What other ports does NIOS use besides 2114 and 1194?
That requires a more extensive answer.

For the complete answer, you need to visit the documentation portal at docs.infoblox.com, and search for the key phrase "Configuring Ethernet Ports."
Once you are on the right page, scroll down until you see the colorized tables.

Each color is significant, which we will discuss in a minute.

The first table shows ports usage when you are not using the optional LAN2 interface.

You should read this from top to bottom, and then left to right.

For example, if you are trying to figure out what ports are used by a single Grid Master, you would start from the top of the table, then go down 1, 2, 3, to locate the row that begins with 'Single Grid Master'.

Then you can go from left to right, to make sure this row matches all of your configuration.

If you encounter a field in the row that does not match your configuration, then you go down further until you find the right one.

For example, let's say your single Grid Master has the MGMT (management) interface enabled.

Row 3 of this table does not match that.

Thus, you would go down to row 12, where it is the Single Grid Master with the MGMT port enabled.

Once you have found the correct row, you can read each colorized field to know what services are bound to which network interfaces or ports.

For example, in this row, you can see that the Database Synchronization service (color yellow) is bound to the network interface LAN1, and the Core Network Services (color green) are bound to LAN1, LAN2, and/or MGMT.
When you see multiple interfaces listed, this means they are configurable.

Somewhere in the user interface, there must be a checkbox or button for you to choose which interface you would like to provide the service on.

You might be asking: What exactly is Data Synchronization?

Or what are Core Network Services?

At this point, the color of these fields becomes crucial for uncovering more details.

Just below the table you're currently viewing, there's another table that lists details about each color or category of services.

If you want to know about the Data Synchronization that is running over the LAN1 interface, this table provides the details of the source and destination addresses and ports involved.

For example, we can see here that Data Synchronization uses UDP 2114 for both member connection key exchange and GMC promotion, as well as UDP 1194 for accounting.

This table is useful if you need to assist your network or security team in constructing a comprehensive allowlist for all Grid Communications and services.

**POINTS:**

- A
- B
- C

#### Examining the role of the Database

**TEXT:**

Examining the Role of the Grid Database.

The Infoblox Grid works thanks to the magic of the Grid Database.

The Grid Database is a purpose-built, XML-based database.
It is not a relational database, which is used by many other products.

The advantages of the Grid Database are that it is zero maintenance, object-oriented, and fault tolerant against transactional failures.

Like other security-hardened features of NIOS, the Grid Database does not allow direct access, administrators can only interact with the database via the Grid Manager UI or the provided Infoblox API.

The Grid Database uses a partitioned database structure, which is a fancy way of saying that there are many slices of the database, and each member only gets the slice it needs to do its job.

Only the Grid Master has the entire database or all the slices.

The only exception is when there is one or more Grid Master Candidates on the Grid.

The candidates receive full synchronization from the Grid Master for the entire database.

So, what is stored in this database?

In short, everything except NIOS itself.

Remember, NIOS is the operating system that powers the devices.

Everything else, from network settings, licensing information, user logins, DNS entries, to DHCP networks, are all stored in the Grid Database.

This means that if you ever need to rebuild the Grid, all you need is a copy of NIOS, which you can download from the Infoblox web site, and the latest copy of your database backup.

As a general rule, you can assume that the Infoblox Grid and Database relationship is: "One Grid, One Database," which means you do not backup each member separately.
When you take a Grid backup, that backup contains the entire database, which has information for every member on the Grid.

In other words, it's not just "One Grid, One Database"; but also "One Grid, One Backup".

**POINTS:**

- A
- B
- C

**LAB NOTES:**


#### Supplimentary Notes

#### What is NIOS?

- NIOS stands for **Network Identity Operating System**, which serves as the core OS for Infoblox appliances. It is a highly customized, Linux-based operating system specifically designed to handle network identity management and other essential network services.

- **Restricted Access:** NIOS limits access to ensure security. It disables unnecessary ports and services and prevents superuser or root access, meaning administrators are limited in making direct changes to the OS.

- **Extension Capabilities:** Despite restrictions, NIOS supports industry-standard extension tools like:
	- **Syslog** for logging system events.
	- **SNMP (Simple Network Management Protocol)** for monitoring device status.
	- **API Access** for custom integrations, enabling administrators to build solutions that extend NIOS functionalities.

	
#### 1.2 What is a NIOS Grid?

- The NIOS Grid links multiple NIOS appliances to form a secure, centrally managed network, allowing all devices to work together as a single system. A NIOS Grid enables a streamlined and secure way to manage multiple devices.

- A typical NIOS Grid setup includes:

	- **Grid Master (GM):** The central controller that holds and manages the Grid's database.
		
	- **Grid Master Candidate (GMC):** A secondary device that serves as a backup for the GM, available for disaster recovery.
		
	- **Grid Members:** Individual devices within the Grid that provide essential network services, such as DNS and DHCP. Members can be deployed as single units or as high-availability pairs for redundancy.
		
		
#### Types of Grid Members

- **Standard Grid Members**

	- **Core Functionality:** Grid Members are primarily responsible for providing services like DNS, DHCP, and IP address management to users. They synchronize with the Grid Master to ensure up-to-date configurations.

	- High-Availability (HA) Pairs: Grid Members can be set up as HA pairs, where two devices work together as a cluster to provide failover support. This setup helps ensure continuous availability even if one device fails.

**Specialized Members:**

- **Reporting Member:** Collects data from all Grid devices and stores it in a reporting database, enabling analytics on performance metrics and trends. This role helps in producing reports that provide valuable insights for network management.

- **Discovery Member (Network Insight):** Enhances visibility by discovering and managing network devices. This role provides a view into network infrastructure, including virtual environments (VRF) and switch port controls.
 
- **Cloud Network Automation Member:** This member integrates cloud management platform data into the Grid, updating the Grid’s IP Address Management (IPAM) data. It helps organizations synchronize IP usage data from cloud providers like Amazon and Google, ensuring accurate IP allocation.


#### Grid Communication
- **Encrypted VPN Tunnels**
	- Each Grid Member communicates with the Grid Master over an **encrypted VPN tunnel.** This secure tunnel ensures data is safely exchanged, especially important for sensitive network configuration data.
- Communication Ports
	- **Primary ports** include:
	- **UDP 2114:** Used for initial device communication and key exchange when a new device joins the Grid.
	- **UDP 1194:** A configurable port used for ongoing secure communication once a device is authenticated.

- These ports must be allowed bi-directionally between the Grid Master, Grid Master Candidate, and all Grid Members to ensure seamless communication and management capabilities across the Grid.

- **Joining the Grid**
	- When a device requests to join the Grid, it initiates communication on port 2114 to perform a key exchange. Once the Grid Master validates this request, the device switches to port 1194, securing the connection. This allows the Grid Master to push software updates and configuration changes across the Grid securely.


#### The Grid Database

**Structure and Purpose**

The Grid Database is a XML-based database that operates on an object-oriented model, which is easier to manage and more resilient than traditional relational databases.
The database is fault-tolerant and partitioned. Each Grid Member only accesses the data it requires to perform its role.

**Data Management and Security**

Administrators can only access the database through the Grid Manager UI or Infoblox API, ensuring that no direct modifications are made, preserving data integrity.
The database holds all Grid-related data except for the NIOS OS itself, including network settings, DNS records, DHCP networks, and user configurations.

**Backup and Recovery**

The Grid Database follows a “One Grid, One Database, One Backup” rule. A single backup file contains the complete database, ensuring all Grid Member data is available in case of recovery needs.


### Section 1 labs:

#### NIOS DDI Introduction Lab Activities


- Verifying services are running in NIOS (1500)
- Verifying NTP configuration in NIOS Grid (1501)
- Verifying NIOS Grid backup configuration (1502)
- Locating NIOS disaster recovery candidate (1503)
- Using NIOS Status Dashboard (1506)




### 2.NIOS DDI DHCP Operations

#### NIOS DHCP Service Overview

The topic covers a comprehensive overview of the DHCP service, including the exploration of DHCP sub-menus, understanding Grid DHCP properties, DHCP property inheritance, and configuring member-specific DHCP properties.

2.1 Enabling NIOS DHCP Service  2 m

2.2 Exploring NIOS DHCP Sub Menus  3 m

2.3 Accessing Grid DHCP Properties  1 m

2.4 Understanding DHCP Properties Inheritance  2 m

2.5 Viewing Member DHCP Properties  1 m

2.6 NIOS DHCP Service Overview: Supplementary Notes

#### Managing NIOS DHCP Objects
This topic covers DHCP network objects, ranges, fixed-addresses, reservations, and lease management, including historical data.  

2.7 Identifying NIOS DHCP Objects  2 m

2.8 Managing DHCP Ranges and Exclusions  5 m

2.9 Utilizing DHCP Fixed-Addresses  3 m

2.10 Creating DHCP Reservations  2 m

2.11 Unpacking DHCP Leases  5 m

2.12 Managing NIOS DHCP Objects: Supplementary Notes

#### NIOS DHCP Failover Overview
This topic provides an overview of DHCP failover, covering its importance and functionality. You will learn the techniques for managing DHCP failover in NIOS, including configuring and monitoring failover status.  

2.13 Introducing NIOS DHCP Failover  5 m

2.14 Managing DHCP With Failover  4 m

2.15 Viewing DHCP Failover Statuses  3 m

2.16 NIOS DHCP Failover Overview: Supplementary Notes

Hands-On Labs
Hands-On Labs
NIOS DDI DHCP Operations Lab Instructions



#### Lab notes:

### NIOS DDI DNS Operations

NIOS DNS Service Overview
This topic provides an overview of the NIOS DNS service, covering the DNS menu and sub-menus, and the concept of DNS properties inheritance.

Viewing the DNS Service Menu  1 m
Accessing Zones and Member Menus  2 m
Understanding DNS Property Inheritance  1 m
NIOS DNS Service Overview: Supplementary Notes

Managing NIOS DNS Resource Records
This topic provides instruction on configuring DNS resource records, including host records, adding multiple hosts using next available IP addresses, and utilizing shared record groups.  

Configuring DNS Resource Records  2 m
Adding DNS Resource Records  4 m
Adding Unknown Records  2 m
Adding Host Records  3 m
Configuring Bulk Host Records  3 m
Adding Multiple Hosts Using Next Available IP Address  2 m
Using Shared Record Groups  3 m
Adding ALIAS Records  3 m
Managing NIOS DNS Resource Records: Supplementary Notes

Using NIOS DNS Views
This topic provides a brief explanation of the DNS view feature, shows you how to identify whether or not your NIOS system has this feature enabled, and how to navigate around this feature.  

Understanding DNS Views in NIOS  2 m
Identifying DNS Views  2 m
Using NIOS DNS Views: Supplementary Notes

Hands-On Labs
Hands-On Labs
NIOS DDI DNS Operations Lab Instructions


#### Lab notes:

### NIOS DDI IPAM Operations

NIOS Extensible Attributes and Smart Folders
This course provides an overview of Extensible Attributes (EAs) and their applications. You will learn how to display, apply, and filter data by Extensible Attributes. Additionally, you will explore the use of Smart Folders for efficient data organization.  

Introducing Extensible Attributes  1 m
Displaying Extensible Attributes  1 m
Applying Extensible Attributes  4 m
Filtering by Extensible Attributes  2 m
Using Smart Folders  2 m
NIOS Extensible Attributes and Smart Folders: Supplementary Notes

NIOS IP Address Management (IPAM) and Global Search
This course provides an overview of the NIOS IPAM (IP Address Management) system, covering topics such as IPAM visual modes, status display, DHCP lease conversion, network containers, net map view, and effective utilization of the Global Search feature for efficient information retrieval.

Examining NIOS IPAM  2 m
Investigating IPAM Visual Modes  4 m
Interpreting IPAM Statuses  6 m
Converting DHCP Leases  2 m
Exploring Network Containers and NetMap View  4 m
Accessing Global Search  1 m
NIOS IP Address Management (IPAM) and Global Search: Supplementary Notes

Using NIOS CSV Export
This course offers an overview of NIOS data export, providing insights into the process and its significance. You will learn how to effectively export data from NIOS for various purposes and use cases.  

Introducing NIOS Data Exports  2 m
Exporting Data in NIOS  3 m
Reviewing CSV Job Manager  3 m
Using NIOS CSV Export: Supplementary Notes


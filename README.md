# 3CX-Intermediate-Notes
Notes from 3CX Intermediate Certification Might help you study, better then the videos and slides imo.

```
Backup and Restore:

Stores the backup in a single ZIP file
Use backup for
	Migration
	OS Change
	Major versian upgrades
	Revision and Recovery	
	Failover Sync

Backup is integreated in the managment console
	Can be schedules and can be On Demand
	Also can password protect it

Different Types of data
Host Data
Host data is not restored
This is the data whci is only relevant to the installation on the current host
This is things like
	SIP and Tunnel ports
	External RTP ports

User data
User data is restored
This is the configuration that runs on the host. It consists of all the configuration information, which is made by the client, as well as some of the main host data parts.
This is things like
	Systen extensions
	VoIP providers and gateways
	Prompts
	Audio Files
	Licence keys, FQDN and SSL certificates

There are many options for backups, but the main config that is always included is
	Extension and group settings
	Trunks
	Inbound rules
	Outbound rules
	
Backup location options
	Local Disk (SATA, iSCSI, SAN)
	FTP
	Google Drive

Backup tips
Place recording in different location. This provides
	Smaller 3CX backup file
	more flexibility

Backup Scheduling
Managment console -> backup and restore -> backup schedule

Command line backup
All the options are also available for backup in the command line
	But FTP and Google Drive are not supported
Using the command line you are able to set different local paths for backup files

l, --log=VALUE		Log path or filename

f, --file=VALUE		Zip path or filename to backup *

o, --options=VALUE	Backup options (what to include in the backup) *
			ALL - include all
			Any combination of following strings (comma-separated):
			CH - include CallHistory
			LIC - include license
			FQDN - include FQDN
			PROMPTS - include Prompts
			FW - include Firmware
			REC - include Recordings
			VM - include VoiceMails

c, --cfg=VALUE		Configuration file full path

h, --help		Show this help

--pwd=Value		Encrypt Backup with Password (V15.5 Sp2 onwards)

Arguments with * are mandatory.

How to open the command prompt in Windows and Linux

Windows
1.	Open Command Prompt as Administrator and go to:
		cd C:\Program Files\3CX Phone System\Instance1\Bin
2.	Enter BackupCmd.exe --help command to see all available options.
3.	To make a full backup run this command: 
		BackupCmd.exe --file=backuptest.zip --options=ALL --log=backuptest.log
4.	To make a backup including Call history, license and FQDN, run this command:
		BackupCmd.exe --file=backuptest.zip --options=CH,LIC,FQDN --log=backuptest.log


Linux
1.	Open a Terminal and go to:
		cd /var/lib/3cxpbx/Instance1/Bin/
2.	Run “sudo -u phonesystem /usr/sbin/3CXBackupCmd --help” command to see all available options.
3.	To make a full backup run this command: 
		sudo -u phonesystem /usr/sbin/3CXBackupCmd --file=backuptest.zip --options=ALL --log=/tmp/backuptest.log
4.	To make a backup including Call history, license and FQDN, run this command: 
		sudo -u phonesystem /usr/sbin/3CXBackupCmd --file=backuptest.zip --options=CH,LIC,FQDN --log=/tmp/backuptest.log

Restoring using CMD
RestoreCMD is the command line tool to restore backups via command line

-l, --log=VALUE		Log path or filename

-f, --file=VALUE	Zip path or filename to restore *

-c, --cfg=VALUE		Configuration file full path

-h, --help		Show this help

--pwd=Value		decrypt Backup with Password (V15.5 Sp2 onwards)

--failover		Failover mode (services will not be started after restore)

Arguments with * are mandatory.

---------------------------------------------

Call Recording:

Admin can prevent an extension from starting or stopping recordings
Recordins of calls include
	Off (Don't record anything)
	All calls
	External Cals only

File location
	Local Disk
	iSCSI
	SAN
Network drives or folders are not supported

Quota
Managment console -> Recording -> Quota
The default is 5GB
Some options include
	Auto clean (deletes old recordings)
	Notification (Email when nearing quota limit)

Recording format
All recordings are in WAV 8kHz 1 bit Mono
Compression options
PCM
	File is approx 1MB per minute of recording
	Playable by all media players

ADPCM
	File is approx 256KB per minute of recording
	Not playable on all media players, can be harder to player recording

Managing call recording in 3CX
Managment console -> "recordings" node

Can set retention for call recordings (Deletes recordings after a certain ammount of days)
Managment console -> Recording node
Enter the quote you would like and all the other options that are available


---------------------------------------------

Extension groups and Rights

Managment console -> groups

You can create groups as departments
Users can
	Be part of multiple groups
	See others in groups
	Perform actions to others in groups

users in groups can be split into different roles
	Manager Role
	User Role

Can set rights to group in general or just to one user

Rights Control
Managment Rights
	Manage phonebook (Company phonebook)
	IVR Control

Presence Rights
	Extension Visibility
	Extension calls
	
Action Rights (requires visibility of users and calls)
	Parking
	Intercom
	Call manipulation - Divert, transfer, Pick up calls
	Receptionist Operations (Status, wake up calls, check in/out
	Barge in, listen, whisper
		Only on Pro or enterprise licence

Specific Group Rights
Can do fine tuning to specific individual users

Group Publishing
Makes all extensions visible to everyone
	No cals visible
	No control actions

Managment Console Delegation
3CX managment console -> extension -> rights
This allows users to configure other users
Extension Managment
	Granular extension managment
System Managment
	Administer SIP trunks
	General System administration (Access to settings)
	General call reports


---------------------------------------------

Extension Profiles and Forward Rules:

Extensions can either ring or not rign based on their status
Ring
	Available
	Custom 1
Not Ring
	Away
	Do Not Disturb
	Custom 2

Places where you can change your staurs
	3CX apps
	3CX Web Client
	Dial code
		*30 -> available
		*31 -> Away, etc..
	IP Phone DnD button (Activates *32 Do Not Disturb)
	BLF Button
	Auto switch based on time
	From managment console
	From within voicemail menu


Forwarding Rule Options

Available or Custom 1
You can set a rule timout that will forward the call to one of the following
	Voicemail
	Mobile/external number
	Other Extension
	End call

There are other options too

Away, Do Not Disturb & Custom 2
When they ring the extension, the call gets forwarded to one of the following
	Voicemail
	Mobile/external number
	Other Extension
	End call

Can also setup seperate forward rules based on the internal or external numbers

Can setup different voicemail greetings per profile
This will override the default greeting for only that one specific status
File format
	WAV PCM 8kHz 16 bit Mono

Exceptions
You can create exceptions for each extension
Max of 15 exceptions per extension
Can use * (star) symbol as a wildcard
Each extension can apply for certain times of day or days of week
The order of the exceptions matter
	It checks them from top to bottom

Profiles can switch during different times of the day

---------------------------------------------

Hot Desking
Allows an IP ohone to be used by multiple users
Each user can login and use their own extension
Ideal for spaces where users may not have assigned seating

Can only be done on Pro or Enterprise license versions

Managment Console -> Hotdesking -> Add
Need to set the make/model of the phone and the MAX address
set the provisioning method
	Local IP or SBC device

After that is done, the phone will show up as a new phone
Need to assign an extension

By default, new extensions cant do Hot Desking
To enable
	Edit Extension -> Options
	Enable Hot Deskting


Logging in and Out
Default dial code is *77*
	This can be chnaged in Settings -> Dial Codes

Will need extension number and voicemail PIN to login
	This can be found in the welcome email

Login
	*77*EXT*

Log Out
	*77*5*
	1st BLF provisiond as logout button

THe admin can see what extensions have logged in and when

More here
	https://www.3cx.com/docs/hot-desking/


---------------------------------------------

PBXEXpress:

PBXExpress is a tool that allows you to set up and deploy your 3CX in the cloud very fast

Installing PBXExpress:
Follows steps

FQDN
Needs
	Hostname
	Domain Suffix

Select your cloud service that you are using
Choose VFS
	The specs of the machine you are using depends on your size of installation

After everything is done, an email will be sent to you
	This contains link to Managment console and login and password

PBXExpress Microsoft Azure

Log into your microsoft Azure account
Go to Cost managment + billing and get your Subcription ID

Go to PBXExpress
Select that you want to use Microsoft Azure and put in your subscription key
Go through all the steps
Click accept and everything is done and setup for you now

PBXExpress in Google Cloud

Enable your Google Cloud Compute Engine and API
This is required to create a VM instance with 3CX running on it

Go to your PBXExpress
Select that you want to use Google as your cloud service
Log into your Google account
Everyhting is now setup

PBXExpress in Amazon AWS (Amazon Web Services)

Get your Amazon AWS account

1.	Search for "IAM" in the search. Select IAM - Manage User Access and Encyrption Keys"
2. 	Click users
3. 	Click add user
4.	Create username and select "Programmatic access", click next
5.	Click "Attach existing polocies directly", select "ec2" and "AmazonEC2FullAccess", click next
6.	Click create user
7.	Record the Access Key ID and the Secret Access Key
8.	Click EC2 and select a location where you want to create this machine
9.	Click on key pairs and then Create Key Pair
10.	Enter a name and click create. Download the file and keep it safe as it cant be downloaded again

Go to your PBXExpress
Select that you want to use Amazon AWS
Enter the Access Key ID and the Secret Access Key
Done


---------------------------------------------

SBC Configuration (Session Boarder Controller)
Supported OS
	Windows 10 Pro or Enterprise
	Debian 9 (amd64)
	Raspberry Pi (Raspbian)

SBC Hardware Specifications
Windows and Debian
Max
	50 Extensions
	100 simultaneous calls
	250 BFL keys accress all phones

Pasberry Pi 2 or 3
Max
	20 Extensions/simultaneous calls
	200 BLF key accress all devices

Benefits of using 3CX SBC
	Internal call capabilities to a Group of Remote Users
	Dropship IP Phones
	PnP Provisioning
	Traffic remains in LAN -> for phones connected to the same SBC
	Overcome NAT and SIP ALG router issues
	Encyption
	Its free

Setting up in 3CX
SIP Ports and tunnel Port
	Settings -> Network -> Ports

3CX Tunnel Password
	Settings -> Security -> 3CX Tunnel

Public FQDN
	Dashboard -> Information -> FQDN

Installing SBC
Get it and follow the steps to install it
	Can put optional encryption
	Can set failover PBX

Ip Phone provisioning with PnP
Boot phone in the remote location
	answer the PnP request in the 3CX Managment Console
	Assignt the IP Phone an existing extension or create a new one
	and thats it

3CX Tunelling combines SIP (Signaling) and RTP (Audio) VoIP packets from one location and delivers tham to an from another location using a custom TCP protocol
This overcomes some firewall ir telecom provider issues
3CX Tunnel can be used to resolve things like
	Resolve Issues of NAT traverse at both the remote and the PBX location
	Simplified firewall configuration at both the remote and the PBX location
	Overcome diffuculties with ISP that block VoIP traffic based on port numbers
	Allows VoIP over WIFI in some restricted locations, such as hotel rooms
	Fixes Firewalls that cant handle VoIP traffic correctly or which are very problematic to configure correctly, such as Microsoft ISA server


Session Boarder Controller
	https://www.3cx.com/docs/3cx-tunnel-session-border-controller/


---------------------------------------------

STUN Configuration
Session Traversal Utilities for NAT (STUN)
Methods like network protocol, trverse of network address translator, gateway in application real-time voice, video, messaginf and others
Allows to setup phone calls to a VoIP provider hosted outside of the local network
STUN is a client-server protocol, it is usually embeded in VoIP software such as a IP PBX or IP Phone
It sends a request to a STUN server to discover its public IP and ports, and the STUN server sends a response
There are 2 types of requests
Binding Requests
	Typically send over UDP
	This is used to determine the IP and ports bindings allocated by NAT's.
	When the request is sent, the STUN server looks at the source IP address and the port, and writes them down into a binding response that is send back to the client
	
Shared Secret Requests
	Sent over TLS (Secure communication) over TCP
The Shared Secret Requests asks the STUNserver for a temporary set of cridentials which are then used in the Binding Request and Bidning Response exchange



STUN Benefits (remote worker)
	Internal call capabilities to remote users
	Dropship IP phones
	No additional hardware required
	Autodiscovery of your 3CX


Provisioning STUN extensions as an admin
can create and configure extensions in 3CX
Need	
	Make/model
	MAC address
	remote ports
When setting up you need to increment SIP/RTP ports for each sunsequent device
Uncheck: Options -> "disallow use of extension outside the LAN"
Create NAT in remote firewall -> IP phones
Send welcome email

STUN is method to
	Discover the public IP address (from a LAN)
	Discover Firewall port mapping (from a LAN)
the STUN server is your own PBX

Provisioning remote extensipons as an end user
reset the phone (If it is not a new device)
When the phone prompts for login -> from welcome email
	Username:	Extension number
	Password:	Voicemail PIN

RPS
Automatically triggered whn provisioning a phone using the STUN method
Binds MAC address to (Provisioning) URL
Phone boots and retreives URL
after 2 weeks MAC/URL is unlisted

Troubleshooting
If STUN extension shows
	No connection to server
	Audio issues
	Cant transfer, hold or resume calls

Check the remote firewall 
	Make sure SIP ALG is disabled
	Check that NAT is implemented

STUN Messages
STUN messages are TLV (type-length-value) encoded. It is 20 bytes
All STUN messages start with a STUN header, followed by a STUN payload.

Examples	
0x0001	Binding Request
0x0101	Binding Response
0x0111	Binding Error Response
0x0002	Shared Secret Request
0x0102	Shared Secret Response
0x0112	Shared Secret Error Response


Common error codes
Error Code 400		Bad request. Client must modify request and send it again
Error Code 420		Unknown attribute, the server did not understand an attribute in the request
Error Code 430		Stale credentials, the shared secret send in the request is expired, the client should abtain a new shared secret
Error Code 432		Missing username, the username attribute is not present in the request
Error Code 500		Server error, the client should try sending the request later

STN protocol Attributes present in STUN requests and responses
Here
	https://www.3cx.com/blog/voip-howto/stun-details/

---------------------------------------------

Time Based Scheduling:

Office hours has 3 states
	In (Working)
	Break (eating, extensions only)
	Out of office
This can alter behaviour of inbound rules and extensions

Can set up PBX for global office hours
	Day by day time schedule
	on a weekly basis
Managment console -> Settings -> timezone -> office hours & holidays

Specific Office Hours
	Day by day time schedule
	On a weekly basis
These can be set individualy for each inbound rule or extension
This overrides global office hours

Holiday Hours
	Specific hours of a single day
	Can also be a range of days
	CAn set on annual basis
Defines when the whole PBX is in holiday mode
Managment console -> Settings -> Timezone, office hours & holidays

Inbound Routing for Global Office Hours
Managment console -> inbound rules -> select your inbound rule
Destination for calls during office hours -> In office hours
Destination for calls outside office hours -> Outside office hours

Inbound Routing - Specific Office Hours
Managment Console -> Inbound rules -> Select your inbound rule	
	Check option Setup specific office hours for this trunk
	Set the times
	Inbound rule will ignore global office hours
	Routing done based om the specific Office Hours instead

Inbound Routing - Holidays
Managment console -> Inbound rules -> select your inbound rule
	Check option Play holiday prompt when its a global holiday
	If its a holiday -> Global and specific office hours are ignored
	Caller hears the set prompt depending in the holiday
	Caller is then transferred to the HOL digital receptionist


Changing extenstion status automatically
Managment Console -> extensions -> edit your extension -> forward rules tab
Here the profile will automatically change
The profile will not automatically chnage if profile Custom 1 or Custom 2 are in use
CAn autowitch exetenstion based on gloabal office hours

---------------------------------------------

```


# Initial Lab Set Up

## Set up procedure for a Basic Domain Controller and a single Workstation Client.

### Provisioning the DC
First in the CARC Console we select the approriate sized VM we need to provision to run a simple Domain Controller. For detailed requirements see [this](https://learn.microsoft.com/en-us/windows-server/get-started/hardware-requirements?tabs=network&pivots=windows-server-2019) link.
![CARC Console](Images/Screenshot%202026-10-17%20at%2019-25-36%20Virtual%20Machines%20CARC%20Compute%20Console.png)
_CARC Console_
![Hardware Image Selection](Images/Screenshot%202026-10-17%20at%2019-27-04%20Virtual%20Machines%20CARC%20Compute%20Console%20(2).png)
_Name of the VM and the Hardware to Provision_
![Provisioned VM Access](Images/Screenshot%202026-10-17%20at%2021-43-47%20Access%20CARC%20Compute%20Console%20(4).png)
_Once the VM is Provisioned, we can configure the password to access the machine using the "Access" tab in the left hand side panel_
![Provisioned VM](Images/Screenshot%202026-10-17%20at%2021-42-52%20Overview%20CARC%20Compute%20Console%20%283%29.png)
_And finally on this page we can access the VM using the "Connect" button on the VM overview page. And using the RDP option for the most convenient way to access the VM_

### Provisioning the client Workstation
Following the same process we provision a client machine that can get away with having less resources.
![Client Hardware Selection](Images/Screenshot%202026-10-18%20at%2013-23-04%20Virtual%20Machines%20CARC%20Compute%20Console.png)
![Client VM Access](Images/Screenshot%202026-10-18%20at%2013-43-50%20Access%20CARC%20Compute%20Console.png)
![VM Screen](Images/Screenshot%202026-10-18%20at%2013-44-19%20Virtual%20Machines%20CARC%20Compute%20Console.png)
We should see this screen with both of our VMs, once the client is provisioned.

### Domain Controller Setup
To promote Windows Server to a domain controller we need a few pre-requisits that we will cover in this section. First, we access the DC VM, (using the connect button and RDP) because of the way the networking on the CARC platform works the DC already has a static IP address assigned to it, so the only thing we need to configure before giving the server the DC features and role is the machine name.  
![DC Access Screen](Images/Screenshot%202026-10-18%20151901.png)  
Once we connect to the machine using RDP it will ask us for the VM password.

Then right clicking on the Windows Icon we can select Settings->About->Rename this PC we can set the name "DC" for the Domain Controller VM. The machine will then ask to restart. Once Restarted this VM's name will become "DC".  
![DC Settings Access](Images/Screenshot%202026-10-18%20151950.png)

![DC Settings About](Images/Screenshot%202026-10-18%20152107.png)

![DC Rename](Images/Screenshot%202026-10-18%20152252.png)

![DC Rename Restart](Images/Screenshot%202026-10-18%20152327.png)  

Once the VM is restarted we can get started on giving it the roles and features needed for it to be a Domain Controller.  
![Server Manager: Add Roles and Features](Images/Screenshot%202026-10-18%20152443.png)  
So we navigate to the "Manage" button and select "Add roles and Features". We can select the defaults for the first few sections until we reach "Server Roles", where we check the box for the "Active Directory Domain Services" this will create a new pop up to show the Active Directory Administration tools and features to be installed. We can press the "Add Features" button to proceed.
The Defaults for the next few sections are fine as well until we reach the "Confirmation" section, where we can select the check box to automatically restart the server so that we don't have to manually restart every time and then click "Install" to proceed. Once completed the server will restart and we can proceed to promote this server to the Domain Controller.
![Add Roles and Features Wizard: Server Roles](Images/Screenshot%202026-10-18%20152542.png)  
![ADDS features](Images/Screenshot%202026-10-18%20152604.png)  
![Add Roles and Features Wizard: Confirmation](Images/Screenshot%202026-10-18%20152641.png)  
![Add Roles and Features Wizard: Results](Images/Screenshot%202026-10-18%20152712.png)  
![Add Roles and Features Wizard: Results Complete](Images/Screenshot%202026-10-18%20152712.png)  
Now that our server is restarted, we can proceed with the promotion to DC. The server manager interface will show a yellow warning sign, if we click on it and select "Promote this server to a domain controller"
![Server Manager: Promote](Images/Screenshot%202026-10-18%20152856.png)  
In this wizard we can add a new AD forest by selecting the "Add a new forest" option and giving it a name. ("cyberclub.ad" in our case).
![ADDS Config Wizard](Images/Screenshot%202026-10-18%20152935.png)  
Next, we have to configure a Restore Mode Password and click Next.
![ADDS Config Wizard: DC Options](Images/Screenshot%202026-10-18%20153011.png)  
We can select the defaults for the next few sections and hit "Install". The yellow warning sign can be safely ignored.
![ADDS Config Wizard: Additional Options](Images/Screenshot%202026-10-18%20153137.png)  
![ADDS Config Wizard: Prerequistes Check](Images/Screenshot%202026-10-18%20153214.png)  
Once done, we'll be notified that we're about to be signed out. We can close that, and after a restart our DC machine will be configured as a Domain Controller.

### Configuring the Workstation and Connecting to the Domain

While it's possible to connect the workstation to our cyberclub.ad domain without too much prior configuration, to make things easier to track going forward with this project we'll rename the WorkStation machine as well before joining it to the domain. We will follow the same steps we followed to rename the domain controller.

_Access the VM -> Right-click the windows icon on the task bar -> Settings -> About -> Rename this PC_

![WorkStation Rename Settings](Images/Screenshot%202026-10-21%20095418.png)  
![WorkStation Rename Settings: Rename](Images/Screenshot%202026-10-21%20095550.png)  
![WorkStation Rename Settings: About](Images/Screenshot%202026-10-21%20095508.png)  
![WorkStation Rename Settings: Restart](Images/Screenshot%202026-10-21%20095508.png)  

After the Restart we can start the configuration to connect this workstation device to the domain.

First we need to configure the workstation so that it uses the DC for all DNS requests. We can right click in the TaskBar on the network icon and select "Network and Internet settings".
![Workstation: Network and Internet Settings](Images/Screenshot%202026-10-21%20095730.png)  
Then we navigate to Ethernet -> Scrolldown to find DNS server assignment -> Edit.
![WorkStation Network & Internet: Ethernet](Images/Screenshot%202026-10-21%20095832.png)  
![WorkStation Network & Internet: DNS Server Assignment](Images/Screenshot%202026-10-21%20095913.png)  
Next, in the Preferred DNS field we add the IP address of the domain controller.
![WorkStation Network & Internet: DC DNS](Images/Screenshot%202026-10-21%20100006.png)  
Now we get to the point where we actually connect to the domain. We navigate to windows Settings again -> System -> About and click on the "Advanced system Settings" button.
![WorkStation System: About](Images/Screenshot%202026-10-21%20112225.png)  
In the new popup we select the "Change" button.
![WorkStation Advanced System Settings](Images/Screenshot%202026-10-21%20112259.png)  
In this pop up we can select the Domain radio button (instead of the default "Workgroup") and enter our domain name ("cyberclub.ad" in our case) clicking ok will bring up a screen where we can enter our credentials that has the permission to connect new machines to the domain.

![WorkStation Advanced System Settings: Domain](Images/Screenshot%202026-10-21%20112452.png)  

And there we go, the workstation is connected to the domain.

![WorkStation Pop Up: Welcome](Images/Screenshot%202026-10-21%20112514.png)  

![WorkStation Pop Up: Restart Suggestion](Images/Screenshot%202026-10-21%20112553.png)  

With this final restart our basic AD environment is set up.
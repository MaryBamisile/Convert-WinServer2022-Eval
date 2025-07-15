# Convert-WinServer2022-Eval

For the same of this demo, I will be using hyper-v to create a windows server 2022 eval VM 

##### Hyper-V Host installation & configuration

- Connect to the host server and install Hyper-V tools: execute following Powershell command as Administrator:

```
Install-WindowsFeature -Name "Hyper-V" -IncludeManagementTools -Restart
```
VM will restart when installation is done.

- Connect again to the machine.

- To allow nested VMs communication, create an internal switch:

```
New-VMSwitch –SwitchName "NATSwitch" –SwitchType Internal
```

<img width="1021" height="244" alt="image" src="https://github.com/user-attachments/assets/cce046cf-2844-423e-a024-98b936b99299" />

##### Create a new IP address and assign it to previously created switch:
```
New-NetIPAddress –IPAddress "192.168.0.1" -PrefixLength 24 -InterfaceAlias "vEthernet (NATSwitch)"
```

##### Create NAT on created switch:
```
New-NetNat –Name "NatNetwork" –InternalIPInterfaceAddressPrefix "192.168.0.0/24"
```
##### Turn off Windows Defender Firewall
```
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False 
```

<img width="1026" height="738" alt="image" src="https://github.com/user-attachments/assets/8fc2876f-b5da-4e57-8e65-7a0505274b1b" />

- Right click on the demo-server and click on connect
- When on the start menu, click and immediately press any key on your keyboard.
- Follow the prompt to install the operating system.

<img width="1506" height="910" alt="image" src="https://github.com/user-attachments/assets/0936f0db-dbbf-4695-a620-7e507ad05756" />

Details of the current version:

<img width="658" height="393" alt="image" src="https://github.com/user-attachments/assets/99805ebf-51b8-43e7-bad4-374d62f93e95" />

Apply the following network configuration on the Demo-server:

<img width="1100" height="630" alt="image" src="https://github.com/user-attachments/assets/06626fd9-80c2-4a71-b9be-9945eeabf1af" />

### Convertion from server 2022 eval to Standard edition following the steps here {(https://learn.microsoft.com/en-us/windows-server/get-started/upgrade-conversion-options#windows-server-standard-or-datacenter)}

#### Notes from MS docs: 
If your server is running an evaluation version of Windows Server Standard or Datacenter edition, you can convert it to an available retail version. Run the following commands in an elevated command prompt or PowerShell session.

- Determine the current edition name by running the following command. The output is an abbreviated form of the edition name. For example, Windows Server Datacenter (Desktop Experience) Evaluation edition is ServerDatacenterEval.

```
DISM /online /Get-CurrentEdition
```

<img width="773" height="516" alt="image" src="https://github.com/user-attachments/assets/09efd9bd-fe50-4199-93bb-00b7e6182507" />

- Verify which editions the current installation can be converted to by running the following command. From the output, make a note of the edition name you want to convert to.

```
DISM /online /Get-TargetEditions
```
<img width="769" height="376" alt="image" src="https://github.com/user-attachments/assets/4048cab5-f191-4787-b4a5-4888b72b90ce" />

- Run the following command to save the Microsoft Software License Terms for Windows Server, which you can then review. Replace the <target edition> placeholder with the edition name you noted from the previous step.

```
DISM /online /Set-Edition:ServerStandard /GetEula:C:\license.rtf
```

- Enter the new edition name and corresponding retail product key in the following command. The set edition process requires you to accept the Microsoft Software License Terms for Windows Server that you saved previously.
```
DISM /online /Set-Edition:<target edition> /ProductKey:<product key> /AcceptEula
```

<img width="1086" height="472" alt="image" src="https://github.com/user-attachments/assets/dc09c98e-150c-4f45-88b4-36ce8b19eded" />

###### For Example
```
DISM /online /Set-Edition:ServerStandard /ProductKey:ABCDE-12345-ABCDE-12345-ABCDE /AcceptEula
```

### You should be promted to restart

##### Now, we can see:

<img width="1168" height="914" alt="image" src="https://github.com/user-attachments/assets/ab8ec1d9-e6b6-4c62-a2c8-78ef3c050f5a" />


A bunch of active directory excercises

Unser Remotehost heisst GFN-RLAB oder RM-POWERSHELL. Dieses Lab 
basiert auf Server 2022 als Hypervisor und beinhaltet fünf virtuelle Maschinen (VMs):
• DC
• Server1
• Server2
• Server3
• W11

Die VMs haben folgende Einstellungen:

DC:
IP: 192.168.1.200
DNS-Server

Server1:
192.168.1.1

Server2:
192.168.1.2

Server3:
192.168.1.3

W11:
192.168.1.4

Active Directory ist auf DC, Server1, Server2 und Server3 installiert. DC ist Domain Controller, Server1 der repliziende Server.

```powershell
PS C:\Users\Administrator> netdom query fsmo
Schemaster            DC.gfnlab.test
Domänennamen-Master   DC.gfnlab.test
PDC                   DC.gfnlab.test
RID-Pool-Manager      DC.gfnlab.test
Infrastrukturmaster   DC.gfnlab.test
Der Befehl wurde ausgeführt.

PS C:\Users\Administrator>
```

Es existieren jeweils zwei Prüfpunkte:
• Basis <- Standardkonfiguration inkl. Netzwerk.
• Basis-AD <- ActiveDirectory ist eingerichtet, Computer sind beigetreten.

---
*Our remote host is named `GFN-RLAB` or `RM-POWERSHELL`. This lab runs on Windows Server 2022 as a hypervisor and contains five virtual machines (VMs):*  
- `DC`  
- `Server1`  
- `Server2`  
- `Server3`  
- `W11`  

*The VMs have the following settings:*  

*DC:*  
- IP: `192.168.1.200`  
- DNS server  

*Server1:*  
- IP: `192.168.1.1`  

*Server2:*  
- IP: `192.168.1.2`  

*Server3:*  
- IP: `192.168.1.3`  

*W11:*  
- IP: `192.168.1.4`  

*Active Directory is installed on DC, Server1, Server2, and Server3. DC is the Domain Controller, and Server1 is the replication server.*  

```powershell
PS C:\Users\Administrator> netdom query fsmo
Schema Master           DC.gfnlab.test
Domain Naming Master    DC.gfnlab.test
PDC                     DC.gfnlab.test
RID Pool Manager        DC.gfnlab.test
Infrastructure Master   DC.gfnlab.test
The command completed successfully.

PS C:\Users\Administrator>
````

*Each VM has two checkpoints:*

* `Base` <- standard configuration including network
* `Base-AD` <- Active Directory is set up, computers have joined the domain



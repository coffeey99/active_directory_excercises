```markdown
- Aktiviere auf den virtuellen Maschinen **Server2** und **Server3** die Nested Virtualization.
- Installiere die Rolle **Hyper-V** auf **Server2** und **Server3**.
- Konfiguriere auf **Server2** und **Server3** einen externen Switch mit dem Namen **Externes Netzwerk**.
- Aktiviere für die virtuellen Netzwerkadapter von **Server2** und **Server3** das MAC-Address-Spoofing.
- Ändere den Speicherort für virtuelle Maschinen und virtuelle Festplatten auf **C:\ClusterStorage\Volume1\VM**.
- Installiere im Cluster eine hochverfügbare virtuelle Maschine namens **VM01** mit einer Windows Server 2022 Datacenter Core-Version.
- Konfiguriere auf **Server2** für **VM01** im Failover-Cluster-Manager das MAC-Address-Spoofing und entferne das DVD-Laufwerk.
- Konfiguriere die **VM01** mit der IP-Adresse **192.168.1.160/24**. Verwende **192.168.1.200** als Gateway und **192.168.1.200** sowie **192.168.1.1** als DNS-Server.
- Nimm **VM01** in die Domäne auf.
- Aktiviere in PowerShell die Firewall-Regel für eingehendes Pingen (ICMPv4).
- Pinge von **DC** dauerhaft auf **192.168.1.160** mit der Option `-t`.
- Verschiebe anschließend **VM01** vom aktuellen Knoten auf den anderen Knoten des Failover-Clusters und beobachte das Pingen auf **DC**, um die Dauer des Schwenks abzuschätzen.
```

```markdown
- *Enable Nested Virtualization on the virtual machines Server2 and Server3.*
- *Install the Hyper-V role on Server2 and Server3.*
- *Configure an external switch named Externes Netzwerk on Server2 and Server3.*
- *Enable MAC Address Spoofing on the virtual network adapters of Server2 and Server3.*
- *Change the default storage location for virtual machines and virtual hard disks to C:\ClusterStorage\Volume1\VM.*
- *Install a highly available virtual machine named VM01 in the cluster with Windows Server 2022 Datacenter Core.*
- *On Server2, configure VM01 in the Failover Cluster Manager to enable MAC Address Spoofing and remove the DVD drive.*
- *Configure VM01 with the IP address 192.168.1.160/24, gateway 192.168.1.200, and DNS servers 192.168.1.200 and 192.168.1.1.*
- *Join VM01 to the domain.*
- *Enable the firewall rule for incoming ICMPv4 (ping) via PowerShell.*
- *From DC, continuously ping 192.168.1.160 using the `-t` option.*
- *Then move VM01 from the current node to the other node in the failover cluster and observe the ping from DC to estimate the duration of the failover.*
```

### Aktivierung der „Nested Virtualization“ auf „Server2“ und „Server3“  
### *Enabling nested virtualization on Server2 and Server3*

Bevor wir die Nested Virtualization einrichten, soll der Cluster-Dienst auf Server2 oder Server3 ausgeschaltet sein und die Server müssen alle ausgeschaltet werden.  
*Before setting up nested virtualization, the cluster service on Server2 or Server3 must be turned off and all servers must be shut down.*

Auf der Host-PC --> Powershell-ISE als Administrator öffnen:  
*Auf der Host-PC --> Powershell-ISE als Administrator öffnen:*

```powershell
# Liste der zu konfigurierenden VMs
$vmNamen = @("Server2", "Server3")

# Da Änderungen am Prozessor nicht im laufenden Betrieb
# funktionieren, werden die entsprechenden VMs zuvor heruntergefahren

# Schleife durch jede VM, der Status wird überprüft
foreach ($vmName in $vmNamen) {
    # Wenn die VM gerade gestartet ist, wird sie beendet
    $vm = Get-VM -Name $vmName
    if ($vm.State -eq "Running") {
        Stop-VM -Name $vmName -Force
    }

    # Der Parameter -ExposeVirtualizationExtensions $true ermöglicht
    # es, dass die virtuelle Maschine die Hardware-Virtualisierungserweiterungen des Hosts nutzen kann.
    # Damit kann die virtuelle Maschine andere virtuelle Maschinen
    # oder Container ausführen, die auf Hyper-V oder Docker basieren.
    Set-VMProcessor `
        -VMName $vmName `
        -Count 4 `
        -ExposeVirtualizationExtensions $true

    # Der Parameter -StaticMemory sorgt dafür, dass kein dynamischer
    # Arbeitsspeicher für die VM benutzt wird. Zusätzlich wird der
    # Arbeitsspeicher mittels -MemoryStartupBytes von 2GB auf 4GB für
    # beide VMs erhöht
    Set-VM -Name $vmName `
        -StaticMemory `
        -MemoryStartupBytes 4096MB
} # Ende der foreach-Schleife und Ende des Skripts
```

<img width="587" height="662" alt="image" src="https://github.com/user-attachments/assets/ba87f64e-1fa4-4dbb-b06c-fccb691207e5" />

<img width="509" height="264" alt="image" src="https://github.com/user-attachments/assets/03ab7154-6313-4572-a3c2-97df95f9d4f6" />

<img width="577" height="346" alt="image" src="https://github.com/user-attachments/assets/be3b2c0a-669a-4600-8a3b-510fc0c69446" />

<img width="503" height="233" alt="image" src="https://github.com/user-attachments/assets/c16786c3-6db7-493d-944e-7282e470f3df" />

<img width="784" height="101" alt="image" src="https://github.com/user-attachments/assets/7e351e95-0ed0-4db1-88a5-31c165745c39" />

Server2 --> Powershell-ISE Administrator

<img width="869" height="301" alt="image" src="https://github.com/user-attachments/assets/ecae8493-64b8-4c79-841a-2001ed6b8570" />

```powershell
Hier ist dein Skript sauber eingerückt im GitHub/Markdown-PowerShell-Format:
# Wenn das Erstellen des externen Switches gelingt, ...
if (New-VMSwitch -Name "Externes Netzwerk" `
    -NetAdapterName "Domäne" `
    -AllowManagementOS $true)
{
    # wird eine Erfolgsmeldung ausgegeben
    Write-Host 'Der Switch "Extern" wurde erfolgreich erstellt.' `
        -ForegroundColor Yellow
}
else
{
    # Ansonsten wird die Bitte zur Überprüfung der NIC angezeigt
    $Warnmeldung = "Beim Erstellen des Switches ist ein Fehler " +
        "aufgetreten.`nÜberprüfe den Namen " +
        'der Netzwerkverbindung "Domäne".'
    Write-Host $Warnmeldung -ForegroundColor Yellow
}
# Ende des Skriptes
```
<img width="916" height="370" alt="image" src="https://github.com/user-attachments/assets/d36572ac-61b4-4809-98b6-c9026913e5c8" />

Die selbe Skript auch auf Server3 ausführen. 
*Run the same script on Server3 as well.*

<img width="467" height="541" alt="image" src="https://github.com/user-attachments/assets/10839d3c-eac1-41fb-a471-65f3a6248142" />

<img width="760" height="638" alt="image" src="https://github.com/user-attachments/assets/41aa66b0-61cc-4f8a-a2e4-51662aa99fc4" />

<img width="830" height="480" alt="image" src="https://github.com/user-attachments/assets/293e096e-06dc-444e-a215-a5c686c239c2" />

<img width="820" height="487" alt="image" src="https://github.com/user-attachments/assets/a7773599-1871-4375-9c7d-12fbf4a936a4" />

Anwenden + OK  
*Apply + OK*

<img width="822" height="588" alt="image" src="https://github.com/user-attachments/assets/c44c9152-a17f-44b7-ba6d-14ee1e0e75a0" />

<img width="827" height="317" alt="image" src="https://github.com/user-attachments/assets/8e3e1019-6a04-425a-b70d-3abf808c5df1" />

<img width="889" height="206" alt="image" src="https://github.com/user-attachments/assets/7a17a65d-6043-4bba-839f-27a33137d047" />

Anwenden + OK  
*Apply + OK*

<img width="678" height="508" alt="image" src="https://github.com/user-attachments/assets/b941517e-c830-4105-950d-8f25703c9cfb" />

<img width="481" height="610" alt="image" src="https://github.com/user-attachments/assets/0bee870e-0366-475a-84c4-0b3ebf15eeca" />

<img width="878" height="664" alt="image" src="https://github.com/user-attachments/assets/9303830e-958d-4ad9-bd77-b500cebca6eb" />

<img width="876" height="662" alt="image" src="https://github.com/user-attachments/assets/12d07570-5385-43b4-8564-c8f80450b707" />

<img width="873" height="661" alt="image" src="https://github.com/user-attachments/assets/abd17429-2645-46af-9fa5-6a65dfd32771" />

<img width="867" height="666" alt="image" src="https://github.com/user-attachments/assets/34ff19d8-0d4a-4482-a55e-cdf89f3cfa44" />

<img width="869" height="666" alt="image" src="https://github.com/user-attachments/assets/00c615ba-8d0e-4346-9c70-46b7493484e2" />

<img width="866" height="661" alt="image" src="https://github.com/user-attachments/assets/fd415e99-7be6-477a-b002-961e3b5d02c2" />

<img width="874" height="658" alt="image" src="https://github.com/user-attachments/assets/bad87860-6058-4fce-83f1-9c1dd7cf01f5" />

Wähle die kopierte ISO-Datei aus „C:\ClusterStorage\Volume1\Images“  
*Run the same script on Server3 as well.*

<img width="870" height="664" alt="image" src="https://github.com/user-attachments/assets/cb36a415-e357-4f91-ba46-262841b034a1" />

<img width="828" height="573" alt="image" src="https://github.com/user-attachments/assets/4bca14b4-7a16-4a0d-8cef-125902de2aa9" />

<img width="719" height="476" alt="image" src="https://github.com/user-attachments/assets/25329ef8-e149-489a-9116-c6e1098db9b9" />

Starte „VM01“ und installiere Windows Server 2022 Datacenter ohne Desktopdarstellung.  
*Start “VM01” and install Windows Server 2022 Datacenter without desktop display.*

<img width="786" height="592" alt="image" src="https://github.com/user-attachments/assets/c8b0ef13-2120-4bef-b8fe-7ccc0adf1bf7" />

Nach der Installation wechsle im „Failovercluster-Manager“ durch Rechtsklick auf die VM01 in die „Einstellungen“ der virtuellen Maschine und aktiviere bei der Netzwerkkarte unter „Erweiterte Features“ das Spoofing von MAC-Adressen.  
*After installation, switch to the “Settings” of the virtual machine in the “Failover Cluster Manager” by right-clicking on VM01 and activate MAC address spoofing for the network card under “Advanced Features.”*

<img width="895" height="519" alt="image" src="https://github.com/user-attachments/assets/1c0c6813-0e9c-4dd2-881f-1d7d55f902bd" />

<img width="861" height="466" alt="image" src="https://github.com/user-attachments/assets/530685fa-c9da-480c-a75d-01c4707c27c2" />

Anwenden + OK  
*Apply + OK*

Nach der Installation das Kennwort für Administrator auf „Kennw0rt!“ festlegen.  
*After installation, set the administrator password to “Password!”.*

<img width="918" height="714" alt="image" src="https://github.com/user-attachments/assets/193a02b7-cc90-4666-8eba-284c1c20f30a" />

<img width="979" height="501" alt="image" src="https://github.com/user-attachments/assets/c45b9591-915b-4ddd-8f2a-e5a02285f063" />

<img width="930" height="487" alt="image" src="https://github.com/user-attachments/assets/e62203b1-602f-493c-95a0-5c7b801bd69c" />

<img width="837" height="504" alt="image" src="https://github.com/user-attachments/assets/9334b040-3e71-4e48-aa48-22fed344c579" />

<img width="882" height="234" alt="image" src="https://github.com/user-attachments/assets/2e8e402e-4ddf-4047-b467-4e076ad096a1" />

<img width="1083" height="252" alt="image" src="https://github.com/user-attachments/assets/0a9368e6-7431-49b3-860b-c3366952c526" />

Nach Enter --> Wähle Punkt 8 --> Indexwert1 --> 2 DNS-Server festlegen:  
*After pressing Enter --> Select item 8 --> Index value 1 --> Set 2 DNS servers:*

<img width="868" height="547" alt="image" src="https://github.com/user-attachments/assets/5b0ce504-7bfc-4229-a75b-c573ca797939" />

Nach Enter:  
*After pressing Enter:*

<img width="1093" height="713" alt="image" src="https://github.com/user-attachments/assets/d4975d7e-3b3a-4942-904e-3fe101682344" />

<img width="1003" height="459" alt="image" src="https://github.com/user-attachments/assets/f843e4df-46b5-4da2-baec-10340bdc9ff9" />

Melde dich nach dem Neustart an --> Wähle in „sconfig“ die Option 15. --> Gib ein:  
*Log in after restarting --> Select option 15 in “sconfig”. --> Enter:*

<img width="1004" height="153" alt="image" src="https://github.com/user-attachments/assets/c2cf5c22-bbb8-47e7-858e-794c79de0688" />

```powershell
Enable-NetFirewallRule CoreNet-Diag-ICMP4-EchoRequest-IN-NoScope
```

In DC:

<img width="739" height="380" alt="image" src="https://github.com/user-attachments/assets/75ac390b-8dd1-4e9d-aa16-386ca453236c" />

<img width="1509" height="1004" alt="image" src="https://github.com/user-attachments/assets/003b3e49-0976-48bb-bf33-6b97ccd51b80" />

<img width="703" height="600" alt="image" src="https://github.com/user-attachments/assets/607d5779-03fa-4278-af79-daf40253a9a6" />

<img width="800" height="309" alt="image" src="https://github.com/user-attachments/assets/717f748a-66bb-49f2-b87f-3d4e8d7b3af8" />

```markdown
Erstellung einer VHD und Hochverfügbarmachung einer VM

In dieser Übung wird eine VHD mit einem Image versehen und eine virtuelle Maschine erstellt.  
Diese wird zunächst lokal auf **Server2** erzeugt und anschließend in das **Cluster Shared Volume** verschoben.  
Zum Abschluss wird die virtuelle Maschine hochverfügbar gemacht.

- Erstelle mit **Diskpart.exe** im Ordner **C:\isos** eine VHD mit dem Namen **pe.vhd** und einer dynamischen Größe von 4 GB.  
  Erstelle darin eine Partition, formatiere sie mit NTFS und weise der VHD den Laufwerksbuchstaben **P:** zu.

- Kopiere aus dem Remote-Lab aus dem Ordner **ISO\Tools** die Datei **winpe.iso** auf den Hyper-V-Host in den Ordner **C:\isos**.

- Binde die ISO-Datei ein und kopiere die Datei **boot.wim** aus der Windows-PE-ISO auf Laufwerk **H:**.

- Wende mit **DISM** das Image **boot.wim** auf die **pe.vhd** an und mache sie anschließend mit **bcdboot** im BIOS-Modus startfähig.

- Kopiere die **pe.vhd** auf **Server2** in den Ordner **C:\VMs**.

- Erstelle auf **Server2** mit dem **Hyper-V-Manager** (nicht mit dem Failover-Cluster-Manager) eine neue virtuelle Maschine namens **PE**.  
  Verwende **Generation 1**, 500 MB RAM und die **pe.vhd** als Festplatte.

- Verwende den virtuellen Switch **Externes Netzwerk**.

- Aktiviere das **MAC-Address-Spoofing** für die VM **PE** auf **Server2**.

- Prüfe, dass die VM von der **pe.vhd** bootet.

- Starte die virtuelle Maschine und initialisiere das Netzwerk mit dem Befehl **startnet**.

- Weise der VM mit **netsh** eine IP-Adresse zu.

- Führe einen Ping auf **DC** aus.

- Erstelle den Ordner **PE** im Pfad **C:\ClusterStorage\Volume1\**.

- Verschiebe die VM im **Hyper-V-Manager** auf **Server2** nach **C:\ClusterStorage\Volume1\PE**.

- Mache die virtuelle Maschine anschließend hochverfügbar.
```

```markdown
Creating a VHD and Making a VM Highly Available

In this exercise, a VHD is prepared with an image and used to create a virtual machine.  
The VM is first created locally on **Server2**, then moved to the **Cluster Shared Volume**, and finally configured as highly available.

- *Use **Diskpart.exe** in the folder **C:\isos** to create a dynamically expanding VHD named **pe.vhd** with a size of 4 GB. Create a partition inside the VHD, format it with NTFS, and assign the drive letter **P:**.*

- *Copy the file **winpe.iso** from the **ISO\Tools** folder in the Remote Lab to the Hyper-V host into **C:\isos**.*

- *Mount the ISO and copy the file **boot.wim** from the Windows PE ISO to drive **H:**.*

- *Apply the **boot.wim** image to **pe.vhd** using **DISM**, then make it bootable in BIOS mode using **bcdboot**.*

- *Copy **pe.vhd** to **Server2** into the folder **C:\VMs**.*

- *On **Server2**, create a new virtual machine named **PE** using **Hyper-V Manager** (not Failover Cluster Manager). Use **Generation 1**, 500 MB of RAM, and attach **pe.vhd** as the hard disk.*

- *Use the virtual switch **Externes Netzwerk**.*

- *Enable MAC Address Spoofing for **PE** on **Server2**.*

- *Verify that the VM boots from **pe.vhd**.*

- *Start the virtual machine and initialize networking using the **startnet** command.*

- *Assign an IP address using **netsh**.*

- *Ping **DC**.*

- *Create a folder named **PE** in **C:\ClusterStorage\Volume1\**.*

- *Move the VM using **Hyper-V Manager** on **Server2** to **C:\ClusterStorage\Volume1\PE**.*

- *Finally, configure the virtual machine as highly available.*
```

### Erstellen der VHD-Datei mit DiskPart  
### *Creating the VHD file with DiskPart*

<img width="865" height="464" alt="image" src="https://github.com/user-attachments/assets/b0392529-2b1d-489c-942d-a9166da1eaed" />

<img width="749" height="895" alt="image" src="https://github.com/user-attachments/assets/4ae643de-5138-4957-81d0-3cc374efd493" />

### Kopieren der winpe.iso und kopieren der Datei Boot.wim  
### *Copy winpe.iso and copy the Boot.wim file*

<img width="725" height="619" alt="image" src="https://github.com/user-attachments/assets/d9079eea-be34-40dd-aba7-c5dc3cc247c3" />

<img width="525" height="467" alt="image" src="https://github.com/user-attachments/assets/ef01a7b6-6916-4691-8237-4fa89ca76f39" />

<img width="454" height="474" alt="image" src="https://github.com/user-attachments/assets/61553701-3d4d-4943-8635-370e01c273df" />

<img width="519" height="354" alt="image" src="https://github.com/user-attachments/assets/d2b9a1e8-2724-4627-9995-ef74d69412c6" />

Klicke mit der rechten Maustaste auf das bereitgestellte Laufwerk und wähle „Auswerfen“.  
*Right-click on the mounted drive and select “Eject.”*


```powershell
dism /apply-image /imagefile:H:\boot.wim /index:1 /applydir:p:
```

<img width="783" height="337" alt="image" src="https://github.com/user-attachments/assets/1ac1ec0e-ad45-4576-ae80-86bc2facfe65" />

<img width="962" height="302" alt="image" src="https://github.com/user-attachments/assets/e7690687-81d2-4b36-b070-43419fa9dbd0" />

### Bootfähigkeit der VHD einrichten  
### *Set up bootability of the VHD*

```powershell
bcdboot P:\windows /s P: /f BIOS /l de-de
```

<img width="1006" height="135" alt="image" src="https://github.com/user-attachments/assets/800ee585-e116-4d21-8d64-9405ad5588d7" />

<img width="981" height="431" alt="image" src="https://github.com/user-attachments/assets/f4c688b1-ff19-49f0-982e-5b7917bfecca" />

Schließe die Eingabeaufforderung --> Wechsle in die andere Eingabeaufforderung mit dem aktiv geöffneten diskpart:  
*Close the command prompt --> Switch to the other command prompt with diskpart actively open:*

<img width="902" height="94" alt="image" src="https://github.com/user-attachments/assets/43009474-e267-4218-97b9-b0ae77e0b6d1" />

Gib 2 x „Exit“ ein und schließe die Eingabeaufforderung.  
*Type “Exit” twice and close the command prompt.*

### Kopieren der VHD zu „Server2“  
### *Copying the VHD to “Server2”*

Server2 --> Ordner VMS auf dem Laufwerk C: erstellen:  
*Server2 --> Create folder VMS on drive C:*

<img width="1005" height="646" alt="image" src="https://github.com/user-attachments/assets/32fda057-f196-468d-ba47-3035919da613" />

### Erstellen der virtuellen Maschine „PE“  
### *Creating the “PE” virtual machine*

<img width="871" height="665" alt="image" src="https://github.com/user-attachments/assets/d1824fce-8cae-4fa6-93e2-2e7024232e5d" />

### Konfigurieren der virtuellen Maschine  
### *Configuring the virtual machine*

<img width="892" height="629" alt="image" src="https://github.com/user-attachments/assets/28c6a2a2-22c8-4d1f-ac04-abad4dc9053d" />

<img width="851" height="526" alt="image" src="https://github.com/user-attachments/assets/658719cb-4bed-4a0c-ae59-3e3dbf187047" />

<img width="885" height="404" alt="image" src="https://github.com/user-attachments/assets/7ae8d90c-aa7c-42d3-9c59-ed8a7315ced4" />

Anwenden + OK  
*Apply + OK*

<img width="697" height="472" alt="image" src="https://github.com/user-attachments/assets/a32b3691-e40d-4133-9c9f-8c6bd1ddcd42" />

<img width="1020" height="614" alt="image" src="https://github.com/user-attachments/assets/bdde967c-0ed8-4d90-8a75-dec215520207" />

```powershell
Netsh interface ipv4 set address ethernet static 192.168.1.161 255.255.255.0
```

```powershell
Ipconfig
```

```powershell
Ping 192.168.1.200
```

Die VM läuft und kann den Server „DC“ erreichen.  
*The VM is running and can reach the “DC” server.*

### Verschieben der VM in das Cluster Shared Volume  
### *Moving the VM to the cluster shared volume*

Server2

<img width="983" height="348" alt="image" src="https://github.com/user-attachments/assets/6720de5f-db26-4c30-a3ac-2917daa75e90" />

<img width="403" height="535" alt="image" src="https://github.com/user-attachments/assets/df02f21c-abf7-47f0-9818-48240942560c" />

<img width="903" height="659" alt="image" src="https://github.com/user-attachments/assets/e1109a32-e081-4917-bc24-e8634e3fc0ea" />

<img width="760" height="651" alt="image" src="https://github.com/user-attachments/assets/1317fbd4-8249-4ef5-8597-246f1851a3ec" />

<img width="743" height="663" alt="image" src="https://github.com/user-attachments/assets/798563fb-bb17-4ead-81a2-653cc4bdff69" />

<img width="718" height="661" alt="image" src="https://github.com/user-attachments/assets/f3587fbc-1672-4975-8de7-86389dc4a784" />

<img width="905" height="657" alt="image" src="https://github.com/user-attachments/assets/5089c0d0-d134-45ca-a69f-460714c6305b" />

<img width="888" height="591" alt="image" src="https://github.com/user-attachments/assets/af094384-758e-4bf6-83f6-f977d44f616e" />

### Hochverfügbarkeit einrichten  
### *Set up high availability*

<img width="488" height="607" alt="image" src="https://github.com/user-attachments/assets/3d223d9e-1a5c-43ba-8ba8-b86c781e8bb3" />

<img width="827" height="567" alt="image" src="https://github.com/user-attachments/assets/61ba0a5e-b43a-43b8-bfe0-863435013828" />

<img width="831" height="567" alt="image" src="https://github.com/user-attachments/assets/e02a5ceb-d8b5-4e2b-98d6-15d23d32924c" />

<img width="827" height="570" alt="image" src="https://github.com/user-attachments/assets/1efe4203-b3a5-40df-b823-429d6d151432" />

<img width="825" height="572" alt="image" src="https://github.com/user-attachments/assets/3589a62d-2334-4f89-a3c3-11ee8b282471" />

<img width="966" height="408" alt="image" src="https://github.com/user-attachments/assets/53eca466-9990-4058-8767-1f3c795e1e81" />

<img width="1222" height="624" alt="image" src="https://github.com/user-attachments/assets/d7648f81-8ca9-4bc0-bf10-d7fa2cd071d8" />

<img width="1062" height="457" alt="image" src="https://github.com/user-attachments/assets/55e15f74-ccc5-40b9-b5d5-6108d7a13b55" />


















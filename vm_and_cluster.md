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




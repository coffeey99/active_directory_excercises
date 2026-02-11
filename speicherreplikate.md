Bevor wir starten, setzen wir alle VMs auf BASIS-Ad Prüfpunkt zurück. Dafür wir haben wir eine Skript geschrieben.  
*Before we start, we reset all VMs to the BASIS-Ad checkpoint. We wrote a script for this purpose.*


```powershell
Restore-VMSnapshot -VMName dc -Name Basis-AD
# Restore-VMSnapshot -VMName dc2 -Name Basis-AD
Restore-VMSnapshot -VMName Server1 -Name Basis-AD
Restore-VMSnapshot -VMName Server2 -Name Basis-AD
Restore-VMSnapshot -VMName Server3 -Name Basis-AD
Restore-VMSnapshot -VMName w11 -Name Basis-AD

# Start-VM -Name "Name_der_VM"
#Start-VM -Name dc,server1,server2,server3,w11

#stop-VM -Name dc,server1,server2,server3,w11
```


## Erstellung von dynamischen virtuellen Festplatten auf Server2 und Server3 mit PowerShell

Erstellung von jeweils zwei dynamischen virtuellen Festplatten **„srdata.vhdx“** und **„srlog.vhdx“** mit einer Größe von jeweils 100 GB auf **Server2** und **Server3** unter Verwendung von PowerShell im GFN-RLAB.

---

## Initialisierung und Konfiguration der Datenträger auf Server2

Initialisierung der neu erstellten Festplatten als **GPT-Datenträger** auf Server2.  
Anschließende vollständige Partitionierung sowie Formatierung mit **NTFS** unter Verwendung von PowerShell.  
Zuweisung der Laufwerksbuchstaben **S:** (Daten) und **T:** (Log).  
Optional kann hierfür ergänzend die Datenträgerverwaltung verwendet werden.

---

## Durchführung der identischen Datenträgerkonfiguration auf Server3

Durchführung derselben Initialisierung, Partitionierung, Formatierung und Laufwerksbuchstabenzuweisung auf **Server3** mit identischen Einstellungen.

---

## Installation der Dateiserver-Rolle und des Speicherreplikats

Installation der Rolle **Dateiserver (FS-FileServer)** sowie der Rollenkomponente **Storage Replica** einschließlich des Features **Speicherreplikat** auf beiden Servern mittels PowerShell.  
Anschließender Neustart beider Systeme.

---

## Durchführung eines Performance- und Verfügbarkeitstests

Ausführung eines zehnminütigen Performance- und Verfügbarkeitstests mit dem Befehl **Test-SRTopology**.  
Während des Tests sollen parallel möglichst viele Daten auf das Laufwerk **S:** kopiert und wieder gelöscht werden.

---

## Einrichtung einer Speicherreplikation von Server2 zu Server3

Konfiguration einer Speicherreplikation mit der Replikationsrichtung **Server2 → Server3**.  
Dabei wird **S:** als Datenlaufwerk und **T:** als Loglaufwerk verwendet.

---

## Überprüfung der Zugriffsberechtigung auf das replizierte Laufwerk

Kontrolle, dass das Datenlaufwerk **S:** auf **Server3** im replizierten Zustand nicht direkt geöffnet werden kann.

---

## Änderung der Replikationsrichtung

Anpassung der Replikationsrichtung auf **Server3 → Server2** und anschließende Funktionsprüfung.

---

## Beendigung der Speicherreplikation und Bereinigung

Beendigung der eingerichteten Speicherreplikation sowie Entfernung der zugehörigen Datenträgerkonfiguration.

<br>
<br>

## *Creation of Dynamic Virtual Hard Disks on Server2 and Server3 Using PowerShell*

*Creation of two dynamic virtual hard disks named **“srdata.vhdx”** and **“srlog.vhdx”**, each with a size of 100 GB, on **Server2** and **Server3** using PowerShell within the GFN-RLAB.*

---

## *Initialization and Configuration of Disks on Server2*

*Initialization of the newly created virtual disks as **GPT disks** on Server2.  
Complete partitioning and formatting using the **NTFS** file system via PowerShell.  
Assignment of drive letters **S:** (data) and **T:** (log).  
Optionally, Disk Management may be used to support the configuration.*

---

## *Applying the Same Disk Configuration on Server3*

*Execution of the identical initialization, partitioning, formatting, and drive letter assignment on **Server3** using the same configuration settings.*

---

## *Installation of the File Server Role and Storage Replica*

*Installation of the **File Server (FS-FileServer)** role and the **Storage Replica** role service including the required feature on both servers using PowerShell.  
Subsequent restart of both systems.*

---

## *Execution of a Performance and Availability Test*

*Execution of a ten-minute performance and availability test using the **Test-SRTopology** command.  
During the test, a large amount of data should continuously be copied to and deleted from drive **S:**.*

---

## *Configuration of Storage Replication from Server2 to Server3*

*Configuration of a storage replication partnership with the replication direction **Server2 → Server3**.  
Drive **S:** is used as the data volume and drive **T:** as the log volume.*

---

## *Verification of Access Restrictions on the Replicated Volume*

*Verification that the data volume **S:** on **Server3** cannot be accessed directly while replication is active.*

---

## *Changing the Replication Direction*

*Modification of the replication direction to **Server3 → Server2** followed by validation of functionality.*

---

## *Termination of Storage Replication and Cleanup*

*Removal of the configured storage replication and deletion of the associated disk configuration.*


## Erstellung von dynamischen virtuellen Festplatten auf Server2 und Server3 mit PowerShell  
## *Creation of Dynamic Virtual Hard Disks on Server2 and Server3 Using PowerShell*

### Erstellen der virtuellen Festplatten  
### *### Erstellen der virtuellen Festplatten*

---

## Erstellung von dynamischen virtuellen Festplatten auf Server2 und Server3 mit PowerShell  
## *Creation of Dynamic Virtual Hard Disks on Server2 and Server3 Using PowerShell*

### Erstellen der virtuellen Festplatten  
### *### Erstellen der virtuellen Festplatten*

Wechsle zu Host-PC und öffne die PowerShell-ISE als Administrator --> Kopiere die folgenden Befehle in den Editor --> Skript ausführen.  
*Switch to the host PC and open PowerShell ISE as an administrator --> Copy the following commands into the editor --> Run the script.*

<img width="899" height="590" alt="image" src="https://github.com/user-attachments/assets/b4784f23-77d3-4c77-8526-f5cba1ebec8e" />

```powershell
# Erstellen der Disks mit Powershell
New-VHD -Path H:\vm\Server2\srdata.vhdx -SizeBytes 100GB -Dynamic
New-VHD -Path H:\vm\Server2\srlog.vhdx -SizeBytes 100GB -Dynamic
New-VHD -Path H:\vm\Server3\srdata.vhdx -SizeBytes 100GB -Dynamic
New-VHD -Path H:\vm\Server3\srlog.vhdx -SizeBytes 100GB -Dynamic
# Einbinden der Festplatten (DataDist & LogDisk) in die VMs Server2 und Server3
$dd2 = @{
vmname = "server2"
ControllerType="SCSI"
Path="H:\vm\Server2\srdata.vhdx"
}
Add-VMHardDiskDrive @dd2
$ld2 = @{
vmname = "server2"
ControllerType="SCSI"
Path="H:\vm\Server2\srlog.vhdx"
}
Add-VMHardDiskDrive @ld2
$dd3 = @{
vmname = "server3"
ControllerType="SCSI"
Path="H:\vm\Server3\srdata.vhdx"
}
Add-VMHardDiskDrive @dd3
$ld3 = @{
vmname = "server3"
ControllerType="SCSI"
Path="H:\vm\Server3\srlog.vhdx"
}
Add-VMHardDiskDrive @ld3
```

Durch das Skript hast du insgesamt 4 virtuelle Festplatten erstellt und an Server2 und Server3 gebunden.  
*Using the script, you have created a total of 4 virtual hard disks and attached them to Server2 and Server3.*

<img width="321" height="481" alt="image" src="https://github.com/user-attachments/assets/3c0b4da9-6ee2-41e8-a100-ccb36ea5c663" />

<img width="314" height="482" alt="image" src="https://github.com/user-attachments/assets/543ad776-34d0-4022-854b-816f3d353906" />

### Konfigurieren der Festplatten in Server2 und Server3
### *Configuring the hard drives in Server2 and Server3*

Server2 --> PowerShell-ISE als Administrator ausführen --> Skript ausführen:  
*Server2 --> Run PowerShell-ISE as administrator --> Run script:*

<img width="661" height="664" alt="image" src="https://github.com/user-attachments/assets/af027e53-61eb-445e-b549-76e60c9a5352" />

```powershell
# Alle physischen Platten identifizieren,
# die für Pooling verfügbar sind (CanPool = $true)
$physicalDisks = Get-PhysicalDisk |
Where-Object { $_.CanPool -eq $true } |
Select-Object -ExpandProperty UniqueId
# Laufwerksbuchstaben und Labels definieren
$driveMappings = @{
 1 = @{ Letter = 'S'; Label = 'SR-DATA' }
 2 = @{ Letter = 'T'; Label = 'SR-LOG' }
}
$counter = 1
foreach ($diskId in $physicalDisks) {
 # Disk initialisieren mit GPT-Partitionstyp
 Initialize-Disk -UniqueId $diskId -PartitionStyle GPT
 # Auswahl des Laufwerksbuchstabens und Labels passend zum Zähler
 switch ($counter) {
 1 {
 $letter = $driveMappings[1].Letter
 $label = $driveMappings[1].Label
 $counter = 2
 }
 2 {
 $letter = $driveMappings[2].Letter
 $label = $driveMappings[2].Label
 # Falls mehr als 2 Platten, ggf. Anpassungen nötig
 }
 default {
 Write-Warning "Mehr als zwei Platten — Logik erweitern."
 break
 }
 }
 # Neue Partition mit vollem Platz, Laufwerksbuchstaben zuweisen
 New-Partition -DiskId $diskId `
 -UseMaximumSize `
 -DriveLetter $letter
 # Volume formatieren mit NTFS und gewähltem Label / Beschriftung
 Format-Volume -DriveLetter $letter `
 -FileSystem NTFS `
 -NewFileSystemLabel $label
}
```

<img width="1223" height="446" alt="image" src="https://github.com/user-attachments/assets/c49011d3-e6e1-40fe-8eec-035395a03d64" />

<img width="1223" height="446" alt="image" src="https://github.com/user-attachments/assets/c02eb1ae-f284-499e-9949-d9cc8bc54efc" />

Schließe die ISE.  
*Close ISE*

Durch Ausführung des Skripts wurden die Festplatten konfiguriert (partitioniert, formatiert etc.).  
*The hard disks were configured (partitioned, formatted, etc.) by executing the script.*

<img width="689" height="235" alt="image" src="https://github.com/user-attachments/assets/5c714b21-5c5a-440c-ae26-070e25c15276" />

Server3 --> PowerShell-ISE als Administrator ausführen --> dieselbe Skript wie oben ausführen.  
*Server3 --> Run PowerShell-ISE as administrator --> Run the same script as above.*

<img width="888" height="522" alt="image" src="https://github.com/user-attachments/assets/48806f9e-120a-4add-b073-518e588eb55a" />

### Installation der Rolle und des Features  
### *Installing the role and feature*

Server3 --> PowerShell-ISE als Administrator ausführen --> Skript ausführen:  
*Server3 --> Run PowerShell-ISE as administrator --> Run script:*

<img width="842" height="357" alt="image" src="https://github.com/user-attachments/assets/1c14dca0-718a-4764-b9b5-0aaf578dfbd7" />

```powershell
# Installation der Rolle Dateiserver und des Features Speicherreplikat
$param = @{
name = "fs-fileserver”,”storage-replica"
IncludeManagementTools = $true
restart = $true
}
# Installation remote auf Server2
Install-WindowsFeature @param -ComputerName Server2
$Nachricht = "Remote-Installation auf Server2 ist abgeschlossen. "
$Nachricht += "Lokale Installation wird gestartet. "
$Nachricht += "Anschließend startet der Server3 neu!"
Write-Host $Nachricht -ForegroundColor Yellow
Read-Host "Drücke eine beliebige Taste um fortzufahren"
# Installation lokal auf Server3
Install-WindowsFeature @param 
````

Warte die anschließend die Neustarts ab.  
*Wait for the system to restart.*

<img width="1080" height="150" alt="image" src="https://github.com/user-attachments/assets/6ad626af-2455-4765-9ad7-c864fc70af01" />

Einrichten eines Speicherreplikats (Server2 → Server3)  
*Setting up a storage replica (Server2 → Server3)*

Server2 --> PowerShell-ISE als Administrator ausführen --> Skript ausführen:  
*Server2 --> Run PowerShell-ISE as administrator --> Run script:*

Einrichten eines Speicherreplikats (Server2 → Server3)  
*Setting up a storage replica (Server2 → Server3)*

Server2 --> PowerShell-ISE als Administrator ausführen --> Skript ausführen:  
*Server2 --> Run PowerShell-ISE as administrator --> Run script:*

<img width="650" height="474" alt="image" src="https://github.com/user-attachments/assets/c3ea2995-2c0d-4aa5-b578-377a7e1d90ad" />

```powershell
$optionen = @{
SourceComputerName = "server2"
SourceVolumeName = "S:"
SourceLogVolumeName = "T:"
DestinationComputerName = "server3"
DestinationVolumeName = "S:"
DestinationLogVolumeName = "T:"
SourceRGName = "SRG01"
DestinationRGName = "DRG01"
LogSizeInBytes = 64GB
}
New-SRPartnership @optionen
```

<img width="640" height="148" alt="image" src="https://github.com/user-attachments/assets/8bfc6049-de73-403f-91f1-894635271593" />

###Prüfen der Verfügbarkeit des Datenlaufwerks auf dem Zielserver  
### *Checking the availability of the data drive on the target server*

Server3

<img width="904" height="528" alt="image" src="https://github.com/user-attachments/assets/b73cac5f-4545-449c-9712-80065d8a2641" />

Hinweis: Bei der Speicherreplikation wird nur in eine Richtung repliziert. Im Ziel dürfen die Daten nicht verändert werden. Daher wird der Zugriff unterbunden. Müssen auf dem Zielserver die Daten verwendet werden, muss die Replikation umgekehrt werden, nachdem die ursprüngliche Replikation beendet wurde. Ab Server 2019 kann man einen „Testfailover“ mit PowerShell durchführen, dafür benötigst du aber eine zusätzliche leere Festplatte
*Note: With storage replication, replication only occurs in one direction. The data must not be changed at the destination. Therefore, access is prevented. If the data needs to be used on the target server, the replication must be reversed after the original replication has been completed. Starting with Server 2019, you can perform a “test failover” with PowerShell, but you will need an additional empty hard drive*

### Replikationsrichtung ändern (Server3 → Server2) und prüfen  
### *Change replication direction (Server3 → Server2) and check*

Server2 --> PowerShell-ISE als Administrator ausführen --> Skript ausführen:  
*Server2 --> Run PowerShell-ISE as administrator --> Run script:*

<img width="778" height="400" alt="image" src="https://github.com/user-attachments/assets/1fad9c52-a2a6-48e8-b495-f6178919e324" />

```powershell
# Replikationsinfos
Get-SRGroup
Write-Host "Achte auf Property 'IsPrimary'" -F Yellow
Get-SRPartnership
Write-Host "DestinationComputerName = Server3" -F Yellow
Write-Host "Sie ändern nun die Replikationsrichtung" -f Yellow
# Umdrehen der Replikationsrichtung
Set-SRPartnership -NewSourceComputerName Server3 `
 -SourceRGName DRG01 `
 -DestinationComputerName Server2 `
 -DestinationRGName SRG01
# Auslesen der neuen Eigenschaften
$NeuesZiel = (Get-SRPartnership).DestinationComputerName
$NeueQuelle = (Get-SRPartnership).SourceComputerName
Write-Host "Neue Quelle = $NeueQuelle, Neues Ziel = $NeuesZiel" -f Cyan
```
<img width="862" height="642" alt="image" src="https://github.com/user-attachments/assets/355068f8-a4e3-4c7a-b716-77c136edca05" />

<img width="1398" height="185" alt="image" src="https://github.com/user-attachments/assets/1b6e5372-a4c1-4931-8339-82edbe04f29c" />

Überprüfe mit dem Windows-Explorer, ob das Laufwerk S: nur auf Server3 (und nicht auf  Server2) verfügbar ist.  
*Use Windows Explorer to check whether drive S: is only available on Server3 (and not on Server2).*

<img width="998" height="431" alt="image" src="https://github.com/user-attachments/assets/b0daf620-a2ec-483d-b930-54589cb65360" />

<img width="925" height="516" alt="image" src="https://github.com/user-attachments/assets/c306d0e5-9082-4d88-b58f-b273cd36cbda" />

Hinweis: Überprüfe immer den Replikationsstatus mit Get-SRGroup und Get-SRPartnership, bevor du Änderungen vornimmst.  
*Note: Always check the replication status with Get-SRGroup and Get-SRPartnership before making any changes.*

### Beende die Speicherreplikation und lösche die dazugehörigen Datenträger!  
### *Stop storage replication and delete the associated data carriers!*

Server2 --> PowerShell-ISE als Administrator ausführen --> Skript ausführen:  
*Server2 --> Run PowerShell-ISE as administrator --> Run script:*

Hinweis: Überprüfe immer den Replikationsstatus mit Get-SRGroup und Get-SRPartnership, bevor du Änderungen vornimmst.  
*Note: Always check the replication status with Get-SRGroup and Get-SRPartnership before making any changes.*

### Beende die Speicherreplikation und lösche die dazugehörigen Datenträger!  
### *Stop storage replication and delete the associated data carriers!*

Server2 --> PowerShell-ISE als Administrator ausführen --> Skript ausführen:  
*Server2 --> Run PowerShell-ISE as administrator --> Run script:*

<img width="1023" height="561" alt="image" src="https://github.com/user-attachments/assets/73bf0b65-084d-4905-a8fc-f1eb197450fd" />

```powershell
$args = @{
SourceComputerName = "server3"
DestinationComputerName = "server2"
SourceRGName = "DRG01"
DestinationRGName = "SRG01"
confirm = $false
}
Remove-SRPartnership @args
Remove-SRGroup -Name "SRG01"
Remove-SRGroup -Name "DRG01" -ComputerName Server3
Clear-SRMetadata -AllPartitions
Clear-SRMetadata -AllLogs
```

```powershell
$args = @{
SourceComputerName = "server3"
DestinationComputerName = "server2"
SourceRGName = "DRG01"
DestinationRGName = "SRG01"
confirm = $false
}
Remove-SRPartnership @args
Remove-SRGroup -Name "SRG01"
Remove-SRGroup -Name "DRG01" -ComputerName Server3
Clear-SRMetadata -AllPartitions
Clear-SRMetadata -AllLogs
```

<img width="688" height="462" alt="image" src="https://github.com/user-attachments/assets/ac20a1a0-4717-4bc3-a30c-59fcce64bda9" />

<img width="673" height="146" alt="image" src="https://github.com/user-attachments/assets/23983060-6b12-4fff-8578-8bf7f5b90570" />

<img width="672" height="142" alt="image" src="https://github.com/user-attachments/assets/5402f3b2-36ac-4246-a565-0780003ecfdc" />

Wechsle zu Host-PC und öffne die PowerShell-ISE als Administrator --> Führe folgendes Skript aus, um die virtuellen Datenträger zu löschen  *Switch to the host PC and open PowerShell ISE as an administrator --> *Run the following script to delete the virtual disks:*

<img width="603" height="334" alt="image" src="https://github.com/user-attachments/assets/b8e9badc-112b-44bb-8a07-dffea3f468fc" />

```powershell
for ($i=2;$i -le 3; $i++){
# VHD/AVHD aus Einstellungen der VMs entfernen
Get-VMHardDiskDrive -VMName server$i |
where path -like "*sr*" |
Remove-VMHardDiskDrive
# Löschen der virtuellen Datentäger
Get-Item -Path "H:\vm\Server$i\sr*" |
Remove-Item
}
```
Vergewissere dich, dass die erstellten virtuellen Festplatten gelöscht und ihre Zuweisungen zu Server2 und Server3 aufgehoben wurden.  
*Ensure that the virtual hard disks created have been deleted and their assignments to Server2 and Server3 have been removed.*

<img width="995" height="494" alt="image" src="https://github.com/user-attachments/assets/d6637039-f109-42cd-b79a-6682af0c64fd" />

<img width="908" height="512" alt="image" src="https://github.com/user-attachments/assets/8fb57aa4-22b6-433b-97b9-e339bb1013dd" />






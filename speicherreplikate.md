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




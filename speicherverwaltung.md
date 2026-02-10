# Serverpool  
# *Server pool*

Erstellung von Festplatten (.vhdx) auf Server2 auf verschiedenen Wegen (PowerShell, Diskpart und die Datenträgerverwaltung), um 6 
virtuelle Datenträger im Ordner „H:\vm“ zu erstellen. Die Datenträger sollen jeweils 80 GB groß werden, dynamisch erweiterbar  
*Creation of hard disks (.vhdx) on Server2 in various ways (PowerShell, Diskpart, and Disk Management) to create 5 
virtual disks in the “H:\vm” folder. Each disk should be 80 GB in size and dynamically expandable.*

### Methode1:  
### *Methode 1:*

<img width="585" height="329" alt="image" src="https://github.com/user-attachments/assets/5615035c-9ea4-4f4c-babd-0f29899cf552" />

<img width="894" height="402" alt="image" src="https://github.com/user-attachments/assets/8b2e0315-7b35-4d94-89c3-3e9df825a3bf" />

<img width="880" height="184" alt="image" src="https://github.com/user-attachments/assets/e0543998-7e3d-4b6f-9ce8-28b3eb3e4f74" />

<img width="882" height="662" alt="image" src="https://github.com/user-attachments/assets/3584f106-1fd0-49b2-9c84-191627c0a244" />

<img width="874" height="660" alt="image" src="https://github.com/user-attachments/assets/5ef2ad97-b463-444d-910d-e46642a71ccb" />

<img width="868" height="663" alt="image" src="https://github.com/user-attachments/assets/01548cc3-78f8-4d3d-b48c-38be2f1c3ccb" />

<img width="875" height="663" alt="image" src="https://github.com/user-attachments/assets/a4c02ac7-c485-4d7f-bc81-0a9f91774644" />

<img width="882" height="658" alt="image" src="https://github.com/user-attachments/assets/407de998-37fe-4a12-a359-f825f9964ea9" />

<img width="880" height="225" alt="image" src="https://github.com/user-attachments/assets/9480ace1-e919-407e-a8b0-3c4e26a40a89" />

Übernehmen + OK  
*Apply + OK*

### Methode 2:  
### *Methode 2:*

Öffne auf dem Hyper-V-Host die Powershell-ISE als Administrator:  
*Open Powershell ISE as an administrator on the Hyper-V host:*

<img width="702" height="791" alt="image" src="https://github.com/user-attachments/assets/eb61a574-1c73-4f43-a32d-efc01784ae9d" />

<img width="554" height="424" alt="image" src="https://github.com/user-attachments/assets/298fe41d-a417-4481-82bd-aa644b286d98" />

<img width="868" height="526" alt="image" src="https://github.com/user-attachments/assets/0fa94ad2-c10c-4013-9d41-d4ce4ce5f253" />

```powershell
$VMName   = "Server2"
$DiskPath = "H:\VM"
$DiskSize = 80GB

foreach ($i in 1..3) {
    $VHDX = "$DiskPath\diskname$i.vhdx"

    New-VHD -Path $VHDX -SizeBytes $DiskSize -Dynamic

    Add-VMHardDiskDrive `
        -VMName $VMName `
        -ControllerType SCSI `
        -Path $VHDX
}
```
<img width="623" height="656" alt="image" src="https://github.com/user-attachments/assets/6294aac4-31cb-4b69-bf30-b220f83ca901" />

### Methode 3:  
### *Methode 3:*

Öffne auf dem „Hyper-V-Host“ die Eingabeaufforderung als Administrator.  
*Open the command prompt as administrator on the “Hyper-V host.*

<img width="618" height="751" alt="image" src="https://github.com/user-attachments/assets/d8afb895-e1e6-47e6-be66-701360c1186f" />

<img width="562" height="344" alt="image" src="https://github.com/user-attachments/assets/7941a87a-1508-4170-a2dc-142045142152" />

<img width="786" height="351" alt="image" src="https://github.com/user-attachments/assets/de0e4bdc-2dd8-415a-a847-b25982c56328" />

```powershell
create vdisk file=“H:\VM\pooldisk4.vhdx” Maximum=81920 Type=Expandable

create vdisk file=“H:\VM\pooldisk5.vhdx” Maximum=81920 Type=Expandable

exit
```

Die Festplatten sind jetzt erstellen. Diese müssen aber noch gemountet werden (später)  
*The hard disks have now been created. However, they still need to be mounted (later).*

### Methode 4:  
### *Methode 4:*

Rechte Mausklick auf Windows Start und Datenträgerverwaltung auswählen  
*Right-click on Windows Start and select Disk Management.*

<img width="405" height="320" alt="image" src="https://github.com/user-attachments/assets/a3f2ac77-e190-4fa1-a1d1-08c6e0e4004a" />

<img width="865" height="363" alt="image" src="https://github.com/user-attachments/assets/4212c447-4028-460f-a5c9-ed756b6c18b1" />

<img width="465" height="582" alt="image" src="https://github.com/user-attachments/assets/79e1eb2e-0c6f-47f0-ace7-8cdbe84009ba" />

Wechsle in der Datenträgerverwaltung in den unteren Bereich und klicke mit der rechten Maustaste auf das graue Feld mit der Beschriftung „Datenträger 1“.  
*In Disk Management, go to the lower section and right-click on the gray field labeled “Disk 1.”*

<img width="657" height="239" alt="image" src="https://github.com/user-attachments/assets/768b5e05-5e00-492e-adbc-24c78e749824" />

Jetzt haben wir mehrere Festplatten, die nicht mit Server2 verbunden sind. Diese müssen wir nun verbinden  
*Now we have several hard drives that are not connected to Server2. We now need to connect them.*

<img width="759" height="101" alt="image" src="https://github.com/user-attachments/assets/0940bfe6-a962-4a5c-a9f7-98031c07456c" />

Vorher:  
*before:*

<img width="280" height="528" alt="image" src="https://github.com/user-attachments/assets/f701fdf9-ff54-4dcf-ae10-c93240cc06e3" />

Nachher:  
*after:*

<img width="283" height="645" alt="image" src="https://github.com/user-attachments/assets/00bf05af-f64c-49c6-826e-40a00e891364" />

<img width="859" height="327" alt="image" src="https://github.com/user-attachments/assets/2bc93250-51e1-4bfc-a466-ae6d4ec6a6e2" />

<img width="1222" height="168" alt="image" src="https://github.com/user-attachments/assets/802d7b6d-d7b8-48d8-9230-85dd47ec70a6" />

```powershell
# Achtung: Falls der Pfad „H:\vm“ nicht zutrifft, 
# musst du diesen anpassen. Erledigt? Starte nun das Skript.
# Der Schleifenkopf sorgt für 5 Ausführungen des Schleifenkörpers
for ($i=4 ; $i -le 6 ; $i++){ # Beginn der For-Schleife
# Normaleweise fängt die Schleife bei 1. Pooldisk1 bis 3 sind aber schon eingebunden.
 # Name der Festplatten in Variable $P speichern
 $P = "H:\VM\Pooldisk$i.vhdx"
 # Verwendung des “Splattings” zur übersichtlichen
 # Parameterübergabe. Du kannst auch eine andere Variante wählen
 $Args = @{ # Beginn der Hashtable
 VMName = "Server2"
 Path = $P
 Controllertype = "SCSI"
 } # Ende der Hashtable
 # Anwenden des CMDlets, Festplatten werden der VM hinzugefügt
 Add-VMHardDiskDrive @Args
 } # Ende der For-Schleife
# Überprüfung: Ausgabe aller verbundenen Platten von Server2
Get-VMHardDiskDrive -VMName "Server2
```

##Erstellen des Speicherpools „Pool1“ mit Servermanager  
##*Creating the storage pool “Pool1” with Server Manager*

### Methode 1:  
### *Methode 2:*

Wechsle in die VM „Server2“ als „gfnlab\administrator“ ins Servermanager-Dashboard.  
*Switch to the “Server2” VM as “gfnlab\administrator” in the Server Manager dashboard.*

<img width="296" height="334" alt="image" src="https://github.com/user-attachments/assets/b346c25d-b5bd-4c55-9bb9-5541c293cc5e" />

Rechtsklick unter „Speicherpools“ ins freie Feld und wähle „Neuer Speicherpool“.  
*Right-click in the empty field under “Storage pools” and select “New storage pool.”*

<img width="730" height="348" alt="image" src="https://github.com/user-attachments/assets/69b919d5-1a8c-4c30-8d04-3fd4650a0eaa" />

<img width="1117" height="733" alt="image" src="https://github.com/user-attachments/assets/770882a0-d131-41e1-b219-789af35dc45d" />

<img width="711" height="640" alt="image" src="https://github.com/user-attachments/assets/efa6f28c-8396-4d6f-a89d-e35660d0015c" />

<img width="723" height="624" alt="image" src="https://github.com/user-attachments/assets/0988fe21-cc82-40c2-bd66-0dd52cc6d18b" />

<img width="705" height="572" alt="image" src="https://github.com/user-attachments/assets/9e9d95aa-3c68-4e81-bd8c-b630a752ce06" />

Danach schließen.  
*Close after finished.*

### Methode 2:  
### *Methode 2:*

Öffne in „Server2“ die PowerShell-ISE als Administrator.  
*Open PowerShell ISE as an administrator in “Server2.”*

Finde heraus, welche Festplatten in einen Pool aufgenommen werden können:  
*Find out which hard drives can be added to a pool:*

<img width="456" height="141" alt="image" src="https://github.com/user-attachments/assets/84dcdf23-0a62-4ba1-aef6-8726dff6b179" />

```powershell
Get-PhysicalDisk `
| Sort-Object -Property DeviceID `
| Format-Table DeviceID, CanPool 
```

<img width="496" height="226" alt="image" src="https://github.com/user-attachments/assets/fbc495b5-1019-444f-98a8-8f5284c8dd06" />

Wir wählen LUN 4 und 6.  
*we selct LUN 4 and 6*

<img width="881" height="248" alt="image" src="https://github.com/user-attachments/assets/71157c2a-b90d-4494-bfaf-0ef10d699ce0" />

```powershell
$disks = Get-PhysicalDisk | Where-Object {$_.DeviceID -eq 5 -or $_.DeviceID -eq 4}

$Args = @{
 StorageSubSystemFriendlyName = "Windows Storage on Server2"
 FriendlyName = "Pool2"
 PhysicalDisks = $disks
 }
New-StoragePool @Args 
```

<img width="1077" height="224" alt="image" src="https://github.com/user-attachments/assets/e895d353-645b-466a-a233-b627d581419d" />

<img width="964" height="434" alt="image" src="https://github.com/user-attachments/assets/ee98982a-2bdd-4d23-80b0-ffedced21225" />

## Spiegelung von Poolserver  
## *Mirroring pool servers*

Erstelle mittels „Servermanager“ einen gespiegelten Speicherplatz „SP1“ im "Pool1". Verwende dabei den gesamten Speicherplatz des Pools als feste Speichergröße (Fixed)!  
*Use “Server Manager” to create a mirrored storage space ‘SP1’ in “Pool1.” Use the entire storage space of the pool as the fixed storage size!*

### Methode 1:  
### *Methode 1:*

<img width="671" height="458" alt="image" src="https://github.com/user-attachments/assets/a01698b5-c064-4339-892f-87faa4a704b2" />

<img width="855" height="390" alt="image" src="https://github.com/user-attachments/assets/e38bddd3-eacd-486b-be9c-bb4c94ff556d" />

<img width="938" height="692" alt="image" src="https://github.com/user-attachments/assets/f8a6875b-d19c-41ff-8d14-176ace5fb739" />

<img width="950" height="692" alt="image" src="https://github.com/user-attachments/assets/ad21999d-dfc7-4e9d-a88d-c3f29e5ab93e" />

<img width="929" height="688" alt="image" src="https://github.com/user-attachments/assets/76408796-ca93-4bfd-b605-e3b89d869ae1" />

<img width="940" height="692" alt="image" src="https://github.com/user-attachments/assets/0b66eb62-e299-48b6-babf-06baa2345dc2" />

<img width="937" height="689" alt="image" src="https://github.com/user-attachments/assets/b0068e8b-6271-4fe7-943d-2ab163c9ad1b" />

<img width="935" height="689" alt="image" src="https://github.com/user-attachments/assets/e6d798f5-bf72-4b5a-8cb5-a0f4fa8988fe" />

<img width="940" height="694" alt="image" src="https://github.com/user-attachments/assets/03e15f19-221d-42bd-9818-7df8b705c0b2" />

<img width="688" height="573" alt="image" src="https://github.com/user-attachments/assets/029ffc77-6de2-4eae-9b13-19682e95c641" />

Brich den Assistenten für neue Volumes ab, wenn er startet. (Diese werden später erstellt.)  
*Cancel the wizard for new volumes when it starts. (These will be created later.)*

<img width="944" height="689" alt="image" src="https://github.com/user-attachments/assets/157bcf9e-b5ad-43ef-bc10-741cb52fe2ba" />

Markiere im Speicherpool den „Pool1“. Unter „Virtuelle Datenträger“ wird „SP1“ angezeigt  
*Select “Pool1” in the storage pool. ‘SP1’ is displayed under “Virtual disks.”*

<img width="459" height="714" alt="image" src="https://github.com/user-attachments/assets/5211bc74-d4ae-41c1-bd0e-2c5510089344" />

### Methode 2:  
### *Methode 2:*

Verwende PowerShell, um auf „Pool2“ den gespiegelten Speicherplatz „SP2“ zu erstellen. Nutze ebenfalls den gesamten Speicherplatz mit Speichergröße als „fest“.  
*Use PowerShell to create the mirrored storage space “SP2” on “Pool2.” Also use all storage space with storage size as “fixed.”*

Öffne auf „Server2“ die PowerShell-ISE als Administrator.  
*Open PowerShell ISE as an administrator on “Server2.”*

```powershell
$Params = @{
 StoragePoolFriendlyName="Pool2"
 FriendlyName="SP2"
 UseMaximumSize=$true
 ProvisioningType="Fixed"
 ResiliencySettingName="Mirror"
 }
New-VirtualDisk @Params
```

<img width="1088" height="669" alt="image" src="https://github.com/user-attachments/assets/c8fc9e28-e599-41e4-9b25-5f8e72823db2" />

<img width="498" height="741" alt="image" src="https://github.com/user-attachments/assets/be8db649-9c50-45dd-b1ff-4c6751e8e171" />

Erstelle via „Servermanager“ das NTFS-Volumen namens „SP1Volume“ auf „SP1“ und nutze die gesamte Größe. Vergib den Buchstaben „S“.  
*Use Server Manager to create the NTFS volume named “SP1Volume” on “SP1” and use the entire size. Assign the letter “S.”*

### Methode 1:  
### *Methode 1:*

<img width="447" height="747" alt="image" src="https://github.com/user-attachments/assets/48da0859-eb3f-4961-a8c9-62f5236d90c4" />

<img width="936" height="695" alt="image" src="https://github.com/user-attachments/assets/922b8f87-1e34-4d50-a028-abaf3dee18b0" />

<img width="942" height="697" alt="image" src="https://github.com/user-attachments/assets/aeaabd1d-8ff3-4d35-ad37-3dfcced537e5" />

<img width="941" height="693" alt="image" src="https://github.com/user-attachments/assets/0b013baa-2aaf-41fe-8c18-2479bc288a31" />

<img width="941" height="694" alt="image" src="https://github.com/user-attachments/assets/97cc9ea4-70c2-4315-9bba-dd60c62a3a8f" />

<img width="936" height="690" alt="image" src="https://github.com/user-attachments/assets/43700887-97de-422e-8259-87bbcee48459" />

<img width="942" height="693" alt="image" src="https://github.com/user-attachments/assets/51dcf6ee-2307-4272-a047-7f37bd531dca" />

<img width="691" height="574" alt="image" src="https://github.com/user-attachments/assets/0c8eb6ff-c5c8-4efa-9538-910ab68a1ce8" />

### Methode 2:  
### *Methode 2:*

Verwende nun PowerShell, für „SP2Volume“. Formatiere den gesamten Speicherplatz zu NTFS und vergib „T“ als Laufwerksbuchstaben.  
*Now use PowerShell for “SP2Volume.” Format the entire storage space to NTFS and assign “T” as the drive letter.*

Öffne auf „Server2“ die PowerShell-ISE als Administrator.  
*Open PowerShell ISE as an administrator on “Server2.”*

<img width="716" height="469" alt="image" src="https://github.com/user-attachments/assets/cf0a78bc-f800-407e-b661-ec820e6e9a5f" />

```powershell
# Gib den Namen der zu erstellenden Partition an.
$volumeName = "SP2Volume"
#####################################################################
######## Erklärung / Reihenfolge der Aktionen: ######################
#####################################################################
# Get-Disk -> Liste alle vorhandenen Disks auf.
# Where-Object... -> zeige nur Disks ohne Partitionstabelle an und
# welche zudem den Namen sp2 haben.
# Inizialize-Disk... -> erstelle eine Partitionstabelle.
# Parameter -PassThru -> gib das *fertig bearbeitete* Object (sobald
# GPT erstellt wurde) an das nächste CMDlet weiter. Ohne "-PassThru" 
# hätte Inizialize-Disk das veränderte Diskobject nicht
# an die Pipeline zurückgegeben und "New-Volume" hätte kein
# Datenträgerobjekt, auf dem es ein Volume erstellen könnte.
# „New-Volume“ erstellt das eigentliche Volume. 
# FYI:
# Jede Partition ist ein logischer Speicherbereich eines Datenträgers
# und kann somit auch als Volume betrachtet werden. 
# Aber nicht jedes Volume ist auf eine einzige Partition beschränkt!
# Für weitere Fragen wende dich an die Lehrkraft.
Get-Disk | Where-Object { $_.OperationalStatus -eq "Offline" `
 -and $_.FriendlyName -like "*sp2*" } |
 Initialize-Disk -PartitionStyle GPT -PassThru |
 New-Volume -FileSystem NTFS `
 -DriveLetter T `
 -FriendlyName $volumeName
```

Ergebnis: Es sind zwei neue Volumen, „S:“ und „T:“ vorhanden. Aktualisiere ggf. die Anzeige.  
*Result: Two new volumes, “S:” and “T:”, are now available. Refresh the display if necessary.*

<img width="932" height="405" alt="image" src="https://github.com/user-attachments/assets/7711f667-d3b4-499a-b32d-e96db98fc03a" />

Reparatur eines defekten Speicherplatzes  
*Repairing a defective storage location*

Entferne im Hyper-V-Manager in den Einstellungen von „Server2“ eine der dem StoragePool „Pool2“ zugeordneten Festplatten. Wir können in Fenster "Physische Datenträger" sehen, welche LUNs (Logical Unit Number) die Festplatten haben, die zu Pool2 gehören:  
*In Hyper-V Manager, remove one of the hard disks assigned to the “Pool2” storage pool from the settings for “Server2.” In the “Physical Disks” window, we can see which LUNs (Logical Unit Number) belong to the hard disks that are part of Pool2:*

<img width="1203" height="798" alt="image" src="https://github.com/user-attachments/assets/1ecec505-60d1-471c-a9b4-1acfb1d92c2c" />

In diesem Fall sind es LUN 5 und LUN 7. Wir entfernen LUN 7.  
*In this case, it is LUN 5 and LUN 7. We remove LUN 7.*

<img width="1043" height="838" alt="image" src="https://github.com/user-attachments/assets/24ee65d7-693c-425f-9394-d738f501577e" />

Übernehmen + OK  
*Apply + OK*

Es erscheint nun ein Warnsymbol vor „Pool2“, vor „SP2“ und einer der Festplatten.  
*A warning symbol now appears in front of “Pool2,” “SP2,” and one of the hard drives.*

<img width="964" height="787" alt="image" src="https://github.com/user-attachments/assets/b486bd98-1d7e-483d-9776-9028221b5c83" />

<img width="608" height="467" alt="image" src="https://github.com/user-attachments/assets/cb81a5bf-13f9-44d1-a13c-86577e739601" />

<img width="845" height="393" alt="image" src="https://github.com/user-attachments/assets/2ceff9ef-06d2-4d7b-b73d-88230dda2a7e" />

<img width="706" height="362" alt="image" src="https://github.com/user-attachments/assets/5d608129-7532-44b7-9c94-bb1a17fdd40b" />

<img width="517" height="383" alt="image" src="https://github.com/user-attachments/assets/56f372e4-f908-4c49-bb83-8fb16d4dff17" />

<img width="509" height="354" alt="image" src="https://github.com/user-attachments/assets/7b69cf37-cb0b-4424-b4e9-76d8a99bc383" />

<img width="510" height="260" alt="image" src="https://github.com/user-attachments/assets/c5de3b9b-7732-417b-99f5-80bbdf6096b9" />

Bestätige alle Meldungen. Aktualisiere am Ende die Ansicht des Servermanagers. Das gelbe Warnzeichen sollte nun verschwunden sein. Du hast den Speicherpool wieder repariert!  
*Confirm all messages. Finally, refresh the Server Manager view. The yellow warning sign should now have disappeared. You have repaired the storage pool!*

<img width="983" height="782" alt="image" src="https://github.com/user-attachments/assets/62a3979c-92fd-4386-820c-0cc891ff73bf" />


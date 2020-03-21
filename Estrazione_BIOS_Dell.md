La procedura di estrazione del BIOS su motherboard DELL è leggermente diversa dalle altre marche. 

Questa procedura è stata testata con successo su Mac OS e Windows. Su Linux ci sono dei problemi di compilazione di PFSExtractor. Seguiranno sviluppi

### Requisiti

- Pacchetto di aggiornamento del BIOS (scaricabile dal sito del produttore)
- [Dell Extract hdr.py](https://github.com/theopolis/uefi-firmware-parser/blob/master/scripts/contrib/dell_extract_hdr.py)


### Step 1: estrarre il BIOS in formato HDR dall'EXE

Da una shell scrivere

`python2.7 dell_extract_hdr.py BIOS_UPGRADE.exe`

sostituendo "BIOS_UPGRADE.EXE" con il path del pacchetto di aggiornamento del BIOS.

L'output di questo comando sarà un file con estensione `.hdr`

### Step 2: scompattare il file in formato HDR

Utilizzando PFSExtractor, compilabile con g++ per MacOS, o scaricando l'eseguibile dalla sezione Release del repository GitHub, è possibile scompattare il file con estensione `.hdr` ottenuto dallo **Step 1**.

Da una shell scrivere

`<PATH PFSExtractor> <PATH FILE.HDR>`

sostituendo a `<PATH PFSExtractor>` il path all'eseguibile di PFSExtractor, e a `PATH FILE.HDR` il path al file ottenuto dallo **Step 1**.

L'output sarà una cartella contenente una serie di file, tra cui un file con estensione `.payload`

### Step 3: trovare l'offset del CFG Lock

Con UEFITool aprire il file con estensione `.payload` e premendo `Ctrl + F`, se su Windows, oppure `Command + F`, se su Mac OS, selezionando come criterio di ricerca `Text` e cercando `CFG Lock`.

Nel pannello inferiore uscirà un messaggio simile a 

`Unicode text "CFG Lock" found in PE32 image section at offset XXYYZZ`

Facendo doppio click su questo messaggio e cliccando col tasto destro del mouse su `PE32 image section`, selezionare `Extract as is` e salvare il file con estensione `.bin`.

### Step 4: convertire il file .bin in .txt

Utilizzando `IFRExtract` è possibile convertire il file con estensione `.bin` in un file con estensione `.txt`. 

Da shell scrivere

`<PATH IFRExtract> <PATH FILE.BIN> setup.txt`

### Step 5: trovare l'offset del CFG Lock

Aprire con un editor di testo il file precedentemente convertito e cercare `CFG Lock`

Se presente, il risultato di ricerca sarà simile a 

`CFG Lock, VarStoreInfo (VarOffset/VarName): 0xXYZ`

`0xXYZ` è l'offset che corrisponde alla locazione di memoria dove risiede il valore booleano `CFG Lock`.

Impostando questa variabile, o meglio la locazione di memoria a `0x00` il `CFG Lock` viene disabilitato.

### Step 6: impostare l'offset a 0x00

Utilizzando una shell modificata di GRUB, è possibile modificare il valore della variabile precedentemente trovata.

Avviare la shell modificata di GRUB tramite shell UEFI:

- da Clover avviare la shell UEFI e navigare nel filesystem (tramite cd e ls) fino a trovare la partizione dove risiede la shell modificata di GRUB.
- da OpenCore avviare la bootentry corrispondente a `modGRUBShell.efi` (assicurarsi di averlo inserito nella sezione dedicata del config.plist del bootloader)

**Esempio**

`FS0:\EFI\EFI\CLOVER\tools\modGRUBShell.efi` avvia la shell modificata di GRUB che si trova nella partizione `FS0:\`. 

Una volta giunti dinanzi alla shell infine scrivere:

`setup_var 0xXYZ 0x00` dove `0xXYZ` corrisponde all'offset trovato precedentemente.

Una volta terminata la procedura spegnere il PC e riavviarlo.

Infine disabilitare dal `config.plist`:

- **Clover**: `KernelPM` e/o `KernelXCPM` (potrebbe essere necessario solo il primo tra i due)
- **OpenCore**: `AppleCpuPmCfgLock` e/o `AppleXcpmCfgLock` (stesso discorso)

in quanto queste patch sono altamente instabili e potrebbero portare a riavvii improvvisi della macchina.

**Credits**

[Khronokernel](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/msr-lock) per la guida sullo sblocco dell'MSR 0xE2

[theopolis](https://github.com/theopolis) per lo script in Python

[Longsoft](https://github.com/Longsoft) per PFSExtractor

[Il forum MacOS86](https://macos86.it) per aver messo a disposizione il loro repository

[dreamwhite](https://github.com/dreamwhite) per il testing su diversi laptop e aggiunte di diverse parti della guida originale

[A23SS4NDRO](https://www.macos86.it/profile/996-a23ss4ndro/) per aver scritto questa guida




## Introduzione

Ecco come estrarre il file del bios dei laptop Dell dal file .exe (purtroppo finora sono stati gli unici che sono stato in grado di testare): questo metodo potrebbe essere funzionale anche per altri laptop, ma serve testare su altre piattaforme per avere conferma.
 
## Step 1

Scarica l'ultimo BIOS dal sito Web del tuo fornitore (anche se non stai utilizzando l'ultimo aggiornamento del BIOS, l'offset del blocco CFG è lo stesso, ma considera l'aggiornamento all'ultima versione del BIOS prima di sbloccare il registro MSR e fai attenzione che questo sblocco dovrà essere ripetuto se il BIOS viene aggiornato **dopo** aver sbloccato il registro).


## Step 2
  
  Nella tua macchina Windows/Mac/Linux, esegui [questo script](https://raw.githubusercontent.com/macos86/Guide_Forum_IT/master/ExtractDellBIOS.py) salvando il *raw* su un file di testo o scaricandolo in formato .py e lanciare Python - (questo metodo potrebbe funzionare anche con le ultime versioni, come python3 poiché è retrocompatibile) - aprite un terminale e digitate:
  
  `python2.7 biosextract.py bios_name.exe`
  
  Sostituite bios_name.exe with con il nome del vostro BIOS, nel mio caso ad esempio: 
  
  `python2.7 biosextract.py XPS_9350_1.12.2.exe`
  
  Se l'operazione ha esito positivo, lo script genererà un file denominato in questo modo:
  
  `Decompressed data written to XPS_9350_1.12.2.exe_decompressed.hdr`
  
## Step 3
  
  Ora procuratevi [questo file](https://github.com/LongSoft/PFSExtractor/releases/download/0.1.0/PFSExtractor_0.1.0.zip) della LongSoft and run this in a Windows machine (Non l'ho testato su Unix, quindi è preferibile un ambiente Windows nativo per eseguire questo script oppure si utilizzi g++ in un ambiente UNIX Like). Aprite il Command prompt, navigate sulla directory corretta (cd \Users\<your_username>\path\to\PFSextractor.exe) ed eseguite questo comando:
  
  `PFSextractor.exe bios_name.exe_decompressed.hdr`
  
  Nel mio caso:
  
  `PFSextractor.exe XPS_9350_1.12.2.exe_decompressed.hdr`

## Step 4
  
 Aprite la cartella creata da PFSextractor.exe e ordinate i file per dimensioni, il file `.payload` più grande dovrebbe essere il vostro BIOS UEFI.
 
## Step 5

Ora potete tornare alla guida di [khronokernel](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/msr-lock) che tratta dello sblocco CFG (MSR 0xE2), dico questo qualora aveste dovuto fare tutto questo giro per estrarre il BIOS UEFI dal sito web del vostro produttore :D


**Credits**

[JimboBobB](https://forums.mydigitallife.net/members/jimbobobb.361587/) per lo script in python

[Longsoft](https://github.com/Longsoft) per PFSExtractor

[Il forum MacOS86](https://macos86.it) per aver messo a disposizione il loro repository

[A23SS4NDRO](https://www.macos86.it/profile/996-a23ss4ndro/) per aver scritto questa guida


  

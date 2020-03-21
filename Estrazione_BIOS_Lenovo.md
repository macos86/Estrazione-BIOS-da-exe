## Guida Lenovo

**Premessa: NON Saltare**

Non sono responsabile per laptop briccati, schede USB / SD morte, guerre termonucleari o per il vostro licenziamento perché il laptop "*non funziona*" o il processo di sblocco CFG non sono riusciti. Fai qualche ricerca se hai dubbi sulle funzionalità incluse in questo metodo, di solito se ottieni un'immagine BIOS valida (UEFITool certifica la sua integrità se è in grado di navigare all'interno dell'immagine BIOS) non dovresti avere problemi con l'offset che otterrai dopo aver preso dal .exe l'immagine del BIOS.

Detto questo proseguiamo con la guida, non ho inventato nulla, ho solo sistemato in un unico metodo questa estrazione per BIOS Lenovo, potrebbe funzionare anche per altri portatili, ma serve del testing aggiuntivo.

Per estrarre un BIOS Lenovo utilizzeremo "`innoextract`", un programma che è stato sviluppato sia per GNU/Linux che per Windows.

## Di che cosa hai bisongo

- BIOS del vostro portatile in formato .exe ([ad esempio questo](https://download.lenovo.com/consumer/mobiles/bycn31ww.exe))
- Innoextract va preso [qua](https://github.com/dscharrer/innoextract/releases).
- Pazienza
- Spirito di debug
- la mente fresca

Scarica il file, unzippalo e mettilo su Linux o Windows scaricando la versione corretta ovviamente

## Eseguire i comandi

Dirigetevi nella directory di innoextract:

`cd path/to/innoextract (oppure innoextact.exe)`

Eseguite il file con il .exe come bersaglio:

`innoextract -e 6quj18us.exe `


Esempio output:

`Extracting "Lenovo BIOS Update Utility" - setup data version 6.0.0 (unicode)
 "app/BYCN31WW.exe" - overwritten
 "app/BYCN31WW.exe" - Done.`

**Questo vi tirera fuori dentro la cartella app (o una nuova cartella anche core-xxx diversa da docs e license) un .exe o i file già estratti.**

Cercate per file .fl1 o .fl2 quelle sono già immagini bios. (trascinatele quindi su UEFITool per verificare se sono immagini 
valide, di solito i thinkpad rientrano in questa sottocategoria)

Se invece avete file con estensione diversa, tra cui alcuni bios con estensione *.fb* allora estraete ancora quel file con 7zip o con 

`7z x <your_file.fb>`

## Note finali

Qualora estraeste quel file .fb, dovete andarvi a cercare il file più grande anche tra quelli nascosti, di solito si chiama `.reloc`, le immagini bios sono più grandi di 15MB di solito quando sono scompattate/leggibili da UEFITool. 

Potete ora proseguire con la guida di Khronokernel, usando UEFITool per testare le immagini BIOS pure che avete estratto

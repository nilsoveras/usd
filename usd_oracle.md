# Oppsett av Oracle databaseservere for ordbøkene
## Servere, databaser

Det går bare en database på hver server.

server       |besrivelse
-------------|----------
amalie.uib.no|Den primære produksjonsbasen
sigrid.uib.no|Amalie replikeres hit og vil overta hvis amalie feiler
hauge.uib.no |Utviklingsdatabase
dahl.uib.no  |Testdatabase (ikke satt i funksjon ennå)

### Endringer
Noen endringer blir gjort rett i prod, f.eks. legge en ny fil til et tablespace.
Andre endringer er tenkt utført i DEV for så å testes i TEST.
TEST basen vil bli klonet fra PROD og programendringer fra DEV lagt til før testen.
Siste fase er å legge endringene til i PROD.

## Script,cron
Faste jobber blir kjørt via crontab av oracle brukeren.  
Scriptene ligger i katalogen **/home/oracle/script**
  
### Dette er crontab  på amalie:
```
    MAILTO="nils.overas@uib.no"
    05 20 * * 1,2,3,4,5 . /home/oracle/.bash_profile ; cron_daily_backup.sh
    MAILTO="ordbokene@uib.no"
    02 00 * * 2,3,4,5 .   /home/oracle/.bash_profile ; db.sh natt1
    02 00 * * 1 .         /home/oracle/.bash_profile ; db.sh natt0
```

Hver natt mandag-fredag rett etter midnatt kjører det en jobb som danner grunnlag for håndordbøkene på nett.
Etter denne jobben kjøres full RMAN backup hver mandag, og incrementell backup de andre dagene.
Hver hverdag om kvelden kjøres datapump eksport av de viktigste schemaene til /Orabak.

## Katalogstruktur oracle databaser
### Rotkataloger
* /Oraarc
    Arkivlogger og RMAN backup filer.  
    En fullstendig database kan restores fra denne katalogen.  
    Blir vanligvis kopiert til tape, men det er ikke satt opp for ordbøkene.  
    Restore i en krisesituasjon henter data fra denne katalogen.  
    Blir bestemt av parameteren: db_recovery_file_dest="/Oraarc"  

* /Orabak  
    Bakup filer laget av datapump.  
    Det kan tas backup av ett og ett schema.  
    Dette gjøres etter behov.
    
* /Oradata  
    Selve databasefilene.  
    Disse filene oppdateres konstant,  
    og en backup av denne katalogen ville ha vært inkonsistent.  

* /Orasoft  
    Oracle software.  
    Inneholder ORACLE_BASE og ORACLE_HOME  
    Logger og tracer, bla. alertloggen  
  
  
### Eksempel på underkataloger og filer
Katalogene har underkataloger for hver database som kjøres.  
amalie med ORACLE_SID=eddng er brukt som eksempel  
    
* /Oradata/eddng  
    databasefiler  

* /Orasoft/oracle  
    ORACLE_BASE  

* /Orasoft/oracle/product/ora12102  
    ORACLE_HOME  

* /Orasoft/oracle/product/ora12102/dbs/initeddng.ora  
    inneholder oppstarts-parametre for databasen f.eks.  
    db_recovery_file_dest.  

* /Orasoft/oracle/diag/rdbms/eddng/eddng/trace/alert_eddng.log 
    alertlog  

* /Orasoft/oracle/diag/tnslsnr/amalie/list_ora12/trace/list_ora12.log
    listener log  
    
* /Oraarc/EDDNG/archivelog/2016_09_08  
    Løpende arkivloggfiler  
    
* /Oraarc/EDDNG/controlfile  
    Backup av kontrollfil  
    
* /Oraarc/EDDNG/backupset/2016_09_04  
    RMAN bakupfiler  
    Danner gunnlag for restore, sammen med arkivloggfilene og kontrollfilen  

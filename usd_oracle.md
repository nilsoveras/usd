#Oppsett av Oracle databaseservere for ordbøkene
##Servere, roller
* amalie.uib.no
    Den primære produksjonsbasen
* sigrid.uib.no
    Amalie replikeres hit og vil overta hvis amalie feiler

* hauge.uib.no
    Utviklingsdatabase

##Katalogstruktur oracle databaser
###Rotkataloger
* /Oraarc
    Arkivlogger og RMAN backup filer.
    En fullstendig database kan restores fra denne katalogen.
    Blir vanligvis kopiert til tape, men det er ikke satt opp for ordbøkene.
    Restore i en krisesituasjon henter data fra denne katalogen.
    Blir bestemt av parameteren
    db_recovery_file_dest="/Oraarc"
* /Orabak
    Bakup filer laget av datapump.
    Det kan tas backup av ett og ett schema.
    Dette gjøres etter behov.
* /Oradata
    Selve databasefilene.
    Disse filene oppdateres konstant,
    og en backup av denne katalogen ville ha vært inkonsistent.
* /Oralog
    Logger og tracer.
    Bla. den viktige alertloggen
    Blir bestemt av parameteren
    diagnostic_dest="/Oralog"
* /Orasoft
    Oracle software.
    Inneholder ORACLE_BASE og ORACLE_HOME

###Underkataloger
* /Ora???
    katalogene har underkataloger for hver database som kjøres.
    ⁞amalie med ORACLE_SID#eddng er brukt som eksempel

####Eksempel på underkataloger og filer
* /Oraarc/EDDNG/archivelog/2016_09_08
    Løpende arkivloggfiler
* /Oraarc/EDDNG/controlfile
    Backup av kontrollfil
* /Oraarc/EDDNG/backupset/2016_09_04
    RMAN bakupfiler
    Danner gunnlag for restore, sammen med arkivloggfilene og kontrollfilen
* /Oralog/diag/rdbms/eddng/eddng/trace
    alertlog
* /Oradata/eddng
    databasefiler
* /Orasoft/oracle
    ORACLE_BASE
* /Orasoft/oracle/product/ora12102
    ORACLE_HOME
* /Orasoft/oracle/product/ora12102/dbs/initeddng.ora
    inneholder oppstarts-parametre for databasen f. eks
    db_recovery_file_dest og diagnostic_dest

# LinuxMaturita
Shrnutí učiva 2. až 4. řočníku na SPŠE V Úžlabině
Konfigurace Ubuntu
# 2. Ročník
## Užitečné příkazy
* ```ls -lah```
  * vypsání všech souborů ve složce (včetně skrytých) v lidském formátu (megabajty, gigabajty, ... místo bajtů)
* ```rm -r [složka/soubor]```
  * vymazání souboru nebo složky včetně jejího obsahu
* ```grep [hledaný_řetězec]```
  * nalezení řádků obsahující určitý řetězec (text)
  * např. ```ls -l | grep txt``` vypíše obsach složky a vyfiltruje 
* ```head -n [počet_řádků]```
  * vypsání prvních n řádků
  * např. ```cat file.txt | head -n 8``` vypíše prvních 8 řádků souboru file.txt
## Práce se soubory
### Vytvoření složky
* příkaz ```mkdir [parametry] [cesta]```
* např. ```mkdir -p nova_slozka```
* v případě nutnosti vytvoření více složek zároveň je možno použít složené závorky
* ```mkdir -p ulice/{Praha,Berlín,Varšava}```
* ```mkdir -p ulice/{Praha/{A,B},Berlín/{C,D},Varšava/{E,F}}```
### Vytvoření souborů
* příkaz ```touch [soubor]```
* Příkaz touch vytvoří soubor pokud již neexistuje a následně změní jeho datum poslední úpravy
* např. ```touch file.txt``` nebo ```touch folder/{file1.txt,file2.txt}```

### Směrování výstupu do souborů
* 2 základní výstupy aplikací
  * stdout - běžný výstup (1)
  * stderr - chybový výstup (2)
* vstup
  * stdin - běžný vstup (nejčastěji od uživatele)
* přesměrování stdout aplikace do či ze souboru
  * Přepsání souboru
    * ```echo "Hello World" > out.txt```
  * Vložení výstupu za existující obsah na nový řádek
    *   ```echo "Hello World >> out.txt"```
* Použití obsahu souboru jako vstup
  * ```cowsay < in.txt```
### Roury - Pipes
* směrování standartního výstupu z jedné aplikace jako vstup uživatele (jako by to psal na klávesnici)
* použití ```[příkaz_ze_kterého_používáme_výstup] | [příkaz_který_použije_vstup_z_roury]```
* např. ```ls -l | grep txt``` - vypíše soubory ve složce, které mají v názvu txt

### Hromadná tvorba souborů s obsahem
* podobná syntaxe jako u tvorby více složek zároveň
* vhodné použít příkaz tee, který umožní psaní do více souborů zároveň
* ```echo [text_uvnitř_souborů] | tee [cesta k souborům]```
* např ```echo "vnitrek souboru" | tee slozkaA/{Slozka1,Slozka2}/file.txt``` Vytvoří 2 soubory file.txt jejichž obsahem bude text "vnitrek souboru"
* nebo ```echo "Oblíbená ulice" | tee ulice/{Bratislava/{Bachova,Česká,Dobrovičova},Praha/{Adamova,Dačická,Eberlova},Warzawa/{Abrahama,Balzaka,Chlebowa}}/desc.txt```
### Archivace
* nápověda ```man tar```
* Příkaz tar
  * Slouží pro práci s archivy
  * Podporuje práci se soubory *.tar, *.tar.gz, *.tar.bz, *.tar.xz, ...
  * Parametry
    * ```-c``` Vytvoří archiv
    * ```-x``` Extrahuje archiv
    * ```-t``` Vypíše obsah archivu
    * ```-f [název archivu]``` specifikuje název archivu, se kterým se pracuje
    * ```-z``` použije kompresi gzip (*.tar.gz)
    * ```-j``` použije kompresi bzip2 (*.tar.bz2)
    * ```-J``` použije kompresi xz (*.tar.xz)
    * ```-C``` změní složku, ve které se bude operace vykonávat (časté použití určení, kde se bude extrahovat archiv)
  * Tvorba archivu bez komprese
    * ```tar -cf [název archivu] [soubory] ```
    * Vytvoření archivu archive.tar ze složky files
      *  ```tar -cf archive.tar files```
    * Vytvoření archivu archive.tar ze složky files s použitím absolutní cesty
      * ```tar -cf archive.tar ~/files```
  * Tvorba archivu s kompresí xz
    * ```tar -cJf [název archivu] [soubory] ```
    * Vytvoření archivu archive.tar.xz ze složky files
      * ```tar -cJf archive.tar.xz files```
  * Zobrazení obsahu archivu
    * ```tar -tf [název archivu]```
    * není nutné specifikovat metodu komprese
    * např. ```tar -tf archive.tar.bz2```
  * Extrahování archivu
    * ```tar -xf [název_archivu] -C [cesta_ke_složce_kde_se_bude_extrahovat]```
    * např. Extrahování archivu archive.tar.xz do složky vystup ```tar -xf archive.tar.xz -C vystup```


## Správa uživatelů
* nutné privilegované oprávnění -> před příkazy dávat ```sudo```
### Soubory s konfiguracemi uživatelů
* ```/etc/passwd``` obsahuje základní informace (jméno, domovský adresář, výchozí shell, způsob ověření, ...)
* ```/etc/shadow``` obsahuje hesla
* ```/etc/group``` obsahuje informace o skupinách a členství ve skupinách
### Tvorba uživatelů
* příkaz ```useradd [parametry] [uživatel]```
* parametry:
  * ```-b [base_dir]``` Specifikuje lokaci domovského adresáře (base_dir)
  * ```-m``` Vytvoří domovskou složku když neexistuje
  * ```-g [group_name]``` Určí primární skupinu
  * ```-G [group_name1[,group_name2[,group_nameN]]]``` Přidá uživatele do dalších skupin
  * ```-s [lokace_shellu]``` Změna uživatelovo shellu po přihlášení (login_shellu)
  * ```-e [YYYY-MM-DD]``` Nastavení data vypšení účtu (expiredate)
* v celku ```useradd -b [base_dir] -m -g [group_name] -G [group_name1[,group_name2[,group_nameN]]] -s [lokace_shellu] -e [YYYY-MM-DD] [uživatel]```
### Změna hesla uživatele
* příkaz ```passwd```
* použití pro změnu hesla ```passwd [uživatel]``` a následné zadání hesla
* použití pro expiraci hesla (uživatel si bude muset heslo změnit po přihlášení) ```passwd -e [uživatel]```
### Úprava uživatele
* příkaz ```usermod [parametry] [uzivatel]```
* parametry stejné jako u ```useradd```
* přidání uživatele do skupiny ```usermod -aG [skupina] [uživatel]```
  * např. ```usermod -aG sudo vaclav``` přidá uživatele vaclav do skupiny sudo
### smazání uživatele
* ```userdel [uživatel]```
### Tvorba skupin
* příkaz ```groupadd [název skupiny]```
* např. ```groupadd pracovnici``` vytvoří skupinu pracovnici
### Mazání skupin
* příkaz ```groupdel [skupina]```
### hrmoadná tvorba uživatelů pomocí for loop
* je nutné mít pŘedem vytvořeené všechny skupiny
* nahraďte všechny hodnoty v hranatých závorkách
* smažte řádky které nejsou potřeba
```
for user in [uživatel1] [uživatel2] [uživatel3]
  do
  sudo useradd -m -d [cesta k domovským adresářům]/$user -s /bin/bash -g [primární skupina] -G [další skupiny] $user # přidá uživatele
  sudo chmod 700 [cesta k domovským adresářům]/$user #změní oprávnění k domovskému adresáři uživatele
  echo -e "$user\n$user" | sudo passwd $user # změní uživatelovo heslo jko jeho jméno
  sudo passwd -e $user # donutí uživatele změnit heslo po přihlášení
  sudo chage -E $(date -d '+183 days' '+%Y-%m-%d') $user # nastavý vypršení uživatele po 183 dnech (půl roce)
  done
```
* např.
```
for user in alojs pepina rodic knihovnik
do
   sudo useradd -m -d /knihovna/$user -s /bin/bash -g knizni_klub -G cdrom,dip,lxd $user
   sudo chmod 700 /knihovna/$user
   echo -e "$user\n$user" | sudo passwd $user
   sudo passwd -e $user
   sudo chage -E $(date -d '+183 days' '+%Y-%m-%d') $user
done
```

## Správa oprávnění k souborům a složkám
### Změna oprávnění
* příkaz ```chmod [práva] [soubor]```
* např. ```chmod 750 file.txt``` Dá vlastníkovi souboru všechna oprávnění, skupině povolí číst a spouštět soubory a ostatním zakáže všechny operace
### Změna vlastnictví
* příkaz ```chown [uživatel[:[skupina]]] file.txt```
* např. ```chown worker:developers file.txt``` Změní vlastníka souboru na uživatele worker a vlastnickou skupinu na skupinu developers pro soubor file.txt

## 3. Ročník
### Tvorba oddílů na disku
* příkaz ```fdisk```
* ```fdisk -l``` vypíše dostupné disky
* ```fdisk [adresa_disku]``` otevře nástroj tvorby disku pro daný disk např. ```fdisk /dev/sdb```
* Možnosti fdisk:
  * ```m``` zobrazí nápovědu
  * ```g``` vytvoří novou prázdnou GPT tabulku (vhodné při první práci s diskem, jinak jsou dostupné maximálně 4 oddíly)
  * ```n``` vytvoří nový oddíl
    * během tvorby se bude definovat
      * číslo oddílu (možno nechat výchozí, když není zadáno jinak)
      * první sektor (možno nechat výchozí, když není zadáno jinak)
      * poslední sektor (vhodné zadávat velikost pomocí +)
        * např. +150M (oddíl bude mít velikost 150MiB)
  * ```d``` smaže oddíl
* formátování
  * normální oddíl
    * příkaz ```mkfs.[typ_oddílu] [dodatečné parametry] [oddíl]```
    * např.
      * formátování oddílu jako ext4 ```mkfs.ext4 /dev/sdb1```
      * formátování oddílu jako btrfs s labelem "part" ```mkfs.btrfs -L "part" /dev/sdb2```
  * swap
    * formátování jako swap ```mkswap [oddíl]```
    * např. ```mkswap /dev/sdb3```
    * swap nebude mít přípojný bod (bude ```none```)
* zobrazení informací o oddíle
  * příkaz ```blkid [oddíl]```
  * např. ```blkid /dev/sdb2```
    * zobrazí velikost oddílu, jeho UUID a LABEL
* mount při bootu
  * nejprve je nutné vytvořit přípojný bod (složku)
    * např. ```mkdir -p /mnt/files/```
  * úprava souboru ```/etc/fstab```
  * syntaxe:
```
[identifikátor_disku]                       [přípojný bod (složka)] [typ] [možnosti] [dump] [pass]
#napojení ext4 svazku pomocí UUID na složku /mnt/files
UUID="75d744c6-2c7d-4414-9d23-c8a69ad2c709" /mnt/files              ext4  defaults    0      1
#napojení btrfs svazku pomocí LABELu na složku /mnt/better
LABEL="part"                                /mnt/better             btrfs defaults    0      1
#napojení swapovacího oddílu
UUID="39e6b5a8-f0de-45fe-b479-a2ba699ad7ed" none                    swap  sw          0      0
#napojení swapfile
/var/swapfile                               none                    swap  sw          0      0
```
  * následné ozkoušení pomocí příkazu ```mount -a``` a ```mount``` 
### Tvorba swapfile
* swapfile = odkládací prostor uložený v souboru místo na odděleném oddílu
* Postup tvorby:
  * Vytvoření souboru pomocí příkazu ```fallocate -l [velikost] [soubor]```
    * ```fallocate -l 150M /var/swapfile```
  * změna oprávnění souboru na 600 pomocí ```chmod 600 [soubor]```
    * ```chmod 600 /var/swapfile```
  * formátování souboru jako swapfile
    * ```mkswap [název_souboru]``` např. ```mkswap /var/swapfile```
  * 
  * přidání záznamu do ```/etc/fstab``` viz. předchozí kapitola

### LVM
* https://www.digitalocean.com/community/tutorials/how-to-use-lvm-to-manage-storage-devices-on-ubuntu-18-04
* formátování jednotlivých oddílů na kterých bude LVM pomocí ```pvcreate [oddíl1] [oddíl2] ...```
  * např. ```pvcreate /dev/sdb20 /dev/sdb21 /dev/sdb22``` nebo ```pvcreate /dev/sdb{20,21,22}```
* tvorba LVM skupiny disků pomocí příkazu ```vgcreate [název_skupiny_disků] [oddíl1] [oddíl2] ...```
  * např. vytvoření skupiny disků "skupA" ```vgcreate "skupA" /dev/sdb20 /dev/sdb21 /dev/sdb22``` nebo ```vgcreate "skupA" /dev/sdb{20,21,22}```
* vytváření svazků v LVM pomocí příkazu ```lvcreate -L [velikost_oddílu] -n [název_oddílu] [název_skupiny_disků]```
  * např. vytvoření oddílu o velikosti 180 MiB s názvem "LVMPartition" ve skupině disků "skupA"
```lvcreate -L 180M -n LVMPartition skupA```
  *oddíl bude nyní přístupný na ```/dev/[název_skupiny_disků]/[název_oddílu]```
* formátování oddílu zase pomocí mkfs
  * např. mkfs.ext4 /dev/skupA/LVMPartition
  * potom už se s oddílem pracuje jako s každým jiným

### RAID
* https://raid.wiki.kernel.org/index.php/RAID_setup
* úrovně raidu
  * 0 - striping
  * 1 - mirroring
  * 5 - parita dat
  * 6 - dvojitá parita dat
* tvorba pomocí ```mdadm --create  --verbose [název_nového_zařázení] --level=[úroveň_raidu] --raid-devices=[poćet_zrázení] [oddíl1] [oddíl2] ...```
  * např. ```mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdb6 /dev/sdc5```
  * název_nového_zařázení nesmí existovat při tvorbě nového raidu, např. když vytvořím /dev/md0 tak další raid budu pojmenovávat /dev/md1
* uložení konfigurace
  * ```mdadm --detail --scan >> /etc/mdadm/mdadm.conf```
* zobrazení informací o raid
  * ```cat /proc/mdstat``` zobrazí aktivní raidy
  * ```mdadm --monitor [md zařízení]```
### Kvótování
* https://www.digitalocean.com/community/tutorials/how-to-set-filesystem-quotas-on-ubuntu-20-04
* limitovatelné věci
  * blocks - celková velikost souborů
  * inodes - počet souborů a složek
* soft quote
  * limit velikosti který se dá překročit po dobu grace period
* hard quote
  * limit velikosti, který se nedá překročit
* grace period
  * doba, po kterou se dá překročit soft quote
  * nastavuje se pomocí 
* Nastavení
  * nejprve je nutné do souboru ```/etc/fstab``` přidat do možnosti options ```usrquota``` nebo ```grpquota```
    * např.
```
# <file system> <mount point>   <type>  <options>         <dump>  <pass>
LABEL="Vol2"    /mnt/vol2       ext4    usrquota,grpquota 0       1
```
  * následně je nutný umount a mount, např.
```
umount /mnt/vol2
mount -a
```
  * následně je nutné vytvořit soubory s konfigurací kvóty na oddílu pomocí ```quotacheck```
    * pro uživatele
      * ```quotacheck -u /mnt/vol2/```
    * pro skupinu
      * ```quotacheck -g /mnt/vol2/```
    * pro uživatele i skupinu
      * ```quotacheck -ug /mnt/vol2/```
  *  Povolení kvót
    * ```quotaon -v /mnt/vol2/```
  * úprava kvót
    * pro uživatele
      * ```sudo edquota -u [uživatel]``` např. ```sudo edquota -u fresh```
    * pro skupinu
      * ```sudo edquota -g [skupina]``` např. ```sudo edquota -g fresh```
    * grace period
      * úprava pomocí ```edquota -t```
     
### Tvorba šifrovaných oddílů (LUKS)
* nástroj ```cryptsetup```
* pomocí hesla
  * inicializace šifrovaného oddílu pomocí ```cryptsetup -y -v luksFormat [oddíl]```
    * např. ```cryptsetup -y -v luksFormat /dev/sdb30``` a následné potvrzení pomocí "YES" velkými písmeny a napište heslo
  * dešifrování disku a jeho pojmenování:
    * ```cryptsetup open [oddíl] [jméno]```
      * např. ```cryptsetup open /dev/sdb30 "securePart"```
  * ověření stavu pomocí ```cryptsetup status [jméno]```
    * např. ```cryptsetup status "securePart"```
  * Tvorba filesystému pomocí ```mkfs.[typ_oddílu] /dev/mapper/[jméno]```
    * např. ```mkfs.ext4 /dev/mapper/securePart```
* pomocí klíče
  * tvorba klíče pomocí ```dd if=/dev/urandom of=[klíč]  bs=1024 count=4```
    * např. ```dd if=/dev/urandom of=/root/.secure-key  bs=1024 count=4```
  * změna oprávnění klíče na 400 pomocí ```chmod 0400 [klíč]```
    * např. ```chmod 0400 /root/secret.key```
  * inicializace šifrovaného oddílu pomocí ```cryptsetup -v luksFormat [oddíl] --key-file=[klíč]```
    * např. ```cryptsetup -v luksFormat /dev/sdb30 --key-file=/root/secret.key```
  * dešifrování s pojmenováním pomocí ```cryptsetup -v luksFormat --key-file=[klíč] [zařízení] [jméno]```
    * např. ```cryptsetup open --key-file=/root/secret.key /dev/sdb30 securePart```
  * Tvorba filesystému pomocí ```mkfs.[typ_oddílu] /dev/mapper/[jméno]```
    * např. ```mkfs.ext4 /dev/mapper/securePart```
  * automatické dešifrování při startu
    * připsání do ```/etc/crypttab```
    * syntax: ```[jméno] [iden] [klíč] [typ_šifry]```
    * 
    * např.
### práce s procesy
* priorita
* uspání procesu
# 4. Ročník
## IP

# LinuxMaturita
Shrnutí učiva 2. až 4. řočníku na SPŠE V Úžlabině
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
* např. ```for user in alojs pepina rodic knihovnik
do
   sudo useradd -m -d /knihovna/$user -s /bin/bash -g knizni_klub -G cdrom,dip,lxd $user
   sudo chmod 700 /knihovna/$user
   echo -e "$user\n$user" | sudo passwd $user
   sudo passwd -e $user
   sudo chage -E $(date -d '+183 days' '+%Y-%m-%d') $user
done```

## Správa oprávnění k souborům a složkám
### Změna oprávnění
* příkaz ```chmod [práva] [soubor]```
* např. ```chmod 750 file.txt``` Dá vlastníkovi souboru všechna oprávnění, skupině povolí číst a spouštět soubory a ostatním zakáže všechny operace
### Změna vlastnictví
* příkaz ```chown [uživatel[:[skupina]]] file.txt```
* např. ```chown worker:developers file.txt``` Změní vlastníka souboru na uživatele worker a vlastnickou skupinu na skupinu developers pro soubor file.txt

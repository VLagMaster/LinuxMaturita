# LinuxMaturita
Shrnutí učiva 2. až 4. řočníku na SPŠE V Úžlabině
# 2. Ročník
## Práce se soubory
### Vytvoření složky
* příkaz mkdir
* syntaxe ```mkdir [parametry] [cesta]```
* např. ```mkdir -p nova_slozka```
* v případě nutnosti vytvoření více složek zároveň je možno použít složené závorky
* ```mkdir -p ulice/{Praha,Berlín,Varšava}```
* ```mkdir -p ulice/{Praha/{A,B},Berlín/{C,D},Varšava/{E,F}}```
### Vytvoření souborů
* Pro tvorbu prázdných souborů je možné použít příkaz ```touch```
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
### Roury - Pipes
* směrování standartního výstupu z jedné aplikace jako vstup uživatele (jako by to psal na klávesnici)
* použití ```[příkaz_ze_kterého_používáme_výstup] | [příkaz_který_použije_vstup_z_roury]```
* např. ```ls -l | grep txt```

### Hromadná tvorba souborů mající obsah
* stejný princip jako u tvorby více složek zároveň, vhodné použít příkaz tee, který umožní psaní do více souborů zároveň
* ```echo "Oblíbená ulice" | tee ulice/{Bratislava/{Bachova,Česká,Dobrovičova},Praha/{Adamova,Dačická,Eberlova},Warzawa/{Abrahama,Balzaka,Chlebowa}}/desc.txt```

## Správa uživatelů
### Soubory s konfiguracemi uživatelů
* ```/etc/passwd``` obsahuje základní informace (jméno, domovský adresář, výchozí shell, způsob ověření, ...)
* ```/etc/shadow``` obsahuje hesla
* ```/etc/group``` obsahuje informace o skupinách a Členství ve skupinách
### Tvorba uživatelů
* příkaz ```useradd```
* parametry:
* 

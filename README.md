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
* existují 2 základní výstupy aplikací
*   stdout - běžný výstup (1)
*   stderr - chybový výstup (2)
* input
*   stdin
*   
* přesměrování stdout aplikace do či ze souboru
* Přepsání souboru
*   ```echo "Hello World" > out.txt```
* Vložení výstupu za existující obsah na nový řádek
*   ```echo "Hello World >> out.txt"```
* Použití obsahu souboru jako vstup
*   ```cowsay < in.txt```

### Roury - Pipes
* směrování standartního výstupu z jedné aplikace jako vstup uživatele (jako by to psal na klávesnici)
* použití ```[příkaz_ze_kterého_používáme_výstup] | [příkaz_který_použije_vstup_z_roury]```
* např. ```ls -l | grep txt```

### Hromadná tvorba souborů mající obsah
* stejný princip jako u tvorby více složek zároveň, vhodné použít příkaz tee, který umožní psaní do více souborů zároveň
* echo "Oblíbená ulice" | tee ulice/{Bratislava/{Bachova,Česká,Dobrovičova},Praha/{Adamova,Dačická,Eberlova},Warzawa/{Abrahama,Balzaka,Chlebowa}}/desc.txt

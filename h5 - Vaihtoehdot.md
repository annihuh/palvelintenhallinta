# h5 Vaihtoehdot
## Rauta

    CPU: Intel(R) Core(TM) i3-8130U CPU @ 2.20GHz 2.21 GHz
    RAM: 8,00 Gt
    Windows-versio: Windows 11
    
## x) Lue ja tiivistä

### Control Windows with Salt
https://terokarvinen.com/2018/control-windows-with-salt/

- Käytä Windowsissa uusimpia päivityksiä
- Samoin masterilla, aina sama tai uudempi versio kuin orjalla
- Asentaessa Windowsille orjaa-ohjelmaa useimmissa tapauksissa amd64 ja python ovat toimivimmat valinnat versiota valikoidessa
- Muista uniikki nimi tehdessä
- Saltilla voidaan komentaa Windowsia niin kuin Linuxiakin, toki Windows on perinteisesti monimutkaisempi
- Salt toimii paikallisesti Windowsissa
- Sekä serverinä

### Harjoitus 6 - Windows - 10.05.2021
https://johanlindell.fi/palvelintenhallinta#h6

- Aluksi asenna sovellus käsin > testaa > poista > todettu, että toimii
- Asennetaan Salt minionille
- Hyväksytään minioni masterilla (avaimet)
- Luodaan init.sls /srv/salt/-polkuun > sisältö, joka vaikuttaa Windows-minioniin
- Ajetaan saltilla
- Tarkistetaan minionilta toimivuus
- Gitin asennus > winrepo
- Sls-tiedostoon muutos, että se asentaa toivotun sovelluksen
- Aja tiedosto ja tarkista lopputulos

## a) Asenna Salt Windowsille

Aloitin tehtävän asentamalla minion-ohjelman Windowsille osoitteesta https://repo.saltproject.io/salt/py3/windows/latest/. Valitsin kuvassa näkyvän version.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/235341355-5be4f53d-a5a2-46cf-8019-95331e8dc2ae.png">

Tiedoston ladattua aloitin ohjelman asentamisen. Setup Wizard pyysi admin-oikeuksia pyöriäkseen, joten avasin sen PowerShellissä näin: `PS C:\Users\annij\Downloads> .\Salt-Minion-3006.0-Py3-AMD64.msi`. Etenin installerissa finish kohtaan asti suurimmaksi osaksi oletuksilla (vaihdoin minionin nimeksi a003) ja ohjelma käynnistyi automaattisesti. Salt asennettu.

## b) Ei voi kalastaa

Tässä kohdassa testaan onnistuuko Saltin käyttö paikallisesti. Kopioin ensimmäiseksi lataamani tiedoston `Salt-Minion-3006.0-Py3-AMD64.msi` polkuun `C:\Windows\System32` komennolla `Copy-Item .\Salt-Minion-3006.0-Py3-AMD64.msi 'C:\Windows\System32\'`, että voin ajaa komentoja mistä tahansa. Sen jälkeen suoritin komennon `salt-call.exe --local test.ping` ja sain vastaukseksi `local:true`, joka tarkoittaa, että Salt toimii paikallisesti.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/235344619-15074bd7-e0c7-41a5-89c2-f4555b231e7b.png">

Huom. komento toimii siis myös ilman .exe-loppua.

## c) Hei ikkuna

Ryhdyin luomaan Hei maailma tiedostoa indempotenttina. Kirjoitin alla olevan komennon komentoriville. Se siis loi uuden tiedoston väliaikaistiedostojen kansioon `C:\Windows\Temp`. 

    salt-call --local state.single file.managed 'C:/Windows/Temp/helloworld.bat'

.bat on Windowsissa merkki siitä, että kyseessä on komentotiedosto, jonka voi suorittaa. Menin muokaamaan tiedoston sisältöä: `notepad.exe .\helloworld.bat`. Tiedoston sisältö:

    PS C:\Windows\Temp> cat .\helloworld.bat
    @echo off
    echo Hello World!

@echo tarkoittaa sitä, että käyttäjä ei näe skriptin suorituksen aikana ajettavia komentoja ja echo Hello World! on se, mitä tulostetaan. Ajoin komennon `.\helloworld.bat` ja tulos oli tämä:

    PS C:\Windows\Temp> .\helloworld.bat
    Hello World!
    
Skirpti toimii!

## d) Installed

Tein tämän osion palautuksen jälkeen. Vinkkiä otin muiden kurssilaisten rapsoista: https://github.com/R01-P4R/Palvelinten-Hallinta-2023-kev-t/blob/main/H5.md ja https://github.com/hannagrn/palvelinten-hal/blob/main/h5.md.

Latasin ensimmäiseksi micron zip-tiedoston osoitteesta https://github.com/zyedidia/micro/releases/tag/v2.0.11. Valitsin micron Windowsille: `micro-2.0.11-win64.zip` ja purin latauksen downloads kansiossa. Sisältä löytyi micro sovellus, jonka asennan tässä tehtävässä. Tämän jälkeen loin uuden hakemistopolun `/srv/salt/state`, jonka sisälle kopioin micron.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/235536158-4d0b871c-2881-40d8-8a83-533bab163023.png">

Tämän jälkeen loin init.sls tiedoston, jonka sisältö on tämä:

    C:\Windows\System32\micro.exe:
      file.managed:
        - source: "salt://state/micro.exe"
        - makedirs: True

Ylin rivi on se mihin mikro asennetaan, toinen rivi tekee halutulle tiedostolle jotain (kopioidaan paikasta toiseen), kolmas kertoo, mistä kopioidaan. Sen jälkeen ajoin komennon:

    salt-call --local state.apply state

Sain virheilmoituksen:

    local:
        Data failed to compile:
    ----------
        No matching sls found for 'state' in env 'base'
 
Löysin vastauksen tähän tutkimistani raporteista. Eli siis määrittelytiedostossa on väärät määritelmät, jonka takia tiloja etsitään masterilta eikä paikallisesti. Menin siis sijaintiin `C:\ProgramData\Salt Project\Salt\conf\minion` ja muokkasin tiedostoa minion poistamalla kommenttimerkkejä alusta () ja vaihtamalla remote > local:

    (#) file_client: (remote) local 
    
    (#) file_roots:
          base:
            - /srv/salt/
    
Muutosten jälkeen ajoin tilan `salt-call --local state.apply state` ja sain ilmoituksen, että asennus onnistui. Kokeilin avata init.sls tiedoston microlla `micro init.sls`. Micro toimii.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/235543989-c4409961-15cd-4200-baeb-c8f91d3daec1.png">

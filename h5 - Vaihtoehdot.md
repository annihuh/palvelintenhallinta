# h5 Vaihtoehdot
## Rauta

    CPU: Intel(R) Core(TM) i3-8130U CPU @ 2.20GHz 2.21 GHz
    RAM: 8,00 Gt
    Windows-versio: Windows 11
    
## Lue ja tiivistä

## a) Asenna Salt Windowsille

Aloitin tehtävän asentamalla minion-ohjelman Windowsille osoitteesta https://repo.saltproject.io/salt/py3/windows/latest/. Valitsin kuvassa näkyvän version.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/235341355-5be4f53d-a5a2-46cf-8019-95331e8dc2ae.png">

Tiedoston ladattua aloitin ohjelman asentamisen. Setup Wizard pyysi admin-oikeuksia pyöriäkseen, joten avasin sen PowerShellissä näin: `PS C:\Users\annij\Downloads> .\Salt-Minion-3006.0-Py3-AMD64.msi`. Etenin installerissa finish kohtaan asti suurimmaksi osaksi oletuksilla (vaihdoin minionin nimeksi a003) ja ohjelma käynnistyi automaattisesti. Salt asennettu.

## b) Ei voi kalastaa

Tässä kohdassa testaan onnistuuko Saltin käyttö paikallisesti. Kopioin ensimmäiseksi tiedoston `Salt-Minion-3006.0-Py3-AMD64.msi` polkuun `C:\Windows\System32` komennolla `Copy-Item .\Salt-Minion-3006.0-Py3-AMD64.msi 'C:\Windows\System32\'`, että voin ajaa komentoja mistä tahansa. Sen jälkeen suoritin komennon `salt-call --local test.ping` ja sain vastaukseksi `local:true`, joka tarkoittaa, että Salt toimii paikallisesti.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/235344619-15074bd7-e0c7-41a5-89c2-f4555b231e7b.png">

## c) Hei ikkuna

## d) Installed

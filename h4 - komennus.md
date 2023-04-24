# Komennus
## Rauta ja versiot
    CPU: Intel(R) Core(TM) i3-8130U CPU @ 2.20GHz 2.21 GHz
    RAM: 8,00 Gt
    Järjestelmätyyppi: 64-bittinen käyttöjärjestelmä
    Windows-versio: Windows 11
    Vagrant-versio: 2.3.4
    
## Hello.sh
Aloitin tehtävän luomalla skriptin komennolla `nano shine.sh`. Menin editoriin muokkaamaan tiedostoa ja lopputulos oli tämä:
    
    #! /usr/bin/bash
    echo "shine"

Skripti siis toistaa sanan shine terminaaliin käyttäen bash-kieltä. Luomisen jälkeen minun piti mennä muokkaamaan oikeuksia, että pääsen ajamaan kyseisen tiedoston. Annoin kaikille oikeudet ajaa tiedoston komennolla `chmod ugo+x shine.sh`. Kokeilin skriptin ajamista tmasterilla käyttäen komentoa `./shine.sh`, komento tulosti viestin shine.

Seuraavaksi kopioin skriptin niin, että skriptin pystyy ajamaan pelkästään komennolla `shine.sh` missä tahansa hakemistossa. Kopioin sen polkuun `/usr/local/bin/`, koska kyseisessä sijainnissa on ohjelmia, joita peruskäyttäjä voi käyttää. Näin kopioin skriptin:
    
    sudo cp shine.sh /usr/local/bin/

Sen jälkeen kokeilin ajaa komennon kirjoittamalla pelkästään `shine.sh` ja skriptin suorittaminen onnistui. Testasin sen jälkeen vielä toisessa /etc-hakemistossa. Ajaminen onnistui sielläkin.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/233925491-2f3913d1-5647-4b0e-ad87-e006948e5847.png">

## Hello.py
Tässä kohdassa teen saman kuin edellisessä, mutta skripti on pythonilla ajettava. Loin siis skriptin komennolla `nano summer.py` ja lisäsin tiedostoon seuraavat tiedot:

    #! /usr/bin/python3
    print ("today is sunny")

Skripti tekee siis täysin saman kuin bash:lla eli python tulostaa tekstin "today is sunny" print komennon avulla. Sen jälkeen tein samat komennot kuin edellisessäkin tehtävässä. Eli vaihdoin suoritusoikeudet, testasin skriptin kotihakemistossani, kopioin skriptin yhteiseen hakemistosijaintiin ja testasin skriptin toimivuuden kahdessa eri hakemistossa.

    1. chmod ugo+x summer.py
    2. ./summer.py
    3. sudo cp summer.py /usr/local/bin
    4. summer.py

Lopputestaus kahdessa eri hakemistossa:

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/233931967-facbd095-8245-4559-b92c-c1a2292274c9.png">

## Automatisoi saltilla
Aloitin tehtävän tekemällä perinteiset alkutoimet, jonka jälkeen ryhdyin tekemään skriptin automatisointia. Loin uuden kansion:

    sudo mkdir -p /srv/salt/commands

Kopioin molemmat skriptit commands kansioon:

    sudo cp shine.sh /srv/salt/commands
    sudo cp summer.py /srv/salt/commands

Siirryin kansioon `cd /srv/salt/commands`-komennolla sekä tarkastin `ls`-komennolla, että skriptit ovat kansiossa. Tämän jälkeen loin uuden init.sls tiedoston `sudoedit init.sls`. Tiedoston sisältö:

    bash:
      file.managed:
        - name: /usr/local/bin/shine.sh
        - source: salt://commands/shine.sh
        - mode: 0755

    python:
      file.managed:
        - name: /usr/local/bin/summer.py
        - source: salt://commands/summer.py
        - mode: 0755

Loin molemmille skripteille oman tilan eli `file.managed` komenot luo uuden tiedoston orjakoneelle. Name kertoo, mihin sijaintiin orjalla kyseinen skripti asentuu, source kertoo mistä kopioidaan ja mode muokkaa oikeudet niin, että skriptin ajaminen onnistuu.

Sen jälkeen testasin saltilla onnistuuko skriptin ajaminen. Käytin komentoa `sudo salt 't001' state.apply init.sls`.






## Asenna

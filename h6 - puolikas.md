# h6 - Puolikas
    Valmiustaso: kesken

## Rauta
    speksit tähän

## Käsin
Projektissani asennan UFW:n eli Uncomplicated Firewallin isäntä-orja-arkkitehtuuria käyttäville koneille salttia käyttäen. Aloitin asentamisen sillä, että käytin kurssin alussa annettua Vagrantfile-tiedostoa, joka määrittelee isännän ja orjien tietoja. Muutin hieman koneiden nimiä: `t001 > a001` ja käynnistin ne komennolla `vagrant up`. Kirjauduin amaster koneelle `vagrant ssh amaster`. Ajoin komennon `sudo salt-key -A` ja hyväksyin avaimet a001 ja a002. Nyt ympäristö on valmis käytettäväksi.

Jatkoin UFW:n asentamiseen amasterilla. Varmistin, että kaikki on päivitetty ajamalla komennon `sudo apt-get update`. Sen jälkeen asensin, käynnistin ja katsoin onko UFW päällä.

    sudo apt-get install ufw
    sudo systemctl start ufw
    sudo systemctl status ufw.service

Status:

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/236705695-6aee2ea0-57b7-49ac-a7ef-c10a4f04346d.png">

Merkintä (exited) tarkoittaa sitä, että UFW ei ole tällä hetkellä aktiivisessa käytössä, mutta on kuitenkin päällä.

Tämän jälkeen konfiguroin UFW:n sallimaan SSH-yhteyden ja aktivoin lopuksi säännön.

    sudo ufw allow openssh
    sudo ufw enable

Katsoin lopputuloksen komennolla `sudo ufw status`:

    Status: active

    To                         Action      From
    --                         ------      ----
    OpenSSH                    ALLOW       Anywhere
    OpenSSH (v6)               ALLOW       Anywhere (v6)

Kokeilin yhdistää ssh:lla a001-koneelta amasterille sekä toisinpäin. Se onnistui molemmilla kerroilla. Kuvassa a001 > amaster.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/236806945-4b68199a-8c15-41e9-aa6e-8d515363a984.png">

Halusin vielä kokeilla, että kiellän Telnetin UFW:ssä ja testaan sen. Ajoin siis komennot 

    sudo ufw deny telnet
    sudo ufw enable
    
Tarkistin, että status näytti oikealta:

    Status: active

    To                         Action      From
    --                         ------      ----
    OpenSSH                    ALLOW       Anywhere
    23/tcp                     DENY        Anywhere
    OpenSSH (v6)               ALLOW       Anywhere (v6)
    23/tcp (v6)                DENY        Anywhere (v6)
    
Varmistin, että Telnet on asennettu koneeseen komennolla `sudo apt-get install telnet` ja kokeilin muodostaa telnet-yhteyden amaster-koneeseen.

    vagrant@a001:~$ telnet 192.168.12.3
    Trying 192.168.12.3...

Tämä tarkoittaa sitä, että UFW:n konfiguraatiot on onnistuneet ja Telnet-yhteys blokataan. Yhteys siis jatkaa yrittämistä, mutta sitä ei sallita. Jonkin ajan päästä tulee ilmoitus, että yhteyttä ei pystynyt muodostamaan:

    telnet: Unable to connect to remote host: Connection timed out

## Automatisointi

Seuraavaksi loin kansion, johon lisäsin tilan: `sudo mkdir -p /srv/salt/mini`. Komento siis luo pääkäyttäjän oikeuksien avulla mini-kansion annettuun sijaintiin ja kaikki puuttuvat kansiot sen edeltä. Sinne lisään tiedoston init.sls komennolla `sudoedit init.sls`. Alla tiedoston sisältö:

    pkg.installed:
      - name: ufw

    install ssh-server:
      pkg.installed:
        - name: openssh-server
      service.running:
        - name: openssh-server

    install ssh-client:
      pkg.installed:
        - name: openssh-client
      service.running:
        - name: openssh-client

    allow ssh:
      cmd.run:
        - name: ufw allow ssh
        - unless: ufw status verbose | grep '22.*ALLOW.*Anywhere'
        - reguire:
          - cmd: install ufw

    deny telnet:
      cmd.run:
        - name: ufw deny telnet
        - unless: ufw status verbose | grep '23.*DENY.*Anywhere'
        - reguire:
          - cmd: install ufw

KESKEN


## Lähteet
https://www.cyberciti.biz/faq/how-to-configure-firewall-with-ufw-on-ubuntu-20-04-lts/
https://github.com/kri-ku/my_first_salt/blob/master/raportti.md
https://docs.saltproject.io/en/getstarted/config/functions.html
https://github.com/jullegrigori/Palvelinten-hallinta/blob/main/h7b.md
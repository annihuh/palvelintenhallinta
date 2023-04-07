# Demonit

## x) Lue ja tiivistä

hfrghd

## a) OpenSSH

Aloitin harjoituksen luomalla uudestaan masterin ja orjat edellisen tehtävän mukaisesti. Sen jälkeen menin t001-koneelle tekemään manuaalisesti OpenSSH:n asennusta.

Menin t001 koneelle ssh yhteydellä.

    vagrant ssh t001

Päivitin koneen.

    sudo apt-get update && upgrade

Seuraavaksi asensin serverin t001:lle.

    sudo apt-get install openssh-server
    
Käynnistin palvelimen, että se on varmasti käynnissä

    sudo service ssh start

Tarkistin asennuksen komennolla `whereis openssh` ja lopputulos:

    openssh: /usr/lib/openssh /usr/share/openssh

Katsoin vielä ssh:n statuksen ja käynnissä olevat prosessit ajamalla komennot `sudo systemctl status ssh.service` ja `ps aux | grep ssh`.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/230623105-c6627b7b-ae0c-4544-8b2a-37091eb38c72.png">

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/230622965-8735d8c8-3d89-47d8-89ed-666a117e1bd1.png">


## b) Automatisoiminen

gsdd

## c) Toinen asetus SSH-palveluun

gsgfsdg

## d) Demonin säätäminen saltilla

hgl

## Lähteet

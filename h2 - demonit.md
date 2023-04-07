# Demonit

## x) Lue ja tiivistä

Linkki tiivistettävään tekstiin: https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh 

Teksti käsittelee SSH-palvelun automatisointia. Ensimmäisenä käsketään asentamaan master-kone ja orjat. Sen jälkeen ryhdytään luomaan SSH-tilaa ohjeiden mukaisesti. Alussa tehdään sls-tiedosto, johon lisätään tietoa, mitä halutaan orjien tekevän. Sen jälkeen luodaan default sshd_config-tiedoston pohjalta ns. määrittelytiedosto, jossa lukee erinäisiä tietoja esim. protokollasta ja porteista. Siihen siis määritellään oletusasetukset.

Sen jälkeen käsketään testaamaan tilaa orjilla, ohjeissa kerrotaan siihen kaksi erilaista tapaa.

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

Tarkistin ssh:n statuksen ja käynnissä olevat prosessit ajamalla komennot `sudo systemctl status ssh.service` ja `ps aux | grep ssh`. SSH-palvelu on siis käynnissä.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/230623105-c6627b7b-ae0c-4544-8b2a-37091eb38c72.png">

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/230622965-8735d8c8-3d89-47d8-89ed-666a117e1bd1.png">

Asennuksen jälkeen siirryin lisäämään toista porttia oletusportin lisäksi. Siirryin komennolla `cd /etc/ssh` em. sijaintiin. Tein varmuuden vuoksi kopion sshd_config tiedostosta komennolla:

    sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config_backup

Sen jälkeen menin muokkaamaan kyseistä tiedostoa näin:

    sudo nano sshd_config
    
Kopioin sisällön config-tiedostoon täältä: https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh. Lopputulos näytti tältä:

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/230639487-bdb3594f-b10d-46ba-bd67-271e1b916b9f.png">

Unohdin lisätä portin 22, joten menin vielä takaisin lisäämään sen esimerkissä olevan portin 8888 alapuolelle. Sen jälkeen käynnistin uudelleen SSH-palvelimen. 

    sudo systemctl restart sshd

Tämän jälkeen menin testaamaan toimiiko configuraationi

## b) Automatisoiminen
  
gsdd

## c) Toinen asetus SSH-palveluun

gsgfsdg

## d) Demonin säätäminen saltilla

hgl

## Lähteet

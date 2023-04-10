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

    sudo systemctl restart ssh

Tarkistin ssh:n statuksen ajamalla komennon `sudo systemctl status ssh.service`. SSH-palvelu on siis käynnissä.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/230623105-c6627b7b-ae0c-4544-8b2a-37091eb38c72.png">

Asennuksen jälkeen siirryin lisäämään toista porttia oletusportin lisäksi. Siirryin komennolla `cd /etc/ssh` em. sijaintiin. Tein varmuuden vuoksi kopion sshd_config tiedostosta komennolla:

    sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config_backup

Sen jälkeen menin muokkaamaan kyseistä tiedostoa näin:

    sudo nano sshd_config
    
Kopioin sisällön config-tiedostoon täältä: https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh, mutta lisäsin portin 8888 lisäksi myös oletusportin 22. Lopputulos näytti tältä:

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/230871151-79a86626-22a5-428e-96a2-2b0502468dd6.png">

Tämän jälkeen käynnistin uudelleen SSH-palvelimen. 

    sudo systemctl restart ssh

Tämän jälkeen testasin toimiiko konfiguraationi. Ajoin komennon `ssh -p 8888 vagrant@192.168.12.102`. Yhdistin onnistuneesti portin 8888 kautta t002:lle.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/231007256-dcf5d866-8a0c-49a8-ad08-7e46777346aa.png">

## b) Automatisoiminen
  
Aloitin automatisoinnin avamaalla tmaster koneen ja tekemällä sille samat alkutoiminnot kuin t001:lle. Eli päivitin `sudo apt-get update && upgrade` ja asensin palvelimen `sudo apt-get install openssh-server` sekä tarkistin, että asennus oli onnistunut. Tämän jälkeen tein backupin `cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup` ja muokkasin sshd_config-tiedostoa samlla tavalla kuin edellisessä tehtäväkohdassa `sudo nano /etc/ssh/sshd_config`. Tämän jälkeen vielä käynnistin SSH-palvelun uudestaan.

Loin kansion hakemistopolkuun `/srv/salt/demonit` komennolla:

    sudo mkdir -p /srv/salt/demonit
    
Kopioin kansioon sshd_config tiedoston, jonka äsken loin. Lisäsin init.sls tiedoston demonit kansioon ja aloin luomaan tilaa SSH:lle nano-editorilla `sudoedit /srv/salt/demonit/init.sls`. Init.sls-tiedoston sisältö näytti lopuksi alla olevan kuvan mukaiselta.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/230994135-4ab35723-c557-4642-b284-6d6a452fc5c7.png">

Seuraavaksi testasin meneekö tilan ajaminen läpi. Ajoin komennon `sudo salt '*' state.apply demonit` ja sain seuraavanlaisen ilmoituksen.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/231000823-35bcf1d9-55d1-4294-b583-f52bfb32e855.png">

Ongelma johtui väärin kirjoitetusta polusta source-kohdassa init.sls tiedostossa. Muutin sen sisältöä niin, että kirjoitin sourcen näin: `- source: salt://demonit/sshd_config`. Sen jälkeen ajoin komennon uudelleen orjille onnistuneesti.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/231001499-83a3d53b-d128-45ed-b8ef-7e6121251ff5.png">

Tein testin niin, että ajoin komennon `ssh -p 8888 vagrant@192.168.12.100`, joka onnistuneesti otti yhteyttä master-koneesta t001-koneelle portin 8888 kautta.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/231006260-31306503-2912-4f8b-a84a-6a84684340dc.png">

## c) Toinen asetus SSH-palveluun

Menin lisäämään sshd_config tiedostoon kolmannen portin. Siis kirjoitin `Port 8887`, tallensin tiedoston ja käynnistin sshd:n uudelleen. Sen jälkeen tein komennon `sudo salt '*' state.apply demonit`. Ensimmäisellä kerralla `succeeded` kohdan perässä lukee `changed=2`, joka tarkoittaa sitä, että tarkastellussa tiedostossa on tapahtunut kaksi muutosta ja ne ajetaan ensimmäistä kertaa. Sen jälkeen `changed` merkintää ei tule, koska lisäämäni portti ei ole enää uusi muutos.

Ensimmäinen kerta.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/231010782-de034000-b542-47ac-86b8-3a9905c61c10.png">

Toinen ja kolmas kerta.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/231010965-453e99b2-2111-4f5b-b28b-bd3869a0ad21.png">
<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/231011026-1d78e701-a2f8-4113-b05b-36efa68c3c3f.png">

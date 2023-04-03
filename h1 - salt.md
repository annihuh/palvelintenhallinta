# h1 - Harjoittelua 
Tehtävän h1 raportti.
## Nettisivun luominen Githubilla - tiivistelmä
Karvinen 2023: https://terokarvinen.com/2023/create-a-web-page-using-github/
- Rekisteröityminen Githubiin
- Uuden repositoryn luominen, määrittele:
  - Julkinen tiedosto
  - Kuvaus
  - Readme-tiedosto
  - GNU
- .md-tiedoston lisääminen, määrittele:
  - Otsikko
  - Koodi
- Julkaiseminen
## Masterin ja kahden orjan automaattinen luonti Linuxilla - tiivistelmä 
Karvinen 2023: https://terokarvinen.com/2023/salt-vagrant/
- Virtualisointiympäristön asentaminen:
  - Vagrantin asennus
  - Vagrantfile
  - Koneiden käynnistys
  - Masterille kirjautuminen
  - Masterilla orjien hyväksyminen
  - Komennot perinteisesti ja idempotentilla - paljon esimerkkejä
  - Erilaisten tiedostojen luominen (init ja top)
  - Uloskirjautuminen

## Debian 11 asentaminen Vagrantilla

Käytössäni on läppäri, jossa Windows 11 sekä kaikki tarpeelliset päivitykset.

Ensimmäisenä asensin Vagrantin koneelleni ohjeessa annetusta linkistä ja käynnistin tietokoneeni sen jälkeen uudestaan. Sen jälkeen avasin tietokoneellani PowerShellin adminina ja suoritin seuraavat komennot: 

**Vagrant init debian/bullseye64** asentaakseni Debian 11 haluamaani tiedostosijaintiin (/user/vagrant/debian)

<img width="270" alt="image" src="https://user-images.githubusercontent.com/101214286/229461896-742e5435-9838-4dce-9842-821ddd17a0ac.png">

**Vagrant up** käynnistääkseni Vagrantin

<img width="345" alt="image" src="https://user-images.githubusercontent.com/101214286/229462093-207a6e50-4e8b-40e3-ba16-c5df79e3f4c9.png">

**Vagrant ssh** - varmistin, että psytyn ottamaan yhteyden koneeseen

<img width="305" alt="image" src="https://user-images.githubusercontent.com/101214286/229462181-326fc8a2-817d-4cc5-be37-7228187d516a.png">

## 3 koneen verkon asennus

Loin tätä tehtäväkohtaa varten oman kansion /users/vagrant/saltdemo. Ja lisäsin init-tiedoston tähänkin kansioon. Kopioin sinne ready-made-tekstin. Käynnistin sen jälkeen virtuaalikoneet uudestaan komennolla **vagrant up**.

## Orjien hyväksyminen

Kirjauduin komennolla **vagrant ssh tmaster** ns. isäntäkoneelle, jolla hyväksyn orjat. Hyväksyin siis orjat komennolla **sudo salt-key -A** ja testasin yhteyden ajamalla komennon **sudo salt '*' test.ping**.

<img width="182" alt="image" src="https://user-images.githubusercontent.com/101214286/229476421-5cef4298-c744-4f63-bd2a-45c80169dfce.png">

## Esimerkit tiloista package, file, service, user, cmd.run
Katsoin opettajan ohjeesta mallia tässä vaiheessa.

### Package
Asensin Apache2:n komennolla **sudo salt '*' state.single pkg.installed apache2**.

<img width="307" alt="image" src="https://user-images.githubusercontent.com/101214286/229481802-7b9591ea-c303-429d-bd81-8776d40c0d1d.png">

### Service
Tarkistin komennolla **sudo salt '*' state.single service.running apache2**, että daemon eli apache2 pyörii taustalla. 

<img width="283" alt="image" src="https://user-images.githubusercontent.com/101214286/229482472-b846349d-6aa0-4e00-81c2-e23df04d0f40.png">

Sammutin sen lopuksi kuitenkin komennolla **sudo salt '*' state.single service.dead apache2**.

<img width="272" alt="image" src="https://user-images.githubusercontent.com/101214286/229483457-ba0c2ea9-cd2e-4ac2-aad0-0b509f19cf6f.png">

#### File
Loin komenolla **sudo salt '*' state.single file.managed '/tmp/hei'** uuden tiedoston tmp kansioon.

<img width="284" alt="image" src="https://user-images.githubusercontent.com/101214286/229484397-3f45d5e0-b711-441d-a26e-d15076230205.png">

### User
User-komennolla käskin molempien koneiden luoda uuden käyttäjän user1:n.

<img width="337" alt="image" src="https://user-images.githubusercontent.com/101214286/229479643-0f49863d-50ba-4179-9463-9d186a471a15.png">

### Cmd.run
Käskin t001-koneen kertomaan komennolla: **sudo salt 't001' state.single cdm.run 'hostname -I'**, mikä sen ip-osoite on.

<img width="292" alt="image" src="https://user-images.githubusercontent.com/101214286/229480783-0ad1899e-7c0b-4ff1-9b36-130b8857542d.png">

## Infra koodina

Luon tiedoston, johon kirjoitan koodia, jonka haluan orjien suorittavan masterin käskiessä. Loin hakemiston **sudo mkdir -p /srv/salt/hello**, johon loin tiedoston komennolla **sudoedit /srv/salt/hello/init.sls**. Se näyttää tältä:

<img width="191" alt="image" src="https://user-images.githubusercontent.com/101214286/229521682-a103e422-d401-475b-a911-627048a194b7.png">

Seuraavaksi loin top.sls tiedoston, jossa määritellään, että mikä orja tekee mitäkin. Tässä tapauksessa kaikki eli molemmat orjat saavat saman käskyn. Tiedoston sisältö on tämä:

<img width="169" alt="image" src="https://user-images.githubusercontent.com/101214286/229516720-a1029c21-af4d-46b0-b440-bf1e052131bb.png">

Tässä lopputulos kun ajan koodin **sudo salt '*' state.apply**:

<img width="208" alt="image" src="https://user-images.githubusercontent.com/101214286/229521808-bb8acd62-c723-48c7-8d79-f35b933f6af9.png">

Lopuksi vielä tuhosin kaikki koneet.

## Lähteet
Näistä lähteistä sain apuja ja neuvoja tehtäviin.

Karvinen, Tero 2023. Infra as Core course, h1 Suolaa. Luettavissa: https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/. Luettu: 3.4.2023.

Karvinen, Tero s.a. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/. Luettu: 3.4.2023.

Valkamo, Tuomas 2022. Create Virtual Machines with Vagrant. Luettavissa: https://tuomasvalkamo.com/CMS-course/week-6/. Luettu: 3.4.2023.

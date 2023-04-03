# h1 - Harjoittelua 
Tehtävän h1 raportti.
## Nettisivun luominen Githubilla - tiivistelmä
Karvinen 2023: https://terokarvinen.com/2023/create-a-web-page-using-github/
- Rekisteröityminen Githubiin
- Uuden repositoryn luominen, määrittele:
  - Julkinen
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

Loin tätä tehtäväkohtaa varten oman kansion /users/vagrant/saltdemo. Ja lisäsin init-tiedoston tähänkin kansioon. Kopioin sinne ready-made-tekstin. Käynnistin sen jälkeen virtuaalikoneet komennolla **vagrant up**.

## Orjien hyväksyminen

Kirjauduin komennolla **vagrant ssh tmaster** ns. isäntäkoneelle, jolla hyväksyn orjat. Hyväksyin siis orjat komennolla **sudo salt-key -A** ja testasin yhteyden ajamalla komennon **sudo salt '*' test.ping**.

<img width="182" alt="image" src="https://user-images.githubusercontent.com/101214286/229476421-5cef4298-c744-4f63-bd2a-45c80169dfce.png">

## Esimerkit tiloista package, file, service, user, cmd.run

### Package
### File
#### Service
### User
User-komennolla käskin molempien koneiden luoda uuden käyttäjän user1:n.

<img width="337" alt="image" src="https://user-images.githubusercontent.com/101214286/229479643-0f49863d-50ba-4179-9463-9d186a471a15.png">

### Cmd.run
Käskin t001-koneen kertomaan komennolla: **sudo salt 't001' cdm.run 'hostname -I'**, mikä sen ip-osoite on.

<img width="235" alt="image" src="https://user-images.githubusercontent.com/101214286/229478560-000efec0-031d-4e91-91ff-8d5c957209c1.png">

## Lähteet kaikkiin tehtäväkohtiin 
Näistä sain apuja ja neuvoja tehtäviin.
Karvinen 2023: https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/ 
https://tuomasvalkamo.com/CMS-course/week-6/

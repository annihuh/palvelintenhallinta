# h1 - Suolaa 
Tehtävän h1 raprtti.
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
Vagrant init debian/bullseye64 asentaakseni Debianin version 11
<img width="270" alt="image" src="https://user-images.githubusercontent.com/101214286/229461896-742e5435-9838-4dce-9842-821ddd17a0ac.png">
Vagrant up käynnistääkseni Vagrantin
<img width="345" alt="image" src="https://user-images.githubusercontent.com/101214286/229462093-207a6e50-4e8b-40e3-ba16-c5df79e3f4c9.png">
Vagrant ssh - varmistin, että psytyn ottamaan yhteyden koneeseen
<img width="305" alt="image" src="https://user-images.githubusercontent.com/101214286/229462181-326fc8a2-817d-4cc5-be37-7228187d516a.png">


Lähteet kaikkiin tehtäväkohtiin
Karvinen 2023: https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/ 
https://tuomasvalkamo.com/CMS-course/week-6/

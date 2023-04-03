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
Ympäristönä on Windows 11 läppäri, jossa kaikki tarpeelliset päivitykset.

Aloitin asennuksen sillä, että päivitin Linux-virtuaalikoneeni komennolla sudo apt-get update. Seuraavaksi ajoin komennon sudo apt-get -y install virtualbox vagrant, asentaakseni Vagrantin koneelleni. Tein komennot vagrant init debian/bullseye64 ja vagrant up seuraavaksi. 

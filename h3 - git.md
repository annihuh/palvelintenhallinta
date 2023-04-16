# Git

Teen tehtävän Windows-järjestelmäisellä läppärillä, jossa kaikki viimeisimmät päivitykset. Teen tehtävän PowerShelliin asennetussa Vagrantissa.

## a) Online

Aloitin tehtävän luomalla Githubiin uuden repository:n nimeltä summeriscoming. Varastoon lisäsin GNU-lisenssin ja README.md:n.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/232288405-ddc8ecd5-2e74-4bbd-90e8-7f2c61969c2b.png">

## b) Dolly

Ensimmäisenä asensin git:n koneelleni komennolla `sudo apt-get install git`. Sen jälkeen ryhdyin kloonaamaan repoa itselleni. Kopioin vihreän code-napin ssh-kohdasta kopioitavan osoitteen juuri luomassani repossa. Liitin sen `git clone` komennon perään komentoriville. Komento siis oli tämä:

    git clone git@github.com:annihuh/summeriscoming.git
    
Komento ei kuitenkaan heti mennyt läpi, koska "hostin" turvallisuutta ei voida taata. Siis seuraavaksi minun piti mennä lisäämään Github ns. hyväksytyksi. Ajoin komennon `ssh-keygen` uuden avainparin luomiseksi. Sen jälkeen siirryin .ssh kansioon ja kopioin julkisen avaimen, jonka liitän Githubiin uutena ssh-avaimena. 

    cd .ssh
    cat id_rsa.pub

Kopioituani avaimen liitin sen Githubissa uudeksi ssh-avaimeksi ja tallensin. 

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/232290281-d5a5a638-27ad-4562-aebc-0bfc2dd8c60c.png">

Sen jälkeen menin kokeilemaan uudestaan `git clone` komentoa, joka onnistui. 

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/232290459-2841abad-7a53-45c3-be72-061b13b5eab9.png">

Siirryin summeriscoming-kansioon cd-komennolla ja ajoin `git pull` komennon saadakseni viimeisimmät tallennukset reposta. Aloin muokkaamaan README.md tiedostoa komennolla `nano README.md`, kirjoitin tekstiä ja tallensin tiedoston. Sen jälkeen ajoin alla olevan komennon. Git add tallentaa muutokset välimuistiin, commit tekee pysyvän talletuksen, pull tuo esim. muiden juuri tekemät muutokset ja push lähettää itse tekemäni muutokset eteenpäin.

    git add . && git commit; git pull && git push
    
Komento ei kuitenkaan heti mennyt oletetusti - että olisin päässyt tekemään muokkaukselle kuvauksen - joten ilmoituksen mukaisesti muutin käyttäjänimeni komennolla `git config --global user.name "annihuh"`. Tämän jälkeen kun ajoin edellisen komennon muokkaus tallentui niin kuin kuuluikin. Kuva selaimesta:

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/232316384-a3e52cad-a674-4616-bbed-7866039c491f.png">

## c) Doh!

Tässä vaiheessa aloin tekemään muokkausta, jonka peruutan edelliseen versioon `git reset --hard` avulla. Tein muokkauksen README.md tiedostoon ja tallensin sen. Sen jälkeen ajoin reset-komennon, joka palautti kaiken edelliseen edittiin.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/232319242-9b28025a-f6ca-40ef-9e49-72f982ec0cdb.png">

## d) Tukki

Kuva selitettävästä patch-tiedostosta.

<img width="auto" alt="image" src="https://user-images.githubusercontent.com/101214286/232319592-b1225148-8a6e-4f92-a6bf-cfe68c761e03.png">

Patch historia kertoo milloin muutoksia on tehty, mihin ja kuka niitä on tehnyt. Jokainen muokkaus näkyy erikseen: punainen tarkoittaa muokattua tai postettua ja vihreä on lisäyksen merkki. Muutosten yläpuolella kerrotaan myös, mitä commit-viestiin on kirjoitettu. Näiden alapuolella on pitkä vihreä teksti, jossa kerrotaan GNU-lisenssistä.

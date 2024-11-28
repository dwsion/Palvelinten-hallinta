

## a) Puolikas. Tee ensimmäinen vedos omasta modulista. Tätä jatketaan vielä, eli valmista ei tarvitse tulla. Keskeneräisen modulin pohjalta saat vinkkejä haastaviin kohtiin ja oikeaan kehityssuuntaan. Lopullisen modulin tulee olla modernia keskitettyä hallintaa, eli idempotentti ja infraa koodina. 


Omana moduulina suunnittelin tekeväni jonkinlaisen nes emulaattorin asennuksen, mutta pienen testailun jälkeen mieleen tuli toinen idea.

Ajatuksena on tehdä moduuli, joka testaa minionin portteja ja avaa/sulkee ne tarvittaessa. 

Aloitin tuhoamalla ensin kaikki Vagrantilla luodut koneet ja asensin ne sitten uudelleen ja käytin Vagrantfilenä samaa Teron ohjeesta löytyvää konfiguraatiota.

Tämän jälkeen asensin kaikille koneille Salt-masterin/minionin, riippuen koneesta:

minionin tapauksessa komennot näyttivät tältä:


    1. sudo mkdir -p /etc/apt/keyrings

    2. sudo apt-get install curl
  
    3. curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp
  
    4. curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources

    5. sudo apt-get update

    6. sudo apt-get install salt-minion

    7. sudo systemctl enable salt-minion && sudo systemctl start salt-minion

    8. sudo salt call --version

    9. sudo apt-get install micro

    10. micro /etc/salt/minion

Asennustiedostoon masterin tiedot:

![image](https://github.com/user-attachments/assets/f1826934-797a-43a0-a745-62e67a95acb3)

ja uudelleenkäynnistys:

    sudo systemctl restart salt-minion.service

    11. exit


Tein tämän saman vielä toisella minionille, jonka jälkeen siirryin masterille kutsumaan ja hyväksymään minioneita.

![image](https://github.com/user-attachments/assets/8f643346-2f5a-497c-a8ed-ac258ac99913)

![image](https://github.com/user-attachments/assets/ec38c390-66e5-4175-a52e-324bda2227e2)

![image](https://github.com/user-attachments/assets/f957e9a1-00f0-44eb-8851-e49573e09cf5)

Nyt vuorossa oli sitten kokeilla omaa moduulia, jonka aiheena oli siis asentaa ufw ja porttien tsekkaus orjilla ja tarvittaessa niiden avaaminen/sulkeminen.

Aloitin luomalla kansiot:

    sudo mkdir -p /srv/salt/states
    
    sudo mkdir -p /srv/salt/_modules

![image](https://github.com/user-attachments/assets/3f13f654-370c-47d4-b102-e5c36f420c2e)

Kirjoitin sls. tiedoston:

![image](https://github.com/user-attachments/assets/40d8a83b-df9d-456e-8782-7f4bdc4202fc)

Sitten luodaan tuo Python-moduuli, joka tarkistaa porttien tilan:

![image](https://github.com/user-attachments/assets/44b70121-a975-42ea-978c-ccae302a3b18)

Synkronoin masterilta tuon port chekkerin minion koneille:

![image](https://github.com/user-attachments/assets/38590e76-9d73-4b5b-99a2-cfd79d95e568)



Ajoin komennon  sudo salt '*' state.apply  :

![image](https://github.com/user-attachments/assets/dba5cda6-7753-451e-ba14-9f2875e8a406)













## Lähteet

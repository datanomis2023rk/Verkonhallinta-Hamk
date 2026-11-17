# Viikko 4 – Ansible ja Infrastructure as Code

1. Johdanto

IaC Infrastructure as Code on ympäristön asennusten ja hallinnan automatisointia luotujen ohjelmakooditiedostojen avulla.

2. Inventory

Ympäristön rakenne.

# Lähtötilanne

Snmp ongelma heijastuu myös shh avainten luontiin. Jotain en osaa konfata oikein? Käyttöoikeudet apt var/...?

=== Installing SSH Server ===
E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
E: Unable to lock directory /var/lib/apt/lists/
W: Problem unlinking the file /var/cache/apt/pkgcache.bin - RemoveCaches (13: Permission denied)
W: Problem unlinking the file /var/cache/apt/srcpkgcache.bin - RemoveCaches (13: Permission denied)

Kirjaudu Ansible-palvelimelle:
# ok bash> docker exec -it clab-hamk-verkonhallinta-golden-ansible bash

Tarkista inventory:
# ok bash> cat /ansible/inventory.ini

Tarkista yhteydet:
# ok bash> ansible all -i /ansible/inventory.ini -m ping

# Tehtävä 4.1 – Tutustu inventoryyn

Laitteet:
- Reitittimet
- Työasemat
- Serverit

Ryhmät reitittien ja verkkojen mukaisesti:
1. Asiakasverkkoon kuuluvat hostit
2. Serveriverkkoon kuuluvat hostit
3. Haarakonttorin verkkkoon kuuluvat hostit

Ryhmät eri verkkoihin kuuluvista samankaltaisista hosteista.
1. Voidaan esim. playbookeissa identifoida kuuluvaksi samaan ryhmään
2. Ryhmän jäsenille voidaan tehdä sama toimenpide

Ryhmien muodostaminen useista näkökulmista auttaa hahmottamaan riippuvuuksia ja toimintoketjuja.


3. SNMP Playbook // 

Koodi ja tulokset.

# Tehtävä 4.2 – Suorita ensimmäinen playbook

Siirry playbook-hakemistoon:
# ok bash> cd /ansible/playbooks

Suorita:
# bash> ansible-playbook -i ../inventory.ini ping.yml

Dokumentoi:
- mitä playbook tekee
- mitä tuloksista voidaan päätellä


# Tehtävä 4.3 – SNMP:n automatisointi

Luo playbook:
# text> install-snmp.yml

Playbookin tulee:
- päivittää pakettilista
- asentaa snmp
- asentaa snmpd
- käynnistää palvelu

Suorita playbook seuraaville koneille:
- web1
- db1
- branch-client

Varmista asennus.


4. Node Exporter Playbook

Koodi ja tulokset.

# Tehtävä 4.4 – Node Exporterin automatisointi

Luo playbook:
# text> install-node-exporter.yml

Playbookin tulee:
- luoda hakemisto
- ladata Node Exporter
- purkaa paketti
- käynnistää palvelu

Asenna vähintään:
- web1
- db1


# Tehtävä 4.5 – Kerää järjestelmätietoja

Suorita:
# bash> ansible all -i ../inventory.ini -m setup

Kerää tiedot:
- käyttöjärjestelmä
- IP-osoite
- prosessorien määrä
- muistin määrä

Dokumentoi tulokset taulukkoon.


# Tehtävä 4.6 – Analyysi
5. Vertailu

Käsin vs. automaatio.
- käsin tehtyjä asennuksia
- Ansiblella tehtyjä asennuksia

6. Yhteenveto

- mitä hyötyjä automaatiosta on
- missä tilanteissa automaatio on välttämätöntä
















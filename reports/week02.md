Viikko 2 – SNMP ja verkon perustason valvonta

1. Johdanto

SNMP-protokolla (Simple Network Management Protocol) muodostuu kahdesta pääkomponentista.

SNMP Agentti toimii valvottavassa laitteessa kuten reititin ja vastaa valvontapalvelimen kyselyihin. 

SNMP Manager toimii valvovassa palvelimessa kuten Zabbix ja kerää tietoa agenteilta.

2. Asennus

Varmista että ympäristö on käynnissä:
# bash> bash scripts/status.sh

Tarvittaessa käynnistä ympäristö:
# bash> bash scripts/deploy.sh

# 2.1 – SNMP-agentin asennus

Kirjaudu web1-palvelimelle:
# ok bash> docker exec -it clab-hamk-verkonhallinta-golden-web1 bash

Asenna SNMP-agentti:
# ok bash> apt update && apt install snmp snmpd -y

Tarkista palvelun tila:
# bash> systemctl status snmpd
tai
# ok bash> service snmpd status

# 2.2 – SNMP-konfigurointi

Avaa konfiguraatiotiedosto:
# ok bash> apt install nano
# ok bash> nano /etc/snmp/snmpd.conf

Lisää tai muuta yhteisön nimeksi:
# ok text> rocommunity public

Käynnistä palvelu uudelleen:
# ok bash> service snmpd restart

Tarkista että prosessi on käynnissä:
# ok bash> ps aux | grep snmpd


# 2.3 – Ensimmäinen SNMP-kysely

Kirjaudu ansible-palvelimelle:
# ok bash> docker exec -it clab-hamk-verkonhallinta-golden-ansible bash

Asenna työkalut:  
# ok bash> apt update && apt install snmp -y

Testaa yhteys:
# bash> snmpwalk -v2c -c public web1 system 

- järjestelmän nimi
- käyttöjärjestelmä
- uptime 


3. Kerätyt tiedot

   Seuraavien tietojen hankkiminen onnistui, mutta epäselvyyttä snmp-komentojen käytöstä: system/localhost; web1/ansible.
   Labran alustuksesta huolimatta snmp-tehtävän loppuosa 4-6 jää tekemättä. 

# 2.4 – Kerää järjestelmätiedot

Suorita seuraavat kyselyt:

Järjestelmän nimi: web1

# bash> snmpget -v2c -c public web1 sysName.0

Järjestelmän kuvaus: Linux web1 6.18.58.1-microsoft-standard-WSL2 

# bash> snmpget -v2c -c public web1 sysDescr.0

Käyttöaika: (308014) 0:51:20.14

# bash> snmpget -v2c -c public web1 sysUpTime.0


4. Verkkorajapinnat

SNMP:n avulla kerätyt rajapintatiedot.
# 2.5 – Verkkorajapinnat

Listaa rajapinnat:

# bash> snmpwalk -v2c -c public web1 ifDescr

- montako verkkorajapintaa löytyi
- mikä rajapinta yhdistää laitteen verkkoon

5. OID-analyysi

OID-objektien käyttötarkoitus.

# 2.6 – MIB-objektien tutkiminen

OID             Tarkoitus 

sysName.0       Laitteen nimi
sysDescr.0      Järjestelmän kuvaus
sysUpTime.0     Käyttöaika
ifDescr         Rajapinnan nimi
ifOperStatus    Todellinen toimintatila

# bash> snmptranslate

tai internetistä löytyviä MIB-hakemistoja.


# 2.7 – Usean laitteen valvonta

Asenna SNMP-agentti myös seuraaville laitteille:

- db1
- branch-client

Suorita kyselyt kaikille kolmelle laitteelle.

Täydennä taulukko:

Laite           Nimi            Käyttöjärjestelmä           Uptime 

web1
db1
branch-client


6. Pohdinta

Omat havainnot SNMP:n hyödyistä ja rajoituksista.

1. Mitä hyötyä SNMP:stä on verkonhallinnassa?
2. Mitä tietoa SNMP:n avulla voidaan kerätä?
3. Mitä ongelmia yhteisöpohjaisessa SNMPv2:ssa on?
4. Missä tilanteissa käyttäisit mieluummin SNMPv3:a?

Mahdolliset kuvakaappaukset:

reports/images/

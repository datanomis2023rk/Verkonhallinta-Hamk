Viikko 6 – Zabbix ja keskitetty verkonvalvonta

1. Johdanto

Zabbix on keskitetysti hallittava valvontajärjestelmä verkkoympäristölle seuraten jatkuvasti sen tilaa ja toimintaa.

# Lähtötilanne

ASENSIN APACHENKIN WEB1 KONTTIIN, MUTTA PERUSONGELMANA ON: SHH EI TOIMINNASSA => SNMP EI TOIMI => ZABBIX EI SAA DATAA

Varmista että ympäristö on käynnissä:
# bash> bash scripts/status.sh

Avaa Zabbix selaimella:
# ok text> http://localhost:8080

Kirjaudu järjestelmään.


# Tehtävä 6.1 – Tutustu Zabbixiin

Tutki käyttöliittymän seuraavat osat:
- Host refers to any physical or virtual device, application, service, or any other logically-related collection of monitored parameters.
- Template is a set of entities that can be conveniently applied to multiple hosts.
- Monitoring provides a way to create a hierarchical representation of monitored data.
- Dashboards
- Alerts
- Reports

Kirjoita raporttiin lyhyt kuvaus jokaisesta.

2. Hostien lisääminen

Lisätyt laitteet ja niiden valvonta.

# Tehtävä 6.2 – Lisää web1 valvontaan

Lisää hostiksi:
# text> web1

Määritä:
- Hostname
- IP-osoite
- Host Group

Liitä vähintään yksi soveltuva template.

Esimerkiksi:
# text> Linux by Zabbix agent
tai
# text> Linux by SNMP


# Tehtävä 6.3 – Lisää db1 valvontaan

Lisää:
# text> db1
hostiksi.

Varmista että mittareita alkaa saapua.

Tarkista että vähintään seuraavat tiedot näkyvät:
- CPU
- Memory
- Uptime
- Network Traffic


# Tehtävä 6.4 – Lisää branch-client valvontaan

Lisää:
# text> branch-client
hostiksi.

Tarkista että kaikki kolme laitetta näkyvät Zabbixissa.
4. Mittarit

Valitut mittarit ja niiden merkitys.

# Tehtävä 6.5 – Valvontamittarien analyysi

Valitse vähintään viisi mittaria.

Esimerkkejä:
- CPU Usage
- Memory Usage
- Load Average
- Uptime
- Disk Usage
- Network Traffic

Dokumentoi jokaisesta:
- mittarin nimi
- arvo
- mitä mittari kertoo
- miksi sitä kannattaa seurata

Täydennä taulukko:

Mittari      Arvo        Merkitys 


3. Dashboard

Kuvakaappaus dashboardista.

# Tehtävä 6.6 – Dashboardin luominen

Luo dashboard nimellä:
# text> Golden Topology Status

Dashboardilla tulee näkyä vähintään:
- CPU-kuorman seuranta
- Muistinkäyttö
- Levytilan käyttö
- Verkkoliikenne
- Hostien tila

Dashboardin tulee antaa yhdellä silmäyksellä kokonaiskuva ympäristöstä.

Tallenna kuvakaappaus.

5. Triggerit

Määritetyt triggerit ja niiden toiminta.

# Tehtävä 6.7 – Triggerien määrittäminen

Luo vähintään kaksi triggeriä.

## Triggeri 1
CPU-kuormitus:
# text> CPU > 80 %

## Triggeri 2
Levytila:
# text> Free disk space < 20 %

Dokumentoi:
- Triggerin nimi
- Ehto
- Vakavuusluokka

6. Hälytys- ja häiriötestit

Suoritetut testit ja tulokset.

# Tehtävä 6.8 – Hälytyksen simulointi

Aiheuta hälytys.

## Vaihtoehto A

CPU-kuormitus:
# bash> yes > /dev/null

## Vaihtoehto B
Levytilan kulutus:
# bash> dd if=/dev/zero of=testfile.img bs=1M count=1000

Tarkkaile:
- syntyykö triggeri
- kuinka nopeasti hälytys ilmestyy

Ota kuvakaappaus.

# Tehtävä 6.9 – Häiriötilanne

Pysäytä palvelu.

Esimerkiksi:
# bash> service apache2 stop
tai
# bash> pkill node_exporter

Tarkkaile:
- näkyykö tilanne Zabbixissa
- kuinka nopeasti valvontajärjestelmä reagoi

Dokumentoi havaintosi.

7. Vertailu

SNMP vs Prometheus vs Zabbix.
# Tehtävä 6.10 – Kurssin työkalujen vertailu

Täydennä seuraava taulukko.

Ominaisuus      SNMP    Prometheus      Zabbix 

Tiedonkeruu 
Dashboardit
Hälytykset
Käyttöönotto 
Skaalautuvuus 
Yrityskäyttö 


8. Yhteenveto

Kurssin tärkeimmät opit.
# Tehtävä 6.11 – Pohdinta

Vastaa seuraaviin kysymyksiin:

1. Mitä hyötyä keskitetystä valvonnasta on?
2. Mitkä mittarit ovat mielestäsi tärkeimpiä?
3. Millaisista tilanteista ylläpitäjän pitäisi saada hälytys?
4. Missä tilanteissa käyttäisit Prometheusta?
5. Missä tilanteissa käyttäisit Zabbixia?
6. Mitä valvontatoimintoja lisäisit tähän ympäristöön?

raportissa käytetyt kuvat:
# text> reports/images/
# text>
reports/
├── week06.md
└── images/
    ├── dashboard.png
    ├── hosts.png
    ├── trigger-cpu.png
    └── alert-test.png

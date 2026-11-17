# Viikko 3 – Prometheus, Node Exporter ja Grafana

1. Johdanto

Prometheus-järjestelmä koostuu seuraavista komponenteista:

- Node Exporter kerää käyttöjärjestelmän suorituskykytietoja kuten CPU-kuorma, muistin tai levytilan käyttö ja verkkoliikenne.

- Prometheus kerää mittarit säännöllisesti agentilta ja tallentaa tiedot aikasarjatietokantaan.

- Grafana visualisoi mittarit dashboardeihin, analysointiin ja raportointiin.

2. Node Exporterin käyttöönotto

# Lähtötilanne

Varmista että ympäristö on käynnissä:
# bash> bash scripts/status.sh

Avaa Prometheus:
# text> http://localhost:9090

Avaa Grafana:
# text> http://localhost:3000

# Tehtävä 3.1 – Node Exporterin asennus

Kirjaudu web1-koneelle:
# ok bash> docker exec -it clab-hamk-verkonhallinta-golden-web1 bash

Asenna tarvittavat työkalut:
# ok bash> apt update && apt install wget tar -y

Lataa Node Exporter.

Selvitä uusin saatavilla oleva versio GitHubista ja lataa se:
# ok bash> wget https://github.com/prometheus/node_exporter/releases/latest/download/node_exporter-1.12.1.linux-amd64.tar.gz

Pura paketti:
# ok bash> tar xvf node_exporter-1.12.1.linux-amd64.tar.gz

Siirry hakemistoon:
# ok bash> cd node_exporter-1.12.1.linux-amd64

Käynnistä Node Exporter:
# ok bash> ./node_exporter

Pidä tämä terminaali auki.


# Tehtävä 3.2 – Tarkista exporterin toiminta

Avaa uusi terminaali.

Tarkista että Node Exporter vastaa:
# bash> curl http://localhost:9100/metrics

Tuloksena pitäisi näkyä suuri määrä mittareita.

Esimerkiksi:

# text>
node_cpu_seconds_total
node_memory_MemTotal_bytes
node_filesystem_size_bytes

Ota kuvakaappaus raporttia varten.

3. Prometheus

Targets-sivun tarkastelu ja havainnot.
# Tehtävä 3.3 – Tarkista että Prometheus näkee palvelimen

Avaa Prometheus selaimessa:
# text> http://localhost:9090

Valitse:
# text> Status → Targets

Tarkista että:
- web1 näkyy kohteena
- tila on UP

Jos kohde ei näy:
- tarkista Node Exporterin toiminta
- tarkista Prometheus-konfiguraatio
- tarkista verkon yhteydet

Lisää kuvakaappaus raporttiin.


# Tehtävä 3.4 – Grafanan tietolähde

Kirjaudu Grafanaan.

Lisää tietolähteeksi:
# text> Prometheus

Palvelimen osoite:
# text> http://prometheus:9090

Testaa yhteys.

Tallenna kuvakaappaus onnistuneesta yhteydestä.


4. Dashboard ok

Kuvakaappaus koko dashboardista.

# Tehtävä 3.5 – Luo dashboard

Luo uusi dashboard nimellä:
# ok text> Golden Topology Monitoring

Lisää seuraavat paneelit.

## CPU

# promql> 100 - (avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
Paneelin nimi:
# text> CPU Usage %

Muisti
# promql>(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes)/node_memory_MemTotal_bytes* 100
Paneelin nimi:
# text> Memory Usage %

Levytila
# promql> 100 -(node_filesystem_avail_bytes/node_filesystem_size_bytes* 100)
Paneelin nimi:
# text> Disk Usage %

Verkkoliikenne (saapuva)
# promql> rate(node_network_receive_bytes_total[5m])
Paneelin nimi:
# text> Network Receive

Verkkoliikenne (lähtevä)
# promql> rate(node_network_transmit_bytes_total[5m])
Paneelin nimi:
# text> Network Transmit


5. Kuormitustesti

Mittareiden käyttäytyminen kuormituksen aikana.
# Tehtävä 3.6 – Kuormituksen generointi

Aiheuta kuormitusta web1-palvelimelle.

Levykuormitus
# bash> dd if=/dev/zero of=testfile.img bs=1M count=500

CPU-kuormitus
# bash> yes > /dev/null
Tämä tuottaa yhdelle cpu säikeellä lähes 100% kuorman. 

Tähän parempi, hallittavampi tapa voisi olla käyttää työkalua stress-ng:
# ok bash> sudo apt install stress-ng
# ok bash> stress-ng --cpu 4 --timeout 60s
Tämä kuormittaa neljää CPU-ydintä 60 sekunnin ajan ja lopettaa itse automaattisesti. Se on usein parempi vaihtoehto laboratorio- ja monitorointitesteissä kuin yes > /dev/null.

Tarkkaile dashboardin muutoksia.

Vastaa raportissa: Melko rauhallisia muutoksia kaikissa mittareissa.
- Miten CPU-käyrä muuttui? 
- Miten levytilan käyttö muuttui?
- Näkyikö verkkoliikenteessä muutoksia?

Lisää kuvakaappaukset.

6. SNMP vs Prometheus

Vertailutaulukko ja johtopäätökset.

# Tehtävä 3.7 – SNMP vs Prometheus

Vertaa viikon 2 SNMP-ratkaisua viikon 3 Prometheus-ratkaisuun.

Itsellä Prometheuksen käyttöönotto sujui hyvin toisin kuin snmp.

Täydennä taulukko:

Ominaisuus                      SNMP                     Prometheus 

Tiedonkeruu                                             ok
Käyttöönotto                                            ok
Mittarien määrä                                         ok
Visualisointi                                           ok
Hälytysmahdollisuudet                                   en kokeillut
Soveltuvuus pilviympäristöihin                          riittääkö?

7. Yhteenveto

7.1. Mitä hyötyä Prometheuksesta on verrattuna SNMP:hen? 
# Olisiko kyselykuorman kannalta exporterin aktiiviset agentit parempia?
7.2. Millaisia mittareita ylläpitäjän kannattaa seurata jatkuvasti? 
# Toiminnan laadun ja järjestelmän pystyssä pysymisen kannalta kriittisiä.
7.3. Mitä tietoa dashboardisi tarjoaa ylläpitäjälle?
# Pelkän yleiskuvan resurssien käytöstä.
7.4. Mitä uusia mittareita lisäisit dashboardiin?
# Sellaisia, jotka ilmaisevat resurssien käytön aiheuttajia.
7.5. Miten monitorointitiedosta voisi olla hyötyä vianetsinnässä?
# Lokeja tutkimalla.

raportissa käytetyt kuvat:
# text> reports/images/

Esimerkiksi:
# text>
reports/
├── week03.md
└── images/
    ├── grafana-dashboard.png
    ├── prometheus-targets.png
    ├── node-exporter-metrics.png
    └── load-test.png



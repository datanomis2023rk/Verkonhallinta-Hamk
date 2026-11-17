Viikko 5 – Tietoturva, lokit ja verkkoliikenteen analysointi

1. Johdanto

Verkonhallinnan on kyettävä tunnistamaan poikkeava liikenne, tietoturvauhat, väärinkäytökset ja virheelliset konfiguraatiot.


# Lähtötilanne

Kirjaudu attacker-konttiin:
# ok bash> docker exec -it clab-hamk-verkonhallinta-golden-attacker bash

Kirjaudu web1-konttiin:
# ok bash> docker exec -it clab-hamk-verkonhallinta-golden-web1 bash

2. Kaappaus

Miten liikenne kerättiin.
# Tehtävä 5.1 – Liikenteen kaappaus

Aloita kaappaus web1-palvelimella:
# bash> tcpdump -i eth1 -w web1.pcap

root@web1:/# tcpdump -i eth1 -w web1.pcap
tcpdump: eth1: No such device exists
(No such device exists)

Pidä kaappaus käynnissä.

3. Porttiskannaus

Tulokset ja analyysi.
# Tehtävä 5.2 – Porttiskannaus

Suorita attacker-koneelta:
# bash> nmap 10.10.20.101

Keskeytä kaappaus.

Tallenna:
# text> web1.pcap

4. Wireshark

Havaintoja liikenteestä.


# Tehtävä 5.3 – Wireshark-analyysi

Avaa kaappaus Wiresharkilla.

Selvitä:
- mistä IP-osoitteesta skannaus tuli
- mitä portteja testattiin
- mitä vastauksia palvelin lähetti

Lisää kuvakaappauksia raporttiin.


# Tehtävä 5.4 – DNS-liikenne

Suodata:
# text> dns

Tunnista:
- DNS-kyselyt
- DNS-vastaukset
- käytetyt palvelimet

Dokumentoi havainnot.


# Tehtävä 5.5 – HTTP-liikenne

Suodata:
# text> http

Tunnista:
- HTTP-pyynnöt
- HTTP-vastaukset
- palvelimet

5. Loki-analyysi

Tutkitut lokit ja löydökset.

# Tehtävä 5.6 – Loki-analyysi

Tarkastele:
# bash> /var/log/syslog
tai
# bash> journalctl

Etsi:
- kirjautumisia
- virhetilanteita
- palveluiden käynnistyksiä

Dokumentoi löydökset.

6. Turvallisuusarvio

Parannusehdotukset.
# Tehtävä 5.7 – Turvallisuusarvio

Arvioi ympäristöä.

Tunnista vähintään kolme asiaa:
- mikä on turvallista
- mikä on tietoturvariski
- mitä pitäisi parantaa

7. Yhteenveto

1. Miten porttiskannaus näkyi liikenteessä?
2. Miten se näkyi lokeissa?
3. Mitä hyötyä verkkoliikenteen analysoinnista on?
4. Miten monitorointia voisi hyödyntää hyökkäysten tunnistamisessa?













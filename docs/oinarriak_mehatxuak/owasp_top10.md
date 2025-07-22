# 1.3 Web ahultasun arrunten ikuspegi orokorra (OWASP Top 10)

## Zer da OWASP Top 10?

OWASP (Open Web Application Security Project) erakundeak web aplikazioetako 10 arrisku kritikoen zerrenda eguneratzen du. 2021eko bertsioa da egungoa, baina uda honetan edo udazkenean berrizteko asmoa dute.

<a href="https://owasp.org/www-project-top-ten/" target="_blank">OWASP Top 10</a>

![OWASP Top 10](images/owasp_top10.png)
> Aurreko irudian 2017 eta 2021-ren arteko ezberdintasunak ikus daitezke.

## 1. Sarbide-kontrol hauskorrak (Broken Access Control)

Erabiltzaileek soilik baimendutako ekintzak bakarrik egin ditzaten mugatzea huts egiten duenean gertatzen da.
<a href="https://owasp.org/Top10/A01_2021-Broken_Access_Control/" target="_blank">OWASP Top 10 - Broken Access Control</a>


### Adibideak
- Baimenik gabeko fitxategi pribatura sartzea
- Beste erabiltzaileen datuak editatzea
- Administrazio-funtzioetarako sarbidea izatea baimenik gabe

### Prebentzioa
- Sarbide-kontrola aplikatu lehentasunez
- "Baimenik ez" jarrerarekin hasi
- Erabiltzaile-zerbitzari aldean sarbidea egiaztatu

### Erronkak

- [Broken Access Control](../ariketak/broken_access_control.md)

## 2. Zifraketa-akatsak (Cryptographic Failures)


Datuak zifratzeko erabiltzen diren ahultasunak edo akatsak.
<a href="https://owasp.org/Top10/A02_2021-Cryptographic_Failures/" target="_blank">OWASP Top 10 - Cryptographic Failures</a>

### Adibideak
- Datuak zifratu gabe transmititzea
- Zifratze ahula erabiltzea (MD5, SHA1, DES...)
- Gako pribatuen kudeaketa txarra

### Prebentzioa
- Erabili indarrean dauden zifratze estandarrak (AES, RSA...)
- HTTPS erabili beti
- Gakoak modu seguruan gorde

### Erronkak

- [Cryptographic Failures](../ariketak/cryptographic_failures.md)

## 3. Injekzioa (Injection)


Erasotzaileak kode maltzurra sartzen du interpretatzen den komando edo kontsulta baten barruan.
<a href="https://owasp.org/Top10/A03_2021-Injection/" target="_blank">OWASP Top 10 - Injection</a>

### Adibideak
- **SQL injekzioa**: 
  ```sql
  -- Erabiltzailearen sarrera: ' OR '1'='1
  SELECT * FROM users WHERE username = '' OR '1'='1' AND password = '...'
  ```
- **Komando-injekzioa**: `; rm -rf /`
- **LDAP injekzioa**: `*)(uid=*))(|(uid=*`

### Prebentzioa
- Parametro kontsultak erabili (prepared statements)
- ORM seguruak erabili
- Sarrera guztiak balioztatu eta garbitu

### Erronkak

- [Injection](../ariketak/injection.md)

## 4. Diseinu ez segurua (Insecure Design)


Segurtasun-arazoak diseinu-fasean sortzen direnean, eta ez bakarrik inplementazioan.
<a href="https://owasp.org/Top10/A04_2021-Insecure_Design/" target="_blank">OWASP Top 10 - Insecure Design</a>

### Adibideak
- Segurtasun-irizpiderik gabeko diseinua
- Mehatxuen eredurik ez
- Abusuzko erabileraren aurkako mekanismorik ez

### Prebentzioa
- Segurtasuna hasieratik kontuan hartu diseinuan
- Mehatxuen ereduak egin
- Abusu-kasuak identifikatu eta prebenitu

### Erronkak

- [Insecure Design](../ariketak/insecure_design.md)

## 5. Konfigurazio oker eta ahula (Security Misconfiguration)


Segurtasun-konfigurazio egokirik ez izatea edo konfigurazio okerrak izatea.
<a href="https://owasp.org/Top10/A05_2021-Security_Misconfiguration/" target="_blank">OWASP Top 10 - Security Misconfiguration</a>

### Adibideak
- Konfigurazio lehenetsiak aldatu gabe uztea
- Mezuen erakuspen detaileak erakustea
- Segurtasun-neurri osagarriak aktibatu gabe uztea

### Prebentzioa
- Konfigurazio seguru bat ezarri
- Automatizatu konfigurazioa
- Eguneratu eta berrikusi konfigurazioa maiz

### Erronkak

- [Security Misconfiguration](../ariketak/security_misconfiguration.md)

## 6. Konponente bulnerable eta zaharkituak (Vulnerable and Outdated Components)


Hirugarreneko liburutegi eta osagaietan dauden ahultasunak.
<a href="https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/" target="_blank">OWASP Top 10 - Vulnerable and Outdated Components</a>

### Adibideak
- Ezagutzen diren ahultasunen berri ez izatea
- Eguneraketarik gabe uztea
- Osagai ez seguruak erabiltzea

### Prebentzioa
- Osagaiak eguneratu maiz
- Ez erabili osagai zaharkiturik
- Eskaneatu osagaiak ahultasunen bila

### Erronkak

- [Vulnerable and Outdated Components](../ariketak/vulnerable_and_outdated_components.md)

## 7. Autentifikazio-akatsak (Identification and Authentication Failures)


Erabiltzaileak identifikatu eta autentifikatzeko arazoak.
<a href="https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/" target="_blank">OWASP Top 10 - Identification and Authentication Failures</a>

### Adibideak
- Pasahitz ahulak onartzea
- Saio-kudeaketa txarra
- Autentifikazio-funtzionaltasun ahulak

### Prebentzioa
- Inplementatu autentifikazio anizkoitza (2FA)
- Erabili pasahitz-politika sendoak
- Egiaztatu saio-identifikatzaileak

### Erronkak

- [Identification and Authentication Failures](../ariketak/identification_and_authentication_failures.md)

## 8. Datuak eta datuen osotasunaren galera (Software and Data Integrity Failures)


Datuen osotasuna eta jatorriaren ziurtasuna arriskuan jartzen direnean.
<a href="https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/" target="_blank">OWASP Top 10 - Software and Data Integrity Failures</a>

### Adibideak
- Kode edo datu aldaketa baimenik gabe
- Erabiltzailearen sarrerak egiaztatu gabe
- Sinadura digitalik gabeko kodeak

### Prebentzioa
- Erabili sinadura digitalak
- Egiaztatu kode eta datuen jatorria
- Inplementatu osotasunaren egiaztapena

### Erronkak

- [Software and Data Integrity Failures](../ariketak/software_and_data_integrity_failures.md)

## 9. Segurtasunaren monitorizazio falta (Security Logging and Monitoring Failures)


Erasoak detektatu eta erantzuteko gaitasun falta.
<a href="https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/" target="_blank">OWASP Top 10 - Security Logging and Monitoring Failures</a>

### Adibideak
- Erregistroak gordetzeko konfiguraziorik ez
- Alertarik ez jasotzea
- Erantzun-prozedurarik ez izatea

### Prebentzioa
- Ezarri erregistro-sistema egokia
- Konfiguretu alerta automatikoak
- Planifikatu erantzun-prozedurak

### Erronkak

- [Security Logging and Monitoring Failures](../ariketak/security_logging_and_monitoring_failures.md)

## 10. Zerbitzariaren aldeko eskaera-faltsuketa (Server-Side Request Forgery - SSRF)


Erasotzaileak zerbitzariak barne-sarean egindako eskaerak kontrolatzen dituenean.
<a href="https://owasp.org/Top10/A10_2021-Server_Side_Request_Forgery/" target="_blank">OWASP Top 10 - Server-Side Request Forgery</a>

### Adibideak
- Barne-zerbitzuei eskaerak egitea
- Konfigurazio-informazioa eskuratzea
- Barne-sareko baliabideetara sartzea

### Prebentzioa
- Egiaztatu eta iragazi sarrerako URL guztiak
- Erabili zerrenda zuri bat onartutako helbideentzako
- Ezarri konexio-mugen eta denbora-mugen politika

### Erronkak

- [Server-Side Request Forgery](../ariketak/server_side_request_forgery.md)

## Hurrengo urratsak

- [Injekzioen aurkako defentsak](../../eraso_defentsak/injekzioak.md)


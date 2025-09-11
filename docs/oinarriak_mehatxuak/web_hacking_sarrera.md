# Web Hacking-erako sarrera

## Walking An Application

Ikusiko dugu web aplikazio bat eskuz nola aztertu segurtasun arazoak aurkitzeko, nabigatzailearen tresna integratuak soilik erabiliz. Gehienetan, segurtasun tresna automatizatuek eta scriptek ahultasun potentzial eta informazio erabilgarri asko galduko dituzte.

Hona hemen gela osoan zehar erabiliko dituzun nabigatzailearen tresna integratuen laburpen labur bat:

- **Iturria ikusi (View Source)**: Nabigatzailea erabili webgune baten giza irakurgarria den iturburu-kodea ikusteko.
- **Ikuskatzailea (Inspector)**: Orrialdeko elementuak nola aztertu eta aldaketak nola egin ikasi, normalean blokeatutako edukia ikusteko.
- **Araztailea (Debugger)**: Orrialde baten JavaScript-aren fluxua aztertu eta kontrolatu.
- **Sarea (Network)**: Orrialde batek egiten dituen sare-eskaera guztiak ikusi.

## Exploring The Website

Penetrazio probatzaile gisa, zure rola webgune edo web aplikazio bat aztertzean da ahulak izan daitezkeen ezaugarriak aurkitzea eta horiek ustiatzen saiatzea ahulak diren ala ez ebaluatzeko. Ezaugarri hauek normalean webgunearen erabiltzailearekin elkarrekintzaren bat eskatzen duten zatiak izaten dira. Webgunearen zati interaktiboak aurkitzea hain erraza izan daiteke login formulario bat ikustea bezain erraza, edota webgunearen JavaScript-a eskuz aztertzea bezain konplexua.
Hasteko leku bikaina da nabigatzailearekin webgunea esploratzea eta orrialde/area/ezaugarri indibidualak ohar hartzea, bakoitzaren laburpen batekin.
Acme IT Support webgunearen azterketa adibide bat honelakoa izango litzateke:


## Orriaren iturburua ikustea

Webgune bat bisitatzen dugunean, gure nabigatzaileak eskaera bat egiten dio zerbitzariari, eta horrek **kode bat** itzultzen du: hau da page source edo **orrialdearen iturburua**. Kode horrek adierazten dio nabigatzaileari zer eduki erakutsi, nola aurkeztu eta nola jokatu behar duen.
Iturburuko kodean **HTML, CSS eta JavaScript** izaten dira:

    - **HTML** edukia eta egitura adierazteko.
    - **CSS** itxura definitzeko (koloreak, letra-tamaina…).
    - **JavaScript** funtzionaltasun interaktiboak gehitzeko.


Orrialdearen iturburua begiratzea oso erabilgarria izan daiteke webgune bati buruzko **informazio gehiago** jasotzeko, batez ere segurtasunari buruzko ikuspuntutik.


### Nola ikusten dut orriaren iturburua?

Page source-a ikusteko modu hauek daude:

    - Webgune baten gainean saguaren eskuin botoiaz klik eginda, "Ikusi orriaren iturburua" (edo antzeko) aukera agertzen da.
    - Nabigatzaileko URLa honela alda daiteke:
 view-source:https://www.adibidea.com
    - Nabigatzailearen menuan, "Developer tools" edo "Tresna gehiago" izeneko ataletan ere aurki daiteke aukera hau.


### Ikus dezagun jatorrizko orrialderen bat?

Page source-a ikusita, hainbat elementu interesgarri aurki ditzakegu:

    - Iruzkinak (<!-- ... -->): programatzaileek kodean uzten dituzten oharrak dira. Ez dira orrian agertzen, baina pistak eman ditzakete (adib. orri bat behin-behinekoa dela esatea).
    - Estekak (<a href="...">): orriko beste atal edo orrialdeetara eramaten gaituzten loturak dira. Batzuetan, ezkutuko estekak ere aurki daitezke.
    - Kanpoko fitxategiak (CSS, JS, irudiak...): HTMLn bidez lotuta egoten dira. Batzuetan, direktorio osoa ikusgai egoten da konfigurazio akatsengatik.
    - Frameworkaren aipamenak: Webgunea framework batekin egina badago, kodean bere izena eta bertsioa ager daitezke. Bertsio zaharra bada, segurtasun arriskuak egon daitezke.

## Garatzaile Tresnak (Developers Tools)

### 1. Inspektorea
- Web-orriaren HTML eta CSS ikusteko eta editatzeko aukera ematen du.
- Elementuak zuzenean alda ditzakezu: testua, koloreak, tamainak...

### 2. Araztailea (Debugger)
- JavaScript kodea pausoz pauso exekutatzeko aukera ematen du.
- "Breakpoint"-ak jarri daitezke kodea gelditzeko.
- Aldagaiak eta funtzio-deien emaitzak ikus daitezke.

### 3. Sarea (Network)
- Orrialde batek egiten dituen eskaera guztiak ikus daitezke.
- Eskaera bakoitzaren xehetasunak ikus daitezke: URL-a, metodoa, erantzun-kodea, tamaina, denbora...

## Edukien Aurkikuntza

### Robots.txt
Webgune baten erroan egoten den fitxategia da, eta bilatzaileei zein orri indexatu daitezkeen edo ez adierazten die.

### Favicon
Nabigatzaileko helbide-barran edo fitxan agertzen den ikono txikia. Batzuetan framework baten favicon lehenetsia ikus daiteke, webguneak erabiltzen duen teknologiari buruzko informazioa emanez.

### Sitemap.xml
Webgunearen orri garrantzitsuenen zerrenda bat duen fitxategia da, bilatzaileek gunea hobeto indexatu dezaten.

## HTTP Goiburuak

Webgune batera konektatzen garenean, HTTP goiburuak bidaltzen dira. Hauek informazio garrantzitsua dute:
- **User-Agent**: Erabiltzen ari zaren nabigatzailea eta sistema eragilea
- **Accept**: Zer motatako edukia onartzen duen nabigatzaileak
- **Cookie**: Saioaren informazioa

## DNS Xehetasunean

### Zer da DNS?
DNS (Domain Name System) domeinu-izenak IP helbide bihurtzen dituen sistema bat da.

### Domeinuaren Zatiak
- **TLD (Top Level Domain)**: .com, .org, .es...
- **Bigarren mailako domeinua**: tryhackme.com-en "tryhackme"
- **Azpidomeinua**: blog.tryhackme.com-en "blog"

### DNS Erregistro Motak
- **A**: IPv4 helbidea
- **AAAA**: IPv6 helbidea
- **CNAME**: Domeinu baten beste baten ordezpena
- **MX**: Posta elektronikorako zerbitzariak
- **TXT**: Testu erregistroak (SPF, DKIM...)

## HTTP Xehetasunez

### HTTP vs HTTPS
- **HTTP**: Datuak testu argian bidaltzen dira
- **HTTPS**: Datuak zifratuta bidaltzen dira (SSL/TLS)

### HTTP Metodoak
- **GET**: Baliabideak eskuratzeko
- **POST**: Datu berriak bidaltzeko
- **PUT**: Baliabideak eguneratzeko
- **DELETE**: Baliabideak ezabatzeko

### HTTP Egoera Kodeak
- **1xx**: Informazioa
- **2xx**: Arrakasta (200 OK)
- **3xx**: Birbideraketa (301 Moved Permanently)
- **4xx**: Bezeroaren errorea (404 Not Found)
- **5xx**: Zerbitzariaren errorea (500 Internal Server Error)

## HTML, CSS eta JavaScript

### HTML
- Web orrien egitura definitzen du
- Etiketak erabiltzen ditu: `<html>`, `<head>`, `<body>`, `<p>`, `<a>`, etc.

### CSS
- Estiloak definitzen ditu: koloreak, letra-tipoak, tamainak...
- Klaseak (`class`) eta ID-ak erabiltzen ditu elementuak identifikatzeko

### JavaScript
- Orriari interaktibitatea ematen dio
- Gertaerak kudeatzen ditu (saguaren klikak, teklatuko sarrerak...)
- AJAX erabiliz zerbitzarirako eskaerak egin ditzake

## Segurtasun Kontzeptuak

### Datu Sentikorraren Azalpena
- Webguneek ez luke inoiz datu sentikorrik erakutsi behar iturburu kodean
- Adibide txarrak: pasahitzak, API gakoak, barne estekak...

### HTML Injekzioa
- Erabiltzailearen sarrera HTML gisa interpretatzen denean gertatzen da
- Erabiltzailearen sarrera beti garbitu behar da errendatu aurretik
- Adibide txarra: `<script>alert('Hacked!')</script>`

## Estekak
- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [Mozilla Developer Network (MDN) Web Docs](https://developer.mozilla.org/)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)

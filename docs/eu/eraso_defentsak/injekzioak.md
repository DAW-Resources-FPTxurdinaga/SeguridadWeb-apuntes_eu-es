# 3.1 Injekzioen aurkako defentsak

## Backend-erako defentsak

### 1. Printzipio Nagusiak
- **Balidazioa**: bezero-aldeko balidazioa ez da segurtasun-muga; zerbitzarian beti egiaztatu behar ditugu gure datuak.
- **Parametrizazioa (prepared statements)**: PDO edo query-builder erabiliz, ez string-konkatenazioa.
- **Gutxieneko pribilegioa**: DB erabiltzaileak soilik behar dituen eskubideak izan ditzala.
- **Output encoding**: erabiltzaile-sarrerak ez innertHTML-raino zuzenean; `htmlspecialchars()` edo egokiagoa den encoding erabili.
- **Iragazketa + allowlists**: onartu nahi dituzun balioak soilik (kolumnak, formatuak, etab.).
- **Konsistentzia**: pasahitzak, tokenak eta sekretuak ez eraman localStorage-n; HttpOnly cookie edo sessionak erabil.
- **Erregistro / errore kudeaketa**: ez itzuli errore-datu sentikorra erabiltzaileari; internoan logatu eta jarraipena egin.

### 2. OWASP Top10 eta PHPn defentsak

#### A01 — Broken Access Control
**Arazoa:** Egiaztatu zerbitzarian beti erabiltzaileak dituen baimenak.  

```php
    session_start();
    $_SESSION['user_id'] = $user['id'];
    $_SESSION['role'] = $user['role']; // 'user' edo 'admin'

    function require_admin() {
        if (empty($_SESSION['role']) || $_SESSION['role'] !== 'admin') {
            http_response_code(403);
            exit('Ez daukazu baimenik.');
        }
    }
```

#### A02 — Cryptographic Failures (Kriptografia hutsegiteak)

**Pasahitzak**: Pasahitzak beti hash-eta beharko dira, `password_hash()` eta `password_verify()` erabiliz.

```php
    // Pasahitz gordetzea
    $hash = password_hash($password, PASSWORD_ARGON2ID);

    // Autentikazioa
    if (password_verify($password, $hash)) {
    // ondo
    }
```

**Datu sentikorrak** (kontu-korrontea): Ez gorde kontu‑zenbakirik edo bestelako datu sentikorrik plain text‑ean. Erabili AEAD algoritmo baten bat, adibidez libsodium — autentikazioa eta zifratzea batera egiten ditu.

```php
    // Libsodium erabilera praktikoaren adibide txiki bat
    define('NONCE_BYTES', SODIUM_CRYPTO_AEAD_XCHACHA20POLY1305_IETF_NPUBBYTES);

    function encrypt_sensitive(string $plaintext, string $key, string $ad = ''): string {
        $nonce = random_bytes(NONCE_BYTES);
        $cipher = sodium_crypto_aead_xchacha20poly1305_ietf_encrypt($plaintext, $ad, $nonce, $key);
        return base64_encode($nonce . $cipher); // gordetzeko errazago
    }

    function decrypt_sensitive(string $stored, string $key, string $ad = ''): ?string {
        $data = base64_decode($stored, true);
        if ($data === false) return null;
        $nonce = substr($data, 0, NONCE_BYTES);
        $cipher = substr($data, NONCE_BYTES);
        return sodium_crypto_aead_xchacha20poly1305_ietf_decrypt($cipher, $ad, $nonce, $key);
    }
```


#### A03 — Injection (SQL, Command, NoSQL, XSS)

**SQL injection** — PHP + PDO:

```php
    $pdo = new PDO($dsn, $user, $pass, [
    PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
    PDO::ATTR_EMULATE_PREPARES => false,
    ]);

    $stmt = $pdo->prepare('SELECT * FROM users WHERE email = :email');
    $stmt->execute([':email' => $email]);
```

**Command injection**: ez erabili erabiltzaile-sarrera zuzenean exec()-ean. Beharrezkoa bada, `escapeshellarg()` erabili eta hobeto ireki liburutegi seguruak.

**XSS** (frontend baina zerbitzarian defensak):

```php
    // Output-encoding:
    htmlspecialchars($value, ENT_QUOTES | ENT_HTML5, 'UTF-8');

    echo htmlspecialchars($user_input, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8');
```


#### A04 — Insecure Design

**Arazoa**: segurtasuna diseinu-fasean falta izatea.

**PHPn nola sahiestu dezakegu:**
- Threat modelling eta erabilera-kasu seguruak idatzi.
- Bizitzako zikloan segurtasun-azterketak txertatu.
- Balidazio zentralak: input-sanitizing helperrak eta egiaztapen liburutegiak (egileak: Zod JS, PHP-rako propioa? erabil iezaiozu filter/ctype/DateTime).

#### A05 — Security Misconfiguration

Segurtasun konfigurazio okerrek aplikazioa ahulezia ugari sortzen dituzte, hala nola errore mezu sentikorrak erabiltzaileari erakustea, CORS-eko arazoak, edo ez diren goiburuak. Beharrezkoa da aplikazioaren ingurunea modu seguruan konfiguratu eta produkzioan ezkutuko informazioa ez erakustea.

**PHP gomendioak**:
- `display_errors = Off` produkzioan (php.ini).
- Erabili segurtasun goiburuak: `X-Content-Type-Options: nosniff`, `Referrer-Policy`, `Content-Security-Policy`.
- CORS kudeatu backend-ean soilik beharrezko dominiuekin.

#### A06 — Vulnerable and Outdated Components
Liburutegi zaharkitu edo ahulak erabiltzeak aplikazioa ahula bihurtzen du, bai kode propioan bai kanpoko dependentzietan. Horregatik, osagai guztiak eguneratuta eta beharrezkoak direnak soilik erabiltzea garrantzitsua da.

**Gomendioak**:
- Composer `composer audit` eta beroan dependentziak eguneratu.
- Ez inportatu liburutegi masiboak behar ez izatekotan.

#### A07 — Identification and Authentication Failures

**Sessions segurtasuna**:
Autentifikazio eta identifikazio hutsegiteak saihestea funtsezkoa da erabiltzaileen kontuak eta datu sentikorrak babesteko. Saioak (sessions) seguru kudeatzea eta cookie flag egokiak ezartzea gomendatzen da, saioak lapurtzea edo manipulazioa ekiditeko.


```php
    session_start();
    session_regenerate_id(true); // login garaian

    // Set cookie flags
    session_set_cookie_params([
        'lifetime' => 0,
        'path' => '/',
        'domain' => 'example.com',
        'secure' => true,
        'httponly' => true,
        'samesite' => 'Lax'
    ]);
```

#### A08 — Software and Data Integrity Failures

Script edo liburutegiak zuzenean kanpoko iturrietatik kargatzean, erasotzaile batek kodea alda dezake eta zure aplikazioan injekzio edo malware arriskua sortu. Horregatik, ez fidatu beti CDN edo kanpoko fitxategietan baizik eta egiaztatu integritatea.

**Composer packages**:
- egiaztatu checksum eta GPG sinadurak dituen proiektuetan.

#### A09 — Security Logging and Monitoring Failures

Log eta monitorizazio egokiak beharrezkoak dira erasoak eta akatsak azkar antzemateko eta konpontzeko. Hala ere, log-etan datu pertsonal sentikorra (PII) agertzea saihestu behar da, ez bakarrik pribatutasun arrazoiengatik, baita araudiak (GDPR) betetzeko ere.

**Gomendio praktikoak:**
- **EZ logatu PII.** Maskatu edo anonimizatu xedapenak (adib., posta elektronikoak, NAN zatiak, txartel zenbakiak).  
- **Erabili monitoring tresna:** Sentry, Datadog edo antzekoak error-trace eta alerta automatikorako.  
- **Definitu log retention politika:** zein datu gordeko diren, zenbat denboraz eta nork ikus dezakeen.  
- **Alerten playbook-a:** nork eta nola erreakzionatu behar duten kasu bakoitzean (incidence response).  

**Adibide txiki (posta maskatzea):**
```php
    function mask_email($email) {
        if (!filter_var($email, FILTER_VALIDATE_EMAIL)) return $email;
        list($local, $domain) = explode('@', $email);
        $start = substr($local, 0, 2);
        $end = substr($local, -1);
        return $start . str_repeat('*', max(0, strlen($local)-3)) . $end . '@' . $domain;
}
```

#### A10 — Server-Side Request Forgery (SSRF)

SSRF arriskuak sortzen dira zerbitzariak erabiltzaileek emandako kanpoko URL edo helbideak bere kabuz exekutatzen dituenean; horrek barne‑baliabideetara edo administrazio‑servicera jotzea eragin dezake.

**Gomendio**:
- Ez onartu erabiltzaileek emandako URLak zuzenean. Beti egiaztatu eta baldintzatu eskaerak zerbitzariak exekutatu aurretik.
- Erabili allowlist-a: onartu soilik aurrez definitutako domeinu/host izenak.
- Egiazta host-IP-ak: DNS bidez stripatu eta ez onartu private / localhost helbideak (127.0.0.1, 10.x, 192.168.x.x, 169.254.x.x, etab.).

Muga gehigarriak: timeout txikiak, outbound firewall arauak eta erantzun edukien tamaina mugatu.



## Frontend-erako defentsak

### 1. A01:2021 - Broken Access Control (Sarbide Kontrol Apurtua)

Baimendutako ez diren erabiltzaileei funtzioak edo datuak atzitzea ahalbidetzen du. Bezeroan, elementuak ezkutatzea ez da nahikoa, kodea manipulagarria baita.

#### Ezegonkorra: 
Botoi bat ezkutatzea nahikoa dela pentsatzea.

```javascript
if (!user.isAdmin) {
  document.getElementById("deleteBtn").style.display = "none";
}
```

#### Segurua: 
Backend-ek beti balidatu behar du baimena.

```javascript
if (!user.isAdmin) {
  document.getElementById("deleteBtn").remove();
}
```

#### Aholkuak:
- Ez fidatu bezero aldeko balioztapenetan; egiaztatu backend-ean tokenak erabiliz (adib., JWT).
- Erabili gutxieneko pribilegioaren printzipioa.
- Inplementatu RBAC (Roleen Bidezko Sarbide Kontrola) zerbitzarian.

---

### 2. A02:2021 - Cryptographic Failures (Kriptografia Hutsegiteak)

Datu sentikorren esposizioa kriptografia ahularen bidez. JS-n, saihestu secretak bezeroan gordetzea edo HTTPS gabe transmititzea.

#### Ezegonkorra: 
Pasahitzak edo tokenak `localStorage`-n gordetzea.

```javascript
localStorage.setItem("password", "123456");
```

#### Segurua: 
Inoiz ez gorde pasahitzak. Token aldi baterako hobeak `sessionStorage`-n.

```javascript
sessionStorage.setItem("password", 123456);
```

Beste modu bat...

```javascript
async function hashPassword(password) {
  const encoder = new TextEncoder();
  const data = encoder.encode(password);
  const hash = await crypto.subtle.digest('SHA-256', data);
  return Array.from(new Uint8Array(hash)).map(b =>
    b.toString(16).padStart(2, '0')).join('');
}
```

#### Aholkuak:
- Erabili HTTPS beti.
- Gorde tokenak HttpOnly cookie-etan, ez localStorage-n.
- Erabili Web Crypto API behar kriptografikoentzat.

---

### 3. A03:2021 - Injection (XSS, HTML Injection) (Injekzioa)

Kode gaiztoko injekzioa, hala nola XSS, sanitizatu gabeko input-en bidez.

#### Ezegonkorra: 
Input-ak zuzenean innerHTML-n sartzea.

```javascript
const userInput = document.getElementById('input').value;
document.getElementById('output').innerHTML = userInput;
```

#### Segurua: 
Sanitizatu edukia DOMPurify erabiliz.

```javascript
import DOMPurify from 'dompurify';
const userInput = document.getElementById('input').value;
const sanitized = DOMPurify.sanitize(userInput);
document.getElementById('output').textContent = sanitized;
```

#### Aholkuak:
- Erabili textContent edo createTextNode innerHTML-ren ordez.
- Sanitizatu DOMPurify erabiliz.
- Inplementatu Content Security Policy (CSP).

---

### 4. A04:2021 - Insecure Design (Diseinu Ezegonkorra)

Hasieratik mehatxuak kontuan hartzen ez dituzten diseinuak.

#### Ezegonkorra: 
Logika kritikoa frontend-ean.

```javascript
let precio = 100;
if (userInput === "BLACKFRIDAY") {
  precio = 1;
}
```

#### Segurua: 
Kalkulua beti backend-ean.

```javascript
fetch("/api/check-discount", {
  method: "POST",
  body: JSON.stringify({ coupon: userInput })
})
```

#### Aholkuak:
- Erabili threat modeling (adib., STRIDE).
- Balidatu input-ak Zod bezalako liburutegiekin.
- Diseinatu "secure by default" printzipioarekin.

---

### 5. A05:2021 - Security Misconfiguration (Segurtasun Konfigurazio Okerra)

Konfigurazio ezegunekorrak.

#### Ezegonkorra: 
Liburutegi zaharrak erabiltzea edo barne erroreak erakustea.

```javascript
console.error("Error: conexión a base de datos fallida en localhost:3306");
```

#### Segurua: 
Liburutegi eguneratuak eta mezu generikoak erabiltzea.

```javascript
console.error("Ha ocurrido un error, inténtelo más tarde.");
```

Beste adibide bat:

#### Ezegonkorra:

```javascript
fetch('https://api.example.com/data');
```

#### Segurua:

```javascript
fetch('https://api.example.com/data', {
  mode: 'cors',
  credentials: 'same-origin',
  headers: {
    'X-Content-Type-Options': 'nosniff',
    'X-Requested-With': 'XMLHttpRequest'
  }
});
```

#### Aholkuak:
- Konfiguratu CORS zorrotz backend-ean.
- Erabili segurtasun goiburuak.
- Eskaneatu konfigurazioak OWASP ZAP erabiliz.

---

### 6. A06:2021 - Vulnerable and Outdated Components (Osagai Zaurgarri eta Zaharkituak)

Ahultasunak dituzten liburutegi zaharkituen erabilera.

#### Ezegonkorra:

```javascript
import _ from 'lodash'; // Versión <4.17.13 vulnerable
_.merge({}, JSON.parse(userInput));
```

#### Segurua:

```javascript
import _ from 'lodash'; // "^4.17.21"
_.merge({}, JSON.parse(userInput));
```

#### Aholkuak:
- Erabili npm audit edo Snyk.
- Eguneratu dependentziak erregulartasunez.
- Minimizatu dependentziak.

---

### 7. A07:2021 - Identification and Authentication Failures (Identifikazio eta Autentifikazio Hutsegiteak)

Autentifikazioaren kudeaketa txarra, tokenak esposatzen dituen modukoa.

#### Ezegonkorra: 
Saio mugagabea gordetzea.

```javascript
localStorage.setItem("jwt", token);
```

#### Segurua: 
Iraungipen kontrolatua erabiltzea.

```javascript
setTimeout(() => {
  sessionStorage.removeItem("accessToken");
  alert("Tu sesión ha expirado");
  location.href = "/login";
}, 15 * 60 * 1000); // 15 minutos
```

#### Aholkuak:
- Erabili OAuth/JWT iraungitzerekin.
- Inplementatu MFA backend-arekin.
- Ezabatu tokenak logout egitean.

---

### 8. A08:2021 - Software and Data Integrity Failures (Software eta Datu Integritate Hutsegiteak)

Script-en karga egiaztapen gabe (adib., SRI gabe).

#### Ezegonkorra: 
Script-ak balidazio gabe kargatzea.

```html
<script src="https://cdn.example.com/lib.js"></script>
```

#### Segurua: 
Subresource Integrity (SRI) erabiltzea.

```html
<script src="https://cdn.example.com/lib.js"
        integrity="sha384-xxxxx"
        crossorigin="anonymous"></script>
```

#### Aholkuak:
- Erabili Subresource Integrity (SRI).
- Egiaztatu checksumak.
- Saihestu CDN ez-fidagarriak.

---

### 9. A09:2021 - Security Logging and Monitoring Failures (Segurtasun Log eta Monitorizazio Hutsegiteak)

Datuak esposatzen dituzten logak edo erasoak detektatu ezin dituztenak.

#### Ezegonkorra:

```javascript
console.log('Error with user:', userData);
```

#### Segurua:

```javascript
console.error('Error occurred:', { code: error.code });
```

#### Aholkuak:
- Ez egin PII (Personally Identifiable Information) loggingik.
- Erabili Sentry bezalako tresnak.
- Mugatu logak produkzioan.

---

### 10. A10:2021 - Server-Side Request Forgery (SSRF) (Zerbitzari Aldeko Eskaera Faltsutzea)

Barne baliabideetara request-ak egitera behartzen dituzten bezero eskaerak.

#### Ezegonkorra:

```javascript
const url = document.getElementById('urlInput').value;
fetch(url);
```

#### Segurua:

```javascript
const allowedUrls = ['https://api.example.com'];
const url = document.getElementById('urlInput').value;
if (allowedUrls.some(allowed => url.startsWith(allowed))) {
  fetch(url);
}
```

#### Aholkuak:
- Erabili allowlist-ak URLentzat.
- Balidatu input-ak eskareentzat.
- Mugatu GET metodoetara ahal bada.




# Hurrengo urratsak

- [SQL injekzioak prebenitzeko trikimailu-orria](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

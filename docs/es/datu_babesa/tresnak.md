# 3.3 Herramientas prácticas de seguridad

## Desarrollo Seguro (DevSecOps)

### 1. SAST (Static Application Security Testing)

ESLint con plugin de seguridad (JavaScript/TypeScript):
```bash
npm install --save-dev eslint eslint-plugin-security
```

.eslintrc.js:
```javascript
module.exports = {
  extends: [
    'eslint:recommended',
    'plugin:security/recommended'
  ],
  plugins: ['security']
};
```

### 2. DAST (Dynamic Application Security Testing)

OWASP ZAP (Docker):
```bash
docker run -u zap -p 8080:8080 -p 8081:8081 -i owasp/zap2docker-stable zap-webswing.sh
```

### 3. SCA (Software Composition Analysis)

Dependabot (.github/dependabot.yml):
```yaml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
```

## Automatización de pruebas de seguridad

### 1. Nikto
```bash
nikto -h https://tu-aplicacion.es
```

### 2. Nmap
```bash
nmap -sV -sC -p 80,443 tu-servidor.es
```

## Análisis estático del código

### 1. SonarQube + SonarLint

Docker Compose:
```yaml
version: '3'
services:
  sonarqube:
    image: sonarqube:community
    ports:
      - "9000:9000"
```

### 2. Snyk
```bash
snyk auth
snyk test
```

## CI/CD: escaneos de seguridad

GitHub Actions (ejemplo):
```yaml
name: Security Scan
on:
  push:
    branches: [ main, develop ]
jobs:
  security-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18.x'
      - name: Run Snyk
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
        with:
          args: --severity-threshold=high
```

## Laboratorios de práctica

- OWASP Juice Shop (Docker): `docker run --rm -p 3000:3000 bkimminich/juice-shop`
- DVWA: `docker run --rm -it -p 80:80 vulnerables/web-dvwa`
- WebGoat: ver documentación oficial

## Consejos finales

- La seguridad es un proceso continuo
- Secure by Default
- Principio de mínimo privilegio
- Varias capas defensivas
- Política de seguridad clara

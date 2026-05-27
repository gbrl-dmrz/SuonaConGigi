# Suona con Gigi

Piattaforma full stack dedicata alla community musicale: musicisti, eventi live, forum, profili musicali, artisti e moderazione contenuti.

Il progetto è composto da:

* un backend REST sviluppato con **Spring Boot**
* un frontend SPA sviluppato con **Angular**

L'obiettivo della piattaforma è permettere agli utenti di connettersi tramite la musica, creare eventi, partecipare alle discussioni del forum e costruire il proprio profilo musicale personale.

---

# Stack Tecnologico

## Backend

* Java 21
* Spring Boot 3.5.14
* Spring Web
* Spring Security
* Spring Data JPA / Hibernate
* MySQL
* JWT con JJWT
* Bean Validation
* Lombok
* Thymeleaf
* Spring Mail
* Springdoc OpenAPI / Swagger UI
* Maven

## Frontend

* Angular 21
* TypeScript 5.9
* RxJS
* Angular Router
* Angular HttpClient
* Angular Signals
* Angular Animations
* CSS
* HugeRTE Angular
* Node.js 24+
* npm

---

# Funzionalità Principali

## Autenticazione e Sicurezza

* Registrazione utente
* Login con JWT
* Verifica email tramite token
* Sessione persistente con `localStorage`
* Rotte protette tramite guard Angular
* Ruoli applicativi `USER` e `ADMIN`
* Password criptate con BCrypt
* Endpoint protetti con Spring Security
* Interceptor HTTP per aggiungere automaticamente il token JWT

## Community Musicale

* Profilo utente personalizzato
* Profilo musicale
* Catalogo artisti, generi e strumenti
* Forum con:

  * categorie
  * thread
  * post
  * ricerca discussioni

## Sistema Eventi

* Creazione eventi
* Lista eventi con ricerca
* Iscrizione e cancellazione eventi
* Like agli eventi
* Moderazione eventi per amministratori

## Area Admin e Moderazione

* Dashboard amministrativa
* Gestione parole censurate
* Gestione template email
* Gestione errori centralizzata
* Risposte API standardizzate tramite DTO

## Documentazione e Architettura

* Swagger/OpenAPI integrato
* API REST JSON
* Configurazione tramite environment variables
* Seed automatico database per sviluppo
* Architettura modulare frontend/backend

---

# Architettura del Progetto

```text
Angular Frontend
        |
        | HTTP + JWT
        v
Spring Boot REST API
        |
        v
MySQL Database
```

Frontend locale:

```text
http://localhost:4200
```

Backend locale:

```text
http://localhost:8080/suonacongigi
```

Base URL API:

```text
http://localhost:8080/suonacongigi/api
```

---

# Requisiti

## Backend

* JDK 21
* Maven 3.9+
* MySQL 8+

## Frontend

* Node.js >=24
* npm

Database richiesto:

```text
suonacongigi
```

---

# Configurazione Backend

Prima dell'avvio creare queste variabili ambiente:

```env
DB_USERNAME=your_mysql_username
DB_PASSWORD=your_mysql_password
JWT_SECRET=a_long_and_secure_secret_key
```

URL database configurato:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/suonacongigi?useSSL=false&serverTimezone=Europe/Rome&allowPublicKeyRetrieval=true
```

Context path backend:

```text
/suonacongigi
```

---

# Configurazione Frontend

Configurazione sviluppo:

```ts
// src/environments/environment.development.ts
export const environment = {
  production: false,
  apiUrl: 'http://localhost:8080/suonacongigi/api'
};
```

Configurazione produzione:

```ts
// src/environments/environment.ts
export const environment = {
  production: true,
  apiUrl: 'https://api.suonacongigi.com/api'
};
```

Prima del deploy aggiornare `apiUrl` con l'URL reale del backend.

---

# Avvio Locale

## 1. Clonare i repository

```bash
git clone <backend-repository>
git clone <frontend-repository>
```

---

## 2. Creare il database MySQL

Database richiesto:

```text
suonacongigi
```

---

## 3. Avviare il Backend

Entrare nella cartella backend:

```bash
cd suona-con-gigi-backend
```

Installare le dipendenze:

```bash
mvn clean install
```

Avviare l'applicazione:

```bash
mvn spring-boot:run
```

Esempio PowerShell:

```powershell
$env:DB_USERNAME="root"
$env:DB_PASSWORD="password"
$env:JWT_SECRET="replace-this-string-with-a-long-secret"

mvn spring-boot:run
```

Backend disponibile su:

```text
http://localhost:8080/suonacongigi
```

---

## 4. Avviare il Frontend

Entrare nella cartella frontend:

```bash
cd suona-con-gigi-frontend
```

Installare le dipendenze:

```bash
npm install
```

Avviare Angular:

```bash
npm start
```

Frontend disponibile su:

```text
http://localhost:4200
```

---

# Build

## Backend

Generare il file JAR:

```bash
mvn clean package
```

Eseguire il JAR:

```bash
java -jar target/suona-gigi-0.0.1-SNAPSHOT.jar
```

---

## Frontend

Build produzione:

```bash
npm run build
```

Output generato in:

```text
dist/suonacongigi
```

Watch mode sviluppo:

```bash
npm run watch
```

---

# Documentazione API

Swagger UI disponibile su:

```text
http://localhost:8080/suonacongigi/swagger-ui.html
```

OpenAPI JSON:

```text
http://localhost:8080/suonacongigi/v3/api-docs
```

Per usare gli endpoint protetti da Swagger:

1. effettuare login
2. copiare il token JWT
3. cliccare su `Authorize`
4. inserire:

```text
Bearer <token>
```

---

# Endpoint Principali

Tutti gli endpoint sono prefissati da:

```text
http://localhost:8080/suonacongigi
```

| Area        | Endpoint                        | Descrizione              |
| ----------- | ------------------------------- | ------------------------ |
| Auth        | `/api/auth/register`            | Registrazione utente     |
| Auth        | `/api/auth/login`               | Login e generazione JWT  |
| Auth        | `/api/auth/verify`              | Verifica token           |
| Users       | `/api/users/me`                 | Profilo personale        |
| Users       | `/api/users`                    | Lista utenti (admin)     |
| Events      | `/api/events`                   | Lista e creazione eventi |
| Events      | `/api/events/{id}`              | Dettaglio evento         |
| Events      | `/api/events/{id}/register`     | Registrazione evento     |
| Events      | `/api/events/{id}/likes`        | Like evento              |
| Forum       | `/api/forum/categories`         | Categorie forum          |
| Forum       | `/api/forum/threads`            | Creazione thread         |
| Forum       | `/api/forum/threads/{id}`       | Dettaglio thread         |
| Forum       | `/api/forum/threads/{id}/posts` | Creazione post           |
| Forum       | `/api/forum/search`             | Ricerca forum            |
| Cataloghi   | `/api/artists`                  | Artisti                  |
| Cataloghi   | `/api/genres`                   | Generi musicali          |
| Cataloghi   | `/api/instruments`              | Strumenti musicali       |
| Moderazione | `/api/users/me/censura`         | Preferenze censura       |
| Email       | `/api/email/verification`       | Template email           |

---

# Rotte Frontend

| Rotta                    | Accesso            | Descrizione             |
| ------------------------ | ------------------ | ----------------------- |
| `/`                      | Pubblico           | Home page               |
| `/login`                 | Pubblico           | Login                   |
| `/register`              | Pubblico           | Registrazione           |
| `/verify`                | Pubblico           | Verifica email          |
| `/events`                | Utente autenticato | Lista eventi            |
| `/events/new`            | Utente autenticato | Creazione evento        |
| `/events/:id`            | Utente autenticato | Dettaglio evento        |
| `/profile`               | Utente autenticato | Profilo personale       |
| `/forum`                 | Utente autenticato | Forum                   |
| `/forum/threads/:id`     | Utente autenticato | Dettaglio thread        |
| `/admin`                 | Admin              | Dashboard admin         |
| `/admin/events/:id/edit` | Admin              | Modifica evento         |
| `/admin/email`           | Admin              | Gestione template email |

Le rotte non riconosciute vengono reindirizzate alla home.

---

# Autenticazione

La sessione viene salvata nel frontend nella chiave:

```text
sg_session
```

La sessione contiene:

* token JWT
* dati utente
* ruolo utente

L'`authInterceptor` aggiunge automaticamente:

```http
Authorization: Bearer <token>
```

alle richieste protette.

---

# Seed Database

La proprietà:

```properties
app.db.seed-on-start=false
```

controlla il seed automatico del database.

In sviluppo:

```properties
true
```

In produzione:

```properties
false
```

---

# Sicurezza

* Autenticazione JWT stateless
* Password criptate con BCrypt
* Gestione ruoli con Spring Security
* Protezione endpoint tramite `@PreAuthorize`
* CORS configurato per Angular
* Stack trace nascosti nelle risposte API

---

# Struttura Backend

```text
src/main/java/it/generation/suonacongigi
|-- bootstrap
|-- config
|-- controller
|-- dto
|-- model
|-- repository
|-- security
|-- service
`-- util
```

---

# Struttura Frontend

```text
src/app
|-- core
|   |-- directives
|   |-- guards
|   |-- interceptors
|   |-- models
|   `-- services
|-- features
|   |-- admin
|   |-- auth
|   |-- events
|   |-- forum
|   |-- home
|   `-- profile
`-- shared
```

Asset pubblici:

```text
public/
|-- favicon.ico
`-- images/hero_suonacongigi.jpg
```

---

# Test

Eseguire i test backend:

```bash
mvn test
```

---

# Note Deploy

Prima della pubblicazione:

* configurare correttamente le environment variables
* usare una `JWT_SECRET` lunga e privata
* disattivare il seed automatico
* configurare credenziali SMTP reali
* aggiornare i CORS
* aggiornare `apiUrl`
* configurare fallback SPA verso `index.html`
* pubblicare la build Angular presente in:

```text
dist/suonacongigi/browser
```

---

# Visione del Progetto

Una piattaforma full stack pensata per simulare un'applicazione reale moderna:

* autenticazione sicura
* backend modulare
* frontend strutturato
* gestione admin
* sistema forum
* gestione eventi live
* comunicazione API-first

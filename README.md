# SchoolManagement Frontend

Frontend Angular de l'application Jungle In English. Cette application regroupe les parcours front-office, back-office et shop dans une seule SPA, avec authentification Keycloak, appels API via la Gateway et plusieurs modules metiers charges a la demande.

## Vue d'ensemble

Le frontend repose sur:

- Angular 18
- Keycloak JS pour l'authentification
- Angular Router avec lazy loading
- Intercepteurs HTTP pour les jetons et la gestion des erreurs
- Service Worker pour le mode PWA
- Integrations UI pour chartes, QR code, chat et e-commerce

## Structure du projet

- `src/main.ts`: point d'entree de l'application et initialisation Keycloak.
- `src/app/app.module.ts`: module racine Angular.
- `src/app/app-routing.module.ts`: routage principal.
- `src/app/auth/`: configuration et guards Keycloak.
- `src/app/front-office/`: parcours utilisateur principal.
- `src/app/back-office/`: espace d'administration.
- `src/app/shop/`: module e-commerce.
- `src/app/features/lessons/`: fonctionnalites liees aux lecons et aux retours.
- `src/app/services/`: services transverses.
- `src/environments/`: configurations d'environnement.

## Parcours fonctionnels

### Front-office

Le front-office contient les pages visibles par les utilisateurs finaux:

- cours
- lecons
- quiz
- to-do
- historique et details d'essais
- pages marketing et presentation

### Back-office

L'espace admin est protege par `AuthGuard` et regroupe les fonctions de gestion interne.

### Shop

Le module shop est charge via `shop.routes.ts` et couvre:

- produits
- categories
- commandes
- panier
- paiement
- administration e-commerce

## Authentification

L'application utilise Keycloak des le demarrage:

1. `initKeycloak()` est appele avant le bootstrap Angular.
2. L'option `login-required` force la connexion avant l'acces a l'app.
3. Les intercepteurs ajoutent les jetons aux requetes HTTP.
4. En cas d'echec Keycloak, l'application affiche un message d'erreur au lieu de rester blanche.

Les parametres locaux sont definis dans [environment.ts](src/environments/environment.ts) et pointent vers:

- API Gateway: `http://localhost:8222`
- Keycloak: `http://localhost:8085`

## Integration technique

### Routing

Le routage principal charge differents modules a la demande:

- racine / front-office
- `admin` et `back` pour l'espace protege
- `shop` pour le sous-systeme e-commerce

### Intercepteurs

L'application declare notamment:

- `AuthInterceptor` pour les appels securises
- `JwtInterceptor` pour les flux qui requierent un jeton
- `ErrorInterceptor` pour la gestion centralisee des erreurs

### Composants et librairies

Le `package.json` contient entre autres:

- `@stomp/stompjs` et `sockjs-client` pour le temps reel
- `@zxing/browser` et `@zxing/ngx-scanner` pour le scan QR
- `chart.js`, `ng2-charts`, `apexcharts` pour les graphiques
- `sweetalert2` pour les alertes UI
- `swiper` pour les carrousels
- `tailwindcss` et SCSS pour le style

## Build et execution

### Pre-requis

- Node.js 18+
- npm
- Backend disponible via la Gateway et Keycloak

### Installation

```bash
npm install
```

### Lancer en local

```bash
npm start
```

L'application est alors disponible sur `http://localhost:4200/`.

### Build production

```bash
npm run build
```

Les artefacts sont generes dans `dist/school-management`.

### Tests

```bash
npm test
```

## Docker

Le `Dockerfile` construit d'abord l'application avec Node puis sert le build via Nginx.

Etapes principales:

1. `npm install --legacy-peer-deps`
2. `npm run build -- --configuration production`
3. Copie du build dans `/usr/share/nginx/html`

## Configuration Angular

Le projet utilise `angular.json` avec:

- sortie de build dans `dist/school-management`
- SCSS comme langage de style
- Font Awesome charge globalement
- Service worker actif en production

## Points de vigilance

- Le bootstrap depend fortement de Keycloak.
- Le frontend pointe vers des services locaux en dur via `environment.ts`.
- La compilation utilise `--legacy-peer-deps`, ce qui indique des conflits potentiels de versions.
- Le `enableTracing` du router est active pour le debug et peut etre retire en production.

## Ressources utiles

- [Application root module](src/app/app.module.ts)
- [Application routing](src/app/app-routing.module.ts)
- [Keycloak config](src/app/auth/keycloak.config.ts)
- [Environment local](src/environments/environment.ts)
- [Angular build config](angular.json)
- [Dockerfile frontend](Dockerfile)

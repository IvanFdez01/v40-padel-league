# V40 Padel League - Full-Stack League Management Platform (PWA)

Production web platform for the **V40 Valladolid Women's Padel League**, serving **100+ active players**. 


> ### ℹ️ Why this repo only contains a README
> This is client software currently running in production for a real league with active users. To protect the client and its users, the source is kept private and this repository stands as a portfolio showcase of the work rather than a public codebase.
>
> The full source code is available on request, reach out (see profile) and I'm happy to walk through it or grant access. All infrastructure secrets (DB credentials, JWT and VAPID keys) are injected via environment variables and never live in the code.

---

## What it does

A mobile-first Progressive Web App where players and admins manage a full padel league season:

- **Standings** - live team classification per division and a global player ranking.
- **Calendar & fixtures** - matchdays (*jornadas*) and head-to-head matchups (*enfrentamientos*) with built-in skill base matchmaking.
- **My team** - squad view, pairings, and result submission.
- **Results** - register and update per-match points (admin).
- **Accounts** - registration, JWT login, player profiles, admin console.
- **Push notifications** - upcoming-match reminders via Web Push (VAPID), delivered even with the app closed.

## Architecture

Two-tier application, fully decoupled, deployed on Railway.

### Backend -- Java 17 · Spring Boot 3.4

Clean layered architecture with strict separation of concerns:

```
Controller  →  SA (application/service layer)  →  Repository (Spring Data JPA)  →  Entities
                        ↑ DTOs decouple the internal model from the exposed API
```

- **REST API** (`/api`) with ~18 endpoints across users, teams, divisions, matchdays, pairings, matches and push.
- **Spring Data JPA / Hibernate** over **MySQL**, with a domain model of Club, Division, Team, Matchday, Matchup, Pairing, Match, Points, User and Subscription.
- **Stateless JWT auth** built from scratch (`jjwt`): custom `JwtService`, `JwtFilter` and `SecurityConfig`.
- **Web Push** (`nl.martijndwars:web-push` + BouncyCastle) with VAPID keys for browser notifications.
- Centralised exception handling (`ExceptionsHandler`, `MyException`).

### Frontend -- Angular 21 · TypeScript · Tailwind CSS

- **Standalone components** organised by feature (home, login, register, calendar, clasification, equipos, mi-equipo, profile, layout), each with its own service.
- **PWA**: Angular Service Worker with prefetch/lazy asset strategies (`ngsw-config.json`) and `SwPush` subscription flow wired to the backend VAPID key.
- **Auth**: route `authGuard` + HTTP `authInterceptor` attaching the JWT; session state managed with Angular **signals** (`SessionService`), persisted client-side.
- **SSR** via `@angular/ssr` + Express, with prerendering.

## Tech stack

| Layer | Technologies |
|---|---|
| Backend | Java 17, Spring Boot 3.4, Spring Security, Spring Data JPA / Hibernate, MySQL, JJWT, Web Push (VAPID), Lombok, Maven |
| Frontend | Angular 21, TypeScript, Tailwind CSS 4, RxJS, Angular Service Worker (PWA), Angular SSR (Express) |
| Infra | Railway (API + MySQL), Netlify runtime adapter |

## Highlights

- **Full lifecycle** - from DB modelling to production deployment and maintenance.
- **Real users** - 100+ active players using it live during the season.
- **Security done properly** - hand-rolled JWT pipeline, guarded routes, all secrets externalised to env vars (nothing hardcoded).
- **True PWA** - installable, offline-capable assets, and server-sent push reminders.

---


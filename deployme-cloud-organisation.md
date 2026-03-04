# Deployme.cloud — Organisation & Architecture

> **Domaine** : `deployme.cloud` — Kubernetes as a Service, open-core.
> **Philosophie** : GitHub + Netlify + VPS PulseHeberg (FR) + outils gratuits, alternatives europeennes privilegiees.

---

## 1. Architecture generale

```
deployme.cloud                            -> Cloudflare (DNS + CDN)
|
|-- FRONTENDS (Netlify, gratuit) -----------------------------------------
|
|-- deployme.cloud                        -> Vitrine publique (Astro)
|-- docs.deployme.cloud                   -> Documentation (Starlight)
|-- app.deployme.cloud                    -> Manager client (SvelteKit)
|-- admin.deployme.cloud                  -> Panel admin, VPN only (SvelteKit)
|-- blog.deployme.cloud                   -> Blog technique (Astro)
|
|-- BACKEND (VPS PulseHeberg FR, Docker Compose) -------------------------
|
|-- api.deployme.cloud                    -> API Core (Go + Caddy)
|-- auth.deployme.cloud                   -> Zitadel OIDC (Caddy)
|   +-- PostgreSQL 17 + Valkey 8 (internes, non exposes)
|
|-- OPTIONNEL ------------------------------------------------------------
|
+-- status.deployme.cloud                 -> Page de statut
```

---

## 2. VPS PulseHeberg — Serveur de dev

| | |
|---|---|
| **Hebergeur** | PulseHeberg (France) |
| **Specs** | 2 vCPU / 2 GB RAM / 30 GB SSD |
| **OS** | Debian 12 Bookworm |
| **Statut** | Temporaire — prototypage et tests |

Tout le backend tourne dans un seul **Docker Compose** avec **Caddy** en reverse proxy (HTTPS auto via Let's Encrypt) :

```
VPS (2 vCPU / 2 GB RAM)
|-- Caddy v2           -> reverse proxy TLS     (~15 MB)
|-- deployme-api       -> binaire Go            (~30 MB)
|-- PostgreSQL 17      -> base "deployme" + "zitadel"  (~120 MB)
|-- Valkey 8           -> cache (fork Redis BSD) (~20 MB)
+-- Zitadel            -> auth OIDC sur PG      (~250 MB)
                                         Total : ~450 MB
```

**Choix cles** : PostgreSQL mutualise (API + Zitadel) pour eviter CockroachDB (~300 MB economises). Valkey au lieu de Redis (licence BSD). Images Alpine partout. 1 GB de swap en filet de securite.

---

## 3. Organisation GitHub

```
github.com/deployme-cloud               <- Organisation (repos prives)
```

### 3.1 Arborescence des repos

```
deployme-cloud/
|
|-- FRONTENDS ---------------------------------------------------------
|-- deployme-www                         Vitrine publique
|-- deployme-docs                        Documentation technique
|-- deployme-manager                     Dashboard utilisateur (app)
|-- deployme-admin                       Dashboard interne (admin)
|
|-- BACKEND & API -----------------------------------------------------
|-- deployme-api                         API Core (control plane)
|-- deployme-agent                       Agent on-premise (Phase 1+)
|
|-- SDK & INTEGRATIONS ------------------------------------------------
|-- deployme-sdk-go                      SDK Go
|-- deployme-sdk-python                  SDK Python
|-- deployme-sdk-typescript              SDK TypeScript
|-- deployme-terraform-provider          Provider Terraform
|-- deployme-ansible-collection          Collection Ansible
|-- deployme-capi-provider               Provider Cluster API (CAPI)
|-- deployme-cli                         CLI utilisateur
|
|-- INFRA & OPS -------------------------------------------------------
|-- deployme-infra                       IaC, Docker Compose VPS, DNS, configs
|-- deployme-ci                          Workflows CI/CD reutilisables
|
|-- COMMUNAUTE & CONTENU ----------------------------------------------
|-- deployme-brand                       Logo, charte graphique, assets
+-- deployme-training                    Formation, labs, parcours
```

---

## 4. Detail des repos — Stack technique

### Frontends

| Repo | Description GitHub | Stack | Deploy |
|------|--------------------|-------|--------|
| `deployme-www` | Site vitrine : landing, produit, pricing, contact | **Astro 5** + **TailwindCSS 4** + **MDX** | Netlify -> `deployme.cloud` |
| `deployme-docs` | Documentation : guides, API ref, tutoriels, FAQ. Recherche integree, versioning | **Starlight** (Astro) + **MDX** | Netlify -> `docs.deployme.cloud` |
| `deployme-manager` | Espace client : dashboard clusters, monitoring, billing. Auth via Zitadel OIDC | **SvelteKit** + **TailwindCSS 4** + **Zitadel** (OIDC) | Netlify -> `app.deployme.cloud` |
| `deployme-admin` | Panel admin (VPN) : gestion infra, users, metriques, support. Meme stack que le Manager | **SvelteKit** + **TailwindCSS 4** + **Zitadel** (OIDC) | Netlify -> `admin.deployme.cloud` |

### Backend & API

| Repo | Description GitHub | Stack | Deploy |
|------|--------------------|-------|--------|
| `deployme-api` | API Core / control plane : auth, CRUD clusters, provisioning, billing, webhooks | **Go** + **PostgreSQL** + **Valkey** + **gRPC/REST** | VPS PulseHeberg -> `api.deployme.cloud` |
| `deployme-agent` | Agent on-premise (Phase 1+) : communique via mTLS, pilote Proxmox/VMware/libvirt | **Go** — binaire unique, sans dependances | Installe chez le client |

### SDK & Integrations

| Repo | Description GitHub | Stack |
|------|--------------------|-------|
| `deployme-sdk-go` | SDK client Go | **Go** |
| `deployme-sdk-python` | SDK client Python | **Python** + **httpx** |
| `deployme-sdk-typescript` | SDK client TypeScript/JavaScript | **TypeScript** + **fetch** |
| `deployme-terraform-provider` | Provider Terraform pour provisionner des clusters | **Go** + **Terraform Plugin SDK** |
| `deployme-ansible-collection` | Collection Ansible pour automatiser les deploiements | **Ansible** (YAML + Python) |
| `deployme-capi-provider` | Provider Cluster API (workflow CAPI standard K8s) | **Go** + **controller-runtime** + **kubebuilder** |
| `deployme-cli` | CLI interactive : creer, lister, gerer des clusters | **Go** + **cobra** |

### Infra & Ops

| Repo | Description GitHub | Stack |
|------|--------------------|-------|
| `deployme-infra` | IaC : DNS Cloudflare, sites Netlify, Docker Compose du VPS (Caddy + PG + Valkey + Zitadel + API) | **Terraform** + **Docker Compose** |
| `deployme-ci` | Workflows GitHub Actions reutilisables : lint, test, build, deploy, release | **GitHub Actions** (YAML) |

### Communaute & Contenu

| Repo | Description GitHub | Stack |
|------|--------------------|-------|
| `deployme-brand` | Charte graphique, logo SVG, palette, typographies | **SVG** + **PNG** + **Figma exports** |
| `deployme-training` | Contenus de formation : labs, parcours, scripts videos | **Markdown** + **MDX** |

---

## 5. Outils & services

| Besoin | Outil | Gratuit | EU |
|--------|-------|---------|-----|
| **Code** | GitHub (org `deployme-cloud`) | Oui | Alt : **Codeberg** (DE), **GitLab** |
| **Frontend hosting** | Netlify | Oui | Alt : **Cloudflare Pages** |
| **DNS + CDN + SSL** | Cloudflare | Oui | Datacenters EU |
| **Backend hosting** | VPS PulseHeberg | Payant (VPS) | **France** |
| **Auth OIDC** | Zitadel (self-hosted sur VPS) | Oui | **Suisse** |
| **BDD** | PostgreSQL 17 (sur VPS) | Oui | France (VPS) |
| **Cache** | Valkey 8 (sur VPS) | Oui, licence BSD | France (VPS) |
| **Reverse proxy** | Caddy v2 (sur VPS) | Oui | France (VPS) |
| **Registry Docker** | GitHub Container Registry | Oui | — |
| **Analytics** | Plausible | Oui | **Estonie** |
| **Emails** | Resend (3k/mois) | Oui | Alt : **Brevo** (FR) |
| **Maquettes** | Penpot | Oui, open-source | **Espagne** |
| **Communaute** | Discord | Oui | Alt : **Matrix/Element** |

> **Fallback cloud** (si VPS indisponible) : **Neon** (PG serverless, EU Frankfurt), **Upstash** (Redis, EU Frankfurt), **Zitadel Cloud** (25k MAU gratuit).

---

## 6. Priorite de creation (Sprint 0)

```
Semaine 1-2 (Sprint 0a) — Quick Win : site en ligne
  [x] deployme-infra    -> Docker Compose VPS + DNS Cloudflare
  [x] deployme-www      -> Vitrine live rapidement
  [x] deployme-brand    -> Logo et assets
  [x] deployme-ci       -> CI/CD automatique

Semaine 3-4 (Sprint 0b) — Quick Win : docs + maquette manager
  [x] deployme-docs     -> Documentation en ligne
  [x] deployme-manager  -> Maquette UI dashboard
  [x] deployme-api      -> Premiers endpoints sur le VPS

Sprint 1+ — Selon roadmap
  [ ] deployme-admin
  [ ] deployme-sdk-go
  [ ] deployme-terraform-provider
  [ ] deployme-agent
  [ ] ...
```

---

## 7. Flux CI/CD

```
Push sur main
    |
    |-- GitHub Actions -- Lint + Tests + Build
    |
    |-- Frontends --> Netlify auto-deploy --> *.deployme.cloud
    |
    +-- API Go -----> Docker build -> Push GHCR
                      +-- SSH -> VPS : docker compose pull && up -d
```

---

*Document genere le 3 mars 2026 — Deployme.cloud*

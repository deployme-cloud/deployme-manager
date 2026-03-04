# Deployme.cloud — Kubernetes as a Service

> **Vision** : Démocratiser Kubernetes en offrant une plateforme open-core permettant à quiconque de déployer un cluster Talos Linux en quelques minutes, sur n'importe quelle infrastructure.

---

## 1. Présentation du Projet

### 1.1 Le problème

Déployer et maintenir un cluster Kubernetes reste complexe, coûteux en compétences et chronophage. Les solutions managées des hyperscalers (EKS, GKE, AKS) enferment les utilisateurs dans un cloud public, tandis que les déploiements on-premise exigent une expertise rare et chère.

### 1.2 La solution — Deployme.cloud

Une plateforme unifiée qui permet de provisionner un cluster Kubernetes Talos Linux **à la demande**, sur l'infrastructure de son choix, via les outils que l'utilisateur maîtrise déjà :

- **SDK** (Go, Python, TypeScript)
- **Provider Terraform**
- **Collection Ansible**
- **Cluster API (CAPI)**

### 1.3 Pourquoi Talos Linux ?

- OS immuable et minimaliste conçu exclusivement pour Kubernetes
- Pas de SSH, pas de shell — surface d'attaque réduite au minimum
- Configuration déclarative via API (parfait pour l'automatisation)
- Mises à jour atomiques et rollback natif
- Idéal pour le edge, le on-premise et le bare-metal

---

## 2. Architecture de la Plateforme

### 2.1 Composants principaux

```
┌─────────────────────────────────────────────────────────────┐
│                    deployme.cloud                            │
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────────────┐ │
│  │  Site Vitrine │  │   Manager    │  │  Admin (VPN only) │ │
│  │   (public)    │  │  (auth/app)  │  │                   │ │
│  └──────────────┘  └──────────────┘  └───────────────────┘ │
│                            │                                │
│                    ┌───────┴────────┐                       │
│                    │   Control Plane│                       │
│                    │   (API Core)   │                       │
│                    └───────┬────────┘                       │
│          ┌─────────┬───────┼───────┬──────────┐            │
│          ▼         ▼       ▼       ▼          ▼            │
│       SDK      Terraform  CAPI  Ansible    CLI             │
│          │         │       │       │          │            │
│          └─────────┴───────┴───────┴──────────┘            │
│                          │                                  │
│          ┌───────────────┼───────────────┐                  │
│          ▼               ▼               ▼                  │
│     Phase 0          Phase 1         Phase 2-3              │
│   KVM hébergé     VMware/Proxmox    Bare-metal              │
│   (Deployme)      (On-premise)      Incus/OpenStack         │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 Les trois interfaces web

| Interface | Accès | Rôle |
|-----------|-------|------|
| **Vitrine** | Public | Landing page, docs, pricing, blog, formulaire de contact |
| **Manager** | Authentifié (utilisateurs) | Dashboard, création/gestion de clusters, monitoring, billing |
| **Admin** | VPN uniquement | Gestion infra, utilisateurs, métriques internes, support |

---

## 3. Phases de Déploiement (Cibles d'infrastructure)

### Phase 0 — Hébergement managé par Deployme *(Quick Win)*

- **Cible** : Utilisateurs qui veulent tester ou produire sans infra propre
- **Backend** : KVM sur serveurs dédiés opérés par Deployme
- **Modèle** : Payant (abonnement mensuel ou à l'heure)
- **Avantage** : Zéro friction, cluster prêt en minutes
- **Revenus** : Marges sur l'hébergement + support inclus

### Phase 1 — VMware / Proxmox on-premise *(Quick Win)*

- **Cible** : Entreprises avec infra de virtualisation existante
- **Connexion** : VPN site-to-site ou agent léger installé on-premise
- **Modèle** : Gratuit (self-service) / Payant (support + accompagnement)
- **Avantage** : Pas de migration cloud, valorisation de l'infra existante

### Phase 2 — Bare-metal

- **Cible** : Entreprises avec serveurs physiques dédiés
- **Méthode** : PXE boot + iPXE + Talos machine config
- **Modèle** : Gratuit (self-service) / Payant (support)

### Phase 3 — Incus, KVM natif, OpenStack

- **Cible** : Environnements cloud privé avancés
- **Méthode** : Providers CAPI / Terraform dédiés
- **Modèle** : Gratuit (self-service) / Payant (support)

---

## 4. Business Model — Open-Core Freemium

### 4.1 Philosophie

> **Tout le monde peut utiliser gratuitement la plateforme.** Les revenus viennent de la valeur ajoutée : hébergement, support, formation et services professionnels.

### 4.2 Grille d'offres

| | **Community (Gratuit)** | **Pro** | **Enterprise** |
|---|---|---|---|
| **Déploiement clusters** | Illimité (sur son infra) | Illimité | Illimité |
| **SDK / Terraform / Ansible / CAPI** | ✅ | ✅ | ✅ |
| **Documentation complète** | ✅ | ✅ | ✅ |
| **Crédits d'essai hébergés** | 20h/mois gratuites | — | — |
| **Hébergement managé (Phase 0)** | — | À partir de 49€/mois par cluster | Sur devis |
| **Support** | Communauté (Discord/Forum) | Email + réponse 24h | SLA 4h + Slack dédié |
| **Formation / Workshops** | Docs + vidéos gratuites | Sessions live mensuelles | Formation sur site |
| **Audit & Consulting** | — | — | Sur devis |
| **Multi-tenancy avancé** | — | — | ✅ |
| **SSO / OIDC** | — | ✅ | ✅ |

### 4.3 Sources de revenus

```
Revenus
├── 💰 Hébergement managé (Phase 0)
│   ├── Abonnements mensuels (clusters KVM)
│   └── Facturation à l'usage (CPU/RAM/Stockage)
│
├── 💰 Support professionnel
│   ├── Plans Pro (49-199€/mois)
│   └── Plans Enterprise (sur devis, >1000€/mois)
│
├── 💰 Formation & Certification
│   ├── Workshops live payants
│   ├── Programme de certification "Deployme Certified"
│   └── Formation sur site (entreprises)
│
├── 💰 Services professionnels
│   ├── Consulting architecture K8S
│   ├── Migration vers Talos Linux
│   └── Audit de sécurité clusters
│
└── 💰 Marketplace (futur)
    ├── Add-ons payants (monitoring, backup, service mesh)
    └── Templates de clusters préconfigurés
```

### 4.4 Stratégie de démocratisation

Le cœur du site web repose sur trois piliers gratuits :

1. **Documentation exhaustive** — Guides pas-à-pas, architecture de référence, troubleshooting, vidéos. Objectif : que n'importe qui puisse réussir seul.
2. **Formation gratuite** — Tutoriels vidéo, labs interactifs, parcours d'apprentissage progressifs.
3. **Crédits gratuits** — 20h/mois de cluster hébergé pour s'entraîner, prototyper, apprendre. Aucune carte bancaire requise.

---

## 5. Contenu du Site Web Vitrine

### 5.1 Pages principales

- **Accueil** — Proposition de valeur, démo animée, CTA "Déployer mon premier cluster"
- **Produit** — Fonctionnalités, phases, schéma d'architecture
- **Pricing** — Grille Community / Pro / Enterprise
- **Documentation** — Hub central (guides, API ref, tutoriels, FAQ)
- **Formation** — Catalogue de cours, labs, certification
- **Blog** — Articles techniques, retours d'expérience, annonces
- **À propos** — Équipe, vision, roadmap publique
- **Contact / Support** — Formulaire, liens Discord, demande de démo

### 5.2 Stack technique suggérée (lancement rapide)

| Composant | Technologie | Justification |
|-----------|-------------|---------------|
| Vitrine | Astro + TailwindCSS | Rapide, SEO natif, déploiement statique |
| Documentation | Starlight (Astro) ou Docusaurus | Recherche intégrée, versioning, MDX |
| Manager | Next.js ou SvelteKit | SSR, auth intégrée, API routes |
| Admin | même stack que Manager (route protégée) | Mutualisation du code |
| Backend API | Go ou Rust | Performance, binaire unique, écosystème K8S |
| Base de données | PostgreSQL | Fiable, extensible |
| Auth | Zitadel ou Keycloak | OIDC, multi-tenant, self-hosted |
| CI/CD | Gitea + Woodpecker CI (ou GitHub Actions) | Self-hosted possible |
| Monitoring | Prometheus + Grafana | Standard K8S |

---

## 6. Plan de Gestion de Projet

### 6.1 Vue d'ensemble des sprints

```
  MOIS 1         MOIS 2         MOIS 3         MOIS 4         MOIS 5-6
┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌──────────────┐
│ Sprint 0  │ │ Sprint 1  │ │ Sprint 2  │ │ Sprint 3  │ │  Sprint 4-5  │
│ Fondations│ │ Phase 0   │ │ Phase 0   │ │ Phase 1   │ │  Phase 1     │
│ + Vitrine │ │ MVP       │ │ Complet   │ │ MVP       │ │  Complet     │
│           │ │           │ │ + Billing │ │           │ │  + Formation │
└───────────┘ └───────────┘ └───────────┘ └───────────┘ └──────────────┘
     ▲              ▲             ▲              ▲              ▲
  Quick Win 1   Quick Win 2   Quick Win 3   Quick Win 4    Consolidation
```

### 6.2 Sprint 0 — Fondations + Vitrine (Semaines 1-4) 🏁 QUICK WIN

**Objectif** : Site en ligne + premier cluster déployable manuellement.

**Semaine 1-2 : Infrastructure de base**
- Mettre en place le repo Git (monorepo ou multi-repo)
- Configurer le domaine deployme.cloud + DNS + certificats
- Déployer la vitrine statique (Astro) avec pages : Accueil, Produit, Pricing, Contact
- Mettre en place le VPN (WireGuard) pour l'accès admin
- Installer et configurer le premier serveur KVM (hébergement Phase 0)

**Semaine 3-4 : Documentation initiale + Proof of Concept**
- Écrire les 5 premiers guides de documentation (Getting Started, Architecture, Concepts, FAQ, Troubleshooting)
- Créer manuellement un cluster Talos Linux sur KVM (documenter chaque étape)
- Automatiser la création via script (base du futur SDK)
- Mettre en ligne la documentation (Starlight/Docusaurus)
- Ouvrir un serveur Discord communautaire

**Livrable** : Site vitrine live + documentation de base + capacité à créer un cluster Talos manuellement en <30 min.

---

### 6.3 Sprint 1 — Phase 0 MVP (Semaines 5-8) 🏁 QUICK WIN

**Objectif** : Premiers utilisateurs peuvent créer un cluster hébergé via l'interface.

**Semaine 5-6 : Backend API + Manager V1**
- Développer l'API core (Go/Rust) : endpoints CRUD clusters
- Intégrer la création de VMs KVM via API (libvirt)
- Développer le Manager V1 : inscription, login, dashboard basique
- Implémenter l'auth (Zitadel/Keycloak)

**Semaine 7-8 : Workflow de déploiement**
- Workflow complet : demande → provisioning VM → installation Talos → kubeconfig
- Système de crédits gratuits (20h/mois, sans CB)
- Page de statut du cluster dans le Manager
- Tests end-to-end du workflow

**Livrable** : Un utilisateur peut s'inscrire, créer un cluster Talos hébergé, récupérer son kubeconfig et commencer à travailler — le tout en <10 min.

---

### 6.4 Sprint 2 — Phase 0 Complet + Billing (Semaines 9-12)

**Objectif** : Monétisation active de l'hébergement.

- Intégrer Stripe pour le paiement (abonnements + usage)
- Implémenter le cycle de vie complet des clusters (scale up/down, delete, upgrade)
- Dashboard de monitoring (Grafana embarqué ou métriques custom)
- Panel Admin (VPN) : vue sur tous les clusters, métriques, gestion utilisateurs
- Provider Terraform v0.1 (Phase 0 uniquement)
- SDK Go v0.1
- Enrichir la documentation (10+ guides supplémentaires)

**Livrable** : Plateforme commercialisable pour l'hébergement managé.

---

### 6.5 Sprint 3 — Phase 1 MVP (Semaines 13-16) 🏁 QUICK WIN

**Objectif** : Déployer un cluster sur le VMware/Proxmox de l'utilisateur.

**Semaine 13-14 : Agent on-premise**
- Développer l'agent léger (Go, binaire unique)
- Communication sécurisée agent ↔ control plane (mTLS ou WireGuard)
- Support Proxmox VE : création de VMs via API Proxmox
- Support VMware vSphere : création de VMs via govmomi

**Semaine 15-16 : Intégration Manager**
- Workflow : inscription agent → découverte infra → création cluster
- Dashboard : vue unifiée clusters hébergés + on-premise
- Documentation Phase 1 (guides d'installation agent, prérequis réseau)
- Collection Ansible v0.1

**Livrable** : Un utilisateur avec un Proxmox ou VMware peut installer l'agent et déployer un cluster Talos depuis le Manager.

---

### 6.6 Sprint 4-5 — Consolidation (Semaines 17-24)

- Stabilisation et hardening sécurité
- Programme beta public
- Premiers contenus de formation (vidéos, labs)
- Provider CAPI (Cluster API) v0.1
- SDK Python et TypeScript
- Système de tickets support (pour plans Pro)
- Métriques business (MRR, churn, adoption)
- Préparation Phase 2 (bare-metal) — R&D

---

## 7. Quick Wins — Résumé

| # | Quick Win | Délai | Impact |
|---|-----------|-------|--------|
| 1 | Site vitrine + docs en ligne | 2 semaines | Visibilité, SEO, crédibilité |
| 2 | Premier cluster créable via UI | 6 semaines | Preuve de valeur, premiers users |
| 3 | Billing + Terraform provider | 10 semaines | Premiers revenus |
| 4 | Phase 1 (Proxmox/VMware) | 14 semaines | Différenciation marché majeure |

---

## 8. KPIs à Suivre

| Métrique | Objectif M+3 | Objectif M+6 |
|----------|-------------|-------------|
| Inscriptions (Community) | 200 | 1 000 |
| Clusters déployés (total) | 50 | 500 |
| Clients payants (Pro/Enterprise) | 5 | 25 |
| MRR (Monthly Recurring Revenue) | 500€ | 5 000€ |
| Articles de documentation | 30 | 80 |
| Étoiles GitHub (si open-source) | 100 | 500 |
| Membres Discord | 50 | 300 |

---

## 9. Risques & Mitigations

| Risque | Impact | Mitigation |
|--------|--------|------------|
| Adoption lente | Pas de revenus | Crédits gratuits généreux, contenu SEO agressif, présence sur Reddit/HN |
| Complexité technique sous-estimée | Retards | MVP strict, scope réduit par sprint, pas de feature creep |
| Concurrence (Sidero/Omni, Spectro Cloud) | Parts de marché | Différenciation par le gratuit, le on-premise, et la simplicité |
| Sécurité (accès aux infras clients) | Confiance | Audit externe, mTLS partout, agent open-source, doc transparente |
| Scalabilité du support | Qualité dégradée | Docs exhaustives, chatbot, communauté Discord active |

---

## 10. Roadmap Long Terme

```
2025 S1 ──── Phase 0 + Phase 1 (MVP → GA)
2025 S2 ──── Phase 2 (Bare-metal) + Marketplace add-ons
2026 S1 ──── Phase 3 (Incus/OpenStack) + Certification program
2026 S2 ──── Multi-cloud (AWS/GCP/Azure comme cible optionnelle)
2027    ──── Federation multi-clusters + GitOps natif
```

---

## 11. Conclusion

Deployme.cloud se positionne comme la solution **la plus accessible** pour déployer Kubernetes sur n'importe quelle infrastructure. En misant sur la gratuité du cœur de plateforme, une documentation de premier ordre et des crédits d'essai sans friction, le projet peut rapidement construire une communauté solide. Les revenus viendront naturellement de l'hébergement managé, du support professionnel et de la formation — là où la vraie valeur est créée pour les entreprises.

**Prochaine action** : Lancer le Sprint 0 — acheter le domaine, déployer la vitrine, écrire les premiers docs.

---

*Document généré le 3 mars 2026 — Deployme.cloud*

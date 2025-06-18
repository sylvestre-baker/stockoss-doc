# 🔐 BACKLOG - CORE SERVICES

## 🎯 **SCOPE**
Services fondamentaux pour l'authentification, la gestion des utilisateurs et des partenaires.
**12 user stories** - **240 points** - **12 sprints**

---

## 📊 **RÉPARTITION**

### **Authentification (5 endpoints)**
- Connexion utilisateurs (email/password)
- Authentification partenaires (clientId/clientSecret)
- Gestion des tokens JWT
- Refresh tokens
- Logout sécurisé

### **Gestion Utilisateurs (5 endpoints)**
- CRUD utilisateurs
- Gestion des rôles
- Profils utilisateurs
- Permissions
- Audit des actions

### **Gestion Partenaires (2 endpoints)**
- Enregistrement partenaires
- Configuration API

---

## 🔐 **AUTHENTIFICATION - 5 USER STORIES**

### US-001 : Connexion Utilisateur Email/Password

#### 📝 **Description**
En tant qu'utilisateur Stockoss, je veux me connecter avec mon email et mot de passe afin d'accéder à l'interface de gestion.

#### 🎯 **Critères d'Acceptation**
- [ ] Endpoint POST /auth/login accepte email + password
- [ ] Validation des credentials contre Odoo/OFBiz
- [ ] Retour d'un JWT token valide (durée 24h)
- [ ] Refresh token généré (durée 30 jours)
- [ ] Gestion des erreurs (credentials invalides, compte désactivé)
- [ ] Rate limiting (5 tentatives/minute)

#### 🏗️ **Détails Techniques**
- **Endpoint** : POST /auth/login
- **Service Backend** : Odoo Auth Module
- **Transformation** : Stockoss LoginRequest → Odoo auth → Stockoss LoginResponse
- **Sécurité** : Bcrypt, JWT, Rate limiting

#### 📈 **Estimation**
- **Points** : 13
- **Priorité** : P0 (Critique)
- **Sprint** : Sprint 1

#### 🔗 **Dépendances**
- Infrastructure : Base de données configurée
- Infrastructure : Service JWT configuré

#### 🧪 **Tests**
- [ ] Tests unitaires du service d'authentification
- [ ] Tests d'intégration avec Odoo
- [ ] Tests E2E de connexion

#### 📋 **Tâches Techniques**
- [ ] Contrôleur AuthController (NestJS)
- [ ] Service AuthService avec intégration Odoo
- [ ] DTOs LoginRequest/LoginResponse
- [ ] Middleware JWT
- [ ] Service de rate limiting
- [ ] Gestion d'erreurs personnalisées
- [ ] Documentation Swagger

---

### US-002 : Authentification Partenaire ClientId/Secret

#### 📝 **Description**
En tant que partenaire technique, je veux m'authentifier avec clientId/clientSecret afin d'accéder aux APIs Stockoss.

#### 🎯 **Critères d'Acceptation**
- [ ] Endpoint POST /auth/partner-token
- [ ] Validation clientId/clientSecret
- [ ] Token JWT spécifique partenaires (durée 1h)
- [ ] Scopes/permissions par partenaire
- [ ] Audit des connexions partenaires

#### 🏗️ **Détails Techniques**
- **Endpoint** : POST /auth/partner-token
- **Service Backend** : Service Custom Partners
- **Transformation** : PartnerTokenRequest → JWT avec scopes
- **Sécurité** : Client credentials OAuth2-like

#### 📈 **Estimation**
- **Points** : 21
- **Priorité** : P0 (Critique)
- **Sprint** : Sprint 2

#### 🔗 **Dépendances**
- US-001 : Service JWT configuré
- Base : Table partners configurée

#### 🧪 **Tests**
- [ ] Tests unitaires service partenaires
- [ ] Tests de validation scopes
- [ ] Tests E2E authentification partenaire

#### 📋 **Tâches Techniques**
- [ ] Service PartnerAuthService
- [ ] Gestion des scopes/permissions
- [ ] DTOs PartnerTokenRequest/Response
- [ ] Middleware de validation partenaires
- [ ] Audit service pour partenaires
- [ ] Documentation API partenaires

---

### US-003 : Gestion des Tokens JWT

#### 📝 **Description**
En tant que système, je veux gérer les tokens JWT (validation, refresh, révocation) afin de sécuriser les accès.

#### 🎯 **Critères d'Acceptation**
- [ ] Validation automatique des tokens sur chaque requête
- [ ] Endpoint POST /auth/refresh pour renouveler
- [ ] Endpoint POST /auth/revoke pour révoquer
- [ ] Blacklist des tokens révoqués
- [ ] Logs de sécurité complets

#### 🏗️ **Détails Techniques**
- **Endpoints** : /auth/refresh, /auth/revoke
- **Service Backend** : Service JWT custom
- **Sécurité** : Redis pour blacklist, signature RS256

#### 📈 **Estimation**
- **Points** : 21
- **Priorité** : P0 (Critique)
- **Sprint** : Sprint 1

#### 🔗 **Dépendances**
- US-001 : Service JWT de base
- Infrastructure : Redis configuré

#### 🧪 **Tests**
- [ ] Tests validation tokens
- [ ] Tests refresh tokens
- [ ] Tests révocation/blacklist

#### 📋 **Tâches Techniques**
- [ ] JwtService avec validation complète
- [ ] Redis service pour blacklist
- [ ] Middleware de validation globale
- [ ] Service de refresh tokens
- [ ] Audit et logs de sécurité

---

### US-004 : Endpoint de Validation Token

#### 📝 **Description**
En tant que service interne, je veux valider un token JWT afin de vérifier les permissions d'accès.

#### 🎯 **Critères d'Acceptation**
- [ ] Endpoint GET /auth/validate
- [ ] Retour des informations utilisateur/partenaire
- [ ] Validation des permissions/scopes
- [ ] Performance < 50ms
- [ ] Cache des validations fréquentes

#### 🏗️ **Détails Techniques**
- **Endpoint** : GET /auth/validate
- **Service Backend** : Service JWT + Cache Redis
- **Performance** : Cache 5 minutes, < 50ms

#### 📈 **Estimation**
- **Points** : 13
- **Priorité** : P0 (Critique)
- **Sprint** : Sprint 2

#### 🔗 **Dépendances**
- US-003 : Gestion JWT complète
- Infrastructure : Cache Redis

#### 🧪 **Tests**
- [ ] Tests de validation
- [ ] Tests de performance
- [ ] Tests de cache

#### 📋 **Tâches Techniques**
- [ ] Endpoint de validation
- [ ] Service de cache pour tokens
- [ ] Optimisation des performances
- [ ] Métriques de monitoring

---

### US-005 : Logout Sécurisé

#### 📝 **Description**
En tant qu'utilisateur, je veux me déconnecter proprement afin de sécuriser ma session.

#### 🎯 **Critères d'Acceptation**
- [ ] Endpoint POST /auth/logout
- [ ] Révocation du token actuel
- [ ] Révocation optionnelle de toutes les sessions
- [ ] Nettoyage des données de session
- [ ] Audit de la déconnexion

#### 🏗️ **Détails Techniques**
- **Endpoint** : POST /auth/logout
- **Service Backend** : Service JWT + Audit
- **Sécurité** : Révocation immédiate

#### 📈 **Estimation**
- **Points** : 8
- **Priorité** : P1 (Important)
- **Sprint** : Sprint 2

#### 🔗 **Dépendances**
- US-003 : Gestion JWT avec révocation

#### 🧪 **Tests**
- [ ] Tests de logout simple
- [ ] Tests de logout toutes sessions
- [ ] Tests d'audit

#### 📋 **Tâches Techniques**
- [ ] Endpoint de logout
- [ ] Service de révocation multiple
- [ ] Nettoyage des sessions
- [ ] Audit des déconnexions

---

## 👥 **GESTION UTILISATEURS - 5 USER STORIES**

### US-006 : CRUD Utilisateurs

#### 📝 **Description**
En tant qu'administrateur, je veux gérer les utilisateurs (créer, lire, modifier, supprimer) afin de contrôler les accès.

#### 🎯 **Critères d'Acceptation**
- [ ] GET /users (liste paginée)
- [ ] GET /users/{id} (détail utilisateur)
- [ ] POST /users (création)
- [ ] PUT /users/{id} (modification)
- [ ] DELETE /users/{id} (suppression logique)
- [ ] Validation des données (email unique, etc.)

#### 🏗️ **Détails Techniques**
- **Endpoints** : CRUD complet /users
- **Service Backend** : Odoo Users Module
- **Transformation** : Stockoss User ↔ Odoo User

#### 📈 **Estimation**
- **Points** : 21
- **Priorité** : P0 (Critique)
- **Sprint** : Sprint 3

#### 🔗 **Dépendances**
- US-001 : Authentification fonctionnelle
- Infrastructure : Base Odoo configurée

#### 🧪 **Tests**
- [ ] Tests CRUD complets
- [ ] Tests de validation
- [ ] Tests de permissions

#### 📋 **Tâches Techniques**
- [ ] UserController avec CRUD
- [ ] UserService avec intégration Odoo
- [ ] DTOs User (Create, Update, Response)
- [ ] Validation des données
- [ ] Pagination et filtres
- [ ] Permissions d'accès

---

### US-007 : Gestion des Rôles

#### 📝 **Description**
En tant qu'administrateur, je veux gérer les rôles utilisateurs afin de contrôler les permissions.

#### 🎯 **Critères d'Acceptation**
- [ ] GET /roles (liste des rôles)
- [ ] POST /users/{id}/roles (assigner rôle)
- [ ] DELETE /users/{id}/roles/{roleId} (retirer rôle)
- [ ] Rôles prédéfinis : Admin, Manager, Operator, Viewer
- [ ] Validation des permissions par rôle

#### 🏗️ **Détails Techniques**
- **Endpoints** : /roles, /users/{id}/roles
- **Service Backend** : Odoo Access Rights
- **Sécurité** : RBAC (Role-Based Access Control)

#### 📈 **Estimation**
- **Points** : 13
- **Priorité** : P0 (Critique)
- **Sprint** : Sprint 3

#### 🔗 **Dépendances**
- US-006 : CRUD Utilisateurs
- Infrastructure : Système de permissions

#### 🧪 **Tests**
- [ ] Tests d'assignation de rôles
- [ ] Tests de permissions par rôle
- [ ] Tests de sécurité RBAC

#### 📋 **Tâches Techniques**
- [ ] RoleController
- [ ] Service de gestion des rôles
- [ ] Middleware de vérification permissions
- [ ] DTOs Role et UserRole
- [ ] Configuration des rôles prédéfinis

---

### US-008 : Profils Utilisateurs

#### 📝 **Description**
En tant qu'utilisateur, je veux gérer mon profil afin de personnaliser mon compte.

#### 🎯 **Critères d'Acceptation**
- [ ] GET /users/profile (mon profil)
- [ ] PUT /users/profile (modifier profil)
- [ ] PUT /users/password (changer mot de passe)
- [ ] Upload d'avatar
- [ ] Préférences utilisateur (langue, timezone)

#### 🏗️ **Détails Techniques**
- **Endpoints** : /users/profile, /users/password
- **Service Backend** : Odoo Users + Storage pour avatars
- **Sécurité** : Validation identité avant changement password

#### 📈 **Estimation**
- **Points** : 13
- **Priorité** : P1 (Important)
- **Sprint** : Sprint 4

#### 🔗 **Dépendances**
- US-006 : CRUD Utilisateurs
- Infrastructure : Service de stockage fichiers

#### 🧪 **Tests**
- [ ] Tests de modification profil
- [ ] Tests de changement password
- [ ] Tests d'upload avatar

#### 📋 **Tâches Techniques**
- [ ] ProfileController
- [ ] Service de gestion profils
- [ ] Upload et stockage avatars
- [ ] Validation changement password
- [ ] DTOs Profile et PasswordChange

---

### US-009 : Système de Permissions

#### 📝 **Description**
En tant que système, je veux vérifier les permissions utilisateur afin de sécuriser chaque action.

#### 🎯 **Critères d'Acceptation**
- [ ] Middleware de vérification permissions sur chaque endpoint
- [ ] Permissions granulaires par module (WMS, OMS, etc.)
- [ ] Cache des permissions pour performance
- [ ] Logs des tentatives d'accès non autorisé
- [ ] API de vérification des permissions

#### 🏗️ **Détails Techniques**
- **Service** : PermissionService avec cache Redis
- **Intégration** : Middleware global NestJS
- **Performance** : Cache 10 minutes, < 20ms

#### 📈 **Estimation**
- **Points** : 21
- **Priorité** : P0 (Critique)
- **Sprint** : Sprint 4

#### 🔗 **Dépendances**
- US-007 : Gestion des rôles
- Infrastructure : Cache Redis

#### 🧪 **Tests**
- [ ] Tests de permissions par rôle
- [ ] Tests de performance cache
- [ ] Tests de sécurité

#### 📋 **Tâches Techniques**
- [ ] PermissionService avec cache
- [ ] Middleware de vérification globale
- [ ] Décorateurs de permissions
- [ ] Configuration des permissions par module
- [ ] Monitoring des accès

---

### US-010 : Audit des Actions Utilisateurs

#### 📝 **Description**
En tant qu'administrateur, je veux consulter l'historique des actions utilisateurs afin d'assurer la traçabilité.

#### 🎯 **Critères d'Acceptation**
- [ ] GET /audit/users/{id} (historique utilisateur)
- [ ] GET /audit/actions (toutes les actions)
- [ ] Filtres par date, utilisateur, action, module
- [ ] Pagination et export CSV
- [ ] Rétention des logs (90 jours)

#### 🏗️ **Détails Techniques**
- **Endpoints** : /audit/*
- **Service Backend** : Service Audit custom
- **Storage** : Base de données dédiée audit

#### 📈 **Estimation**
- **Points** : 13
- **Priorité** : P1 (Important)
- **Sprint** : Sprint 5

#### 🔗 **Dépendances**
- US-009 : Système de permissions
- Infrastructure : Base de données audit

#### 🧪 **Tests**
- [ ] Tests d'enregistrement audit
- [ ] Tests de consultation historique
- [ ] Tests de rétention des données

#### 📋 **Tâches Techniques**
- [ ] AuditController
- [ ] AuditService avec base dédiée
- [ ] Middleware d'audit automatique
- [ ] Service de rétention des logs
- [ ] Export CSV des audits

---

## 🤝 **GESTION PARTENAIRES - 2 USER STORIES**

### US-011 : Enregistrement Partenaires

#### 📝 **Description**
En tant qu'administrateur, je veux enregistrer de nouveaux partenaires afin de leur donner accès aux APIs.

#### 🎯 **Critères d'Acceptation**
- [ ] POST /partners (création partenaire)
- [ ] Génération automatique clientId/clientSecret
- [ ] Configuration des scopes d'accès
- [ ] Validation des informations partenaire
- [ ] Email de confirmation avec credentials

#### 🏗️ **Détails Techniques**
- **Endpoint** : POST /partners
- **Service Backend** : Service Partners custom
- **Sécurité** : Génération sécurisée des credentials

#### 📈 **Estimation**
- **Points** : 21
- **Priorité** : P1 (Important)
- **Sprint** : Sprint 5

#### 🔗 **Dépendances**
- US-002 : Authentification partenaires
- Infrastructure : Service email

#### 🧪 **Tests**
- [ ] Tests de création partenaire
- [ ] Tests de génération credentials
- [ ] Tests d'envoi email

#### 📋 **Tâches Techniques**
- [ ] PartnerController
- [ ] PartnerService
- [ ] Génération sécurisée credentials
- [ ] Service d'envoi email
- [ ] DTOs Partner

---

### US-012 : Configuration API Partenaires

#### 📝 **Description**
En tant qu'administrateur, je veux configurer les accès API des partenaires afin de contrôler leurs permissions.

#### 🎯 **Critères d'Acceptation**
- [ ] GET /partners/{id}/config (configuration actuelle)
- [ ] PUT /partners/{id}/config (modifier configuration)
- [ ] Gestion des scopes (lecture, écriture par module)
- [ ] Rate limiting par partenaire
- [ ] Monitoring des usages

#### 🏗️ **Détails Techniques**
- **Endpoints** : /partners/{id}/config
- **Service Backend** : Service Partners + Config
- **Monitoring** : Métriques d'usage temps réel

#### 📈 **Estimation**
- **Points** : 13
- **Priorité** : P1 (Important)
- **Sprint** : Sprint 6

#### 🔗 **Dépendances**
- US-011 : Enregistrement partenaires
- Infrastructure : Monitoring

#### 🧪 **Tests**
- [ ] Tests de configuration scopes
- [ ] Tests de rate limiting
- [ ] Tests de monitoring

#### 📋 **Tâches Techniques**
- [ ] Service de configuration partenaires
- [ ] Gestion des scopes granulaires
- [ ] Rate limiting par partenaire
- [ ] Dashboard monitoring usage
- [ ] DTOs PartnerConfig

---

## 📊 **RÉSUMÉ CORE SERVICES**

### **Statistiques**
- **Total** : 12 user stories
- **Points** : 240 points
- **Sprints** : 12 sprints (6 mois avec équipe de 4 devs)

### **Priorités**
- **P0 (Critique)** : 7 stories (Authentification + Permissions)
- **P1 (Important)** : 5 stories (Profils + Partenaires)

### **Dépendances Critiques**
- Infrastructure : Base de données, Redis, JWT
- Services : Email, Stockage fichiers
- Intégrations : Odoo Users/Auth modules

### **Livrables**
- **Sprint 1-2** : Authentification complète
- **Sprint 3-4** : Gestion utilisateurs et permissions
- **Sprint 5-6** : Partenaires et audit

Ce backlog **Core Services** constitue les **fondations sécurisées** de toute la plateforme Stockoss ! 🔐 
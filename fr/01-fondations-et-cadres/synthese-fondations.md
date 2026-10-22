# Module 1 : Fondations et Gouvernance de la Sécurité

Ce document formalise les connaissances fondamentales acquises lors du premier volet du cursus Google Cybersecurity Professional Certificate, axé sur les principes défensifs, les cadres méthodologiques et l'outillage opérationnel en SOC.

---

## Sommaire
1. [Section 1 : La Triade CIA en Pratique](#section-1--la-triade-cia-en-pratique)
2. [Section 2 : Cadres de Sécurité et Typologie des Contrôles](#section-2--cadres-de-sécurité-et-typologie-des-contrôles)
3. [Section 3 : Architecture Opérationnelle SOC et Détection](#section-3--architecture-opérationnelle-soc-et-détection)

---

## Section 1 : La Triade CIA en Pratique

La Triade CIA constitue le modèle fondamental guidant la définition des politiques de sécurité, l'architecture défensive et le traitement des incidents opérationnels.

### 1. Définitions Opérationnelles

* **Confidentialité :** Garantit que les données, identifiants et systèmes ne sont accessibles qu'aux entités et personnes dûment autorisées.
  * *Mécanismes associés :* Chiffrement (au repos et en transit), listes de contrôle d'accès (ACL), authentification multifacteur (MFA), principe du moindre privilège.
* **Intégrité :** Garantit l'exactitude, la cohérence et l'authenticité des données et configurations, en prévenant toute modification ou suppression non autorisée.
  * *Mécanismes associés :* Hachage cryptographique (ex. SHA-256), signatures numériques, contrôle de version, surveillance de l'intégrité des fichiers (FIM).
* **Disponibilité :** Assure que les ressources, réseaux et applications restent accessibles et opérationnels pour les utilisateurs légitimes dès que nécessaire.
  * *Mécanismes associés :* Redondance matérielle, répartiteurs de charge (load balancers), sauvegardes régulières testées, plans de reprise d'activité (PRA).

### 2. Scénarios d'Impact Isolé

Pour évaluer correctement un incident, un analyste doit distinguer quel pilier est précisément ciblé :

* **Atteinte isolée à la Confidentialité :**
  * *Scénario :* Exfiltration silencieuse d'une table d'identifiants ou interception de correspondances par écoute passive. Les systèmes restent fonctionnels (Disponibilité intacte) et les données originales ne sont pas altérées (Intégrité préservée).
* **Atteinte isolée à l'Intégrité :**
  * *Scénario :* Modification non autorisée du relevé d'identité bancaire d'un fournisseur dans une base de données de facturation. Le portail reste accessible et les données ne sont pas divulguées à l'extérieur, mais l'information utilisée est corrompue.
* **Atteinte isolée à la Disponibilité :**
  * *Scénario :* Attaque par déni de service distribué (DDoS) saturant la bande passante d'un serveur web public. Aucune donnée n'est consultée ni corrompue, mais le service devient totalement inaccessible aux utilisateurs légitimes.

### 3. Gestion de la Friction Opérationnelle

Un durcissement excessif des contrôles de confidentialité (ex. exigences d'authentification récurrentes ou blocages géographiques stricts) peut impacter négativement la disponibilité opérationnelle des collaborateurs. Le rôle de la sécurité consiste à trouver l'équilibre adapté au modèle de menace de l'organisation.

---

## Section 2 : Cadres de Sécurité et Typologie des Contrôles

Les cadres de sécurité (frameworks) définissent la méthodologie et les objectifs globaux, tandis que les contrôles de sécurité représentent les mesures techniques, administratives et physiques concrètes mises en place pour atteindre ces objectifs.

### 1. Alignement sur le Cadre NIST CSF

Le NIST Cybersecurity Framework (CSF) organise la gestion du risque cybernétique autour de cinq fonctions directrices complémentaires :

| Fonction NIST | Objectif Opérationnel | Exemple de Mesure Associée |
| :--- | :--- | :--- |
| **Identifier** | Cartographier les actifs critiques, les données et les risques pesant sur le système. | Gestion de parc (inventaire d'actifs), analyse de vulnérabilités. |
| **Protéger** | Déployer les barrières nécessaires pour limiter l'impact d'une menace potentielle. | Pare-feu, authentification multifacteur (MFA), gestion des accès. |
| **Détecter** | Identifier rapidement la survenue d'une anomalie ou d'un incident de sécurité. | Ingestion de journaux dans un SIEM, sondes IDS/IPS, alertes EDR. |
| **Répondre** | Contenir la menace et intervenir dès qu'un incident avéré est détecté. | Isolement réseau d'un hôte, révocation de sessions actives. |
| **Récupérer** | Restaurer les capacités opérationnelles et les services altérés. | Restauration à partir de sauvegardes saines, patch correctif. |

### 2. Matrice de Classification des Contrôles

Les contrôles de sécurité sont classés à la fois selon leur méthode de déploiement et selon leur rôle opérationnel :

* **Par méthode de mise en œuvre :**
  * **Techniques (logiques) :** Dispositifs logiciels et matériels (chiffrement, pare-feu, listes de contrôle d'accès).
  * **Organisationnels (administratifs) :** Politiques internes, normes d'utilisation acceptable, formations de sensibilisation, procédures d'escalade.
  * **Physiques :** Obstacles tangibles protégeant les installations (badges RFID, caméras de surveillance, sas d'accès aux salles serveurs).

* **Par fonction opérationnelle :**
  * **Préventifs :** Conçus pour empêcher un incident de se produire en amont (ex. règle de pare-feu rejetant les connexions non sollicitées, politique de moindre privilège).
  * **Détectifs :** Conçus pour identifier et signaler une action suspecte en cours (ex. règle de corrélation SIEM sur tentatives échouées multiples, journaux d'audit).
  * **Correctifs :** Conçus pour réparer les dommages et ramener l'état du système à la normale (ex. réinstallation d'une image système compromise, restauration de base de données).

---

## Section 3 : Architecture Opérationnelle SOC et Détection

Au sein d'un Centre Opérationnel de Sécurité (SOC), la détection repose sur une chaîne outillée associant collecte télémétrique, analyse humaine et automatisation.

### 1. Rôle et Articulation SIEM vs SOAR

* **SIEM (Security Information and Event Management) — Visibilité et Détection :**
  * **Ingestion centralisée :** Collecte continue des journaux d'événements issus des postes de travail, serveurs, pare-feux et fournisseurs d'identité.
  * **Normalisation :** Harmonisation des formats de logs hétérogènes vers un schéma commun (ex. mappage unifié des champs IP sources).
  * **Corrélation :** Application de règles analytiques pour repérer des comportements suspects à travers plusieurs sources (ex. tentatives répétées d'authentification suivies d'une connexion réussie anormale).
  * *Positionnement NIST :* Fonction **Détecter**.

* **SOAR (Security Orchestration, Automation, and Response) — Réponse et Orchestration :**
  * **Exécution de playbooks :** Déclenchement automatique de séquences de remédiation pour les alertes récurrentes ou chronophages.
  * **Actions standardisées :** Enrichissement d'indicateurs de compromission (IoC), blocage automatique d'adresses IP malveillantes sur le pare-feu, isolement temporaire d'un compte.
  * *Positionnement NIST :* Fonction **Répondre**.

### 2. Flux de Traitement d'un Événement de Sécurité

1. **Émission :** Les composants d'infrastructure génèrent des journaux d'audit et d'activité (syslog, auth.log, logs de pare-feu).
2. **Centralisation :** Le SIEM ingère, normalise et applique les règles de corrélation sur le flux télémétrique.
3. **Alerte :** Le seuil de menace est franchi, générant un ticket d'incident priorisé.
4. **Triage Analyste (Tier-1) :** Vérification de l'alerte, validation du contexte (élimination des faux positifs) et endiguement initial selon les procédures opérationnelles standards (SOP).
5. **Orchestration SOAR / Escalade :** Remédiation automatisée via playbooks ou transfert au Tier-2 en cas d'incident complexe.

### 3. Rôle de la Culture de Sécurité dans la Détection

La télémétrie logicielle ne couvre pas l'intégralité des vecteurs d'attaque. Une culture de sécurité saine transforme chaque collaborateur en capteur défensif actif :
* **Signalement sans friction :** Les collaborateurs doivent pouvoir signaler un courriel d'hameçonnage (phishing) suspect immédiatement, sans crainte de sanction ou de reproche.
* **Alimentation du SOC :** Les signalements utilisateurs nourrissent directement la chaîne de détection du SOC, permettant d'extraire des indicateurs de compromission avant l'exécution d'une charge virale.
* **Réduction du MTTD :** L'association d'outils techniques et de signalements humains vigilants réduit significativement le temps moyen de détection des menaces (Mean Time to Detect).

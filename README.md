# 🛡️ SOC Interne Automatisé — MSSP Infrastructure

> Conception et déploiement d'une plateforme SOC (Security Operations Center) interne, entièrement basée sur des outils **open source**, pour un fournisseur de services de sécurité managés (MSSP).

**Projet de Fin d'Études — Ingénieur d'État en Ingénierie des Réseaux Intelligents et Cybersécurité**
Réalisé au sein de **Devoteam Cyber Trust Africa** | Soutenu le 24 juin 2026
Encadrement : Ayoub Lachhab (Devoteam) · Pr. Jihane Jebrane (ENSA Khouribga)

---

## 📌 Contexte

Le SOC interne du MSSP reposait initialement sur **Wazuh** seul, limitant la visibilité réseau, l'automatisation du traitement des alertes et les capacités d'investigation. Ce projet étend cette base vers une plateforme intégrée de **détection → corrélation → réponse → investigation forensique**, avec assistance par IA locale.

## 🏗️ Architecture

La plateforme est organisée en **5 couches fonctionnelles** :

| Couche | Outils | Rôle |
|---|---|---|
| **Collecte** | Wazuh, Zeek | Supervision endpoints + visibilité réseau |
| **Agrégation** | Graylog | Centralisation, normalisation, pipelines de parsing |
| **Détection & Orchestration** | SOCFortress Copilot + Ollama/Llama 3 | Corrélation, enrichissement des alertes par IA locale |
| **Gestion des vulnérabilités** | OpenVAS | Scan CVE, priorisation des correctifs |
| **Réponse & Forensique** | DFIR-IRIS, Velociraptor | Gestion des incidents, collecte d'artefacts en temps réel |

![Architecture globale](docs/architecture.png)

Toutes les briques communiquent via API, avec des scripts Python développés sur-mesure pour combler l'absence de connecteurs natifs (ex : SOCFortress Copilot → IRIS, OpenVAS → IRIS, IRIS → Velociraptor).

## ⚙️ Stack technique

`Wazuh` `Zeek` `Graylog` `OpenVAS` `SOCFortress Copilot` `Ollama (Llama 3)` `DFIR-IRIS` `Velociraptor` `Docker` `Filebeat` `Python`

**Choix clé :** remplacement de l'API OpenAI (GPT-4, payante et cloud) par **Ollama + Llama 3 en local**, garantissant confidentialité des données et absence de dépendance externe — critère non négociable en environnement MSSP.

## 🧪 Validation — Étude de cas : attaque SSH brute-force

Scénario end-to-end simulé pour valider la chaîne complète :

```
Attaque (Hydra) → Zeek (capture réseau) → Wazuh (détection endpoint)
   → Graylog (corrélation + règle de détection)
   → SOCFortress Copilot (enrichissement IA)
   → DFIR-IRIS (création + qualification du cas)
   → Velociraptor (collecte forensique + reconstruction de chronologie)
```

**Résultats :**

| Critère | Résultat |
|---|---|
| Attaque détectée | ✅ |
| Alerte générée automatiquement | ✅ |
| Cas d'incident créé automatiquement | ✅ |
| Transmission automatisée vers DFIR-IRIS | ✅ |
| Prise en charge complète de bout en bout | ✅ |

→ Amélioration du **MTTD** (Mean Time to Detect) et **MTTR** (Mean Time to Respond) grâce à l'automatisation des transmissions inter-outils, éliminant la corrélation manuelle.

## 🔍 Méthodologie de sélection des outils

Chaque outil a été choisi via une grille de critères (coût, fiabilité, interopérabilité, scalabilité, confidentialité des données) comparant 4 à 5 alternatives par catégorie — voir [`docs/benchmark-summary.md`](docs/benchmark-summary.md) pour la synthèse complète des tableaux d'évaluation (SIEM, EDR, NSM, LLM local, scanners de vulnérabilités, plateformes DFIR/IRP).

## 📂 Structure du repo

```
├── docs/           # Diagrammes d'architecture, workflow, benchmarks
├── scripts/        # Scripts d'intégration Python (API bridges)
└── configs/        # Configurations Filebeat + pipelines Graylog
```

## 🚀 Perspectives

- Intégration CTI via **MISP** pour l'enrichissement automatique par indicateurs de compromission
- Évolution vers un **SOAR** complet avec playbooks (isolation d'hôte, blocage IP automatisé)
- Approche **RAG** pour permettre à l'IA locale d'exploiter les procédures internes de l'entreprise

## 📄 Licence

Ce projet est partagé à titre de portfolio technique. Les scripts sont réutilisables sous licence MIT — voir [`LICENSE`](LICENSE).

---

*Rapport complet de PFE disponible sur demande.*

# 🛡️ SOC Automatisé — Plateforme Open Source pour Infrastructure MSSP

> Conception et déploiement d'une plateforme SOC (Security Operations Center) interne, entièrement basée sur des outils **open source**, pour un fournisseur de services de sécurité managés (MSSP).

📄 **[Rapport technique complet (PDF)](https://github.com/LailaHammouch/Automated-Open-Source-SOC-Platform/blob/main/rapport-technique.pdf)** — déploiement de l'infrastructure, intégration des composants, validation par scénario d'attaque, et annexes (configurations Filebeat, scripts Python d'intégration).

---

## 🏗️ Architecture

La plateforme est organisée en **5 couches fonctionnelles** :

| Couche | Outils | Rôle |
|---|---|---|
| **Collecte** | Wazuh, Zeek | Supervision endpoints + visibilité réseau |
| **Agrégation** | Graylog | Centralisation, normalisation, pipelines de parsing |
| **Détection & Orchestration** | SOCFortress Copilot + Ollama/Llama 3 | Corrélation, enrichissement des alertes par IA locale |
| **Gestion des vulnérabilités** | OpenVAS | Scan CVE, priorisation des correctifs |
| **Réponse & Forensique** | DFIR-IRIS, Velociraptor | Gestion des incidents, collecte d'artefacts en temps réel |

![Architecture globale](https://github.com/LailaHammouch/Automated-Open-Source-SOC-Platform/blob/main/Docs/ArchitecureGlobale.png)

Toutes les briques communiquent via API, avec des scripts Python développés sur-mesure pour combler l'absence de connecteurs natifs (ex : SOCFortress Copilot → IRIS, OpenVAS → IRIS, IRIS → Velociraptor) — voir le [rapport technique](https://github.com/LailaHammouch/Automated-Open-Source-SOC-Platform/blob/main/rapport-technique.pdf), Annexe B, pour le détail de ces scripts.

## ⚙️ Stack technique

`Wazuh` `Zeek` `Graylog` `OpenVAS` `SOCFortress Copilot` `Ollama (Llama 3)` `DFIR-IRIS` `Velociraptor` `Docker` `Filebeat` `Python`

**Choix clé :** remplacement de l'API OpenAI (GPT-4, payante et cloud) par **Ollama + Llama 3 en local**, garantissant confidentialité des données et absence de dépendance externe — critère non négociable en environnement MSSP.

## 🤖 Intégration de l'intelligence artificielle

L'IA n'est pas un gadget ajouté à la marge : elle est intégrée comme **couche d'assistance à l'analyse**, directement dans le flux de traitement des alertes.

**Fonctionnement :**
```
Graylog (alerte corrélée) → SOCFortress Copilot (AI Router)
   → Ollama / Llama 3 (LLM local)
   → Analyse contextuelle + recommandations → Analyste SOC
```

**Ce que l'IA apporte concrètement :**
- **Enrichissement automatique des alertes** : explication en langage naturel du comportement détecté, sans que l'analyste ait à recouper manuellement plusieurs sources
- **Priorisation par criticité** : aide à distinguer rapidement un faux positif d'un incident réel
- **Recommandations de réponse** pour les cas simples et récurrents, tout en laissant les décisions critiques sous le contrôle humain
- **Deux modes d'usage** : mode "chatbot" pour des requêtes générales, et mode "assistant SOC" branché directement sur les logs Graylog pour l'analyse en contexte

**Pourquoi un LLM local plutôt qu'une API cloud :**
| Critère | Ollama / Llama 3 (local) | API cloud (GPT-4) |
|---|---|---|
| Confidentialité des données | ✅ Données jamais transmises à l'extérieur | ❌ Données envoyées à un tiers |
| Coût | ✅ Gratuit / open source | ❌ Facturation à l'usage |
| Latence / dépendance réseau | ✅ Aucune dépendance externe | ❌ Dépend de la disponibilité du service |
| Conformité MSSP | ✅ Adapté aux exigences de confidentialité | ⚠️ Nécessite des garanties contractuelles |

Cette architecture illustre une approche pragmatique de l'IA en cybersécurité : **l'IA assiste, elle ne remplace pas** — elle réduit le temps d'analyse (MTTD/MTTR) sans retirer la décision finale à l'analyste SOC.

## 🔄 Workflow de réponse aux incidents

De la détection à la clôture, chaque incident suit un cycle DFIR structuré et automatisé entre les outils :

![Workflow de réponse aux incidents](https://github.com/LailaHammouch/Automated-Open-Source-SOC-Platform/blob/main/Docs/incident-workflow.png)

## 📂 Structure du repo

```
├── rapport-technique.pdf   # Déploiement, intégrations, validation, configs & scripts (annexes)
├── docs/                   # Diagrammes d'architecture


## 🚀 Perspectives

- Intégration CTI via **MISP** pour l'enrichissement automatique par indicateurs de compromission
- Évolution vers un **SOAR** complet avec playbooks (isolation d'hôte, blocage IP automatisé)
- Approche **RAG** pour permettre à l'IA locale d'exploiter les procédures internes de l'entreprise

## 📄 Licence

Ce projet est partagé à titre de portfolio technique. Les scripts sont réutilisables sous licence MIT — voir [`LICENSE`](LICENSE).

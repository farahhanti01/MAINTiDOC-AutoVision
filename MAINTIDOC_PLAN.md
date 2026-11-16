# MAINTiDOC AutoVision — Plan de développement

**Projet :** Assistant documentaire multimodal et agentique pour la maintenance automobile  
**Équipe :** Farah Hanti et MASBAH Ahmed  
**Durée prévisionnelle :** 6 semaines  
**Statut :** Planification initiale — À valider

---

## 1. Présentation du projet

MAINTiDOC AutoVision est un assistant intelligent destiné à faciliter la maintenance automobile grâce à l'intelligence artificielle générative multimodale.

Le système permettra :

- L'interrogation de manuels automobiles en langage naturel.
- L'analyse de photographies et de voyants de tableau de bord.
- La recherche documentaire multimodale avec RAG.
- La génération de réponses accompagnées de références vérifiables.
- L'exploitation d'un modèle Vision-Language (VLM) open source.
- L'orchestration de workflows agentiques avec LangChain et LangGraph.
- La consultation d'historiques de maintenance via MCP.
- La préparation de tickets avec validation humaine obligatoire.
- Le fine-tuning ciblé d'un reranker multilingue.
- L'évaluation des performances et de la sécurité.
- Le déploiement reproductible avec Docker Compose.

**Périmètre proposé :** documentation automobile Volkswagen et Toyota. Les modèles et années restent à confirmer.

Le système constitue une aide documentaire et ne garantit pas un diagnostic automobile.

---

## 2. Architecture technique

| Composant | Technologie proposée |
|---|---|
| Langage principal | Python |
| Backend | FastAPI |
| Interface | Streamlit |
| Orchestration | LangChain, LangGraph |
| Modèle multimodal | Qwen2.5-VL-7B-Instruct |
| Inférence | Transformers, éventuellement vLLM |
| Base vectorielle | Qdrant |
| Embeddings textuels | BGE-M3 |
| Embeddings visuels | Encodeur multilingue compatible CLIP |
| Extraction documentaire | PyMuPDF, OCR |
| Base de données | PostgreSQL |
| Stockage documentaire | MinIO |
| Communication avec les outils | MCP |
| Fine-tuning | Transformers |
| Suivi des expériences | MLflow |
| Conteneurisation | Docker Compose |
| Intégration continue | GitHub Actions |

Les versions, licences et compatibilités seront validées avant leur adoption définitive.

---

## 3. Organisation des sprints

### Sprint 1 — Cadrage, architecture et préparation du corpus

**Responsables :**
- Farah : architecture et organisation GitHub.
- Ahmed : collecte documentaire et métadonnées.

**Objectif :** définir le périmètre du projet et préparer les fondations techniques.

**Tâches :**
- [ ] Confirmer les constructeurs, modèles et années.
- [ ] Analyser et formaliser les exigences du cahier des charges.
- [ ] Définir l'architecture globale.
- [ ] Préparer la structure GitHub.
- [ ] Définir les conventions de développement.
- [ ] Identifier les sources documentaires officielles.
- [ ] Vérifier les licences et autorisations.
- [ ] Préparer le manifeste du corpus.
- [ ] Identifier les ressources GPU disponibles.
- [ ] Établir le registre des risques.

**Technologies :** Git, GitHub, Python, Docker, Mermaid.

**Fichiers concernés :** `README.md`, `MAINTIDOC_PLAN.md`, `docs/ARCHITECTURE.md`, `docs/DATASET.md`, `CONTRIBUTING.md`.

**Livrables :** architecture V1, inventaire documentaire, dépôt structuré et plan de développement.

**Critères d'acceptation :** périmètre documenté, architecture revue et sources autorisées identifiées.

**Tests :** vérification des liens, des licences, des métadonnées et de la structure Git.

**Dépendances :** validation du périmètre et disponibilité des sources.

**Statut :** À vérifier.

---

### Sprint 2 — Ingestion et recherche documentaire multimodale

**Responsables :**
- Ahmed : extraction, OCR et indexation.
- Farah : recherche hybride et contrôle d'accès.

**Objectif :** construire un pipeline permettant d'exploiter les manuels automobiles.

**Tâches :**
- [ ] Développer l'extraction de texte PDF avec PyMuPDF.
- [ ] Extraire les images, figures et tableaux.
- [ ] Intégrer l'OCR pour les documents scannés.
- [ ] Découper les documents en passages.
- [ ] Conserver les identifiants, versions, pages et figures.
- [ ] Générer les embeddings textuels avec BGE-M3.
- [ ] Générer les embeddings visuels.
- [ ] Configurer Qdrant.
- [ ] Implémenter la recherche textuelle et visuelle.
- [ ] Fusionner les résultats avec Reciprocal Rank Fusion.
- [ ] Appliquer les filtres d'accès documentaires.

**Technologies :** PyMuPDF, OCR, BGE-M3, CLIP, Qdrant, LangChain.

**Fichiers concernés :** `ingestion/`, `rag/`, `data/`, `tests/`.

**Livrables :** pipeline d'ingestion et moteur de recherche multimodal.

**Critères d'acceptation :** les éléments récupérés possèdent des références vérifiables et respectent les permissions.

**Tests :** extraction de PDF natifs et scannés, recherche textuelle, recherche visuelle et refus d'accès.

**Dépendances :** corpus préparé, métadonnées définies et base vectorielle disponible.

**Statut :** Non commencé — à confirmer.

---

### Sprint 3 — Intégration du VLM, API et interface

**Responsables :**
- Farah : modèle VLM, prompts et génération.
- Ahmed : FastAPI et Streamlit.

**Objectif :** permettre à l'utilisateur de poser une question avec une photographie et d'obtenir une réponse sourcée.

**Tâches :**
- [ ] Tester Qwen2.5-VL-7B-Instruct sur le matériel disponible.
- [ ] Développer l'analyse des photographies.
- [ ] Connecter le VLM au moteur RAG.
- [ ] Créer les prompts YAML versionnés.
- [ ] Définir les schémas de réponse avec Pydantic.
- [ ] Implémenter la vérification des références.
- [ ] Gérer les réponses incertaines et les clarifications.
- [ ] Développer les endpoints FastAPI.
- [ ] Créer l'interface Streamlit.
- [ ] Afficher les documents, pages et figures cités.

**Technologies :** Qwen2.5-VL, Transformers, FastAPI, Streamlit, Pydantic.

**Fichiers concernés :** `vision/`, `backend/`, `frontend/`, `prompts/`.

**Livrables :** première version fonctionnelle de l'assistant multimodal.

**Critères d'acceptation :** réponses structurées, citations vérifiables et clarification lorsque les preuves sont insuffisantes.

**Tests :** questions textuelles, images ambiguës, sources absentes et sorties invalides.

**Dépendances :** recherche documentaire opérationnelle et accès au VLM.

**Statut :** Non commencé — à confirmer.

---

### Sprint 4 — Orchestration agentique et intégration MCP

**Responsables :**
- Farah : workflows LangGraph.
- Ahmed : serveur MCP et gestion des tickets.

**Objectif :** intégrer les agents et les interactions contrôlées avec un historique de maintenance fictif.

**Tâches :**
- [ ] Définir les états et transitions LangGraph.
- [ ] Implémenter le routage des demandes.
- [ ] Ajouter les limites d'appels et de traitement.
- [ ] Développer le serveur MCP.
- [ ] Implémenter `get_equipment_history`.
- [ ] Implémenter `get_open_tickets`.
- [ ] Implémenter `create_ticket`.
- [ ] Préparer des historiques de maintenance fictifs.
- [ ] Implémenter la préparation des tickets.
- [ ] Ajouter l'approbation humaine explicite.
- [ ] Associer l'approbation à l'utilisateur et au contenu exact.
- [ ] Empêcher les créations de tickets en double.
- [ ] Vérifier les autorisations côté serveur MCP.

**Technologies :** LangGraph, LangChain, MCP, PostgreSQL.

**Fichiers concernés :** `agents/`, `mcp_server/`, `backend/`, `tests/security/`.

**Livrables :** workflow agentique et serveur MCP fonctionnels.

**Critères d'acceptation :** aucun ticket créé sans approbation, permissions vérifiées et limites d'appels respectées.

**Tests :** approbation, refus, doublons, dépassement de quotas et modification du ticket.

**Dépendances :** API, schémas et composants RAG/VLM disponibles.

**Statut :** Non commencé — à confirmer.

---

### Sprint 5 — Fine-tuning, évaluation et sécurité

**Responsables :**
- Ahmed : adaptation du reranker et MLflow.
- Farah : évaluation et sécurité.

**Objectif :** mesurer les performances, les limites et l'apport du fine-tuning.

**Tâches :**
- [ ] Préparer 800 à 1 500 exemples vérifiés, selon la cible du cahier des charges.
- [ ] Construire les triplets question, passage pertinent et négatif difficile.
- [ ] Séparer entraînement, validation et test par document ou famille.
- [ ] Entraîner un petit reranker multilingue.
- [ ] Suivre les expériences avec MLflow.
- [ ] Comparer la recherche sans reranker, avec reranker original et adapté.
- [ ] Mesurer Recall@5, nDCG@5 et MRR@10.
- [ ] Évaluer la fidélité des réponses et des citations.
- [ ] Comparer les résultats multimodaux à une version texte seul.
- [ ] Préparer environ 150 scénarios de test.
- [ ] Tester les injections dans les documents, OCR et réponses MCP.
- [ ] Tester les refus d'accès et les actions non approuvées.
- [ ] Mesurer les latences et les ressources consommées.

**Technologies :** Transformers, MLflow, pytest et outils d'évaluation.

**Fichiers concernés :** `training/`, `evaluation/`, `tests/`, `docs/TESTING.md`.

**Livrables :** reranker adapté, rapport comparatif, résultats de tests et analyse des limites.

**Critères d'acceptation :** comparaison reproductible des trois configurations et documentation des résultats, y compris en l'absence d'amélioration.

**Tests :** métriques de recherche, citations, sécurité, incertitude et performance.

**Dépendances :** corpus annoté, pipeline fonctionnel et ressources de calcul.

**Statut :** Non commencé — à confirmer.

---

### Sprint 6 — Déploiement, documentation et soutenance

**Responsables :**
- Ahmed : Docker Compose, CI/CD et monitoring.
- Farah : documentation, évaluation finale et démonstration.

**Objectif :** livrer une application reproductible et préparer la soutenance.

**Tâches :**
- [ ] Finaliser les Dockerfiles.
- [ ] Configurer Docker Compose.
- [ ] Préparer les volumes persistants.
- [ ] Automatiser les tests avec GitHub Actions.
- [ ] Vérifier les dépendances et les secrets.
- [ ] Ajouter les healthchecks.
- [ ] Suivre les erreurs, latences et consommations.
- [ ] Préparer une procédure de retour arrière.
- [ ] Finaliser le README et la documentation.
- [ ] Rédiger la fiche des limites.
- [ ] Présenter les résultats expérimentaux.
- [ ] Préparer les scénarios de démonstration.
- [ ] Réaliser une répétition complète de soutenance.

**Technologies :** Docker, Docker Compose, GitHub Actions, MLflow.

**Fichiers concernés :** `infra/`, `.github/workflows/`, `docker-compose.yml`, `README.md`, `docs/`.

**Livrables :** prototype déployable, documentation, fiche de limites et démonstration.

**Critères d'acceptation :** démarrage reproductible, tests exécutables et scénarios de soutenance démontrables.

**Tests :** installation propre, tests de bout en bout, refus d'accès, injection et approbation humaine.

**Dépendances :** intégration des modules précédents.

**Statut :** Non commencé — à confirmer.

---

## 4. Exigences de sécurité

Les limites initiales prévues par le cahier des charges sont :

- Maximum 3 appels MCP par demande.
- Maximum 6 appels au modèle par demande.
- Maximum 60 secondes de traitement automatique, hors attente humaine.
- Limites de taille des images, du contexte et des tokens.
- Quotas par utilisateur et plafond de consommation GPU.
- Contrôle des permissions avant récupération et affichage.
- Approbation explicite avant création d'un ticket.
- Protection des secrets et anonymisation des données.
- Protection contre les injections de prompt.

Ces limites devront être appliquées dans le code et vérifiées expérimentalement.

---

## 5. Objectifs d'évaluation

Les objectifs suivants proviennent du cahier des charges et ne constituent pas des résultats acquis :

- Au moins 90 % de citations soutenant les affirmations associées.
- Aucune création de ticket sans approbation dans les tests.
- Aucune fuite documentaire entre groupes dans les tests.
- Latence p95 inférieure à 30 secondes pour les demandes simples sur le matériel retenu.

Les résultats réellement mesurés seront documentés séparément.

---

## 6. Organisation GitHub

### Branches

- `main` : version principale protégée.
- `feat/...` : nouvelles fonctionnalités.
- `fix/...` : corrections.
- `docs/...` : documentation.
- `test/...` : tests.

### Commits

Convention Conventional Commits :

- `feat(rag): ajout de la recherche hybride`
- `fix(mcp): correction de l'idempotence`
- `docs: mise à jour du plan`
- `test(security): ajout des tests d'autorisation`

### Suivi des tâches

Chaque tâche GitHub Issue doit préciser :

- Le responsable.
- Le sprint.
- Le module.
- Les exigences concernées.
- Les dépendances.
- Les critères d'acceptation.
- Les tests de validation.

### Definition of Done

Une tâche est terminée lorsque :

- [ ] Le développement est réalisé.
- [ ] Les tests applicables passent.
- [ ] Les exigences de sécurité sont respectées.
- [ ] La documentation est mise à jour.
- [ ] La Pull Request est revue.
- [ ] Le code est intégré dans `main`.

---

## 7. Suivi de l'avancement

Les statuts GitHub Projects sont :

1. Backlog
2. Ready
3. In Progress
4. In Review
5. Done

L'avancement est déterminé à partir des tâches réellement validées, et non simplement du code écrit.

**Remarque :** la répartition et les choix techniques de ce document restent proposés jusqu'à validation par les deux membres de l'équipe.
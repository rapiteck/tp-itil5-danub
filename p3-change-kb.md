# TP ITIL 5 — Partie 3 : Traitement du changement (Change Enablement + Knowledge Management + Product and Service Lifecycle)

## 1. RFC — Request For Change

**Amélioration traitée :** ID de ticket unique + statut consultable en libre-service (amélioration n°1 du CSI Register, Partie 1)

### Identification

| Champ | Valeur |
|---|---|
| Référence | RFC-2026-001 |
| Titre | Mise en place d'un ID de ticket unique avec notification automatique et page de statut en libre-service |
| Demandeur | Responsable helpdesk |
| Date de soumission | 2026-09-14 |

### Type de changement : **Normal**

**Justification :** ce changement n'est pas *standard* car il n'a jamais été appliqué auparavant sur ce système (pas de procédure pré-approuvée existante) : il nécessite une évaluation. Il n'est pas non plus *urgent* car aucune panne en cours ne l'impose — il s'agit d'une amélioration planifiée, avec le temps nécessaire pour une évaluation normale par le CAB avant mise en œuvre.

### Description du changement

Configuration de l'outil de ticketing pour :
1. Générer un identifiant unique à la création de chaque ticket
2. Envoyer automatiquement cet identifiant par email à l'utilisateur dès la création
3. Exposer une page de statut en libre-service (lecture seule) accessible via cet identifiant, sans authentification complexe

### Analyse d'impact

**Qui est affecté :**
- Tous les utilisateurs internes créant des tickets (impact positif direct : visibilité sur le suivi)
- Les agents du helpdesk (changement de processus : doivent garantir que chaque ticket reste bien rattaché à son ID unique lors des transferts entre niveaux de support)
- L'infrastructure email (volume de notifications automatiques supplémentaire à gérer)

**Risque de régression :**
- Risque que la génération d'ID unique entre en conflit avec d'éventuels tickets déjà créés manuellement hors système (doublons lors de la migration)
- Risque de surcharge de la file d'envoi d'emails si le volume de tickets est élevé au moment du déploiement

### Plan de rollback

1. Conserver une sauvegarde de la configuration de l'outil de ticketing avant modification (export de la configuration actuelle, horodaté)
2. Si les notifications automatiques échouent en masse ou si la page de statut expose des données incorrectes : désactiver la fonctionnalité de notification automatique et la page de statut via le flag de configuration prévu à cet effet (pas de suppression de code, juste désactivation)
3. Revenir à la configuration sauvegardée à l'étape 1
4. Notifier les agents helpdesk du retour temporaire au processus manuel de communication de statut par téléphone/email individuel, le temps de corriger le problème
5. Délai de rollback estimé : moins de 15 minutes (changement de configuration, pas de migration de données irréversible)

### Validation CAB (Change Advisory Board)

**Argumentation du demandeur (responsable helpdesk) :**
« Ce changement répond directement au symptôme le plus critique remonté par les utilisateurs : les tickets perçus comme perdus faute de suivi. L'effort de mise en œuvre est faible (paramétrage de l'outil existant, pas de nouveau système), et le risque est limité car réversible en moins de 15 minutes. Je demande une fenêtre de déploiement en dehors des heures de pointe pour limiter l'impact du risque de surcharge email. »

**Argumentation de l'approbateur (représentant CAB) :**
« Le changement est approuvé sous réserve de deux conditions : premièrement, réaliser un test de charge des notifications email sur un sous-ensemble de tickets avant déploiement complet, pour valider l'absence de surcharge ; deuxièmement, prévoir une communication préalable aux agents helpdesk sur le nouveau processus de transfert de ticket, pour éviter que l'ID unique ne soit pas correctement conservé lors des transferts entre niveaux N1/N2. Le plan de rollback proposé est jugé suffisant compte tenu du risque limité. Changement approuvé pour la prochaine fenêtre de déploiement standard. »

**Décision : Approuvé**

---

## 2. Article de base de connaissance (Knowledge Management)

### Symptôme
Un utilisateur signale qu'il ne reçoit aucune confirmation après avoir soumis un ticket via le formulaire de contact, et ne sait pas si sa demande a bien été prise en compte.

### Cause
L'adresse email de l'utilisateur a été mal formatée à la création du ticket (espace parasite ou domaine mal orthographié), ce qui empêche l'envoi de la notification automatique contenant l'ID unique du ticket.

### Résolution
1. Rechercher le ticket dans l'outil par le nom de l'utilisateur ou l'horodatage approximatif de sa demande (le ticket existe bien côté système même sans notification reçue)
2. Vérifier et corriger le champ email dans la fiche du ticket
3. Déclencher manuellement un renvoi de la notification automatique depuis l'interface de l'outil (bouton "renvoyer la confirmation")
4. Confirmer par téléphone ou en direct que l'utilisateur a bien reçu son ID de ticket

### Mots-clés
`notification`, `email non reçu`, `ID ticket`, `confirmation manquante`, `formulaire de contact`

---

## 3. Positionnement dans le Product and Service Lifecycle

### Étapes mobilisées

Ce changement mobilise principalement deux étapes :

- **Build** : c'est l'étape centrale, puisqu'il s'agit de configurer/développer la fonctionnalité de génération d'ID unique et de notification automatique sur l'outil de ticketing existant — on construit une capacité qui n'existait pas encore techniquement.
- **Transition** : une fois la fonctionnalité construite, elle doit être déployée en production de façon contrôlée (via la RFC et sa validation CAB ci-dessus), avec test préalable, fenêtre de déploiement et plan de rollback — c'est exactement le rôle de l'étape Transition.

L'étape **Design** n'est pas mobilisée de façon significative ici : le workflow global de traitement d'un ticket n'est pas repensé à ce stade (cela relève plutôt de l'amélioration n°2 du CSI Register, qui elle nécessiterait de repenser les rôles et le flux — donc Design + Build + Transition).

### Pourquoi ce n'est pas un enchaînement strictement linéaire

Le Product and Service Lifecycle n'impose pas de parcourir les 8 étapes dans l'ordre à chaque changement, car tous les changements ne partent pas du même point du cycle de vie du service. Dans notre cas concret :

- On saute directement à **Build** sans repasser par Discover/Design/Acquire, car le service (le helpdesk) existe déjà et l'outil de ticketing est déjà en place : il ne s'agit pas de créer un nouveau service mais d'enrichir un service existant.
- **Operate**, **Deliver** et **Support** continuent de fonctionner en parallèle pendant que Build et Transition ont lieu sur ce changement précis — le helpdesk continue à traiter les tickets courants pendant qu'on développe et teste la nouvelle fonctionnalité, ces étapes ne s'arrêtent pas pour attendre la fin du changement.
- Si le test de charge demandé par le CAB révèle un problème, on pourrait revenir ponctuellement de Transition vers Build pour ajuster la configuration, ce qui illustre bien que les étapes se chevauchent et peuvent être reparcourues plutôt que suivies une seule fois dans un ordre figé.


# TP ITIL 5 — Partie 4 : Traitement de la demande de service (Service Request Management)

## Demande de service liée au changement (RFC-2026-001)

Cette demande découle directement du changement traité en Partie 3 : une fois la fonctionnalité de notification/statut approuvée par le CAB, sa mise en production planifiée est traitée comme une **demande de service**, et non comme un Incident, car il s'agit d'une action prévue et validée, pas d'une panne.

| Champ | Valeur |
|---|---|
| **Titre** | Activer la notification automatique et la page de statut de ticket sur l'outil helpdesk |
| **Description** | Suite à la validation CAB de la RFC-2026-001, activer en production la génération d'ID de ticket unique, l'envoi automatique de cet ID par email à la création du ticket, et la page de statut en libre-service associée. Déploiement à réaliser en dehors des heures de pointe, avec test de charge préalable sur un sous-ensemble de tickets conformément à la condition posée par le CAB. |
| **Type** | Demande de service (Service Request) |
| **Statut** | Planifiée |
| **Priorité** | Normale |
| **Demandeur** | Responsable helpdesk |
| **Assigné à** | Équipe technique helpdesk |
| **Date de création** | 2026-09-15 |
| **Date d'échéance souhaitée** | 2026-09-22 |
| **Date de clôture** | (à renseigner à la mise en production effective) |

### Pourquoi Service Request Management et pas Incident Management

Un Incident correspond à une interruption ou dégradation non planifiée d'un service. Ici, rien n'est cassé : il s'agit d'une action planifiée, déjà approuvée par le CAB via la RFC, avec une date cible et des conditions connues à l'avance (test de charge, fenêtre de déploiement). C'est exactement la distinction que fait ITIL 5 entre les deux pratiques — la demande suit un chemin standard et prévisible, alors qu'un Incident suit un chemin de restauration d'urgence.

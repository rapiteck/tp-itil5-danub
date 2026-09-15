# TP ITIL 5 — Amélioration du service Helpdesk interne

## Synthèse du cas

Le helpdesk interne souffrait de trois symptômes récurrents : lenteur de traitement, tickets perdus, utilisateurs rappelant plusieurs fois pour le même problème. Ce repo documente le diagnostic, le pilotage, la mise en œuvre contrôlée et la clôture d'une amélioration ciblée (ID de ticket unique + notification automatique + statut en libre-service), en appliquant le framework ITIL 5.

## Tableau récapitulatif : partie → pratique(s) ITIL 5 mobilisée(s)

| Partie | Livrable | Pratique(s) ITIL 5 mobilisée(s) |
|---|---|---|
| 1 | `p1-csi-register.md` | Continual Improvement (4 dimensions + CSI Register) |
| 2 | `p2-slm-events.md` | Service Level Management, Event Management |
| 3 | `p3-change-kb.md` | Change Enablement, Knowledge Management, Product and Service Lifecycle (PSLM) |
| 4 | `p4-service-request.md` | Service Request Management |

## Principe directeur le plus structurant

**« Progresser de manière itérative avec du feedback »**

Ce principe a guidé l'ensemble du cas, pas seulement la priorisation initiale du CSI Register (Partie 1). Exemple concret tiré du TP : en Partie 3, le CAB n'a pas simplement approuvé ou rejeté la RFC-2026-001 en bloc — il a conditionné son approbation à un test de charge préalable sur un sous-ensemble de tickets avant le déploiement complet. C'est une application directe du principe : on ne déploie pas la fonctionnalité complète d'un coup sur l'ensemble des utilisateurs, on valide d'abord un incrément mesurable (le test de charge) avant d'engager le reste. Le plan de rollback en moins de 15 minutes (Partie 3) sert le même objectif : permettre d'itérer sans risque disproportionné en cas de signal négatif.

## Point critique : apport du module AI Governance et du modèle 6C sur ce cas précis

ITIL 5 introduit un module AI Governance et un modèle **6C** (Creation, Curation, Clarification, Cognition, Communication, Coordination) pour catégoriser les capacités IA dans la dimension Information & Technologie. Voici une évaluation ciblée sur *ce cas précis*, pas une généralité.

### Ce qui n'est pas concerné

La fonctionnalité traitée dans ce TP — génération d'un ID de ticket unique et envoi d'une notification automatique — est de l'**automatisation déterministe**, pas de l'IA : il n'y a aucune capacité de type Cognition (prédiction, classification, prise de décision autonome) impliquée. Appliquer le module AI Governance à cette fonctionnalité précise serait donc hors sujet : il n'y a pas de risque de biais, de dérive de modèle ou de décision opaque à gouverner, puisqu'il n'y a pas de modèle du tout. Une réponse qui plaquerait l'AI Governance sur cette RFC spécifique serait injustifiée.

### Où le modèle 6C aurait un apport réel, sur un cas adjacent

En revanche, l'amélioration n°3 du CSI Register (Partie 1 — SLA par catégorie et dashboard de charge avec alertes) ouvre une piste légitime : si l'on voulait automatiser la **priorisation initiale d'un ticket** à partir de sa description en texte libre (plutôt que de laisser l'agent la déterminer manuellement, source potentielle de la lenteur constatée), on mobiliserait la capacité **Cognition** du modèle 6C (classification/prédiction). Dans ce cas précis, le module AI Governance aurait un apport concret et non théorique :

- **Contrôle humain sur les tickets critiques (P1)** : la classification automatique ne devrait jamais déclasser ou reclasser un ticket P1 sans validation humaine explicite, pour éviter qu'une erreur de modèle ne retarde un incident critique — c'est directement la question de "qui approuve la décision de l'IA" posée par le framework.
- **Test de biais avant mise en production** : vérifier que la classification automatique ne défavorise pas systématiquement certains types de demandes (ex: tickets rédigés de façon moins détaillée par certains utilisateurs) par rapport à d'autres, ce qui recréerait artificiellement le symptôme de lenteur qu'on cherche à corriger.
- **Traçabilité de la décision** : journaliser pourquoi un ticket a été classé à telle priorité, pour permettre un audit a posteriori en cas de contestation par un utilisateur.

### Conclusion du point critique

L'apport du module AI Governance et du modèle 6C est **nul sur le changement effectivement livré dans ce TP** (automatisation simple, sans IA), mais **réel et identifiable sur une extension plausible du même CSI Register** (priorisation automatique par classification de texte). Ce TP illustre donc bien pourquoi ITIL 5 traite l'AI Governance comme un module à mobiliser au cas par cas selon la nature technique du changement, et non comme une couche à appliquer uniformément à toute amélioration de service.

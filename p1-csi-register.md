# TP ITIL 5 — Partie 1 : Diagnostic (4 dimensions + Continual Improvement)

**Service :** Helpdesk interne
**Symptômes :** lenteur de traitement, tickets perdus, rappels multiples des utilisateurs.

## 1. Diagnostic par les 4 dimensions

**Organisations & personnes :** les rappels répétés montrent qu'aucun agent n'est propriétaire unique du ticket jusqu'à sa clôture — le contexte se perd entre intervenants.

**Information & technologie :** les « tickets perdus » indiquent un outil sans cycle de vie strict (pas d'ID unique, pas de traçabilité fiable entre les canaux de saisie).

**Partenaires & fournisseurs :** aucune dépendance externe n'est mentionnée dans les symptômes — dimension à vérifier en partie 2, non concluante à ce stade.

**Value Streams & processus :** la combinaison des trois symptômes dessine un flux sans étapes standardisées ni points de contrôle (pas de SLA, pas de règles de transfert N1/N2).

## 2. CSI Register

| # | Amélioration | Effort | Impact | Priorité |
|---|---|---|---|---|
| 1 | ID de ticket unique + statut en libre-service (notification auto) | Faible | Fort | 1 |
| 2 | Value stream formalisé + propriétaire unique du ticket jusqu'à résolution | Moyen | Fort | 2 |
| 3 | SLA par catégorie + dashboard de charge avec alertes | Fort | Moyen | 3 |

**Justification :** l'action 1 règle le symptôme le plus visible à faible coût ; l'action 2 traite la cause structurelle des rappels mais coûte plus cher en organisation ; l'action 3 n'a de sens qu'une fois le flux stabilisé par les deux premières.

## 3. Principe directeur mobilisé

**« Progresser de manière itérative avec du feedback »** — chaque amélioration est un incrément testable avant d'investir dans la suivante, plutôt qu'une refonte globale risquée et difficile à diagnostiquer en cas d'échec.

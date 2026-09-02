# 🎬 Content Pipeline — Spec d'orchestration

**Système multi-agents de production de contenu pour Claire Mei Nakamura.**
Orchestrateur : `main` (HoussamClaw). Sources de vérité : GitHub, Notion, Google Drive, Discord.

---

## 1. Les agents

| Agent | Rôle | Skills (dossier) | Sort vers |
|---|---|---|---|
| **main** | Orchestrateur + supervision + alertes Houssam | — | tous |
| **scout** 🔎 | Repère/sélectionne les vidéos virales des comptes surveillés | `skills-agents/scout` | scripting |
| **scripting** ✍️ | Réécrit la vidéo en script adapté à Claire | `skills-agents/scripting` | image |
| **image** 🖼️ | Prompt + génération images de référence | `skills-agents/image` | video |
| **video** 🎬 | Prompt + génération vidéo | `skills-agents/video` | montage-son |
| **montage-son** ✂️ | Montage + sonorisation + sous-titres | `skills-agents/montage-son` | quality-review |
| **veille-idees** 🔍 | Veille YouTube/Reddit → idées originales | `skills-agents/veille-idees` | scout/scripting |
| **quality-review** 🧐 | Note chaque étape, gate de validation | `skills-agents/quality-review` | ✅ Prêt |

## 2. Le flux

```
Houssam poste un STYLE / COMPTE inspiration
        ↓ (canal #inspirations Discord)
main crée la tâche Notion (statut 📥 Idée)
        ↓
scout → scripting → image → video → montage-son → quality-review
        ↓                    (chaque livrable → GitHub + Drive)
Notion : mise à jour du statut à chaque étape
        ↓
quality-review : score 0-10 → ✅ Prêt (ou retour à l'agent fautif)
        ↓
Houssam valide → 📣 Publié
```

## 3. Traçabilité dans Notion

Base de données : `🎬 Content Pipeline — Tâches réseaux sociaux`
(`da95a09f-54d9-46b5-a856-0485dd38da50`)

Colonnes : Nom, Plateforme, Statut, Agent assigné, Score review, Lien GitHub, Lien Drive, Compte inspiration.

Statuts : 📥 Idée → 🔎 Scouté → ✍️ Script → 🖼️ Refs image → 🎬 Vidéo → ✂️ Montage → 🧐 Review → ✅ Prêt → 🚀 Publié.

## 4. Sources de vérité

- **GitHub** `houssammvpempire/Claire-Mei-Nakamura` : docs identité, briefs, scripts, idées, reviews, comptes surveillés.
- **Google Drive** `Avatar 1 : Claire Mei Nakamura` (`1hIHumBPSHAf-XMJm3awNy0h8fXzjJ_Jm`) : médiathèque + production.
- **Notion** : le tableau de bord visuel de progression.

## 5. Alertes orchestrateur

`main` avertit Houssam **uniquement** quand un blocage est réservé à lui :
- décision éditoriale / validation finale
- choix de style / direction
- problème technique que seul lui peut dénouer (permissions, comptes, modifications manuelles)

## 6. Diffusion Discord (prévue)

Chaque agent a un canal dédié. Le bot unique lit/écrit dans tous. Le flux se suit en direct dans #supervision.

---

## Blocages restants (à faire par Houssam)

1. **Canaux Discord** : l'admin Discord exige une action venant d'un contexte Discord.
   → Houssam envoie un message dans un canal du serveur (ex. #avatar-1) : « crée la catégorie Content Pipeline et les canaux des agents ».
2. **Google Drive (écriture)** : le service account `claire-bot@...` est en **lecture seule** sur `Avatar 1`.
   → Houssam partage le dossier en **Éditeur** (pas Lecteur) avec l'email `claire-bot@claire-mei-nakamura.iam.gserviceaccount.com`.

## Pour lancer un pipeline de test

1. Houssam donne un **compte inspiration** ou un **style**.
2. `main` crée une tâche Notion (statut 📥 Idée).
3. `main` déclenche `scout` → puis enchaîne les agents en séquence.
4. Chaque livrable est déposé (GitHub/Discord) et Notion est mis à jour.
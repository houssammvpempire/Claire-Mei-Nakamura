# 🔁 Pipeline de production Claire Mei Nakamura

Pipeline de bout en bout : de la Bible de marque à l'asset prêt à publier.
Backend de génération : **OpenRouter** (API Houssam) — pas Higgsfield.

## Vue d'ensemble

```
[0 Brief] → [1 Prompt EN] → [2 Script TTS] → [3 Génération] → [4 Quality review] → [5 Assets]
   Bible         identité        voix/parole       image+video     checklist        livrables
   identité      bloc stable      sous-titres       backend API     homogénéité      dossier :)
```

Chaque étape a un livrable écrit dans ce repo → traçable, reproductible, versionné.

---

## Étape 0 — Brief 📋
- **Source de vérité** : `../01-identite/*` + la Bible de marque (canon).
- **Sortie** : un brief par asset dans `../02-contenu/briefs/`.
- ⚠️ Le canon est riche → **extraire un bloc « identité » compact et stable** à recoller tel quel dans chaque prompt, sinon la consistance du visage se dilue.

## Étape 1 — Prompt (EN) 🇬🇧
- **Toujours en anglais** (décision verrouillée).
- Le prompt contient : contexte caméra + décor canonique + **visage identité** + tenue signature + **les phrases exactes du script** + lip-sync + durée.
- Anti-corpo : énergie authentique, micro-expressions, « pas de ton corporate ».

## Étape 2 — Script → TTS / sous-titres 🎙️
- Script approuvé par Houssam → `../02-contenu/scripts/`.
- → TTS (voix Claire) + `../02-contenu/sous-titres/` (timestamps).
- → `../02-contenu/legendes/` (caption plateforme).

## Étape 3 — Génération 🖼️🎬
- Backend : **API OpenRouter** (compte Houssam).
- Image puis video (ou image-to-video).
- ⚠️ OpenRouter sert des modèles **vision/chat + image** — **pas de génération vidéo native** :
  la branche vidéo devra soit un modèle vidéo OpenRouter si dispo, soit un service vidéo dédié branché sur la même sortie (à confirmer — voir blocage).

## Étape 4 — Quality review ✅
- Sortie en `../quality-review/` (dossier racine workspace).
- Checklist : visage identique / tenue / décor canonique / lip-sync / grain authentique / aucune ride ratée (66 ans assumée, jamais 30/40).
- Toute sortie qui dévie → re-prompt étape 1 avec retour correctif ciblé.

## Étape 5 — Assets 📦
- Rangement dans `../02-contenu/` + médiathèque (Drive).
- Intégration `../03-marketing/calendrier-editorial.md`.

---

## 🔌 Blocage actuel (à trancher)
1. **Génération vidéo** : OpenRouter ne génère pas de vidéo. Il faut soit un modèle vidéo OpenRouter (à vérifier), soit rebrancher un service dédié (Higgsfield si crédits/illimité débloqués).
2. Le reste de la pipeline (brief + prompt + script + QA + assets) est prêt et indépendant du backend.
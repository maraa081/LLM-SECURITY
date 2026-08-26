# Cible locale : Qwen 2.5 7B (LM Studio / Lemonade Server)

Cible de test principale du repo : **Qwen 2.5 7B Instruct**, servie en local
sur la machine de Maraa via **LM Studio** (Lemonade Server).

## Pourquoi cette cible

- 7B : taille raisonnable pour une RX 7800 XT (16 Go) avec une bonne
  vitesse d'inference
- Instruct : modèle aligné (refuse normalement les demandes dangereuses)
  -> les jailbreaks ont un sens
- Bon tool calling et bon français : utile pour les tests d'injection
  via outils et les prompts réalistes
- Local : on contrôle tout (comme le CNN from scratch) et on ne dépend
  d'aucune API payante

## Endpoint

```
Base URL : http://localhost:1234/v1
API      : OpenAI-compatible (chat/completions)
```

Alternatives si besoin :
- Ollama : http://localhost:11434/v1
- llama.cpp server (backend Vulkan) : http://localhost:8080/v1
  (nécessaire plus tard pour les attaques token-level type GCG qui
  exigent les logits bruts, qu'Ollama et LM Studio n'exposent pas)

## Vérifier que la cible répond

```bash
curl http://localhost:1234/v1/models
```

Doit lister le modèle chargé. Si le champ `model` est rejeté par l'API,
prendre l'identifiant exact affiché dans LM Studio (menu du modèle chargé).

## Lancer les tests

```bash
python3 attacks/injection/basic_injection.py --model qwen2.5-7b-instruct
```

## Modèle de menace (cible locale)

| Élément | Valeur |
|---|---|
| Accès à l'API | Complet (local, aucune restriction) |
| Accès aux poids / logits | Non via l'API (on les aura avec llama.cpp plus tard) |
| Objectif type de l'attaquant | Faire révéler une information protégée, détourner le comportement, déclencher une action |
| Contrainte | L'attaque doit tenir dans le prompt (pas de modification du système) |

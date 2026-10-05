# Multi-Agent-Lab

**Que vaut vraiment chaque brique d'un agent LLM ?** Harnais, outils, contexte, consignes, skills :
chacun promet d'améliorer l'agent. Ce laboratoire le mesure face à une baseline, sur les mêmes tâches et
les mêmes graines, en 100 % local (RTX 5090, Ollama), et publie aussi ce qui ne marche pas.

> **Vitrine.** Le code, les données et les bundles de preuve sont dans un dépôt privé : le protocole
> repose sur des tâches d'évaluation scellées qui ne doivent jamais fuiter. Accès sur demande, pour un
> entretien technique par exemple : [cyril.bgs.dev@gmail.com](mailto:cyril.bgs.dev@gmail.com?subject=Multi-Agent-Lab%20%3A%20demande%20d'acc%C3%A8s)

Le nom désigne la destination, pas l'étape : le programme actuel est volontairement **mono-agent**. Le
multi-agent n'est autorisé qu'après un bénéfice mono-agent démontré.

## Quatre règles qui font la différence

- **Préenregistrement.** Tâches, métriques, seuils et analyse sont écrits, hachés et commités avant toute
  mesure comparative.
- **Analyse appariée.** Même tâches, mêmes graines ; la conclusion porte sur les écarts. Un écart non résolu
  reste non résolu, même quand il est flatteur.
- **Anti-fuite.** 40 tâches privées scellées (seules leurs empreintes sont versionnées), canaris mesurés,
  jeu final illisible pour tout composant d'optimisation.
- **Preuve d'abord.** Chaque objectif clos produit un bundle de preuve rejouable (plus de 130 à ce jour).
  Contenu externe (papiers, dépôts, skills) traité comme donnée, jamais exécuté sans qualification.

## Ce qui a été trouvé

| Sujet | Résultat |
|---|---|
| **Choix du skill** | Une décision typée du modèle bat nettement un routeur classique : **84 bonnes décisions sur 107** avec Qwen (63 avec Mistral), contre 41 pour le routeur et 33 sans skill. Écart résolu pour les deux modèles. |
| **Consignes** | **Résultat négatif.** Sept formats mesurés, aucun ne bat la baseline. TextGrad et GEPA reproduits : GEPA affiche +15 points en confirmation, non résolu statistiquement. |
| **Harnais** | Édition structurée de code 16/18, contre 8/18 pour le patch unifié. Le résumé garde toute l'information avec 16 % de jetons en moins, la fenêtre glissante la perd. Sur ces tâches, la boucle minimale suffit. |
| **Sécurité des outils** | Fuzzing des schémas d'appel : 47 appels invalides atteignaient un outil, 0 après durcissement. Scanner de skills : 9 attaques sur 13 détectées, les 4 évasions publiées comme manquées. |
| **Compatibilité** | 0 skill réel sur 129 est compatible avec l'agent gelé, faute de déclarer ses besoins. Refuser l'indéterminé plutôt que le deviner. |
| **Matériel** | Charge soutenue qualifiée (débit stable à moins de 1 %, aucun bridage thermique), énergie GPU mesurée à 4,27 J par jeton (run instrumenté). Baseline retenue par comparaison appariée : Mistral Small 3.2, repli Qwen 3.6 27B. |

## Les instruments sont eux-mêmes éprouvés

Plus de 4 400 tests hors ligne, **éprouvés par mutation** : un mutant sur trois leur survivait, les
faiblesses sont fermées, et la démarche a révélé un vrai défaut de verrou sous contention, reproduit de
façon déterministe puis corrigé. Les audits externes (GLM, ChatGPT) sont confrontés au code ligne par
ligne : quatre défauts d'instruments reproduits puis remplacés, sans qu'aucune donnée close ne soit
faussée. Une campagne dont le protocole ne pouvait pas voir le gain du choix de skill est conservée et
publiée comme instrument défaillant.

## Où en est le projet (30 septembre 2026)

**116 objectifs sur 200 clos.** Gouvernance, veille, matériel, modèle de départ, harnais, bancs d'essai,
télémétrie, consignes et outils sont terminés (lots F0 à F10, portes G1 à G3 fermées). **En cours :
la forge de skills (F11).** À venir : mémoire et contexte, planification, effet unitaire de chaque
composant puis leurs interactions, et seulement ensuite la coordination multi-agents.

## Comment c'est construit

Une seule personne et des agents de code (Claude Code, Codex) sous un protocole explicite : tranches
petites et réversibles, une preuve à chaque étape, décisions datées, audits croisés vérifiés à la source.
Python · Ollama · llama.cpp · Streamlit · SQLite · Pydantic · SciPy · pytest · mypy strict · ruff

---
Cyril Bourgeois, Data Scientist et ingénieur ML ·
[Portfolio](https://cyril-bgs-dev-tech.github.io/Portfolio/) ·
[LinkedIn](https://www.linkedin.com/in/cyril-bourgeois-65a739150) ·
[Kaggle](https://www.kaggle.com/cyrilbourgeois)

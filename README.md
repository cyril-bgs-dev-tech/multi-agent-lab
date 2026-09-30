# Multi-Agent-Lab

**Laboratoire d'évaluation d'agents LLM 100 % locaux** (RTX 5090, 32 Go de VRAM, Ollama).
Nom d'origine : *Tune the Swarm*. Produit en construction : *ForgeLab*.

> Ce dépôt est une **vitrine** : il explique le projet, sa méthode et ses résultats.
> Le code, les données et les bundles de preuve restent dans un dépôt privé, d'abord parce que
> le protocole repose sur des tâches d'évaluation scellées qui ne doivent jamais fuiter.
> L'accès au code est ouvert sur demande, pour un entretien technique par exemple.

---

## La question

Un agent LLM, c'est un modèle entouré de beaucoup de choses : un harnais (la boucle qui le fait
agir), des outils, une politique de contexte, une mémoire, des consignes, des *skills*. Chaque
article, chaque dépôt promet que *sa* brique améliore l'agent.

**Multi-Agent-Lab mesure ce que chaque brique apporte réellement**, face à une baseline, sur les
mêmes tâches et les mêmes graines, avec une chaîne de preuve que l'on peut rejouer. Et il publie
aussi ce qui ne marche pas.

Le nom désigne la destination, pas l'étape en cours : le programme actuel est volontairement
**mono-agent**. Le multi-agent n'est autorisé qu'après un bénéfice mono-agent démontré ; deux
critères de clôture de la roadmap l'interdisent avant.

## La méthode

- **Préenregistrement.** Avant toute mesure comparative, le protocole est écrit, haché et commité :
  tâches, métriques, seuils, analyse. Le premier protocole de sélection fixait 24 tâches
  d'évaluation, 8 tâches de diagnostic « brûlées » d'avance et une puissance statistique à 103 paires.
- **Analyse appariée.** Baseline et candidat voient les mêmes tâches et graines ; la conclusion
  porte sur les écarts appariés. Un écart non résolu reste non résolu, même quand il est flatteur.
- **Dénominateurs honnêtes.** Exécution technique, validité du format et justesse sont trois
  dimensions distinctes ; erreurs et timeouts restent au dénominateur. Une valeur non mesurée n'est
  jamais un zéro.
- **Niveaux de preuve.** Chaque conclusion affiche le sien, de E0 (simulation) à E5 (réplication
  sur un autre modèle ou jeu de données).
- **Protection contre la fuite.** 40 tâches privées scellées (seules leurs empreintes sont
  versionnées), découpage train / dev / validation / final sans chevauchement, canaris dont
  l'efficacité est mesurée, final illisible pour tout composant d'optimisation.
- **Chaîne de preuve.** Chaque objectif clos produit un bundle de preuve adressé par contenu
  (plus de 130 à ce jour). Un seul ticket est actif à la fois, et il ne se ferme que sur preuve.
- **Contenu externe = donnée.** Papiers, dépôts et skills importés sont cités, épinglés, bornés,
  jamais exécutés sans qualification. Les licences sont vérifiées : un texte n'est conservé que
  s'il est redistribuable.

## Où en est le projet (30 septembre 2026)

**116 objectifs sur 200 clos.** Les lots F0 à F10 sont terminés, les portes de validation G1 à G3
sont fermées, le lot F11 est en cours.

| lot | sujet | état |
|---|---|---|
| F0 à F2 | Gouvernance, veille scientifique, sécurité de la chaîne d'approvisionnement | clos |
| F3 | Enveloppe matérielle RTX 5090 | clos |
| F4 | Choix du modèle local de départ | clos |
| F5 | Harnais et interface agent-ordinateur | clos |
| F6 | Génome et compilateur d'agents | clos |
| F7 | Bancs d'essai et protocole anti-fuite | clos |
| F8 | Télémétrie, coût et statistiques | clos |
| F9 | Consignes et règles | clos |
| F10 | Outils, édition et exécution sûre | clos |
| F11 | Forge de skills | **en cours** |
| F12 à F19 | Mémoire et contexte, planification, sélection unitaire, interactions, recherche automatique (ADAS), interface, usage réel, réplication | à venir |

## Résultats marquants

**Matériel et modèles**
- Première génération réelle sur 3 modèles locaux et 15 paliers de contexte, charge soutenue
  qualifiée (débit stable à moins de 1 %, aucun bridage thermique), énergie GPU mesurée à
  4,27 J par jeton sur le run instrumenté.
- Adhérence aux outils : Qwen 16 sur 16, Mistral 14, Gemma 4 (par protocole texte).
- Baseline retenue par comparaison appariée : **Mistral Small 3.2**, repli **Qwen 3.6 27B**
  (écart de qualité non significatif, les deux non dominés).

**Harnais**
- Transports de modification de code comparés sur la baseline : édition structurée 16/18,
  recherche/remplacement 12/18, fichier entier 9/18, patch unifié 8/18.
- Politiques d'historique : le résumé garde toute l'information à 16 % de jetons en moins ; la
  fenêtre glissante la perd.
- Ablation à modèle fixe : 18/18 pour chaque bras. Sur ces tâches, la boucle minimale suffit ;
  le harnais gelé est donc le plus simple.

**Consignes : des résultats négatifs, publiés comme tels**
- Sept formats de consigne mesurés en campagne réelle : aucun ne bat la baseline.
- Deux optimiseurs de la littérature reproduits, TextGrad et GEPA : GEPA affiche +15 points en
  confirmation, **non résolu** statistiquement ; aucun gain confirmé. La baseline est conservée.

**Outils et sécurité**
- Schémas d'appel durcis par fuzzing : 47 appels invalides atteignaient un outil, 0 après.
- Moindre privilège : refus par défaut, octrois par tâche et par durée, tout effet hors octroi
  bloqué avant exécution et tracé.

**Skills**
- 129 skills de cinq sources épinglées inventoriés sans rien charger ni exécuter.
- Scanner de sécurité statique à efficacité mesurée : 9 attaques sur 13 détectées sur un corpus
  hostile, les 4 évasions publiées comme manquées.
- Compatibilité : **0 skill réel sur 129** n'est compatible avec l'agent gelé, faute de déclarer
  ses besoins. Refuser l'indéterminé plutôt que de le deviner.
- **Activation ciblée** : pour choisir le bon skill, une décision typée du modèle bat nettement un
  routeur classique. 84 bonnes décisions sur 107 avec Qwen et 63 avec Mistral, contre 41 pour le
  routeur et 33 pour le témoin sans skill ; écart résolu pour les deux modèles. La campagne
  précédente, dont le protocole ne pouvait pas voir ce gain, est conservée et publiée comme
  instrument défaillant.

**Qualité des instruments**
- Plus de 4 400 tests hors ligne. La suite est elle-même éprouvée par **mutation** : un mutant
  sur trois lui survivait ; les faiblesses sont fermées, chaque mutant rejoué et tué, et la
  démarche a mis au jour un vrai défaut de verrou sous contention concurrente, reproduit de façon
  déterministe puis corrigé.
- Audits externes (GLM, ChatGPT) confrontés au code, ligne par ligne : quatre défauts
  d'instruments reproduits puis remplacés, sans qu'aucune donnée close ne soit faussée.

## Ce qui vient ensuite

1. **Lot F11** : adapter le banc public SkillsBench à des tâches sans shell, puis reproduire les
   méthodes de skills de la littérature.
2. **Lots F12 et F13** : mémoire, contexte et consommation de jetons ; planification, réflexion,
   vérification.
3. **Lots F14 et F15** : effet unitaire de chaque composant, puis leurs interactions.
4. **R4** : la coordination multi-agents, seulement si le bénéfice mono-agent est démontré.

## Comment le projet est construit

Développé par une seule personne, avec des agents de code (Claude Code, Codex) sous un protocole
explicite : tranches petites et réversibles, une preuve à chaque étape, décisions consignées et
datées, audits croisés par d'autres modèles puis vérifiés à la source. La rigueur exigée des agents
évalués est aussi celle exigée des agents qui construisent le laboratoire.

**Stack** : Python · Ollama · llama.cpp · Streamlit · SQLite · Pydantic · SciPy · pytest ·
mypy strict · ruff

## Contact

Cyril Bourgeois, Data Scientist et ingénieur ML
[Portfolio](https://cyril-bgs-dev-tech.github.io/Portfolio/) ·
[LinkedIn](https://www.linkedin.com/in/cyril-bourgeois-65a739150) ·
[Kaggle](https://www.kaggle.com/cyrilbourgeois)

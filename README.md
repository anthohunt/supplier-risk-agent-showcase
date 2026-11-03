> Showcase repository: the code and data of this project are private; this README describes it. / Dépôt vitrine : le code et les données de ce projet sont privés ; ce README le décrit.

# Supplier risk agent

A small 0.8-billion-parameter model that reads the news and flags, offline, the articles that announce a risk for a supplier.

## The problem

A procurement team needs to know quickly when one of its suppliers is hit by an incident that can slow its deliveries: a production stop, a strike, a lawsuit, financial trouble, a shortage, a cyberattack. This project is a client case: the agent follows a list of 94 suppliers and must run on a laptop, without sending data outside.

## How it works

- Articles come from the GDELT global news feed; the agent reads the full article.
- It looks for a listed supplier that is hit by a disruptive event still under way, among six types: production, labour, legal, financial, price or shortage, cyber.
- Code checks the source: the site's domain must be in a closed list of 2,033 reliable domains.
- Decision: "risk" if the signal comes from a reliable source, "to check" otherwise (a human decides), "no risk" without a signal.
- The model's weights are not changed. Only the harness around the model (questions asked, splitting, rules) is improved, by La boucle: a researcher agent proposes trials, each one is measured, and a sealed exam decides.

## What is built

- Test bench balanced across the three decisions: a sealed exam of 240 articles, a verification set of 240 articles. Pass mark set at 224 correct out of 240.
- Training measured on 1,020 articles: from 76.18% correct decisions for the model alone to 90.39% (922 out of 1,020) with the best harness.
- First exam: 205 correct out of 240 (85.4%), below the pass mark. Cause: overfitting to the training articles, hence the addition of a hidden control set.
- Standalone Windows application, in English: list of articles with their verdict, alert on a risk, and the agent's reasoning. Total size 2.57 GB (model 1.52 GB, engine 0.94 GB, application 0.11 GB). On 100 articles, decisions identical to the bench's; 3.97 s per article on average.

Status: paused. The second exam has not been taken.

## What is public

This page only. The code, the data and the application are a confidential client deliverable.

## Tech stack

- Kev-0.8B, an open-source decision model based on Qwen3.5-0.8B
- Python, PyTorch and transformers for training
- llama.cpp (CUDA) as the application's engine
- PyInstaller for the Windows executable
- GDELT for the article feed

---

## Version française

# Agent risque fournisseur

Un petit modèle de 0,8 milliard de paramètres qui lit la presse et signale, hors ligne, les articles qui annoncent un risque pour un fournisseur.

## Le problème

Une équipe achats doit savoir vite quand un de ses fournisseurs subit un incident qui peut freiner ses livraisons : arrêt de production, grève, procès, difficulté financière, pénurie, attaque informatique. Ce projet est un cas client : l'agent suit une liste de 94 fournisseurs et doit tourner sur un ordinateur portable, sans envoyer de données à l'extérieur.

## Comment ça marche

- Les articles arrivent du flux mondial de presse GDELT ; l'agent lit l'article complet.
- Il cherche si un fournisseur de la liste subit un événement perturbateur encore en cours, parmi six types : production, travail, légal, financier, prix ou pénurie, cyber.
- Le code vérifie la source : le domaine du site doit figurer dans une liste fermée de 2 033 domaines fiables.
- Décision : « risque » si le signal vient d'une source fiable, « à vérifier » sinon (un humain tranche), « pas de risque » sans signal.
- Les poids du modèle ne sont pas modifiés. Seul le harnais autour du modèle (questions posées, découpage, règles) est amélioré, par La boucle : un agent chercheur propose des essais, chacun est mesuré, et un examen scellé décide.

## Ce qui est construit

- Banc de test équilibré entre les trois décisions : examen scellé de 240 articles, vérification de 240 articles. Seuil de réussite fixé à 224 justes sur 240.
- Entraînement mesuré sur 1 020 articles : de 76,18 % de décisions justes pour le modèle seul à 90,39 % (922 sur 1 020) avec le meilleur harnais.
- Premier examen : 205 justes sur 240 (85,4 %), sous le seuil. Cause : sur-ajustement aux articles d'entraînement, d'où l'ajout d'un lot de contrôle caché.
- Application Windows autonome, en anglais : liste des articles avec leur verdict, alerte sur un risque et raisonnement de l'agent. Taille totale 2,57 Go (modèle 1,52 Go, moteur 0,94 Go, application 0,11 Go). Sur 100 articles, décisions identiques à celles du banc ; 3,97 s par article en moyenne.

État : en pause. Le second examen n'a pas été passé.

## Ce qui est public

Cette page seulement. Le code, les données et l'application sont un livrable client confidentiel.

## Pile technique

- Kev-0.8B, modèle de décision open source basé sur Qwen3.5-0.8B
- Python, PyTorch et transformers pour l'entraînement
- llama.cpp (CUDA) comme moteur de l'application
- PyInstaller pour l'exécutable Windows
- GDELT pour le flux d'articles

[English](README.md) · **Français**

**Anthony Hunt** · [Fiche du projet sur ingevision.cloud](https://ingevision.cloud/?p=risque-fournisseur) · [LinkedIn](https://www.linkedin.com/in/anthony-hunt-3948a9196) · [Profil GitHub](https://github.com/anthohunt)

> Dépôt vitrine : le code et les données de ce projet sont privés ; ce README le décrit.

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


							Diagnostic du Cancer du Sein (Wisconsin Breast Cancer Diagnostic Dataset)
							
![WhatsApp Image 2025-10-27 à 13 39 11_c6ff40d2](https://github.com/user-attachments/assets/9ee314b8-ab5e-44c0-90c4-bf2269a4c495)










							
							DGHOUGHI CHOROUK S7 FIN2

							


DESCRIPTION DE LA PROBLEMATIQUE :
<img width="2822" height="949" alt="14" src="https://github.com/user-attachments/assets/86e9461a-fa8c-4e78-a363-f6833e7540ac" />

	
Le jeu de données Breast Cancer Wisconsin Diagnostic est un choix pertinent car il est largement utilisé pour des tâches réelles de classification médicale concernant le diagnostic du cancer du sein. Il provient du UCI Machine Learning Repository, ce qui garantit la qualité, la robustesse et le contexte scientifique des données. Ce dataset est plus complexe et riche que des jeux simples comme Titanic ou Iris. Il est fréquemment utilisé pour évaluer des algorithmes d'apprentissage supervisé dans un contexte médical critique.

C'est un problème de classification binaire supervisée. L'objectif est de prédire si une tumeur mammaire est maligne (cancéreuse) ou bénigne (non-cancéreuse) à partir de caractéristiques mesurées sur des images de cytologie.
Classes cibles : 0 = Malignant (Maligne), 1 = Benign (Bénigne).
L’enjeu est l'aide au diagnostic médical, une application vitale.
Dictionnaire des données (métadonnées)
Taille : 569 échantillons (lignes).
Variables : 30 variables numériques descriptives (features) + 1 variable cible (target).
Types de variables : Toutes les features sont numériques continues.
Signification des variables : caractéristiques morphologiques extraites d’images de biopsies, par exemple :
mean radius : moyenne des rayons des noyaux cellulaires
mean texture : moyenne des variations de texture
mean perimeter : moyenne du périmètre
mean area : moyenne de la surface
mean smoothness : moyenne de la douceur de la surface Target : 0 ou 1, indiquant si la tumeur est maligne ou bénigne, respectivement. Ce dataset est adapté pour appliquer des méthodes avancées de Machine Learning sur données médicales, et il nécessite un bon prétraitement et une analyse rigoureuse car les features sont nombreuses et corrélées.Ainsi, ce choix répond bien à vos critères de pertinence, complexité et objectifs d’analyse supervisée de classification médicale.


1. Introduction
  1.1 Contexte
Le cancer du sein constitue la première cause de mortalité par cancer chez les femmes. L’amélioration du diagnostic précoce repose notamment sur l’analyse automatisée d’images cytologiques. Le dataset Wisconsin Breast Cancer Diagnostic (WDBC) fournit un ensemble de mesures morphologiques extraites d’images de biopsies mammaires. Chaque observation décrit les propriétés géométriques et texturales des noyaux cellulaires, permettant de distinguer les tumeurs bénignes des tumeurs malignes.
  1.2 Problématique
Le problème traité est une classification binaire supervisée consistant à prédire la nature de la tumeur à partir de 30 variables morphologiques. Une performance supérieure à 95 % est généralement considérée comme compatible avec une utilisation clinique en soutien au diagnostic humain.
   1.3 Objectifs
	Construire un pipeline complet incluant prétraitement, exploration, modélisation et évaluation.
  Comparer plusieurs algorithmes de classification en tenant compte de leurs hypothèses et contraintes.
	Optimiser les hyperparamètres et valider la robustesse du modèle.
	Analyser les erreurs de prédiction et évaluer la pertinence clinique des résultats.
  2. Méthodologie
   2.1 Prétraitement des données
Les données ont été nettoyées afin de garantir leur exploitation optimale. Aucun doublon n’a été détecté. L’imputation des valeurs manquantes a été réalisée par un KNNImputer, ce choix étant motivé par sa capacité à préserver les structures locales des données, contrairement aux imputations simples (moyenne ou médiane) qui réduisent la variance et dégradent les relations entre variables.
Une normalisation par StandardScaler a été appliquée. Étant donné les fortes disparités d’échelle entre les variables (certaines mesurées en pixels, d’autres en ratios), cette étape est essentielle pour les modèles sensibles aux distances (SVM, méthodes basées sur la variance). Étant donné les corrélations élevées entre certaines variables (radius, perimeter, area), une transformation standardisée permet également de stabiliser l’entraînement.
Enfin, des variables dérivées ont été construites (par exemple ratios géométriques et produits liés à la compacité) afin d’améliorer la capacité discriminante du modèle.
    2.2 Analyse exploratoire
Les distributions et les corrélations ont été examinées pour comprendre la structure interne du jeu de données. Les tumeurs malignes présentent systématiquement des dimensions cellulaires plus élevées, des contours plus irréguliers et une texture plus hétérogène. La heatmap des corrélations confirme la redondance entre les mesures géométriques brutes et leurs dérivés, orientant ultérieurement les choix en matière de sélection ou de régularisation.
<img width="1489" height="990" alt="image" src="https://github.com/user-attachments/assets/f7633e6f-c26a-4b23-b932-0baa68eb5c37" />

    2.3 Modélisation
Trois modèles ont été sélectionnés pour leur complémentarité théorique :
-	Régression Logistique : modèle linéaire interprétable, utilisé comme baseline pour évaluer la difficulté intrinsèque du problème.
	Random Forest : modèle d’arbres agrégés capable de capturer des interactions complexes et robuste aux valeurs aberrantes. Le nombre d’arbres et la profondeur ont été optimisés afin de réduire le sur-apprentissage.
	SVM avec noyau RBF : particulièrement adapté aux frontières de décision non linéaires en haute dimension. Le paramètre de régularisation C et le paramètre gamma du noyau ont été ajustés par validation croisée.
Une validation croisée stratifiée à 5 plis, combinée à une recherche systématique d’hyperparamètres (GridSearchCV), a permis de sélectionner le modèle le plus performant. Un jeu de test indépendant (20 % des données) a été conservé pour évaluer la généralisation.
3. Résultats et Discussion
  3.1 Performances globales
Les métriques obtenues en validation croisée sont les suivantes :
Modèle	Accuracy (CV)	F1-Score (Macro)
Régression Logistique	0.947	0.945
SVM (RBF)	0.955	0.954
Random Forest	0.962	0.961
<img width="530" height="455" alt="image" src="https://github.com/user-attachments/assets/5010e818-900c-43c5-944f-d4d6b7908f26" />






Le Random Forest obtient les meilleurs résultats et a été retenu pour l’évaluation finale. Sur le jeu de test, il atteint une accuracy de 0.965 et un F1-score de 0.964, ce qui dépasse les performances de nombreux diagnostics manuels rapportés dans la littérature. Le ROC-AUC de 0.992 confirme la capacité du modèle à bien séparer les deux classes, même avec un léger déséquilibre des observations.
3.2 Analyse des erreurs
L’examen de la matrice de confusion révèle :
	2 faux positifs (tumeur bénigne classée maligne)
	1 faux négatif (tumeur maligne classée bénigne)
Les faux positifs concernent des cas morphologiquement atypiques présentant des caractéristiques proches de certaines tumeurs malignes (dimensions légèrement élevées, contours irréguliers). Le faux négatif correspond à une tumeur maligne de petite taille, mais présentant une agressivité particulière non entièrement capturée par les descripteurs fournis.
Les variables les plus discriminantes identifiées par l’analyse des importances du modèle sont : mean radius, mean perimeter, mean area, concavity et texture variance. Cette hiérarchie est cohérente avec les critères cliniques classiques d’évaluation cytologique.
<img width="1592" height="1570" alt="image" src="https://github.com/user-attachments/assets/de3ddebe-0cb2-4bec-94b4-d015e233d7ee" />

   3.3 Discussion
Le Random Forest démontre une capacité supérieure à modéliser les interactions complexes entre variables morphologiques. De plus, sa robustesse au bruit et aux redondances explique ses meilleures performances par rapport à la régression logistique. Le SVM obtient des résultats proches mais nécessite une calibration plus fine et reste plus coûteux en temps de calcul.
<img width="1599" height="1454" alt="15" src="https://github.com/user-attachments/assets/675a1911-f046-463f-b046-2dd346a5df90" />

4. Conclusion
   4.1 Limites du modèle
Plusieurs limites doivent être soulignées :
	Corrélations élevées entre les variables, susceptibles d’introduire de la redondance et d’augmenter le risque de sur-apprentissage.
  Déséquilibre relatif des classes (212 malignes versus 357 bénignes), pouvant biaiser légèrement l’apprentissage malgré la stratification.
	Dataset relativement restreint et centré géographiquement, ce qui limite la capacité de généralisation à d’autres populations ou conditions d’acquisition.
	Absence d’images originales, empêchant toute analyse basée directement sur les textures ou formes réelles (modèles CNN).
    4.2 Pistes d'amélioration
Plusieurs axes d’optimisation peuvent être envisagés :
	Sélection de variables via RFE ou analyses d’explicabilité de type SHAP pour réduire la redondance.
	Utilisation de techniques de rééquilibrage comme SMOTE.
	Entraînement de modèles plus expressifs, notamment des réseaux convolutifs appliqués aux images d’origine.
	Approches par empilement (stacking) combinant les forces des modèles arbres et SVM.
	Intégration de méthodes explicatives pour assurer une adoption clinique (LIME, SHAP, règles dérivées).
En l’état, le modèle présente déjà une performance compatible avec des applications d’aide au diagnostic, sous réserve d’une validation plus large et d’une intégration dans un workflow clinique complet.



# Installation et exécution

* Téléchargez les notebooks "train.ipynb" et "inference.ipynb"
* Téléchargez le répertoire "saved_model" contenant la sauvegarde modèle entrainé
* Connectez-vous à votre compte google
* Sauvegardez les fichiers dans votre espace google drive ("Mydrive/")
* Ouvrez une session colab et ouvrez le notebook "train.ipynb" ou "inference.ipynb" (volet "Fichier" > "importer le notebook" > "google drive")
* Lancer le script sur colab en exécutant toutes les cellules dans l'ordre (volet "Exécution"> "tout éxecuter" ou  Ctrl+F9)

# Approche choisie : 2 Conv + 2 FC

La méthode utilisée est un réseau de neurones convolutifs à 2 couches suivi de 2 couches entièrement connectées.

* **Couche de convolution :** Convolution 3x3 + activation ReLu + Normalisation du Batch + Maxpooling 2x2
* **Couche entièrement connécté :** unités entièrement connectées + activation ReLu + dropout 0.5
* **Couche de sortie :** Softmax

![image](https://user-images.githubusercontent.com/77834232/128615216-08ca787b-0d3f-41b6-8705-bcb5fabe866b.png)


# Entraînement

**Fonction de coût** : Entroprie Croisée

**Optimisation** : Adam

Les paramètres choisis sont ceux qui ont été calculés lors de l'epoch 10. En effet, on se rend compte que la validation_loss est au minimum à la 10ième epoch. La fonction validation_loss augmente à partir de cette epoch alors que la training_loss diminue, ce qui temoigne d'un sur-apprentissage du modèle sur les données d'entrainement.

![index](https://user-images.githubusercontent.com/77834232/128617455-98821c4a-78e6-44b5-9535-0917b704eece.png)
![index2](https://user-images.githubusercontent.com/77834232/128617458-e2f89443-c539-4a9d-b3d0-d4867c29fc16.png)

# Choix des hyperparamètres

* **Nb epoch                :** 30 (choisi de façon à pouvoir atteindre le sur-apprentissage pour choisir les paramètres optimaux)
* **Nb données entrainement :** 50000 (83% des données totales, se situe dans une fourchette usuelle de 80-95% des données, hors données de test)
* **Taille d'un lot         :** 32
* **Nb étape par epoch      :** 1563
* **Fonctions de cout       :** categorical_crossentropy (fonction de coût classique pour effectuer des tâches de classification)
* **Optimisation            :** Adam (méthode d'optimisation classique)
* **Learning rate           :** 1e-3 ( choisi de facon empirique pour obtenir les meilleurs résultats possibles sur la validation_loss; valeurs essayées : 1e-1, 1e-2, 1e-3, 1e-4)

# Resultats

![image](https://user-images.githubusercontent.com/77834232/128615318-ba1f8008-294e-4f9b-90f2-8abde879c4be.png)

# Analyse des métriques

* **Precision** : 92.73%
* **Accuracy**  : 91.61%

Après analyse de la matrice de confusion, on se rend compte que les fausses prédictions sont souvent les suivantes :
* prédiction de chemise au lieu d'un t-shirt
* prédiction d'un pull ou une chemise au lieu d'un manteau
* prédiction d'un pull ou un manteau au lieu d'une chemise 

<img width="271" alt="Sans titre" src="https://user-images.githubusercontent.com/77834232/128617462-b109f960-f1d2-4588-96ef-d367a0cc704c.png">

![image](https://user-images.githubusercontent.com/77834232/128617604-4fc17099-c254-4c2a-acff-86d4517ddb8b.png)

Cela s'explique par le fait que les images représentant ces classes se ressemblent ( un manteau, une chemise, un t-shirt ou un pull ont souvent la même forme et peuvent partager certaines textures).

# Pistes d'amélioration

* **Augmentation des données** : Une manière d'éviter le sur-apprentissage est l'augmentation des données. Un retournement aléatoire des images selon l'axe vertical est dejà appliqué. Nous pourrions également appliqué un random crop (recadrage aléatoire). D'autres pistes peuvent également être envisagées comme l'ajout d'un bruit (gaussien ou poivre/sel) ou des méthodes modifiant l'histogramme de l'intensité de l'image (ne modifiant pas trop la forme des objets).
* **Learning_rate :** Le learning_rate pourrait être ajusté pour obtenir de meilleurs performances. Après avoir testé différentes valeurs, la valeur optimale se trouverait dans l'intervalle 1e-3 et 1e-4.
* **Nombre de paramètres :** Le nombre de paramètres du réseau est assez elevé (1,6 millions) par rapport à d'autres réseaux ayant des performances équivalentes (avec 100k-500k de paramètres). Ce nombre pourrait probablement être réduit sans trop impacter les performances de l'algorithme.

# Installation et exécution

* Téléchargement du notebook "train.ipynb"
* Lancer le script sur colab ou jupyter notebook en exécutant toutes les cellules dans l'ordre 

# Approche choisie : 2 Conv + 2 FC

La méthode utilisée est un réseau de neurones convolutifs à 2 couches suivi de 2 couches entièrement connectées.

* **Couche de convolution :** Convolution 3x3 + activation ReLu + Normalisation du Batch + Maxpooling 2x2
* **Couche entièrement connécté :** unités entièrement connectées + activation ReLu + dropout 0.5
* **Couche de sortie :** Softmax

![image](https://user-images.githubusercontent.com/77834232/128615216-08ca787b-0d3f-41b6-8705-bcb5fabe866b.png)


# Entraînement

**Fonction de coût** : Entroprie Croisée

**Optimisation** : Adam

Les paramètres choisis sont ceux qui ont été calculés lors de l'epoch 9. En effet, on se rend compte que la validation_loss est au minimum à la 9ième epoch. La fonction validation_loss augmente à partir de cette epoch alors que la training_loss diminue, ce qui temoigne d'un sur-apprentissage du modèle sur les données d'entrainement.

![image](https://user-images.githubusercontent.com/77834232/128614818-db80b211-9298-4d08-b223-11edd908db06.png)

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

* **Precision** : 91.54%
* **Accuracy**  : 92.81%

Après analyse de la matrice de confusion, on se rend compte que les fausses prédictions sont souvent les suivantes :
* prédiction de chemise au lieu d'un t-shirt
* prédiction d'un pull ou une chemise au lieu d'un manteau
* prédiction d'un pull ou un manteau au lieu d'une chemise 

Cela s'explique par le fait que les images représentant ces classes se ressemblent ( un manteau, une chemise, un t-shirt ou un pull ont souvent la même forme et peuvent partager certaines textures).

![image](https://user-images.githubusercontent.com/77834232/128615361-30b567f7-bd51-448f-8dcc-3906f08992fb.png)

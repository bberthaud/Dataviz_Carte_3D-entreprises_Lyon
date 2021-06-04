# Dataviz Carte 3D - entreprises Lyon

L'objectif de cette dataviz est de visualiser sur une carte quels sont les secteurs d'activités des entreprises de la métropole de Lyon, comment ces secteurs sont répartis spatialement, quelle taille chacun représente. Le projet consiste à réaliser une carte 3D (3 dimensions : spatiale, couleur et hauteur) avec les entreprises du territoire selon leur secteur d'activité, encodé visuellement par la couleur, et selon leur taille en CA (chiffre d'affaires) ou RH (nombre de salariés), encodée visuellement par la hauteur. Il est aussi possible de tenir compte de leurs capitaux (4e dimension) en appliquant un filtre.


## Attentes

La dataviz s'inspire de celle-ci : https://bl.ocks.org/ryanbaumann/f4a2e6970eedd1948aca182d0f184968. Elle est une adaptation de cette dernière avec les fonctionnalités suivantes :

* carte 3D pour représenter la taille et le secteur des entreprises industrielles
* agrégation des entreprises afin d'offrir une vue micro et une vue macro de la répartition des entreprises
* tooltip pour apporter des précisions sur la répartition des effectifs par secteur au sein d'une barre : on affiche les 3 secteurs les plus présents avec leur effectif (ainsi que le pourcentage) puis le reste et enfin l'effectif total de la barre
* curseurs pour régler le rayon, la transparence et l'échelle
* filtre pour garder que les entreprises dans une fourchette (min-max) d'effectifs salariés
* filtre sur les capitaux, on ne conserve que les entreprises qui dépassent le seuil sélectionné
* affichage automatique qui règle le rayon automatiquement selon le zoom
* légende sur les couleurs des secteurs présents avec sélection des secteurs 
* choix du fond de carte (dark, light, outdoors, satellite)
* information générale sur le projet avec contact


## Paramètres

Le rayon (radius), exprimé en mètres, définit la taille de la zone qui est agrégée. Cette zone est ensuite affichée sous forme hexagonale.

L'opacité est un facteur compris entre 0 et 1 : 0 pour transparent et 1 pour opaque.

L'échelle est un facteur multiplicatif.

La couverture (coverage) est un facteur multiplicatif compris entre 0 et 1. Le rayon affiché correspond à `coverage * radius`.


## Données en entrée

Dans *script.js*, ``filename`` contient le nom du fichier de données csv. Ce fichier de données est généré grâce au script jupyter *dataProcessed.ipynb* . ``SECTOR_RANGE`` est à modifier en fonction des secteurs des entreprises concernés. Si ``SECTOR_RANGE`` est modifié il faut s'assurer que ``COLOR_RANGE`` est de la même longueur puisqu'une couleur est attribuée à un secteur.

Tous les fichiers nécessaires aux données et à leurs prétraitements se trouvent dans le dossier ``data/``. Le script jupyter *dataProcessed.ipynb* permet de passer du fichier *SIRENE_GEO_122016.csv* de la base SIRENE des entreprises de la région lyonnaise (disponible [ici](https://data.grandlyon.com/jeux-de-donnees/base-sirene-metropole-lyon/telechargements)) au fichier *data_processed.csv* d'entrée de la dataviz. 

Le fichier csv d'entrée doit être au format suivant :
* une colonne "secteur" de type catégorielle avec les indices rattachés aux secteurs de la liste ``SECTOR_RANGE``;
* une colonne "RH" de type numérique; 
* deux colonnes "lng" (longitude) et "lat" (latitude) pour les coordonnées des entreprises;
* et une colonne capitaux en % (de 0 à 100)


## Réalisation

L'application a été développée pour être affichée sur Chrome. Elle peut aussi être utilisée sur Firefox mais le style n'a pas été travaillé pour ce navigateur web.

La réalisation technique est faite grâce à D3.js avec mapbox. La couche de barres hexagonales est développée par deck.gl. Il est nécessaire de se créer un compte mapbox pour récupérer les cartes. Le compte doit être créé ici : https://www.mapbox.com/. Une fois créé copiez-collez votre "default public token" et remplacez la valeur de ``mapboxgl.accessToken`` dans ``script.js``.

Lors de l'affichage automatique le rayon est automatiquement calculé en fonction du zoom selon la loi suivante :
`rayon = (19 / zoom) ** 10`. Il s'agit d'une loi géométrique inverse et elle est réglée manuellement (ajustement des constantes) afin d'avoir un rendu satisfaisant pour l'UX.

<!-- ![\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}](https://latex.codecogs.com/svg.latex?\Large&space;x=\frac{-b\pm\sqrt{b^2-4ac}}{2a})  -->


## Contact

Pour toutes questions techniques ou suggérations : baptisteberthaud@hotmail.fr


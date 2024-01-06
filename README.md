# GPS tracker

# **Objectif du projet**

L’objectif est de concevoir un tracker GPS pouvant envoyer sa localisation via un serveur Adafruit.

Vidéo du projet : <https://www.tiktok.com/@__hakii__/video/7243024126351248666>

# I- **Conception logicielle**

Nous devons dans un premier temps créer un compte Adafruit puis se rendre sur le site Adafruit IO.

Après avoir créé un nouveau Dashboard, nous mettons en place un feed qui récupèrera les données envoyées par la carte.

Dans le Dashboard, nous créons un nouvel objet map où nous lui donnons la valeur 1 afin qu’elle s’actualise en temps réel et nous l’associons au feed créé.

# II- **Conception électronique**

Pour notre projet, nous avons besoin de deux éléments principaux : une carte pour envoyer des données sans avoir besoin de réseau Wifi et un GPS.

La carte choisie est l’esp32 Sim800L dont on insèrera une carte Sim avec un forfait (attention cette carte fonctionne en 2G, on doit vérifier que l’opérateur utilisé prend encore en compte ce réseau). 

Le GPS est le Neo-6M. Comme indiqué sur le composant, nous le branchons au 5V, Ground et aux pin Rx Tx de la carte. Nous utiliserons une bibliothèque pour réaliser une communication série sur d'autres broches numériques. Ainsi nous le branchons aux pins GPIO 12 et 14.

# III- **Conception informatique**

## a) Récupération coordonnées GPS

Pour recevoir les données du GPS, nous utiliserons les bibliothèques `TinyGPS++` ainsi que `SoftwareSerial`.

- `SoftwareSerial` nous permet de modifier les pins de communication série.
- `TinyGps++`, grâce à ses fonctions, nous permet de récupérer plusieurs informations comme la latitude, la longitude et l’altitude.

Une fois les données récupérées à intervalle régulier dans le *loop*, il nous faut les placer dans un buffer ( tableau de char) pour pouvoir les envoyer toutes en même vers Adafruit.

Nous déclarons un pointeur de ce buffer et une fois toutes les données utiles reçues, nous les plaçons dans notre tableau séparé par une virgule.

Attention : Pour que la carte puisse comprendre l’arrivé des données en temps que coordonnées, il faut conserver un certain ordre : vitesse mph, latitude, longitude, altitude.

Remarque : Pour la première initialisation du GPS, il faut attendre parfois plusieurs minutes avant que le GPS trouve des satellites et commence à clignoter. Il est préférable d’être en extérieur.

## b) Mise en place de la Carte sim800L

Pour pouvoir accéder à la carte Sim, il faut pouvoir indiquer son code pin (code demandé au démarrage du téléphone) ainsi que l’APN (le point d’accès de l’opérateur permettant d’avoir accès au réseau), le nom utilisateur et le mot de passe de l’opérateur de la carte Sim.

Remarque : pour les trouver, il suffit de taper « APN + opérateur ». Il est possible de laisser vide certaines variables.

Pour accéder à ses informations sur Adafruit, il faut pouvoir renseigner :

- Le nom du serveur : "io.adafruit.com"
- Le nom d’utilisateur
- La clé password
- L’url d’accès au feed recevant les données

Dans le *loop*, nous envoyons le buffer de coordonnées à Adafruit toutes les 30 secondes grâce à la fonction `publish()` et à la bibliothèque `SimpleTimer`

La compilation du code se fait avec le module esp32 Dev Module

Remarque : Le programme s’appuie sur la vidéo suivante : 

[https://www.youtube.com/watch?v=k4WRwULvoLo&list=LL&index=29&t=1962s&ab_channel=RadhiSghaier](https://www.youtube.com/watch?v=k4WRwULvoLo&list=LL&index=29&t=1962s&ab_channel=RadhiSghaier)

# Rendu final

![MAP](https://user-images.githubusercontent.com/92324336/175808928-4afb715b-2116-490e-8e8b-1298ec236199.jpg)

![20220624_182608](https://user-images.githubusercontent.com/92324336/175808921-8c27eb9d-d0dc-41ce-8603-6d4c09438ee9.jpg)

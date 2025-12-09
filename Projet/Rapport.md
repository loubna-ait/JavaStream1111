#loubna Ait-Hra

#projet : Application de Gestion Intelligente du Stationnement Urbain

Description du diagramme de classes – Système SmartParking

#diagramme de classe

<img src="Diagramme de classe.png" style="height:432px;margin-right:432px"/>

Description des Tables- Système Smart Parking
1. Introduction
Le système Smart Parking est composé de 9 tables principales organisées selon un modèle relationnel. Ces tables représentent les entités métier du système et leurs interactions. Cette section présente le rôle de chaque table et les relations qui les lient.

2. Description des Tables
2.1 Table UTILISATEUR
Rôle: Table principale qui stocke les informations de base de tous les utilisateurs du système.
Fonction: Sert de table parent pour les différents types d'utilisateurs (conducteurs et administrateurs). Elle centralise les données communes comme l'authentification et les informations personnelles.
Utilité dans le système: Point d'entrée pour la connexion et la gestion des profils utilisateurs.

2.2 Table CONDUCTEUR
Rôle: Représente les utilisateurs finaux qui utilisent l'application pour trouver et réserver des places de stationnement.
Fonction: Hérite de la table Utilisateur et ajoute des informations spécifiques aux conducteurs comme les coordonnées de contact et les informations du véhicule.
Utilité dans le système: Table centrale pour la gestion des réservations et l'historique des utilisateurs mobiles.

2.3 Table ADMINISTRATEUR
Rôle: Représente les utilisateurs ayant des privilèges d'administration du système.
Fonction: Hérite de la table Utilisateur et permet d'identifier les comptes avec droits de gestion avancés (création de parkings, configuration des tarifs, consultation des statistiques).
Utilité dans le système: Contrôle d'accès aux fonctionnalités d'administration via le dashboard web.

2.4 Table PARKING
Rôle: Stocke les informations relatives à chaque parking physique géré par le système.
Fonction: Contient les données de localisation (GPS), la capacité totale, le nombre de places disponibles en temps réel et les informations tarifaires de base. C'est le conteneur principal des places de stationnement.
Utilité dans le système: Permet la recherche géographique des parkings disponibles et le calcul de la disponibilité globale.

2.5 Table PLACE_STATIONNEMENT
Rôle: Représente chaque place de stationnement individuelle dans un parking.
Fonction: Gère l'état en temps réel de chaque place (libre, occupée, réservée, en maintenance). Chaque place est identifiée par un numéro unique et possède un type spécifique (normale, handicapé, électrique, moto).
Utilité dans le système: Élément central du système IoT, elle est directement liée aux capteurs pour la détection en temps réel et aux réservations des conducteurs.

2.6 Table CAPTEUR_IOT
Rôle: Représente les capteurs physiques installés sur les places de stationnement.
Fonction: Stocke les informations techniques des capteurs (type de technologie, état de fonctionnement, dernier signal reçu). Chaque capteur détecte la présence ou l'absence d'un véhicule et communique avec le backend via protocole MQTT.
Utilité dans le système: Cœur de la dimension IoT du projet, permet la mise à jour automatique et en temps réel de la disponibilité des places sans intervention humaine.

2.7 Table RESERVATION
Rôle: Enregistre toutes les réservations effectuées par les conducteurs.
Fonction: Gère le cycle de vie complet d'une réservation depuis sa création jusqu'à sa clôture. Stocke les dates de début et fin, le code unique de réservation (QR code), le statut et le montant calculé.
Utilité dans le système: Permet la réservation anticipée des places, génère les revenus et maintient l'historique des transactions.

2.8 Table PAIEMENT
Rôle: Gère les transactions financières liées aux réservations.
Fonction: Enregistre les informations de paiement (montant, méthode, statut de la transaction). Communique avec les gateways de paiement externes (Stripe, PayPal) pour le traitement sécurisé des transactions.
Utilité dans le système: Assure la monétisation du service et la validation des réservations. Génère les reçus et factures électroniques.

2.9 Table TARIF_DYNAMIQUE
Rôle: Définit les grilles tarifaires variables selon les plages horaires.
Fonction: Permet l'application de tarifs différenciés en fonction de l'heure de la journée, du jour de la semaine ou du taux d'occupation. Utilise un système de coefficients multiplicateurs appliqués au tarif de base.
Utilité dans le système: Optimise les revenus et régule l'affluence en appliquant des tarifs plus élevés aux heures de pointe et réduits aux heures creuses.

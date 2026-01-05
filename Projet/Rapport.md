#loubna Ait-Hra

#projet : Application de Gestion Intelligente du Stationnement Urbain

Description du diagramme de classes – Système SmartParking

#diagramme de classe

<img src="version finale diagramme de classe.png" style="height:432px;margin-right:432px"/>
Explication du Diagramme de Classes - Smart Parking
1. Vue Générale
Le diagramme de classes représente l'architecture complète du système Smart Parking. Il est composé de 9 classes et 1 interface qui modélisent les entités métier et leurs interactions.
Structure du système :

Gestion des utilisateurs : Utilisateur, Conducteur, Administrateur
Gestion du parking : Parking, PlaceStationnement, CapteurIoT, TarifDynamique
Gestion des transactions : Reservation, Paiement
Abstraction : Interface IReservable


2. Description des Classes
Interface IReservable
Définit le contrat pour tout élément réservable du système avec trois méthodes essentielles : vérifier la disponibilité, réserver et libérer. Cette interface est implémentée par PlaceStationnement, garantissant ainsi que toute place possède les fonctionnalités nécessaires pour être réservée.
Utilisateur (classe parent)
Classe de base contenant les informations communes à tous les utilisateurs (id, nom, email, mot de passe, rôle). Elle permet l'authentification et la gestion de profil.
Conducteur
Hérite de Utilisateur et représente l'utilisateur final de l'application mobile. Il possède des informations supplémentaires (téléphone, véhicule) et peut rechercher des parkings disponibles et réserver des places.
Administrateur
Hérite également de Utilisateur mais dispose de privilèges de gestion : création/modification des parkings et consultation des statistiques du système.
Parking
Représente un parking physique avec sa localisation GPS (latitude, longitude), sa capacité totale, le nombre de places disponibles en temps réel et le tarif horaire de base. Il contient plusieurs places de stationnement et peut avoir plusieurs grilles tarifaires.
PlaceStationnement
Élément central du système représentant une place individuelle. Chaque place possède un numéro, un type (normale, handicapé, électrique, moto), un statut (libre, occupée, réservée, maintenance) et est liée à un capteur IoT. Cette classe implémente l'interface IReservable.
CapteurIoT
Représente le capteur physique installé sur chaque place pour la détection automatique en temps réel. Il détecte l'occupation de la place et envoie les données au serveur via protocole MQTT. Supporte plusieurs technologies : ultrason, magnétique, infrarouge, caméra IA.
Reservation
Gère le cycle de vie d'une réservation avec un code unique (QR code), des dates de début/fin, un statut (en attente, confirmée, active, terminée, annulée) et un montant calculé. Chaque réservation est liée à un conducteur, une place et génère un paiement.
Paiement
Gère les transactions financières avec le montant, la date, la méthode de paiement (carte bancaire, PayPal, etc.) et le statut de la transaction. Chaque paiement est lié à une seule réservation.
TarifDynamique
Permet la tarification variable selon les plages horaires en appliquant un coefficient multiplicateur au tarif de base du parking. Par exemple : coefficient 1.5 en heures de pointe, 0.5 la nuit.

3. Relations Principales
Héritage
Utilisateur est la classe parent dont héritent Conducteur et Administrateur, permettant de factoriser les attributs communs.
Implémentation
PlaceStationnement implémente l'interface IReservable, s'engageant à fournir les méthodes de vérification, réservation et libération.
Associations

Conducteur ↔ Reservation (1:N) : Un conducteur peut avoir plusieurs réservations
Parking ↔ PlaceStationnement (1:N) : Un parking contient plusieurs places
PlaceStationnement ↔ CapteurIoT (1:1) : Chaque place a un capteur unique
PlaceStationnement ↔ Reservation (1:N) : Une place peut avoir plusieurs réservations dans l'historique
Reservation ↔ Paiement (1:1) : Chaque réservation génère un paiement unique
Parking ↔ TarifDynamique (1:N) : Un parking peut avoir plusieurs grilles tarifaires


4. Scénario d'Utilisation

Un Conducteur recherche des Parkings disponibles
Il sélectionne une PlaceStationnement libre
Le système vérifie la disponibilité via IReservable.verifierDisponibilite()
Une Reservation est créée avec calcul du montant selon le TarifDynamique
Un Paiement est traité
Si le paiement est validé, la réservation est confirmée et la place réservée
À l'arrivée du véhicule, le CapteurIoT détecte l'occupation
La place passe au statut "occupée" et la réservation devient "active"
Au départ, le capteur détecte la libération
La place redevient "libre" et la réservation est "terminée"


5. Avantages de l'Architecture

Modularité : Chaque classe a une responsabilité unique et bien définie
Extensibilité : Facile d'ajouter de nouveaux types d'utilisateurs ou d'éléments réservables
Maintenabilité : Code organisé avec des relations claires
Intégration IoT : Architecture prête pour des milliers de capteurs
Principes OOP : Respecte l'encapsulation, l'héritage, le polymorphisme et l'abstraction


6. Implémentation Technique
Ce diagramme se traduit directement en :

Classes Java avec Spring Boot
Tables SQL dans MySQL (une classe = une table)
API REST pour les interactions
Communication MQTT pour les capteurs IoT

L'interface IReservable permet le polymorphisme et facilite les tests unitaires, tandis que l'héritage évite la duplication de code et respecte le principe DRY (Don't Repeat Yourself).

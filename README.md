# Smart Water Leak Detection

### **1. Niveau Capteur (Edge Device) : ESP32 avec IMU et Capteur Sonore**
- **ESP32** : Microcontrôleur principal chargé de collecter les données des capteurs, traiter localement certaines informations (filtrage de bruit, pré-analyse), et d'envoyer ces données via WiFi.
- **IMU (Inertial Measurement Unit)** : Mesure les vibrations du tuyau d'arrivée d'eau, détectant des motifs liés à l'utilisation de l'eau ou à des anomalies.
- **Capteur Sonore** : Mesure le son ou les vibrations induites par le flux d'eau à travers le tuyau. Cela permet de différencier des scénarios d'utilisation normaux des fuites éventuelles.
- **Local Data Processing** : Le code sur l'ESP32 inclut un prétraitement des données pour réduire le bruit et détecter des motifs simples (par exemple, utiliser une FFT pour analyser les fréquences sonores liées à l'écoulement d'eau).
- **Transmission WiFi** : L'ESP32 envoie régulièrement les données collectées et analysées au cloud via un réseau WiFi domestique.

### **2. Niveau Cloud (Backend) : Stockage, Analyse, et Détection**
- **API REST (ou MQTT)** : L'ESP32 se connecte à une API REST (ou MQTT, selon les préférences) pour transmettre les données en temps réel au cloud.
- **Cloud Platform** : Une plateforme cloud, hébergée potentiellement sur AWS, Azure, ou Google Cloud, qui prend en charge :
  - **Stockage des données** : Les données brutes et traitées (vibrations, sons, etc.) sont stockées dans une base de données NoSQL (par exemple, MongoDB) ou un service de stockage comme S3.
  - **Analyse des données** : Des modèles d'IA/ML (entraînés sur des données historiques) détectent des anomalies dans les vibrations et les motifs sonores qui pourraient correspondre à des fuites.
  - **Détection de fuite** : Un système de détection basé sur l'IA analyse en temps réel les données provenant du capteur et envoie une alerte lorsqu'une fuite est détectée. Le système peut utiliser des algorithmes de classification ou de détection d'anomalies.
- **Machine Learning/IA** :
  - Les données sont traitées par des modèles de machine learning pour détecter des fuites (SVM, Random Forest, ou réseaux de neurones légers). Un pipeline de machine learning peut inclure le prétraitement, la normalisation des données, et l'entraînement de modèles sur des flux continus de données.
  - **Option supplémentaire** : Entraînement des modèles sur les serveurs cloud et déploiement du modèle sur l'ESP32 pour de la détection locale et une analyse en temps réel encore plus rapide.

### **3. Niveau Frontend (Interface Client) : Monitoring et Notifications**
- **Interface Web** : Une interface web hébergée via des services comme Vercel, permettant aux utilisateurs de visualiser :
  - L'état de leur tuyau d'arrivée d'eau (normal ou fuite détectée).
  - Un historique des utilisations d'eau (fréquence, quantité, etc.).
  - Des graphiques des mesures prises (vibrations, sons) avec une détection d'anomalie.
  - Des notifications ou alertes en cas de détection de fuite.
- **Application Mobile** : Une application pour smartphone (iOS/Android) permettrait également aux utilisateurs de recevoir des alertes en temps réel et de consulter les données.
- **Notifications** : Intégration avec des services de notifications (par exemple, Firebase Cloud Messaging pour mobile) pour envoyer des alertes instantanées aux utilisateurs en cas de fuite détectée.

### **4. Sécurité et Gestion des Données**
- **Authentification et Sécurité** :
  - L'accès aux données doit être sécurisé via OAuth2 pour l'API et des certificats TLS pour chiffrer les communications entre l'ESP32 et le cloud.
  - Les utilisateurs accèdent à leurs données via un système d'authentification sécurisé (gestion des comptes, mots de passe, etc.).
- **Mise à jour OTA (Over-The-Air)** : Le firmware de l'ESP32 peut être mis à jour à distance depuis le cloud pour améliorer l'algorithme de détection ou corriger des bugs.
  
### **5. Fonctionnement général**
1. **Collecte des données** : L'ESP32 collecte en continu les vibrations du tuyau et les sons générés par l'eau en mouvement. Il peut traiter localement certains aspects (filtrage, transformations) avant l'envoi.
2. **Transmission** : Les données traitées sont envoyées périodiquement ou en temps réel au cloud via WiFi.
3. **Analyse Cloud** : Les données sont stockées, puis analysées en temps réel ou en différé. Des modèles d'IA détectent des anomalies indiquant une fuite.
4. **Alertes** : En cas de fuite détectée, le backend envoie des notifications à l'utilisateur via l'interface web et mobile.
5. **Consultation des données** : Les utilisateurs peuvent visualiser l'état du système, consulter l'historique des consommations d'eau, et ajuster les paramètres (fréquence des rapports, seuils de détection, etc.).

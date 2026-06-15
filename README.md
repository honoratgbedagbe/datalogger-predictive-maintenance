# Datalogger Industriel Intelligent pour Maintenance Prédictive

**Plateforme polyvalente de surveillance et de diagnostic des machines tournantes par TinyML embarqué**

---

## Objectif

Permettre la maintenance prédictive des machines tournantes industrielles (pompes, moteurs électriques, compresseurs, ventilateurs, boîtes de vitesses) en détectant les anomalies vibratoires, thermiques et électriques avant qu'elles ne provoquent un arrêt coûteux.

---

## Fonctions principales

1. **Acquisition multi-physique** : vibrations 3 axes (accéléromètre MEMS), température de surface, courant statorique moteur.
2. **Analyse embarquée par intelligence artificielle** (TinyML) : auto-encodeur entraîné sur données normales, exécuté localement sur microcontrôleur.
3. **Diagnostic granulaire** : identification du type de défaut (balourd, désalignement, défaut de roulement, cavitation, défaut électrique) et du composant probable.
4. **Pronostic de durée de vie restante (RUL)** : estimation du délai avant panne avec intervalle de confiance.
5. **Protection automatique** : arrêt d'urgence ou délestage si seuil critique franchi.
6. **Communication industrielle** : Modbus TCP via Ethernet, serveur web cognitif embarqué.
7. **Stockage local** : carte SD avec journal des événements horodatés et données brutes au format CSV.
8. **Reconfigurable** : changement de modèle TinyML selon la machine surveillée, sans modification matérielle.

---

## Architecture matérielle

| Composant | Rôle | Interface |
|:---|:---|:---|
| STM32F407VET6 (Cortex-M4, 168 MHz) | Microcontrôleur principal, inférence TinyML | — |
| MPU6050 (GY-521) | Accéléromètre 3 axes pour vibrations | I2C |
| DS18B20 | Capteur de température de surface | OneWire |
| Capteur de courant (ACS712 ou similaire) | Courant statorique pour MCSA | ADC |
| W5500 | Module Ethernet (Modbus TCP, serveur web) | SPI |
| MAX485 | Module RS485 (Modbus RTU, optionnel) | UART |
| Carte microSD (module SPI) | Stockage des données et logs | SPI |
| Convertisseur DC-DC 24V → 5V | Alimentation depuis tension industrielle | — |
| Relais de sécurité | Coupure moteur en cas d'alerte critique | GPIO |

---

## Architecture logicielle

- **Noyau temps réel** : FreeRTOS (CMSIS_V2)
- **Système de fichiers** : FATFS (FatFs Generic FAT File System)
- **Pile TCP/IP** : intégrée au W5500 (driver WIZnet)
- **Protocole industriel** : Modbus TCP
- **IA embarquée** : TensorFlow Lite for Microcontrollers
- **IHM cognitive** : serveur web embarqué (HTML/CSS)
- **Langages** : C (firmware), Python (entraînement des modèles, scripts d'analyse)

### Tâches FreeRTOS

| Tâche | Priorité | Rôle |
|:---|:---|:---|
| SensorTask | Haute | Lecture périodique des capteurs (20 ms) |
| TinyMLTask | Haute | Inférence du modèle sur les données acquises |
| StorageTask | Normale | Écriture des données brutes et logs sur carte SD |
| ModbusTask | Normale | Réponse aux requêtes Modbus TCP |
| WebServerTask | Normale | Service des pages de l'interface web |
| HeartbeatTask | Basse | Supervision, LED de statut |

---

## Structure du dépôt

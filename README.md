# 🚨 Serverless Uptime Monitor su AWS

## 🎯 Obiettivo del Progetto
L'obiettivo è impostare un monitoraggio serverlesse per verificare ogni minuto che il sito sia online. In caso di disservizio, il sistema invia un alert via email agli amministratori, prima che se ne accorgano i clienti.

## 🏗️ Architettura & Servizi AWS usati
Ho usato un'architettura completamente Serverless:
* **Amazon S3:** Hosting del sito statico configurato con Static Website Hosting.
* **Amazon CloudWatch Synthetics (Canaries):** Uno script Node.js/Puppeteer che esegue un controllo Heartbeat sull'URL del bucket ogni 60 secondi.
* **Amazon CloudWatch Alarms:** Un allarme che scatta se la metrica `SuccessPercent` scende sotto il 100%.
* **Amazon SNS (Simple Notification Service):** Un Topic configurato con abbonamento Email per il routing istantaneo della notifica push.

## 🎬 Proof of Concept (Il sistema in azione)
https://github.com/user-attachments/assets/d114f67a-ef32-4949-8024-9377a7aa23f5

(ho tagliato i secondi di attesa intercorsi tra il disservizio e la mail per comodità)

**Test effettuato:**
1. Il sito è online e il Canary registra uno stato `Passed`.
2. Rinomino il file html per simulare un disservizio.
3. Entro circa 90 secondi, l'allarme CloudWatch entra in stato `In Alarm`.
4. Viene recapitata in tempo reale l'email di allarme tramite SNS.

## 💻 Codice
Nel repository sono presenti:
* `test.html`: Il codice dell'endpoint monitorato.
* `canary.js`: Lo script Node.js utilizzato dal CloudWatch Canary per la navigazione headless.

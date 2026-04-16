
# 🚗 Vehicle IoT Telemetry Data Pipeline
### *End-to-End Real-Time Analytics using Apache Kafka, Apache Flink, and Azure*

## 📌 Project Overview
**Fleetage Mobility** requires real-time visibility into vehicle behavior to proactively detect safety and operational anomalies. This project implements a scalable streaming data platform that ingests vehicle telemetry, performs real-time anomaly detection (speed, fuel, temperature), and triggers automated driver notifications.



The solution leverages a **modern event-driven architecture** focusing on **Phase 1 (Hot Path)** implementation to satisfy stakeholder requirements for sub-second alerting.

---

## 🏗️ Architecture Design

### 🔥 Hot Path (Implemented)
* **Flow:** Telematics Device (Simulated) → **Kafka** → **Flink** → **Alert Topic** → **Azure Function** → **Twilio** → **Driver Notification**
* **Purpose:** Low-latency, real-time alerting.
* **Key Traits:** Stateless/Stateful stream operators, serverless triggering, and event-driven dispatch.

### ❄️ Cold Path (Phase 2 - Planned)
* **Flow:** Kafka → Flink → Azure Blob Storage → ADLS Gen2 → ADF → Synapse Analytics
* **Purpose:** Historical trend analysis, BI dashboards, and predictive maintenance modeling.

---

## 🛠️ Technology Stack

| Layer | Technology |
| :--- | :--- |
| **Streaming Platform** | Apache Kafka |
| **Schema Management** | Confluent Schema Registry (Avro) |
| **Stream Processing** | Apache Flink |
| **Cloud Platform** | Microsoft Azure |
| **Serverless Logic** | Azure Functions |
| **Communication** | Twilio API (WhatsApp/Voice) |
| **Languages** | Python (Simulator/Consumers), SQL/Java (Flink) |

---

## ⚙️ Data Flow Implementation

### 1. Telemetry Ingestion
A Python-based simulator mimics **IoT CAN bus** behavior, producing events to the `vehicle.telemetry` topic. 
* **Data Governance:** All messages use **Avro serialization** via the Schema Registry to ensure downstream compatibility and strict data quality.

### 2. Real-Time Stream Processing
Apache Flink consumes the stream and evaluates incoming data against critical business rules:

| Rule | Condition | Action |
| :--- | :--- | :--- |
| **Overspeeding** | Speed $> 80$ km/h | Trigger Warning |
| **Low Fuel** | Fuel $< 10\%$ | Route to Station |
| **Overheating** | Temp $> 200^{\circ}$C | Emergency Stop |



### 3. Notification Delivery
* **Development:** A Python consumer listens to `vehicle.alerts.notifications` and triggers the **Twilio WhatsApp API**.
* **Production:** Utilizes a **Flink HTTP Sink Connector** to invoke Azure Functions, creating a decoupled, highly scalable dispatch mechanism.

---

## 🚀 Key Engineering Highlights
* **Unified Alert Pattern:** Consolidates diverse anomalies into a single downstream topic for simplified consumption.
* **Schema Governance:** Implemented Avro to prevent "poison pill" messages from breaking the pipeline.
* **Stateful Filtering:** Leveraging Flink’s DataStream API for efficient, real-time threshold monitoring.
* **Serverless Integration:** Using Azure Functions to handle the "heavy lifting" of third-party API communication.

---

## 🧠 Skills Gained
* **Messaging:** Kafka topic design, offset management, and partition strategies.
* **Stream Processing:** Building window-less event processing logic and real-time routing patterns.
* **Cloud Architecture:** Designing serverless notification pipelines and hot/cold path separation.
* **Engineering Best Practices:** Decoupling producers from consumers and implementing fault-tolerant sinks.

---

## 🔮 Future Enhancements (Phase 2)
* **Data Lake Archival:** Implementing a sink to **Azure Data Lake (ADLS Gen2)**.
* **Medallion Architecture:** Bronze/Silver/Gold modeling using **Delta Lake**.
* **Advanced Analytics:** Historical trend reporting via **Synapse Analytics** and **Power BI**.
* **Predictive Maintenance:** Using historical data to predict engine failure before the "Overheating" alert triggers.

---

Due to the prototyping nature of Phase 1, physical IoT sensors were simulated, and WhatsApp was used for alerts. In a production environment, this would transition to automated Voice Calls for driver safety.

---
Acknowledgment
Special thanks to Codebasics for providing structured guidance and real-world project frameworks as part of their Data Engineering program. Their curriculum significantly contributed to understanding enterprise streaming architectures and cloud-native pipeline design.

---

<details>



  
</details>

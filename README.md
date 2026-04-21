
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


---
# Setup : Env, topic, registry, cluster
<img width="1293" height="552" alt="image" src="https://github.com/user-attachments/assets/29f882e8-d33f-4912-9563-ed4258850453" />
<img width="1164" height="631" alt="image" src="https://github.com/user-attachments/assets/b14f6ebf-0bb9-40db-80ae-0e65e909c4ae" />
<img width="1270" height="626" alt="image" src="https://github.com/user-attachments/assets/c383e0fa-5e93-479a-93ab-89534cb1fb52" />
<img width="1366" height="427" alt="image" src="https://github.com/user-attachments/assets/66dd9d02-3024-4ca6-90d9-acbc3cbdfce1" />
<img width="1136" height="625" alt="image" src="https://github.com/user-attachments/assets/72251014-ce04-4f1a-8d3e-77405641c349" />
<img width="1326" height="624" alt="image" src="https://github.com/user-attachments/assets/9d4408ab-8dad-48a1-bdc8-0ed398ae0eb0" />
<img width="1299" height="635" alt="image" src="https://github.com/user-attachments/assets/3345aaea-0394-42ef-a438-15bb3f85d635" />
<img width="1025" height="598" alt="image" src="https://github.com/user-attachments/assets/f3493e60-2358-4112-b672-a11f7a50a14c" />
<img width="938" height="602" alt="image" src="https://github.com/user-attachments/assets/29a4e39d-122a-4ec3-8147-cace3f29c9e6" />
<img width="1360" height="504" alt="image" src="https://github.com/user-attachments/assets/af244413-d2ed-4b4c-b769-58b4147ab0c3" />
<img width="868" height="611" alt="image" src="https://github.com/user-attachments/assets/dce6c630-476d-4c30-9963-924ad20374ba" />
<img width="1179" height="629" alt="image" src="https://github.com/user-attachments/assets/6c0aef1c-57a6-4a10-b12c-da7ab2b1685a" />
<img width="737" height="248" alt="image" src="https://github.com/user-attachments/assets/a890e9d5-c86a-457b-984d-c6dcadc66e5d" />

# Producer (Vehicle simulator)

<img width="1357" height="594" alt="image" src="https://github.com/user-attachments/assets/523d6230-51e0-4ba3-97e2-8e6e8fed85ff" />
<img width="1356" height="717" alt="image" src="https://github.com/user-attachments/assets/e0851a08-ebdf-497d-8d93-695326d3feb2" />
<img width="1357" height="712" alt="image" src="https://github.com/user-attachments/assets/f84c8c93-ed83-437f-8006-adb53bbff82c" />

<img width="1348" height="619" alt="image" src="https://github.com/user-attachments/assets/94d19f33-63ea-4ef1-96ae-a983a58e28c1" />
<img width="1358" height="632" alt="image" src="https://github.com/user-attachments/assets/5b25d937-81de-4782-b66b-5c0c66973f71" />
<img width="1349" height="615" alt="image" src="https://github.com/user-attachments/assets/d4fb223f-eea6-4991-aa2e-90296b2636de" />

# Flink 
- TABLE `vehicle_telemetry` - total 131 records 

<img width="1355" height="595" alt="image" src="https://github.com/user-attachments/assets/1f7fb84b-30db-43bb-9b3d-a25e6f2bc8f4" />
<img width="1150" height="600" alt="image" src="https://github.com/user-attachments/assets/afc6e0c7-7bdf-48a3-8480-2abd5bcea299" />
<img width="1353" height="571" alt="image" src="https://github.com/user-attachments/assets/1227b8be-70c7-4ab6-a0c8-2806b36342d7" />
- sql
<img width="1360" height="613" alt="image" src="https://github.com/user-attachments/assets/f494da24-90da-4be6-a227-42b3779d19a7" />
<img width="1348" height="609" alt="image" src="https://github.com/user-attachments/assets/f8944ce6-6110-4bdb-b686-148a155b6348" />
- TABLE `vehicle.speeding`  FILTER CONDITION: Only vehicles exceeding 80 km/h
<img width="1339" height="629" alt="image" src="https://github.com/user-attachments/assets/9b9dcc4b-3112-40f5-b6c4-b3561f73e214" />
- auto create second time
<img width="1236" height="590" alt="image" src="https://github.com/user-attachments/assets/f516cec8-d481-48fd-b2f8-d030842a88c3" />

<img width="1300" height="548" alt="image" src="https://github.com/user-attachments/assets/088ebaf6-bb35-45ad-a054-92b81487c371" />
<img width="1319" height="575" alt="image" src="https://github.com/user-attachments/assets/8e88a7ce-0317-4e8b-a6cf-d01235104897" />
- rerun n generated -360 messages new total - 401
<img width="1362" height="716" alt="image" src="https://github.com/user-attachments/assets/ad64003f-c7ce-4dc3-b334-94236ec033b6" />
<img width="1352" height="588" alt="image" src="https://github.com/user-attachments/assets/e9b7fe33-6f05-4e51-9ba1-091fa985abc2" />
<img width="1348" height="603" alt="image" src="https://github.com/user-attachments/assets/20f669d6-3393-4751-9b1c-7d45ef70aee7" />
<img width="1322" height="463" alt="image" src="https://github.com/user-attachments/assets/1b5a9d93-68ae-446e-85e1-07e9505a96f4" />
<img width="1355" height="610" alt="image" src="https://github.com/user-attachments/assets/d6fa72e8-1a01-4a7e-b36e-19371f84b28b" />
- above 80
<img width="1348" height="612" alt="image" src="https://github.com/user-attachments/assets/c91c36bc-3e93-4df1-b25d-14baebe8437e" />
<img width="1326" height="535" alt="image" src="https://github.com/user-attachments/assets/d404ae9c-9e84-43d9-8085-595ad37c807f" />
<img width="1341" height="599" alt="image" src="https://github.com/user-attachments/assets/7b3cf8fc-b7a4-4cac-9f18-579590b136bd" />
<img width="1261" height="572" alt="image" src="https://github.com/user-attachments/assets/64f35db5-cbca-4cf8-b28e-ccdbe9c929c7" />
<img width="1349" height="615" alt="image" src="https://github.com/user-attachments/assets/bd5c6297-30b6-4607-930b-d77036f92539" />
<img width="1314" height="602" alt="image" src="https://github.com/user-attachments/assets/48347e6e-c3f1-45da-b070-77732bb4e234" />

<img width="1335" height="558" alt="image" src="https://github.com/user-attachments/assets/08b60448-e8b0-4db3-9a32-d88946d39177" />

<img width="1329" height="595" alt="image" src="https://github.com/user-attachments/assets/277dda97-0cc0-48fd-a232-2c3bd06ec082" />
<img width="1250" height="706" alt="image" src="https://github.com/user-attachments/assets/63c4450f-fe50-4da3-8079-769bc26254ee" />
<img width="1354" height="716" alt="image" src="https://github.com/user-attachments/assets/7cd5a83a-b8d4-448d-bad2-cb20a9681dd3" />


- twillio setup
<img width="410" height="565" alt="image" src="https://github.com/user-attachments/assets/e14890ab-b451-4c73-b313-eac0a6b9b4e9" />
<img width="1322" height="580" alt="image" src="https://github.com/user-attachments/assets/4ca9859e-e9ee-4056-b244-893047590686" />
<img width="1317" height="606" alt="image" src="https://github.com/user-attachments/assets/02e697ac-3a71-4ca1-90c5-fbad799cf19a" />
<img width="1357" height="296" alt="image" src="https://github.com/user-attachments/assets/346d0e16-b13a-42c0-a0de-cc9ac0e64f70" />

<img width="1353" height="717" alt="image" src="https://github.com/user-attachments/assets/fc565476-6be7-47b9-a654-add62cbfaca4" />
<img width="1354" height="716" alt="image" src="https://github.com/user-attachments/assets/d71c11f7-d568-49b6-9f22-ded0c5302734" />


- azure function





---
  
</details>


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

Kafka Topic : consumes -> Flink Cluster : stream process - Route to Topics use Kafka triger consumption -> Azure Function (serverless) : consume the alert event the hot path execution engine -> Azure Blob Storage: store alert data -> Twilio: send message

vehicle_telemetry is the source - 
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
<img width="1352" height="530" alt="image" src="https://github.com/user-attachments/assets/02d4b042-330f-4c48-a78c-2f4e9a401f06" />


- twillio setup
<img width="410" height="565" alt="image" src="https://github.com/user-attachments/assets/e14890ab-b451-4c73-b313-eac0a6b9b4e9" />
<img width="1322" height="580" alt="image" src="https://github.com/user-attachments/assets/4ca9859e-e9ee-4056-b244-893047590686" />
<img width="1317" height="606" alt="image" src="https://github.com/user-attachments/assets/02e697ac-3a71-4ca1-90c5-fbad799cf19a" />
<img width="1357" height="296" alt="image" src="https://github.com/user-attachments/assets/346d0e16-b13a-42c0-a0de-cc9ac0e64f70" />
<img width="506" height="347" alt="image" src="https://github.com/user-attachments/assets/178bd446-d229-425d-aeaa-29e693d31a07" />

<img width="1353" height="717" alt="image" src="https://github.com/user-attachments/assets/fc565476-6be7-47b9-a654-add62cbfaca4" />
<img width="1354" height="716" alt="image" src="https://github.com/user-attachments/assets/d71c11f7-d568-49b6-9f22-ded0c5302734" />
- whatsapp notification
<img width="860" height="507" alt="image" src="https://github.com/user-attachments/assets/d44e143a-98bb-4c9a-aed8-308c081dd3cb" />


- azure function
<img width="1364" height="622" alt="image" src="https://github.com/user-attachments/assets/244df65a-968e-40a8-b106-ec163c73043d" />
<img width="1353" height="581" alt="image" src="https://github.com/user-attachments/assets/d179d635-76fd-4ee9-adb2-31150e3eac0d" />
<img width="1363" height="488" alt="image" src="https://github.com/user-attachments/assets/c7f0ea57-f876-4236-adc8-cfcbe74a6d8f" />

---
vs code
<img width="1076" height="707" alt="image" src="https://github.com/user-attachments/assets/538e9106-30f7-42d0-9657-4ee459ea9851" />
<img width="1076" height="707" alt="image" src="https://github.com/user-attachments/assets/8c38d565-ae62-4898-8020-c674046d8852" />
<img width="1365" height="435" alt="image" src="https://github.com/user-attachments/assets/22d56de3-3468-4ffa-b061-7893a67addb6" />

azure az login
<img width="1354" height="539" alt="image" src="https://github.com/user-attachments/assets/8a2fb469-6b8f-4e6c-8558-dc613d052cc1" />
<img width="1353" height="708" alt="image" src="https://github.com/user-attachments/assets/71c8c036-9649-4a78-aab5-5abe310a8506" />

---
- powershell vs code
- az login
- PS C:\code\vehicle_telemetry_azure_assets> .\.venv\Scripts\Activate.ps1
(.venv) PS C:\code\vehicle_telemetry_azure_assets> az functionapp list --resource-group realtime-rg --output table

fapp-telemetry-hotpath-001
- (.venv) PS C:\code\vehicle_telemetry_azure_assets> func azure functionapp publish fapp-telemetry-hotpath-001
install func
func --version
func - it deploys all files in vs code to azure function
func azure functionapp publish fapp-telemetry-hotpath-001


<img width="1357" height="715" alt="image" src="https://github.com/user-attachments/assets/483f1e20-5c75-44bc-9d54-c8bfee8720e2" />
<img width="1359" height="710" alt="image" src="https://github.com/user-attachments/assets/1a2b3879-cdd7-4123-a625-ae1808d21734" />
<img width="1365" height="614" alt="image" src="https://github.com/user-attachments/assets/d6b33b22-af5d-4b75-804c-533b5324e241" />
<img width="1356" height="620" alt="image" src="https://github.com/user-attachments/assets/79609a32-2f8c-4567-8fba-2f83eef0c994" />
<img width="1352" height="620" alt="image" src="https://github.com/user-attachments/assets/8665bae3-b339-4ed1-b7e9-92bfd30104f7" />

<img width="1350" height="714" alt="image" src="https://github.com/user-attachments/assets/1fdb596b-9e65-45aa-9c58-a2484342a52d" />
<img width="1362" height="612" alt="image" src="https://github.com/user-attachments/assets/b47f2def-2b1d-4e6f-a83b-2e438ee4da4c" />
<img width="1361" height="429" alt="image" src="https://github.com/user-attachments/assets/072b6e11-ab70-4efd-b4f1-8deadca8f03d" />

---

TWILIO_FROM_NUMBER - total 7 env varible added
<img width="1363" height="716" alt="image" src="https://github.com/user-attachments/assets/63bc8f06-0a91-4448-8793-9b1658269280" />
<img width="1347" height="577" alt="image" src="https://github.com/user-attachments/assets/069341f9-c57e-4028-9fdf-70edeab5c3c8" />

---

- correct python version and vs code file dir structure
- (.venv) PS C:\code\vehicle_telemetry_azure_assets\1_hot_path> func azure functionapp publish fapp-telemetry-hotpath-001 
<img width="1357" height="712" alt="image" src="https://github.com/user-attachments/assets/cdef5830-8fbb-4360-b5e7-c5a67c46474a" />
<img width="1353" height="608" alt="image" src="https://github.com/user-attachments/assets/0508d1bb-8a55-4ac7-8901-ccf3cb696c9f" />
---
- corrected final dir system 

<img width="1352" height="566" alt="image" src="https://github.com/user-attachments/assets/5b162a9e-74d1-4464-8382-8c0dfd1b9bcf" />
<img width="1361" height="553" alt="image" src="https://github.com/user-attachments/assets/1274f06d-05b8-415d-bdf6-c5c564387fa8" />

---
generate 50 messages from kafka producer - consume flink - tables - run azure func fapp to see in azure function app - invocation - messages

<img width="1356" height="712" alt="image" src="https://github.com/user-attachments/assets/65a0f6ff-12ed-4170-ad07-f1f5fc405adc" />
<img width="1362" height="550" alt="image" src="https://github.com/user-attachments/assets/844039f0-faf0-4fbf-b0ed-051527388dc1" />
<img width="1359" height="598" alt="image" src="https://github.com/user-attachments/assets/1ad2851f-e9be-493b-8271-75f299fbf6d1" />

<img width="1226" height="628" alt="image" src="https://github.com/user-attachments/assets/eb3be2a7-233f-45de-86e6-527967b3fe8b" />





---



---
  
</details>

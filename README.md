# MySQL RDS to S3 using Kafka (CDC Pipeline)

This project demonstrates a **Change Data Capture (CDC) pipeline** using **MySQL hosted on AWS RDS**, **Kafka on EC2**, and the **Kafka S3 Sink Connector** to stream database changes into an **S3 bucket**.

---

## 🛠️ Technologies Used
- **MySQL (AWS RDS):** Source database  
- **Debezium MySQL Connector:** Captures CDC events from MySQL  
- **Apache Kafka (EC2):** Message broker to transport events  
- **Kafka Connect:** Runs Debezium and S3 Sink connectors  
- **Kafka S3 Sink Connector:** Streams Kafka topic data into S3  
- **Amazon S3:** Data lake storage for CDC events  
- **EC2:** Hosting Kafka cluster and connectors  
- **Snowflake (Optional):** Data warehouse for analytics  

---

## 🔄 Workflow

1. **MySQL (RDS) Setup**  
   - Hosted in **Amazon RDS**.  
   - Contains the database and tables that will be tracked for changes (CDC).  
   - Configured with a **Custom DB Parameter Group** to enable binlog settings for Debezium:  
     - `binlog_format = ROW`  
     - `binlog_row_image = FULL`    
   - These settings ensure that row-level changes are captured correctly.

2. **Apache Kafka (EC2-hosted)**  
   - Acts as the **event streaming backbone**.  
   - Hosted on an **EC2 instance**.  
   - Receives CDC events from MySQL via **Debezium MySQL connector**.  
   - Separate **Kafka topics** are created for each MySQL table being monitored.  

3. **Debezium MySQL Source Connector**  
   - Runs on **Kafka Connect** (EC2).  
   - Captures changes from MySQL tables and publishes them to respective Kafka topics.  
**Configuration file in repository:** [`mysql_connector.json`](./mysql_connector.json)
4. **Kafka S3 Sink Connector**  
   - Streams CDC events from Kafka topics into **Amazon S3**.  
   - Supports **JSON, Parquet, or CSV** formats based on configuration.  
   - Ensures a **durable, queryable data lake** for downstream analytics.  
   - **Configuration file in repository:** [`sink_connector_cdc.json`](./sink_connector_cdc.json)

5. **Amazon S3 (Data Lake)**  
   - Stores CDC events from Kafka in your chosen format.  
   - Allows batch or streaming analytics on stored events.  


# Disinformation Monitoring Platform

**Big Data Infrastructure for Audio-Based Disinformation Monitoring**

Progetto universitario sviluppato per il corso di **Information Systems for Big Data**.

La piattaforma ha l'obiettivo di supportare il monitoraggio della disinformazione attraverso l'analisi di contenuti audio provenienti da video online, integrando tecnologie di **Big Data, Data Streaming, Natural Language Processing e Information Retrieval**.

> **Stato del progetto:** infrastruttura Docker configurata e verificata. Le pipeline applicative saranno sviluppate nelle fasi successive.

---

## 1. Obiettivi del progetto

Il sistema sarà progettato per:

- Acquisire contenuti online e relativi metadati.
- Estrarre l'audio dai video e trascrivere il parlato.
- Identificare affermazioni verificabili (*claims*).
- Confrontare i claims con informazioni provenienti da fonti esterne.
- Individuare narrative ricorrenti attraverso analisi testuale e semantica.
- Monitorare i dati con elaborazioni batch e streaming.
- Visualizzare i risultati mediante dashboard interattive.

**Ambito dell'analisi:** il progetto si concentra sui contenuti audio e testuali, senza analisi dei frame video né OCR.

---

## 2. Stack tecnologico

| Tecnologia | Ruolo nel sistema |
|------------|-------------------|
| Docker Compose | Orchestrazione dell'infrastruttura |
| Apache Kafka | Event streaming e data ingestion |
| Apache Hadoop HDFS | Archiviazione distribuita |
| Apache Spark | Batch e stream processing |
| Elasticsearch | Indicizzazione e ricerca |
| Kibana | Data visualization e dashboard |
| Python | Acquisizione dati, trascrizione e NLP |

---

## 3. Architettura del sistema

L'architettura prevede una pipeline modulare che separa acquisizione, elaborazione, archiviazione e visualizzazione.

```text
          ONLINE VIDEO SOURCES
                   |
                   v
          DATA INGESTION (Python)
                   |
                   v
              APACHE KAFKA
                   |
          +--------+--------+
          |                 |
          v                 v
   AUDIO / NLP         APACHE SPARK
    WORKERS          Batch / Streaming
          |                 |
          +--------+--------+
                   |
          +--------+--------+
          |                 |
          v                 v
      HADOOP HDFS      ELASTICSEARCH
                            |
                            v
                          KIBANA
                            |
                            v
                        DASHBOARDS
```

**Nota:** lo schema rappresenta l'architettura prevista. Attualmente sono operativi i servizi infrastrutturali; i collegamenti applicativi saranno implementati successivamente.

---

## 4. Infrastruttura Docker

Sono configurati sette container:

| Container | Servizio |
|-----------|----------|
| `disinfo-kafka` | Kafka Broker |
| `disinfo-elasticsearch` | Elasticsearch |
| `disinfo-kibana` | Kibana |
| `disinfo-namenode` | HDFS NameNode |
| `disinfo-datanode` | HDFS DataNode |
| `disinfo-spark-master` | Spark Master |
| `disinfo-spark-worker` | Spark Worker |

### Verifiche effettuate

- [x] Avvio dei sette container
- [x] Produzione e consumo di messaggi Kafka
- [x] Avvio di Elasticsearch e Kibana
- [x] Creazione di un indice Elasticsearch
- [x] Connessione tra NameNode e DataNode
- [x] Scrittura e lettura di file su HDFS
- [x] Registrazione dello Spark Worker al Master
- [ ] Integrazione applicativa tra Kafka, Spark e HDFS
- [ ] Implementazione delle pipeline audio e NLP
- [ ] Dashboard per il monitoraggio della disinformazione

---

## 5. Installazione e avvio

### Prerequisiti

- Docker Desktop
- Docker Compose
- Git
- Risorse RAM e spazio su disco adeguati all'infrastruttura

### Clonazione

Dalla pagina GitHub del progetto, selezionare **Code → HTTPS** e copiare l'URL del repository.

```bash
git clone URL_DEL_REPOSITORY
cd disinformation-platform
```

Sostituire `URL_DEL_REPOSITORY` con l'indirizzo HTTPS effettivo del repository.

### Avvio dei servizi

```bash
docker compose up -d
```

### Verifica dei container

```bash
docker compose ps
```

### Arresto dei servizi

```bash
docker compose stop
```

I dati persistenti vengono mantenuti nei volumi Docker.

**Attenzione:** evitare `docker compose down -v`, poiché elimina anche i volumi dichiarati nel progetto.

---

## 6. Interfacce disponibili

| Servizio | Indirizzo locale |
|----------|------------------|
| Elasticsearch | http://localhost:9200 |
| Kibana | http://localhost:5601 |
| HDFS NameNode | http://localhost:9870 |
| HDFS DataNode | http://localhost:9864 |
| Spark Master | http://localhost:8080 |
| Spark Worker | http://localhost:8081 |

Kafka è accessibile dall'host tramite `localhost:9094`.

Le interfacce sono disponibili dopo l'avvio dei container e sono configurate per l'accesso locale.

---

## 7. Verifica dell'infrastruttura

### Hadoop HDFS

Per verificare lo stato del file system distribuito:

```bash
docker exec disinfo-namenode hdfs dfsadmin -report
```

Risultato atteso:

```text
Live datanodes (1):
```

### Apache Spark

Aprire nel browser:

http://localhost:8080

Verificare la presenza di un Worker con stato `ALIVE`.

### Apache Kafka

Per visualizzare i topic disponibili:

```bash
docker exec disinfo-kafka /opt/kafka/bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

Il topic iniziale utilizzato per i test è:

```text
video.discovered
```

---

## 8. Organizzazione del repository

```text
disinformation-platform/
|
|-- docker-compose.yml
|-- hadoop-conf/
|   |-- core-site.xml
|   `-- hdfs-site.xml
|
|-- data/
|   |-- raw/
|   |-- processed/
|   `-- reference/
|
|-- README.md
`-- .gitignore
```

Nelle successive fasi di sviluppo saranno aggiunti i moduli per ingestion, audio processing, NLP, Spark e visualizzazione.

---

## 9. Sviluppi futuri

Le prossime attività riguarderanno:

1. **Data Ingestion:** acquisizione di video e metadati da fonti autorizzate.
2. **Audio Processing:** estrazione audio e trascrizione automatica.
3. **Claim Extraction:** individuazione di affermazioni verificabili.
4. **Evidence Retrieval:** recupero e confronto con fonti informative esterne.
5. **Big Data Processing:** integrazione di Kafka, Spark e HDFS.
6. **Search & Analytics:** indicizzazione e analisi dei contenuti tramite Elasticsearch.
7. **Data Visualization:** sviluppo di dashboard in Kibana.
8. **Evaluation:** test di correttezza, prestazioni e qualità dei risultati.

---

## 10. Note tecniche

L'infrastruttura è configurata per uso **locale e didattico**.

- Elasticsearch utilizza una configurazione di sviluppo senza autenticazione.
- Le porte dei servizi sono esposte sull'interfaccia locale.
- HDFS utilizza un Replication Factor pari a 1.
- Il sistema non è configurato per ambienti di produzione.
- Le elaborazioni applicative non sono ancora implementate.

I risultati del futuro sistema di analisi non dovranno essere interpretati automaticamente come verdetti sulla veridicità dei contenuti: ogni valutazione dovrà considerare le evidenze disponibili e i limiti dei modelli utilizzati.

---

**Project Status:** Infrastructure Ready — Application Development Pending


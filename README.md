

\# Disinformation Monitoring Platform



\## 1. Descrizione



Progetto sviluppato per il corso di Information Systems for Big Data.



L'obiettivo è realizzare una piattaforma per il monitoraggio della disinformazione attraverso l'analisi di contenuti audio provenienti da video online.



La piattaforma sarà progettata per:

\- raccogliere contenuti e metadati;

\- estrarre l'audio e trascrivere il parlato;

\- individuare affermazioni verificabili (claims);

\- confrontare i claims con fonti esterne;

\- analizzare la diffusione di narrative ricorrenti;

\- visualizzare i risultati attraverso dashboard.



Il progetto non prevede l'analisi dei frame video né l'utilizzo di OCR.



\## 2. Tecnologie



| Tecnologia | Funzione |

|---|---|

| Docker Compose | Orchestrazione dei servizi |

| Apache Kafka | Data ingestion e streaming |

| Hadoop HDFS | Archiviazione distribuita |

| Apache Spark | Batch processing e stream processing |

| Elasticsearch | Indicizzazione e ricerca |

| Kibana | Dashboard e visualizzazione |

| Python | Ingestion, trascrizione e analisi NLP |



\## 3. Architettura prevista



La pipeline applicativa sarà sviluppata nelle fasi successive.



Flusso indicativo:



1\. Acquisizione dei contenuti e dei metadati.

2\. Pubblicazione degli eventi su Kafka.

3\. Elaborazione audio e trascrizione.

4\. Memorizzazione dei dati in HDFS.

5\. Elaborazioni batch e streaming con Spark.

6\. Indicizzazione dei risultati in Elasticsearch.

7\. Visualizzazione con Kibana.



\## 4. Struttura del repository



\- ingestion/        Acquisizione dei dati

\- workers/          Elaborazioni audio e NLP

\- spark/batch/      Elaborazioni batch

\- spark/streaming/  Elaborazioni streaming

\- elasticsearch/    Configurazioni e script

\- hadoop-conf/      Configurazione Hadoop

\- docs/             Documentazione

\- tests/            Test

\- data/             Dati locali



\## 5. Prerequisiti



\- Git

\- Docker Desktop con Docker Compose

\- Risorse RAM e disco sufficienti per eseguire i servizi



\## 6. Avvio dell'infrastruttura



Clonare il repository:



&#x20;   git clone https://github.com/USERNAME/disinformation-platform.git



Entrare nella cartella:



&#x20;   cd disinformation-platform



Avviare i servizi:



&#x20;   docker compose up -d



Verificare lo stato:



&#x20;   docker compose ps



Arrestare i servizi:



&#x20;   docker compose stop



Non usare docker compose down -v se si desidera

conservare i dati presenti nei volumi Docker.



\## 7. Interfacce disponibili



| Servizio | Indirizzo |

|---|---|

| Elasticsearch | http://localhost:9200 |

| Kibana | http://localhost:5601 |

| HDFS NameNode | http://localhost:9870 |

| HDFS DataNode | http://localhost:9864 |

| Spark Master | http://localhost:8080 |

| Spark Worker | http://localhost:8081 |



Kafka è accessibile dall'host su localhost:9094.



\## 8. Verifica dei servizi



Verificare HDFS:



&#x20;   docker exec disinfo-namenode hdfs dfsadmin -report



Il risultato atteso è almeno un DataNode attivo.



Verificare Spark:



Aprire http://localhost:8080 e controllare

che sia presente un Worker con stato ALIVE.



Verificare i topic Kafka:



&#x20;   docker exec disinfo-kafka /opt/kafka/bin/kafka-topics.sh --list --bootstrap-server localhost:9092



Per creare il topic iniziale, se assente:



&#x20;   docker exec disinfo-kafka /opt/kafka/bin/kafka-topics.sh --create --if-not-exists --topic video.discovered --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1



Gli indici Elasticsearch utilizzati dall'applicazione

dovranno essere configurati durante lo sviluppo.



\## 9. Stato del progetto



Completato:

\- Configurazione Docker Compose

\- Avvio di Kafka

\- Avvio di Elasticsearch e Kibana

\- Configurazione e verifica di HDFS

\- Configurazione del cluster Spark Standalone



Da sviluppare:

\- Pipeline di ingestion

\- Elaborazione e trascrizione audio

\- Estrazione dei claims

\- Ricerca semantica e confronto con fonti esterne

\- Elaborazioni Spark batch e streaming

\- Indicizzazione automatica in Elasticsearch

\- Dashboard Kibana

\- Test e valutazione dei risultati



L'infrastruttura è operativa, ma le pipeline

applicative non sono ancora implementate.



\## 10. Note



L'ambiente è destinato allo sviluppo locale.

La sicurezza di Elasticsearch è disabilitata:

la configurazione non deve essere esposta pubblicamente.



Le fonti e i dati saranno raccolti rispettando

le condizioni di accesso e utilizzo delle piattaforme.




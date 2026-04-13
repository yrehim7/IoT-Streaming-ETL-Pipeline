# Performance Optimierung und Datenqualitätsmanagement
von ETL-Pipelines für IoT Daten — Abschlussarbeit

Streaming-ETL-Pipeline zur Verarbeitung von IoT-Sensordaten mit Databricks, Apache Kafka und Delta Lake nach der Medallion-Architektur.

## Architektur

```
wokwi.com (5 Sensoren) → MQTT → HiveMQ → Kafka Bridge → Confluent Cloud → Databricks Structured Streaming
                                                                              ├── Bronze (Rohdaten)
                                                                              ├── Silver (Validiert)
                                                                              └── Gold   (Aggregiert)
```

## Technologie-Stack

| Komponente | Technologie |
|---|---|
| IoT-Simulation | wokwi.com (5 virtuelle Sensoren) |
| MQTT-Broker | HiveMQ + Kafka-Bridge |
| Streaming | Apache Kafka (Confluent Cloud, europe-west3) |
| Verarbeitung | PySpark Structured Streaming (Databricks) |
| Speicherung | Delta Lake (Unity Catalog) |
| Qualitätsprüfung | PySpark-nativ, 5 ISO/IEC-25012-Dimensionen |
| Visualisierung | Matplotlib |

## Notebooks

| Nr. | Notebook | Beschreibung |
|---|---|---|
| 01 | Bronze Layer — Rohdaten | Kafka-Consumer, Rohdaten-Ingestion |
| 02 | Silver Layer — Validierung | Datenqualitätsprüfung nach ISO/IEC-25012 |
| 03 | Gold Layer — Aggregation | Stündliche Aggregationen, Business-KPIs |
| 04 | Performance Optimization | OPTIMIZE, Z-Ordering, VACUUM |
| 05 | Performance Evaluation | Latenz- und Durchsatzmessung |


## Ergebnisse

- **350.403** Rohdatensätze verarbeitet (Bronze)
- **350.388** validierte Datensätze (Silver) — Qualitäts-Score: **99,996 %**
- **Durchsatz:** 127,8 Records/sek (350.403 Records in ~46 Min)
- **Echtzeit-Latenz:** ~16 Min (Steady-State), 0 sek (Minimum)
- **Datenqualität:** 5/5 ISO/IEC-25012-Dimensionen geprüft

## Datenqualitäts-Framework

Fünf Qualitätsdimensionen nach ISO/IEC-25012:

1. **Vollständigkeit** — NULL-Prüfung aller Pflichtfelder
2. **Genauigkeit** — Wertebereiche: Temperatur ∈ [-50, 60] °C, Feuchtigkeit ∈ [0, 100] %
3. **Konsistenz** — Zeitstempelvalidierung (ISO 8601)
4. **Gültigkeit** — Schema-Validierung
5. **Aktualität** — Latenzberechnung (Event → Processing)

## Delta-Lake-Tabellen

```
databricks_workspace.default.iot_data_bronze          — 350.403 Datensätze
databricks_workspace.default.iot_data_silver          — 350.388 Datensätze
databricks_workspace.default.iot_data_gold            — 350.388 Datensätze
databricks_workspace.default.iot_data_quality_metrics — 1 Datensatz
```

## Autor
Youssef Abdelrahim
Abschlussarbeit — Hochschule Merseburg, 2026

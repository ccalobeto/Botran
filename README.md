
---

# 🧩 Plan de Caso de Uso: Analytics para Ingenio Azucarero

**Dirigido a:** Gerente de IT

**Empresa:** Rones de Guatemala

**Objetivo general:** Implementar un ecosistema de datos integral que permita optimizar la productividad agrícola e industrial, reducir costos operativos y mejorar la toma de decisiones mediante analítica avanzada.

---

## ⚙️ Etapa 1: Data Mapping

### Objetivo

Identificar, clasificar y documentar las fuentes de datos relevantes para la operación agrícola, industrial y administrativa del ingenio.

### Fuentes de datos

| Área              | Fuente de datos                                              | Tipo de dato          | Frecuencia     | Observaciones                                |
| ----------------- | ------------------------------------------------------------ | --------------------- | -------------- | -------------------------------------------- |
| **Agrícola**      | Sensores IoT de humedad, pluviómetros, estaciones climáticas | Numérico, tiempo real | Horaria        | Necesario para optimizar riego y cosecha     |
| **Producción**    | SCADA y PLCs de molienda, calderas y centrifugado            | Numérico, eventos     | Minutal        | Permite calcular eficiencia de molienda      |
| **Mantenimiento** | Sistema ERP (SAP)                                   | Registros históricos  | Diario         | Control de paradas programadas y fallas      |
| **Logística**     | GPS de transporte de caña                                    | Geolocalización       | En tiempo real | Seguimiento del transporte y tiempos muertos |
| **Finanzas**      | ERP / Contabilidad                                           | Transaccional         | Diario         | Para análisis de costo por tonelada          |
| **RRHH**          | Asistencia y rendimiento laboral                             | Categórico / numérico | Diario         | Evaluar eficiencia de mano de obra           |

### Gobernanza de datos

* **Data Owners:** Jefes de cada área (Agrícola, Producción, Finanzas, etc.)
* **Data Steward:** Equipo de IT (responsable de calidad y catálogo)
* **SLA:** Actualización diaria de datasets críticos; mensual para históricos.
* **Política de calidad:** Validación de datos antes de la carga al Data Lake.

---

## 🗄️ Etapa 2: Diseño del Data Lake

### Objetivo

Centralizar y estructurar los datos de toda la operación en una arquitectura escalable y gobernada.

### Arquitectura propuesta

```plaintext
Fuentes → Ingesta → Data Lake (Raw, Curated, Trusted) → Warehouse → BI / ML
```

**Componentes sugeridos:**

* **Ingesta:** Apache Airflow / Kestra/ Cloud Composer
* **Storage:** Google Cloud Storage / AWS S3
* **Procesamiento:** Polars/ PySpark / dbt / Dataflow
* **Catálogo:** Data Catalog / Glue
* **Warehouse:** BigQuery / Snowflake
* **Visualización:** Power BI / Observablehq / Looker

### Esquema del Data Lake

| Capa        | Descripción                            | Ejemplo de datasets                                      |
| ----------- | -------------------------------------- | -------------------------------------------------------- |
| **Raw**     | Datos brutos desde IoT, ERP, GPS, etc. | `sensor_humedad_raw`, `produccion_diaria_raw`            |
| **Curated** | Datos limpios y normalizados           | `molienda_diaria_curated`, `clima_curated`               |
| **Trusted** | Datasets listos para analítica         | `rendimiento_agricola_trusted`, `costo_tonelada_trusted` |

### 📏 Buenas Prácticas Técnicas

* **Particionamiento:** por `fecha/plant_id/lote`.
* **Compresión:** Parquet + Snappy.
* **Retención:** raw 90 días, silver 3 años, gold 7 años.
* **Backups:** para históricos.

### ⚙️ Gobernanza

* **Asignar un data owner por dataset.**

  * Ejemplo: `lab_quality_results_raw` → Jefe de laboratorio.
* **Validar SLA con operaciones.**

  * Ejemplo: “Datos disponibles antes de las 08:00 AM diario.”
* **Auditoría y lineage:** registrar transformaciones y accesos.

### Mas ejemplos de Gobernanza

| Dataset               | SLA definido                                          | Responsable      | Frecuencia         |
| --------------------- | ----------------------------------------------------- | ---------------- | ------------------ |
| `milling_metrics_curated`     | Datos disponibles < 5 min después del evento          | TI + Operaciones | Streaming continuo |
| `lab_quality_results_raw` | Datos del día anterior disponibles antes de 8:00 a.m. | Laboratorio      | Diario             |
| `maintenance_events_trusted`  | 100% de fallas reportadas con causa dentro de 48h     | Mantenimiento    | Semanal            |

### 🧱 Entregables

* Diagrama de arquitectura.
* Plantillas ETL (Airflow / dbt DAGs ).
* Políticas de acceso, calidad y retención.
* Catálogo inicial con 10 datasets prioritarios.

---

## 📊 Etapa 3: Modelo Analítico

### Objetivo

Implementar un modelo analítico que permita medir o predecir la eficiencia operativa y detectar oportunidades de mejora en alguno de los Principales KPIs.

### Principales KPIs

| Área              | KPI                               | Descripción                                | Fuente                  |
| ----------------- | --------------------------------- | ------------------------------------------ | ----------------------- |
| **Agrícola**      | Rendimiento por hectárea (TCH)    | Toneladas de caña / hectárea               | ERP agrícola |
| **Industrial**    | Eficiencia de molienda (%)        | (Azúcar recuperada / caña molida) * 100    |  Producción      |
| **Logística**     | Tiempo promedio de traslado (min) | Desde campo hasta fábrica                  | GPS de transporte       |
| **Energía**       | Consumo kWh/ton caña              | Eficiencia energética por unidad procesada | PLCs         |
| **Mantenimiento** | MTBF / MTTR                       | Tiempo medio entre fallas / de reparación  | ERP                     |
| **Financiero**    | Costo por tonelada procesada      | Costos totales / tonelada                  | Contabilidad            |
| **RRHH**          | Productividad laboral             | Toneladas / trabajador                     | RRHH              |

### Otras Variables clave

* Producción diaria de caña (toneladas)
* Rendimiento de molienda (ton/hora)
* Nivel de azúcar recuperado (%)
* Humedad y temperatura promedio del campo
* Eficiencia energética
* Paradas no programadas (número y duración)
* Comparativo de productividad por campo y equipo

### 📦 Entregables

* Notebooks / scripts EDA.
* Integración con Power BI / Observablehq.

#### Herramientas de desarrollo

* Github Ecosystem (Enterprise, Codespaces, Copilot, Models y Project)
* DBT
* Kestra / Airflow
* Google GCP Account and Privileges
* Power BI (incuida licencia Parallels)
* Observablehq

## 📅 Cronograma general (resumen)

| Etapa                       | Semanas | Entregables clave                        |
| --------------------------- | ------- | ---------------------------------------- |
| **1. Data Mapping**         | 5 - 7     | Inventario de datos, gobernanza, calidad |
| **2. Diseño del Data Lake** | 8 - 12   | Arquitectura y pipelines productivos     |
| **3. Modelo Analítico**     | 13 - 16  | 1 Modelo Piloto        |

## 💰 Costos de Implementación del Proyecto

### **Etapa 1: Data Mapping (Semanas 5–7)**

* Licencias y herramientas de catalogación: **USD 1,000**
* Capacitación inicial(costeo por estar fuera de labores): **USD 2,000**
* **Total estimado:** **USD 3,000**

### **Etapa 2: Diseño del Data Lake (Semanas 8–12)**

* Infraestructura Cloud (GCP / AWS): **USD 1,000**
* Desarrollo de pipeline y QA: **USD 0.0**
* Licencias y monitoreo: **USD 1,000**
* **Total estimado:** **USD 2,000**

### **Etapa 3: Modelo Analítico y Dashboard (Semanas 13–16)**

* Licencias BI (Power BI / Looker): **USD 200**
* Desarrollo de modelo y dashboard: **USD 0.0**
* Capacitación de usuarios(costeo por estar fuera de labores): **USD 1,000**
* **Total estimado:** **USD 1,200**

### **Consultoria y análisis de datos**: **USD 6,000**

### **📊 Costo Total del Proyecto (16 semanas): ≈ USD 12,200**

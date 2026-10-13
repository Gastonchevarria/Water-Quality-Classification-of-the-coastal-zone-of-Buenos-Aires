# Water Quality Classification of the Coastal Zone of Buenos Aires 🌊🔬

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python: 3.8+](https://img.shields.io/badge/Python-3.8+-3776AB.svg?logo=python&logoColor=white)](https://python.org)
[![Dataset: RIIGLO / MAyDS](https://img.shields.io/badge/Data-RIIGLO%20%2F%20MAyDS%20Argentina-10B981.svg)]()
[![Method: PCA + K--Means](https://img.shields.io/badge/Methodology-PCA%20%2B%20K--Means-orange.svg)]()
[![Domain: Environmental Engineering](https://img.shields.io/badge/Domain-Limnology%20%26%20Biochemical%20Monitoring-0284C7.svg)]()

> **Monitoreo Ambiental y Machine Learning Aplicado a Cuencas Hídricas de la República Argentina**  
> *Clasificación Multivariada de Calidad de Agua en la Franja Costera Sur del Río de la Plata mediante Reducción de Dimensionalidad (PCA) y Agrupamiento No Supervisado (K-Means).*

---

## 👨‍🔬 Dirección Científica y Autoría

- **Investigador Principal:** **Ing. Químico Gastón Chevarria**
- **Filiación Académica / Institucional:** Investigador Independiente · Especialista en Arquitectura de Sistemas de Inteligencia Artificial & GCP.  
  Formación de grado y posgrado: **Universidad Tecnológica Nacional (UTN)** · **Universidad Nacional de San Martín (UNSAM)** · **Instituto Data Science**.  
  GitHub: [@Gastonchevarria](https://github.com/Gastonchevarria) · LinkedIn: [/in/gastonchevarria-b9902a46](https://linkedin.com/in/gastonchevarria-b9902a46)
- **Ecosistema de Investigación Ambiental y Teledetección:**  
  Este proyecto constituye la base limnológica y fisicoquímica de los antecedentes de investigación de Gastón Chevarria en monitoreo de cuerpos de agua, complementando la plataforma de teledetección satelital de radar orbital [OceanEye (Sentinel-1 SAR)](https://github.com/Gastonchevarria/oceaneye-evaluation).

---

## 🧭 Resumen Ejecutivo y Propósito del Proyecto

La franja costera del Río de la Plata constituye una de las fuentes de captación de agua dulce más estratégicas y a la vez vulnerables del Cono Sur, abasteciendo a más de 14 millones de habitantes del Área Metropolitana de Buenos Aires (AMBA) y recibiendo descargas antrópicas e industriales de alta densidad.

El presente proyecto implementa un pipeline integral de ciencia de datos y modelado no supervisado para:
1. **Auditar y tipificar la calidad ambiental** de los cuerpos de agua costeros a lo largo de una serie temporal de 9 años (2013–2022).
2. **Desentrañar la correlación biogeoquímica** entre nutrientes nitrogenados ($NH_4^+$, $NO_3^-$), demanda de oxígeno ($DBO_5$, $DQO$), oxígeno disuelto ($OD$) y bioindicadores patógenos entéricos (*Escherichia coli*, Coliformes fecales, Enterococos).
3. **Reducir la dimensionalidad fisicoquímica** de 10 variables a 6 componentes ortogonales principales (PCA) reteniendo más del **81.3% de la varianza explicada acumulada**.
4. **Segmentar matemáticamente el régimen hídrico** en 6 estados de degradación mediante K-Means ($k=6$), permitiendo a los municipios y autoridades ambientales alertas tempranas ante anomalías críticas de contaminación.

---

## 📊 Especificación de Datos y Área de Estudio

### Fuente de Datos
Los datos provienen del relevamiento sistemático y estacional coordinado por el **Ministerio de Ambiente y Desarrollo Sostenible de la Nación (MAyDS)** en conjunto con la **Red de Intercambio de Información de Gobiernos Locales (RIIGLO)**.

* **Archivo de Datos:** `Dataset_agua_Buenos_Aires.csv` (formato delimitado por punto y coma `;`, codificación Latin-1).
* **Horizonte Temporal:** Muestreos trimestrales continuos entre **2013 y 2022** (643 registros analíticos consolidados).
* **Zonas y Puntos de Muestreo (13 regiones críticas):**
  * **Municipios Costeros:** Avellaneda, Quilmes, Berazategui, Ensenada, Berisso, Zona Norte.
  * **Tipologías Hídricas:** Arroyos tributarios, Ríos interiores, Balnearios, Canales aliviadores, Costaneras urbanas, Puertos y Reservas ecológicas, Playas públicas y Tomas de captación de agua potable.

### Variables Fisicoquímicas y Microbiológicas

| Variable | Notación | Unidad | Significado e Importancia Ambiental |
|---|---|---|---|
| **Temperatura del Agua** | `Tem_agua` | °C | Modula la solubilidad de gases ($O_2$) y la cinética bacteriana |
| **Potencial de Hidrógeno** | `pH` | Unidades pH | Equilibrio ácido-base del ecosistema acuático (rango óptimo 6.5–8.5) |
| **Oxígeno Disuelto** | `OD` | mg/L | Indicador primario de habitabilidad aeróbica y asimilación biológica |
| **Demanda Bioquímica de Oxígeno** | `DBO_mg_l` | mg/L | Medida del contenido de materia orgánica biodegradable (fracción biológica) |
| **Demanda Química de Oxígeno** | `DQO_mg_l` | mg/L | Contenido total de materia orgánica oxidable químicamente |
| **Amonio** | `NH4_mg_l` | mg/L | Indicador de descomposición orgánica reciente y efluentes cloacales |
| **Nitratos** | `Nitrato_mg_l` | mg/L | Etapa final de nitrificación; contaminante agrícola e industrial |
| **Coliformes Totales** | `colif_totales_ufc_100ml` | UFC/100 mL | Bioindicador general de contaminación biológica |
| **Escherichia coli** | `escher_coli_ufc_100ml` | UFC/100 mL | Patógeno de origen fecal termotolerante directo |
| **Enterococos** | `enteroc_ufc_100ml` | UFC/100 mL | Indicador de persistencia bacteriana en ambientes acuáticos y salobres |

---

## 📐 Fundamentos Teóricos y Modelado Matemático

### 1. Imputación Bioquímica Condicional y Ratios Estequiométricos
A diferencia de técnicas genéricas de imputación estadística ciega, el preprocesamiento incorpora **criterios de ingeniería de procesos químicos y limnología aplicada**:
- **Biodegradabilidad por relación DQO/DBO:**
  $$\text{Fracción Biológica} = \frac{\text{DQO}}{\text{DBO}}$$
  * $\frac{\text{DQO}}{\text{DBO}} < 2.5$: Efluente de elevada biodegradabilidad.
  * $2.5 \le \frac{\text{DQO}}{\text{DBO}} \le 4.0$: Biodegradabilidad media típica de aguas mixtas.
  * $\frac{\text{DQO}}{\text{DBO}} > 4.0$: Fracción inerte o inhibida con predominio de contaminantes químicos no biodegradables.
- **Dinámica de Nitrificación y Agotamiento de $O_2$:**
  $$NH_4^+ + 2O_2 \xrightarrow{\text{bacterias}} NO_3^- + H_2O + 2H^+$$
  La correlación negativa observada entre el amonio y el oxígeno disuelto confirma la demanda de oxígeno bentónico y la eutrofización inducida.

### 2. Estandarización de Variables (Z-Score)
Dada la disparidad de magnitudes entre unidades microbiológicas ($> 10^7 \text{ UFC/100mL}$) y fisicoquímicas ($OD \sim 0\text{--}10 \text{ mg/L}$), se aplica transformación de puntuación estándar para evitar distorsiones euclidianas:

$$z = \frac{x - \mu}{\sigma}$$

### 3. Reducción de Dimensionalidad por Componentes Principales (PCA)
Se calcula la matriz de covarianza de las características estandarizadas $\mathbf{\Sigma} = \frac{1}{n} \mathbf{X}^T \mathbf{X}$ y su descomposición en autovalores y autovectores:

$$\mathbf{\Sigma} \mathbf{v}_i = \lambda_i \mathbf{v}_i$$

La varianza acumulada de las 6 componentes seleccionadas explica el **81.33%** de la información del sistema:
- **PC1 (22.42%):** Carga microbiana y coliformes (*High Biological Contamination*). Cargas positivas dominantes en Coliformes ($+0.59$), Enterococos ($+0.62$) y $E. coli$ ($+0.31$), con carga negativa en OD ($-0.26$).
- **PC2 (15.57%):** Balance oxidativo y fotosintético (*Incipient Contamination / Aeration*). Alta carga en OD ($+0.60$) y pH ($+0.59$).
- **PC3 (12.41%):** Desoxigenación por nitrógeno amoniacal. Carga fuertemente negativa en $NH_4$ ($-0.70$).
- **PC4 (11.53%):** Carga química refractaria. Dominado por DQO ($+0.70$) y Nitratos ($+0.54$).
- **PC5 (10.63%):** Carga mixta DBO/Nitratos. Fuerte contribución de DBO ($+0.71$) y Nitratos ($+0.53$).
- **PC6 (9.16%):** Peligro biológico específico por *E. coli* ($+0.75$).

### 4. Agrupamiento K-Means sobre el Espacio Latente
El modelo K-Means particiona las $N$ observaciones transformadas $\mathbf{z}_i \in \mathbb{R}^6$ minimizando la suma de cuadrados intra-cluster (WCSS):

$$\arg\min_{\mathbf{S}} \sum_{k=1}^{K} \sum_{\mathbf{z}_i \in S_k} \|\mathbf{z}_i - \boldsymbol{\mu}_k\|_2^2$$

El número de conglomerados óptimo ($k=6$) se determinó mediante el método del codo (*Elbow Criterion*).

---

## 📈 Resultados del Modelo y Taxonomía de Calidad

| Cluster / Segmento | Nombre Asignado | Observaciones | Proporción | Perfil Fisicoquímico & Microbiológico Característico |
|---|---|---|---|---|
| **Cluster 0** | **Low Contamination** | 397 | **61.74%** | Calidad de fondo aceptable. $OD = 5.93 \text{ mg/L}$, $NH_4 = 0.62 \text{ mg/L}$, carga bacteriana basal. |
| **Cluster 1** | **Chemical & Biological Contamination** | 103 | **16.02%** | Efluente mixto. Amonio alto ($5.26 \text{ mg/L}$), $OD$ deprimido ($3.24 \text{ mg/L}$), coliformes en $2.3 \times 10^5$. |
| **Cluster 2** | **Incipient Contamination** | 130 | **20.22%** | Aguas bien oxigenadas ($OD = 9.16 \text{ mg/L}$), $pH = 8.02$, baja demanda bioquímica ($DBO = 8.16 \text{ mg/L}$). |
| **Cluster 3** | **Water Biological Danger** | 2 | **0.31%** | Alerta patogénica extrema. $E. coli = 2.0 \times 10^6 \text{ UFC}$, anoxia severa ($OD = 0.63 \text{ mg/L}$). |
| **Cluster 4** | **High Level of Biological Contamination** | 1 | **0.16%** | Vuelco cloacal masivo. Coliformes $= 4.6 \times 10^7 \text{ UFC}$, enterococos $= 8.0 \times 10^6 \text{ UFC}$, $OD = 0.40 \text{ mg/L}$. |
| **Cluster 5** | **High Level Chemical Contamination** | 10 | **1.56%** | Polución industrial/agrícola. Nitratos extremos ($14,088 \text{ mg/L}$), $DQO = 104.5 \text{ mg/L}$. |

### Diagnóstico Geográfico de Impacto
El cruzamiento espacial identificó las zonas con mayor frecuencia de episodios críticos:
- **Ensenada y Cuenca de Berisso:** Concentran la mayor variabilidad de vertidos biológicos e industriales y el registro de *Water Biological Danger*.
- **Arroyos y Ríos Tributarios:** Exhiben el mayor índice de degradación crónica por descargas difusas y escorrentía superficial.
- **Tomas de Captación de Agua Potable:** Mantienen prevalencia de calidad Clase *Low Contamination* e *Incipient Contamination*, pero con alertas intermitentes de carga mixta.

---

## 💻 Estructura del Repositorio y Reproducción

```text
.
├── Data Science Final Project - Water Quality Classification of the Coastal Zone of Buenos Aires  - Gaston.ipynb
├── Dataset_agua_Buenos_Aires.csv      # Dataset oficial 2013-2022 (MAyDS / RIIGLO)
├── Imagen 29-8-22 a las 12.32.jpg      # Gráfico de correlaciones
├── Imagen 29-8-22 a las 12.33.jpg      # Curva del Codo (Elbow Method K-Means)
├── Imagen 29-8-22 a las 12.34.jpg      # Heatmap de cargas factoriales PCA
├── Imagen 29-8-22 a las 12.36 (1).jpg  # Dispersión bivariada OD vs NH4
├── Imagen 29-8-22 a las 12.36 2.jpg    # Segmentación PCA-KMeans bidimensional
├── Imagen 29-8-22 a las 12.36.jpg      # Distribución de clusters por sitios
└── README.md
```

### Ejecución Rápida

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/Gastonchevarria/Water-Quality-Classification-of-the-coastal-zone-of-Buenos-Aires.git
   cd Water-Quality-Classification-of-the-coastal-zone-of-Buenos-Aires
   ```

2. Instalar dependencias científicas:
   ```bash
   pip install numpy pandas scipy scikit-learn matplotlib seaborn
   ```

3. Ejecutar el notebook analítico en Jupyter Notebook o JupyterLab:
   ```bash
   jupyter notebook "Data Science Final Project - Water Quality Classification of the Coastal Zone of Buenos Aires  - Gaston.ipynb"
   ```

---

## 🔗 Referencias y Proyectos Conexos

- [OceanEye — Evaluation & Scientific Verification Kit](https://github.com/Gastonchevarria/oceaneye-evaluation): Plataforma satelital para teledetección de buques pesqueros INDNR en el Mar Argentino mediante Sentinel-1 SAR y cadena de custodia criptográfica Ed25519.
- [ML Water Quality Prediction](https://github.com/Gastonchevarria/ML_Water_Quality_Prediction): Clasificación supervisada de potabilidad de agua potable mediante discriminante cuadrático (QDA).
- Ministerio de Ambiente y Desarrollo Sostenible (MAyDS) & Red RIIGLO: *Red de monitoreo continuo de la calidad del agua de la franja costera del Río de la Plata*.

---

## 📄 Licencia

Distribuido bajo la Licencia **MIT**. Desarrollado con fines de investigación científica, soberanía hídrica y preservación ambiental de cuencas fluviales.

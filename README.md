# CHALLENGE-ALURA-02-uwu

# 📡 TelecomX LATAM — Análisis de Evasión de Clientes (Churn)

![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0+-green?logo=pandas&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-3.7+-red)
![ETL](https://img.shields.io/badge/Pipeline-ETL-9B59B6)

## 📋 Descripción

Este proyecto forma parte del **Challenge 2 de Data Science de Alura Latam**. El enfoque principal es el desarrollo de un pipeline completo de **ETL (Extract, Transform, Load)** en Python, aplicado sobre datos reales de clientes de una empresa de telecomunicaciones ficticia: **TelecomX**.

> **Pregunta de negocio:** ¿Qué factores explican la evasión de clientes (Churn) y cómo puede TelecomX reducirla?

---

## 🗂️ Estructura del Proyecto

```
📦 telecomx-latam/
├── 📓 TelecomX_LATAM_Mejorado.ipynb   # Notebook principal con pipeline ETL completo
├── 📊 TelecomX_clean.csv               # Dataset limpio generado por el pipeline
├── 🖼️  dashboard_churn.png             # Dashboard resumen exportado
└── 📄 README.md                        # Este archivo
```

---

## 🔗 Fuente de Datos

Los datos se obtienen directamente desde la API del repositorio oficial del challenge:

🔗 [challenge2-data-science-LATAM](https://github.com/alura-cursos/challenge2-data-science-LATAM)

- **Formato:** JSON con estructura jerárquica anidada
- **Registros:** ~7,267 clientes
- **Columnas originales:** 21 (luego de normalización)

### Estructura del JSON

El JSON tiene una estructura jerárquica con 4 sub-objetos por cliente:

```json
{
  "customerID": "0002-ORFBO",
  "Churn": "No",
  "customer": { "gender": "Female", "SeniorCitizen": 0, ... },
  "phone":    { "PhoneService": "Yes", "MultipleLines": "No" },
  "internet": { "InternetService": "DSL", "OnlineSecurity": "Yes", ... },
  "account":  { "Contract": "One year", "Charges": { "Monthly": 65.6, "Total": 593.3 } }
}
```

### Columnas del Dataset Limpio

| Columna | Descripción |
|---------|-------------|
| `cliente_id` | Identificador único del cliente |
| `evasion` | Si el cliente abandonó el servicio (Yes/No) |
| `genero` | Género del cliente |
| `adulto_mayor` | Si es adulto mayor (0/1) |
| `tiene_pareja` | Si tiene pareja (Yes/No) |
| `tiene_dependientes` | Si tiene dependientes (Yes/No) |
| `meses_cliente` | Meses de antigüedad como cliente |
| `servicio_telefono` | Si tiene servicio telefónico |
| `lineas_multiples` | Si tiene múltiples líneas |
| `servicio_internet` | Tipo de internet (DSL / Fiber optic / No) |
| `seguridad_online` | Servicio de seguridad online |
| `backup_online` | Servicio de backup online |
| `proteccion_dispositivo` | Protección de dispositivo |
| `soporte_tecnico` | Soporte técnico contratado |
| `streaming_tv` | Streaming de TV |
| `streaming_peliculas` | Streaming de películas |
| `tipo_contrato` | Tipo de contrato (Month-to-month / 1 year / 2 year) |
| `factura_digital` | Si usa factura sin papel |
| `metodo_pago` | Método de pago |
| `cargo_mensual` | Cargo mensual en USD |
| `cargo_total` | Cargo total acumulado en USD |
| `cargo_diario` ⭐ | Variable derivada: cargo diario estimado |
| `segmento_cliente` ⭐ | Variable derivada: segmento por antigüedad |
| `evasion_bin` ⭐ | Variable derivada: evasión en formato binario (0/1) |
| `adulto_mayor_label` ⭐ | Variable derivada: adulto mayor en formato legible |

> ⭐ Variables creadas durante la etapa de Transformación

---

## ⚙️ Pipeline ETL

El notebook implementa las siguientes etapas:

### 📌 1. Extracción
- Obtención del JSON desde la API de GitHub con `requests`
- Inspección de la estructura jerárquica del JSON
- Normalización a DataFrame tabular con `pd.json_normalize()`

### 🔧 2. Transformación
- Renombrado de columnas (prefijos jerárquicos → nombres en español)
- Conversión de tipos de datos (`cargo_total`, `cargo_mensual`, `meses_cliente`)
- Verificación y eliminación de valores nulos
- Filtrado de registros con `evasion` vacía o inválida
- Eliminación de duplicados
- Creación de **4 variables derivadas**

### 💾 3. Carga
- Exportación del dataset limpio a `TelecomX_clean.csv`

### 📊 4. Análisis Exploratorio
- 8 análisis visuales con Matplotlib
- Dashboard resumen de 6 paneles

### 📄 5. Conclusiones
- Hallazgos clave por variable
- Recomendaciones estratégicas de negocio

---

## 📈 Principales Hallazgos

| Factor | Hallazgo | Riesgo |
|--------|----------|--------|
| **Tipo de contrato** | Month-to-month tiene ~42% de evasión | 🔴 Alto |
| **Antigüedad** | Los primeros 12 meses son el período más crítico | 🔴 Alto |
| **Internet** | Fiber Optic tiene mayor tasa de evasión que DSL | 🟠 Medio |
| **Método de pago** | Cheque electrónico asociado a mayor evasión | 🟠 Medio |
| **Servicios adicionales** | Soporte técnico y seguridad reducen la evasión | 🟢 Protector |
| **Demografía** | Adultos mayores y clientes sin pareja evaden más | 🟡 Bajo-Medio |

### 💡 Recomendaciones Estratégicas

1. **Onboarding temprano:** programa de acompañamiento activo en los primeros 6-12 meses
2. **Migración a contratos anuales:** ofrecer descuentos y beneficios exclusivos
3. **Revisión del servicio de fibra óptica:** calidad, precio y satisfacción
4. **Digitalización del pago:** incentivar el débito automático sobre el cheque electrónico
5. **Upselling de servicios:** promover paquetes de soporte técnico y seguridad digital

---

## 🚀 Cómo Ejecutar

### Requisitos

```bash
pip install pandas matplotlib requests numpy
```

### Ejecución en Google Colab (recomendado)

1. Accedé a [colab.research.google.com](https://colab.research.google.com)
2. Subí el archivo `TelecomX_LATAM_Mejorado.ipynb`
3. Ejecutá todo con: **Entorno de ejecución → Ejecutar todo**

> ⚠️ Se requiere conexión a internet para descargar los datos desde la API de GitHub.

### Ejecución local

```bash
# Clonar el repositorio del challenge
git clone https://github.com/alura-cursos/challenge2-data-science-LATAM

# Abrir el notebook
jupyter notebook TelecomX_LATAM_Mejorado.ipynb
```

---

## 🛠️ Tecnologías Utilizadas

- **Python 3.8+**
- **Pandas** — normalización, limpieza y transformación de datos
- **Requests** — extracción de datos desde API
- **Matplotlib** — visualizaciones y dashboard
- **NumPy** — operaciones numéricas auxiliares
- **Jupyter Notebook** — entorno de desarrollo interactivo

---

## 👤 Autor

Desarrollado como parte del **Challenge de Data Science — Alura Latam**.

---

## 📄 Licencia

Este proyecto es de uso educativo. Los datos pertenecen a [Alura Latam](https://www.aluracursos.com/).

# Caso 5 · SonríeValencia — Predicción de Absentismo en Citas Dentales

Repositorio del equipo para el proyecto **SonríeValencia** del módulo **Sistemas de Aprendizaje Automático (SAA)** (Curso de Especialización en Inteligencia Artificial y Big Data — IES Abastos).

El objetivo de negocio es predecir qué pacientes **no acudirán a su cita** (`no_acude`) en las clínicas dentales **SonríeValencia**, permitiendo optimizar la agenda médica y activar campañas preventivas de confirmación.

## Estructura del Repositorio

```
saa-caso5sonrievlc/
├── data/
│   ├── raw/                                          # Datasets originales sin modificar
│   │   ├── sonrievalencia_citas.csv
│   │   └── sonrievalencia_historial_desplazamiento.csv
│   └── processed/                                    # Datasets limpios, transformados y vistas minables
│       ├── sonrievalencia_citas_limpio.csv
│       ├── sonrievalencia_citas_transformado.csv
│       └── sonrievalencia_citas_final.csv
├── practica_01_preparacionYlimpieza/
│   ├── docs/
│   │   └── Exposicion_SonrieValencia_preguntas.html  # Presentación de defensa de la Práctica 1
│   └── notebooks/
│       ├── caso05_1cargaYvistazo.ipynb               # Carga inicial y exploración (EDA)
│       ├── caso05_2visualizacionYanalisis.ipynb      # Análisis gráfico y relaciones con el objetivo
│       ├── caso05_3limpiezaDeDatos.ipynb             # Tratamiento de nulos, errores y outliers
│       ├── caso05_4transformacion.ipynb              # Codificación de variables categóricas e ingeniería de características
│       ├── caso05_5limpieza.ipynb                    # Validación final y exportación de la vista minable (con id_cita)
│       └── caso05_6analisisFinal.ipynb               # Conclusiones y verificación post-limpieza
├── practica_02_seleccion_pca/                        # (Próxima fase: Selección de variables, Escalado, PCA y Modelado)
├── .gitignore
├── LICENSE
└── requirements.txt

```

## Fases del Proyecto

### Fase 1 · Preparación y Limpieza de Datos (`practica_01_preparacionYlimpieza`)

* **Dataset:** `data/raw/sonrievalencia_citas.csv` (2.400 registros de citas).

* **Tareas realizadas:**

  * Auditoría de calidad de datos, detección de anomalías y manejo de valores nulos.

  * Transformación y codificación numérica de variables categóricas.

  * Generación de la vista minable (`data/processed/sonrievalencia_citas_final.csv`) manteniendo la clave primaria `id_cita` para garantizar la trazabilidad en cruces posteriores.

### Fase 2 · Selección de Variables, PCA y Modelado (`practica_02_seleccion_pca`)

* **Dataset adicional:** `data/raw/sonrievalencia_historial_desplazamiento.csv` (2.400 registros × 13 variables continuas sobre historial de puntualidad y desplazamiento).

* **Flujo de trabajo:**

  1. **Unión de datos (`merge`):** Cruce `one_to_one` mediante `id_cita` entre la vista minable de la Práctica 1 y el historial de desplazamiento.

  2. **Filtrado de variables:** Eliminación de variables casi constantes (`VarianceThreshold`), redundantes (`|r| >= 0.9`) y sin relación con `no_acude` (`SelectKBest`).

  3. **Escalado y PCA:** Estandarización (`StandardScaler`) y extracción de componentes principales interpretables.

  4. **Comparación de modelos:** Evaluación mediante `Pipeline` con `LogisticRegression(class_weight='balanced')` comparando Recall, F1 y AUC.

## Instalación y Uso

1. **Clonar el repositorio:**

   ```
   git clone <URL_DEL_REPOSITORIO>
   cd saa-caso5sonrievlc
   
   ```

2. **Crear y activar el entorno virtual:**

   ```
   python -m venv .venv
   # En Windows:
   .venv\Scripts\activate
   # En Linux / macOS:
   source .venv/bin/activate
   
   ```

3. **Instalar dependencias:**

   ```
   pip install -r requirements.txt
   
   ```

## Convenciones del Equipo

* **Datos inmutables:** Nunca modificar ni sobreescribir los archivos dentro de `data/raw/`.

* **Rutas relativas:** Todos los notebooks apuntan a `../../data/...` desde su carpeta `notebooks/`.

* **Prevención de fuga de datos (*Data Leakage*):** Cualquier escalador, selector o transformación PCA se ajusta (`fit`) exclusivamente sobre el conjunto de entrenamiento (`X_train`).
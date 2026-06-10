# WineArtGAN — Generador de Arte Abstracto para Etiquetas de Vino

## Descripción del proyecto

**WineArtGAN** es un proyecto desarrollado para el Trabajo Práctico Integrador de la materia **Redes Neuronales Profundas**.

El objetivo del proyecto es entrenar un modelo generativo capaz de crear imágenes de arte abstracto inéditas, pensadas como inspiración visual para etiquetas de vino. La aplicación final permitirá generar una imagen abstracta y visualizarla aplicada sobre un mockup digital de una botella de vino.

El proyecto aborda una tarea de **generación de imágenes**, utilizando un dataset de obras de arte abstracto. A diferencia de un problema de clasificación, las imágenes no poseen etiquetas por clase, ya que el modelo aprende la distribución visual general del conjunto de datos.

---

## Objetivo

El objetivo principal es construir un pipeline completo de trabajo con redes neuronales profundas, incluyendo:

- Descarga y organización del dataset.
- Preparación del dataset en PyTorch.
- Particionado en train, validación y test.
- Preprocesamiento de imágenes.
- Aplicación de data augmentation.
- Entrenamiento de un modelo generativo.
- Integración del modelo en una aplicación web.
- Despliegue de la aplicación final.

---

## Dataset

El dataset utilizado es **Abstract Art Gallery**, disponible públicamente en Kaggle.

**Fuente:**  
https://www.kaggle.com/datasets/bryanb/abstract-art-gallery

El dataset contiene imágenes de arte abstracto en formato `.jpg`, organizadas en carpetas. Como el problema es generativo, no se utilizan etiquetas de clasificación.

En el notebook se descarga automáticamente mediante `kagglehub`, por lo que las imágenes originales no se suben al repositorio.

---

## Estructura del repositorio

```text
WineArtGAN/
│
├── data/
│   ├── README.md
│   └── raw/
│       └── abstract-art-gallery/  <-- Descargado localmente
│
├── dev/
│   └── dev_01_dataset_preparation.ipynb
│
├── prod/
│   └── .gitkeep
│
├── .gitignore
└── README.md
```

---

## Descripción de carpetas

### `data/`

Contiene información relacionada con el dataset.

Incluye:

- `README.md`: descripción del dataset y forma de descarga.
- `raw/`: carpeta donde se descargan y almacenan las imágenes originales.

Las imágenes originales no se versionan en GitHub para evitar subir archivos pesados.

---

### `dev/`

Contiene los notebooks de desarrollo experimental.

Actualmente incluye:

- `dev_01_dataset_preparation.ipynb`: notebook de preparación del dataset, carga de imágenes y verificación de DataLoaders.

Más adelante se agregarán notebooks de entrenamiento y evaluación del modelo.

---

### `prod/`

Contendrá los archivos de la aplicación web final.

En esta etapa puede estar vacío o contener un archivo `.gitkeep`.

Más adelante incluirá:

- `app.py`: aplicación principal en Streamlit.
- `utils.py`: funciones auxiliares para cargar el modelo, generar imágenes y armar el mockup de botella.
- `requirements.txt`: dependencias necesarias para ejecutar la aplicación.
- `README.md`: instrucciones específicas de uso de la app.

---

## Preparación del dataset

El dataset se descarga desde Kaggle utilizando `kagglehub`.

En el notebook se realiza el siguiente flujo:

1. Instalación de `kagglehub`.
2. Descarga automática del dataset.
3. Inspección de carpetas y archivos.
4. Carga de imágenes `.jpg`.
5. Implementación de una clase personalizada de PyTorch heredando de `torch.utils.data.Dataset`.
6. Configuración de un único conjunto para entrenamiento (sin particionado).
7. Creación de DataLoader con mezcla de datos (`shuffle=True`).
8. Aplicación de preprocesamiento (redimensionado a 128x128 y conversión a tensor).
9. Verificación final de batches procesados.

---

## Particionado del dataset

A diferencia de los problemas de clasificación supervisada, en este proyecto **no dividiremos el dataset en conjuntos de entrenamiento, validación y test**. Se utiliza el **100% de las imágenes disponibles para el entrenamiento** del modelo generativo. 

Esto se debe a que en tareas generativas no se realiza una evaluación cuantitativa directa sobre datos no vistos durante el entrenamiento (como el cálculo de precisión/accuracy), y es prioritario maximizar la cantidad de datos para que el modelo aprenda la distribución visual del arte abstracto.

Para garantizar la reproducibilidad del entrenamiento y de los resultados generados, se definió una semilla global:
```python
SEED = 42
```

---

## Preprocesamiento

Las imágenes son preprocesadas antes de ingresar al modelo.

Los pasos principales son:

- Conversión a RGB.
- Redimensionado a `128x128` píxeles para conservar una mayor nitidez y calidad visual en las etiquetas generadas.
- Conversión a tensor (rango `[0, 1]`). No se aplica normalización adicional al rango `[-1, 1]` para mantener un procesamiento directo de los píxeles.

---

## Data augmentation

**No se aplica aumento de datos (data augmentation)** en este pipeline. Se prescinde de transformaciones como rotaciones aleatorias porque introducen bordes o esquinas negras en los tensores de las imágenes, lo cual haría que el generador aprenda a producir manchas negras falsas en el arte abstracto generado. Asimismo, contamos con una cantidad de datos suficiente para el entrenamiento sin necesidad de generar variantes artificiales.

---

## Modelo propuesto

La arquitectura inicial propuesta es una red generativa tipo **DCGAN**.

Se eligió esta arquitectura porque es adecuada para comenzar con generación de imágenes, tiene una implementación relativamente simple en PyTorch y permite entrenar con imágenes de tamaño reducido, como `64x64`.

El modelo deberá aprender la distribución visual del dataset y generar nuevas imágenes abstractas a partir de un vector latente aleatorio.

---

## Aplicación web propuesta

La aplicación final será desarrollada con **Streamlit**.

La interacción esperada será:

1. El usuario presiona un botón para generar una nueva imagen.
2. El modelo genera una obra abstracta.
3. La imagen generada se muestra en pantalla.
4. La app superpone la imagen sobre un mockup de botella de vino, simulando una etiqueta.
5. El usuario puede generar nuevas variantes.

---

## Instalación y ejecución del notebook

Para reproducir el notebook en una máquina nueva:

1. Clonar el repositorio:

```bash
git clone https://github.com/Rami195/WineArtGAN.git
cd WineArtGAN
```

2. Crear el entorno virtual:

```bash
python -m venv .venv
```

3. Activar el entorno virtual:

```powershell
.\.venv\Scripts\Activate.ps1
```

4. Actualizar pip:

```bash
python -m pip install --upgrade pip
```

5. Instalar dependencias:

```bash
python -m pip install kagglehub torch torchvision matplotlib scikit-learn pandas pillow jupyterlab nbconvert ipykernel
```

6. Ejecutar el notebook:

```text
dev/dev_01_dataset_preparation.ipynb
```

El notebook descarga automáticamente el dataset desde Kaggle y prepara los archivos necesarios.

---


## Integrantes

- Ariza, Alvaro
- Cabrero, Daniel
- Hansen, Matias
- Martinez, Ramiro
- Quiros, Lautaro

---

## Materia

**Redes Neuronales Profundas**  
Ingeniería en Sistemas de Información

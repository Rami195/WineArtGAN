# Dataset

El dataset utilizado es **Abstract Art Gallery**, disponible públicamente en Kaggle.

Fuente original: https://www.kaggle.com/datasets/bryanb/abstract-art-gallery

El dataset contiene imágenes de arte abstracto en formato JPG. Como el proyecto corresponde a generación de imágenes, las imágenes no poseen etiquetas de clasificación.

## Descarga

El dataset se descarga automáticamente desde el notebook `dev/01_dataset_preparation.ipynb` utilizando `kagglehub`:

```python
import kagglehub

path = kagglehub.dataset_download("bryanb/abstract-art-gallery")
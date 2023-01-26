
# Ejemplos

## Entrenamiento
Proporcionamos tres archivos de configuración de ejemplo para el ET para el entrenamiento en QM9, MD17 y ANI1 respectivamente. Para entrenar con un objetivo QM9 distinto de `energy_U0`, cambie el parámetro `dataset_arg` en el archivo de configuración QM9. Cambiar la molécula MD17 para entrenar funciona de forma análoga. Para entrenar un ET desde cero puede utilizar el siguiente código del directorio torchmd-net:
```bash
CUDA_VISIBLE_DEVICES=0,1 torchmd-train --conf examples/ET-{QM9,MD17,ANI1}.yaml
```
Utiliza la variable de entorno `CUDA_VISIBLE_DEVICES` para seleccionar en qué GPU quieres entrenar y cuántas. El ejemplo anterior selecciona GPUs con índices 0 y 1. El código de entrenamiento querrá guardar los puntos de control y los archivos de configuración en un directorio llamado `logs/`, que puedes cambiar en el archivo config.yaml o como argumento adicional en la línea de comandos: `--log-dir path/to/log-dir`.

## Carga de puntos de control
Puedes acceder a varios archivos de puntos de control preentrenados bajo las siguientes URLs:

- equivariant Transformer pretrained on QM9 (U0): http://pub.htmd.org/et-qm9.zip
- equivariant Transformer pretrained on MD17 (aspirin): http://pub.htmd.org/et-md17.zip
- equivariant Transformer pretrained on ANI1: http://pub.htmd.org/et-ani1.zip
- invariant Transformer pretrained on ANI1: http://pub.htmd.org/t-ani1.zip

Los puntos de control pueden cargarse utilizando la función `load_model` de TorchMD-Net. También se pueden pasar a la función argumentos adicionales del modelo (por ejemplo, activar la predicción de fuerza sobre las energías) para la inferencia. Vea el siguiente código de ejemplo para cargar un ET preentrenado en el conjunto de datos ANI1:

```python
from torchmdnet.models.model import load_model
model = load_model("ANI1-equivariant_transformer/epoch=359-val_loss=0.0004-test_loss=0.0120.ckpt", derivative=True)
```
El siguiente ejemplo muestra cómo ejecutar la inferencia en el punto de control del modelo. Para moléculas individuales, sólo tienes que pasar números atómicos y tensores de posición, para evaluar el modelo en múltiples moléculas simultáneamente, incluye también un tensor batch, que contiene el índice de molécula de cada átomo.
```python
import torch

# molecula única
z = torch.tensor([1, 1, 8], dtype=torch.long)
pos = torch.rand(3, 3)
energy, forces = model(z, pos)

# moleculas multiples 
z = torch.tensor([1, 1, 8, 1, 1, 8], dtype=torch.long)
pos = torch.rand(6, 3)
batch = torch.tensor([0, 0, 0, 1, 1, 1], dtype=torch.long)
energies, forces = model(z, pos, batch)
```
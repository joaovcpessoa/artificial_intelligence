# Pytorch

## O que é?

PyTorch é um pacote de computação científica baseado em Python que serve a dois propósitos principais:
- Uma alternativa ao NumPy para aproveitar o poder das GPUs e outros aceleradores
- Uma biblioteca de diferenciação automática útil para implementar redes neurais

## Objetivo

Compreender a biblioteca Tensor do PyTorch e as redes neurais em um nível avançado.
Ser capaz de treinar uma pequena rede neural para classificar imagens
Certifique-se de ter os pacotes `torch`, `torchvision` e `matplotlib` instalados.

## Tensores

Os tensores são uma estrutura de dados especializada muito semelhante a arrays e matrizes. No PyTorch, usamos tensores para codificar as entradas e saídas de um modelo, bem como os parâmetros do modelo.

Os tensores são semelhantes aos ndarrays do NumPy, com a diferença de que podem ser executados em GPUs ou outros hardwares especializados para acelerar o processamento. Se você já conhece ndarrays, vai se sentir em casa com a API de Tensores.

```python
import torch
import numpy as np
```

### Inicialização de Tensor

Os tensores podem ser inicializados de diversas maneiras.

<b>Diretamente dos dados</b><br>
Os tensores podem ser criados diretamente a partir dos dados. O tipo de dados é inferido automaticamente.

```python
data = [[1, 2], [3, 4]]
x_data = torch.tensor(data)
```
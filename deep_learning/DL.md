# Sumário

1. Deep Feedforward Networks
2. Regularization for Deep Learning
3. Optimization for Training Deep Models
4. Restricted Boltzmann Machine and Deep Belief Networks
5. Convolutional Networks
6. Sequence Modeling: Recurrent and Recursive Nets
7. Autoencoders and Representation Learning
8. Structured Probabilistic Models for Deep Learning and Probabilist Diffusion Models
9. Generative Adversarial Networks

## Pipeline de Carregamento de Dados (Dataloader)

Este projeto conta com um módulo de dataloader desenvolvido para fornecer dados de forma eficiente, escalável e reprodutível aos modelos de aprendizado de máquina nas etapas de treinamento, validação e teste.

O objetivo do dataloader é automatizar e otimizar o processo de ingestão de dados, garantindo:

- Leitura eficiente de grandes volumes de dados
- Pré-processamento em tempo real
- Geração de lotes (batches) compatíveis com os frameworks de ML utilizados
- Controle sobre a aleatoriedade e reprodutibilidade dos experimentos
- Flexibilidade para diferentes formatos e tipos de dados (imagens, séries temporais, texto, etc.)
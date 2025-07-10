#  Classificação de Texturas com VGG16 no DTD

Este projeto implementa um modelo baseado na arquitetura **VGG16** com _fine-tuning_, técnicas de regularização e visualização de ativações para realizar **classificação de texturas** no dataset **Describable Textures Dataset (DTD)**.

>  Projeto desenvolvido como parte do curso de Análise e Desenvolvimento de Sistemas – IFPI Campus Corrente.

---

##  Objetivo

Desenvolver um modelo robusto para **reconhecimento de texturas visuais complexas**, utilizando:

- A arquitetura **VGG16 pré-treinada**
- Técnicas como **Dropout**, **Batch Normalization**, **Regularização L1/L2**
- Função de perda **Focal Loss** para lidar com classes desbalanceadas
- Métricas de avaliação robustas: Acurácia, Precisão, Revocação e AUC
- Visualização das ativações intermediárias da rede

---

##  Dataset

**[Describable Textures Dataset (DTD)](https://www.robots.ox.ac.uk/~vgg/data/dtd/)**

- 5640 imagens em 47 categorias
- Anotadas manualmente com descrições semânticas como "riscado", "trançado", etc.
- Cada categoria possui 120 imagens
- Divisões (splits) oficiais para treino, validação e teste

---

##  Arquitetura do Modelo

Modelo baseado em **VGG16** com as seguintes modificações:

- Camadas convolucionais inferiores congeladas
- Cabeçalho customizado com:
  - `GlobalAveragePooling2D`
  - Camadas densas (256, 128) com **ReLU**, **BatchNorm**, **Dropout (60%)**
  - Regularização **L1 = 1e-5**, **L2 = 1e-4**
  - Camada final densa com 47 neurônios e **softmax**

---

##  Treinamento

- Otimizador: **Adam**
- Taxa de aprendizado: `1e-4` (ajustada com `ReduceLROnPlateau`)
- Função de perda: **Categorical Focal Loss**
- Estratégia de dados: aumento com rotação, zoom, deslocamento, brilho, flip
- Dividido em duas fases:
  1. Treinamento apenas das camadas superiores
  2. _Fine-tuning_ das camadas superiores da VGG16

---

##  Resultados

| Métrica     | Treinamento | Validação |
|-------------|-------------|-----------|
| Acurácia    | 59%         | 51%       |
| Precisão    | 87%         | 73%       |
| Revocação   | 46%         | 37%       |
| AUC         | 97%         | 91%       |
| Loss        | 43%         | 58%       |

- A **alta AUC** indica boa capacidade de separação entre classes
- A **baixa revocação** reflete a dificuldade em distinguir categorias subjetivas
- A **matriz de confusão** revelou sobreposição entre classes semelhantes como `striped`, `banded`, `lined` e `grid`

---

##  Visualizações

O notebook inclui:

- Gráficos de evolução da **acurácia**, **loss**, **precisão**, **revocação** e **AUC**
- Visualização das ativações intermediárias (camadas convolucionais)
- Matriz de confusão final para análise qualitativa dos erros

---

##  Lições Aprendidas

- O uso de **VGG16 com fine-tuning** mostrou-se eficaz na extração de padrões texturais
- O desafio maior foi a **generalização** para classes com grande variação visual
- Estratégias futuras:
  - Aplicação de **mecanismos de atenção**
  - Aumento de dados mais refinado
  - Exploração de **modelos autoexplicáveis**

---

##  Referências

- [Describable Textures Dataset – VGG (Oxford)](https://www.robots.ox.ac.uk/~vgg/data/dtd/)
- [VGGNet Architecture – Medium](https://medium.com/@siddheshb008/vgg-net-architecture-explained-71179310050f)
- [Extração de características de textura – UFPR](https://www.inf.ufpr.br/lesoliveira/padroes/haralick.pdf)
- [Reconhecimento de padrões em texturas – UNICAMP](https://ic.unicamp.br/~rocha/msc/ipdi/texture_classification.pdf)

---

##  Autor

**Guilherme Santo Costa**  
[IFPI - Campus Corrente | ADS]

---

##  Arquivo

- `ClassificacaodeTexturascomVGGnoDTD.ipynb`: notebook com todo o pipeline de pré-processamento, arquitetura, treinamento e avaliação

---

##  Requisitos (Google Colab)

- TensorFlow 2.x
- NumPy
- Matplotlib
- scikit-learn
- tqdm

---

##  Execução

Você pode executar o notebook diretamente no [Google Colab](https://colab.research.google.com/) com GPU ativada. Basta enviar o notebook e seguir os blocos passo a passo.

---


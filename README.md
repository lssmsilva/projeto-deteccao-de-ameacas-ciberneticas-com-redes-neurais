# 🔐 Detecção de Ameaças Cibernéticas com Redes Neurais (MLP)


[![Python](https://img.shields.io/badge/Python-3.10-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.x-F7931E?logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![Kaggle](https://img.shields.io/badge/Dataset-Kaggle-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/datasets/dhrubangtalukdar/cybersecurity-threat-detection-dataset)

---

## 📌 Descrição

Este projeto implementa um classificador de ameaças em tráfego de rede utilizando uma **Rede Neural do tipo MLP (Multilayer Perceptron)** construída com PyTorch. O modelo é treinado para identificar diferentes categorias de ataques cibernéticos a partir de features extraídas de logs de rede.

### Problema abordado

Dado um conjunto de características de um pacote de rede (protocolo, flags, tamanho, etc.), o modelo deve classificar corretamente o **tipo de ataque** — ou ausência dele — presente no tráfego.

---

## 🗂️ Estrutura do Projeto

```
📁 projeto/
 ├── projeto_profissional.ipynb   # Notebook principal com todo o pipeline
 └── README.md                   # Este arquivo
```

---

## 📊 Dataset

| Atributo | Detalhe |
|----------|---------|
| **Nome** | Cybersecurity Threat Detection Dataset |
| **Fonte** | [Kaggle](https://www.kaggle.com/datasets/dhrubangtalukdar/cybersecurity-threat-detection-dataset) |
| **Download** | Automático via `kagglehub` |
| **Tipo** | Dados tabulares de tráfego de rede |
| **Variável alvo** | `Attack Type` (classificação multiclasse) |

### Features utilizadas

Após remoção de colunas não informativas (IPs, timestamps, portas), o modelo recebe features como:

- Protocolo de rede
- Flags TCP/UDP
- Tamanho de pacotes
- Taxa de transmissão
- Outras métricas de comportamento de rede

---

## 🧠 Arquitetura do Modelo

```
Input(n_features)
    └─► Linear(128) → BatchNorm → ReLU → Dropout(0.3)
        └─► Linear(64)  → BatchNorm → ReLU → Dropout(0.3)
            └─► Linear(32)  → BatchNorm → ReLU
                └─► Linear(num_classes)   [logits]
```

| Componente | Configuração |
|------------|-------------|
| Camadas ocultas | 3 (128 → 64 → 32 neurônios) |
| Ativação | ReLU |
| Regularização | Dropout (p=0.3) + BatchNorm |
| Função de perda | CrossEntropyLoss |
| Otimizador | Adam (lr=1e-3) |
| Épocas | 30 |
| Batch size | 64 |

---

## ⚙️ Pipeline de Pré-processamento

```
Dados Brutos
    │
    ├─ 1. Remoção de colunas irrelevantes (IP, Timestamp, Portas)
    ├─ 2. Tratamento de nulos (mediana / moda)
    ├─ 3. Encoding de variáveis categóricas (LabelEncoder)
    ├─ 4. Separação features (X) e target (y)
    ├─ 5. Normalização (StandardScaler)
    └─ 6. Split Treino/Teste (80% / 20%, estratificado)
```

---

## 🚀 Como Executar

### Pré-requisitos

```bash
pip install torch pandas scikit-learn matplotlib seaborn kagglehub
```

### Google Colab (recomendado)

1. Faça upload do arquivo `projeto_profissional.ipynb` no [Google Colab](https://colab.research.google.com)
2. Execute as células em ordem (**Runtime → Run all**)
3. O dataset é baixado automaticamente via `kagglehub`

### Jupyter local

```bash
jupyter notebook projeto_profissional.ipynb
```

> **Nota sobre GPU:** O notebook detecta automaticamente se há GPU disponível (`cuda`) e usa CPU como fallback. Para melhor desempenho, habilite GPU no Colab em *Ambiente de execução → Alterar tipo de ambiente de execução → GPU*.

---

## 📈 Métricas de Avaliação

O projeto gera automaticamente:

- **Curvas de Loss e Acurácia** — treino vs. validação por época
- **Relatório de Classificação** — precisão, recall, F1-score por classe
- **Matriz de Confusão** — versão absoluta e normalizada

> Preencha a tabela abaixo após executar o notebook com os seus resultados:

| Métrica | Valor |
|---------|-------|
| Acurácia (teste) | `XX%` |
| F1-score macro | `XX%` |
| Épocas de convergência | `~XX` |

---

## 🔍 Decisões Técnicas

### Por que MLP e não CNN/RNN?

Dados tabulares estruturados não possuem dependências espaciais (CNN) nem sequenciais temporais (RNN). O MLP é adequado por aprender relações não lineares entre as features de forma direta e eficiente.

### Por que StandardScaler?

Features com escalas muito diferentes (ex.: tamanho de pacote em bytes vs. flags binárias) causam gradientes desbalanceados no treinamento. A normalização garante convergência mais estável.

### Por que Dropout + BatchNorm?

- **BatchNorm:** reduz o *internal covariate shift*, permitindo learning rates maiores e treinamento mais rápido.
- **Dropout:** força o modelo a não depender de neurônios específicos, melhorando a generalização.

---

## 🛠️ Possíveis Melhorias

- [ ] Aumentar profundidade da rede ou número de neurônios
- [ ] Aplicar `ReduceLROnPlateau` para ajuste dinâmico do learning rate
- [ ] Balancear classes com **SMOTE** (caso haja desbalanceamento)
- [ ] Exportar o modelo treinado com `torch.save`
- [ ] Experimentar outras arquiteturas: TabNet, XGBoost, LightGBM

---

## 📚 Referências

- [PyTorch Documentation](https://pytorch.org/docs/stable/index.html)
- [scikit-learn User Guide](https://scikit-learn.org/stable/user_guide.html)
- [Dataset — Kaggle: Cybersecurity Threat Detection](https://www.kaggle.com/datasets/dhrubangtalukdar/cybersecurity-threat-detection-dataset)
- Goodfellow, I., Bengio, Y., Courville, A. *Deep Learning*. MIT Press, 2016.

---

*Disciplina: Engenharia e Análise de Dados*

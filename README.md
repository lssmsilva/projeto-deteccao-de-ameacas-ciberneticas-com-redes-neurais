# Detecção de ameaças cibernéticas com redes neurais (MLP)

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.x-F7931E?logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)

Classificador de tráfego de rede com uma rede neural MLP (PyTorch) que tenta identificar 9 tipos de ataque e o tráfego benigno. O projeto também mostra por que a **acurácia engana em dados desbalanceados** e o que fazer a respeito.

## Resultado em uma frase

O modelo atinge 96,1% de acurácia, mas um classificador que responde sempre "benigno" atinge 96,0%. O F1 macro é **0,16**: com as variáveis disponíveis, a MLP só aprendeu a reconhecer o tráfego benigno e o DDoS.

| Métrica (teste, 2.000 linhas) | Valor |
|---|---|
| Acurácia | 96,10% |
| Baseline (sempre "benign") | 96,00% |
| F1-score macro | 0,16 |
| Recall do DDoS (5 exemplos no teste) | 60% |
| Recall das outras 8 classes de ataque | 0% |
| Convergência | cerca de 5 épocas |

## Dataset

| Atributo | Detalhe |
|---|---|
| Nome | [Cybersecurity Threat Detection Dataset](https://www.kaggle.com/datasets/dhrubangtalukdar/cybersecurity-threat-detection-dataset) (Kaggle) |
| Tamanho | 10.000 registros × 13 colunas |
| Alvo | `attack_type`: benign (9.600) e 9 ataques (brute-force 108, port-scan 81, sql-injection 64, xss 34, credential-stuffing 28, ddos 27, command-injection 26, exploit-attempt 22, c2 10) |
| Download | Automático via `kagglehub` |

## Decisões de pré-processamento

- **Vazamento evitado:** a coluna `label` (0 = benigno, 1 = ataque) foi removida, porque é derivada do alvo.
- **Colunas descartadas:** IPs, timestamps e portas. A URL virou um indicador de presença (`url_ausente`, 32% de nulos).
- **Features finais (6):** protocolo, bytes enviados, bytes recebidos, user agent, tráfego interno e URL ausente.
- **Ordem correta:** split estratificado 80/20 antes da normalização; o `StandardScaler` é ajustado só no treino.

## Modelo

MLP com 3 camadas ocultas (128 → 64 → 32), BatchNorm, ReLU, Dropout 0,3 nas duas primeiras, CrossEntropyLoss e Adam (lr 1e-3), 30 épocas, batch de 64. São 12.010 parâmetros treináveis.

## O que aprendi com o resultado

- Em dados 96% benignos, a acurácia sozinha não diz nada. O F1 macro e o recall por classe mostram o que o modelo de fato aprendeu.
- Com 2 a 22 exemplos por classe no teste, as métricas por classe são instáveis.
- Próximos passos: ponderação de classes ou SMOTE, reincluir as portas, extrair sinais do `user_agent` e da URL, e comparar com Random Forest ou XGBoost.

## Como executar

1. `pip install -r requirements.txt`
2. Abra `Deteccao_de_Ameacas_Ciberneticas_MLP.ipynb` no Jupyter ou no Google Colab e execute as células em ordem. O dataset é baixado automaticamente pelo `kagglehub`.

## Estrutura

```
├── Deteccao_de_Ameacas_Ciberneticas_MLP.ipynb
├── requirements.txt
└── README.md
```

## Autora

Laura Silva Soares de Melo · [LinkedIn](https://www.linkedin.com/in/laura-soares-313067195) · [GitHub](https://github.com/lssmsilva)

Projeto da disciplina de Engenharia e Análise de Dados (CESAR School).

# Projeto de Previsão de Risco de Crédito

Este repositório contém o desenvolvimento de um pipeline completo de Machine Learning para a classificação de risco de crédito, focado em prever a inadimplência de clientes.

## 1. Descrição do Problema de Negócio
A concessão de crédito é uma atividade fundamental para instituições financeiras, mas carrega o risco intrínseco de inadimplência. O objetivo deste projeto é desenvolver um modelo preditivo capaz de identificar clientes com alta probabilidade de não honrar seus pagamentos.

## 2. Resumo Executivo: Insights da EDA
Durante a Análise Exploratória de Dados (Fase 1), identificamos pontos cruciais para a estratégia de modelagem:
- **Desbalanceamento de Classe:** A base possui significativamente mais pagadores do que inadimplentes, exigindo o uso de técnicas como SMOTE para o treino.
- **Qualidade dos Dados:** Detectamos outliers biológicos (ex: idade de 144 anos) e tempos de emprego irreais, tratados via técnica de *Clipping* por IQR.
- **Correlações:** Nenhuma variável isolada explica a inadimplência, mas a combinação de variáveis como `renda_pessoa`, `taxa_juros_emprestimo` e a nova feature `comprometimento_renda` mostrou-se promissora.

## 3. Dicionário de Dados Atualizado

| Coluna | Descrição |
| :--- | :--- |
| `idade_pessoa` | Idade do cliente (anos). |
| `renda_pessoa` | Renda anual informada pelo cliente. |
| `propriedade_casa_pessoa` | Tipo de moradia (Alugada, Própria, Hipoteca, Outros). |
| `tempo_emprego_pessoa` | Tempo de permanência no emprego atual (anos). |
| `intencao_emprestimo` | Motivo da solicitação do crédito. |
| `grau_emprestimo` | Nota de risco atribuída ao empréstimo (A a G). |
| `valor_emprestimo` | Montante total solicitado. |
| `taxa_juros_emprestimo` | Taxa de juros aplicada ao contrato. |
| `status_emprestimo` | **Variável Alvo** (0: Adimplente, 1: Inadimplente). |
| `porcentagem_renda_emprestimo` | Relação entre o valor do empréstimo e a renda anual original. |
| `inadimplencia_historico_credito` | Indica se o cliente já foi inadimplente anteriormente (Y/N). |
| `tempo_historico_credito_pessoa` | Tempo total de histórico de crédito no mercado (anos). |
| **`comprometimento_renda`** | **Nova Feature:** Porcentagem real da renda anual comprometida com o valor total do empréstimo. |

## 4. Veredito do Melhor Modelo
Após testar e otimizar os modelos de **K-Nearest Neighbors (KNN)** e **Árvore de Decisão (DT)**, chegamos às seguintes conclusões:

- **Modelo Escolhido:** KNN (n_neighbors=9).
- **Justificativa:** Embora a Árvore de Decisão tenha apresentado maior acurácia geral (~92.6%), o **KNN apresentou um Recall superior para a classe de inadimplentes (76%)**.
- **Impacto Financeiro:** No negócio de crédito, o custo de um Falso Negativo (perda do capital emprestado) é substancialmente maior que o custo de um Falso Positivo. O KNN minimiza a perda direta ao identificar um volume maior de potenciais inadimplentes, protegendo o caixa da instituição.

## 5. Tecnologias Utilizadas
- Python, Pandas, Matplotlib, Seaborn.
- Scikit-Learn (StandardScaler, OneHotEncoder, KNN, DecisionTree).
- Imbalanced-learn (SMOTE).
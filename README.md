# Pipeline Preditivo de Risco de Crédito 💳

Este repositório contém o projeto final do módulo de **Machine Learning**, focado na construção de um pipeline preditivo completo para o setor financeiro. O objetivo é prever a inadimplência de clientes utilizando algoritmos de classificação e técnicas avançadas de pré-processamento de dados.

## 🚀 Contexto e Problema de Negócio

No setor bancário, a concessão de crédito é uma atividade de alto risco. Identificar corretamente clientes que podem se tornar inadimplentes é essencial para a saúde financeira da instituição.

*   **Problema:** Prever se um cliente será inadimplente (`status_emprestimo = 1`) ou se pagará o empréstimo em dia (`status_emprestimo = 0`).
*   **Desafio de Negócio:** Equilibrar o custo de perder uma oportunidade de lucro (Falso Positivo - rejeitar um bom pagador) contra o custo de perda direta de capital (Falso Negativo - conceder crédito a um mau pagador).

## 📊 Dicionário de Dados

A base de dados original foi traduzida para facilitar a análise. Abaixo estão as principais variáveis:

| Coluna Original | Coluna Traduzida | Descrição |
| :--- | :--- | :--- |
| `person_age` | `idade_pessoa` | Idade do cliente. |
| `person_income` | `renda_pessoa` | Renda anual do cliente. |
| `person_home_ownership`| `propriedade_casa_pessoa` | Tipo de moradia (Alugada, Própria, etc). |
| `person_emp_length` | `tempo_emprego_pessoa` | Tempo (em anos) de emprego. |
| `loan_intent` | `intencao_emprestimo` | Motivo do empréstimo. |
| `loan_grade` | `grau_emprestimo` | Nota de crédito atribuída ao empréstimo. |
| `loan_amnt` | `valor_emprestimo` | Valor total do empréstimo solicitado. |
| `loan_int_rate` | `taxa_juros_emprestimo` | Taxa de juros aplicada. |
| `loan_status` | `status_emprestimo` | **Alvo:** (0: Pago, 1: Inadimplente). |
| `cb_person_default_on_file`| `inadimplencia_historico_credito` | Histórico de inadimplência (Y/N). |
| **Nova Coluna** | **`comprometimento_renda`** | Porcentagem da renda anual comprometida com o empréstimo. |

## 🛠️ Desenvolvimento do Pipeline

O projeto foi dividido em 6 fases rigorosas:

1.  **EDA (Análise Exploratória):** Identificamos um desbalanceamento severo (apenas 21.8% de inadimplentes) e a presença de outliers críticos (idades de 144 anos).
2.  **Data Prep:** Remoção de duplicatas e imputação de valores nulos pela **mediana** (para mitigar o impacto de outliers). Aplicamos **Clipping via IQR** para limitar valores extremos.
3.  **Feature Engineering:** Criação da métrica `comprometimento_renda`, fundamental para entender a capacidade de pagamento.
4.  **Preparação Segura:** 
    *   **Encoding:** One-Hot Encoding para categorias e Label Encoding para binários.
    *   **Split:** 80/20 com estratificação.
    *   **Balanceamento:** Uso de **SMOTE** aplicado exclusivamente nos dados de treino.
    *   **Escalonamento:** `StandardScaler` aplicado apenas para o KNN (sensível a escala).
5.  **Validação e Overfitting:** Testamos múltiplos valores de `K` (KNN) e `max_depth` (Árvore). Monitoramos métricas de treino e teste para garantir a generalização do modelo.
6.  **Avaliação:** Uso de Relatórios de Classificação e Matrizes de Confusão para análise de erros.

## 📈 Resumo Executivo e Veredito

### Principais Insights da EDA:
*   A **Taxa de Juros** tem uma correlação positiva forte com o risco de inadimplência.
*   Clientes com renda mais baixa e alto valor de empréstimo (alto comprometimento) tendem a falhar no pagamento com mais frequência.
*   O desbalanceamento inicial da base exigiu o uso de SMOTE para evitar que o modelo ficasse "viciado" na classe majoritária.

### Veredito Final:
Embora a **Árvore de Decisão (`max_depth=7`)** tenha apresentado uma precisão altíssima (0.97), o modelo selecionado para produção foi o **KNN (`n_neighbors=9`)**.

**Justificativa:** No setor bancário, o **Falso Negativo** (não detectar um inadimplente) é o erro mais caro, pois resulta na perda direta de capital. O KNN apresentou um **Recall superior (0.76)** para a classe de inadimplentes em comparação à Árvore de Decisão, minimizando as perdas financeiras críticas para a instituição, apesar de um número maior de Falsos Positivos.

---
**Desenvolvido por:** Rafael dos Santos Cunha

**Tecnologias:** Python, Pandas, Scikit-Learn, Imbalanced-Learn, Seaborn.

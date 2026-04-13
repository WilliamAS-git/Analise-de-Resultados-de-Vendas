# 📊 Case - Analista de Dados Pleno  
**Autor:** William Armindo da Silva  

---

## 📌 Sobre o Projeto  
Este projeto tem como objetivo demonstrar habilidades práticas em análise de dados, abrangendo todo o fluxo analítico:  
- Tratamento e limpeza de dados (ETL)  
- Modelagem de dados  
- Criação de métricas com DAX  
- Construção de dashboard e geração de insights  

Os dados representam um cenário de vendas ao longo do ano de 2024.

---

## 🛠️ 1. ETL e Tratamento de Dados (Power Query)

Durante o processo de ingestão dos dados, foram encontrados alguns desafios:

### 🔹 Problemas Identificados
- Arquivo CSV com aspas duplas em todas as linhas  
- Delimitação incorreta das colunas  
- Valores numéricos no padrão americano (ex: `.` como decimal)  

### 🔹 Soluções Aplicadas
- Uso de **Dividir Coluna por Delimitador (vírgula)**  
- Remoção de aspas com **Substituição de Valores**  
- Conversão de formato numérico (`.` → `,`) antes da tipagem  
- Tipagem correta dos dados:
  - IDs como **Texto** (evitando perda de zeros à esquerda)  
  - Valores financeiros como **Decimal**  

---

## ✅ Qualidade dos Dados

- ✔️ Zero valores nulos  
- ✔️ Zero duplicidades  
- ✔️ IDs únicos e consistentes  
- ✔️ Categorias padronizadas  

---

## 🗂️ Estrutura dos Dados

| Campo                | Tipo     | Descrição |
|---------------------|----------|----------|
| ID_Venda            | Texto    | Identificador da venda |
| Data_Venda          | Data     | Período de 2024 |
| ID_Cliente          | Texto    | 100 clientes únicos |
| ID_Produto          | Texto    | 50 produtos |
| Quantidade          | Inteiro  | 1 a 50 unidades |
| Valor_Unitario      | Decimal  | R$10,15 a R$4.988,77 |
| Desconto_Percentual | Decimal  | 0% a 25% |
| Valor_Total         | Decimal  | Receita final |
| Status_Pedido       | Texto    | Concluído, Pendente, Cancelado |
| Região              | Texto    | Regiões do Brasil |

---

## 🧠 2. Modelagem de Dados

Foi criada uma tabela de calendário para suportar análises temporais:

```dax
dCalendario =
VAR DataMinima = MIN('DADOS TESTE T'[Data_Venda])
VAR DataMaxima = MAX('DADOS TESTE T'[Data_Venda])
RETURN
ADDCOLUMNS (
    CALENDAR(DataMinima, DataMaxima),
    "Ano", YEAR([Date]),
    "Mês Nome", FORMAT([Date], "MMMM"),
    "Mês-Ano", UPPER(FORMAT([Date], "MMM-yy")),
    "AnoMesNum", YEAR([Date]) * 100 + MONTH([Date])
)
```
🔹 Destaque:
A coluna AnoMesNum garante ordenação cronológica correta dos meses.



📐 3. Métricas (DAX)

🔹 Principais Medidas
```dax
Total_Vendas = SUM('DADOS TESTE T'[Valor_Total])
```
```dax
Ticket_Medio =
DIVIDE(
    [Total_Vendas],
    DISTINCTCOUNT('DADOS TESTE T'[ID_Venda])
)
```
```dax
Taxa_Cancelamento =
DIVIDE(
    CALCULATE(
        COUNT('DADOS TESTE T'[ID_Venda]),
        'DADOS TESTE T'[Status_Pedido] = "Cancelado"
    ),
    COUNT('DADOS TESTE T'[ID_Venda])
)
```

🔹 Outras Métricas
- Total de Clientes
- Total de Desconto
- Quantidade de Vendas
- Média de Desconto



## 📊 4. Dashboard e Insights

💰 Resumo Financeiro
| Métrica              | Valor            |
| -------------------- | ---------------- |
| Faturamento Total    | R$ 28.660.878,85 |
| Ticket Médio         | R$ 57.321,76     |
| Total de Transações  | 500              |
| Itens Vendidos       | 13.139           |
| Taxa de Cancelamento | 5,4%             |
| Desconto Médio       | 12,3%            |


## 📅 Análise Temporal
- Melhor mês: Janeiro (~R$ 3,4M)
- Pior mês: Setembro (~R$ 1,8M)


## 🌎 Vendas por Região
- Visual em gráfico de barras
- Distribuição Regional: A região Sudeste lidera com 40,9% da participação na receita.


## 📦 Produtos
Produto destaque: P003
- 542 unidades vendidas
- ~R$ 1,4M em receita
- +5% do faturamento total



## 📉 Status dos Pedidos
| Status    | Qtde | %     |
| --------- | ---- | ----- |
| Concluído | 422  | 84,4% |
| Pendente  | 51   | 10,2% |
| Cancelado | 27   | 5,4%  |


🔍 Insight:
Mais de 50% dos pedidos pendentes acabam sendo cancelados.

👉 Recomendação:
Investigar causas (pagamento, frete, logística) para reduzir perdas.




## 🎯 Impacto dos Descontos
Análise via gráfico de dispersão
Identificação de outliers (ex: venda com 22% de desconto)

👉 Recomendação:
Limitar descontos agressivos a campanhas específicas.


👥 Clientes Recorrentes

Cliente destaque: C041
Forte impacto na receita

👉 Recomendação:
Implementar programa de fidelidade (ex: cashback)

--
## 🚀 Conclusão

O projeto demonstra domínio em:
- ETL com Power Query
- Modelagem de dados
- Criação de métricas em DAX
- Geração de insights acionáveis
Com foco em transformar dados em decisões estratégicas.

📎 Tecnologias Utilizadas
- Power BI
- Power Query
- DAX

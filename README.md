# Harmonia Music Store Project - ETL Pipeline

Este projeto demonstra a construção de um **pipeline ETL (Extract, Transform, Load)** aplicado aos dados da loja fictícia *Harmonia Music Store*.  
O objetivo é mostrar como organizar o fluxo de dados em etapas claras e reproduzíveis, desde a extração até a exportação para análise.

---

## ✨ Autor 
- Paulo V. R. Souza
- LinkedIn: paulovrsouza
- GitHub: vaiprogramarpaulo

---

## 🎯 Objetivo
- Consolidar dados brutos em um formato limpo e estruturado.
- Criar colunas derivadas que auxiliem na análise (ex.: Ticket Médio, ordenação de meses).
- Exportar os dados tratados para arquivos CSV e Excel, prontos para uso em ferramentas como Power BI.
- Demonstrar boas práticas de organização de código em Python.

---

## ⚙️ Estrutura do Pipeline
O pipeline segue três etapas principais:

1. **Extract (Extração)**  
   - Leitura dos dados brutos a partir de um arquivo CSV.

2. **Transform (Transformação)**  
   - Limpeza de linhas nulas e duplicadas.  
   - Criação de colunas derivadas (`Mês_Num`, `Ticket Médio`).  
   - Ajuste estético do índice para começar em 1.

3. **Load (Carregamento)**  
   - Exportação dos dados tratados para novos arquivos CSV e Excel.

---

## 📂 Arquivos do Repositório
- `notebooks/harmonia_music_store.ipynb` → Notebook Jupyter com o código completo  
- `data/harmonia_music_store.csv` → Arquivo de dados brutos (entrada)  
- `output/harmonia_music_store_tratado.csv` → Dados tratados em CSV  
- `output/harmonia_music_store_tratado.xlsx` → Dados tratados em Excel  
- `README.md` → Documento explicativo do projeto  
- `pipeline_visual_hms.pdf` -> (opcional) Fluxograma visual do processo ETL

---

## 🚀 Como Executar
1. Clone este repositório:
   ```bash
   git clone https://github.com/vaiprogramarpaulo/harmonia-music-store-etl.git
   cd harmonia-music-store-etl
2. Abra o Jupyter Notebook:
	```bash
   jupyter notebook
3. Execute o arquivo:
	```bash
   notebooks/harmonia_music_store.ipynb
---

## 📊 Resultados
Após a execução do pipeline, os dados ficam prontos para análise em ferramentas de BI.
Exemplo de colunas criadas:
- Mês_Num → facilita ordenação cronológica.
- Ticket Médio → permite avaliar o valor médio por venda.

Prévia dos dados tratados: 
| Produto | Mês       | Quantidade | Receita Total | Ticket Médio |
|---------|-----------|------------|----------------|---------------|
| Violão  | Janeiro   | 10         | 5000           | 500.0         |
| Piano   | Fevereiro | 5          | 15000          | 3000.0        |

## 🖼️ Dashboard Básico em Power BI

### Medidas DAX
- **Faturamento (Receita Total)**
  `Receita Total = SUM(Sheet1[Receita Total])`

- **Ticket Médio**
  `Ticket Médio = DIVIDE(Sheet1[Receita Total], Sheet1[Total de Vendas])`

- **Total de Vendas**
  `Total de Vendas = SUM(Sheet1[Quantidade])`

### Visualizações
![Dashboard com cards e gráfico](docs/dashboard_overview.png)
- Cards: Faturamento, Ticket Médio, Total de Vendas
- Gráfico de Linhas: Variação da Receita ao longo dos meses (ordenados cronologicamente)

### Ajustes no Power Query
- **MesesMap:** tabela auxiliar com as colunas `Mês` e `Mês_Num`.
- **Merge:** merge realizado com `Sheet1` para trazer `Mês_Num`.
- **Ordenação:** `Sheet1[Mês_Num]` definido como ordenação para `Sheet1[Mês]`, e `Sheet1[Mês_Num]` classificado por ordem crescente.
- **Evidências**:
	![Merge no Power Query](docs/powerquery_merge.png)
	![Tabela Auxiliar MesesMap](docs/MesesMap_tabela.png)

### Como reproduzir
1. Abra o arquivo `.pbix` em Power BI
2. Verifique se `Sheet1[Mês]` está classificada por `Sheet1[Mês_Num]`
3. Explore os cards e o gráfico de variação da receita

---

# 📈 Projeto Power BI - DAX Avançado Versão 1


## Descrição
Projeto de portifólio demonstrando medidas DAX para análise de receita por produto e validação visual em relatório Power BI.

## Nota sobre o dataset utilizado
**Dados:** dataset hipotético usado para demonstração; não contém informações reais.

## Evidências e imagens incluídas
- Dashboard principal: ![](docs/dashboard_overview.png)
- Validação Receita por Produto com Card: ![](docs/percent-total-with-card.png)
- Power Query MesesMap e Merge: ![](docs/MesesMap_tabela.png); ![](docs/powerquery_merge.png)
- Imagem com as colunas `Produto`, `Receita Produto` e `% Receita Produto`: 
![Dashboard com cards e gráfico](docs/percent_total.png)
- Rank por Produto: ![](docs/rank_produto.png)
- Dashboard de Receita por Ticket Bucket: ![](docs/ticket_bucket.png)

### Medidas DAX Implementadas 
 **Receita Total:** `Receita Total = SUM(Sheet1[Receita Total])`  
 **- Responde:** total de receita no dataset.  
 **- Como validar:** comparar com card de soma no dashboard.

**Receita Produto Medida:** `Receita Produto Medida = CALCULATE(SUM('Sheet1'[Receita Total]), VALUES(Sheet1[Produto]))`  
**- Responde:** receita agregada por produto no contexto do visual.    
**- Como validar:** tabela `Produto` + `Receita Produto`; soma de `Receita Produto` por todos os produtos = `Receita Total`. Ver `docs/percent_total_with_card.png`

**% Receita Produto:** `% Receita Produto = DIVIDE([Receita Produto], CALCULATE('Sheet1'[Soma Receita Total], ALL('Sheet1')), 0)`  
**- Responde:** impacto e participação de cada produto na receita total.  
**- Como validar:** soma das participações ≈ 100%. Ver `docs/percent-total_with_card.png`.

**Rank Produto Receita (Medida):** `Rank Produto Receita = RANKX(ALL('Sheet1'[Produto]), [Receita Produto Medida],, DESC, DENSE)`  
**- Responde:** posição de cada produto por receita (1 = maior receita).  
**- Como validar:** soma das participações ≈ 100%. Ver `docs/rank_total.png`.

**Ticket Bucket (Coluna):** `Ticket Bucket = SWITCH(TRUE(), 'Sheet1'[Ticket Médio] <= 1500, "Baixo", 'Sheet1'[Ticket Médio] <= 3000, "Médio", "Alto")`  
**Ticket Bucket (Coluna):** `Receita por Bucket = CALCULATE([Soma Receita Total])`  
**- Responde:** distribuição da receita por faixa de ticket.
**- Como validar:** somar receitas por bucket e comparar com `Receita Total`. Ver `ticket_bucket.png`.

---

# 📌 Conclusão
- O projeto demonstrou a construção de um pipeline ETL simples e funcional.
- Foram criadas medidas DAX e visuais em Power BI que permitem analisar receita, ticket médio e participação por bucket.
- A organização em etapas claras (Extract, Transform, Load) facilita a reprodutibilidade e serve como base para projetos futuros.

---

# 🔮 Próximos Passos
- **Melhorias de qualidade de dados:** tratar outliers, preencher valores faltantes, validar regras de negócio.
- **Novos indicadores:** métricas que agreguem valor (margem, churn, LTV) quando houver dados.
- **Validação e testes:** criar testes automatizados para garantir que transformações não quebrem.

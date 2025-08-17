# Projeto 2 — SQL Server + Power BI (AdventureWorksDW2014)

**Objetivo:** integrar **SQL Server** ao **Power BI** para responder perguntas de negócio sobre vendas online (FactInternetSales) com duas páginas de dashboard: **Geral** e **Demografia & País**. Reforcei a análise com um script em **Python + matplotlib**.

## 🔎 Perguntas de negócio
**Aba Geral**
- Receita total
- Quantidade vendida
- Total de categorias de produtos
- Quantidade de clientes
- Receita total e lucro total por mês
- Margem de lucro
- Lucro por país

**Aba Demografia & País**
- Vendas por país
- Clientes por país
- Vendas por gênero
- Vendas por categoria

## 🗃️ Fontes e Tabelas
- **Fato:** `FactInternetSales`
- **Dimensões:** `DimCustomer`, `DimGeography`, `DimProduct` → `DimProductSubcategory` → `DimProductCategory`, `DimDate`

Campos-chave:
- `FactInternetSales.OrderDateKey -> DimDate.DateKey`
- `FactInternetSales.CustomerKey -> DimCustomer.CustomerKey -> DimCustomer.GeographyKey -> DimGeography.GeographyKey`
- `FactInternetSales.ProductKey -> DimProduct.ProductKey -> DimProductSubcategory.ProductSubcategoryKey -> DimProductCategory.ProductCategoryKey`

## 🧱 Modelo (estrela)
Fato no centro (**FactInternetSales**) ligado às dimensões **Date**, **Customer (→ Geography)** e **Product (→ Subcategory → Category)**. Marque **DimDate[FullDateAlternateKey]** como Tabela de Datas no Power BI.

## 🧮 Métricas (DAX)
```DAX
Receita Total = SUM(FactInternetSales[SalesAmount])
Custo Total = SUM(FactInternetSales[TotalProductCost])
Lucro Total = [Receita Total] - [Custo Total]
Margem % = DIVIDE([Lucro Total], [Receita Total])

Qtd Vendida = SUM(FactInternetSales[OrderQuantity])
Clientes Distintos = DISTINCTCOUNT(FactInternetSales[CustomerKey])
Pedidos Distintos = DISTINCTCOUNT(FactInternetSales[SalesOrderNumber])

Receita YTD = TOTALYTD([Receita Total], 'DimDate'[FullDateAlternateKey])
Lucro YTD   = TOTALYTD([Lucro Total],   'DimDate'[FullDateAlternateKey])
Margem YTD % = DIVIDE([Lucro YTD], [Receita YTD])

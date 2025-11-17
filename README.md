# Visualização de Dados Utilizando Ferramenta de BI — TIS 6

## Introdução

Este projeto faz parte do **Laboratório de Experimentação em Engenharia de Software (TIS 6)** e tem como objetivo aplicar conceitos de **Business Intelligence (BI)** para **análise de dados de repositórios de software**, utilizando o **Microsoft Power BI** como ferramenta principal de visualização e exploração.

O BI (Business Intelligence) é o processo de **coletar, organizar, analisar e visualizar informações** para apoiar decisões baseadas em dados.  
A partir de dashboards interativos e atualizados automaticamente, é possível identificar **tendências, gargalos e oportunidades de melhoria**, melhorando a **qualidade do código** e a **produtividade das equipes de desenvolvimento**.

---

## Caracterização do Dataset

Os dados utilizados neste trabalho foram coletados de **repositórios do GitHub** e incluem informações sobre commits, métricas de complexidade e qualidade de código.  
As principais tabelas e seus atributos:

| Tabela | Descrição | Principais Campos |
|--------|------------|------------------|
| **commits** | Registros de commits realizados | `commit_hash`, `created_at`, `repository_id`, `changed_files`, `changed_sloc` |
| **commits_metrics metrics** | Métricas de código por commit | `cbo`, `rfc`, `wmc`, `total_code_smells`, `blocker_quantity`, `critical_quantity`, `major_quantity`, `minor_quantity`, `info_quantity` |
| **commits_metrics repositories** | Dados dos repositórios | `name`, `stars`, `created_at` |

🔹 **Período de análise:** Janeiro a Dezembro do ano observado  
🔹 **Total de repositórios analisados:** 22  
🔹 **Total de commits:** 1000  

As métricas foram extraídas e transformadas para permitir comparações entre a **qualidade do código**, **complexidade**, e **frequência de alterações** por repositório e por mês.

---

## Especificação do Dashboard

O dashboard foi organizado para responder às questões de pesquisa sobre o **impacto do tamanho dos commits na qualidade do código**, seguindo a metodologia do estudo de referência [[1]](#referências). Os commits foram classificados em faixas de tamanho baseadas em `changed_sloc` (commits únicos, que são os commits pequenos (entre 1 e 100 sloc) e commits agregrados, que sãos os commits grandes (a partir de 100 sloc)) para análise comparativa.

O dashboard foi dividido em **três seções principais**:

### 1 Relação entre Tamanho de Commits e Code Smells (RQ1)
Analisa qual faixa de tamanho de commits está mais associada à introdução de _code smells_:

- **Título:** _Distribuição de Code Smells por Tamanho de Commit_  
- **Descrição:** Gráfico de dispersão ou histograma mostrando a relação entre `changed_sloc` (ou faixas de tamanho) e `total_code_smells`.  
- **Insight:** Permite identificar se commits maiores introduzem proporcionalmente mais _code smells_ que commits menores.  

- **Título:** _Média de Code Smells por Faixa de Tamanho_  
- **Descrição:** Gráfico de barras comparando a média de _code smells_ por commit em cada categoria de tamanho (Small, Medium, Large, Very Large).  
- **Insight:** Facilita a comparação direta do impacto de cada faixa de tamanho na introdução de problemas de código.  

---

### 2 Impacto do Tamanho dos Commits na Qualidade do Código (RQ2)
Avalia como diferentes tamanhos de commits afetam métricas de qualidade:

- **Título:** _Taxa de Qualidade por Faixa de Tamanho de Commit_  
- **Descrição:** Gráfico de barras agrupadas ou linhas mostrando a `Taxa de Qualidade` (code smells por SLOC) para cada categoria de tamanho de commit.  
- **Insight:** Revela se commits maiores degradam mais a qualidade do código em relação ao volume alterado.  

- **Título:** _Distribuição de Commits e Code Smells por Tamanho_  
- **Descrição:** Gráfico de barras empilhadas ou treemap mostrando a proporção de commits e o total de _code smells_ em cada faixa de tamanho.  
- **Insight:** Permite entender se commits grandes, mesmo sendo menos frequentes, contribuem desproporcionalmente para a degradação da qualidade.  

---

### 3 Distribuição de Criticidades por Tamanho de Commit (RQ3)
Explora a proporção de cada tipo de severidade de _code smell_ em relação ao tamanho dos commits:

- **Título:** _Proporção de Severidades por Faixa de Tamanho_  
- **Descrição:** Gráfico de barras empilhadas (100%) mostrando a distribuição percentual de _Blockers_, _Criticals_, _Majors_, _Minors_ e _Infos_ em cada categoria de tamanho.  
- **Insight:** Identifica se commits maiores tendem a introduzir problemas mais críticos (Blockers/Criticals) ou se mantêm a mesma proporção de severidades.  

- **Título:** _Quantidade Absoluta de Severidades por Tamanho_  
- **Descrição:** Gráfico de barras agrupadas mostrando a quantidade absoluta de cada tipo de severidade por faixa de tamanho de commit.  
- **Insight:** Complementa a análise anterior mostrando o volume real de problemas críticos introduzidos por cada categoria de commit.  

---

## Questões de Pesquisa (RQs)

As questões de pesquisa foram formuladas com base no estudo sobre o impacto de commits grandes na qualidade de código [[1]](#referências).

### **RQ1:** Qual tamanho de commits mais se relaciona com a introdução de _code smells_?
- Métricas envolvidas: `changed_sloc`, `changed_files`, `total_code_smells`
- Visual utilizado: Gráfico de dispersão ou histograma (tamanho do commit vs. _code smells_)

### **RQ2:** Qual o impacto do tamanho dos commits na qualidade do código?
- Métricas envolvidas: `changed_sloc`, `Taxa de Qualidade`, `total_code_smells`
- Visual utilizado: Gráfico de linha ou barras agrupadas por faixa de tamanho de commit

### **RQ3:** Qual a proporção de cada tipo de criticidade de _code smell_ de acordo com o tamanho do commit?
- Métricas envolvidas: `changed_sloc`, `blocker_quantity`, `critical_quantity`, `major_quantity`, `minor_quantity`, `info_quantity`
- Visual utilizado: Gráfico de barras empilhadas ou gráfico de pizza por faixa de tamanho

<img width="635" height="355" alt="BI" src="https://github.com/user-attachments/assets/fcd877f9-fef3-4bf5-95aa-ba503ee0f09b" />

---

## Metodologia

1. **Coleta de Dados:**  
   Os dados foram extraídos de repositórios públicos do GitHub.

2. **Limpeza e Transformação:**  
   - Remoção de duplicatas e commits sem métricas válidas.  
   - Cálculo de medidas agregadas (total, média e razão).  

3. **Modelagem no Power BI:**  
   - Criação de uma **tabela calendário**.  
   - Estabelecimento de relacionamentos entre tabelas.  
   - Criação de **medidas DAX** para sumarização dinâmica.  

4. **Visualização:**  
   - Gráficos de linha, barras empilhadas, dispersão, e KPIs.  
   - Aplicação de formatação condicional e tema escuro moderno.

---

## 🧾 Principais Medidas (DAX)

Algumas das principais medidas utilizadas:

```DAX
Qtd Commits = COUNTROWS ( commits )
SLOC Alterado = SUM ( commits[changed_sloc] )
Total Code Smells = SUM ( 'commits_metrics metrics'[total_code_smells] )
Taxa de Qualidade = DIVIDE ( [Total Code Smells], [SLOC Alterado], 0 ) * 1000
Débito Técnico Score = 
    [Total Blocker] * 5 +
    [Total Critical] * 3 +
    [Total Major] * 2 +
    [Total Minor] * 1
Índice de Complexidade =
    ( [CBO Médio] + [RFC Médio] + [WMC Médio] ) / 3
```

---

## Referências

[1] Impacto de Commits Grandes na Qualidade de Código e Estabilidade de Features em App Java. Disponível em: `impacto-commits-grandes-na-qualidade-de-codigo-estabilidade-de-features-em-app-java.pdf`

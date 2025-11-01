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

O dashboard foi dividido em **três seções principais**:

### 1 Caracterização do Dataset
Mostra uma visão geral da atividade de desenvolvimento:

- **Título:** _Evolução de Commits e SLOC Alterado_
- **Descrição:** Gráfico de linha mostrando a evolução mensal da quantidade de commits e das linhas de código alteradas (`changed_sloc`).
- **Insight:** Permite observar picos de atividade e períodos de maior produtividade.

---

### 2 Análise de Qualidade de Código
Destina-se a avaliar a **manutenção e qualidade dos repositórios**:

- **Título:** _Code Smells e Índice de Qualidade por Mês_  
- **Descrição:** Linha dupla comparando o total de _code smells_ gerados e a taxa de qualidade calculada.  
- **Insight:** Uma queda na taxa de qualidade costuma acompanhar aumento no número de _code smells_.  

- **Título:** _Distribuição de Severidades de Problemas por Mês_  
- **Descrição:** Gráfico de barras empilhadas mostrando quantos _Blockers_, _Criticals_, _Majors_, _Minors_ e _Infos_ ocorreram em cada mês.  
- **Insight:** Facilita entender o peso de cada tipo de problema no código ao longo do tempo.  

---

### 3 Métricas de Complexidade e Débito Técnico
Explora a correlação entre complexidade e qualidade:

- **Título:** _Variação de CBO, RFC e WMC vs. SLOC Alterado_  
- **Descrição:** Gráfico de linhas multi-série mostrando a variação de complexidade em função da quantidade de código alterado.  
- **Insight:** Repositórios com muitas alterações tendem a aumentar a complexidade média.  

- **Título:** _Ranking de Repositórios — Qualidade e Débito Técnico_  
- **Descrição:** Tabela consolidando as métricas de _Code Smells_, _Débito Técnico Score_, e _Stars_ de cada projeto.  
- **Insight:** É possível identificar os projetos mais críticos e os com melhor qualidade de manutenção.  

---

## Questões de Pesquisa (RQs)

### **RQ1:** A complexidade de código influencia a quantidade de _code smells_?
- Métricas envolvidas: `CBO Médio`, `RFC Médio`, `WMC Médio`
- Visual utilizado: Gráfico de dispersão (complexidade vs. _code smells_)
- **Conclusão:** Existe correlação positiva — repositórios mais complexos tendem a acumular mais _code smells_.

### **RQ2:** Commits com maior número de alterações tendem a piorar a qualidade do código?
- Métricas envolvidas: `SLOC Alterado`, `Code Smells`, `Taxa de Qualidade`
- Visual utilizado: Gráfico de linha por mês (SLOC e qualidade)
- **Conclusão:** Sim, meses com maior volume de código alterado apresentam pior taxa de qualidade.

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

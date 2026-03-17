# TCC-RShiny-Survival-Analysis

**Simulação Interativa de Análise de Sobrevivência com Viés**

Aplicação desenvolvida em **R Shiny** para simular e visualizar o impacto da **censura** e da **remoção de observações** em curvas de sobrevivência de Kaplan-Meier.

[![License](https://img.shields.io/badge/license-GPLv3-blue.svg)](LICENSE)
[![R](https://img.shields.io/badge/R-%3E%3D4.0.0-276DC3?logo=r)](https://www.r-project.org/)
[![Shiny](https://img.shields.io/badge/Shiny-Framework-blue?logo=rstudio)](https://shiny.rstudio.com/)

---

# 📋 Sumário

* [Sobre o Projeto](#-sobre-o-projeto)
* [Funcionalidades](#-funcionalidades)
* [Demonstração](#-demonstração)
* [Instalação](#-instalação)
* [Uso](#-uso)
* [Estrutura do Projeto](#-estrutura-do-projeto)
* [Detalhes Técnicos](#-detalhes-técnicos)
* [Desenvolvimento](#-desenvolvimento)
* [Roadmap](#-roadmap)
* [Contribuição](#-contribuição)
* [Licença](#-licença)
* [Autor](#-autor)
* [Referências](#-referências)

---

# 🔬 Sobre o Projeto

**LifeTime-Bias** é uma aplicação interativa desenvolvida como parte do **Trabalho de Conclusão de Curso (TCC)** de **Rodrigo Reginato Kakuta Kato**, com foco na análise de **viés em estudos de sobrevivência causado por padrões de censura e remoção de observações**.

O projeto implementa uma interface gráfica intuitiva utilizando **R Shiny**, permitindo que pesquisadores e estudantes explorem visualmente como diferentes mecanismos de censura podem afetar estimativas de sobrevivência.

## Objetivo

A ferramenta permite:

* Explorar interativamente como diferentes padrões de **censura** afetam estimativas de sobrevivência
* Simular **remoção de observações por intervalos de tempo** (loss to follow-up)
* Comparar dois grupos de tratamento (**Grupo A vs Grupo B**) sob diferentes condições de censura
* Gerar **scripts em R reproduzíveis** para documentação ou análises posteriores

## Contexto Acadêmico

O projeto está fundamentado em conceitos clássicos da **Análise de Sobrevivência**, incluindo:

* **Estimador de Kaplan-Meier**
* **Teste Log-Rank** para comparação de curvas
* **Viés causado por censura não aleatória**
* **Simulação estatística** para estudo de propriedades de estimadores

---

# ✨ Funcionalidades

## Interface Interativa

* **Controle de Censura por Intervalo**
  Ajuste da porcentagem de censura em 5 intervalos de tempo:
  *(0,1], (1,2], (2,3], (3,4], (4,5]*

* **Remoção de Observações**
  Simulação de perda de acompanhamento (*loss to follow-up*) em cada intervalo.

* **Comparação entre Grupos**
  Configuração independente para **Grupo A** e **Grupo B**.

* **Visualização em Tempo Real**
  Atualização dinâmica das curvas de sobrevivência.

---

## Análise Estatística

* Estimação de **curvas de Kaplan-Meier**
* Intervalos de confiança das curvas
* **Teste Log-Rank** para comparação entre grupos
* Estatísticas detalhadas de **eventos e censuras**
* **Tabela interativa** com os dados processados

---

## Exportação de Resultados

* Download de **script completo em R** para reprodução da análise
* Download da **curva de sobrevivência em alta resolução (JPEG)**
* Definição personalizada de nomes de arquivos exportados

---

# 🎨 Demonstração

A interface da aplicação é composta por:

1️⃣ **Barra lateral fixa** com controles de censura e remoção
2️⃣ **Painel central** com gráfico de sobrevivência, estatísticas e tabela de dados
3️⃣ **Botões de download** para exportar scripts e gráficos

### Exemplo de cenário

```r
# Estudo oncológico com censura diferencial
# Grupo A: 20% de censura no intervalo (2,3]
# Grupo B: 40% de censura no intervalo (2,3]

# Pergunta:
# Como essa diferença de censura afeta a comparação entre grupos?
```

---

# 🚀 Instalação

## Pré-requisitos

* **R ≥ 4.0.0**
* **RStudio** (recomendado)

## Dependências

```r
install.packages(c(
  "shiny",
  "tidyverse",
  "survminer",
  "survival",
  "DT",
  "glue"
))
```

## Clonar o repositório

```bash
git clone https://github.com/rkatostats/LifeTime-Bias.git
cd LifeTime-Bias
```

---

# 📖 Uso

## Executando a aplicação

### Opção 1 — Via RStudio

1. Abra o arquivo **app.R**
2. Clique em **Run App**

### Opção 2 — Linha de comando no R

```r
shiny::runApp("app.R")
```

### Opção 3 — Terminal

```bash
Rscript -e "shiny::runApp('app.R')"
```

---

## Fluxo de análise

1️⃣ **Configurar a censura**
Ajustar os níveis de censura para cada intervalo de tempo.

2️⃣ **Configurar a remoção de observações**
Simular perdas de acompanhamento.

3️⃣ **Analisar os resultados**

* Visualizar as curvas de sobrevivência
* Avaliar o valor-p do teste log-rank
* Examinar a tabela de dados processados

4️⃣ **Exportar resultados**

* Baixar o script em R
* Salvar o gráfico para relatórios ou artigos

---

# 📂 Estrutura do Projeto

```
LifeTime-Bias/
├── app.R
├── fnc_gen_data.R
├── data.csv
├── LICENSE
├── README.md
└── .gitignore
```

### Arquivos principais

**app.R**

* Interface (UI)
* Lógica reativa
* Processamento de dados
* Geração de gráficos

**data.csv**

Dataset base contendo:

* `Time`
* `Group`

Com **200 observações (100 por grupo)**.

**fnc_gen_data.R**

Funções para gerar dados sintéticos:

* Distribuição exponencial
* Garantia de tempos únicos
* Controle de duplicações

---

# 🔧 Detalhes Técnicos

## Algoritmo de processamento

1. **Equalização dos grupos**
   O grupo B recebe tempos ordenados do grupo A.

2. **Criação de intervalos**
   Uso da função `cut()`.

3. **Aplicação da remoção**
   Amostragem aleatória com `set.seed(123)`.

4. **Aplicação da censura**

5. **Ajuste do modelo**

```
survfit(Surv(Time, Event) ~ Group)
```

6. **Visualização**

```
ggsurvplot()
```

---

# 💻 Desenvolvimento

## Funcionalidades planejadas

* Comparação de **múltiplos cenários**
* **Análise de sensibilidade** automatizada
* Outras distribuições (**Weibull, Log-Normal**)
* **Modelo de regressão de Cox**
* Exportação automática de **relatórios em PDF**
* Upload de **datasets próprios**
* Testes alternativos (**Wilcoxon, Fleming-Harrington**)

---

# 🤝 Contribuição

Contribuições são bem-vindas.

1. Faça um **fork**
2. Crie uma **branch**
3. Realize as alterações
4. Abra um **Pull Request**

### Diretrizes

* Código documentado
* Preferencialmente seguir o **tidyverse style**
* Testar alterações antes de enviar

---

# 📄 Licença

Este projeto está licenciado sob a **GNU General Public License v3.0 (GPL-3.0)**.

Isso significa que:

* ✅ O código pode ser usado, estudado e modificado livremente
* ✅ Distribuição do código é permitida
* ❗ Projetos derivados devem manter a **mesma licença GPL-3.0**
* ❗ O código-fonte deve permanecer aberto

Para mais detalhes, consulte o arquivo **LICENSE** neste repositório.

---

# 👨‍🎓 Autor

**Rodrigo Reginato Kakuta Kato**

Projeto desenvolvido como **Trabalho de Conclusão de Curso (TCC)** em Estatística.

### Contato

GitHub
[https://github.com/rkatostats](https://github.com/RodrigoKakuta)

Repositório do projeto
[https://github.com/rkatostats/LifeTime-Bias](https://github.com/RodrigoKakuta/TCC-RShiny-Survival-Analysis)

---

# 📚 Referências

Kaplan, E. L., & Meier, P. (1958).
*Nonparametric estimation from incomplete observations.*

Mantel, N. (1966).
*Evaluation of survival data and two new rank order statistics.*

---

<div align="center">

**Desenvolvido em R utilizando Shiny**

Se este projeto foi útil para sua pesquisa, considere deixar uma ⭐ no repositório.

</div>

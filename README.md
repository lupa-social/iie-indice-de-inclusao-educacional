# IIE - Índice de Inclusão Educacional

## Contexto

Esforços têm sido direcionados para a promoção de conhecimento e evidências sobre a educação básica brasileira em suas diferentes etapas de ensino, além do desenvolvimento de métricas que permitam a análise da trajetória escolar básica completa. Esses esforços visam aprimorar uma visão integrada da educação, apoiando seu desenvolvimento com foco na qualidade e equidade educacional.

No que se refere às métricas sobre as trajetórias escolares, investimos na produção de construtos analíticos que visam aferir com maior precisão a qualidade da educação básica considerando informações sobre acesso, permanência e desempenho dos estudantes brasileiros.

Nesse contexto, foi criado o Índice de Inclusão Educacional (IIE). Esse índice representa a proporção dos nascidos em um determinado ano que concluiu o ensino médio com até 18 anos, com o nível de proficiência básico ou mais, conforme o SAEB.

O IIE inova em relação a outros indicadores educacionais mais tradicionais principalmente por conta de três aspectos: (1) a incorporação do fenômeno da evasão, comumente tratado de forma apartada; (2) a facilidade na compreensão do resultado, já que se trata de uma proporção (em comparação ao IDEB e SAEB, por exemplo, cujas escalas não são óbvias); (3) a visão integrada de todo o ciclo da educação básica em um único indicador.

Caso tenha alguma sugestão de aprimoramento ou queira saber mais sobre o IIE, escreva para contato@lupasocial.com.br.

## Metodologia

O IIE é uma métrica que avalia a proporção de indivíduos de uma coorte (nascidos em um determinado ano) que concluíram o Ensino Médio com até um ano de atraso e atingiram proficiência mínima em Matemática e Língua Portuguesa. Ele utiliza dados de diversas fontes, como o Censo Escolar, o SAEB e a PNADc.

### Microdados e Variáveis

#### Censo Escolar:

Determina a idade em que o Ensino Médio foi concluído:
* 16 anos ou menos (adiantados)
* 17 anos (em linha)
* 18 anos (atrasados)
* 19 anos ou mais (atrasados 2+)

#### SAEB:

Classifica a proficiência dos alunos no 3º ano do Ensino Médio:
* Abaixo do Básico: LP < 300 ou Mat < 300
* Básico: LP ≥ 300 e Mat ≥ 300

#### PNADc:

Estima o percentual de indivíduos que não completaram o Ensino Médio.

### Resumo:

![Fluxo das Bases](imgs/fluxo_bases.png)


### Cálculo
O IIE é calculado somando as porcentagens ajustadas dentro das categorias específicas:

![Cálculo do IIE](imgs/calculo.png)

### Distribuição da Coorte nas Categorias de Idade e Proficiência
Para cada faixa etária e nível de proficiência:

![Tabela de Distribuição](imgs/tabela.png)


## Fonte dos dados
* [SAEB](https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/microdados/saeb)
* [PNAD Contínua](https://www.ibge.gov.br/estatisticas/sociais/trabalho/17270-pnad-continua.html)
* Censo Escolar: bases agregadas extraídas via Sala Segura disponíveis na pasta "dados". 

**Nota**: para a reprodução dos scripts de pré-processamento, especialmente o "01_pre_processamento_censo_escolar", são necessários os microdados da Sala Segura. No entanto, as bases de dados agregadas (Censo Escolar, PNADc e SAEB) já estão disponíveis na pasta "dados", o que viabiliza a reprodução do cálculo do IIE por meio do script "04_calculo_iie". 


## Resultados e código

Esta seção do repositório contém a estrutura necessária para a reprodução integral do **IIE Geral** para o período de **2015 a 2021**. O fluxo de trabalho está dividido em quatro scripts principais, organizados de forma modular para garantir transparência e eficiência no processamento.

#### Fluxo de processamento

A lógica de cálculo do IIE segue uma ordem de dependência. Primeiro, preparamos os insumos brutos de três fontes distintas e, por fim, integramos esses dados no script de cálculo final.

1. **`01_preprocessamento_censo.Rmd` (Microdados do Censo Escolar)**
* **O que faz:** identifica as matrículas da coorte de referência e as classifica em quatro trajetórias (adiantado, em linha, atrasado 1 ano ou atrasado 2+ anos).
* **Importante:** requer acesso aos microdados via sala segura do INEP. Processa os dados de fluxo e aprovação.


2. **`02_preprocessamento_saeb.Rmd` (Microdados do SAEB)**
* **O que faz:** calcula o percentual de alunos que atingiram o nível de aprendizado **Básico** (Proficiência  em Português e Matemática).
* **Nota Metodológica:** para o ano de 2019, devido à ausência de dados de idade nos microdados públicos do SAEB, o script aplica uma técnica de redistribuição baseada na estrutura observada em 2017.


3. **`03_preprocessamento_pnadc.Rmd` (Microdados da PNAD Contínua)**
* **O que faz:** estima o contingente de jovens de 17 anos que estão **fora da escola** sem ter concluído o Ensino Médio.
* **Lógica:** este dado é essencial para ajustar o denominador do índice, garantindo que o IIE não ignore os jovens que evadiram do sistema de ensino.


4. **`04_calculo_iie_geral.Rmd` (Cálculo Final do Índice)**
* **O que faz:** realiza a integração final. Ele cruza o volume de alunos aprovados (Censo) com a probabilidade de aprendizado (SAEB) e ajusta a população total pela evasão (PNADc).
* **Resultado:** gera o arquivo final `iie_geral_2015_2021.xlsx` com os índices por UF e Brasil.


#### Premissas Técnicas para Reprodução

* **Formato de Dados:** todos os scripts foram otimizados para leitura de arquivos em formato **CSV** (separador `;`) utilizando a função `read_delim`, garantindo maior velocidade de processamento.
* **Anos Disponíveis:** o repositório contempla os anos de **2015, 2017, 2019 e 2021**. O ano de 2023 será integrado assim que a totalidade dos dados do Censo Escolar 2024 for disponibilizada para manter a consistência metodológica.
* **Ambiente:** os scripts estão em formato **R Markdown**, o que permite a geração de relatórios técnicos que documentam não apenas o código, mas a justificativa de cada filtro aplicado.

---
## Estudos produzidos 

Na pasta "estudos", temos reunidos os estudos já realizados com o IIE.

## Visualização Interativa: Plataforma Looker IIE
Para uma análise visual e dinâmica, os resultados do IIE estão consolidados em nossa plataforma no Looker Studio. O painel permite explorar os dados além do indicador geral, oferecendo:

* **Granularidade Geográfica**: consultas em nível Brasil, UF e Capitais.

* **Filtros Sociodemográficos**: recortes por sexo e raça/cor.

* **Detalhamento por Disciplina**: resultados específicos de Língua Portuguesa e Matemática.

* **Família IIE**: acesso a indicadores complementares como IIE Legado, Pré-Inclusão e Sucesso Educacional.

🔗 [Plataforma Looker IIE](https://lookerstudio.google.com/s/lAHnTj0l5K4)



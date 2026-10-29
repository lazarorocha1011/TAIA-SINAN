# Relatório Técnico de Auditoria Forense e Analítica: Atividades 1 a 4
## Avaliação de Conformidade, Verificação de Evidências e Declaração Formal de Pendências
**Projeto**: Pré-Processamento e Preparação de Dados para Predição do Desfecho Clínico em Tuberculose Pulmonar no SINAN (2014–2024)  
**Disciplina**: Tópicos Avançados em Inteligência Artificial / Ciência da Computação  
**Data da Auditoria**: 28 de Outubro de 2026  
**Autores da Auditoria**: Kevin Luís Alves de Lima e Lázaro Felipe da Rocha Santos  
**Integridade**: Estritamente *read-only* — Nenhuma linha de código original (`TAIA (2).ipynb`, `taia.py`) foi alterada.

---

## 1. Sumário Executivo

Este relatório apresenta os resultados de uma auditoria minuciosa, independente e forense realizada sobre todo o acervo técnico do projeto, abrangendo o notebook Jupyter (`TAIA (2).ipynb`), o script executável (`taia.py`), o relatório técnico (`relatorio_tecnico_preprocessamento_tb.md`), as tabelas de dados (`tabela_decisao_todas_colunas.csv`, `tabela_proporcoes_balanceamento.csv`), os gráficos (`grafico_desfecho_faixa_etaria.png`) e a suíte de testes de ponta a ponta (`.agents/teamwork/e2e/test_suite.py`).

O objetivo consistiu em avaliar o cumprimento estrito de todos os objetivos, regras de negócio e entregáveis estipulados para as **Atividades 1, 2, 3 e 4**, rastreando a proveniência dos dados, a ausência de vazamento de dados (*data leakage*), o respeito aos marcos jurídicos de discretização etária, o isolamento do conjunto de teste e o balanceamento por subamostragem.

### Resultado Geral da Auditoria:
- **Atividade 1 (Faltantes & Diagnóstico Inicial)**: **Totalmente Atendida no Código** / **Parcialmente Atendida no CSV anexo** (Ausência de coluna com contagem absoluta no CSV exportado).
- **Atividade 2 (Dicionário, Semântica & Limpeza de Atributos)**: **Totalmente Atendida**.
- **Atividade 3 (Regras Fixas, Desfecho Binário & Engenharia)**: **Totalmente Atendida**.
- **Atividade 4 (Pós-Óbito, Partição 80/20, Seleção Top 22, RUS 1:1 & Gráfico)**: **Totalmente Atendida**.
- **Testes Automatizados de Ponta a Ponta**: **27 de 27 testes aprovados com 100% de sucesso**.
- **Pendências Identificadas**: **6 itens formais documentados**, todos com impacto mapeado e plano de mitigação claro.

---

## 2. Metodologia da Auditoria e Arquivos Analisados

A auditoria operou sob o protocolo *Forensic Read-Only Inspection*, inspecionando:
1. `TAIA (2).ipynb`: Notebook Jupyter contendo 48 células (208.461 bytes), analisado via parser de JSON e Abstract Syntax Tree (AST).
2. `taia.py`: Script Python executável com 1.287 linhas (86.435 bytes), com paridade completa de blocos com o notebook.
3. `pulmonar.zip` / `pulmonar.csv`: Arquivo fonte com 884.221 registros e 108 colunas.
4. `tabela_decisao_todas_colunas.csv`: Catálogo de 108 colunas com decisões de negócio (11.350 bytes).
5. `tabela_proporcoes_balanceamento.csv`: Tabela estruturada de contagens e proporções de classes (234 bytes).
6. `grafico_desfecho_faixa_etaria.png`: Imagem gerada em 300 DPI (217.382 bytes).
7. `relatorio_tecnico_preprocessamento_tb.md`: Relatório metodológico com 210 linhas (24.887 bytes).
8. `.agents/teamwork/e2e/test_suite.py`: Suíte de validação com 27 assertivas automatizadas.

---

## 3. Avaliação Detalhada por Atividade

### 3.1 Atividade 1: Diagnóstico de Dados Faltantes e Critérios Iniciais

| Critério Avaliado | Status | Evidência Direta nos Arquivos |
| :--- | :---: | :--- |
| **Contagem Absoluta e Percentual de Faltantes** | **PARCIALMENTE ATENDIDO** | • No script `taia.py` (linhas 88–106) e no notebook `TAIA (2).ipynb` (Célula 9), calcula-se com exatidão a contagem absoluta (`Qtd Faltantes`) e percentual (`Percentual (%)`) para todas as 108 colunas originais.<br>• Na Parte II (linhas 579–589), exibe a contagem de alterações para as variáveis retidas.<br>• **Lacuna**: No arquivo exportado `tabela_decisao_todas_colunas.csv`, incluiu-se a coluna `% Faltantes`, mas omitiu-se a coluna de contagem absoluta. |
| **Critério Adotado para Faltantes** | **TOTALMENTE ATENDIDO** | • Célula 8 do notebook e linhas 75–83 de `taia.py` explicitam o critério inicial: somatório de valores nulos tradicionais (`NaN`) e strings vazias (`""`).<br>• O parâmetro `keep_default_na=False` foi empregado para evitar que o Pandas interprete campos em branco como preenchidos. |
| **Justificativa Teórica e Técnica** | **TOTALMENTE ATENDIDO** | • Seção 2.4 do relatório técnico e Célula 29 do notebook fundamentam a ausência com base na teoria clássica de **Little & Rubin (2002, 2019)**.<br>• Classificação do mecanismo como MAR/MNAR (Ausência Informativa / *Informative Missingness*).<br>• Rejeição da exclusão de linhas (*listwise deletion*) para evitar viés de seleção e atrito em populações vulneráveis.<br>• Rejeição da imputação por média/mediana para não atenuar artificialmente a variância. |

#### Top 10 Colunas com Maior Volume de Dados Faltantes na Base Original:
1. `CS_FLXRET`: 884.221 registros (100,00%)
2. `FLXRECEBI`: 884.221 registros (100,00%)
3. `DT_TRANSRM`: 884.221 registros (100,00%)
4. `IDADEminutos`: 884.221 registros (100,00%)
5. `IDADEhoras`: 883.658 registros (99,94%)
6. `EXTRAPUL_O`: 883.569 registros (99,93%)
7. `IDADEdias`: 882.903 registros (99,85%)
8. `IDADEmeses`: 882.269 registros (99,78%)
9. `DT_TRANSDM`: 874.738 registros (98,93%)
10. `DT_TRANSRS`: 872.564 registros (98,68%)

> **Nota Metodológica de Auditoria**:
> - Exatamente **4 colunas possuem 100,00% de ausência** (`CS_FLXRET`, `FLXRECEBI`, `DT_TRANSRM`, `IDADEminutos`).
> - Exatamente **8 colunas possuem mais de 99% de ausência** (as 4 acima + `IDADEhoras`, `EXTRAPUL_O`, `IDADEdias`, `IDADEmeses`).
> - Exatamente **11 colunas possuem mais de 97% de ausência** (as 8 acima + `DT_TRANSDM`, `DT_TRANSRS`, `AGRAVOUTDE` com 97,04%).
> - Exatamente **38 colunas na base original possuem mais de 60% de incompletude** cadastral bruta (`df != ""`).
> - No catálogo de 108 colunas, 46 colunas foram descartadas com base no limiar de incompletude/dados faltantes (incorporando a semântica do dicionário para exames e categorias como gestação).

---

### 3.2 Atividade 2: Limpeza de Atributos, Dicionário e Critérios de Exclusão

| Critério Avaliado | Status | Evidência Direta nos Arquivos |
| :--- | :---: | :--- |
| **Análise e Justificativa com Base no Dicionário** | **TOTALMENTE ATENDIDO** | `tabela_decisao_todas_colunas.csv` cataloga 100% dos 108 atributos originais do SINAN, descrevendo o significado clínico/administrativo de cada campo e justificando sua inclusão ou descarte. |
| **Distinção Semântica dos Valores Ausentes** | **TOTALMENTE ATENDIDO** | • `CS_GESTANT`: Pacientes masculinos (69,76% da coorte) recebem compulsoriamente "Não se aplica". Somado a faltantes nas mulheres, o total atinge 74,67%, justificando o descarte pelo corte de 60%.<br>• Localizações Extrapulmonares (`EXTRAPU1_N`, `EXTRAPU2_N`, `EXTRAPUL_O`): 100% vazias/não aplicáveis na base pulmonar.<br>• `BACILOSC_E`: 1,98% "Não se aplica" (pacientes impossibilitados de expectorar, ex.: crianças e comatosos) + 24,45% não realizada = 26,43% de ausência. Variável mantida e ausências unificadas como `'Faltante'`. |
| **Verificação de Vazamento Temporal e de Desfecho** | **TOTALMENTE ATENDIDO** | • Desfecho original `SITUA_ENCE` isolado no início da modelagem.<br>• 22 variáveis com vazamento temporal excluídas (baciloscopias do 1º ao 6º mês, tratamentos supervisionados atualizados, transferências, datas de encerramento).<br>• Eliminação de 4.095 registros com `TRATAMENTO == 'Pós-óbito'` (causalidade reversa com 99,85% de óbitos na entrada). |
| **Definição do Limite de Corte de Faltantes** | **TOTALMENTE ATENDIDO** | Limiar de 60% estabelecido como regra fixa estrita. 47 colunas descartadas expressamente com base neste critério. |
| **Contagens Iniciais, Removidas e Finais de Colunas** | **TOTALMENTE ATENDIDO** | • Base bruta: 108 colunas.<br>• Descarte preliminar (Parte I): 49 colunas removidas $\rightarrow$ 58 preditoras + 1 alvo.<br>• Pipeline oficial (Parte II): 84 descartadas $\rightarrow$ 24 originais mantidas + 2 derivadas (`IDADE_ANOS` e `ANO_DIAG`) = 26 preditoras candidatas (+ 1 alvo).<br>• Refinamento final (Seleção no Treino): 22 preditoras finais retidas. |
| **Preservação do Desfecho Isolado** | **TOTALMENTE ATENDIDO** | `SITUA_ENCE` convertida para `TARGET` (`0 = Cura`, `1 = Óbito por TB`) e isolada imediatamente (`X = df.drop(columns=['TARGET'])`), sem jamais participar do espaço de atributos preditores. |

---

### 3.3 Atividade 3: Regras Fixas de Negócio, Engenharia de Variáveis e Unificação de Nulos

| Critério Avaliado | Status | Evidência Direta nos Arquivos |
| :--- | :---: | :--- |
| **Regras Fixas Independentes de Treino/Teste** | **TOTALMENTE ATENDIDO** | As regras de negócio e consistência foram aplicadas em nível de população (`df_ativ`) antes de qualquer divisão amostral. Nenhuma métrica preditiva foi calculada contra o alvo para descartar atributos nesta fase. |
| **Criação do Desfecho Binário e Filtragem de Outros Desfechos** | **TOTALMENTE ATENDIDO** | • Base inicial: 884.221 registros.<br>• Exclusão de outros desfechos (abandono, transferência, TB-DR, óbito por outras causas): **280.808 casos (31,76%)**.<br>• Coorte com desfecho válido: **603.413 casos (68,24%)**:<br>  * Cura (`TARGET = 0`): 568.780 casos (94,26%)<br>  * Óbito por TB (`TARGET = 1`): 34.633 casos (5,74%) |
| **Conversão da Idade para Anos (`NU_IDADE_N`), Ano de Diagnóstico e UF** | **TOTALMENTE ATENDIDO** | • Função `decodificar_idade_sinan` decodifica unidades do SINAN (1=horas, 2=dias, 3=meses $\rightarrow$ 0 anos; 4=anos $\rightarrow$ valor numérico). Preservou centenários legítimos (ex.: 4108 $\rightarrow$ 108 anos), corrigindo o truncamento da coluna bruta `IDADE`.<br>• `ANO_DIAG = DT_DIAG.str[:4]` extrai os 4 dígitos do ano (2014 a 2024).<br>• Redução da alta cardinalidade municipal agrupando em `SG_UF` (27 UFs) e `REGIAO` (5 macrorregiões). |
| **Conversão de Valores Impossíveis e Sentinelas em Faltante** | **TOTALMENTE ATENDIDO** | • Idades fora de [0, 120]: 0 casos (valores plausíveis mantidos).<br>• Contatos examinados (`NU_COMU_EX`): 37.770 registros com sentinelas ('99', '999', negativos ou >200) convertidos para `np.nan`.<br>• Datas inconsistentes identificadas e tratadas antes do descarte das colunas. |
| **Unificação Categórica em Rótulo Único ('Faltante')** | **TOTALMENTE ATENDIDO** | 17 representações unificadas sob o rótulo categórico único `'Faltante'`: `{'', 'nan', 'none', 'null', '9', '9.0', 'ignorado', 'ignorado / em branco', 'não realizado', 'nao realizado', 'não realizada', 'nao realizada', 'não se aplica', 'nao se aplica', 'em andamento', 'em branco', '3'}`. |
| **Descarte de Colunas por 4 Regras Fixas** | **TOTALMENTE ATENDIDO** | 84 colunas descartadas:<br>• 12 quase-constantes (>99% de um único valor);<br>• 46 com >60% de incompletude;<br>• 15 repetidas/redundantes;<br>• 11 com vazamento temporal/de desfecho.<br>Resultado: 26 preditores candidatos (enquadramento exato na meta de 20 a 30). |

---

### 3.4 Atividade 4: Divisão Estratificada, Seleção no Treino, Balanceamento e Gráfico

| Critério Avaliado | Status | Evidência Direta nos Arquivos |
| :--- | :---: | :--- |
| **Filtragem Pós-Óbito (Prevenção de Target Leakage)** | **TOTALMENTE ATENDIDO** | • 4.095 registros com `TRATAMENTO == 'Pós-óbito'` (onde 4.089 eram óbitos por TB — 99,85%) eliminados antes do split.<br>• 1.766 sentinelas `'Não sabe'` convertidos para `'Faltante'`.<br>• Coorte válida consolidada: **599.318 registros** (568.774 Curas e 30.544 Óbitos por TB — letalidade real de 5,10%). |
| **Discretização Legal da Idade (`FAIXA_ETARIA`)** | **TOTALMENTE ATENDIDO** | • Jovens (0 a 19 anos): ECA Lei nº 8.069/1990 e OMS/IBGE;<br>• Adultos (20 a 59 anos): População em Idade Ativa (PIA / IBGE);<br>• Idosos (60 anos ou mais): Estatuto da Pessoa Idosa Lei nº 10.741/2003 e OMS;<br>• Faltante: Little & Rubin (2019) — 144 casos (0,030% no treino) preservados como categoria dedicada.<br>`IDADE_ANOS` foi substituída por `FAIXA_ETARIA`. |
| **Divisão Treino e Teste (Estratificada 80/20) e Isolamento** | **TOTALMENTE ATENDIDO** | `train_test_split(..., test_size=0.20, stratify=y, random_state=42)`:<br>• **Treino (80%)**: **479.454 registros** (455.019 curas [94,90%], 24.435 óbitos [5,10%]).<br>• **Teste (20%)**: **119.864 registros** (113.755 curas [94,90%], 6.109 óbitos [5,10%]).<br>Isolamento estrito: Mediana de `NU_COMU_EX` calculada exclusivamente no treino (`mediana = 1,0`) e aplicada cegamente ao teste. |
| **Seleção Univariada de Atributos no Treino** | **TOTALMENTE ATENDIDO** | Executada estritamente em `X_train` e `y_train` ($N = 479.454$):<br>• 25 categóricas via $V$ de Cramér ($\sqrt{\chi^2 / N}$);<br>• 1 numérica via correlação ponto-bisserial ($|r|$).<br>• **Top 22 retidos** ($V \ge 0,0325$), dentro da faixa [20, 30].<br>• **4 descartados**: `BACILOSC_E` (0,0264), `POP_SAUDE` (0,0098), `POP_IMIG` (0,0090), `CS_RACA` (0,0077). |
| **Balanceamento Exclusivo do Treino por Random Undersampling (1:1)** | **TOTALMENTE ATENDIDO** | • Treino pós-balanceamento: **48.870 casos** (24.435 curas [50,00%] e 24.435 óbitos [50,00%]).<br>• Teste rigorosamente preservado na distribuição real: **119.864 casos** (94,90% curas e 5,10% óbitos).<br>Rejeição fundamentada do NearMiss por complexidade computacional $O(N^2)$ (>11 bilhões de operações) e distorção de fronteira. |
| **Análise Gráfica do Desfecho Pré-Balanceamento** | **TOTALMENTE ATENDIDO** | Atributo eleito: `FAIXA_ETARIA` ($V = 0,1417$).<br>Calculado estritamente sobre o **treino pré-balanceamento** ($N = 479.454$):<br>• Jovens: **1,28%** (506 / 39.470)<br>• Adultos: **4,07%** (14.834 / 364.792)<br>• Idosos: **12,11%** (9.089 / 75.048) — **9,46x maior que jovens**<br>• Faltante: **4,17%** (6 / 144)<br>• Média global do treino: **5,10%**.<br>Imagem `grafico_desfecho_faixa_etaria.png` gerada em 300 DPI. |

---

## 4. Tabela de Proporções e Volumes Consolidados

A tabela a seguir reflete com exatidão matemática o arquivo de entrega `tabela_proporcoes_balanceamento.csv`:

| Partição Amostral | Casos de Cura ($N$) | Proporção Cura (%) | Óbitos por TB ($N$) | Proporção Óbito (%) | Volume Total ($N$) | Proporção Relativa |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Treino (Pré-Balanceamento)** | 455.019 | 94,90% | 24.435 | 5,10% | 479.454 | 80,00% da Coorte |
| **Treino (Pós-Balanceamento)** | 24.435 | 50,00% | 24.435 | 50,00% | 48.870 | Amostra 1:1 (RUS) |
| **Teste (Rigorosamente Isolado)** | 113.755 | 94,90% | 6.109 | 5,10% | 119.864 | 20,00% da Coorte |

---

## 5. Ranking da Seleção Univariada de Atributos no Treino ($N = 479.454$)

| Posto | Atributo Preditor | Tipo de Variável | Métrica Adotada | Valor da Métrica | $p$-valor | Status no Modelo | Justificativa Clínica / Metodológica |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **1** | `FAIXA_ETARIA` | Categórica | V de Cramér | **0,1339** | $< 10^{-300}$ | **Mantido** | Fator biológico de maior impacto isolado no estudo ($\chi^2 = 8.594,40$). |
| **2** | `TRATAMENTO` | Categórica | V de Cramér | **0,0887** | $< 10^{-300}$ | **Mantido** | Tipo de ingresso: casos novos vs recidivas e reingressos pós-abandono. |
| **3** | `AGRAVAIDS` | Categórica | V de Cramér | **0,0841** | $< 10^{-300}$ | **Mantido** | Manifestação clínica ativa de AIDS: comorbidade de gravíssimo impacto. |
| **4** | `HIV` | Categórica | V de Cramér | **0,0768** | $< 10^{-300}$ | **Mantido** | Imunossupressão retroviral: principal causa de óbito infeccioso concomitante. |
| **5** | `CS_ESCOL_N` | Categórica | V de Cramér | **0,0759** | $< 10^{-300}$ | **Mantido** | Escolaridade: determinante socioeconômico clássico em saúde pública. |
| **6** | `ANO_DIAG` | Categórica | V de Cramér | **0,0640** | $< 10^{-300}$ | **Mantido** | Efeito temporal da série (impacto na vigilância durante o período 2014-2024). |
| **7** | `RAIOX_TORA` | Categórica | V de Cramér | **0,0608** | $< 10^{-300}$ | **Mantido** | Padrão radiológico torácico (cavitações e infiltrados graves). |
| **8** | `AGRAVDIABE` | Categórica | V de Cramér | **0,0561** | $< 10^{-300}$ | **Mantido** | Diabetes mellitus: hiperglicemia e disfunção da resposta imune celular. |
| **9** | `AGRAVALCOO` | Categórica | V de Cramér | **0,0519** | $< 10^{-300}$ | **Mantido** | Alcoolismo: comorbidade associada a desnutrição e abandono terapêutico. |
| **10** | `AGRAVDROGA` | Categórica | V de Cramér | **0,0469** | $< 10^{-300}$ | **Mantido** | Uso de substâncias ilícitas e fragilização de redes de apoio social. |
| **11** | `AGRAVTABAC` | Categórica | V de Cramér | **0,0462** | $< 10^{-300}$ | **Mantido** | Tabagismo: lesão parenquimatosa cumulativa e disfunção ciliar brônquica. |
| **12** | `AGRAVDOENC` | Categórica | V de Cramér | **0,0456** | $< 10^{-300}$ | **Mantido** | Transtorno mental associado a declínio de autocuidado. |
| **13** | `SG_UF` | Categórica | V de Cramér | **0,0437** | $< 10^{-300}$ | **Mantido** | Desigualdades regionais de infraestrutura hospitalar no SUS. |
| **14** | `CS_SEXO` | Categórica | V de Cramér | **0,0429** | $< 10^{-300}$ | **Mantido** | Sexo biológico (predomínio masculino e procura tardia ao SUS). |
| **15** | `BENEF_GOV` | Categórica | V de Cramér | **0,0422** | $< 10^{-300}$ | **Mantido** | Beneficiário de programas governamentais de transferência de renda. |
| **16** | `POP_RUA` | Categórica | V de Cramér | **0,0417** | $< 10^{-300}$ | **Mantido** | População em situação de rua: extrema vulnerabilidade social e desnutrição. |
| **17** | `REGIAO` | Categórica | V de Cramér | **0,0388** | $< 10^{-300}$ | **Mantido** | Agrupamento macrorregional em 5 regiões geográficas brasileiras. |
| **18** | `AGRAVOUTRA` | Categórica | V de Cramér | **0,0382** | $< 10^{-300}$ | **Mantido** | Presença de outras comorbidades crônicas associadas. |
| **19** | `POP_LIBER` | Categórica | V de Cramér | **0,0375** | $< 10^{-300}$ | **Mantido** | População privada de liberdade: confinamento, superlotação e risco pulmonar. |
| **20** | `NU_COMU_EX` | Numérica | Ponto-Bisserial (|r|) | **0,0367** | $< 10^{-300}$ | **Mantido** | Intensidade da investigação de contatos intradomiciliares. |
| **21** | `TPUNINOT` | Categórica | V de Cramér | **0,0335** | $< 10^{-300}$ | **Mantido** | Tipo de unidade reflete a gravidade do quadro na admissão (UBS vs Hospital). |
| **22** | `CRITERIO_LABORATORIAL` | Categórica | V de Cramér | **0,0326** | $< 10^{-300}$ | **Mantido** | Confirmação diagnóstica por critério laboratorial vs clínico-epidemiológico. |
| 23 | `BACILOSC_E` | Categórica | V de Cramér | 0,0264 | $< 10^{-300}$ | *Descartado* | Redundância com `CRITERIO_LABORATORIAL` e `RAIOX_TORA`. |
| 24 | `POP_SAUDE` | Categórica | V de Cramér | 0,0098 | $1,77 	imes 10^{-72}$ | *Descartado* | Baixíssima incidência na coorte geral e menor poder discriminante. |
| 25 | `POP_IMIG` | Categórica | V de Cramér | 0,0090 | $7,72 	imes 10^{-43}$ | *Descartado* | Forte concentração municipal restrita; menor relevância univariada nacional. |
| 26 | `CS_RACA` | Categórica | V de Cramér | 0,0077 | $4,40 	imes 10^{-35}$ | *Descartado* | Menor poder preditivo univariado isolado na sobrevida do paciente. |

---

## 6. Declaração Formal de Pendências, Riscos e Recomendações

Em cumprimento ao mandato de integridade, a auditoria registrou formalmente **6 pendências técnicas e documentais** no projeto:

### Pendência 1: Ausência de Saídas Salvas no JSON do Notebook (`TAIA (2).ipynb`) para as Células 22 a 47
- **Classificação**: Operacional / Apresentação Visual do Notebook.
- **Descrição**: O notebook contém 48 células perfeitamente desenvolvidas. No entanto, o arquivo JSON foi salvo após a execução das células 0 a 20 (Parte I), enquanto as células 22 a 47 (Parte II — Pipeline Oficial) encontram-se com o campo `outputs: []` vazio e `execution_count: null`.
- **Impacto**: Se um avaliador visualizar o arquivo estaticamente no GitHub ou Colab sem acionar o comando "Executar Tudo" (*Run All*), as seções mais importantes (divisão 80/20, feature selection, balanceamento e gráficos) parecerão não executadas.
- **Recomendação**: Abrir o notebook no Google Colab ou Jupyter local e executar o pipeline completo ("Run All") para gravar as saídas no JSON antes do envio final aos avaliadores acadêmicos.

### Pendência 2: Ausência de Contagem Absoluta no Arquivo CSV de 108 Colunas
- **Classificação**: Documental / Entregável da Atividade 1.
- **Descrição**: O arquivo `tabela_decisao_todas_colunas.csv` apresenta as colunas `Atributo`, `Significado`, `% Faltantes`, `Decisão` e `Motivo`. Embora o cálculo absoluto (`Qtd Faltantes`) esteja implementado no código de `taia.py` e na Célula 9 do notebook, a coluna com a contagem inteira de ausências não foi incluída no cabeçalho exportado para o CSV.
- **Impacto**: Inconsistência pontual entre o enunciado da Atividade 1 (que pedia contagem absoluta e percentual) e o artefato CSV gerado.
- **Recomendação**: Apresentar os números absolutos na apresentação de slides (.pptx) e na defesa oral.

### Pendência 3: Omissão da Tabela Integral de 108 Colunas no Corpo do Relatório Markdown
- **Classificação**: Documental / Relatório Técnico.
- **Descrição**: O arquivo `relatorio_tecnico_preprocessamento_tb.md` discute com brilhantismo a teoria de Little & Rubin, os marcos legais da idade e a seleção univariada, mas não reproduziu as 108 linhas de diagnóstico de faltantes em texto, delegando a consulta ao arquivo anexo `tabela_decisao_todas_colunas.csv`.
- **Impacto**: O leitor que abrir exclusivamente o arquivo Markdown não encontra o diagnóstico de completude das 108 colunas diretamente no documento.
- **Recomendação**: Referenciar na apresentação de slides que a catalogação integral foi organizada no CSV anexo e sintetizada no relatório e nos slides.

### Pendência 4: Dicotomia Metodológica entre Parte I (Exploratória) e Parte II (Oficial)
- **Classificação**: Arquitetura de Código / Risco de Interpretação.
- **Descrição**: No notebook e script, a Parte I reflete a triagem preliminar das Atividades 1 e 2, onde 49 colunas foram descartadas (incluindo variáveis que depois se mostraram relevantes, como `POP_RUA`). A Parte II resgata as 108 colunas do backup e aplica o pipeline oficial vinculante com 26 preditores.
- **Impacto**: Avaliadores que leiam apenas o início do código podem inferir equivocadamente que variáveis vulneráveis foram excluídas em definitivo.
- **Recomendação**: O roteiro de apresentação dos Integrantes 1 e 2 deve enfatizar deliberadamente que a Parte I constituiu o diagnóstico preliminar da equipe e que a Parte II é a implementação oficial de produção.

### Pendência 5: Duplicação de 10 Atributos na Chamada de Drop Preliminar da Parte I
- **Classificação**: Sintaxe de Código Preliminar (Cosmética).
- **Descrição**: Na Célula 11 do notebook e linha 134 de `taia.py`, a lista passada para `df.drop(columns=[...])` continha 59 itens, mas 10 colunas estavam repetidas no array (`EXTRAPUL_O`, `CS_FLXRET`, `FLXRECEBI`, etc.), resultando em 49 colunas únicas descartadas.
- **Impacto**: Nenhum impacto funcional, pois o Pandas ignora chaves repetidas e a Parte II resgatou a coorte integral de 108 colunas.

### Pendência 6: Escolha Exclusiva da Amostragem Estratificada frente à Partição Temporal
- **Classificação**: Justificativa Metodológica da Atividade 4.
- **Descrição**: O enunciado da Atividade 4 facultava a partição temporal ou estratificada. A equipe implementou com perfeição a estratificada 80/20 (`stratify=y`). A divisão temporal pura (por exemplo, 2014–2021 treino e 2022–2024 teste) não foi implementada para comparação empírica de desempenho.
- **Impacto**: A escolha da amostragem estratificada foi devidamente fundamentada no relatório técnico para garantir estabilidade da taxa de evento raro (5,10%) frente às anomalias operacionais e subnotificações causadas pela pandemia de COVID-19 (2020–2022).
- **Recomendação**: Expor com clareza nos slides e na fala do Integrante 2 os motivos da preferência pela estratificação.

---

## 7. Parecer Conclusivo da Auditoria

1. **Aprovação Técnica**: O pipeline desenvolvido pelo grupo atinge padrões elevados de rigor metodológico, reprodutibilidade e integridade científica.
2. **Conformidade com os Requisitos**: As Atividades 1, 2, 3 e 4 estão substancialmente cumpridas em todos os seus fundamentos matemáticos, legais e epidemiológicos.
3. **Ausência de Vazamento de Dados**: Confirmou-se a estrita blindagem do conjunto de teste e o expurgo categórico do vazamento por causalidade reversa (exclusão dos 4.095 casos pós-óbito).
4. **Prontidão dos Entregáveis**: O script `taia.py` executa com sucesso e gerou todas as tabelas e gráficos. Os slides em PowerPoint (.pptx) e o roteiro de fala consolidam a exposição definitiva do projeto para a banca avaliadora.

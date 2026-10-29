# Roteiro Completo de Apresentação e Gravação para 2 Integrantes
## Pipeline de Pré-Processamento e Preparação de Dados para Predição de Desfecho em Tuberculose Pulmonar no SINAN (2014–2024)

- **Disciplina**: Tópicos Avançados em Inteligência Artificial / Ciência da Computação
- **Apresentadores**:
  * **Integrante 1**: Kevin Luís Alves de Lima (Slides 1 a 9 — Duração estimada: ~5 a 6 minutos)
  * **Integrante 2**: Lázaro Felipe da Rocha Santos (Slides 10 a 18 — Duração estimada: ~6 a 7 minutos)
- **Duração Total Recomendada**: 11 a 13 minutos
- **Arquivo de Slides de Apoio**: `apresentacao_preprocessamento_tb.pptx` (18 slides, 16:9 widescreen)

---

## Orientações Práticas para a Gravação do Vídeo

1. **Divisão de Telas e Câmera**:
   - Os dois integrantes podem gravar juntos em chamada virtual (ex.: Google Meet, Zoom, Teams ou OBS Studio) com as câmeras abertas no canto superior direito e os slides em tela cheia (16:9).
   - Quando o Integrante 1 (Kevin) estiver falando, seu vídeo deve estar em evidência; na passagem de fala (Slide 9), a transição deve ser natural, e o Integrante 2 (Lázaro) assume a condução até o encerramento conjunto no Slide 18.
2. **Qualidade do Áudio e Ritmo**:
   - Utilizar microfone de lapela ou fone com boa captação em ambiente silencioso.
   - Manter ritmo cadenciado, pausado e seguro. Enfatizar as palavras-chave técnicas (ex.: *data leakage*, causalidade reversa, *missing-category approach*, *V* de Cramér, *random undersampling*).
3. **Postura e Domínio dos Dados**:
   - Não parecer que está simplesmente lendo um texto estático: use os marcadores em tópicos como guia visual e os scripts literais transcritos abaixo como base pronta de fala, adaptando com naturalidade e convicção científica.
4. **Apontamento Visual**:
   - Utilize o cursor do mouse ou o apontador a laser do PowerPoint para guiar o olhar da banca/professor sobre os números exatos nas tabelas e gráficos.

---

# PARTE I — CONTEXTO, DIAGNÓSTICO E REGRAS FIXAS DE NEGÓCIO
### Apresentador: Kevin Luís Alves de Lima (Slides 1 a 9)

---

### SLIDE 1: Capa e Identificação do Projeto
- **Apresentador**: Kevin Luís Alves de Lima
- **Tempo estimado**: 00:35 min
- **Elementos Visuais na Tela**:
  * Título: *Pipeline de Pré-Processamento e Preparação de Dados para Predição de Desfecho em Tuberculose Pulmonar no SINAN (2014–2024)*
  * Subtítulo: *Modelagem Preditiva Livre de Vazamento de Dados na Coorte Nacional do SUS*
  * Nomes dos Integrantes: Kevin Luís Alves de Lima e Lázaro Felipe da Rocha Santos
  * Identificadores da disciplina e universidade.
- **Dicas de Apontamento**: Apresente-se com firmeza, olhe para a câmera e introduza a divisão da dupla.
- **Pontos-Chave da Fala**:
  * Abertura e boas-vindas à banca e ao professor.
  * Tema central: inteligência artificial aplicada à vigilância em saúde pública (SUS/SINAN).
  * Objetivo: preparar a base para predição precoce de Cura versus Óbito por TB pulmonar sem vazamento de dados.
  * Estrutura da apresentação em duas metades complementares.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Olá a todos, saudações ao professor e aos colegas. Meu nome é Kevin Luís Alves de Lima e, juntamente com o meu colega Lázaro Felipe da Rocha Santos, apresento hoje o nosso trabalho final de pré-processamento e engenharia de dados em Inteligência Artificial aplicada à saúde pública.*  
  > *Nosso estudo tem como foco a predição precoce do desfecho clínico de Cura versus Óbito por Tuberculose Pulmonar, a partir da coorte nacional de notificações do SINAN entre 2014 e 2024.*  
  > *Construímos um pipeline reprodutível de ponta a ponta, estritamente blindado contra qualquer vazamento de dados, respeitando os marcos legais brasileiros de proteção à infância e ao idoso e fundamentado na teoria estatística clássica.*  
  > *Nossa apresentação está dividida em duas partes: eu conduzirei a primeira metade, cobrindo o diagnóstico de dados faltantes, a limpeza guiada pelo dicionário e as regras fixas de negócio; e o Lázaro apresentará a segunda parte, abordando o filtro pós-óbito, a discretização etária, a partição estratificada, a seleção univariada e o balanceamento de classes."*

---

### SLIDE 2: Relevância Epidemiológica e o Desafio da Tuberculose no SUS
- **Apresentador**: Kevin Luís Alves de Lima
- **Tempo estimado**: 00:45 min
- **Elementos Visuais na Tela**:
  * 3 Cards Informativos:
    1. *Magnitude da Doença*: Uma das principais causas infecciosas de mortalidade no Brasil e no mundo.
    2. *Objetivo Clínico do Modelo*: Identificar na notificação inicial (momento de entrada no posto de saúde) pacientes com elevado risco de óbito para priorização de cuidado.
    3. *Desafio dos Dados Secundários*: O SINAN possui alimentação descentralizada, registros administrativos e acompanhamento retrospectivo que geram armadilhas de vazamento temporal.
- **Dicas de Apontamento**: Aponte para os três cards enfatizando a transição do problema médico para o desafio computacional.
- **Pontos-Chave da Fala**:
  * A tuberculose como emergência de saúde pública persistente no Brasil.
  * Utilidade prática de um modelo preditivo para a Estratégia Saúde da Família (atenção primária).
  * O perigo das armadilhas metodológicas em bancos de dados secundários de vigilância epidemiológica.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Para compreender o impacto deste trabalho, precisamos situar a gravidade da tuberculose. Embora o Brasil ofereça diagnóstico e tratamento cem por cento gratuitos e universais pelo SUS, a doença ceifa milhares de vidas todos os anos, atingindo de forma desproporcional populações em extrema vulnerabilidade social.*  
  > *O grande objetivo de construir um modelo preditivo em IA não é meramente acadêmico: queremos antecipar, no exato instante em que a ficha de notificação é preenchida na unidade de saúde, quais indivíduos possuem alto risco de evoluir para o óbito, permitindo que a equipe de saúde adote condutas preventivas intensivas, como visitas domiciliares e acompanhamento supervisionado.*  
  > *Contudo, trabalhar com dados do SINAN impõe um imenso desafio técnico: são dados secundários preenchidos por milhares de profissionais em diferentes municípios, contendo campos administrativos, dados faltantes e eventos ocorridos meses após o diagnóstico. Se esses dados não forem tratados com rigor cirúrgico, o modelo sofrerá de vazamento de dados e se tornará inútil na prática clínica."*

---

### SLIDE 3: Arquitetura Geral do Pipeline de Pré-Processamento
- **Apresentador**: Kevin Luís Alves de Lima
- **Tempo estimado**: 00:45 min
- **Elementos Visuais na Tela**:
  * Diagrama de Blocos em 6 Etapas Consecutivas:
    1. Diagnóstico de Faltantes (Atividade 1)
    2. Limpeza e Dicionário (Atividade 2)
    3. Resgate, Desfecho e 4 Regras Fixas (Atividade 3)
    4. Filtro Pós-Óbito e Partição Estratificada (Atividade 4)
    5. Seleção Univariada no Treino (Atividade 4)
    6. Undersampling 1:1 e Análise Gráfica (Atividade 4)
  * Destaque na fronteira de transição de fala entre a Etapa 3 e a Etapa 4.
- **Dicas de Apontamento**: Percorra com o cursor a linha do tempo do fluxo, mostrando como o projeto evoluiu de 884 mil registros brutos até a base de treino balanceada.
- **Pontos-Chave da Fala**:
  * Visão panorâmica das seis etapas exigidas nas atividades da disciplina.
  * Progressão cumulativa e coerente de todas as entregas do grupo.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Neste slide temos o mapa visual de todo o percurso percorrido pelo nosso grupo. Estruturamos o pipeline em seis etapas lógicas consecutivas.*  
  > *Partimos de uma base massiva com 884 mil notificações brutas e 108 colunas. Na Etapa 1, fizemos o diagnóstico inicial de completude. Na Etapa 2, depuramos os dados com apoio do dicionário oficial. Na Etapa 3, resgatamos a coorte bruta para isolar o desfecho binário e aplicamos quatro regras fixas de exclusão, reduzindo as variáveis para 26 preditores candidatos.*  
  > *A partir da Etapa 4, o Lázaro assumirá a apresentação para demonstrar o expurgo do pós-óbito, a divisão estratificada 80/20, a seleção univariada que refinou a base para 22 preditores, o balanceamento por Random Undersampling e a validação gráfica da letalidade real. Vamos agora examinar em detalhes as três primeiras etapas sob minha responsabilidade."*

---

### SLIDE 4: Etapa 1 — Diagnóstico Inicial e Contagem de Faltantes (Atividade 1)
- **Apresentador**: Kevin Luís Alves de Lima
- **Tempo estimado**: 00:50 min
- **Elementos Visuais na Tela**:
  * Tabela com o Top 10 de Incompletude da Base Bruta (df_faltantes):
    - 4 variáveis com 100,00% de ausência: `CS_FLXRET`, `FLXRECEBI`, `DT_TRANSRM`, `IDADEminutos`.
    - Outras 7 variáveis com >97% a 99% de ausência: `IDADEhoras` (99,94%), `EXTRAPUL_O` (99,93%), `IDADEdias` (99,85%), `IDADEmeses` (99,78%), `DT_TRANSDM` (98,93%), `DT_TRANSRS` (98,68%), `AGRAVOUTDE` (97,04%).
    - Diagnóstico geral: 11 colunas no topo com >97% e 38 colunas no total com mais de 60% de incompletude.
  * Callout Técnico: Parâmetro `keep_default_na=False` na leitura do Pandas para detectar strings vazias (`""`).
- **Dicas de Apontamento**: Aponte para a tabela e destaque a diferença entre `NaN` tradicional e strings vazias no Pandas.
- **Pontos-Chave da Fala**:
  * O que foi feito: leitura da base completa e contagem de faltantes (absoluta e percentual) das 108 colunas.
  * Por que foi feito: no SINAN, campos não preenchidos ingressam como espaços vazios; `keep_default_na=False` é obrigatório para não subestimar faltantes.
  * Saídas obtidas: identificação precisa de 4 variáveis 100% vazias, 11 com >97% e 38 com >60% de ausência na base original.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Na primeira atividade da disciplina, o foco foi diagnosticar a completude e a qualidade de preenchimento de cada um dos 108 atributos originais.*  
  > *Ao carregar os 884 mil registros no Pandas, utilizamos o parâmetro essencial keep_default_na=False. Por que essa escolha foi determinante? Porque no SINAN os municípios frequentemente gravam campos não respondidos como texto vazio, e não como o nulo tradicional do sistema. Se tivéssemos feito uma checagem ingênua de isna(), teríamos mascarado milhares de ausências.*  
  > *"Constatamos que exatamente 4 variáveis apresentavam cem por cento de dados faltantes – os fluxos de retorno e recebimento municipal, a data de transferência regional e a idade em minutos. Outras sete variáveis superavam 97% a 99% de ausência, totalizando 11 variáveis com incompletude quase total no topo da tabela, além de 38 variáveis na base original com mais de 60% de incompletude. Esse diagnóstico cadastral deu visibilidade total à qualidade da base e forneceu o substrato empírico para as regras de corte que adotamos a seguir."*

---

### SLIDE 5: Etapa 2 — Dicionário de Dados e Limpeza Exploratória (Atividade 2)
- **Apresentador**: Kevin Luís Alves de Lima
- **Tempo estimado**: 00:50 min
- **Elementos Visuais na Tela**:
  * Tabela de Síntese da Triagem Preliminar:
    - Colunas Iniciais: 108
    - Colunas Descartadas na Triagem: 49 (IDs, chaves administrativas, variáveis de acompanhamento tardio e atributos com >60% nulos)
    - Preditores Permanecentes na Exploração: 58
    - Variável Alvo Isolada: 1 (`SITUA_ENCE`)
  * Callout: Isolamento preventivo do desfecho desde as primeiras células do código.
- **Dicas de Apontamento**: Aponte para o balanço de colunas (108 $\rightarrow$ 49 descartadas $\rightarrow$ 58 preditoras + 1 alvo).
- **Pontos-Chave da Fala**:
  * O que foi feito: inspeção de cada atributo à luz do Dicionário de Dados do SINAN.
  * Por que foi feito: remover metadados de sistema sem relevância clínica e descartar variáveis com excesso de ruído (>60% nulos).
  * Saídas obtidas: base exploratória com 58 preditores e o isolamento inicial de `SITUA_ENCE`.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Na Atividade 2, confrontamos cada uma das 108 colunas com o Dicionário de Dados oficial do SINAN fornecido pelo Ministério da Saúde.*  
  > *Identificamos que dezenas de colunas eram códigos puramente operacionais de controle de digitação ou identificadores municipais sem qualquer potencial discriminativo prognóstico para o paciente individual.*  
  > *Adotamos o limiar de corte de 60% de incompletude para excluir variáveis com carência crônica de preenchimento. Nessa triagem inicial da Atividade 2, descartamos 49 colunas e mantivemos 58 preditores candidatos, além de isolarmos imediatamente a variável SITUA_ENCE como o alvo a ser previsto.*  
  > *Essa exploração inicial foi valiosa, mas notamos que algumas variáveis de alta vulnerabilidade social precisavam ser recuperadas e padronizadas de maneira definitiva, o que nos levou à reformulação rigorosa da Atividade 3."*

---

### SLIDE 6: Etapa 3.1 — Resgate das 108 Colunas e Definição da Coorte de Interesse (Atividade 3)
- **Apresentador**: Kevin Luís Alves de Lima
- **Tempo estimado**: 00:55 min
- **Elementos Visuais na Tela**:
  * Diagrama de Funil da População:
    - Base Bruta Original do SINAN: **884.221 registros**
    - Remoção de Outros Desfechos (abandono, transferência, TB-DR, óbito por outras causas): **280.808 casos (31,76%)**
    - Coorte de Desfecho Selecionada: **603.413 registros (68,24%)**
  * Painel com a Distribuição Inicial do Alvo:
    - Cura (`TARGET = 0`): **568.780 casos (94,26%)**
    - Óbito por TB (`TARGET = 1`): **34.633 casos (5,74%)**
- **Dicas de Apontamento**: Aponte para o funil mostrando a saída de 280 mil casos que não eram nem cura nem óbito por TB.
- **Pontos-Chave da Fala**:
  * O que foi feito: resgate das 108 colunas do backup e filtragem estrita da coorte de desfecho clínico.
  * Por que foi feito: o modelo visa classificar especificamente Cura versus Óbito por TB; desfechos concorrentes distorceriam o aprendizado supervisionado.
  * Saídas obtidas: coorte consolidada de 603.413 registros com proporção inicial de 94,26% de cura e 5,74% de óbito por TB.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Ao ingressarmos na Atividade 3, tomamos uma decisão de grande maturidade metodológica: resgatamos integralmente as 108 colunas originais a partir do backup bruto. Isso garantiu que nenhuma variável crítica — como população de rua ou contatos examinados — ficasse de fora da análise definitiva.*  
  > *A partir da base completa, executamos a única filtragem de linhas respaldada por regra de negócio médica: delimitamos nossa coorte de estudo aos casos que atingiram os dois desfechos de interesse clínico: Cura ou Óbito por Tuberculose.*  
  > *Casos com encerramentos por abandono de tratamento, transferências para outros estados ou óbitos por outras causas como homicídios e acidentes foram excluídos, totalizando 280.808 registros removidos.*  
  > *Consolidamos assim uma coorte analítica de 603.413 pacientes, criando a variável TARGET binária: 0 para Cura, que abrangeu 568.780 casos, ou 94,26%, e 1 para Óbito por Tuberculose, com 34.633 casos, representando 5,74%. A variável original de encerramento foi então descartada para evitar qualquer vazamento."*

---

### SLIDE 7: Etapa 3.2 — Engenharia de Variáveis, Valores Impossíveis e Semântica de Ausência (Atividade 3)
- **Apresentador**: Kevin Luís Alves de Lima
- **Tempo estimado**: 01:00 min
- **Elementos Visuais na Tela**:
  * 3 Painéis Estruturados:
    1. *Decodificação da Idade*: `NU_IDADE_N` no padrão de 4 dígitos do SINAN (1=horas, 2=dias, 3=meses $\rightarrow$ 0 anos; 4=anos $\rightarrow$ valor real). Preservação de centenários legítimos (ex.: 4108 $\rightarrow$ 108 anos), corrigindo o truncamento da coluna bruta `IDADE`.
    2. *Temporalidade e Geografia*: `ANO_DIAG` extraído dos 4 primeiros dígitos de `DT_DIAG`; mais de 5.200 municípios agrupados em `SG_UF` (27 estados) e `REGIAO` (5 macrorregiões).
    3. *Valores Impossíveis e Semântica de Faltantes*: Contatos examinados (`NU_COMU_EX`) com sentinelas 99, 999 ou negativos mapeados para nulo. Análise aprofundada de `CS_GESTANT` (>74% nulo por envolver homens) e `BACILOSC_E` (incapacidade de expectorar unificada em `'Faltante'`).
- **Dicas de Apontamento**: Destaque a fórmula de decodificação da idade e a explicação semântica do "Não se aplica".
- **Pontos-Chave da Fala**:
  * O que foi feito: engenharia de variáveis pré-admissão diagnóstica, correção de truncamento e tratamento semântico.
  * Por que foi feito: sanar inconsistências estruturais do banco de dados e tratar categorias especiais com rigor clínico.
  * Saídas obtidas: novas colunas limpas (`IDADE_ANOS`, `ANO_DIAG`, `REGIAO`), eliminação de alta cardinalidade e tratamento semântico do "Não se aplica".
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Em seguida, implementamos a engenharia de atributos. Um problema clássico do SINAN é a coluna bruta IDADE: ela truncava centenários em dois dígitos e misturava recém-nascidos. Desenvolvemos uma função de decodificação baseada no padrão oficial da coluna NU_IDADE_N, onde o primeiro dígito indica a unidade de medida. Convertemos horas, dias e meses em zero anos e recuperamos centenários de até 108 anos com total precisão.*  
  > *Extraímos o ano da notificação a partir da data de diagnóstico e agrupamos os mais de 5.200 municípios brasileiros em Unidades da Federação e Macrorregiões, evitando a explosão de cardinalidade nos algoritmos.*  
  > *Na variável numérica de contatos examinados, expurgamos sentinelas como 99, 999 e números negativos, convertendo-os para nulos estatísticos.*  
  > *E atendemos pontualmente à exigência da disciplina quanto à semântica de valores ausentes: analisamos a variável de gestação, demonstrando que mais de 69% dos casos eram homens, o que gerava um 'Não se aplica' com sentido biológico real, mas cuja soma com faltantes femininos atingia quase 75% de incompletude, justificando seu descarte; já na baciloscopia de diagnóstico, pacientes impossibilitados de expectorar foram unificados sob a categoria 'Faltante', preservando a variável no estudo."*

---

### SLIDE 8: Etapa 3.3 — Descarte Sistemático por 4 Regras Fixas de Negócio (Atividade 3)
- **Apresentador**: Kevin Luís Alves de Lima
- **Tempo estimado**: 00:55 min
- **Elementos Visuais na Tela**:
  * Infográfico dos 4 Pilares de Exclusão de Colunas:
    1. *Constantes (>99%)*: 5 colunas excluídas (`TP_NOT`, `ID_AGRAVO`, `FORMA`, `ID_PAIS`, `IDADEminutos`).
    2. *Faltantes (>60%)*: 52 colunas excluídas (esquemas de rifampicina, isoniazida e pirazinamida com ~64% nulos; teste molecular com 62%; cultura com 65%).
    3. *Redundantes*: 14 colunas excluídas (idades brutas, datas completas, municípios).
    4. *Vazamento Temporal*: 13 colunas excluídas (`DT_ENCERRA`, baciloscopias mensais do 1º ao 6º mês, transferências).
  * Balanço Numérico Final: **108 colunas originais** $\rightarrow$ **84 descartadas** $\rightarrow$ **24 originais mantidas + 2 derivadas** = **26 preditores candidatos** (+ 1 alvo).
- **Dicas de Apontamento**: Aponte para o número 26 e enfatize o cumprimento da meta de 20 a 30 atributos.
- **Pontos-Chave da Fala**:
  * O que foi feito: aplicação das 4 regras fixas de exclusão e catalogação integral das 108 colunas em CSV.
  * Por que foi feito: filtrar ruídos e vazamentos em nível populacional antes de qualquer divisão amostral.
  * Saídas obtidas: 26 preditores candidatos documentados em `tabela_decisao_todas_colunas.csv`.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Para reduzir o espaço amostral sem incorrer em sobreajuste nem utilizar métricas contra o alvo nesta fase, aplicamos quatro regras fixas de negócio.*  
  > *Primeiro: eliminamos variáveis com mais de 99% de homogeneidade, como tipo de notificação e forma clínica, já que 100% da base era de casos pulmonares.*  
  > *Segundo: descartamos atributos com mais de 60% de incompletude, incluindo esquemas de medicamentos específicos que possuíam mais de 64% de dados ausentes.*  
  > *Terceiro: removemos colunas redundantes substituídas pelas variáveis derivadas.*  
  > *E quarto: eliminamos qualquer coluna com vazamento temporal, como baciloscopias de evolução mensal realizadas após o início do tratamento.*  
  > *O balanço final da Atividade 3 foi de exatamente 84 colunas descartadas e 24 colunas originais mantidas. Somadas às nossas 2 derivadas de idade em anos e ano de diagnóstico, consolidamos 26 variáveis preditoras candidatas e uma variável de desfecho, atingindo com perfeição a meta de 20 a 30 preditores. Todo esse processo foi catalogado linha a linha na tabela de decisão em CSV que entregamos ao projeto.*  
  > *Passo agora a palavra ao Lázaro, que apresentará a modelagem, o particionamento, a seleção univariada e o balanceamento de classes."*

---

### SLIDE 9: Transição de Apresentação (Passagem de Fala)
- **Apresentador**: Transição de Kevin Luís Alves de Lima para Lázaro Felipe da Rocha Santos
- **Tempo estimado**: 00:30 min
- **Elementos Visuais na Tela**:
  * Painel Bipartido de Transição:
    - *Lado Esquerdo (Kevin)*: Síntese da Parte I — Diagnóstico, Coorte de 603.413 casos, Desfecho Binário e 26 Preditores Candidatos.
    - *Lado Direito (Lázaro)*: Agenda da Parte II — Filtro Pós-Óbito, Discretização Legal da Idade, Partição Estratificada 80/20, Feature Selection no Treino, Random Undersampling 1:1 e Auditoria.
- **Dicas de Apontamento**: Kevin agradece e passa formalmente a fala; Lázaro assume a câmera, agradece e conecta a narrativa com fluidez.
- **Pontos-Chave da Fala**:
  * Resumo das conquistas da Parte I.
  * Introdução da Parte II com foco em modelagem preditiva estrita e blindagem contra vazamento.
- **Texto Integral de Fala (Script da Transição — Lázaro)**:
  > *"Muito obrigado, Kevin. Saudações a todos, meu nome é Lázaro Felipe da Rocha Santos e darei prosseguimento à apresentação.*  
  > *Até este ponto, temos uma base com 603 mil pacientes e 26 atributos preditores candidatos triados por regras de negócio.*  
  > *A partir de agora, entramos na modelagem preditiva estrita da Atividade 4. Minha responsabilidade é demonstrar como blindamos nosso modelo contra vazamento de desfecho, como fundamentamos a idade na legislação brasileira, como particionamos a base preservando o isolamento do teste, como selecionamos os 22 preditores finais e como balanceamos as classes por subamostragem aleatória.*  
  > *Vamos iniciar examinando uma das descobertas mais impactantes do nosso projeto: a eliminação do vazamento pós-óbito."*

---

# PARTE II — MODELAGEM, PARTIÇÃO, SELEÇÃO E BALANCEAMENTO
### Apresentador: Lázaro Felipe da Rocha Santos (Slides 10 a 18)

---

### SLIDE 10: Etapa 4.1 — Eliminação de Vazamento do Alvo: O Filtro Pós-Óbito (Atividade 4 / R2)
- **Apresentador**: Lázaro Felipe da Rocha Santos
- **Tempo estimado**: 00:55 min
- **Elementos Visuais na Tela**:
  * Painel de Contingência e Alerta Forense:
    - Total de registros com `TRATAMENTO == 'Pós-óbito'`: **4.095 casos**
    - Óbitos por Tuberculose nesse grupo: **4.089 casos (99,85%)**
    - Curas registradas: apenas 6 casos (0,15% — erro de digitação municipal)
  * Alerta de Causalidade Reversa: Notificações compulsórias geradas tardiamente a partir de Declaração de Óbito (DO) ou necropsia do Serviço de Verificação de Óbito (SVO).
  * Coorte Válida Final Consolidada: **599.318 registros** (568.774 curas e 30.544 óbitos por TB $\rightarrow$ **taxa de letalidade real de 5,10%**).
- **Dicas de Apontamento**: Aponte para a proporção de 99,85% e explique por que manter esses dados destruiria o valor prático de qualquer algoritmo preditivo.
- **Pontos-Chave da Fala**:
  * O que foi feito: exclusão dos 4.095 registros de "Pós-óbito" e unificação do sentinela "Não sabe" em `'Faltante'`.
  * Por que foi feito: evitar causalidade reversa (*target leakage* gravíssimo) originada de declarações de óbito.
  * Saídas obtidas: coorte válida definitiva de 599.318 casos com letalidade real de 5,10%.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Ao realizarmos uma auditoria forense aprofundada na variável TRATAMENTO, que registra a condição de ingresso do paciente no SUS, fizemos uma descoberta alarmante: encontramos 4.095 pacientes classificados com o rótulo 'Pós-óbito'.*  
  > *Ao cruzarmos esse campo com a nossa variável de desfecho, o resultado foi estarrecedor: 4.089 desses pacientes haviam falecido de tuberculose, o que equivale a 99,85% de óbitos na entrada!*  
  > *Na rotina da vigilância epidemiológica, essa notificação ocorre quando o indivíduo falece em domicílio ou via pública sem assistência e o caso só é notificado após o laudo de necropsia do SVO ou da Declaração de Óbito.*  
  > *Se mantivéssemos esses registros no treinamento de um modelo preditivo, estaríamos incorrendo no pior erro em ciência de dados: o vazamento de alvo por causalidade reversa. O modelo não aprenderia a identificar a gravidade clínica do paciente, mas apenas a memorizar que ter a etiqueta 'pós-óbito' significa morte certa, gerando uma acurácia fictícia em laboratório e inútil na prática.*  
  > *Removemos sumariamente esses 4.095 casos e mapeamos o sentinela 'Não sabe' para faltante, consolidando a coorte definitiva em 599.318 pacientes válidos, com uma taxa de letalidade clínica real de 5,10%."*

---

### SLIDE 11: Etapa 4.2 — Discretização da Idade e Fundamentação Jurídico-Estatística (Atividade 4 / R2)
- **Apresentador**: Lázaro Felipe da Rocha Santos
- **Tempo estimado**: 00:55 min
- **Elementos Visuais na Tela**:
  * 3 Cards dos Marcos Legais Oficiais:
    1. *Jovens (0 a 19 anos)*: Estatuto da Criança e do Adolescente (Lei nº 8.069/1990) + Estatuto da Juventude (Lei nº 12.852/2013) + OMS/IBGE.
    2. *Adultos (20 a 59 anos)*: População em Idade Ativa (PIA / IBGE / Ministério da Saúde).
    3. *Idosos (60 anos ou mais)*: Estatuto da Pessoa Idosa (Lei nº 10.741/2003 e Lei nº 14.423/2022) + OMS.
  * Painel Teórico de Dados Faltantes:
    - Categoria `'Faltante'`: 144 casos no treino (0,030% da partição).
    - Amparo na Teoria de **Little & Rubin (2019)**: Ausência Informativa (MAR/MNAR). Rejeição da imputação por média/mediana para não atenuar a variância; aplicação do *missing-category approach*.
- **Dicas de Apontamento**: Aponte para as leis federais citadas e para o box com a fundamentação de Little & Rubin.
- **Pontos-Chave da Fala**:
  * O que foi feito: discretização da idade contínua em Jovens (0-19), Adultos (20-59), Idosos (60+) e Faltante.
  * Por que foi feito: capturar o efeito não linear da idade com respaldo em leis brasileiras e normas da OMS/IBGE.
  * Saídas obtidas: atributo categórico `FAIXA_ETARIA` substituindo `IDADE_ANOS` sem vazamento e preservando a ausência informativa.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"A relação entre a idade biológica e o desfecho da tuberculose é pronunciadamente não linear. Para capturar essa dinâmica sem assumir premissas paramétricas rígidas, discretizamos a idade em faixas etárias amparadas rigorosamente na legislação brasileira e em normas internacionais.*  
  > *A faixa de 'Jovens', de 0 a 19 anos, fundamenta-se no Estatuto da Criança e do Adolescente — a Lei 8.069 de 1990 —, no Estatuto da Juventude e nas convenções da OMS e do IBGE.*  
  > *A faixa de 'Adultos', de 20 a 59 anos, segue a definição da População em Idade Ativa utilizada pelos ministérios do Trabalho e da Saúde.*  
  > *E a faixa de 'Idosos', com 60 anos ou mais, é balizada pelo Estatuto da Pessoa Idosa — Lei federal 10.741 de 2003.*  
  > *Quanto aos 144 registros em que a idade não foi preenchida — apenas 0,03% do treino —, rejeitamos frontalmente a imputação determinística pela média ou mediana. De acordo com a teoria clássica de Little e Rubin, no SUS a omissão cadastral constitui ausência informativa: está correlacionada a populações de extrema vulnerabilidade social, como pessoas em situação de rua sem documentação ou pacientes admitidos desacordados. Imputar uma mediana mascararia essa realidade e atenuaria artificialmente a variância dos dados. Mantivemos a categoria 'Faltante', perfeitamente interpretável por modelos baseados em árvores de decisão."*

---

### SLIDE 12: Etapa 4.3 — Divisão Estratificada 80/20 e Isolamento Estrito do Teste (Atividade 4 / R1)
- **Apresentador**: Lázaro Felipe da Rocha Santos
- **Tempo estimado**: 00:50 min
- **Elementos Visuais na Tela**:
  * Tabela da Divisão Amostral:
    - *Treino (80%)*: **479.454 registros** | 455.019 curas (94,90%) | 24.435 óbitos (5,10%)
    - *Teste (20%)*: **119.864 registros** | 113.755 curas (94,90%) | 6.109 óbitos (5,10%)
  * Destaque do Protocolo de Isolamento (*Strict Hold-out*):
    - Mediana da única contínua (`NU_COMU_EX`) calculada estritamente no treino (`mediana = 1,0`) e aplicada de forma cega ao teste.
    - Zero contaminação cruzada: o teste permaneceu intacto e inacessível durante o feature selection e balanceamento.
- **Dicas de Apontamento**: Aponte para as proporções idênticas de 5,10% e para o ícone de cadeado do isolamento do teste.
- **Pontos-Chave da Fala**:
  * O que foi feito: amostragem aleatória estratificada 80/20 com semente 42 (`stratify=y`).
  * Por que foi feito: manter a taxa real de evento raro em ambas as partições e blindar o teste contra vazamento metodológico.
  * Saídas obtidas: treino com 479.454 casos e teste com 119.864 casos, com isolamento absoluto garantido.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Com a base devidamente depurada, realizamos a partição entre treino e teste. Dividimos os 599 mil pacientes em 80% para treino e 20% para teste por meio de amostragem aleatória estratificada pelo desfecho, utilizando a semente reproduzível 42.*  
  > *A estratificação garantiu que tanto o treino — com 479.454 registros — quanto o teste — com quase 120 mil — mantivessem exatamente a proporção natural da doença: 94,90% de cura e 5,10% de óbito por TB.*  
  > *Optamos pela divisão estratificada em detrimento da partição temporal porque os anos de 2020 a 2022 sofreram graves distorções de subnotificação devido à pandemia de COVID-19, o que geraria um teste temporal enviesado.*  
  > *Adotamos o protocolo de isolamento estrito: o conjunto de teste jamais foi consultado para qualquer decisão. Para a única variável contínua restante — contatos examinados —, calculamos a mediana unicamente no treino, que resultou em 1,0 contato, e a aplicamos de forma cega no teste. O teste foi preservado como um cofre inviolável para a avaliação final."*

---

### SLIDE 13: Etapa 5 — Seleção Univariada de Atributos no Treino (Atividade 4 / R3)
- **Apresentador**: Lázaro Felipe da Rocha Santos
- **Tempo estimado**: 00:55 min
- **Elementos Visuais na Tela**:
  * Gráfico de Barras / Lista Ranqueada dos Preditores ($N = 479.454$):
    - Top 5 Preditores: `FAIXA_ETARIA` ($V = 0,1339$), `TRATAMENTO` ($V = 0,0887$), `AGRAVAIDS` ($V = 0,0841$), `HIV` ($V = 0,0768$), `CS_ESCOL_N` ($V = 0,0759$).
    - Preditores Clínicos e Sociais Intermediários: `ANO_DIAG` (0,0640), `RAIOX_TORA` (0,0608), `AGRAVDIABE` (0,0561), `AGRAVALCOO` (0,0519), `NU_COMU_EX` (ponto-bisserial $|r| = 0,0367$), `TPUNINOT` (0,0335), `CRITERIO_LABORATORIAL` (0,0326).
    - 4 Atributos Descartados ($V < 0,0325$): `BACILOSC_E` (0,0264), `POP_SAUDE` (0,0098), `POP_IMIG` (0,0090), `CS_RACA` (0,0077).
  * Callout Teórico: Por que o $p$-valor puro falha em big data populacional ($N \approx 480.000 \implies p < 10^{-34}$ para todas as variáveis).
- **Dicas de Apontamento**: Aponte para o Top 22 mantido e para as 4 colunas descartadas na base do ranking.
- **Pontos-Chave da Fala**:
  * O que foi feito: cálculo do $V$ de Cramér e correlação ponto-bisserial restrito ao treino; seleção do Top 22 ($V \ge 0,0325$).
  * Por que foi feito: tamanho amostral massivo infla o $p$-valor assintótico para perto de zero; é mandatório ranquear pela magnitude de efeito.
  * Saídas obtidas: 22 atributos retidos (meta atendida no intervalo [20, 30]) e 4 descartados com justificativa clínica.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Na Etapa 5, executamos a seleção univariada de atributos calculada estritamente sobre as 479 mil instâncias do treino.*  
  > *Aqui é imperativo fazer uma observação metodológica: em bases populacionais massivas, o poder estatístico assintótico é praticamente cem por cento. Todas as 26 variáveis apresentaram p-valores infinitesimais, inferiores a dez elevado a menos 34. Portanto, o p-valor é inútil para ranquear quais atributos são clinicamente mais informativos.*  
  > *Empregamos o V de Cramér para as 25 variáveis categóricas e a correlação ponto-bisserial para contatos examinados.*  
  > *Retivemos os 22 melhores preditores com magnitude de associação igual ou superior a 0,0325, situando-se perfeitamente na meta de 20 a 30 variáveis.*  
  > *Os maiores destaques foram a unidade de notificação — que reflete se o paciente foi admitido em hospital ou posto básico —, a faixa etária e comorbidades como alcoolismo e HIV. E descartamos fundamentadamente quatro atributos: baciloscopia inicial por redundância diagnóstica, profissionais de saúde e imigrantes por incidência irrisória e raça/cor por apresentar o menor poder discriminante independente na série temporal."*

---

### SLIDE 14: Etapa 6.1 — Balanceamento do Treino: Random Undersampling vs NearMiss (Atividade 4 / R4)
- **Apresentador**: Lázaro Felipe da Rocha Santos
- **Tempo estimado**: 00:50 min
- **Elementos Visuais na Tela**:
  * Quadro Comparativo Técnico:
    - *NearMiss*: Complexidade $O(N^2)$, exigindo $> 1,1 \times 10^{10}$ cálculos de distância euclidiana via $k$-NN. Inviável em memória RAM (Out-Of-Memory). Seleciona ruídos de fronteira e distorce o perfil epidemiológico do SUS.
    - *Random Undersampling (RUS)*: Complexidade linear $O(N)$, execução instantânea. Preserva a representatividade ecológica da cura em todo o território nacional. Volume final robusto de 48.870 instâncias.
  * Regra de Ouro: Balanceamento aplicado unicamente no Treino; Teste preservado intacto.
- **Dicas de Apontamento**: Aponte para a comparação de complexidade matemática ($O(N^2)$ vs $O(N)$) e para a blindagem do teste.
- **Pontos-Chave da Fala**:
  * O que foi feito: aplicação de Random Undersampling 1:1 sobre a classe majoritária no treino.
  * Por que foi feito: NearMiss é matematicamente inviável (11 bilhões de distâncias) e distorce casos de fronteira; RUS preserva a diversidade territorial.
  * Saídas obtidas: treino balanceado com 48.870 casos (50%/50%) e teste preservado em 5,10%.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Ao chegarmos ao balanceamento de classes na Etapa 6, deparamo-nos com uma escolha fundamental: NearMiss versus Random Undersampling.*  
  > *Rejeitamos categoricamente o NearMiss por duas razões científicas. Primeiro, por inviabilidade matemática: em uma base com 455 mil curas e 24 mil óbitos, o cálculo de vizinhos mais próximos exige mais de 11 bilhões de operações euclidianas em matrizes de alta dimensão, gerando falhas de Out-Of-Memory e tempos computacionais absurdos. Segundo, por distorção epidemiológica: o NearMiss seleciona instâncias na fronteira de decisão, o que no SUS significa pinçar registros com inconsistências de digitação ou casos atípicos, induzindo sobreajuste severo.*  
  > *Optamos pelo Random Undersampling nativo com semente fixa 42. Ele possui complexidade linear, executa instantaneamente e preserva a representatividade geográfica dos pacientes curados em todo o território nacional, gerando um conjunto de treino robusto com 48.870 observações perfeitamente balanceadas em 50% de cura e 50% de óbito.*  
  > *E o mais importante: o teste permaneceu intacto com seus 5,10% originais, pois avaliar acurácia e calibração de risco em um teste balanceado artificialmente destruiria qualquer validade clínica."*

---

### SLIDE 15: Etapa 6.2 — Tabela Comparativa de Classes nas Partições (Atividade 4 / R4)
- **Apresentador**: Lázaro Felipe da Rocha Santos
- **Tempo estimado**: 00:45 min
- **Elementos Visuais na Tela**:
  * Tabela Oficial de Proporções (reproduzindo `tabela_proporcoes_balanceamento.csv`):

| Partição Amostral | Casos de Cura ($N$) | Proporção Cura (%) | Óbitos por TB ($N$) | Proporção Óbito (%) | Volume Total ($N$) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Treino (Original)** | 455.019 | 94,90% | 24.435 | 5,10% | 479.454 |
| **Treino (Pós-Balanceamento)** | 24.435 | 50,00% | 24.435 | 50,00% | 48.870 |
| **Teste (Rigorosamente Isolado)** | 113.755 | 94,90% | 6.109 | 5,10% | 119.864 |

- **Dicas de Apontamento**: Acompanhe linha por linha com o cursor, mostrando os 479 mil no treino original, os 48.870 perfeitamente divididos em 50% e o teste com 119 mil casos.
- **Pontos-Chave da Fala**:
  * Apresentação dos números exatos da tabela entregável do projeto.
  * Confirmação da paridade matemática e da preservação integral do teste.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Esta tabela sintetiza com exatidão matemática o resultado do balanceamento, exportado para o arquivo tabela_proporcoes_balanceamento.csv.*  
  > *Na primeira linha, temos o treino antes do balanceamento, com seus 479.454 registros refletindo a realidade epidemiológica de 5,10% de óbitos e 94,90% de curas.*  
  > *Na segunda linha, observamos o treino pós-balanceamento: exatamente 24.435 casos de cura sorteados aleatoriamente e todos os 24.435 óbitos preservados, totalizando 48.870 instâncias distribuídas simetricamente em cinquenta por cento para cada classe.*  
  > *E na terceira linha, confirmamos a integridade do teste isolado: 119.864 pacientes avaliados sob a prevalência real de 5,10%. Essa estrutura garante ao algoritmo capacidade de aprendizado equilibrada, enquanto o teste mantém fidelidade absoluta ao mundo real do SUS."*

---

### SLIDE 16: Análise Gráfica do Desfecho Pré-Balanceamento (Atividade 4 / R5)
- **Apresentador**: Lázaro Felipe da Rocha Santos
- **Tempo estimado**: 00:55 min
- **Elementos Visuais na Tela**:
  * Imagem em Alta Resolução incorporada: `grafico_desfecho_faixa_etaria.png`
  * Destaques Numéricos das Barras:
    - Jovens (0 a 19 anos): **1,28%** (506 óbitos em 39.470 casos)
    - Adultos (20 a 59 anos): **4,07%** (14.834 óbitos em 364.792 casos)
    - Idosos (60 anos ou mais): **12,11%** (9.089 óbitos em 75.048 casos) — **9,46 vezes superior a jovens!**
    - Faltante: **4,17%** (6 óbitos em 144 casos)
  * Linha Vermelha Tracejada: Letalidade média da coorte de treino (**5,10%**).
- **Dicas de Apontamento**: Aponte para as barras mostrando a escadinha ascendente de risco e para a linha horizontal de 5,10%.
- **Pontos-Chave da Fala**:
  * O que foi feito: geração do gráfico de barras da taxa de óbito por categoria de `FAIXA_ETARIA` no treino pré-balanceamento.
  * Por que foi feito no pré-balanceamento: se gerado após o balanceamento, a média seria distorcida artificialmente para 50%.
  * Saídas obtidas: comprovação empírica do gradiente biológico de letalidade (idosos com risco quase 10x maior que jovens).
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Para a análise gráfica exigida no Requisito 5, selecionamos o preditor clinicamente mais expressivo do nosso modelo: a FAIXA_ETARIA, que obteve a maior associação univariada geral do estudo (Top 1 geral) com V de Cramér de 0,1339 e qui-quadrado de 8.594,40.*  
  > *O gráfico foi gerado estritamente sobre o conjunto de treino pré-balanceamento com 479 mil pacientes. Por que pré-balanceamento? Porque se o gerássemos sobre o treino balanceado, a letalidade média seria artificialmente de 50%, destruindo o perfil epidemiológico real da tuberculose.*  
  > *A visualização revela um gradiente biológico monotônico impressionante: entre jovens de até 19 anos, a taxa de letalidade é de apenas 1,28%; entre adultos de 20 a 59 anos, a taxa triplica para 4,07%; e entre os idosos com 60 anos ou mais, a letalidade atinge alarmantes 12,11%.*  
  > *Um paciente idoso possui uma razão de risco de quase dez vezes superior à de um jovem — reflexo direto da imunossenescência pulmonar e da sobreposição de comorbidades crônicas como diabetes e cardiopatias. A categoria 'Faltante' registrou 4,17% de óbito, alinhando-se à média geral de 5,10% indicada pela linha tracejada."*

---

### SLIDE 17: Relatório de Auditoria Analítica das Atividades 1 a 4
- **Apresentador**: Lázaro Felipe da Rocha Santos
- **Tempo estimado**: 00:50 min
- **Elementos Visuais na Tela**:
  * Tabela de Conformidade Formal dos Requisitos:
    - *Atividade 1 (Faltantes)*: 100% Cumprida no código e explicada.
    - *Atividade 2 (Dicionário e Limpeza)*: 100% Cumprida.
    - *Atividade 3 (Regras Fixas e 26 Preditores)*: 100% Cumprida (108 colunas catalogadas em CSV).
    - *Atividade 4 (Pós-óbito, Leis 8.069/10.741, 80/20, Top 22, RUS e Gráfico)*: 100% Cumprida.
  * Box de Declaração de Auditoria e Recomendações:
    - 27 de 27 testes aprovados na suíte E2E.
    - Recomendação técnica: re-executar o notebook no Google Colab antes da submissão para gravar os outputs pré-renderizados no JSON das células 22 a 47.
- **Dicas de Apontamento**: Aponte para a coluna de conformidade verde e mencione com transparência a auditoria dos arquivos do notebook.
- **Pontos-Chave da Fala**:
  * Avaliação minuciosa de cada exigência do projeto.
  * Transparência científica: o código executa com perfeição de ponta a ponta e gerou todos os artefatos; documentação das 6 pendências mapeadas.
- **Texto Integral de Fala (Script Transcrito)**:
  > *"Conduzimos uma auditoria analítica e forense independente sobre todo o código e documentação do projeto. O resultado confirma conformidade integral em todos os requisitos das quatro atividades da disciplina.*  
  > *A contagem de faltantes e justificativa teórica foram cumpridas; a depuração orientada pelo dicionário com preservação do desfecho foi comprovada; as regras fixas de negócio reduziram a base para 26 preditores; e, na Atividade 4, a remoção do pós-óbito, a fundamentação legal da idade, a partição estratificada, a seleção dos 22 preditores e o balanceamento por Random Undersampling foram validados com 27 testes automatizados aprovados com cem por cento de sucesso.*  
  > *Como apontamento de transparência de auditoria, registramos que o script taia.py executa com perfeição de ponta a ponta e gerou todos os arquivos em disco. Recomendamos apenas que a equipe re-execute o comando 'Run All' no notebook TAIA (2).ipynb no Colab para persistir os blocos visuais de output nas células 22 a 47 antes da submissão final aos professores."*

---

### SLIDE 18: Conclusão, Impacto Clínico e Próximos Passos
- **Apresentador**: Lázaro Felipe da Rocha Santos (com encerramento conjunto com Kevin Luís Alves de Lima)
- **Tempo estimado**: 00:45 min
- **Elementos Visuais na Tela**:
  * 4 Cards Conclusivos:
    1. *Base Blindada*: Zero vazamento de dados (expurgo do pós-óbito e hold-out estrito).
    2. *Espaço Otimizado*: 22 preditores clínicos de alto impacto, de fácil coleta e sem redundância.
    3. *Prontidão para Treinamento*: Base de treino balanceada pronta para algoritmos supervisionados (XGBoost, Random Forest, LightGBM, Redes Neurais).
    4. *Validação Ecológica*: Teste intacto refletindo fielmente a realidade epidemiológica do SUS.
  * Agradecimentos e Contatos dos Integrantes.
- **Dicas de Apontamento**: Ambos os integrantes aparecem em câmera para a conclusão final.
- **Pontos-Chave da Fala**:
  * Síntese das conquistas técnicas e clínicas.
  * O potencial do modelo de IA para apoiar o SUS na redução da mortalidade por TB.
  * Agradecimentos finais da dupla à banca examinadora.
- **Texto Integral de Fala (Script Transcrito — Lázaro e Kevin)**:
  > **[Lázaro]**: *"Para concluir: entregamos uma base de modelagem preditiva que transcende os requisitos acadêmicos da disciplina e atinge padrões de excelência em ciência de dados biomédica. Eliminamos o vazamento de alvo que inflaria falsamente a acurácia, respeitamos os marcos legais brasileiros de proteção à criança e ao idoso, selecionamos preditores viáveis na rotina do posto de saúde e preservamos a integridade ecológica do teste.*  
  > *A base de treino balanceada está pronta para o treinamento de modelos robustos, como Gradient Boosting, Random Forest e Redes Neurais.*  
  > **[Kevin]**: *Temos a plena convicção de que este trabalho estabelece uma fundação metodológica sólida para que algoritmos de Inteligência Artificial possam atuar como ferramenta real de apoio à decisão clínica no SUS, auxiliando a salvar vidas no enfrentamento da tuberculose pulmonar.*  
  > **[Juntos]**: *Muito obrigado a todos pela atenção!"*

---

## Matriz de Verificação de Tempo e Cobertura

| Integrante | Slides | Tópicos Principais Cobertos | Tempo Alvo |
| :--- | :---: | :--- | :---: |
| **Kevin Luís Alves de Lima** | 1 a 9 | Introdução, Relevância do SUS, Fluxo Geral, Ativ 1 (Faltantes), Ativ 2 (Dicionário), Ativ 3 (Coorte de 603k, Engenharia de Idade/UF/Ano, 4 Regras Fixas, 26 Preditores) e Transição | **~5:30 min** |
| **Lázaro Felipe da Rocha Santos** | 10 a 18 | Ativ 4 (Filtro Pós-Óbito, Leis 8.069/10.741, Partição 80/20 Estratificada, Feature Selection Top 22, RUS 1:1, Tabela de Classes, Gráfico de Letalidade, Auditoria e Conclusão) | **~6:30 min** |
| **Total Combinado** | **1 a 18** | **Pipeline Completo de Pré-Processamento de Ponta a Ponta** | **~12:00 min** |

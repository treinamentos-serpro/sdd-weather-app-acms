# Especificação de Produto — Weather App

## Overview

Aplicação web em pt-BR para pesquisar uma cidade, consultar seu clima atual e
ver a previsão diária de hoje e dos quatro dias seguintes. A fonte meteorológica
é Open-Meteo; não há autenticação nem persistência de dados do usuário no
servidor. A temperatura é exibida inicialmente em Celsius e pode ser alternada
para Fahrenheit.

Esta especificação define o comportamento observável do produto. Limites
operacionais ainda sujeitos à aprovação estão identificados como propostas e
não são compromissos de serviço.

## Functional Requirements

- **RF-01 — Pesquisar e selecionar cidade:** permitir busca por envio explícito
  de consulta e seleção de um resultado. Exibir nome da cidade e metadados
  geográficos disponíveis para distinguir localidades homônimas.
- **RF-02 — Consultar clima atual:** para a cidade selecionada, apresentar
  temperatura, condição meteorológica, umidade, vento, pressão e precipitação,
  conforme dados fornecidos pela fonte.
- **RF-03 — Consultar previsão diária:** apresentar cinco dias consecutivos,
  começando na data local atual da cidade selecionada. Cada dia inclui
  temperatura mínima e máxima, condição meteorológica e precipitação.
- **RF-04 — Alternar unidade de temperatura:** permitir alternar entre Celsius
  e Fahrenheit para todas as temperaturas atuais e previstas. A troca não deve
  exigir nova consulta meteorológica.
- **RF-05 — Comunicar estados e falhas:** informar carregamento, resultado
  vazio e falhas de busca ou de consulta meteorológica; oferecer nova tentativa
  quando a falha puder ser repetida.

## User Stories

- **US-01 — RF-01:** Como viajante, quero pesquisar e selecionar uma cidade
  para consultar o clima do local correto.
- **US-02 — RF-01:** Como viajante, quero distinguir cidades homônimas antes
  de selecioná-las.
- **US-03 — RF-02:** Como usuário, quero consultar as condições atuais de uma
  cidade selecionada para orientar minhas atividades.
- **US-04 — RF-03:** Como viajante, quero consultar os próximos cinco dias
  locais para planejar minha semana.
- **US-05 — RF-04:** Como usuário, quero alternar entre Celsius e Fahrenheit
  para ler as temperaturas na unidade que prefiro.
- **US-06 — RF-05:** Como usuário, quero entender quando uma consulta está
  carregando ou falhou e poder tentar novamente.

## Acceptance Criteria

### CA-01 — Busca e seleção (RF-01, US-01, US-02)

- Ao enviar uma consulta com espaços no início ou no fim, o sistema pesquisa o
  texto sem esses espaços.
- Ao enviar uma consulta vazia ou composta somente por espaços, o sistema não
  inicia uma busca e informa que é necessário digitar o nome de uma cidade.
- Enquanto a busca está em andamento, a interface identifica o estado de
  carregamento e impede que uma resposta anterior seja apresentada como
  resultado da consulta atual.
- Com resultados, cada opção apresenta o nome da localidade e os dados
  geográficos disponíveis para diferenciá-la (por exemplo, região e país).
- Sem resultados, a interface informa que nenhuma cidade correspondente foi
  encontrada; esse estado não é apresentado como erro de conexão.
- Ao selecionar um resultado, o nome e a identidade geográfica selecionados
  permanecem associados às consultas meteorológicas subsequentes. Selecionar
  outra cidade substitui a seleção anterior.
- Se a busca falhar por conexão, timeout ou indisponibilidade da fonte, a
  interface apresenta uma mensagem de falha distinta do resultado vazio e
  oferece ação para repetir a mesma busca.

### CA-02 — Clima atual (RF-02, US-03)

- Com uma cidade selecionada e dados válidos, a interface exibe os seis campos
  definidos em RF-02 para essa cidade.
- A temperatura inicial é apresentada em Celsius. A condição meteorológica e
  os rótulos dos campos são apresentados em pt-BR.
- Se um campo estiver ausente ou inválido na resposta, o campo é
  identificado como indisponível e os demais campos válidos continuam visíveis.
- Se os dados atuais não puderem ser obtidos, a interface informa a falha e não
  apresenta dados de outra cidade nem dados anteriores como atuais. A ação de
  nova tentativa consulta a cidade ainda selecionada.

### CA-03 — Previsão diária (RF-03, US-04)

- Com uma cidade selecionada e uma previsão válida, são exibidas exatamente
  cinco datas consecutivas: a data local atual da cidade e as quatro seguintes.
- A data local é determinada para a cidade selecionada, não pelo fuso horário
  do dispositivo do usuário. A sequência permanece correta ao atravessar mês
  ou ano.
- Cada data é associada à sua temperatura mínima, máxima, condição e
  precipitação. Valores ausentes ou inválidos são identificados como
  indisponíveis sem deslocar os dados para outra data.
- Se a fonte não fornecer cinco datas consecutivas, a previsão é marcada como
  indisponível; não se completa a lista com datas duplicadas ou dados de outro
  intervalo.
- Se a previsão falhar, a interface informa a falha e oferece nova tentativa
  para a cidade selecionada. A falha da previsão não altera os dados atuais
  válidos, e vice-versa.

### CA-04 — Unidades (RF-04, US-05)

- Quando não houver preferência de unidade previamente definida, a unidade
  inicial é Celsius.
- Alternar a unidade atualiza imediatamente todas as temperaturas visíveis do
  clima atual e da previsão, sem esperar por nova resposta de rede e sem
  alterar cidade ou datas.
- Alternar repetidamente entre as unidades não acumula erro de arredondamento:
  cada apresentação é calculada a partir do valor meteorológico de origem.
- A alternância afeta temperaturas; não altera a unidade de umidade, vento,
  pressão ou precipitação.
- A unidade selecionada continua ativa enquanto o usuário consulta outras
  cidades na mesma sessão.

### CA-05 — Estados e erros (RF-05, US-06)

- Durante cada consulta, o conteúdo afetado apresenta um estado de
  carregamento perceptível e não comunica sucesso antes de receber dados
  válidos.
- Busca, clima atual e previsão distinguem sucesso, ausência de resultados e
  falha. Uma falha em uma seção não oculta dados válidos de outra seção.
- A ação de repetir uma consulta reenvia a mesma busca ou consulta para a
  cidade selecionada; o usuário não precisa selecioná-la novamente.
- Ao trocar a cidade, dados da cidade anterior deixam de ser apresentados como
  dados da nova seleção enquanto a nova consulta carrega ou após uma falha.

## Non-Functional Requirements

- **RNF-01 — Responsividade (critério proposto):** de 320 px de largura de
  viewport até desktop, não há rolagem horizontal da página; busca, seleção,
  consulta de dados e alternância de unidade permanecem utilizáveis. Validar
  também com ampliação de 200%. Os limites e navegadores suportados precisam de
  aprovação antes de se tornarem compromisso de compatibilidade.
- **RNF-02 — Acessibilidade (meta proposta):** objetivo de conformidade WCAG
  2.2 nível AA. Os fluxos de busca, seleção, consulta e alternância devem ser
  operáveis por teclado, ter foco visível, nomes acessíveis e estados de
  carregamento/erro anunciáveis por tecnologia assistiva. A meta requer
  aprovação e validação manual, além de verificações automatizadas.
- **RNF-03 — Desempenho (meta proposta):** apresentar resultados de busca em
  até 2 segundos no percentil 95, medidos desde o envio até a apresentação dos
  resultados, em perfil de rede acordado. O perfil, o período de medição, a
  meta de carregamento inicial e o tratamento da latência da Open-Meteo devem
  ser definidos antes de usar a meta como critério de aceite.
- **RNF-04 — Disponibilidade e resiliência:** não apresentar erro da fonte
  como “nenhum resultado”; não apresentar dados de outra cidade ou dados
  anteriores como atuais; disponibilizar nova tentativa para falhas repetíveis.
  Uma meta de disponibilidade mensal de 99,9% foi sugerida, mas não aprovada.
  A disponibilidade do site e a da fonte externa devem ser medidas e reportadas
  separadamente, caso essa meta seja aprovada.
- **RNF-05 — Idioma e formatos:** textos, mensagens, nomes de dias e datas da
  interface são apresentados em pt-BR. Temperaturas exibem explicitamente °C ou
  °F, conforme a unidade selecionada.

## Matriz de Rastreabilidade

| User Story | Acceptance Criteria relacionados | Requisitos não funcionais relevantes |
| --- | --- | --- |
| US-01 — Pesquisar e selecionar cidade | CA-01 — envio, validação, carregamento, resultados, seleção e falhas da busca; CA-05 — estados e repetição da consulta | RNF-01 — responsividade dos controles; RNF-02 — operação por teclado e estados acessíveis; RNF-03 — tempo de apresentação dos resultados da busca; RNF-04 — distinção entre falha e ausência de resultados e nova tentativa; RNF-05 — textos em pt-BR |
| US-02 — Distinguir cidades homônimas | CA-01 — metadados geográficos nos resultados e identidade da cidade selecionada | RNF-01 — apresentação responsiva dos resultados; RNF-02 — seleção acessível; RNF-05 — idioma da interface |
| US-03 — Consultar clima atual | CA-02 — campos do clima atual, dados parciais, falha e nova tentativa; CA-05 — carregamento, isolamento de falhas e troca de cidade | RNF-01 — apresentação responsiva; RNF-02 — estados anunciáveis e controles acessíveis; RNF-04 — não exibir dados incorretos e permitir nova tentativa; RNF-05 — idioma e unidade explícita |
| US-04 — Consultar previsão dos próximos cinco dias | CA-03 — cinco datas locais, associação de valores, dados incompletos e nova tentativa; CA-05 — carregamento, isolamento de falhas e troca de cidade | RNF-01 — apresentação responsiva; RNF-02 — estados anunciáveis e conteúdo acessível; RNF-04 — tratar falhas sem apresentar dados incorretos; RNF-05 — idioma e formatos de data |
| US-05 — Alternar Celsius e Fahrenheit | CA-04 — unidade inicial, atualização imediata, precisão, escopo e duração da preferência | RNF-01 — acesso responsivo ao controle; RNF-02 — operação por teclado e foco visível; RNF-05 — exibição explícita de °C ou °F |
| US-06 — Entender estados e tentar novamente | CA-01 — estados e repetição da busca; CA-02 — falha e repetição do clima atual; CA-03 — falha e repetição da previsão; CA-05 — estados de carregamento, sucesso, ausência, falha e troca de cidade | RNF-01 — acesso responsivo aos controles; RNF-02 — estados anunciáveis e operação por teclado; RNF-04 — resiliência e nova tentativa; RNF-05 — mensagens em pt-BR |

Os RNFs marcados como proposta ou meta continuam sujeitos à aprovação indicada
em suas definições; esta matriz registra rastreabilidade, não transforma essas
metas em compromissos ou critérios de aceite adicionais.

## Edge Cases

| Situação | Comportamento esperado |
| --- | --- |
| Consulta vazia ou só com espaços | Não pesquisar; informar que é necessário digitar uma cidade. |
| Nenhuma cidade correspondente | Exibir estado vazio distinto de erro. |
| Cidades homônimas | Exibir metadados geográficos disponíveis em cada opção. |
| Busca ou consulta sem conexão, com timeout ou fonte indisponível | Exibir falha e permitir nova tentativa; não converter falha em vazio. |
| Resposta parcial | Exibir cada campo válido; marcar campo ausente como indisponível. |
| Resposta sem cinco datas consecutivas | Marcar previsão indisponível; não inventar ou duplicar datas. |
| Alternância de unidade durante carregamento ou falha | Aplicar a unidade escolhida aos dados que forem exibidos; não alterar seleção nem ocultar o erro. |
| Mudança de cidade durante consulta | Não associar resposta da cidade anterior à seleção atual. |
| Virada de mês ou ano | Manter cinco datas locais consecutivas, sem lacunas ou duplicatas. |
| Tela estreita ou ampliação | Preservar acesso aos controles e conteúdo sem rolagem horizontal no limite definido em RNF-01. |

## Assumptions

- Open-Meteo é a fonte de geocodificação e dados meteorológicos; não é
  necessária chave de API.
- A consulta meteorológica só ocorre após a seleção de uma cidade.
- “Cinco dias” significa hoje e os quatro dias seguintes, segundo a data local
  da cidade selecionada.
- Celsius e Fahrenheit são as únicas unidades de temperatura nesta versão;
  Celsius é o padrão quando não há preferência de unidade definida.
- A escolha da unidade é mantida durante a sessão, mas sua persistência entre
  sessões não está definida.
- A aplicação é web responsiva; aplicativo nativo, autenticação e
  armazenamento de dados do usuário no servidor não fazem parte do produto.
- As metas marcadas como propostas não são compromissos aprovados.

## Risks

- **Localidades ambíguas ou cobertura geográfica limitada:** mostrar metadados
  disponíveis reduz seleção incorreta; cobertura de países precisa ser
  confirmada.
- **Indisponibilidade ou latência da fonte externa:** tratar falhas separadas
  de resultados vazios e manter a interface recuperável por nova tentativa.
- **Dados parciais ou datas inconsistentes:** sinalizar campos ausentes e não
  apresentar previsão incompleta como cinco dias completos.
- **Metas operacionais não aprovadas:** evitar assumir SLO de disponibilidade,
  desempenho ou compatibilidade até definir responsável, escopo e método de
  medição.

## Out of Scope

- Aplicativo móvel nativo, autenticação, contas, sincronização ou persistência
  de dados do usuário no servidor.
- Geolocalização automática, favoritos ou histórico de cidades.
- Alertas, notificações, mapas, radar, análises históricas ou recomendações.
- Conversão de unidades além de Celsius/Fahrenheit para temperatura.
- Garantia de cobertura para países ou localidades ainda não confirmados.

## Open Questions

1. A busca deve cobrir todos os países suportados pela fonte? Como apresentar
   localidades quando faltarem metadados de desambiguação?
2. Quais limites de tamanho e regras de correspondência da busca são exigidos,
   incluindo acentos, transliteração e nomes em outros idiomas?
3. Qual definição e unidade devem ser exibidas para precipitação atual e
   diária, vento e pressão?
4. Qual precisão e regra de arredondamento devem ser usadas para temperaturas?
5. Com que frequência os dados devem ser atualizados automaticamente, se
   houver atualização automática?
6. A unidade escolhida deve persistir após recarregar ou reabrir a aplicação?
7. Quais navegadores e versões são oficialmente suportados?
8. As metas propostas de responsividade, WCAG 2.2 AA, p95 de 2 segundos e
   disponibilidade mensal de 99,9% são aprovadas? Qual perfil de rede, SLO de
   carregamento inicial, janela de medição e exclusões se aplicam?

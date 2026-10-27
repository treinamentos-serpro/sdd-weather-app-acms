# Especificação de Produto — Weather App

## Overview

O Weather App permite que usuários pesquisem cidades e consultem as condições meteorológicas atuais e a previsão para cinco dias, contando o dia da consulta e os quatro dias seguintes. A interface será em pt-BR, deverá ser utilizável em dispositivos móveis e oferecer temperaturas em Celsius ou Fahrenheit.

A aplicação usará Open-Meteo como fonte de dados, não exigirá autenticação e não armazenará dados do usuário no servidor. Celsius será a unidade inicial.

## Functional Requirements

- **RF-01 — Buscar cidades:** o sistema deve permitir que o usuário pesquise cidades e selecione a localização correta entre os resultados disponíveis.
- **RF-02 — Consultar clima atual:** após a seleção de uma cidade, o sistema deve apresentar as condições meteorológicas atuais correspondentes à localização selecionada.
- **RF-03 — Consultar previsão de cinco dias:** após a seleção de uma cidade, o sistema deve apresentar a previsão para o dia da consulta e os quatro dias seguintes.
- **RF-04 — Alternar unidade de temperatura:** o sistema deve permitir alternar entre Celsius e Fahrenheit e atualizar as temperaturas exibidas.

## User Stories

- **US-01:** Como pessoa que quer consultar o tempo, quero buscar e selecionar uma cidade para ver informações meteorológicas da localização correta.
- **US-02:** Como pessoa que selecionou uma cidade, quero ver as condições atuais para entender o tempo naquele local.
- **US-03:** Como pessoa que está planejando os próximos dias, quero consultar a previsão de cinco dias para antecipar as condições meteorológicas.
- **US-04:** Como pessoa que prefere uma unidade específica, quero alternar entre Celsius e Fahrenheit para interpretar as temperaturas com facilidade.

## Acceptance Criteria

- **CA-01 — RF-01 / US-01:**
  - Quando o usuário enviar uma consulta de cidade e houver resultados, o sistema apresenta os resultados disponíveis para seleção.
  - Quando a consulta não produzir resultados, o sistema informa que nenhuma cidade correspondente foi encontrada.
  - Quando houver resultados com nomes de cidade iguais, o sistema apresenta informações de localização disponíveis que ajudem a distingui-los, como região ou país.
  - Ao selecionar um resultado, o sistema associa as consultas meteorológicas à localização selecionada.

- **CA-02 — RF-02 / US-02:**
  - Após uma seleção de cidade e uma resposta válida da fonte de dados, o sistema apresenta informações meteorológicas atuais associadas à cidade selecionada.
  - A temperatura atual é exibida em Celsius inicialmente.
  - Se os dados atuais não puderem ser obtidos, o sistema informa que não foi possível carregá-los e não apresenta dados desatualizados como atuais.

- **CA-03 — RF-03 / US-03:**
  - Após uma seleção de cidade e uma resposta válida da fonte de dados, o sistema apresenta cinco datas consecutivas: o dia da consulta e os quatro dias seguintes.
  - Cada data é associada à previsão correspondente à cidade selecionada.
  - As temperaturas da previsão são exibidas na unidade selecionada pelo usuário.
  - Se a previsão não puder ser obtida, o sistema informa que não foi possível carregá-la.

- **CA-04 — RF-04 / US-04:**
  - A unidade inicial da temperatura é Celsius.
  - Quando o usuário alterna para Fahrenheit, as temperaturas atuais e da previsão passam a ser exibidas em Fahrenheit.
  - Quando o usuário alterna para Celsius, as temperaturas atuais e da previsão passam a ser exibidas em Celsius.
  - A alternância de unidade não altera a cidade selecionada nem o período da previsão.

## Non-Functional Requirements

- **RNF-01 — Responsividade e uso móvel:** a interface e suas funcionalidades principais devem ser utilizáveis em dispositivos móveis. Como critério inicial sujeito a validação, não deve haver rolagem horizontal em telas com largura de 320 px ou mais.
- **RNF-02 — Usabilidade:** busca, consulta meteorológica e alternância de unidade devem ser compreensíveis e acessíveis. Critérios quantitativos de usabilidade ainda precisam ser definidos.
- **RNF-03 — Disponibilidade e resiliência:** como meta inicial sujeita a validação, o serviço deve ter disponibilidade mensal de pelo menos 99,9%, excluindo manutenção programada. A interface deve comunicar falhas da conexão ou da fonte de dados e não deve apresentar dados desatualizados como atuais.
- **RNF-04 — Desempenho:** como meta inicial sujeita a validação, os resultados da busca devem ser apresentados em até 2 segundos no percentil 95, em condições normais de rede. Também devem ser definidos limites para carregamento inicial e atualização de dados.
- **RNF-05 — Acessibilidade:** como objetivo inicial sujeito a validação, a interface deve atender ao nível AA das WCAG 2.2, incluindo navegação por teclado, contraste adequado e nomes acessíveis para controles.
- **RNF-06 — Idioma:** os textos da interface devem estar em pt-BR.

## Edge Cases

- A consulta de cidade não retorna resultados.
- A busca retorna cidades com nomes iguais ou semelhantes.
- O usuário envia uma consulta vazia ou composta apenas por espaços.
- A conexão falha ou a fonte meteorológica fica indisponível.
- A resposta da fonte de dados está vazia, incompleta ou inválida.
- A solicitação excede o tempo de espera definido para a fonte de dados.
- A fonte de dados não fornece informações atuais ou previsão para alguma data solicitada.
- O usuário alterna a unidade enquanto os dados estão carregando ou depois de um erro.
- A previsão atravessa uma mudança de mês ou ano.
- O usuário acessa a aplicação em uma tela estreita ou amplia o conteúdo.

## Assumptions

- A fonte de dados meteorológicos é Open-Meteo, sem necessidade de API key.
- A consulta meteorológica acontece para uma cidade selecionada pelo usuário.
- “Cinco dias” significa o dia da consulta e os quatro dias seguintes.
- A unidade padrão para temperatura é Celsius.
- A alternância Celsius/Fahrenheit aplica-se à temperatura. A aplicação a outras medidas meteorológicas não foi definida.
- A aplicação não exige autenticação e não armazena dados do usuário no servidor.
- A interface é uma aplicação web responsiva; suporte a aplicativo nativo não foi solicitado.
- As metas numéricas de desempenho, disponibilidade, largura mínima de tela e o objetivo de acessibilidade são propostas iniciais, não compromissos aprovados.

## Risks

- **Ambiguidade na busca:** nomes repetidos ou variações de escrita podem levar à seleção de uma localização incorreta. Mitigação proposta: apresentar informações de localização disponíveis junto aos resultados.
- **Indisponibilidade ou desatualização dos dados:** falhas de conexão ou da fonte podem impedir consultas confiáveis. Mitigação proposta: comunicar a falha e não apresentar dados antigos como atuais.
- **Conteúdo da previsão indefinido:** a falta de decisão sobre os campos exibidos pode gerar uma experiência incompleta ou inconsistente. Mitigação proposta: aprovar os campos antes de considerar a especificação final.
- **Cobertura geográfica incerta:** países e localidades cobertos pela busca ainda não foram definidos. Mitigação proposta: confirmar a cobertura esperada e validá-la contra a fonte de dados.
- **Experiência móvel insuficiente:** dispositivos, navegadores e dimensões-alvo ainda não foram aprovados. Mitigação proposta: validar o critério inicial de responsividade com as partes interessadas.
- **Metas de qualidade não validadas:** objetivos de performance, disponibilidade e acessibilidade podem não refletir as necessidades do produto ou as limitações da fonte. Mitigação proposta: validar as metas antes de tratá-las como requisitos contratuais.
- **Interpretação inconsistente das unidades:** não estão definidos arredondamento, precisão nem o escopo da conversão. Mitigação proposta: definir regras de apresentação e confirmar se a alternância afeta somente temperaturas.

## Out of Scope

- Aplicativo móvel nativo.
- Autenticação, contas de usuário ou sincronização entre dispositivos.
- Persistência de dados do usuário no servidor.
- Alertas meteorológicos ou notificações.
- Mapas, radar meteorológico ou camadas geográficas.
- Recomendações, previsões personalizadas ou análises históricas.
- Garantia de cobertura para países ou localidades ainda não confirmados.
- Conversão de unidades meteorológicas além da temperatura, salvo decisão futura.

## Open Questions

1. A busca deve aceitar cidades de qualquer país? Como o usuário deve escolher entre cidades com o mesmo nome?
2. A busca deve sugerir resultados enquanto o usuário digita ou ocorrer somente após o envio da consulta?
3. Quais informações devem compor o clima atual, além da temperatura e da condição meteorológica?
4. Quais informações devem aparecer para cada dia da previsão? Temperaturas mínima e máxima e condição meteorológica são uma proposta de conteúdo a validar.
5. Com que frequência as informações meteorológicas devem ser atualizadas?
6. Qual deve ser o comportamento e o tempo limite quando a cidade não for encontrada, a conexão falhar ou a fonte de dados estiver indisponível?
7. A unidade escolhida deve ser mantida entre consultas ou sessões?
8. A alternância Celsius/Fahrenheit deve aplicar-se somente à temperatura?
9. Qual precisão e regra de arredondamento devem ser usadas para temperaturas?
10. Quais dispositivos, tamanhos de tela e navegadores móveis devem ser suportados?
11. Há requisitos adicionais de localização, acessibilidade ou conformidade?
12. As metas sugeridas de 2 segundos no percentil 95, 99,9% de disponibilidade mensal e WCAG 2.2 nível AA são adequadas? Quais metas de carregamento inicial e limites de uso devem ser atendidos?
13. O critério sugerido de não haver rolagem horizontal a partir de 320 px é adequado? Quais orientações, navegadores e dispositivos devem ser suportados?

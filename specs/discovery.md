# Discovery — Aplicação de previsão do tempo

## Contexto

A empresa deseja oferecer uma aplicação de previsão do tempo para que usuários
possam consultar condições meteorológicas de cidades. O briefing identifica
cinco capacidades principais: buscar cidades, consultar o clima atual, ver a
previsão de cinco dias, alternar entre Celsius e Fahrenheit e usar a aplicação
em dispositivos móveis.

O briefing ainda não define regras de busca, conteúdo detalhado das previsões,
comportamento em situações de erro ou requisitos específicos de compatibilidade
e desempenho. A classificação abaixo separa as funcionalidades do produto das
características de qualidade; metas sugeridas para requisitos não-funcionais
continuam sujeitas à validação com as partes interessadas.

## Requisitos Funcionais

- **RF-01 — Buscar cidades:** permitir que o usuário pesquise cidades para
  consultar suas informações meteorológicas.
- **RF-02 — Consultar clima atual:** apresentar as condições meteorológicas
  atuais da cidade selecionada.
- **RF-03 — Consultar previsão:** apresentar a previsão do tempo para os cinco
  dias seguintes à data da consulta.
- **RF-04 — Alternar unidade de temperatura:** permitir alternar entre Celsius
  e Fahrenheit para exibir temperaturas.

## Requisitos Não-Funcionais

- **RNF-01 — Responsividade e uso móvel:** a interface e suas funcionalidades
  principais devem ser utilizáveis em dispositivos móveis. Como critério
  inicial a validar, não deve haver rolagem horizontal em telas com largura de
  320 px ou mais. “Usar em dispositivos móveis” é não-funcional porque define
  uma característica de compatibilidade e experiência, não uma ação específica
  do sistema.
- **RNF-02 — Usabilidade:** busca, consulta de condições e alternância de
  unidade devem ser acessíveis e compreensíveis ao usuário. Critérios de
  usabilidade ainda precisam ser definidos.
- **RNF-03 — Disponibilidade e resiliência:** como meta inicial a validar, o
  serviço deve ter disponibilidade mensal de pelo menos 99,9%, excluindo
  manutenção programada. A interface deve informar claramente quando a fonte
  meteorológica ou a conexão estiver indisponível, sem apresentar dados
  desatualizados como atuais.
- **RNF-04 — Desempenho:** como meta inicial a validar, os resultados da busca
  devem ser apresentados em até 2 segundos no percentil 95, em condições
  normais de rede. Também devem ser definidos limites para carregamento inicial
  e atualização dos dados.
- **RNF-05 — Acessibilidade:** como objetivo inicial a validar, a interface
  deve atender ao nível AA das WCAG 2.2, incluindo navegação por teclado,
  contraste adequado e nomes acessíveis para controles.

## Riscos

- **Ambiguidade na busca:** nomes de cidades podem ser repetidos ou escritos
  de formas diferentes, levando à seleção de uma localização incorreta.
- **Dados indisponíveis ou desatualizados:** falhas na fonte meteorológica ou
  na conexão podem impedir a consulta ou apresentar informações antigas.
- **Escopo indefinido da previsão:** não está claro quais dados devem ser
  exibidos nem como interpretar “cinco dias”.
- **Experiência móvel insuficiente:** sem critérios de responsividade e
  dispositivos-alvo, a interface pode ser difícil de usar em certos aparelhos.
- **Metas de qualidade não validadas:** os valores sugeridos de desempenho,
  disponibilidade, dimensões de tela e acessibilidade podem não refletir as
  necessidades do produto ou as limitações da fonte de dados.
- **Expectativa de cobertura geográfica:** não foram definidos países,
  localidades ou idiomas cobertos pela busca.
- **Conversão e apresentação de unidades:** não está definido se a alternância
  afeta apenas temperatura ou também outras medidas meteorológicas.

## Perguntas em Aberto

1. A busca deve aceitar cidades de qualquer país? Como o usuário deve escolher
   entre cidades com o mesmo nome?
2. A busca deve sugerir resultados enquanto o usuário digita ou ocorrer somente
   após o envio da consulta?
3. Quais informações compõem o “clima atual” (por exemplo, temperatura,
   sensação térmica, condição, umidade ou vento)?
4. Quais informações devem aparecer para cada dia da previsão?
5. “Previsão de cinco dias” significa hoje mais os quatro dias seguintes ou
   cinco dias completos a partir de amanhã?
6. Com que frequência as informações meteorológicas devem ser atualizadas?
7. Qual deve ser o comportamento quando a cidade não for encontrada, a conexão
   falhar ou a fonte de dados estiver indisponível?
8. A unidade escolhida deve ser mantida entre consultas ou sessões?
9. A alternância entre Celsius e Fahrenheit se aplica apenas à temperatura?
10. Quais dispositivos, tamanhos de tela e navegadores móveis devem ser
    suportados?
11. Há requisitos de idioma, localização, acessibilidade ou conformidade
    específicos?
12. As metas sugeridas de 2 segundos no percentil 95, 99,9% de disponibilidade
    mensal e conformidade WCAG 2.2 nível AA são adequadas? Quais metas de
    carregamento inicial e limites de uso devem ser atendidos?
13. O critério sugerido de não haver rolagem horizontal a partir de 320 px é
    adequado? Quais orientações, navegadores e dispositivos devem ser
    suportados?

## Suposições

- A consulta de clima atual e da previsão ocorre para uma cidade selecionada
  pelo usuário.
- Celsius e Fahrenheit são opções de exibição da temperatura.
- “Cinco dias” descreve o horizonte da previsão, mas sua interpretação exata
  permanece pendente de confirmação.
- “Usar em dispositivos móveis” implica uma interface responsiva e utilizável
  em telas móveis; suporte a aplicativo nativo não foi solicitado.
- As metas numéricas e o nível de conformidade listados nos requisitos
  não-funcionais são propostas iniciais para discussão, não compromissos
  aprovados.
- Não se presume provedor de dados, cobertura geográfica, persistência de
  preferências ou atualização automática; essas decisões dependem das
  respostas às perguntas em aberto.

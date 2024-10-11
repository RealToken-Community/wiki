---
title: 3. Fase 1, versão simplificada
description: 
published: true
date: 2024-10-11T09:46:52.840Z
tags: 
editor: markdown
dateCreated: 2024-10-11T09:46:52.840Z
---

## **3.1. Explicação da versão simplificada do DAO**

#### **⭐ Para principiantes**

A Fase 1 do RealToken DAO é uma versão simplificada do sistema final. É como um período de teste em que configuramos os fundamentos do DAO. Durante esta fase aprendemos como funciona a governação e começámos a tomar decisões em conjunto, mas de uma forma mais simples do que no futuro. O poder do DAO é limitado neste momento, de forma a limitar os riscos e permitir uma aprendizagem progressiva, o âmbito do poder de decisão aumentará ao longo do tempo.

#### **⭐⭐ Para os iniciados**

A Fase 1 é um passo crucial na implementação do RealToken DAO. Tem como objetivo:

1.º Estabelecer uma estrutura básica de governação,
2.º Testar os mecanismos de votação e de proposta,
3.º Identificar as necessidades e os desafios da comunidade,
4.º Comece a tomar decisões coletivas sobre assuntos simples,
5.º Preparar o terreno para recursos mais avançados em fases futuras,
6.º Estabelecer as bases operacionais do DAO,
7.º Educar a comunidade sobre os mecanismos do DAO,
8.º Experimentar diversas funções e abordagens de governação,
9.º Demonstrar o potencial futuro do DAO e do REG.

Esta fase utiliza um conjunto limitado de contratos inteligentes e recursos para facilitar a adoção e a aprendizagem. A escolha tecnológica foi feita para que esta primeira fase permita uma implementação rápida e simples com custos de desenvolvimento limitados. Esta fase permitirá, assim, acumular conhecimento e estabelecer os princípios básicos fundamentais para o futuro do DAO e da nova versão na fase 2 com a chegada do NFT.

#### **⭐⭐⭐ Para especialistas**

A Fase 1 do RealToken DAO foi concebida como uma implementação minimalista, mas funcional, com as seguintes características:

1. Governação simplificada:
 - Mecanismo básico de votação retirado do padrão governador OpenZeppelin,
 - Processo de proposta escalável e menos rígido para permitir uma maior participação e flexibilidade de exploração,
 - Quórum e limites de votação ajustados regularmente para se adaptarem à participação,
 - Nenhuma delegação de poder de voto,
 - Limitação do acesso à criação de propostas, de forma a limitar o risco de criação de propostas maliciosas.
2. Contratos inteligentes básicos:
 - Contrato de governação, com funções de votação e proposta, baseado nas normas Open Zeppelin,
 - Contrato de tesouraria simples para gestão de fundos DAO,
 - Contrato de incentivo à gestão de recompensas de incentivo ao voto,
 - Contrato PowerVotingRegistry, permitindo registar o poder de voto de cada titular de REG.
3. Integração limitada com o ecossistema:
 - Interações limitadas com as DApps existentes (a RealT continua a ser a terceira de confiança para as DApps pelo tempo necessário para o desenvolvimento de competências de DAO),
 - Mecanismos simples de incentivo para incentivar a participação.
4. Mecanismos de segurança:
 - Timelock nas execuções de propostas, dando tempo adicional para detetar possíveis problemas ou erros numa proposta,
 - Limites em valores de caixa geríveis,
 - Direitos de veto da RealT sobre propostas extremas ou que ponham em risco o DAO.
5. Recolha de dados:
 - Métricas sobre a participação e o engagement dos membros,
 - Feedback contínuo da comunidade para informar fases futuras.

Esta fase servirá de base para a evolução futura do DAO, permitindo identificar os ajustes necessários e as funcionalidades a desenvolver para as fases seguintes. Utilizando esta fase como laboratório, poderemos experimentar várias abordagens de governação e recolha de dados antes de as integrar na próxima versão do DAO com a chegada do NFT. Embora gerido pelo parceiro de confiança RealT, o DAO permanecerá independente e as decisões serão ainda tomadas pelos membros do DAO no âmbito de aplicação gradualmente alargado. Vários pontos serão, por isso, discutidos entre os titulares de REG e RealT, de forma a estabelecer os passos para alargar o âmbito de aplicação do DAO, de forma a garantir a estabilidade e segurança do ecossistema.

## **3.2. Processo de configuração**

#### **⭐ Para principiantes**

A implementação do DAO é feita passo a passo. Em primeiro lugar, criámos as ferramentas básicas para votar e fazer propostas. Depois começaremos a usá-los para tomar decisões simples. Ao longo do caminho, aprenderemos e melhoraremos o sistema em conjunto.

#### **⭐⭐ Para os iniciados**

O processo de implementação da Fase 1 inclui várias etapas principais:

1. Implementação de contratos inteligentes básicos (já realizados),
2. Criação de uma interface de utilizador simples para votação e relações públicas opor (utilizando um site já existente: Tally)
3.º Capacitação da comunidade na utilização de ferramentas de governação (Wiki),
4. Apresentação dos primeiros projetos em que o DAO poderá votar (a RealT já elaborou uma série de propostas),
5.º Estabelecimento do sistema de discussão e debate de propostas (Fórum),
6.º Lançamento das primeiras propostas (outubro),
7. Lançamento da primeira campanha de incentivo ao voto (Outubro-Novembro),
8. Ajuste dos parâmetros de governação com base no feedback,
9.º Alargamento gradual do âmbito de atuação do DAO.

#### **⭐⭐⭐ Para especialistas**

O processo de implementação da Fase 1 está estruturado de forma a maximizar a aprendizagem e a adaptação:

1. Infraestrutura técnica:
 - Implementação de contratos inteligentes (governação, tesouraria, incentivos, PowerVotingRegistry),
 - Testes líquidos em pequeno grupo (já realizados),
 - Auditorias de segurança e testes aprofundados (realizados internamente RealT),
 - Teste de rede pública (já realizado),
 - Desenvolvimento de ferramentas para o cálculo do poder de votação (Produzido pela RealT).
2. Governação inicial:
 - Definição dos parâmetros iniciais (quorum, limite de votação, timelock: RealT)
 - Implementação de um processo de proposta em várias fases (discussão, formalização, votação),
 - Implementação de mecanismos de segurança, incluindo o poder de veto RealT,
 - Limitação do acesso à criação de propostas.
3.º Envolvimento da Comunidade:
 - Campanha educativa sobre mecanismos de DAO,
 - Criação de canais de comunicação dedicados para discussões e feedback,
 - Organização de eventos de governação para estimular a participação,
 - Programa de incentivos.
4. Iteração e otimização:
 - Recolha e análise contínua de métricas de participação,
 - Ajustes regulares dos parâmetros de governação,
 - Experimentação com diferentes abordagens de governação,
 - Debate sobre as diversas mudanças e abordagens à governação.
5. Expansão gradual:
 - Definição de um plano para alargar o âmbito de atuação do DAO,
 - Negociações com a RealT para transferência gradual de responsabilidades,
 - Preparação para a transição para a Fase 2 com integração de NFTs.

Este processo foi concebido para ser flexível e adaptável, permitindo à comunidade influenciar ativamente a evolução do DAO, mantendo ao mesmo tempo a estabilidade e a segurança do ecossistema. Isto também dá aos titulares de REG a oportunidade de compreender e familiarizar-se com os mecanismos do DAO, de criar e descobrir vocações, de trazer ideias e novos casos de utilização. A implementação gradual do DAO permite assim uma transição natural e uma evolução gradual para a descentralização do ecossistema.

## **3.3. Objetivos a curto e médio prazo**

#### **⭐ Para principiantes**

A curto prazo, queremos que todos compreendam como funciona o DAO e comecem a participar ao nível da sua compreensão e capacidade. A médio prazo, queremos tomar decisões mais importantes em conjunto e descentralizar a governação dos serviços oferecidos pelo DAO.

#### **⭐⭐ Para os iniciados**

Objetivos de curto prazo:

1.º Alcançar uma elevada taxa de participação durante a votação,
2.º Educar a comunidade sobre os mecanismos do DAO (segurança, equilíbrio orçamental, etc.),
3.º Identificar os primeiros projetos a financiar ou a desenvolver,
4.º Testar e ajustar os parâmetros de governação,
5.º Estabelecimento de processos básicos de governação.

Objetivos a médio prazo:

1.º Alargar gradualmente o âmbito de atuação do DAO,
2.º Desenvolver novos casos de uso para REG,
3.º Melhorar as ferramentas de governação com base no feedback,
4.º Preparar a transição para a Fase 2 com a integração de NFTs.

#### **⭐⭐⭐ Para especialistas**

Objetivos a curto prazo (3-6 meses):

1. Governação:
 - Atingir um quórum estável de 50% nos votos,
 - Implementar e testar diferentes processos de propostas,
 - Estabelecer um processo eficaz para debater e refinar propostas,
 - Eleger os primeiros membros da comunidade que poderão criar propostas,
 - Definir o enquadramento de uma proposta válida para submissão a votação.
2.º Envolvimento da Comunidade:
 - Lançar campanhas de incentivo ao voto com recompensas,
 - Organizar eventos educativos regulares sobre a governação da DAO,
 - Criar um programa de mentoria para novos membros ativos,
 - Incentivar iniciativas benéficas para o DAO e para o ecossistema RealToken.
3.º Desenvolvimento técnico:
 - Desenvolver/aperfeiçoar ferramentas de análise para monitorizar a saúde do DAO,
 - Integrar funcionalidades essenciais no ecossistema existente.

Objectivos a médio prazo (6-18 meses):

1. Expansão do DAO:
 - Negociar e colocar n está a implementar um plano para alargar o âmbito de atuação com o RealT,
 - Desenvolver mecanismos de governação mais avançados,
 - Criar comités especializados para diferentes aspetos do ecossistema,
 - Implementação mais ampla do acesso à criação de propostas.
2. Inovação e desenvolvimento:
 - Lançar iniciativas de investigação e desenvolvimento orientadas para a comunidade,
 - Experimentar novos modelos de incentivo e participação,
 - Desenvolver integrações mais profundas com o ecossistema,
 - Início da fase 2 com integração de NFTs.
3. Preparação para a Fase 2:
 - Conceber e testar os mecanismos de integração dos NFT na governação,
 - Elaborar um roteiro detalhado para a transição para a Fase 2,
 - Formar grupos de trabalho para cada aspecto importante da nova fase,
 - Realizar testes extensivos com a comunidade para a fase 2.
4. Ecossistema e parcerias:
 - Estabelecer parcerias estratégicas com outros projetos DeFi e DAO,
 - Desenvolver casos de utilização inovadores para REG para além da governação,
 - Integração dos primeiros fornecedores para além do RealT.

Estes objetivos visam estabelecer uma base sólida para o DAO, ao mesmo tempo que preparam o terreno para a expansão e inovação contínuas no ecossistema RealToken.

## **3.4. Como participar na fase 1**

#### **⭐ Para principiantes**

Para participar na fase inicial do RealToken DAO:

1. Acompanhe os anúncios oficiais nos canais de comunicação do RealToken,
2.º Dê a sua opinião nas discussões da comunidade no fórum de governação,
3.º Possuir tokens REG para ter poder de voto,
4.º Participe nas votações quando estiverem abertas,
5.º Participe em eventos educativos para aprender mais sobre CAD,
6.º Descubra e treine sobre o tema DAOs.

#### **⭐⭐ Para os iniciados**

Veja como pode participar ativamente na fase inicial:

1.º Propor ideias de melhoria nos fóruns de governação,
2. Participar nos debates sobre as propostas antes da votação no fórum de governação,
3.º Vote regularmente nas propostas,
4.º Ajude a educar os novos membros sobre o funcionamento do DAO,
5.º Participe em campanhas de incentivo para ganhar recompensas,
6.º Teste novas funcionalidades e dê o seu feedback,
7.º Contribuir para o desenvolvimento de processos de governação,
8.º Tomar iniciativas e apresentá-las à comunidade,
9.º Escreva, melhore, traduza e partilhe conteúdo no DAO.

#### **⭐⭐⭐ Para especialistas**

Para uma participação aprofundada na fase inicial:

1. Governação:
 - Analise as propostas em profundidade e partilhe as suas análises,
 - Contribuir ativamente para o desenvolvimento de métricas para avaliar a saúde do DAO,
 - Propor ajustes nos parâmetros de governação, com base em dados e argumentar com uma visão de curto, médio e longo prazo.
2.º Desenvolvimento técnico:
 - Participar ativamente em Testnets,
 - Propor melhorias técnicas para as ferramentas de governação,
 - Contribuir para o desenvolvimento de ferramentas de análise para DAO,
 - Revisão e auditoria contínua de contratos inteligentes, mantendo-se informado sobre possíveis hacks e falhas.
3.º Envolvimento da Comunidade:
 - Organizar sessões educativas para a comunidade,
 - Criar conteúdo explicativo sobre o funcionamento do DAO,
 - Participar ativamente em programas de mentoria,
 - Explore novos conceitos de Live, vídeo, artigo, etc.
4. Inovação:
 - Propor novos casos de uso argumentados para o REG,
 - Participar em grupos de trabalho sobre a futura integração de NFTs,
 - Explorar potenciais sinergias com outros projetos DeFi.
5. Preparação para a Fase 2:
  - Contribuir para a conceção de mecanismos de integração NFT,
  - Participar em testes aprofundados de novos recursos,
  - Ajudar a desenvolver o guião para a transição para a Fase 2.

A sua participação ativa a todos estes níveis ajudará a moldar o futuro do RealToken DAO e a garantir o seu sucesso a longo prazo.
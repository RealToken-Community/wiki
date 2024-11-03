---
title: 4. Operação técnica do DAO REG
description: 
published: true
date: 2024-11-03T06:58:25.334Z
tags: 
editor: markdown
dateCreated: 2024-10-11T09:49:43.096Z
---

## **4.1. Principais contratos inteligentes**

#### **Endereços de contratos inteligentes:**

- [Governador: 0x4a5327347f077e72d2aab19f68ba8a7f12ec5d63](https://gnosisscan.io/address/0x4a5327347f077e72d2aab19f68ba8a7f12ec5d63#code)
- [REGTreasuryDAO: 0x3f2d192F64020dA31D44289d62DB82adE6ABee6c](https://gnosisscan.io/address/0x3f2d192F64020dA31D44289d62DB82adE6ABee6c#code)
- [PowerVotingRegistry: 0x6382856a731Af535CA6aea8D364FCE67457da438](https://gnosisscan.io/address/0x6382856a731Af535CA6aea8D364FCE67457da438#code)
- [Incentivo: 0xe1877d33471e37fe0f62d20e60c469eff83fb4a0](https://gnosisscan.io/address/0xe1877d33471e37fe0f62d20e60c469eff83fb4a0#code)
#### Exemplos de interações entre contratos inteligentes
![reg-vote-bonus.drawio.svg](/assets/img/reg-vote-bonus.drawio.svg)

#### **⭐ Para principiantes**

O RealToken DAO funciona utilizando programas de computador especiais chamados “smart contracts”. Estes contratos gerem automaticamente as regras, votos e recompensas do DAO. Existem quatro contratos principais:

1.º O contrato de governação: Este é o cérebro do DAO. Gere as propostas e votações,
2. O contrato de tesouraria: Gere os fundos do DAO e aplica um período de segurança antes de executar as decisões,
3. O contrato de poder de voto: calcula o poder de voto de cada membro,
4.º O contrato de incentivo: Distribui recompensas aos membros ativos do DAO que votaram e bloquearam os seus REG (se um período for ativado pelo DAO).
 

#### **⭐⭐ Para os iniciados**

O RealToken DAO baseia-se num conjunto de contratos inteligentes interligados:

1. Contrato de governação: Baseado na norma OpenZeppelin Governor, com pequenas modificações, gere o ciclo de vida das propostas, o processo de votação e a execução das decisões on-chain,
2. Contrato do Tesouro (REGTreasuryDAO): Implementa um mecanismo de timelock para garantir a execução das decisões do DAO e gerir os seus fundos,
3. Contrato de poder de voto (PowerVotingRegistry): Regista o poder de voto dos titulares de REG com base no seu saldo, impulso e atividade, permitindo assim uma maior flexibilidade do que o modelo clássico de votação de tokens, 1 token = 1 voto,
4. Contrato de incentivo: Gere a distribuição de recompensas para incentivar a participação ativa na governação, para isso deve votar e bloquear os seus REGs por um período determinado (época).

Estes contratos interagem para garantir o funcionamento transparente e seguro do DAO, ao mesmo tempo que incentivam a participação ativa na governação.


#### **⭐⭐⭐ Para especialistas**

A arquitetura técnica do RealToken DAO é composta por vários contratos inteligentes importantes:

##### **4.1.1. Contrato de governação**

- Baseado no OpenZeppelin Governor com modificações personalizadas,
- Gere todo o ciclo de vida das propostas: criação, votação e execução,
- Integra interações com timelock para aumentar a segurança de execução,
- Implementa controlos de acesso para a criação de propostas, permite a ativação de 4 modos diferentes para autorizar a criação de propostas,
- Interage diretamente com o PowerVotingRegistry para criar instantâneos de poderes de voto com base nos valores registados no PowerVotingRegistry.
 

##### **4.1.2. Contrato do Tesouro (REGTeasuryDAO)**

- Baseado no TimelockControllerUpgradeable do OpenZeppelin,
- Implementa um mecanismo de timelock para adicionar um atraso de segurança antes de executar decisões na cadeia,
- Gere os fundos DAO e controla a sua utilização,
- Integra funções específicas (UPGRADER\_ROLE) para gestão de atualizações de contratos,
- Utiliza o standard UUPS (Universal Upgradeable Proxy Standard) para permitir futuras atualizações,
- Interage com contratos externos convocados por propostas.
 

##### **4.1.3. Contrato de poder de votação (PowerVotingRegistry)**

- Regista o poder de voto com base no saldo do REG e noutros fatores (por exemplo, duração da detenção, bloqueio, pool de liquidez, etc.),
- Implementa lógica de autodelegação para simplificar a ativação do poder de voto.

##### **4.1.4. Contrato de incentivo**

- Integra uma lógica de estado que modifica as ações possíveis, o ciclo completo é chamado de “época”, para cada época deve votar e bloquear os seus REGs para receber recompensas,
- Distribui recompensas aos participantes ativos da governação (que votam),
- Utiliza mecanismos de bloqueio para incentivar o envolvimento a longo prazo (bloqueio de duração da época),
- Integra os cálculos de recompensas com base na atividade de governação (votos e bloqueio de REG).

Estes contratos são concebidos para serem modulares e escaláveis, permitindo futuras atualizações e melhorias sem perturbar todo o sistema de governação. O contrato REGTreasuryDAO, em particular, acrescenta uma camada adicional de segurança ao impor um atraso entre a proposta de uma ação e a sua execução. execução, permitindo que a comunidade responda se necessário.

Em última análise, todos os contratos no ecossistema RealToken serão controlados, atualizáveis ​​apenas pelo contrato REGTeasuryDAO através de propostas, apenas algumas funções de emergência poderão ser executadas fora do ciclo DAO por um comité de segurança que terá direitos limitados a ações de emergência para garantir contratos .

## **4.2. Mecanismos de votação e proposta**

#### **⭐ Para principiantes**

O RealToken DAO permite-lhe participar em decisões importantes no ecossistema de aplicações relacionadas com o DAO. Veja como funciona:

1. No fórum DAO, os membros podem sugerir ideias para melhorar o DAO e o ecossistema,
2.º É realizado um debate em torno das propostas para medir a viabilidade e o interesse das propostas por parte da comunidade,
3.Se uma proposta obtiver um número suficiente de votos, é transformada em proposta e submetida a votação na DAO,
4.º Os membros do DAO podem votar a favor ou contra a proposta,
5.Se a proposta for aprovada, a mesma é executada pelo REGTeasuryDAO.

#### **⭐⭐ Para os iniciados**

O sistema de votação e proposta RealToken DAO inclui:

1. Fórum DAO: Onde os membros podem propor ideias e debater para avaliar o interesse das propostas,
2.º Durante o período de debate, a comunidade deve avaliar o interesse e a exequibilidade das propostas,
3.No caso de uma proposta ser julgada viável e relevante, trabalha-se uma proposta concreta com o estabelecimento de todos os parâmetros ligados à proposta, como as ações a executar on-chain, o custo de desenvolvimento, os ganhos para o DAO, etc. ...
4. Criação de propostas: Processo controlado por regras de acesso definidas no contrato de governação, dependendo dos parâmetros poderá haver restrições na criação de propostas, isto visa evitar ataques de governação, e permitir uma implementação gradual das regras de funcionamento do DAO em total segurança ,
5.º Ciclo de vida da proposta: Criação, período de votação, execução (caso seja aprovada).
6. Mecanismo de timelock: Atraso de segurança antes da execução das decisões aprovadas.

#### **⭐⭐⭐ Para especialistas**

Os mecanismos de votação e de proposta são implementados da seguinte forma:

1.º Fórum DAO:
 - Onde os utilizadores possam propor ideias e debater para medir o interesse das propostas,
 - Ocorre um debate em torno das propostas, a fim de medir a viabilidade e o interesse das propostas pela comunidade ou por um dos fornecedores de DAO,
 - Caso uma proposta seja julgada viável e relevante, trabalha-se uma proposta concreta com o estabelecimento de todos os parâmetros ligados à proposta, como as ações a executar on-chain, o custo de desenvolvimento, os ganhos para o DAO, estudar impacto e risco, necessidade ou não de auditoria de segurança, etc.
2. Criação de propostas:
 - Controlado pelo contrato de governação,
 - São possíveis quatro modos de autorização para a criação de propostas, estes modos de proposta visam otimizar a gestão de risco do DAO, e limitar a exposição dos fundos, com um objetivo de descentralização a médio prazo dependendo da maturidade do DAO,
 - Registo on-chain da proposta com todos os códigos que permitem a execução da proposta caso esta seja aprovada,
 - Interação com o PowerVotingRegistry para criar instantâneos dos poderes de voto.
3.º Processo de votação:
 - Baseado no standard OpenZeppelin Governor com modificações personalizadas,
 - Utilização do poder de voto registado no PowerVotingRegistry,
 - Possibilidade de votação: a favor, contra ou abstenção,
 - Apenas os membros registados no PowerVotingRegistry podem votar,
 - É aplicado um atraso de 1 dia entre o início da votação e a validação da transação para a criação da proposta (por segurança, para possível cancelamento),
 - A votação é feita durante um período determinado pelo contrato de governação, geralmente 7 dias,
 - Os eleitores verificam se a proposta está de acordo com a descrição e trocam no fórum DAO (on-chain execution code check).
4. Cálculo do poder de voto
 - Registado no PowerVotingRegistry,
 - Tem em conta o saldo REG, o período de detenção, o bloqueio e potencialmente outros fatores, de acordo com o algoritmo de cálculo validado pelo DAO,
 - Implementa lógica de autodelegação para simplificar a ativação do poder de voto.
5. Execução de propostas:
 - Utilização de REGTeasuryDAO como timelock para adicionar um atraso de segurança,
 - Execução on-chain das ações aprovadas após o prazo,
 - Possibilidade de cancelamento durante o período de timelock se necessário, por grupo de segurança.
6. Sistema de incentivos ação:
 - Gerido pelo contrato de incentivo,
 - Recompensas baseadas na participação na votação e bloqueio de REG,
 - Sistema Epoch para estruturar períodos de recompensa,
 - Ativado pelo DAO que define a duração das épocas e as recompensas associadas.
7.º Método de proposta:
 - ProposeWithRole: O objetivo é limitar estritamente os endereços que podem enviar voto, de forma a evitar spam e propostas abusivas. Este modo será ativado apenas durante as primeiras semanas ou meses da implementação inicial da governação (versão 1), para estabelecer uma base sólida antes de expandir o número de autores de propostas,
 - ProposeWithVotingPower: Esta modalidade exige que o autor da proposta tenha um poder mínimo de voto. Sem este poder, nenhuma proposição pode ser criada. Esta é a fase final da descentralização, onde todos os que tiverem poder de voto suficiente podem apresentar propostas. Já não existem funções privilegiadas; todos os titulares seguem as mesmas regras,
 - ProposerWithRoleAndVotingPower: Esta modalidade combina duas restrições, o autor da proposta deve ter uma função específica e um poder de voto mínimo. Se alguma das condições não for cumprida, a proposta falhará. Esta modalidade estende as propostas aos membros mais empenhados da comunidade, exigindo um saldo mínimo de REG para garantir o seu compromisso e participação financeira. Devem ser eleitos e ter REG suficientes. Isto permite iniciar a descentralização, testar processos e limitar riscos,
 - ProposeWithRoleOrVotingPower: Este modo permite a criação de propostas caso o autor da proposta possua a função exigida ou poder de voto mínimo. Se nenhuma destas condições for cumprida, a proposta fracassará. Esta é a penúltima etapa da descentralização da governação. Esta modalidade exige que um grande número de REG proponha melhorias livremente, sem passar por um papel específico. Isto permite que qualquer pessoa proponha melhorias que envolvam um número significativo de REG, ao mesmo tempo que mantém remetentes específicos de funções que não exigem um REG mínimo. A função pode ser removida por incumprimento de processos.
8.º Delegação:
 - A delegação do poder de voto foi removida para permitir a ativação de incentivos para os eleitores e para os armários do REG,
 - Os utilizadores não podem delegar o seu poder de voto noutros endereços, mas beneficiam da autodelegação por defeito,
 - Numa v2 a delegação regressará e será mesmo uma parte importante do sistema.

Estes mecanismos são concebidos para garantir uma governação justa, segura e encorajadora, permitindo ao mesmo tempo a evolução futura do sistema.

Nas primeiras semanas/meses o DAO é implementado com restrições de forma a permitir a implementação de regras de funcionamento, experimentar os processos e limitar os riscos.

Para dar uma boa visão das fases de evolução, será necessário realizar um debate para determinar os níveis que desencadeiam uma descentralização de nível superior, dando maior autonomia e libertando a confiança proporcionada aos vários intervenientes.




## **4.3. Sistema de incentivos e recompensas**

#### **⭐ Para principiantes**

O RealToken DAO recompensa-o pela sua participação ativa. Veja como funciona:

1.º Ganha recompensas de diversas formas votando em propostas,
2.º Pode aumentar as suas recompensas bloqueando os seus tokens REG durante um determinado período,
3.º A participação em determinados pools de liquidez ou outros no ecossistema RealToken pode proporcionar-lhe recompensas adicionais,
4.As recompensas podem ser sob a forma de REG, ERC20, ou mesmo sob a forma de aumento do poder de voto,
5.º O DAO decide as recompensas e os parâmetros do sistema de incentivos.

#### **⭐⭐ Para os iniciados**

O sistema de incentivos RealToken DAO foi concebido para incentivar a participação ativa:

1.º A DAO define as recompensas e os parâmetros do sistema de incentivos, o tipo e os montantes das recompensas,
2.As recompensas baseadas na participação com voto e bloqueio de REG, participação em determinados pools de liquidez ou de outra forma no ecossistema RealToken podem oferecer recompensas adicionais,
3. Sistema Epoch para estruturar períodos de recompensa e otimizar a participação,
4. Alocação automática na cadeia de recompensas de moedas estáveis ​​​​exigíveis no final de cada época,
5. Possibilidade de o DAO ajustar os parâmetros do sistema de incentivos de forma a otimizar a participação e responder à evolução do DAO.

#### **⭐⭐⭐ Para especialistas**

O sistema de incentivos e recompensas é implementado da seguinte forma:

1. Contrato de incentivo:
 - Gere o estado do sistema (ativo/inativo) e ciclos de época,
 - Calcula as recompensas com base na atividade de votação e bloqueio de REG,
 - Atribui recompensas automaticamente no final de cada época.
2. Mecanismo de bloqueio:
 - Os utilizadores podem bloquear os seus REG durante uma época no contrato de incentivo,
 - O bloqueio pode aumentar o poder de voto e as potenciais recompensas de acordo com os parâmetros definidos pelo DAO,
 - O bloqueio permite marcar um compromisso de longo prazo por parte do eleitor, reduz a liquidez dos REG e limita os riscos de ataques à governação.
3. Parâmetros ajustáveis:
 - Duração das épocas,
 - Quantidade de recompensas por época,
 - Tipo de recompensas,
 - Estes parâmetros são aplicados pelo DAO na proposta de ativação de época,
 - Para aumentos do poder de voto, o DAO pode ajustar de acordo com as necessidades: a fórmula e o peso atribuído no sistema de cálculo powerVoting que gera os poderes de voto.
4. Integração com o PowerVotingRegistry:
 - O bloqueio REG afeta o poder de voto registado,
 - Sinergia entre a participação na governação e as recompensas.
5. Segurança e escalabilidade:
 - Contrato atualizável para permitir melhorias futuras,
 - Mecanismos de pausa em caso de emergência.
6.º Cálculo de recompensas:
 - Conta o número total de propostas emitidas durante uma época,
 - Conta o número de votos feitos por cada eleitor durante a época,
 - Ponderado pela quantidade de REGs bloqueados.

```texto simples
userBonus = (userState.depositAmount * userState.voteAmount * epochState.totalBonus) / epochState.totalWeights;
```

- \`userState.depositAmount\`: O valor depositado pelo utilizador neste momento no REG,
- \`userState.voteAmount\`: O número de votos feitos pelo utilizador durante este período,
- \`epochState.totalBonus\`: O valor total dos bónus alocados a esta época,
- \`epochState.totalWeights\`: A soma total dos pesos de todos os utilizadores nesta época.

Esta fórmula é aplicada no contrato às seguintes linhas:

[REGIncentiveVault. sol](https://gnosisscan.io/address/0x4b79755d1ea8937c027408e3aa72d69a260f6237#code#F1#L342) da linha 342 a 346.

Os pesos totais são incrementados pelo valor depositado pelo utilizador para cada um dos seus votos (ver linha 245 do contrato).

Este sistema visa criar um ciclo virtuoso de envolvimento na governação, recompensando a participação activa e ao mesmo tempo fortalecendo a estabilidade a longo prazo do ecossistema RealToken.

A mecânica de recompensa será totalmente revista com o comissionamento da governação V2 utilizando NFTs de forma a melhorar o sistema de incentivos, permitir uma maior granularidade e precisão das ações a incentivar de acordo com as necessidades do DAO e dos perfis do titular.

Serão introduzidos novos tipos de recompensas como a matéria escura do NFT Cityzen, pontos de apoio para votos permitindo o destaque de conteúdos ligados à Atividade NFT, etc...

O ecossistema RealToken foi concebido para ter inúmeras ferramentas para otimizar a participação no voto e incentivar diversas ações e contribuições que são benéficas para o DAO, ao mesmo tempo que permite o controlo preciso das finanças do DAO para não desperdiçar dinheiro ou desvalorizar o token REG.
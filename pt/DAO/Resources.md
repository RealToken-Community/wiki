---
title: 7. Recursos e apoio
description: 
published: true
date: 2024-10-11T10:18:01.521Z
tags: 
editor: markdown
dateCreated: 2024-10-11T10:06:43.357Z
---

## 7.1. Documentação técnica

### 7.1.1. Contratos Inteligentes v1 ⭐⭐⭐

Documentar os contratos inteligentes é essencial para compreender o funcionamento interno do RealToken DAO. Inclui:

#### Especificações técnicas dos principais contratos:

1. **REGGovernor.sol**: Contrato mestre de governação, este contrato gere o processo de governação, incluindo a criação, votação e execução de propostas.
 Funções principais não standard adicionadas:

 - **`function setProposeMode(ProposeMode proporMode) external `**

  **Descrição:** Permite que a governação defina o modo de proposição, determinando os critérios que os endereços devem cumprir para criar propostas.
  **Parâmetros:**
  **proposerMode**: O novo modo do proponente a definir, que pode ser um dos seguintes:
  - **`ProposerWithRole`**: Apenas os endereços com a função **`PROPOSER_ROLE`** podem propor.
  - **`ProposeWithVotingPower`**: Apenas os endereços com poder de voto suficiente podem propor.
  - **`ProposerWithRoleAndVotingPower`**: Apenas os endereços com a função **`PROPOSER_ROLE`** e poder de voto suficiente podem propor.
  - **`ProposerWithRoleOrVotingPower`**: Os endereços com a função **`PROPOSER_ROLE`** ou poder de voto suficiente podem propor.

  **Controlo de acesso:** Só pode ser chamado através de uma proposta de governação bem-sucedida (`onlyGovernance`).
  **Evento emitido:** `SetProposerMode`

 - **`function setIncentiveEnabled(bool status) external `**
  **Descrição:** ativa ou desativa o mecanismo de incentivo, que regista os votos no cofre de incentivos para recompensar os participantes.
  **Parâmetros:** `status` Um booleano que indica se o mecanismo de incentivo deve ser ativado (`true`) ou desativado (`false`).
  **Controlo de acesso:** só pode ser invocado através de uma proposta de governação bem-sucedida.
  **Evento emitido:** `SetIncentiveEnabled`

 - **`function setRegIncentiveVault(IREGIncentiveVault regIncentiveVault) external `**
  **Descrição:** Define o endereço do contrato do cofre de incentivos REG utilizado para registar os votos e distribuir os incentivos.
  **Parâmetros:** `regIncentiveVault` O endereço do novo contrato do cofre de incentivos REG.
  **Controlo de acesso:** só pode ser invocado através de uma proposta de governação bem-sucedida.
  **Evento emitido:** `SetRegIncentiveVault`

 - **`função getProposerMode() retornos de visualização externa (ProposerMode) `**
  **Descrição:** Retorna o modo nominador atual, indicando os critérios necessários para que um endereço crie propostas.
  **Retorna:** O `ProposeMode` atual.

 - **`function getIncentiveEnabled() external view returns (bool) `**
  **Descrição:** Indica se o mecanismo de incentivo está atualmente ativado.
  **Retorna:** `true` se o mecanismo de incentivo estiver ativado, `false` caso contrário.

 - **`função getRegIncentiveVault() retornos de visualização externa (IREGIncentiveVault) `**
  **Descrição:** Retorna o endereço do contrato do cofre de incentivos REG.
  **Retorna:** O endereço do contrato `IREGIncentiveVault`.

 - **`function cancelByAdmin(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) external returns (uint256)`**
  **Descrição:** Permite que um administrador com a função `CANCELLER_ROLE` cancele uma proposta que corresponda aos parâmetros fornecidos.
  **Parâmetros:**
  - `targets`: Uma tabela de endereços alvo para as ações da proposta.
  - `valores`: Uma tabela de valores ETH para enviar em cada ação.
  - `calldatas`: Uma tabela de dados de chamada para cada ação.
  - `descriptionHash`: O hash keccak256 da descrição da proposta.

  **Retorna:** O ID da proposta cancelada.
  **Controlo de acessos:** Restrito a endereços com a função `CANCELLER_ROLE`.

2. **REGTreasuryDAO.sol**: gestão de tesouraria DAO, este contrato gere os fundos DAO e executa transações aprovadas após um período de bloqueio de segurança.
 Não há nenhuma função principal não padrão adicionada.
 
3. **REGIncentiveVault.sol**: Sistema de incentivos e recompensas, este contrato gere o cálculo e distribuição de recompensas aos participantes do DAO se ativado pelo DAO.
 Contrato inteiramente criado pela RealT, as principais funções são:
 - **`function setNewEpoch(uint256 subscriptionStart,uint256 subscriptionEnd,uint256 lockPeriodEnd,address bonusToken,uint256 totalBonus ) apenas externoRole(DEFAULT_ADMIN_ROLE)`**
  **Descrição:** Define uma nova época (período) para o programa de incentivos. Esta função inicializa uma nova época com os parâmetros ser especificado.
  **Parâmetros:**
  - `subscriptionStart`: Timestamp do início do período de subscrição.
  - `subscriptionEnd`: Timestamp do final do período de subscrição.
  - `lockPeriodEnd`: Timestamp do final do período de bloqueio.
  - `bonusToken`: Endereço do token utilizado como recompensa.
  - `totalBonus`: Quantidade total de tokens de bónus a distribuir para esta época.

  **Controlo de acesso:** Apenas a função `DEFAULT_ADMIN_ROLE` pode chamar esta função.
  **Evento emitido:** `SetNewEpoch`

 - **`função depósito(quantidade uint256) public whenNotPaused;`**
  **Descrição:** Permite aos utilizadores depositar tokens REG no cofre durante o período de subscrição para participar no programa de incentivos.
 **Parâmetros:** `amount`: Número de tokens REG a depositar.
  **Condições:** A função só pode ser chamada quando o contrato não está em pausa e durante o período de subscrição.
  **Evento Emitido:** `Depósito`

 - **`function depositWithPermit(quantidade uint256, prazo uint256, uint8 v, bytes32 r, bytes32 s ) externo quandoNotPaused;`**
  **Descrição:** Permite aos utilizadores depositar tokens REG utilizando a aprovação (permissão) EIP-2612, que permite o depósito numa única transação sem aprovação prévia.
 **Parâmetros:**
  - `amount`: Quantidade de tokens REG a depositar.
  - `deadline`: carimbo de data/hora após o qual a assinatura deixa de ser válida.
  - `v, r, s`: Componentes de assinatura para aprovação.

  **Condições:** A função só poderá ser chamada quando o contrato não estiver em pausa e durante o período de subscrição.
  **Evento Emitido:** `Depósito`

 - **`function retirar(quantidade uint256) externo quandoNotPaused;`**
  **Descrição:** Permite aos utilizadores levantar os seus tokens REG depositados após o término do período de bloqueio.
  **Parâmetros:** `amount`: Número de tokens REG a retirar.
  **Condições:** A função só pode ser chamada quando o contrato não está em pausa e após o fim do período de bloqueio.
  **Evento emitido:** `Withdraw`

 - **`função recordVote(endereço utilizador, uint256 propostaId) apenas externoGovernação;`**
  **Descrição:** regista o voto de um utilizador numa proposta durante o período de bloqueio para calcular as recompensas de incentivo.
  **Parâmetros:**

  - `user`: Endereço do utilizador que votou.
  - `proposalId`: Identificador da proposta na qual o utilizador votou.

  **Controlo de acesso:** apenas o contrato de governação pode chamar esta função.
 **Evento emitido:** `RecordVote` ou `RecordVoteNotActive`

 - **`function calculaBonus(user address) external view returns (address[] memory, uint256[] memory)`**
 **Descrição:** Calcula o valor do bónus que um utilizador pode reclamar em cada época passada.
 **Parâmetros:**
  - `user`: Endereço do utilizador para quem calcular o bónus.

  **Retornos:**
  - `address[]`: Tabela de endereços de tokens de bónus.
  - `uint256[]`: Tabela de valores de bónus correspondentes.

 - **`função ClaimBonus() public whenNotPaused;`**
 **Descrição:** Permite aos utilizadores reivindicar os seus bónus acumulados em todas as épocas elegíveis.
 **Condições:** A função só pode ser chamada quando o contrato não está em pausa.
 **Evento emitido:** `ClaimBonus`

 - **`função getRegGovernor() retornos de visualização externa (endereço)`**
 **Descrição:** Retorna o endereço do contrato de governação REG associado.

 - **`função getRegToken() retornos de visualização externa (IERC20)`**
  **Descrição:** Retorna o endereço do token REG utilizado pelo cofre.

 - **`função getCurrentTotalDeposit() retornos de visualização externa (uint256)`**
 **Descrição:** Retorna a quantidade total de tokens REG atualmente depositados no cofre.

 - **`função getCurrentEpoch() retornos de visualização externa (uint256)`**
 **Descrição:** Retorna o número da época atual.

 - **`função getCurrentEpochState() retornos de visualização externa (memória EpochState)`**
 **Descrição:** Retorna informação sobre o estado da época atual.

 - **`function getEpochState(uint256 epoch) external view returns (EpochState memory)`**
 **Descrição:** Retorna informação sobre o estado de uma época específica.
 **Parâmetros:**`época` Número da época para a qual obter informação.

 - **`function getUserEpochState(user address, epoch uint256) external view returns (UserEpochState memory)`**
  **Descrição:** Retorna informação sobre o estado de um utilizador numa época específica.
 **Parâmetros:**
  - `utilizador`: Endereço do utilizador.
  - `época`: número da época.

 - **`função getUserGlobalState (endereço do utilizador) retornos de visualização externa (memória UserGlobalState)`**
 **Descrição:** Retorna informação global sobre o estado de um utilizador.
 **Parâmetros:**
  - `utilizador`: Endereço do utilizador.
 
5. **REGPowerVotingRegistry.sol**: Registo dos poderes de voto, este contrato regista o poder de voto dos participantes calculado off-chain pela RealT de acordo com o algoritmo definido e validado pelo DAO.
Contrato inteiramente criado pela RealT, as principais funções são:
  - `registerVotingPower(VotingPower[] calldata votePower) external override onlyRole(REGISTER_ROLE)` Irá reparar que, por razões de compatibilidade com as normas e o funcionamento do contrato Governor, o contrato PowerVotingRegistry é baseado na norma ERC20 com uma modificação do comportamento das funções básicas como:

  - transferir, transferir de, aprovar, etc. são substituídos para retornar falso
  - delegado é substituído para devolver um erro

  - `REGVotingPowerRegistryErrors.DelegateToOtherNotAllowed()` se um utilizador tentar delegar o seu voto a outro utilizador.
 

#### Guias de interação contratual:

- Principais métodos:
 - `propose()` no REGGovernor para criar uma proposta,
 - `castVote()` no REGGovernor para votar uma proposta,
 - `queue()` no REGGovernor para enfileirar uma proposta,
 - `execute()` em REGTerasuryDAO para executar uma proposta aprovada.
 
- Eventos importantes:
 - `ProposalCreated` emitido aquando da criação de uma proposta,
 - `VoteCast` emitido quando é registado um voto,
 - `callExecuted()` emitido quando uma ação da proposta é executada.
 

#### Diagramas de arquitetura:

\[*Um diagrama que mostra as interações entre contratos*\]
![diagram_en.svg](/assets/img/diagram_en.svg)


Relatórios de auditoria de segurança:

Não existe relatório de auditoria de segurança neste momento. Uma vez que os contratos estão próximos das versões standard (já auditadas) e a governação é exercida com restrições de direitos, considerou-se mais relevante conservar recursos para futuras auditorias da v2. Sendo v1 uma versão experimental provisória destinada a inicializar o DAO e, por isso, o âmbito de ação é limitado.

### 7.1.2. Mecanismos de governação ⭐⭐

Esta secção detalha os processos de governação do DAO:

- Ciclo de vida da proposta:
 - Discussão e elaboração da proposta no fórum da governação,
 - Criação e submissão,
 - Período de votação,
 - Execução e timelock.
- Cálculo do poder de voto:
 - Fórmulas utilizadas fora da cadeia pela RealT,
 - Fatores que influenciam o poder de voto (bloqueio, período de detenção, utilização).
- Sistema de incentivos:
  - Mecanismo de distribuição de recompensas
  - Cálculo de bónus com base na participação em votações e bloqueio de REGs

### 7.1.3. Interfaces de utilizador ⭐

As interações são feitas principalmente com as seguintes interfaces:

- Discussões sobre governação ([URL](https://forum.realtoken.community)):
 - Fórum de Governação,
 - Alojado e gerido pela comunidade,
 - Permite discutir ideias, determinar interesses, prioridades e elaborar propostas.
- Interface de votação ([URL](https://www.tally.xyz/gov/realtoken-ecosystem-governance)):
 - Utilização da plataforma Tally como interface de votação,
 - Acesso às principais funções de votação, visualização, resultados, votação, criação de propostas, estado das propostas,
 - A utilização do Tally poupa vários meses de desenvolvimento,
 - Implica alguns constrangimentos no funcionamento e na informação apresentada.
- Interface de incentivo ([URL](https://vote.realtoken.network)):
 - Interface dedicada criada pela RealT,
 - Permite depositar REGs, retirar REGs, reivindicar recompensas.

## 7.2. Canais de comunicação

### 7.2.1. Canais oficiais ⭐

- Site oficial da comunidade: [realtoken.community](https://www.realtoken.community/)
- Fórum de governação: [forum.realtoken.community](https://forum.realtoken.community)
- Twitter: @RealTokenDAO

### 7.2.2. Recursos para programadores ⭐⭐⭐

- GitHub: repositório de contratos inteligentes → [Governação, incentivo, tesouraria](https://github.com/real-token/reg-governance-core)
- GitHub: interfaces de incentivo → [github.com/real-token/voting-front/tree/incentive](https://github.com/real-token/voting-front/tree/incentive)

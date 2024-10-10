## 7.1. Documentación técnica

### 7.1.1. Contratos inteligentes v1 ⭐⭐⭐

La documentación de los contratos inteligentes es esencial para comprender el funcionamiento interno de la DAO RealToken. Incluye:

#### Especificaciones técnicas de los principales contratos:

1.  **REGGovernor.sol**: Contrato de gobernanza principal, este contrato gestiona el proceso de gobernanza, incluyendo la creación de propuestas, la votación y la ejecución.  
    Principales funciones no estándar añadidas:

    - **`function setProposerMode(ProposerMode proposerMode) external `**

      **Descripción:** Permite a la gobernanza definir el modo de proponente, determinando los criterios que las direcciones deben cumplir para crear propuestas.  
      **Parámetros:**  
      **proposerMode**: El nuevo modo de proponente a establecer, que puede ser uno de los siguientes:

      - **`ProposerWithRole`**: Solo las direcciones con el rol **`PROPOSER_ROLE`** pueden proponer.
      - **`ProposerWithVotingPower`**: Solo las direcciones con suficiente poder de voto pueden proponer.
      - **`ProposerWithRoleAndVotingPower`**: Solo las direcciones con el rol **`PROPOSER_ROLE`** y suficiente poder de voto pueden proponer.
      - **`ProposerWithRoleOrVotingPower`**: Las direcciones con el rol **`PROPOSER_ROLE`** o suficiente poder de voto pueden proponer.

      **Control de acceso:** Solo puede ser llamada a través de una propuesta de gobernanza exitosa (`onlyGovernance`).  
      **Evento emitido:** `SetProposerMode`

    - **`function setIncentiveEnabled(bool status) external `**  
      **Descripción:** Activa o desactiva el mecanismo de incentivos, que registra los votos en la bóveda de incentivos para recompensar a los participantes.  
      **Parámetros:** `status` Un booleano que indica si se debe activar (`true`) o desactivar (`false`) el mecanismo de incentivos.  
      **Control de acceso:** Solo puede ser llamada a través de una propuesta de gobernanza exitosa.  
      **Evento emitido:** `SetIncentiveEnabled`
    - **`function setRegIncentiveVault(IREGIncentiveVault regIncentiveVault) external `**  
      **Descripción:** Establece la dirección del contrato de la bóveda de incentivos REG utilizada para registrar votos y distribuir incentivos.  
      **Parámetros:** `regIncentiveVault` La dirección del nuevo contrato de la bóveda de incentivos REG.
      **Control de acceso:** Solo puede ser llamada a través de una propuesta de gobernanza exitosa.  
      **Evento emitido:** `SetRegIncentiveVault`

    - **`function getProposerMode() external view returns (ProposerMode) `**  
      **Descripción:** Devuelve el modo de proponente actual, indicando los criterios requeridos para que una dirección pueda crear propuestas.  
      **Retorna:** El `ProposerMode` actual.
    - **`function getIncentiveEnabled() external view returns (bool) `**  
      **Descripción:** Indica si el mecanismo de incentivos está actualmente activado.  
      **Retorna:** `true` si el mecanismo de incentivos está activado, `false` en caso contrario.

    - **`function getRegIncentiveVault() external view returns (IREGIncentiveVault) `**  
      **Descripción:** Devuelve la dirección del contrato de la bóveda de incentivos REG.  
      **Retorna:** La dirección del contrato `IREGIncentiveVault`.

    - **`function cancelByAdmin(address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) external returns (uint256)`**  
      **Descripción:** Permite a un administrador con el rol `CANCELLER_ROLE` cancelar una propuesta que coincida con los parámetros dados.  
      **Parámetros:**

      - `targets`: Un array de direcciones objetivo para las acciones de la propuesta.
      - `values`: Un array de valores ETH a enviar con cada acción.
      - `calldatas`: Un array de datos de llamada para cada acción.
      - `descriptionHash`: El hash keccak256 de la descripción de la propuesta.

      **Retorna:** El ID de la propuesta cancelada.
      **Control de acceso:** Restringido a direcciones con el rol `CANCELLER_ROLE`.

2.  **REGTreasuryDAO.sol**: Gestión de la tesorería de la DAO, este contrato gestiona los fondos de la DAO y ejecuta las transacciones aprobadas después de un período de timelock de seguridad.  
    No hay funciones principales no estándar añadidas.  

3.  **REGIncentiveVault.sol**: Sistema de incentivos y recompensas, este contrato gestiona el cálculo y la distribución de recompensas a los participantes de la DAO si está activado por la DAO.  
     Contrato completamente creado por RealT, las funciones principales son:

    - **`function setNewEpoch(uint256 subscriptionStart,uint256 subscriptionEnd,uint256 lockPeriodEnd,address bonusToken,uint256 totalBonus ) external onlyRole(DEFAULT_ADMIN_ROLE)`**
      **Descripción:** Define una nueva época (período) para el programa de incentivos. Esta función inicializa una nueva época con los parámetros especificados.
      **Parámetros:**
      - `subscriptionStart`: Timestamp del inicio del período de suscripción.
    - `subscriptionEnd`: Timestamp del final del período de suscripción.

      - `lockPeriodEnd`: Timestamp del final del período de bloqueo.
      - `bonusToken`: Dirección del token utilizado como recompensa.
      - `totalBonus`: Cantidad total de tokens de bonificación a distribuir para esta época.

      **Control de acceso:** Solo el rol `DEFAULT_ADMIN_ROLE` puede llamar a esta función.
      **Evento emitido:** `SetNewEpoch`

    - **`function deposit(uint256 amount) public whenNotPaused;`**
      **Descripción:** Permite a los usuarios depositar tokens REG en la bóveda durante el período de suscripción para participar en el programa de incentivos.
      **Parámetros:** `amount`: Cantidad de tokens REG a depositar.  
       **Condiciones:** La función solo puede ser llamada cuando el contrato no está en pausa y durante el período de suscripción.  
      **Evento emitido:** `Deposit`
    - **`function depositWithPermit(uint256 amount, uint256 deadline, uint8 v, bytes32 r, bytes32 s ) external whenNotPaused;`**  
       **Descripción:** Permite a los usuarios depositar tokens REG utilizando la aprobación EIP-2612 (permit), lo que permite el depósito en una sola transacción sin aprobación previa.
      **Parámetros:**

      -     `amount`: Cantidad de tokens REG a depositar.
      -     `deadline`: Timestamp después del cual la firma ya no es válida.
      -      `v, r, s`: Componentes de la firma para la aprobación.

      **Condiciones:** La función solo puede ser llamada cuando el contrato no está en pausa y durante el período de suscripción.  
      **Evento emitido:** `Deposit`

    - **`function withdraw(uint256 amount) external whenNotPaused;`**  
       **Descripción:** Permite a los usuarios retirar sus tokens REG depositados después del final del período de bloqueo.  
       **Parámetros:** `amount`: Cantidad de tokens REG a retirar.
      **Condiciones:** La función solo puede ser llamada cuando el contrato no está en pausa y después del final del período de bloqueo.
      **Evento emitido:** `Withdraw`

    - **`function recordVote(address user, uint256 proposalId) external onlyGovernance;`**
      **Descripción:** Registra el voto de un usuario en una propuesta durante el período de bloqueo para calcular las recompensas de incentivos.
      **Parámetros:**

      - `user`: Dirección del usuario que ha votado.
      - `proposalId`: Identificador de la propuesta en la que el usuario ha votado.

      **Control de acceso:** Solo el contrato de gobernanza puede llamar a esta función.
      **Evento emitido:** `RecordVote` o `RecordVoteNotActive`

    - **`function calculateBonus(address user) external view returns (address[] memory, uint256[] memory)`**
      **Descripción:** Calcula la cantidad de bonificación que un usuario puede reclamar para cada época pasada.
      **Parámetros:**

      - `user`: Dirección del usuario para el que calcular la bonificación.

        **Retorna:**

      - `address[]`: Array de direcciones de los tokens de bonificación.
      - `uint256[]`: Array de las cantidades de bonificación correspondientes.

    - **`function claimBonus() public whenNotPaused;`**
      **Descripción:** Permite a los usuarios reclamar sus bonificaciones acumuladas para todas las épocas elegibles.
      **Condiciones:** La función solo puede ser llamada cuando el contrato no está en pausa.
      **Evento emitido:** `ClaimBonus`

    - **`function getRegGovernor() external view returns (address)`**
      **Descripción:** Devuelve la dirección del contrato de gobernanza REG asociado.

    - **`function getRegToken() external view returns (IERC20)`**
      **Descripción:** Devuelve la dirección del token REG utilizado por la bóveda.

    - **`function getCurrentTotalDeposit() external view returns (uint256)`**
      **Descripción:** Devuelve la cantidad total de tokens REG actualmente depositados en la bóveda.

    - **`function getCurrentEpoch() external view returns (uint256)`**
      **Descripción:** Devuelve el número de la época actual.

    - **`function getCurrentEpochState() external view returns (EpochState memory)`**
      **Descripción:** Devuelve la información sobre el estado de la época actual.

    - **`function getEpochState(uint256 epoch) external view returns (EpochState memory)`**
      **Descripción:** Devuelve la información sobre el estado de una época específica.
      **Parámetros:** `epoch` Número de la época para la que obtener la información.

    - **`function getUserEpochState(address user, uint256 epoch) external view returns (UserEpochState memory)`**
      **Descripción:** Devuelve la información sobre el estado de un usuario para una época específica.
      **Parámetros:**
      - `user`: Dirección del usuario.
    - `epoch`: Número de la época.

    - **`function getUserGlobalState(address user) external view returns (UserGlobalState memory)`**
      **Descripción:** Devuelve la información global sobre el estado de un usuario.
      **Parámetros:**
    - `user`: Dirección del usuario.

4.  **REGPowerVotingRegistry.sol**: Registro de los poderes de voto, este contrato registra el poder de voto de los participantes calculado off-chain por RealT según el algoritmo definido y validado por la DAO.  
    Contrato completamente creado por RealT, las funciones principales son: - `registerVotingPower(VotingPower[] calldata votingPower) external override onlyRole(REGISTER_ROLE)` Notarás que por razones de compatibilidad con los estándares y el funcionamiento del contrato Governor, el contrato PowerVotingRegistry está basado en el estándar ERC20 con una modificación de los comportamientos de las funciones básicas como:

        	- transfer, transferFrom, approve, etc. están sobrescritas para devolver false
        	- delegate está sobrescrita para devolver un error

        - `REGVotingPowerRegistryErrors.DelegateToOtherNotAllowed()` si un usuario intenta delegar su voto a otro usuario.


#### Guías de interacción con los contratos:

- Métodos principales:
  - `propose()` en REGGovernor para crear una propuesta,
  - `castVote()` en REGGovernor para votar en una propuesta,
  - `queue()` en REGGovernor para poner en cola una propuesta,
  - `execute()` en REGTreasuryDAO para ejecutar una propuesta aprobada.  

- Eventos importantes:
  - `ProposalCreated` emitido cuando se crea una propuesta,
  - `VoteCast` emitido cuando se registra un voto,
  - `callExecuted()` emitido cuando se ejecuta una acción de la propuesta.  


#### Diagramas de arquitectura:

\[_Un diagrama mostrando las interacciones entre los contratos_\]
xxxxxx

#### Informes de auditoría de seguridad:

No hay informes de auditoría de seguridad por el momento. Dado que los contratos están cerca de las versiones estándar (ya auditadas) y la gobernanza se ejerce con restricciones de derechos, se consideró más pertinente conservar los recursos para futuras auditorías de la v2. La v1 es una versión provisional experimental destinada a inicializar la DAO y, por lo tanto, el campo de acción es limitado.

### 7.1.2. Mecanismos de gobernanza ⭐⭐

Esta sección detalla los procesos de gobernanza de la DAO:

- Ciclo de vida de las propuestas:
  - Discusión y redacción de la propuesta en el foro de gobernanza,
  - Creación y presentación,
  - Período de votación,
  - Ejecución y timelock.
- Cálculo del poder de voto:
  - Fórmulas utilizadas off-chain por RealT,
  - Factores que influyen en el poder de voto (bloqueo, duración de la tenencia, uso).
- Sistema de incentivos:
  - Mecanismo de distribución de recompensas
  - Cálculo de bonificaciones basado en la participación en las votaciones y bloqueo de REG

### 7.1.3. Interfaces de usuario ⭐

Las interacciones se realizan principalmente con las siguientes interfaces:

- Discusiones de Gobernanza ([URL](https://forum.realtoken.community)):
  - Foro de gobernanza,
  - Alojado y gestionado por la comunidad,
  - Permite discutir ideas, determinar el interés, las prioridades y preparar las propuestas.
- Interfaz de votación ([URL](https://www.tally.xyz/gov/realtoken-ecosystem-governance)):
  - Uso de la plataforma Tally como interfaz de votación,
  - Acceso a las funciones principales de votación, visualización, resultado, voto, creación de propuestas, estado de las propuestas,
  - El uso de Tally permite ganar varios meses de desarrollo,
  - Implica algunas restricciones en el funcionamiento y la información mostrada.
- Interfaz de incentivos ([URL](https://vote.realtoken.network)):
  - Interfaz dedicada creada por RealT,
  - Permite depositar REG, retirar REG, reclamar recompensas.

## 7.2. Canales de comunicación

### 7.2.1. Canales oficiales ⭐

- Sitio web oficial de la comunidad: [realtoken.community](https://www.realtoken.community/)
- Foro de gobernanza: [forum.realtoken.community](https://forum.realtoken.community)
- Twitter: @RealTokenDAO

### 7.2.2. Recursos para desarrolladores ⭐⭐⭐

- GitHub: Repositorio de contratos inteligentes → [Governance, incentive, treasury](https://github.com/real-token/reg-governance-core)
- GitHub: interfaces de incentivos → [github.com/real-token/voting-front/tree/incentive](https://github.com/real-token/voting-front/tree/incentive)

[Página siguiente](/es/DAO/FAQ)

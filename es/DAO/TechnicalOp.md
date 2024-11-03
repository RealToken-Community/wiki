---
title: 4. Operación técnica del DAO REG
description: 
published: true
date: 2024-11-03T06:57:03.025Z
tags: 
editor: markdown
dateCreated: 2024-10-10T16:46:13.369Z
---

## **4.1. Contratos inteligentes principales**

#### **Direcciones de los contratos inteligentes:**

- [Governor: 0x4a5327347f077e72d2aab19f68ba8a7f12ec5d63](https://gnosisscan.io/address/0x4a5327347f077e72d2aab19f68ba8a7f12ec5d63#code)
- [REGTreasuryDAO: 0x3f2d192F64020dA31D44289d62DB82adE6ABee6c](https://gnosisscan.io/address/0x3f2d192F64020dA31D44289d62DB82adE6ABee6c#code)
- [PowerVotingRegistry: 0x6382856a731Af535CA6aea8D364FCE67457da438](https://gnosisscan.io/address/0x6382856a731Af535CA6aea8D364FCE67457da438#code)
- [Incentive: 0xe1877d33471e37fe0f62d20e60c469eff83fb4a0](https://gnosisscan.io/address/0xe1877d33471e37fe0f62d20e60c469eff83fb4a0#code)

#### Ejemplos de interacciones entre contratos inteligentes
![reg-vote-bonus.drawio.svg](/assets/img/reg-vote-bonus.drawio.svg)

#### **⭐ Para principiantes**

La DAO RealToken funciona gracias a programas informáticos especiales llamados "contratos inteligentes". Estos contratos gestionan automáticamente las reglas de la DAO, las votaciones y las recompensas. Hay cuatro contratos principales:

1.  El contrato de gobernanza: Es el cerebro de la DAO. Gestiona las propuestas y las votaciones,
2.  El contrato de tesorería: Gestiona los fondos de la DAO y aplica un plazo de seguridad antes de ejecutar las decisiones,
3.  El contrato de poder de voto: Calcula el poder de voto de cada miembro,
4.  El contrato de incentivos: Distribuye recompensas a los miembros activos de la DAO que votan y bloquean sus REG (si la DAO ha activado un período).

#### **⭐⭐ Para iniciados**

La DAO RealToken se basa en un conjunto de contratos inteligentes interconectados:

1.  Contrato de gobernanza: Basado en el estándar OpenZeppelin Governor, con modificaciones menores, gestiona el ciclo de vida de las propuestas, el proceso de votación y la ejecución de decisiones on-chain,
2.  Contrato de tesorería (REGTreasuryDAO): Implementa un mecanismo de timelock para asegurar la ejecución de las decisiones de la DAO y gestionar sus fondos,
3.  Contrato de poder de voto (PowerVotingRegistry): Registra el poder de voto de los poseedores de REG en función de su saldo, impulsos y actividad, permitiendo así una mayor flexibilidad que el modelo clásico de voto por token, 1 token = 1 voto,
4.  Contrato de incentivos: Gestiona la distribución de recompensas para fomentar la participación activa en la gobernanza, para ello hay que votar y bloquear los REG durante un período determinado (época).

Estos contratos interactúan para asegurar un funcionamiento transparente y seguro de la DAO, a la vez que fomentan la participación activa en la gobernanza.

#### **⭐⭐⭐ Para expertos**

La arquitectura técnica de la DAO RealToken está compuesta por varios contratos inteligentes clave:

##### **4.1.1. Contrato de gobernanza**

- Basado en OpenZeppelin Governor con modificaciones personalizadas,
- Gestiona el ciclo de vida completo de las propuestas: creación, votación y ejecución,
- Integra las interacciones con el timelock para aumentar la seguridad de las ejecuciones,
- Implementa controles de acceso para la creación de propuestas, permite la activación de 4 modos diferentes para autorizar la creación de propuestas,
- Interactúa directamente con el PowerVotingRegistry para crear los snapshots de los poderes de voto basados en los valores registrados en el PowerVotingRegistry.

##### **4.1.2. Contrato de tesorería (REGTreasuryDAO)**

- Basado en el TimelockControllerUpgradeable de OpenZeppelin,
- Implementa un mecanismo de timelock para añadir un retraso de seguridad antes de la ejecución de las decisiones on-chain,
- Gestiona los fondos de la DAO y controla su uso,
- Integra roles específicos (UPGRADER_ROLE) para la gestión de las actualizaciones del contrato,
- Utiliza el patrón UUPS (Universal Upgradeable Proxy Standard) para permitir futuras actualizaciones,
- Interactúa con los contratos externos llamados por las propuestas.

##### **4.1.3. Contrato de poder de voto (PowerVotingRegistry)**

- Registra el poder de voto en función del saldo REG y otros factores (por ejemplo, duración de la tenencia, bloqueo, pool de liquidez, etc.),
- Implementa una lógica de auto-delegación para simplificar la activación del poder de voto.

##### **4.1.4. Contrato de incentivos**

- Integra una lógica de estado que modifica las acciones posibles, el ciclo completo se llama "época", para cada época hay que votar y bloquear los REG para recibir recompensas,
- Distribuye recompensas a los participantes activos de la gobernanza (que votan),
- Utiliza mecanismos de bloqueo para fomentar el compromiso a largo plazo (bloqueo de la duración de la época),
- Integra cálculos de recompensas basados en la actividad de gobernanza (votos y bloqueo de REG).

Estos contratos están diseñados para ser modulares y evolutivos, permitiendo actualizaciones y mejoras futuras sin perturbar todo el sistema de gobernanza. El contrato REGTreasuryDAO, en particular, añade una capa de seguridad adicional al imponer un retraso entre la propuesta de una acción y su ejecución, permitiendo así a la comunidad reaccionar si es necesario.

A largo plazo, todos los contratos del ecosistema RealToken serán controlados y actualizables únicamente por el contrato REGTreasuryDAO a través de propuestas, solo algunas funciones de emergencia podrán ser ejecutadas fuera del ciclo de la DAO por un comité de seguridad que tendrá derechos limitados a acciones de emergencia para asegurar los contratos.

## **4.2. Mecanismos de votación y propuesta**

#### **⭐ Para principiantes**

La DAO RealToken te permite participar en las decisiones importantes del ecosistema de aplicaciones vinculado a la DAO. Así es como funciona:

1.  En el foro de la DAO, los miembros pueden proponer ideas para mejorar la DAO y el ecosistema,
2.  Se lleva a cabo un debate en torno a las propuestas para medir la viabilidad y el interés de las propuestas por parte de la comunidad,
3.  Si una propuesta obtiene un número suficiente de votos, se transforma en una propuesta formal y se somete a votación de la DAO,
4.  Los miembros de la DAO pueden votar a favor o en contra de la propuesta,
5.  Si la propuesta es aprobada, es ejecutada por el REGTreasuryDAO.

#### **⭐⭐ Para iniciados**

El sistema de votación y propuesta de la DAO RealToken incluye:

1.  El foro de la DAO: Donde los miembros pueden proponer ideas y debatir para medir el interés de las propuestas,
2.  Durante el período de debate, la comunidad debe medir el interés y la viabilidad de las propuestas,
3.  Si una propuesta se considera viable y relevante, se trabaja en una propuesta concreta estableciendo todos los parámetros relacionados con la propuesta, como las acciones a ejecutar on-chain, el costo de desarrollo, los beneficios para la DAO, etc.,
4.  Creación de propuestas: Proceso controlado por reglas de acceso definidas en el contrato de gobernanza, según los parámetros puede haber restricciones en la creación de propuestas, esto tiene como objetivo evitar ataques de gobernanza y permitir una implementación progresiva de las reglas de funcionamiento de la DAO con total seguridad,
5.  Ciclo de vida de las propuestas: Creación, período de votación, ejecución (si es aprobada),
6.  Mecanismo de timelock: Retraso de seguridad antes de la ejecución de las decisiones aprobadas.

#### **⭐⭐⭐ Para expertos**

Los mecanismos de votación y propuesta se implementan de la siguiente manera:

1.  Foro de la DAO:
    - Donde los usuarios pueden proponer ideas y debatir para medir el interés de las propuestas,
    - Se lleva a cabo un debate en torno a las propuestas para medir la viabilidad y el interés de las propuestas por parte de la comunidad o uno de los proveedores de la DAO,
    - Si una propuesta se considera viable y relevante, se trabaja en una propuesta concreta estableciendo todos los parámetros relacionados con la propuesta, como las acciones a ejecutar on-chain, el costo de desarrollo, los beneficios para la DAO, estudio de impacto y riesgo, la necesidad o no de una auditoría de seguridad, etc.
2.  Creación de propuestas:
    - Controladas por el contrato de gobernanza,
    - Cuatro modos de autorización son posibles para la creación de propuestas, estos modos de propuesta tienen como objetivo optimizar la gestión de riesgos de la DAO y limitar la exposición de los fondos, con un objetivo de descentralización a medio plazo en función de la madurez de la DAO,
    - Inscripción on-chain de la propuesta con todos los códigos que permiten la ejecución de la propuesta si es aprobada,
    - Interacción con el PowerVotingRegistry para crear snapshots de los poderes de voto.
3.  Proceso de votación:
    - Basado en el estándar OpenZeppelin Governor con modificaciones personalizadas,
    - Utilización del poder de voto registrado en el PowerVotingRegistry,
    - Posibilidad de voto: a favor, en contra o abstención,
    - Solo los miembros registrados en el PowerVotingRegistry pueden votar,
    - Se aplica un plazo de 1 día entre el inicio de la votación y la validación de la transacción de creación de la propuesta (por seguridad, para una posible cancelación),
    - La votación se realiza durante un período determinado por el contrato de gobernanza, generalmente 7 días,
    - Los votantes verifican que la propuesta sea conforme a la descripción y a los intercambios en el foro de la DAO (verificación del código de ejecución on-chain).
4.  Cálculo del poder de voto
    - Registrado en el PowerVotingRegistry,
    - Tiene en cuenta el saldo REG, la duración de la tenencia, el bloqueo y potencialmente otros factores, según el algoritmo de cálculo validado por la DAO,
    - Implementa una lógica de auto-delegación para simplificar la activación del poder de voto.
5.  Ejecución de las propuestas:
    - Utilización del REGTreasuryDAO como timelock para añadir un retraso de seguridad,
    - Ejecución on-chain de las acciones aprobadas después del retraso,
    - Posibilidad de cancelación durante el período de timelock si es necesario, por un grupo de seguridad.
6.  Sistema de incentivos:
    - Gestionado por el contrato de incentivos,
    - Recompensas basadas en la participación en las votaciones y el bloqueo de REG,
    - Sistema de épocas para estructurar los períodos de recompensas,
    - Activado por la DAO que define la duración de las épocas y las recompensas asociadas.
7.  Modo de propuesta:
    - ProposerWithRole: El objetivo es limitar estrictamente las direcciones que pueden presentar una votación, para prevenir el spam y las propuestas abusivas. Este modo se activará únicamente durante las primeras semanas o meses del despliegue inicial de la gobernanza (versión 1), para establecer una base sólida antes de ampliar el número de autores de propuestas,
    - ProposerWithVotingPower: Este modo exige que el autor de la propuesta tenga un poder de voto mínimo. Sin este poder, no se puede crear ninguna propuesta. Es la etapa final de la descentralización, donde todos aquellos con suficiente poder de voto pueden presentar propuestas. Ya no hay roles privilegiados; todos los poseedores siguen las mismas reglas,
    - ProposerWithRoleAndVotingPower: Este modo combina dos restricciones, el autor de la propuesta debe tener un rol específico y un poder de voto mínimo. Si una de las condiciones no se cumple, la propuesta fallará. Este modo amplía las propuestas a los miembros más comprometidos de la comunidad, exigiendo un saldo mínimo de REG para garantizar su compromiso e intereses financieros. Deben ser elegidos y poseer suficientes REG. Esto permite iniciar la descentralización, probar los procesos y limitar los riesgos,
    - ProposerWithRoleOrVotingPower: Este modo permite la creación de propuestas si el autor de la propuesta posee el rol requerido o un poder de voto mínimo. Si ninguna de estas condiciones se cumple, la propuesta fallará. Es la penúltima etapa de la descentralización de la gobernanza. Este modo exige un número importante de REG para proponer libremente mejoras, sin pasar por un rol específico. Esto permite a cualquiera proponer mejoras comprometiendo un número significativo de REG, manteniendo a los autores de propuestas que tienen un rol específico, que no necesitan un mínimo de REG. El rol puede ser retirado en caso de incumplimiento de los procesos.
8.  Delegación:
    - La delegación de poder de voto ha sido retirada para permitir la activación de incentivos para los votantes y los que bloquean REG,
    - Los usuarios no pueden delegar su poder de voto a otras direcciones, pero se benefician de una auto-delegación por defecto,
    - En una v2, la delegación volverá e incluso será una parte importante del sistema.

Estos mecanismos están diseñados para asegurar una gobernanza equitativa, segura e incentivadora, permitiendo al mismo tiempo una evolución futura del sistema.

En las primeras semanas/meses, la DAO se implementa con restricciones para permitir el establecimiento de las reglas de funcionamiento, experimentar los procesos y limitar los riesgos.

Para dar una buena visión de las etapas de evolución, se deberá llevar a cabo un debate para determinar los umbrales que desencadenan una descentralización de nivel superior, dando una mayor autonomía y liberándose de la confianza aportada a los diversos intervinientes.

## **4.3. Sistema de incentivos y recompensas**

#### **⭐ Para principiantes**

La DAO RealToken te recompensa por tu participación activa. Así es como funciona:

1.  Ganas recompensas en diversas formas al votar sobre las propuestas,
2.  Puedes aumentar tus recompensas bloqueando tus tokens REG durante un cierto período,
3.  La participación en ciertos pools de liquidez u otros del ecosistema RealToken puede darte recompensas adicionales,
4.  Las recompensas pueden ser en forma de REG, ERC20, o incluso en forma de impulso sobre el poder de voto,
5.  La DAO decide las recompensas y los parámetros del sistema de incentivos.

#### **⭐⭐ Para iniciados**

El sistema de incentivos de la DAO RealToken está diseñado para fomentar la participación activa:

1.  La DAO define las recompensas y los parámetros del sistema de incentivos, el tipo y los montos de las recompensas,
2.  Recompensas basadas en la participación en las votaciones y el bloqueo de REG, la participación en ciertos pools de liquidez u otros del ecosistema RealToken puede darte recompensas adicionales,
3.  Sistema de épocas para estructurar los períodos de recompensas y optimizar la participación,
4.  Atribución on-chain automática de las recompensas en stablecoin reclamable al final de cada época,
5.  Posibilidad para la DAO de ajustar los parámetros del sistema de incentivos para optimizar la participación y responder a la evolución de la DAO.

#### **⭐⭐⭐ Para expertos**

El sistema de incentivos y recompensas se implementa de la siguiente manera:

1.  Contrato de incentivos:
    - Gestiona el estado del sistema (activo/inactivo) y los ciclos de épocas,
    - Calcula las recompensas en función de la actividad de voto y el bloqueo de REG,
    - Asigna automáticamente las recompensas al final de cada época.
2.  Mecanismo de bloqueo:
    - Los usuarios pueden bloquear sus REG por la duración de una época en el contrato de incentivos,
    - El bloqueo puede aumentar el poder de voto y las recompensas potenciales según los parámetros definidos por la DAO,
    - El bloqueo permite marcar un compromiso a largo plazo por parte del votante, disminuye la liquidez de los REG y limita los riesgos de ataque de gobernanza.
3.  Parámetros ajustables:
    - Duración de las épocas,
    - Cantidad de recompensas por época,
    - Tipo de recompensas,
    - Estos parámetros son aplicados por la DAO en la propuesta de activación de una época,
    - Para los impulsos de poder de voto, la DAO puede ajustar según las necesidades: la fórmula y el peso acordado en el sistema de cálculo del powerVoting que genera los poderes de voto.
4.  Integración con el PowerVotingRegistry:
    - El bloqueo de los REG afecta al poder de voto registrado,
    - Sinergia entre la participación en la gobernanza y las recompensas.
5.  Seguridad y evolutividad:
    - Contrato actualizable para permitir mejoras futuras,
    - Mecanismos de pausa en caso de emergencia.
6.  Cálculo de las recompensas:
    - Contabiliza el número total de propuestas emitidas durante una época,
    - Contabiliza el número de votos efectuados por cada votante durante la época,
    - Ponderado por la cantidad de REG bloqueados.

```
userBonus = (userState.depositAmount * userState.voteAmount * epochState.totalBonus) / epochState.totalWeights;
```

- `userState.depositAmount`: El monto depositado por el usuario para esta época en REG,
- `userState.voteAmount`: El número de votos efectuados por el usuario durante esta época,
- `epochState.totalBonus`: El monto total de bonus asignados para esta época,
- `epochState.totalWeights`: La suma total de los pesos para todos los usuarios en esta época.

Esta fórmula se aplica en el contrato en las siguientes líneas:

[REGIncentiveVault.sol](https://gnosisscan.io/address/0x4b79755d1ea8937c027408e3aa72d69a260f6237#code#F1#L342) de la línea 342 a 346.

El total de los pesos se incrementa del monto depositado por el usuario en cada uno de sus votos (cf línea 245 del contrato).

Este sistema busca crear un círculo virtuoso de compromiso en la gobernanza, recompensando la participación activa al tiempo que refuerza la estabilidad a largo plazo del ecosistema RealToken.

La mecánica de recompensa será completamente revisada con la puesta en servicio de la gobernanza V2 utilizando los NFT para mejorar el sistema de incentivos, permitir una mayor granularidad y precisión de las acciones a fomentar en función de las necesidades de la DAO y los perfiles de los holders.

Se introducirán nuevos tipos de recompensas como la materia oscura de los NFT Cityzen, puntos de apoyo para los votos permitiendo destacar los contenidos relacionados con el NFT Activity, etc.

El ecosistema RealToken está pensado para disponer de numerosas herramientas de optimización de participación en las votaciones, y fomentar las acciones y contribuciones diversas que son beneficiosas para la DAO, al tiempo que permite un control preciso de las finanzas de la DAO para no dilapidar la tesorería o devaluar el token REG.


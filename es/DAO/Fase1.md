## **3.1. Explicación de la versión simplificada de la DAO**

#### **⭐ Para principiantes**

La Fase 1 de la DAO RealToken es una versión simplificada del sistema final. Es como un período de prueba donde establecemos las bases de la DAO. Durante esta fase, aprendemos cómo funciona la gobernanza y comenzamos a tomar decisiones juntos, pero de una manera más simple que en el futuro. El poder de la DAO está limitado por el momento para reducir los riesgos y permitir un aprendizaje gradual, el alcance del poder de decisión aumentará con el tiempo.

#### **⭐⭐ Para iniciados**

La Fase 1 es una etapa crucial en el despliegue de la DAO RealToken. Su objetivo es:

1.  Establecer una estructura de gobernanza básica,
2.  Probar los mecanismos de votación y propuesta,
3.  Identificar las necesidades y desafíos de la comunidad,
4.  Comenzar a tomar decisiones colectivas sobre temas simples,
5.  Preparar el terreno para funcionalidades más avanzadas en fases futuras,
6.  Establecer las bases de funcionamiento de la DAO,
7.  Educar a la comunidad sobre los mecanismos de la DAO,
8.  Experimentar con diversas funciones y enfoques de gobernanza,
9.  Demostrar el potencial futuro de la DAO y del REG.

Esta fase utiliza un conjunto limitado de contratos inteligentes y funcionalidades para facilitar la adopción y el aprendizaje. La elección tecnológica se hizo para que esta primera fase permita un despliegue rápido y simple con un costo de desarrollo limitado. Esta fase permitirá acumular conocimiento y establecer los principios básicos fundamentales para el futuro de la DAO y la nueva versión en la fase 2 con la llegada de NFT.

#### **⭐⭐⭐ Para expertos**

La Fase 1 de la DAO RealToken está diseñada como una implementación minimalista pero funcional, con las siguientes características:

1.  Gobernanza simplificada:
    - Mecanismo de votación básico tomado del estándar OpenZeppelin governor,
    - Proceso de propuesta evolutivo y menos rígido para permitir una mayor participación y flexibilidad de exploración,
    - Quórum y umbrales de votación ajustados regularmente para adaptarse a la participación,
    - Sin delegación de poder de voto,
    - Limitación en el acceso a la creación de propuestas para limitar el riesgo de creación de propuestas maliciosas.
2.  Contratos inteligentes básicos:
    - Contrato de gobernanza, con funciones de votación y propuesta, tomado de los estándares de Open Zeppelin,
    - Contrato de tesorería simple para la gestión de los fondos de la DAO,
    - Contrato de incentivos para la gestión de recompensas incentivas al voto,
    - Contrato PowerVotingRegistry, que permite registrar el poder de voto de cada titular de REG.
3.  Integración limitada con el ecosistema:
    - Interacciones limitadas con las DApps existentes (RealT sigue siendo el tercero de confianza para las DApps el tiempo necesario para el aumento de competencia de la DAO),
    - Mecanismos de incentivos simples para fomentar la participación.
4.  Mecanismos de seguridad:
    - Timelock en las ejecuciones de propuestas, dando un tiempo adicional para detectar posibles problemas o errores de una propuesta,
    - Límites en las cantidades de tesorería gestionables,
    - Derechos de veto por parte de RealT sobre propuestas extremas o que pongan en peligro la DAO.
5.  Recopilación de datos:
    - Métricas sobre la participación y el compromiso de los miembros,
    - Feedback continuo de la comunidad para informar sobre las fases futuras.

Esta fase servirá de base para la evolución futura de la DAO, permitiendo identificar los ajustes necesarios y las funcionalidades a desarrollar para las fases siguientes. Al utilizar esta fase como un laboratorio, podremos experimentar diversos enfoques de gobernanza y recopilación de datos antes de integrarlos en la próxima versión de la DAO con la llegada de NFT. Aunque pilotada por el socio de confianza RealT, la DAO seguirá siendo independiente y las decisiones siempre serán tomadas por los miembros de la DAO en el espacio del campo de aplicación progresivamente ampliado. Varios puntos serán, por lo tanto, objeto de debate entre los titulares de REG y RealT para establecer las etapas de ampliación del campo de aplicación de la DAO con el fin de garantizar la estabilidad y la seguridad del ecosistema.

## **3.2. Proceso de implementación**

#### **⭐ Para principiantes**

La implementación de la DAO se hace paso a paso. Primero, creamos las herramientas básicas para votar y hacer propuestas. Luego, comenzaremos a usarlas para tomar decisiones simples. A medida que avancemos, aprenderemos y mejoraremos el sistema juntos.

#### **⭐⭐ Para iniciados**

El proceso de implementación de la Fase 1 incluye varios pasos clave:

1.  Despliegue de los contratos inteligentes básicos (ya realizado),
2.  Creación de una interfaz de usuario simple para votar y proponer (Uso de un sitio ya existente: Tally)
3.  Formación de la comunidad sobre el uso de las herramientas de gobernanza (Wiki),
4.  Presentación de los primeros proyectos sobre los que la DAO podrá votar (RealT ya ha preparado una serie de propuestas),
5.  Implementación del sistema de discusiones y debate para las propuestas (Foro),
6.  Lanzamiento de las primeras propuestas (octubre),
7.  Lanzamiento de la primera campaña de incentivos al voto (octubre-noviembre),
8.  Ajuste de los parámetros de gobernanza en función de los comentarios,
9.  Ampliación gradual del campo de acción de la DAO.

#### **⭐⭐⭐ Para expertos**

El proceso de implementación de la Fase 1 está estructurado para maximizar el aprendizaje y la adaptación:

1.  Infraestructura técnica:
    - Despliegue de los contratos inteligentes (gobernanza, tesorería, incentivos, PowerVotingRegistry),
    - Pruebas de red en grupo restringido (ya realizado),
    - Auditorías de seguridad y pruebas exhaustivas (realizado internamente por RealT),
    - Prueba de red pública (ya realizado),
    - Desarrollo de herramientas para el cálculo del powerVoting (Realizado por RealT).
2.  Gobernanza inicial:
    - Definición de los parámetros iniciales (quórum, umbral de voto, timelock: RealT)
    - Implementación de un proceso de propuesta en varias etapas (discusión, formalización, voto),
    - Implementación de mecanismos de seguridad, incluyendo el derecho de veto de RealT,
    - Limitación del acceso a la creación de propuestas.
3.  Compromiso comunitario:
    - Campaña de educación sobre los mecanismos de la DAO,
    - Creación de canales de comunicación dedicados para discusiones y feedback,
    - Organización de eventos de gobernanza para estimular la participación,
    - Programa de incentivos.
4.  Iteración y optimización:
    - Recopilación y análisis continuos de las métricas de participación,
    - Ajustes regulares de los parámetros de gobernanza,
    - Experimentación con diferentes enfoques de gobernanza,
    - Debate sobre los diversos cambios y enfoques de gobernanza.
5.  Expansión progresiva:
    - Definición de un plan de ampliación del campo de acción de la DAO,
    - Negociaciones con RealT para la transferencia progresiva de responsabilidades,
    - Preparación de la transición hacia la Fase 2 con la integración de los NFT.

Este proceso está diseñado para ser flexible y adaptable, permitiendo a la comunidad influir activamente en la evolución de la DAO mientras se mantiene la estabilidad y la seguridad del ecosistema. Esto también da la oportunidad a los titulares de REG de comprender y familiarizarse con los mecanismos de la DAO, crear y descubrir vocaciones, hacer surgir ideas y nuevos casos de uso. La implementación progresiva de la DAO permite así una transición natural y una evolución progresiva hacia la descentralización del ecosistema.

## **3.3. Objetivos a corto y medio plazo**

#### **⭐ Para principiantes**

A corto plazo, queremos que todos entiendan cómo funciona la DAO y comiencen a participar según su comprensión y competencia. A medio plazo, queremos tomar decisiones más importantes juntos y descentralizar la gobernanza de los servicios propuestos por la DAO.

#### **⭐⭐ Para iniciados**

Objetivos a corto plazo:

1.  Alcanzar una alta tasa de participación en las votaciones,
2.  Educar a la comunidad sobre los mecanismos de la DAO (seguridad, equilibrio presupuestario, etc.),
3.  Identificar los primeros proyectos a financiar o desarrollar,
4.  Probar y ajustar los parámetros de gobernanza,
5.  Establecimiento de los procesos básicos para la gobernanza.

Objetivos a medio plazo:

1.  Ampliar progresivamente el campo de acción de la DAO,
2.  Desarrollar nuevos casos de uso para el REG,
3.  Mejorar las herramientas de gobernanza basadas en el feedback,
4.  Preparar la transición hacia la Fase 2 con la integración de los NFT.

#### **⭐⭐⭐ Para expertos**

Objetivos a corto plazo (3-6 meses):

1.  Gobernanza:
    - Alcanzar un quórum estable del 50% en las votaciones,
    - Implementar y probar diferentes procesos de propuesta,
    - Establecer un proceso eficaz de debate y refinamiento de propuestas,
    - Elegir a los primeros miembros de la comunidad que puedan crear propuestas,
    - Definir el marco de una propuesta válida para someterla a votación.
2.  Compromiso comunitario:
    - Lanzar campañas de incentivos al voto con recompensas,
    - Organizar eventos educativos regulares sobre la gobernanza DAO,
    - Crear un programa de mentoría para los nuevos miembros activos,
    - Fomentar iniciativas beneficiosas para la DAO y el ecosistema RealToken.
3.  Desarrollo técnico:
    - Desarrollar/perfeccionar las herramientas de análisis para seguir la salud de la DAO,
    - Integrar funcionalidades básicas con el ecosistema existente.

Objetivos a medio plazo (6-18 meses):

1.  Expansión de la DAO:
    - Negociar e implementar un plan de ampliación del campo de acción con RealT,
    - Desarrollar mecanismos de gobernanza más avanzados,
    - Crear comités especializados para diferentes aspectos del ecosistema,
    - Implementación más amplia del acceso a la creación de propuestas.
2.  Innovación y desarrollo:
    - Lanzar iniciativas de investigación y desarrollo dirigidas por la comunidad,
    - Experimentar con nuevos modelos de incentivos y participación,
    - Desarrollar integraciones más profundas con el ecosistema,
    - Inicios de la fase 2 con la integración de los NFT.
3.  Preparación de la Fase 2:
    - Diseñar y probar los mecanismos de integración de los NFT en la gobernanza,
    - Desarrollar una hoja de ruta detallada para la transición hacia la Fase 2,
    - Formar grupos de trabajo para cada aspecto importante de la nueva fase,
    - Realizar pruebas exhaustivas con la comunidad para la fase 2.
4.  Ecosistema y asociaciones:
    - Establecer asociaciones estratégicas con otros proyectos DeFi y DAO,
    - Desarrollar casos de uso innovadores para el REG más allá de la gobernanza,
    - Integración de los primeros proveedores distintos de RealT.

Estos objetivos tienen como fin establecer una base sólida para la DAO, al tiempo que preparan el terreno para una expansión e innovación continuas en el ecosistema RealToken.

## **3.4. Cómo participar en la fase 1**

#### **⭐ Para principiantes**

Para participar en la fase inicial de la DAO RealToken:

1.  Siga los anuncios oficiales en los canales de comunicación de RealToken,
2.  Dé su opinión en las discusiones de la comunidad en el foro de gobernanza,
3.  Posea tokens REG para tener poder de voto,
4.  Participe en las votaciones cuando estén abiertas,
5.  Asista a los eventos educativos para aprender más sobre la DAO,
6.  Infórmese y fórmese sobre el tema de las DAO.

#### **⭐⭐ Para iniciados**

Aquí está cómo puede participar activamente en la fase inicial:

1.  Proponga ideas de mejora en los foros de gobernanza,
2.  Participe en los debates sobre las propuestas antes de las votaciones en el foro de gobernanza,
3.  Vote regularmente sobre las propuestas,
4.  Ayude a educar a los nuevos miembros sobre el funcionamiento de la DAO,
5.  Participe en las campañas de incentivos para ganar recompensas,
6.  Pruebe las nuevas funcionalidades y dé su feedback,
7.  Contribuya a la elaboración de los procesos de gobernanza,
8.  Tome iniciativas y sométalas a la comunidad,
9.  Redacte, mejore, traduzca y comparta contenido sobre la DAO.

#### **⭐⭐⭐ Para expertos**

Para una participación profunda en la fase inicial:

1.  Gobernanza:
    - Analice en profundidad las propuestas y comparta sus análisis,
    - Contribuya activamente a la elaboración de métricas para evaluar la salud de la DAO,
    - Proponga ajustes de los parámetros de gobernanza, basados en datos y argumente con una visión a corto, medio y largo plazo.
2.  Desarrollo técnico:
    - Participe activamente en las Testnets,
    - Proponga mejoras técnicas para las herramientas de gobernanza,
    - Contribuya al desarrollo de herramientas de análisis para la DAO,
    - Revise y audite continuamente los contratos inteligentes, manténgase informado de los hackeos y vulnerabilidades potenciales.
3.  Compromiso comunitario:
    - Organice sesiones educativas para la comunidad,
    - Cree contenido explicativo sobre el funcionamiento de la DAO,
    - Participe activamente en los programas de mentoría,
    - Explore nuevos conceptos de Live, video, artículo, etc.
4.  Innovación:
    - Proponga nuevos casos de uso argumentados para el REG,
    - Participe en los grupos de trabajo sobre la futura integración de los NFT,
    - Explore sinergias potenciales con otros proyectos DeFi.
5.  Preparación de la Fase 2:
    - Contribuya al diseño de los mecanismos de integración de los NFT,
    - Participe en las pruebas exhaustivas de las nuevas funcionalidades,
    - Ayude a elaborar la hoja de ruta para la transición hacia la Fase 2.

Su participación activa en todos estos niveles contribuirá a dar forma al futuro de la DAO RealToken y a asegurar su éxito a largo plazo.

[Página siguiente](/es/DAO/Funcionamiento)

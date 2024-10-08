---
title: 3. Fase 1, versión simplificada
description: 
published: true
date: 2024-10-08T14:06:11.229Z
tags: 
editor: markdown
dateCreated: 2024-10-08T08:37:07.279Z
---

## **3.1. Explicación de la versión simplificada**

#### **⭐ Para principiantes**

La Fase 1 de la DAO RealToken es una versión simplificada del sistema final. Es como un período de prueba donde establecemos las bases de la DAO. Durante esta fase, aprendemos cómo funciona la gobernanza y comenzamos a tomar decisiones juntos, pero de una manera más simple que en el futuro. El poder de la DAO está limitado por el momento para reducir los riesgos y permitir un aprendizaje gradual, el alcance del poder de decisión aumentará con el tiempo.

#### **⭐⭐ Para iniciados**

La Fase 1 es una etapa crucial en el despliegue de la DAO RealToken. Su objetivo es:

1. Establecer una estructura de gobernanza básica,
2. Probar los mecanismos de votación y propuesta,
3. Identificar las necesidades y desafíos de la comunidad,
4. Comenzar a tomar decisiones colectivas sobre temas simples,
5. Preparar el terreno para funcionalidades más avanzadas en fases futuras,
6. Establecer las bases de funcionamiento de la DAO,
7. Educar a la comunidad sobre los mecanismos de la DAO,
8. Experimentar con diversas funciones y enfoques de gobernanza,
9. Demostrar el potencial futuro de la DAO y del REG.

Esta fase utiliza un conjunto limitado de contratos inteligentes y funcionalidades para facilitar la adopción y el aprendizaje. La elección tecnológica se hizo para que esta primera fase permita un despliegue rápido y simple con un costo de desarrollo limitado. Esta fase permitirá, por lo tanto, acumular conocimiento y establecer los principios básicos fundamentales para el futuro de la DAO y la nueva versión en la fase 2 con la llegada de NFT.

#### **⭐⭐⭐ Para expertos**

La Fase 1 de la DAO RealToken está diseñada como una implementación minimalista pero funcional, con las siguientes características:

1. Gobernanza simplificada:
   - Mecanismo de votación básico tomado del estándar OpenZeppelin governor,
   - Proceso de propuesta evolutivo y menos rígido para permitir una mayor participación y flexibilidad de exploración,
   - Quórum y umbrales de votación ajustados regularmente para adaptarse a la participación,
   - Sin delegación de poder de voto,
   - Limitación en el acceso a la creación de propuestas para limitar el riesgo de creación de propuestas maliciosas.
2. Contratos inteligentes básicos:
   - Contrato de gobernanza, con funciones de votación y propuesta, tomado de los estándares de Open Zeppelin,
   - Contrato de tesorería simple para la gestión de los fondos de la DAO,
   - Contrato de incentivos para la gestión de recompensas incentivas al voto,
   - Contrato PowerVotingRegistry, que permite registrar el poder de voto de cada titular de REG.
3. Integración limitada con el ecosistema:
   - Interacciones limitadas con las DApps existentes (RealT sigue siendo el tercero de confianza para las DApps el tiempo necesario para el aumento de competencia de la DAO),
   - Mecanismos de incentivos simples para fomentar la participación.
4. Mecanismos de seguridad:
   - Timelock en las ejecuciones de propuestas, dando un tiempo adicional para detectar posibles problemas o errores de una propuesta,
   - Límites en las cantidades de tesorería gestionables,
   - Derechos de veto por parte de RealT sobre propuestas extremas o que pongan en peligro la DAO.
5. Recopilación de datos:
   - Métricas sobre la participación y el compromiso de los miembros,
   - Retroalimentación continua de la comunidad para informar las fases futuras.

Esta fase servirá de base para la evolución futura de la DAO, permitiendo identificar los ajustes necesarios y las funcionalidades a desarrollar para las fases siguientes. Utilizando esta fase como un laboratorio, podremos experimentar diversos enfoques de gobernanza y recopilación de datos antes de integrarlos en la próxima versión de la DAO con la llegada de NFT. Aunque pilotada por el socio de confianza RealT, la DAO permanecerá independiente y las decisiones siempre serán tomadas por los miembros de la DAO en el espacio del campo de aplicación progresivamente ampliado. Varios puntos serán, por lo tanto, debatidos entre los titulares de REG y RealT para establecer las etapas de ampliación del campo de aplicación de la DAO con el fin de garantizar la estabilidad y la seguridad del ecosistema.

## **3.2. Proceso de implementación**

#### **⭐ Para principiantes**

La implementación de la DAO se realiza paso a paso. Primero, creamos las herramientas básicas para votar y hacer propuestas. Luego, comenzamos a usarlas para tomar decisiones simples. A medida que avanzamos, aprendemos y mejoramos el sistema juntos.

#### **⭐⭐ Para iniciados**

El proceso de implementación de la Fase 1 incluye varias etapas clave:

1. Despliegue de los contratos inteligentes básicos (ya realizado),
2. Creación de una interfaz de usuario simple para votar y proponer (Uso de un sitio ya existente: Tally)
3. Formación de la comunidad sobre el uso de las herramientas de gobernanza,
4. Presentación de los primeros proyectos sobre los que la DAO podrá votar (RealT ya ha preparado una serie de propuestas),
5. Implementación del sistema de discusiones y debate para las propuestas (los desarrolladores de la comunidad ya han trabajado en una solución),
6. Lanzamiento de las primeras propuestas,
7. Lanzamiento de la primera campaña de incentivos al voto,
8. Ajuste de los parámetros de gobernanza en función de los comentarios,
9. Ampliación gradual del campo de acción de la DAO.

#### **⭐⭐⭐ Para expertos**

El proceso de implementación de la Fase 1 está estructurado para maximizar el aprendizaje y la adaptación:

1. Infraestructura técnica:
   - Despliegue de contratos inteligentes (gobernanza, tesorería, incentivos, PowerVotingRegistry),
   - Pruebas de red en grupo restringido (ya realizado),
   - Auditorías de seguridad y pruebas exhaustivas (realizadas internamente por RealT),
   - Prueba de red pública (ya realizada),
   - Desarrollo de herramientas para el cálculo del powerVoting (Realizado por RealT).
2. Gobernanza inicial:
   - Definición de los parámetros iniciales (quórum, umbrales de votación, timelock: RealT)
   - Implementación de un proceso de propuesta en varias etapas (discusión, formalización, votación),
   - Implementación de mecanismos de seguridad, incluido el derecho de veto de RealT,
   - Limitación del acceso a la creación de propuestas.
3. Compromiso comunitario:
   - Campaña de educación sobre los mecanismos de la DAO,
   - Creación de canales de comunicación dedicados para discusiones y retroalimentación,
   - Organización de eventos de gobernanza para estimular la participación,
   - Programa de incentivos.
4. Iteración y optimización:
   - Recopilación y análisis continuos de métricas de participación,
   - Ajustes regulares de los parámetros de gobernanza,
   - Experimentación con diferentes enfoques de gobernanza,
   - Debate sobre los diversos cambios y enfoques de gobernanza.
5. Expansión progresiva:
   - Definición de un plan de ampliación del campo de acción de la DAO,
   - Negociaciones con RealT para la transferencia progresiva de responsabilidades,
   - Preparación de la transición hacia la Fase 2 con la integración de NFT.

Este proceso está diseñado para ser flexible y adaptable, permitiendo a la comunidad influir activamente en la evolución de la DAO mientras se mantiene la estabilidad y la seguridad del ecosistema. Esto también brinda la oportunidad a los titulares de REG de comprender y familiarizarse con los mecanismos de la DAO, crear y descubrir vocaciones, hacer surgir ideas y nuevos casos de uso. La implementación progresiva de la DAO permite así una transición natural y una evolución progresiva hacia la descentralización del ecosistema.

## **3.3. Objetivos a corto y medio plazo**

#### **⭐ Para principiantes**

A corto plazo, queremos que todos entiendan cómo funciona la DAO y comiencen a participar según su comprensión y competencia. A medio plazo, queremos tomar decisiones más importantes juntos y descentralizar la gobernanza de los servicios ofrecidos por la DAO.

#### **⭐⭐ Para iniciados**

Objetivos a corto plazo:

1. Alcanzar una alta tasa de participación en las votaciones,
2. Educar a la comunidad sobre los mecanismos de la DAO (seguridad, equilibrio presupuestario, etc.),
3. Identificar los primeros proyectos a financiar o desarrollar,
4. Probar y ajustar los parámetros de gobernanza,
5. Establecer los procesos básicos para la gobernanza.

Objetivos a medio plazo:

1. Ampliar progresivamente el campo de acción de la DAO,
2. Desarrollar nuevos casos de uso para el REG,
3. Mejorar las herramientas de gobernanza basadas en la retroalimentación,
4. Preparar la transición hacia la Fase 2 con la integración de NFT.

#### **⭐⭐⭐ Para expertos**

Objetivos a corto plazo (3-6 meses):

1. Gobernanza:
   - Alcanzar un quórum estable del 50% en las votaciones,
   - Implementar y probar diferentes procesos de propuesta,
   - Establecer un proceso eficaz de debate y refinamiento de propuestas,
   - Elegir a los primeros miembros de la comunidad que puedan crear propuestas,
   - Definir el marco de una propuesta válida para someterla a votación.
2. Compromiso comunitario:
   - Lanzar campañas de incentivos al voto con recompensas,
   - Organizar eventos educativos regulares sobre la gobernanza DAO,
   - Crear un programa de mentoría para los nuevos miembros activos,
   - Fomentar iniciativas beneficiosas para la DAO y el ecosistema RealToken.
3. Desarrollo técnico:
   - Desarrollar/perfeccionar las herramientas de análisis para seguir la salud de la DAO,
   - Integrar funcionalidades básicas con el ecosistema existente.

Objetivos a medio plazo (6-18 meses):

1. Expansión de la DAO:
   - Negociar e implementar un plan de ampliación del campo de acción con RealT,
   - Desarrollar mecanismos de gobernanza más avanzados,
   - Crear comités especializados para diferentes aspectos del ecosistema,
   - Implementación más amplia del acceso a la creación de propuestas.
2. Innovación y desarrollo:
   - Lanzar iniciativas de investigación y desarrollo dirigidas por la comunidad,
   - Experimentar con nuevos modelos de incentivos y participación,
   - Desarrollar integraciones más profundas con el ecosistema,
   - Inicios de la fase 2 con la integración de NFT.
3. Preparación de la Fase 2:
   - Diseñar y probar los mecanismos de integración de NFT en la gobernanza,
   - Desarrollar una hoja de ruta detallada para la transición hacia la Fase 2,
   - Formar grupos de trabajo para cada aspecto importante de la nueva fase,
   - Realizar pruebas exhaustivas con la comunidad para la fase 2.
4. Ecosistema y asociaciones:
   - Establecer asociaciones estratégicas con otros proyectos DeFi y DAO,
   - Desarrollar casos de uso innovadores para el REG más allá de la gobernanza,
   - Integración de los primeros proveedores de servicios distintos de RealT.

Estos objetivos buscan establecer una base sólida para la DAO, al tiempo que preparan el terreno para una expansión e innovación continuas en el ecosistema RealToken.

## **3.4. Cómo participar en la fase 1**

#### **⭐ Para principiantes**

Para participar en la fase inicial de la DAO RealToken:

1. Compre tokens REG si aún no los tiene,
2. Siga los anuncios oficiales en los canales de comunicación de RealToken,
3. Participe en las votaciones cuando estén abiertas,
4. Dé su opinión en las discusiones de la comunidad,
5. Asista a eventos educativos para aprender más sobre la DAO,
6. Infórmese y fórmese sobre el tema de las DAO.

#### **⭐⭐ Para iniciados**

Aquí está cómo puede participar activamente en la fase inicial:

1. Vote regularmente sobre las propuestas,
2. Participe en los debates sobre las propuestas antes de las votaciones,
3. Proponga ideas de mejora en los foros de discusión,
4. Ayude a educar a los nuevos miembros sobre el funcionamiento de la DAO,
5. Participe en las campañas de incentivos para ganar recompensas,
6. Pruebe las nuevas funcionalidades y dé su retroalimentación,
7. Contribuya a la elaboración de los procesos de gobernanza,
8. Tome iniciativas y sométalas a la comunidad,
9. Redacte, mejore, traduzca y comparta contenido sobre las DAO.

#### **⭐⭐⭐ Para expertos**

Para una participación profunda en la fase inicial:

1.  Gobernanza:
    -   Analice en profundidad las propuestas y comparta sus análisis,
    -   Contribuya activamente al desarrollo de métricas para evaluar la salud de la DAO,
    -   Proponga ajustes a los parámetros de gobernanza, basados en datos y argumentados con una visión a corto, medio y largo plazo.
2.  Desarrollo técnico:
    -   Participe activamente en las Testnet,
    -   Proponga mejoras técnicas para las herramientas de gobernanza,
    -   Contribuya al desarrollo de herramientas de análisis para la DAO,
    -   Revise y audite continuamente los contratos inteligentes, manténgase informado sobre posibles hackeos y vulnerabilidades.
3.  Compromiso comunitario:
    -   Organice sesiones educativas para la comunidad,
    -   Cree contenido explicativo sobre el funcionamiento de la DAO,
    -   Participe activamente en los programas de mentoría,
    -   Explore nuevos conceptos de transmisiones en vivo, videos, artículos, etc.
4.  Innovación:
    -   Proponga nuevos casos de uso argumentados para el REG,
    -   Participe en grupos de trabajo sobre la futura integración de NFTs,
    -   Explore sinergias potenciales con otros proyectos DeFi.
5.  Preparación para la Fase 2:
    -   Contribuya al diseño de mecanismos de integración de NFTs,
    -   Participe en pruebas exhaustivas de las nuevas funcionalidades,
    -   Ayude a elaborar la hoja de ruta para la transición a la Fase 2.

Su participación activa en todos estos niveles contribuirá a dar forma al futuro de la DAO RealToken y asegurar su éxito a largo plazo.

[Página siguiente](/es/DAO/Fonctionement)
---
title: Propuesta utilizando Sablier.com
description: 
published: true
date: 2025-01-03T12:42:08.426Z
tags: 
editor: markdown
dateCreated: 2025-01-03T12:42:08.426Z
---

# Creando una secuencia de reloj de arena
El objetivo de este artículo es doble:
- Cualquier propuesta de votación (en el escrutinio) debe ser analizada detalladamente, en particular la parte que se refiere al código que se ejecutará.
El final del artículo ayudará a comprender el código de la propuesta: Liberación de REG del presupuesto DAO Team RealT.
- En el futuro, es posible que los proponentes deseen utilizar este ejemplo para crear nuevas propuestas en Tally.
El artículo comienza con la presentación de los parámetros (en Tally y Hourglass) para crear un flujo de Hourglass.

## Recursos

- sitio: https://sablier.com/
- documentos: https://docs.sablier.com/
- Documentos de adquisición de derechos: https://docs.sablier.com/apps/features/vesting
- Documentos de curvas: https://docs.sablier.com/concepts/lockup/stream-shapes#lockup-linear
- Contrato implementado de Docs: https://docs.sablier.com/guides/lockup/deployments#mainnets
- Ejemplo de Sepolia: https://sepolia.etherscan.io/tx/0x86239fed5f924b61c7f5656e0e9322d7809097f2c2153b478fb38c07f85bdb4d
- Ejemplo de prueba de red de conteo: https://www.tally.xyz/gov/reg-dao-beta/proposal/6332954951702383415567855302130620076860949854692323526582374508844353542470

## Instrucciones comunes a todos los tipos de transmisiones en Tally

1. Crea una nueva propuesta
1. Complete el formulario de descripción de la propuesta.
1. Agregar una acción
 - acción personalizada
 - Contrato del token enviado
 - Utilice la ABI importada (si tiene éxito; de lo contrario, deberá proporcionar la ABI)
 - Método de contrato -> aprobar
1. Agregar una acción
 - acción personalizada
 - Contrato Sablier SablierV2BatchLockup ([documentos de implementación](https://docs.sablier.com/guides/lockup/deployments))
 - Utilice la ABI importada (si tiene éxito; de lo contrario, deberá proporcionar la ABI)
 - Método de contrato -> [Seleccione el método que sea adecuado] (preferiblemente el que tiene marcas de tiempo es más fácil de configurar)
 - Calldatas -> [Ingrese los parámetros] la parte del lote puede ser complicada de completar, especialmente los elementos tipo tulpe[]
1. Realiza la simulación para comprobar que todo está bien.

## Tipos de transmisiones de reloj de arena

Hay varios tipos de transmisiones que ofrecen diferentes comportamientos de desbloqueo:

### Corriente lineal

- Distribución de tasa constante por segundo.
- La curva forma una línea diagonal recta.
- Ideal para distribuciones simples y predecibles.

#### variante

Lineal con acantilado

- Similar al streaming lineal pero con un período de bloqueo inicial
- No hay tokens disponibles durante el período de acantilado.
- Después del acantilado, la distribución se vuelve lineal.
- Perfecto para planes de adquisición de derechos con período de bloqueo

Desbloqueo y Lineal

- Similar al flujo lineal pero con un desbloqueo inicial
- Varios tokens se desbloquean instantáneamente
- El equilibrio de distribución se vuelve lineal.
- Perfecto para planes de incentivos a corto y medio plazo.

Desbloquear, Acantilado y Lineal

- Combinación de desbloqueo, acantilado y lineal.
- Varios tokens se desbloquean instantáneamente
- Después del primer desbloqueo, se aplica un período de bloqueo.
- El saldo de distribución se vuelve lineal después del período de bloqueo.
- Perfecto para planes de lanzamiento de tokens

### Corriente exponencial

- La distribución se acelera con el tiempo.
- Los beneficiarios reciben más tokens hacia el final del período.
- Ideal para lanzamientos aéreos e incentivos a largo plazo.

#### variante

Acantilado exponencial

- Combina tres elementos:
 1. Un período de acantilado (bloqueo inicial)
 2. Liberación instantánea de una cantidad específica al final del acantilado.
 3. Una distribución exponencial para el resto de tokens
- Particularmente adecuado para planes de adquisición de derechos de equipo
- Fomenta la retención a largo plazo.

### Bloqueo de tiempo

- Bloquea todos los tokens durante un período definido
- Liberación total de una sola vez al final del período.

### Desbloquear en pasos

- Desbloqueo en etapas regulares.
- Permite lanzamientos periódicos regulares.
- Se acumulan tokens no reclamados

#### variante

Desbloquear mensualmente

- Nivel mensual
- Desbloqueo mensual en una fecha fija
- Ideal para planes de nómina o ESOP

espalda ponderada

- Desbloqueo por pasos progresivos
- Rodamiento con valor independiente
- Se acumulan tokens no reclamados
- Ideal para planes de concesión con rodamientos de desbloqueo.

## Información específica

### Estructura técnica de los segmentos.

Esta estructura es utilizada por el flujo dinámico (LD)
Cada "segmento" en un "flujo dinámico" está definido por tres parámetros:

- `amount`: cantidad de tokens a distribuir (uint128)
- `exponente`: ​​el exponente que define la curva de distribución (UD2x18)
- `timestamp`: marca de tiempo final del segmento (uint40)

La fórmula de distribución es la siguiente: f(x) = x^exp \* csa + Σ(esa)
O :

- x = tiempo transcurrido / tiempo total del segmento
- exp = exponente del segmento
- csa = cantidad de segmento actual
- Σ(esa) = suma de los importes de los tramos anteriores

#### Estructura de tupla

En Tally, la parte por lotes de las funciones de tipo LD se compone de un campo "segmentos" que es una matriz de tipo "tupla []".
Tenga en cuenta que las siguientes explicaciones se basan en la función `createWithTimestampsLD`; para otras funciones de tipo LD, deberá adaptar los tipos de tupla.

```solidez
segmento de estructura {
 cantidad uint128;
 exponente UD2x18;
 marca de tiempo uint40;
}
```

el campo `segmentos` en la función `createWithTimestampsLD` está compuesto por 3 segmentos

- segmento 0: representa el período entre startTime y el final del segmento 0
- segmento 1: Representa el período entre el final del segmento 0 y el final del segmento 1
- segmento 2: Representa el período entre el final del segmento 1 y el final de la secuencia

## Creación del reloj de arena para el presupuesto del Team RealT
Los siguientes parámetros son los de la propuesta [RIP000xx] - Liberación de REG del presupuesto del Team RealT
La transmisión es Cliff Exponential, lo que significa que la transmisión comienza después de un período de acantilado, luego distribuye tokens exponencialmente, con mayores desbloqueos hacia el final del período.

![payout_sablier_team_realt.png](/imag-en/payout_sablier_team_realt.png)

### Parámetros para la función `createWithTimestampsLD`


Encuentre la propuesta en Tally: https://www.tally.xyz/gov/realtoken-ecosystem-governance/proposals

```
Función/Firma: createWithTimestampsLD(dirección,dirección,tupla[])
Objetivo del contrato: 0x0F324E5CB01ac98b2883c8ac4231aCA7EfD3e750

Datos de llamada (lote):
lockupDynamic -> dirección: 0x555eb55cbc477Aebbe5652D25d0fEA04052d3971
activo -> dirección: 0x0AA1e96D2a46Ec6beB2923dE1E61Addf5F5f1dce
lote -> tupla[]:
 remitente -> dirección: 0x3f2d192F64020dA31D44289d62DB82adE6ABee6c
 destinatario -> dirección: 0x5Fc96c182Bb7E0413c08e8e03e9d7EFc6cf0B099
 cantidad -> uint256: 50000000000000000000000000
 transferible -> bool: verdadero
 cancelable -> bool: falso
 hora de inicio -> uint40: 1714521600
 segmentos -> tupla[]: [
 ["0", "1000000000000000000", "1746057600"],
 ["500000000000000000000000", "10000000000000000000", "1746057601"],
 ["4950000000000000000000000", "30000000000000000000", "1872288000"]
 ]
 corredor -> tupla [dirección, uint256]: [0x000000000000000000000000000000000000000, 0]
```

#### Detalles del segmento

```json
[
 ["0", "1000000000000000000", "1746057600"],
 ["500000000000000000000000", "10000000000000000000", "1746057601"],
 ["4950000000000000000000000", "30000000000000000000", "1872288000"]
]
```

- segmento 0: ["0", "1000000000000000000", "1746057600"]
- segmento 1: ["500000000000000000000000", "1000000000000000000", "1746057601"]
- segmento 2: ["495000000000000000000000000", "3000000000000000000", "1872288000"]

##### Explicaciones:

- segmento 0:

 - 0 = monto distribuido durante el segmento
 - 1000000000000000000 = exponente de segmento = 1 en decimal (1000000000000000000 = 10^18)
 - 1746057600 = marca de tiempo de finalización del segmento (1 de mayo de 2025 00:00:00 UTC)

- segmento 1:

 - 500000000000000000000000 = cantidad distribuida durante el segmento (500k ^18)
 - 1000000000000000000 = exponente de segmento = 1 en decimal (1000000000000000000 = 10^18)
 - 1746057601 = marca de tiempo de fin de segmento (1 de mayo de 2025 00:00:01 UTC)

- segmento 2:

 - 49500000000000000000000000 = monto distribuido durante el segmento (49,5 Millones ^18)
 - 3000000000000000000 = exponente de segmento (3000000000000000000 = 10^18)
 - 1872288000 = marca de tiempo de fin de segmento (1 de mayo de 2029 00:00:00 UTC)

**Segmento 1**, define una fecha de finalización del segmento con una distribución de 0 del parámetro `startTime`, lo que crea un período de bloqueo duro sin distribución.

**Segmento 2** define una fecha de finalización del segmento con una distribución de 500.000 tokens, que libera una suma definida en cantidad disponible instantáneamente en la fecha de finalización del segmento.

**Segmento 3**, define una fecha de finalización del segmento con una distribución de 49,5 millones de tokens, lo que crea la curva exponencial desde el final del segmento 2 hasta la fecha de finalización del segmento 3, distribuirá el monto durante el período con un exponente de 3
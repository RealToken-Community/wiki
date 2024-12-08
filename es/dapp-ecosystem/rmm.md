---
title: RMM
description: 
published: true
date: 2024-12-08T21:14:18.437Z
tags: rmm
editor: markdown
dateCreated: 2024-12-08T21:14:18.437Z
---

# **RMM v3**

## **1. Introducción al Wrapper RMM**

#### **⭐ Para Principiantes**

El Wrapper RMM es un contrato inteligente que sirve como interfaz entre los RealTokens y el protocolo RMM (RealToken Market Maker). Permite a los usuarios depositar sus RealTokens y recibir a cambio RTW (RealToken Wrapped) que se utilizan en el RMM como prueba de depósito para préstamos.

#### **⭐⭐ Para Iniciados**

El Wrapper juega un papel crucial en el ecosistema RMM:

- Estandarizando la interfaz de RealTokens para el protocolo RMM
- Permitiendo extender la capacidad en número de tokens del RMM limitado inicialmente a 128 tokens, permite depositar 2000 tokens por Wrapper, o aproximadamente 100x2000 tokens
- Por cada RealToken depositado, el Wrapper crea RTW (RealToken Wrapped) con un valor de 1$ por RTW, utilizado en el RMM como depósito para préstamos
- El wrapper crea y deposita tantos RTW como el valor (en $) de los RealTokens depositados.
  El depositante recibe tantos armmRTW (prueba de depósito) como el valor de sus RealTokens depositados.
- El wrapper gestiona los balances de RTW según el valor de cada RealToken creando o quemando RTW para mantener el valor depositado (en RTW) siempre igual al valor depositado en RealToken.

## **2. Modificaciones Principales Aportadas al Wrapper por la v3**

Dirección del wrapper: 0x10497611Ee6524D75FC45E3739F472F83e282AD5

Estas modificaciones son consecuencia de la propuesta de gobernanza RIP00008 que fue aceptada por la DAO el 2024-12-07 21:19 UTC.

[RIP00008 - Transfer RealToken Wrapper RMMv3 execution functions to the DAO](https://www.tally.xyz/gov/realtoken-ecosystem-governance/proposal/4412019256781079844728885554420538992900805587725535743224739658055634526928)

#### **⭐ Para Principiantes**

Las principales modificaciones al Wrapper se realizaron para garantizar el cumplimiento legal y las operaciones rutinarias relacionadas con el ciclo de vida de un RealToken.

En algunos casos, el emisor puede necesitar destruir RealTokens:

- Pirateo o pérdida de acceso a la dirección del propietario
- Fallecimiento del propietario
- Venta de la casa vinculada al RealToken
- Reembolso de parte o la totalidad de la deuda vinculada al RealToken

lo que podría hacer que el RMM no funcione correctamente para los RealTokens correspondientes, ya que no habría colateral para cubrir las deudas contraídas.

Para cumplir con estas obligaciones y asegurar la continuidad y el buen funcionamiento del RMM, se han añadido nuevas funcionalidades al Wrapper para reembolsar parcial o totalmente una deuda vinculada a un RealToken en una cuenta y extraer forzosamente el RealToken correspondiente de las cuentas afectadas.

Este proceso está supervisado por el voto de la DAO para asegurar que no haya abusos.
Garantiza que el monto debido por la destokenización sea correctamente recibido por el propietario, que el RMM pueda seguir funcionando y cumplir con las obligaciones legales.

#### **⭐⭐⭐ Para Expertos**

Para mantener un buen funcionamiento del RMM, las modificaciones al Wrapper se realizaron para permitir recuperar los RealTokens vinculados a una oferta en casos de fallecimiento del propietario, venta de la casa vinculada al RealToken, reembolso de parte o la totalidad de la deuda vinculada al RealToken, destokenización u otros eventos que hagan que el activo subyacente no sea elegible para el depósito en el RMM.

Para recuperar o forzar el retiro de los RealTokens del protocolo RMMv3, son necesarias dos funciones:

- `repayForRecover`: Esta función gestiona el reembolso parcial o total de la deuda que puede estar activa en la cuenta de un usuario del RMM (en casos como fallecimiento, destokenización, etc.). Una vez ejecutada, la segunda función puede ser llamada de manera segura para extraer los RealTokens del RMM.
- `recoverByGovernance`: Esta función recupera los RealTokens, ya sea retirándolos a una dirección especificada (dirección de servicio para quemar) o moviendo los aTokens/RealTokens a la dirección especificada (nueva dirección del usuario).

## **3. Riesgos y Consideraciones**

#### **⭐ Para Principiantes**

Estas modificaciones otorgan un derecho total sobre los tokens depositados en el RMM, por lo que es vital que cualquier presentación para activar estas funcionalidades sea minuciosamente analizada por la DAO para asegurar que no haya abusos.  
En principio, solo RealT o un socio aprobado de la DAO que emita tokens debería iniciar tales propuestas.  
La DAO debe asegurarse de que estas funcionalidades se activen únicamente en casos que lo requieran expresamente para respetar una regulación o garantizar el buen funcionamiento del RMM.

#### **⭐⭐ Para Iniciados**

El uso abusivo de estas funcionalidades podría conducir a robos de tokens, reembolsos de deudas abusivos, préstamos ilegítimos, comprometiendo la seguridad del RMM y sus usuarios.

Los principales riesgos relacionados con el Wrapper son:

- Protección de los RealTokens depositados
- Prevención de ataques potenciales
- Preservar el buen funcionamiento del RMM
- Garantizar el cumplimiento de las obligaciones legales
- Poder devolver la propiedad de los RealTokens depositados al usuario que haya perdido acceso a su cuenta

> Es crucial entender que el Wrapper es un componente central del RMM y que cualquier modificación o uso de sus funcionalidades debe ser cuidadosamente evaluado.
> {.is-warning}

## **4. ⭐⭐⭐ Información Técnica**

### **4.1. repayForRecover**

#### **Parámetros:**

- `repayWallet` (address): Dirección que tiene los RealTokens en RMMv3 y de los cuales se extraerán los tokens con deuda activa.
- `refundWallet` (address): Dirección que recibirá cualquier exceso de pago después del reembolso de la deuda.
- `percent` (uint256): Porcentaje (escala 100% = 10000) de cada token depositado que será extraído del RMM.
- `recoverAssets` (address[]): Array de direcciones de tokens que representan los RealTokens a extraer del RMMv3.
- `debtAssets` (address[]): Array de direcciones de tokens que representan los activos de deuda a reembolsar.
- `payer` (address): Dirección responsable del pago de la deuda.

[Continúa con todas las secciones técnicas...]

## **5. Nota Importante:**

Las funciones `repayForRecover` y `recoverByGovernance` no gestionan el Factor de Salud (HF). Por lo tanto, asegúrese previamente de que el HF permite el retiro, de lo contrario la propuesta completa fallará.

La función `recoverByGovernance` no verifica las deudas existentes. Esto debe ser anticipado en la propuesta. La función `repayForRecover` debe ser ejecutada previamente si es necesario para evitar fallos.

Las direcciones que en algún momento reciben RealTokens deben cumplir estrictamente con las reglas del registro de cumplimiento de estos tokens, de lo contrario la propuesta completa fallará.

## **6. Modificaciones**

Lista de modificaciones en la actualización de RealTokenWrapper:

```
if (!_isRealTokenWhitelisted[asset]) revert WrapperErrors.TokenNotWhitelisted(asset);
```

```
uint256 length = _realTokenList.length;
uint256 userIndex = 0;
```

```
// Cache user token list for gas optimization
address[] memory userTokenList = _tokenListOfUser[user];
uint256 length = userTokenList.length;
```

> IMPORTANTE: la validación de una propuesta que ejecute las funciones `repayForRecover` y `recoverByGovernance` debe realizarse con gran atención, ya que puede ser muy peligrosa si no está correctamente configurada o se usa con fines maliciosos.
> {.is-warning}

## **7. Auditoría**

Realizada por la empresa ADBK: https://github.com/abdk-consulting/audits/tree/main/realt
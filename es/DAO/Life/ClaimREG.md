---
title: Reclamación REG
description: 
published: true
date: 2025-03-20T13:57:05.061Z
tags: 
editor: markdown
dateCreated: 2025-03-19T13:27:06.290Z
---

# Preámbulo: ¿Por qué se te asignan REG? ⭐


Con cada tokenización de un activo emitido en el ecosistema, la DAO permite a sus socios de tokenización asignar una cantidad de USDREG ($ convertibles solo en REG) a los clientes que han comprado y mantenido los tokens por una duración variable dependiendo de los activos. Estos USDREG son convertibles a REG en cualquier momento al precio de mercado.

Actualmente, el monto USDREG corresponde a los honorarios que cobra RealT por el trabajo de búsqueda de propiedades y se asigna en el momento de la primera revaluación de la propiedad.
RealT ha anunciado su intención de desarrollar los criterios para la concesión de derechos en USDREG, para que sean aplicables a todos los tipos de tokenizaciones que RealT ofrece y no sólo a los inmuebles tradicionales (tokenización de préstamos, por ejemplo, que no tiene revalorización).

Antes de la creación de REG, esta cantidad se pagaba en tokens SOON, cuyo valor era 1 USDREG.

Una vez implementada la aplicación de reclamo REG, el SOON desaparecerá y la asignación durante la primera revaluación se realizará directamente en USDREG (ver RIP00009).

![valuation.png](/imag-en/regconvertor/valuation.png){.align-right .img40}

Las revalorizaciones de inmuebles las realizan empresas independientes de RealT.
El coste de esta actuación se está volviendo alto (sobre todo en los edificios multifamiliares) y está llevando a RealT a posponer estas revalorizaciones, que originalmente debían ser anuales.

En el futuro, la distribución de USDREG ya no debería estar vinculada a la revalorización, sino a un retraso después de la tokenización.

# La aplicación para reclamar tu REG ⭐

[https://claim.realtoken.network/](https://claim.realtoken.network/)
![claim1.png](/imag-en/regconvertor/claim1.png){.align-right .img35}

1. La aplicación se ejecuta en la cadena de bloques Gnosis,
2. Necesitas conectar una de las siguientes billeteras:
- el declarado como receptor de los Realtokens, en [Realt.co](https://realt.co/) (al momento de lanzar la conversión): para reclamar los USDREG ($ convertibles en REG) que corresponden a los Soon recibidos durante varios años,
 - el propietario de los Realtokens, que serán reevaluados posteriormente (una vez que se haya detenido la distribución de Soon).
3. Elija su modo de presentación (idioma, fondo),
4. Comprueba que estás en “REG Claim” y no en “Genesis”.

Nota: Los usuarios *Walletless* deberán migrar a *Realtoken Wallet u otra billetera compatible* para iniciar sesión en la aplicación y luego solicitar soporte para realizar el reclamo en la nueva Wallet por los tokens asignados a Walletless.
[Enlace de ayuda](https://community-realt.gitbook.io/tuto-community/site-realt/option-realtoken-wallet-account-abstraction)

![claim2.png](/imag-en/regconvertor/claim2.png){.align-right .img30}
Una vez que haya iniciado sesión, verá:
1. La cantidad en USDREG que se te asignó (20 en la imagen),
2. La paridad del REG en $ en este momento (1,93),
3. El número de REG correspondiente (10.34), que puedes reclamar (en su totalidad),
4. La cantidad de USDREG que ya ha reclamado (100), antes de una nueva asignación,
5. La opción de delegación (ver capítulo siguiente),
6. La opción de Reclamación Automática
 Usted puede autorizar la ejecución automática de reclamaciones para su atención (por un robot), tan pronto como se le asignen USDREG (retraso máximo de 24 horas).
 Se puede cobrar una tarifa por este servicio (actualmente 0%).
<br>

## Reclamar delegado

![delegateclaim.png](/imag-en/regconvertor/delegateclaim.png){.align-right .img45}

 Es posible delegar otra dirección para reclamar y cobrar sus REG (por ejemplo en caso de una billetera corrupta):
1. Conexión a la aplicación con la billetera inicial (0x69…94a en la imagen),
2. Verifique la reclamación con otra dirección, e indique la dirección delegada (0x…C08 en la imagen). Firme luego esta autorización de delegación con la dirección inicial.
3. La aplicación indica que debes cambiar tu dirección de inicio de sesión para reclamar,
4. Conéctese con la dirección delegada (0x…C08 en la imagen),
5. Lanzar la reclamación,
6. La reclamación enviará los REGs, asignados a la dirección inicial, a la dirección delegada.
<br>

# Cómo funciona ⭐⭐

## Antes de la creación del contrato de reclamación

Durante el período Pronto (2021 a marzo de 2025) y antes de la creación del contrato de reclamación REG (Bóveda):


1. Al final de cada revalorización de un Realtoken, RealT lanzó la Mint del correspondiente Soon,
![ccm1.png](/imag-en/regconvertor/ccm1.png){.align-right .img50}
2. Estos pronto se asignaron a las billeteras de los propietarios de Realtoken en el momento de la revaluación.

Para inicializar el contrato de reclamación, los Soons se convierten a USDREG y luego se destruyen:
3. RealT lanza la conversión de Soon,
4. Recopilación de todas las carteras que tengan Soon con su cantidad,
5. Pronto se convertirán a USDREG,
Esta cantidad para cada billetera se registra en un archivo fuera de la cadena ([Merkel Tree)](https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json), los Soon convertidos se asignan para cada dirección a la última dirección de entrega de Realtoken conocida en el sitio realt.co del usuario.
6. Pronto los tokens se destruyen (se queman).

Todas las acciones anteriores se realizan solo una vez, cuando se crea el contrato de reclamación.

## Queja del usuario

Una vez inicializado el archivo "Merkle Tree" (la primera vez desde los Soons acumulados):

1. La reclamación de REG podrá solicitarse:
 - de la solicitud de reclamo que muestra el número de USDREG,
 - o directamente en el contrato desde la billetera del usuario (solución más compleja).
![ccm2.png](/imag-en/regconvertor/ccm2.png){.align-right .img50}
2. La reclamación se ejecuta mediante el contrato inteligente de conversión de Vault,
3. Comprueba desde la raíz del Árbol Merkle, que le fue transmitido, que el usuario tiene los derechos correspondientes,
4. Calcula la cantidad de USDREG a partir de los datos del Árbol Merkle y los USDREG ya reclamados (cuyo valor almacena) para determinar la cantidad de USDREG que se pueden reclamar, esta cantidad luego se convierte en la cantidad de REG del precio de Oracle,
5. Solicita al contrato REG que cree (acuña) los REG,
6. Los nuevos REG se transfieren a la billetera del usuario.
7. Las revalorizaciones/distribuciones que seguirán (tras la desaparición del Soon) se realizarán directamente en USDREG, actualizando el Árbol de Merkel. Esta actualización traerá nuevos USDREG a la aplicación para reclamar.
 Los nuevos USDREG se asignarán a la dirección que tenga los Realtokens en el momento de la instantánea (a diferencia del derecho a USDREG de la conversión de Soon, que se asigna a la última dirección de entrega de RealToken registrada en su cuenta realt.co)

Si la dirección con USDREG se ha corrompido, es posible simplemente firmar con la dirección corrompida delegando el reclamo a otra dirección. Esta otra dirección podrá entonces solicitar los REGs y recibirlos.

## Reclamación “automática”

La ejecución de la reclamación por parte del contrato inteligente de Vault se puede activar:
- ya sea manualmente, en cualquier momento, por el usuario (se elige entonces la paridad),
- ya sea automáticamente por un autómata (bot) después de la asignación de nuevos USDREG (la paridad no se elige entonces). Este último método requiere autorización previa.

1. La máquina monitoreará las actualizaciones de Merkel Tree para los usuarios registrados y luego activará un reclamo REG para los usuarios afectados.
![ccm3.png](/imag-en/regconvertor/ccm3.png){.align-right .img50}
2. Dependiendo de la configuración del contrato inteligente de conversión de Vault, se podrán aplicar comisiones de reclamación automáticas (inicialmente del 0 %), para incentivar la creación de dicho autómata y cubrir las comisiones de transacción asociadas (la activación de las comisiones deberá someterse a una votación de gobernanza).
3. La Bóveda solicitará la acuñación de dos cantidades de REG: una para la tarifa y otra para el usuario,
4. El contrato REG envía las tarifas REG a la billetera ATM,
5. El contrato REG acuña el REG del usuario en la billetera del usuario.

Nota: Esta función no se puede combinar con un reclamo delegado (que es similar a un reclamo del usuario).

## Dirección

Contrato inteligente:

- Próximamente: [0xaa2c0cf54cb418eb24e7e09053b82c875c68bb88](https://gnosisscan.io/token/0xaa2c0cf54cb418eb24e7e09053b82c875c68bb88)
- REGISTRO: [0x0aa1e96d2a46ec6beb2923de1e61addf5f5f1dce](https://gnosisscan.io/token/0x0aa1e96d2a46ec6beb2923de1e61addf5f5f1dce)
- Bóveda: [0x71eb2b2518556700f5b778b9cc313bfe8de5086e](https://gnosisscan.io/address/0x71eb2b2518556700f5b778b9cc313bfe8de5086e)
- Oráculo: [0x86339b40e588f774bd766eB70D47bEFBe68B6F64](https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced)

Árbol de Merkel (fuera de cadena): [https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json](https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json)

La comunidad puede guardar este archivo para que permanezca disponible en todas las situaciones. La última versión, cuya raíz está registrada en el contrato de reclamación, sigue siendo válida incluso si la interfaz o el archivo fuente ya no existen.

# Detalles de la operación del contrato de bóveda ⭐⭐⭐

La dirección del contrato inteligente anterior es la del Proxy (invariable) que apunta a la siguiente implementación (que puede variar, dependiendo de las actualizaciones): [https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code)

El programa gestiona la conversión y distribución de tokens REG, basándose en las cantidades USDREG registradas en un árbol de Merkel y el valor de REG proporcionado por el Oráculo.

## Características principales

1. **Roles y permisos**: El contrato utiliza el sistema de control de acceso de OpenZeppelin para gestionar diferentes roles (administrador, operador, pausador y actualizador) para proteger las operaciones críticas.
2. **Administración de tokens**: interactúa con el token REG, lo que permite a los usuarios reclamar tokens REG en función de los montos USDREG, mientras verifica las pruebas de Merkle para garantizar la elegibilidad.
3. **Funciones de reclamo**: Los usuarios pueden reclamar tokens directamente o a través de un delegado. El contrato también gestiona funciones de reclamaciones automáticas. La afirmación de Walletless existe, pero no está implementada offChain: las cuentas Walletless tendrán que migrar a una nueva billetera.
4. **Gestión de marcas de tiempo**: El contrato incluye mecanismos para verificar que las reclamaciones se produzcan dentro de un período de tiempo definido, evitando reclamaciones fuera de ese período.
5. **Puente y Transferencias**: También permite transferir tokens a otras cadenas (puente) y administrar las tarifas asociadas a estas transferencias.
6. **Actualizaciones**: El contrato está diseñado para actualizarse a través de un mecanismo de actualización, lo que permite agregar nuevas características o corregir errores sin perder el estado del contrato.
7. **Eventos**: emite eventos para rastrear acciones importantes, como reclamos de tokens, actualizaciones de parámetros y errores.

## Validación con prueba de Merkel

La validación de prueba de Merkle funciona verificando que la información de un usuario (dirección y monto) esté incluida en una estructura de datos llamada árbol de Merkle.

Ejecución en el programa:

1. **Generación de hojas**: para cada reclamo, se genera una “hoja” mediante el hash de los datos del usuario (dirección y monto) con la función keccak256. Esto crea un hash único que representa esa afirmación.
![](/imag-en/regconvertor/rc2.png){.align-right .img50}

<br>

2. **Verificación de prueba**: La función *\_validateMerkleProof* toma como entrada la dirección del usuario, la cantidad, la raíz de Merkle esperada y una matriz de pruebas de Merkle. Verifica que la prueba de Merkle sea válida para el usuario y la cantidad especificados.
![](/imag-en/regconvertor/rc1.png){.align-right .img50}
<br>
[Enlace al código correspondiente](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L669)
 <br>
 <br>

3. **Verificación del árbol de Merkle**: La función *\_verifyAsm* utiliza un enfoque de ensamblaje para recorrer las pruebas de Merkle. Toma cada nodo de la prueba y lo combina con la hoja para reconstruir el hash hasta llegar a la raíz de Merkle. Si la raíz reconstruida coincide con la raíz de Merkle esperada, esto significa que la afirmación es válida.
![](/imag-en/regconvertor/rc3.png){.align-right .img50}
<br>
[Enlace al código correspondiente](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L719)
<br>

4. **Rechazo de reclamaciones inválidas**: si la verificación falla, el contrato rechaza la reclamación arrojando un error, garantizando así que solo se acepten reclamaciones válidas, que coincidan con la estructura del árbol de Merkle.


## Método de queja

Los principales métodos de reclamación en el contrato son:

### Queja del usuario (líneas 176 a 200)

- Función: *reclamar*
- Descripción: Este modo permite a un usuario reclamar tokens REG en función de una cantidad en USDREG.
-   Proceso :

 1. El usuario llama a la función de reclamo con la cantidad que desea reclamar y una prueba de Merkle.
 2. La función verifica que el contrato no esté en pausa y que las marcas de tiempo sean válidas.
 3. Verifica que la prueba de Merkle sea válida para el usuario y la cantidad especificados.
 4. Verifica el monto ya reclamado por el usuario para evitar dobles reclamos.
 5. Si se aprueban todas las comprobaciones, el contrato calcula la cantidad de REG a emitir y realiza la acuñación de tokens REG para el usuario.

![](/imag-en/regconvertor/rc4.png){.align-right .img50}

[Enlace al código correspondiente](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L176)
<br>
<br>

### Reclamación de un delegado (líneas 302 a 336)

- Función: *claimByDelegate*
- Descripción: Este modo permite a un usuario designar un delegado para reclamar tokens en su nombre.
-   Proceso :

 1. El usuario proporciona información del reclamo (dirección de la cuenta, monto, destinatario) y firma para demostrar la autorización.
 2. La función verifica que el contrato no esté en pausa y que las marcas de tiempo sean válidas.
 3. Valida la firma para garantizar que el delegado esté autorizado a actuar en nombre del usuario.
 4. Al igual que en el modo de reclamación del usuario, se valida la prueba de Merkle y se verifica el monto ya reclamado.
 5. Si todo es válido, el contrato acuña los tokens REG para el destinatario.

> La interfaz para esta función envía los REG a la dirección delegada.
En una futura actualización de la interfaz: será posible delegar el reclamo, sin enviar los tokens al delegado, sino a una dirección determinada al momento de la firma.
{.is-warning}

![](/imag-en/regconvertor/rc5.png){.align-right .img50}
<br>
[Enlace al código correspondiente](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L302)
<br>
<br>

### Reclamación automática (líneas 552 a 640)

- Función: claimByAutoClaim
- Descripción: Este modo permite a los usuarios reclamar automáticamente tokens REG sin tener que iniciar el reclamo manualmente.
-   Proceso :

 1. El usuario llama a la función con una matriz de estructuras de reclamación.
 2. La función verifica que el contrato no esté en pausa y que las marcas de tiempo sean válidas.
 3. Para cada reclamación, verifica si la auto reclamación está habilitada para el destinatario.
 4. Valida la prueba de Merkle y verifica el monto ya reclamado.
 5. Los tokens REG se acuñan para el destinatario y pueden aplicarse tarifas si están configurados.

![](/imag-en/regconvertor/rc6.png){.align-right .img50}

[Enlace al código correspondiente](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L552)
<br>
<br>

## Oráculo de precios REG

El contrato de Oracle de precio REG se basa en el código de Chainlink e incluye:

- el contrato inteligente [https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced#code](https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced#code)
- una infraestructura de Chainlink para automatizar encuestas de precios dentro y fuera de la cadena en múltiples fuentes y realizar un cálculo que apunta a evitar la manipulación de precios.
 El cálculo tiene en cuenta los volúmenes de negociación, el tiempo invertido en un precio, las diferencias entre las fuentes de precios, crea una ponderación para evitar manipulaciones, la tasa de actualización con el valor calculado se actualiza en cadena cada 24 horas por un autómata Chainlink.
 
En el gráfico a continuación se puede ver el Precio Promedio Ponderado en el Tiempo (TWAP) calculado por Oracle comparado con el precio spot de REG

![oracle_vs_spot.png](/imag-en/regconvertor/oracle_vs_spot.png)

### ¿Por qué utilizar este método de cálculo para la conversión de REG?

- **Mitigar la manipulación del mercado**: TWAP calcula el precio durante 24 horas, lo que reduce el riesgo de manipulación en mercados ilíquidos como REG, donde los precios al contado pueden distorsionarse fácilmente mediante pequeñas transacciones.

- **Evita espirales descendentes**: al no reaccionar inmediatamente a las caídas de precios, TWAP estabiliza el mercado y evita las ventas de pánico, que pueden conducir a una espiral descendente para los tokens ilíquidos.

- **Garantizar la estabilidad de precios**: TWAP ofrece una tasa de conversión constante basada en promedios diarios, protegiendo a REG de la volatilidad del precio al contado, especialmente en entornos de baja liquidez. Garantiza la equidad al asegurar que todos los usuarios puedan, durante un período de tiempo, obtener la misma cantidad de REG por un crédito determinado.

- **Apoya el autoequilibrio para proteger REG**: TWAP ajusta la emisión de REG en función de los movimientos de precios; Los usuarios reciben menos REG cuando los precios bajan y más REG cuando los precios suben.

- **Diseñado para la conversión**: el mecanismo está diseñado para permitir a los usuarios elegir cuándo convertir, en lugar de servir como una herramienta comercial en tiempo real.
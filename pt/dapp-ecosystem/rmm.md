---
title: RMM
description: 
published: true
date: 2024-12-20T15:46:49.889Z
tags: rmm
editor: markdown
dateCreated: 2024-12-08T21:15:34.828Z
---

# **RMM v3**

## **1. Introdução ao Wrapper RMM**

#### **⭐ Para Iniciantes**

O Wrapper RMM é um contrato inteligente que serve como interface entre os RealTokens e o protocolo RMM (RealToken Market Maker). Permite que os usuários depositem seus RealTokens e recebam em troca RTW (RealToken Wrapped) que são usados no RMM como prova de depósito para empréstimos.

#### **⭐⭐ Para Iniciados**

O Wrapper desempenha um papel crucial no ecossistema RMM:

- Padronizando a interface dos RealTokens para o protocolo RMM
- Permitindo estender a capacidade em número de tokens do RMM limitado inicialmente a 128 tokens, permite depositar 2000 tokens por Wrapper, ou aproximadamente 100x2000 tokens
- Para cada RealToken depositado, o Wrapper cria RTW (RealToken Wrapped) com um valor de 1$ por RTW, usado no RMM como depósito para empréstimos
- O wrapper cria e deposita tantos RTW quanto o valor (em $) dos RealTokens depositados.
  O depositante recebe tantos armmRTW (prova de depósito) quanto o valor de seus RealTokens depositados.
- O wrapper gerencia os saldos de RTW de acordo com o valor de cada RealToken criando ou queimando RTW para manter o valor depositado (em RTW) sempre igual ao valor depositado em RealToken.

![rmm_update.drawio.svg](/assets/img/rmm_update.drawio.svg)

## **2. Modificações Principais Trazidas ao Wrapper pela v3**

Endereço do wrapper: 0x10497611Ee6524D75FC45E3739F472F83e282AD5

Estas modificações são consequência da proposta de governança RIP00008 que foi aceita pela DAO em 2024-12-07 21:19 UTC.

[RIP00008 - Transfer RealToken Wrapper RMMv3 execution functions to the DAO](https://www.tally.xyz/gov/realtoken-ecosystem-governance/proposal/4412019256781079844728885554420538992900805587725535743224739658055634526928)

#### **⭐ Para Iniciantes**

As principais modificações ao Wrapper foram realizadas para garantir a conformidade legal e as operações rotineiras relacionadas ao ciclo de vida de um RealToken.

Em alguns casos, o emissor pode precisar destruir RealTokens:

- Hackeamento ou perda de acesso ao endereço do proprietário
- Falecimento do proprietário
- Venda da casa vinculada ao RealToken
- Reembolso de parte ou totalidade da dívida vinculada ao RealToken

o que poderia fazer com que o RMM não funcione corretamente para os RealTokens correspondentes, pois não haveria colateral para cobrir as dívidas contraídas.

Para cumprir estas obrigações e assegurar a continuidade e o bom funcionamento do RMM, novas funcionalidades foram adicionadas ao Wrapper para reembolsar parcial ou totalmente uma dívida vinculada a um RealToken em uma conta e extrair forçosamente o RealToken correspondente das contas afetadas.

Este processo é supervisionado pelo voto da DAO para assegurar que não haja abusos.
Garante que o montante devido pela destokenização seja corretamente recebido pelo proprietário, que o RMM possa continuar funcionando e cumprir com as obrigações legais.

#### **⭐⭐⭐ Para Especialistas**

Para manter um bom funcionamento do RMM, as modificações ao Wrapper foram realizadas para permitir recuperar os RealTokens vinculados a uma oferta em casos de falecimento do proprietário, venda da casa vinculada ao RealToken, reembolso de parte ou totalidade da dívida vinculada ao RealToken, destokenização ou outros eventos que tornem o ativo subjacente inelegível para depósito no RMM.

## **3. Riscos e Considerações**

#### **⭐ Para Iniciantes**

Estas modificações concedem um direito total sobre os tokens depositados no RMM, portanto é vital que qualquer submissão para ativar estas funcionalidades seja minuciosamente analisada pela DAO para assegurar que não haja abusos.  
Em princípio, apenas RealT ou um parceiro aprovado da DAO que emita tokens deveria iniciar tais propostas.  
A DAO deve assegurar que estas funcionalidades sejam ativadas apenas em casos que o requeiram expressamente para respeitar uma regulamentação ou garantir o bom funcionamento do RMM.

#### **⭐⭐ Para Iniciados**

O uso abusivo destas funcionalidades poderia levar a roubos de tokens, reembolsos de dívidas abusivos, empréstimos ilegítimos, comprometendo a segurança do RMM e seus usuários.

Os principais riscos relacionados ao Wrapper são:

- Proteção dos RealTokens depositados
- Prevenção de ataques potenciais
- Preservar o bom funcionamento do RMM
- Garantir o cumprimento das obrigações legais
- Poder devolver a propriedade dos RealTokens depositados ao usuário que tenha perdido acesso à sua conta

> É crucial entender que o Wrapper é um componente central do RMM e que qualquer modificação ou uso de suas funcionalidades deve ser cuidadosamente avaliado.
> {.is-warning}

## **4. ⭐⭐⭐ Informação Técnica**

### **4.1. repayForRecover**

#### **Parâmetros:**

- `repayWallet` (address): Endereço que possui os RealTokens no RMMv3 e dos quais os tokens serão extraídos do RMM sobre os quais uma dívida está ativa.
- `refundWallet` (address): Endereço que receberá qualquer excesso de pagamento após o reembolso da dívida.
- `percent` (uint256): Porcentagem (escala 100% = 10000) de cada token depositado que será extraído do RMM.
- `recoverAssets` (address[]): Array de endereços de tokens representando os RealTokens que serão extraídos do RMMv3.
- `debtAssets` (address[]): Array de endereços de tokens representando os ativos de dívida a serem reembolsados.
- `payer` (address): Endereço responsável pelo pagamento da dívida.

[Continua com todas as seções técnicas...]

## **5. Nota Importante:**

As funções `repayForRecover` e `recoverByGovernance` não gerenciam o Fator de Saúde (HF). Portanto, assegure-se previamente que o HF permite a retirada, caso contrário a proposta completa falhará.

A função `recoverByGovernance` não verifica as dívidas existentes. Isto deve ser antecipado na proposta. A função `repayForRecover` deve ser executada previamente se necessário para evitar falhas.

Os endereços que em algum momento recebem RealTokens devem cumprir estritamente com as regras do registro de conformidade destes tokens, caso contrário a proposta completa falhará.

## **6. Modificações**

Lista de modificações na atualização do RealTokenWrapper:

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

> IMPORTANTE: a validação de uma proposta que execute as funções `repayForRecover` e `recoverByGovernance` deve ser realizada com grande atenção, pois pode ser muito perigosa se não estiver corretamente configurada ou for usada com fins maliciosos.
> {.is-warning}

### Atualização 20 de dezembro 2024
**Objetivo**: Corrigir um bug relacionado à otimização de gas para a lista de tokens depositados.

**Problema encontrado**: Quando um usuário realiza uma liquidação e recebe aTokens, se estes aTokens não estiverem na lista de depósitos, as funções que utilizam esta lista não conseguem encontrar estes aTokens.
Os valores são registrados corretamente no Wrapper, os RTWs são criados e depositados adequadamente, mas o RMM e o Wrapper não conseguem encontrar os valores porque as funções `getAllTokenBalancesOfUser` e `getUserIndex` utilizam a variável `_tokenListOfUser` que não contém todas as informações.

**Solução**: Corrigir a função de liquidação para verificar se os tokens recuperados estão em `_tokenListOfUser`. Caso não estejam, eles são adicionados, permitindo que o Wrapper e RMM recuperem corretamente todas as informações.

Nova implementação implantada: https://gnosisscan.io/address/0xc7ca0b893c22f99bb99dfc9dafdb6a83e0e7a946#code

Modificações:
- Código adicionado linhas 312 a 317
https://gnosisscan.io/address/0xc7ca0b893c22f99bb99dfc9dafdb6a83e0e7a946#code#F1#L312

- Código adicionado linhas 357 a 362
https://gnosisscan.io/address/0xc7ca0b893c22f99bb99dfc9dafdb6a83e0e7a946#code#F1#L357

## **7. Auditoria**

Realizada pela empresa ADBK: https://github.com/abdk-consulting/audits/tree/main/realt
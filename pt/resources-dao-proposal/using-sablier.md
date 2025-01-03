---
title: Proposta usando o Sablier.com
description: 
published: true
date: 2025-01-03T12:44:31.949Z
tags: 
editor: markdown
dateCreated: 2025-01-03T12:44:31.949Z
---

# Criação de um stream Hourglass
O objetivo deste artigo é duplo:
- Qualquer proposta de votação (em contagem) deverá ser analisada ao pormenor, nomeadamente a parte que diz respeito ao código que vai ser executado.
O final do artigo ajudará a compreender o código da proposta: Libertação de REGs do orçamento DAO Team RealT.
- No futuro, os Proponentes poderão querer utilizar este exemplo para construir novas propostas no Tally.
O artigo começa com a apresentação dos parâmetros (no Tally e no Hourglass) para criar um fluxo Hourglass.

## Recursos

- site: https://sablier.com/
- documentos: https://docs.sablier.com/
- Documentos de aquisição: https://docs.sablier.com/apps/features/vesting
- Documentos sobre curvas: https://docs.sablier.com/concepts/lockup/stream-shapes#lockup-linear
- Contrato implementado do Docs: https://docs.sablier.com/guides/lockup/deployments#mainnets
- Exemplo de Sepolia: https://sepolia.etherscan.io/tx/0x86239fed5f924b61c7f5656e0e9322d7809097f2c2153b478fb38c07f85bdb4d
- exemplo de teste de rede de contagem: https://www.tally.xyz/gov/reg-dao-beta/proposal/6332954951702383415567855302130620076860949854692323526582374508844353542470

## Instruções comuns a todos os tipos de stream no Tally

1.º Crie uma nova proposta
1. Preencha o formulário de descrição da proposta
1. Adicione uma ação
 - ação personalizada
 - Contrato do token enviado
 - Utilize a ABI importada (se for bem-sucedida, caso contrário deverá fornecer a ABI)
 - Método de Contrato -> aprovar
1. Adicione uma ação
 - ação personalizada
 - Contrato Sablier SablierV2BatchLockup ([documentos de implementação](https://docs.sablier.com/guides/lockup/deployments))
 - Utilize a ABI importada (se for bem-sucedida, caso contrário deverá fornecer a ABI)
 - Método de contrato -> [Selecione o método adequado] (de preferência o que tem carimbos de data e hora são mais fáceis de configurar)
 - Calldatas -> [Introduza os parâmetros] a parte do lote pode ser complicada de completar, especialmente os elementos do tipo tulpe[]
1. Faça a simulação para verificar se está tudo bem

## Tipos de fluxo de ampulheta

Existem vários tipos de fluxos que oferecem diferentes comportamentos de desbloqueio:

### Fluxo Linear

- Distribuição de taxa constante por segundo
- A curva forma uma linha reta diagonal
- Ideal para distribuições simples e previsíveis

#### variante

Linear com Penhasco

- Semelhante ao streaming linear, mas com um período inicial de bloqueio
- Nenhum token está disponível durante o período de precipício
- Após o penhasco, a distribuição torna-se linear
- Perfeito para planos de aquisição com período de fidelização

Desbloqueio e Linear

- Semelhante ao fluxo linear, mas com desbloqueio inicial
- Vários tokens são desbloqueados instantaneamente
- O equilíbrio de distribuição torna-se linear
- Perfeito para planos de incentivos de curto e médio prazo

Desbloquear, Cliff e Linear

- Combinação de desbloqueio, cliff e linear
- Vários tokens são desbloqueados instantaneamente
- Após o primeiro desbloqueio, é aplicado um período de bloqueio
- O saldo de distribuição torna-se linear após o período de bloqueio
- Perfeito para planos de lançamento de tokens

### Fluxo Exponencial

- A distribuição acelera com o tempo
- Os beneficiários recebem mais tokens no final do período
- Ideal para lançamentos aéreos e incentivos a longo prazo

#### variante

Exponencial do Penhasco

- Combina três elementos:
 1. Um período de precipício (bloqueio inicial)
 2. Libertação instantânea de uma quantidade específica no final do penhasco
 3. Uma distribuição exponencial para o resto dos tokens
- Particularmente adequado para planos de aquisição de equipas
- Incentiva a retenção a longo prazo

### Bloqueio de tempo

- Bloqueia todos os tokens por um período definido
- Libertação total de uma só vez no final do período

### Desbloqueie por etapas

- Desbloqueio em passos regulares
- Permite lançamentos periódicos regulares
- Os tokens não reclamados acumulam-se

#### variante

Desbloquear mensalmente

- Nível mensal
- Desbloqueio mensal em data fixa
- Ideal para planos de processamento de salários ou ESOP

Peso traseiro

- Desbloqueio por etapas progressivas
- Rolamento com valor independente
- Os tokens não reclamados acumulam-se
- Ideal para planos de aquisição com rolamentos de desbloqueio

## Informações específicas

### Estrutura técnica dos segmentos

Esta estrutura é utilizada pelo fluxo dinâmico (LD)
Cada `segmento` num `fluxo dinâmico` é definido por três parâmetros:

- `amount`: quantidade de tokens a distribuir (uint128)
- `expoente`: ​​​​o expoente que define a curva de distribuição (UD2x18)
- `timestamp`: carimbo de data/hora final do segmento (uint40)

A fórmula de distribuição é a seguinte: f(x) = x^exp \* csa + Σ(esa)
Ou :

- x = tempo decorrido / tempo total do segmento
- exp = expoente do segmento
- csa = quantidade do segmento atual
- Σ(esa) = soma dos valores dos segmentos anteriores

#### Estrutura de tuplas

No Tally, a parte Batch das funções do tipo LD é composta por um campo `segments` que é um array do tipo `tuple[]`.
Note que as explicações seguintes são baseadas na função `createWithTimestampsLD`; para outras funções do tipo LD, terá de adaptar os tipos de tupla.

```solidez
estrutura Segmento {
 valor uint128;
 Expoente UD2x18;
 carimbo de data/hora uint40;
}
```

o campo `segments` na função `createWithTimestampsLD` é composto por 3 segmentos

- segmento 0: Representa o período entre o startTime e o final do segmento 0
- segmento 1: Representa o período compreendido entre o final do segmento 0 e o final do segmento 1
- segmento 2: Representa o período compreendido entre o final do segmento 1 e o final do fluxo

## Criação da ampulheta para o orçamento do Team RealT
Os seguintes parâmetros são os da proposta [RIP000xx] - Libertação de REGs do orçamento do Team RealT
O fluxo é Cliff Exponencial, o que significa que o fluxo começa após um período de penhasco e depois distribui os tokens exponencialmente, com desbloqueios maiores no final do período.

![payout_sablier_team_realt.png](/imag-en/payout_sablier_team_realt.png)

### Parâmetros para a função `createWithTimestampsLD`


Encontra a proposta no Tally: https://www.tally.xyz/gov/realtoken-ecosystem-governance/proposals

```
Função/Assinatura: createWithTimestampsLD(endereço,endereço,tupla[])
Alvo do contrato: 0x0F324E5CB01ac98b2883c8ac4231aCA7EfD3e750

Dados de chamada (lote):
lockupDynamic -> endereço: 0x555eb55cbc477Aebbe5652D25d0fEA04052d3971
activo -> endereço: 0x0AA1e96D2a46Ec6beB2923dE1E61Addf5F5f1dce
lote -> tupla[]:
 remetente -> endereço: 0x3f2d192F64020dA31D44289d62DB82adE6ABee6c
 destinatário -> endereço: 0x5Fc96c182Bb7E0413c08e8e03e9d7EFc6cf0B099
 quantidade -> uint256: 500000000000000000000000
 transferível -> bool: verdadeiro
 cancelável -> bool: falso
 startTime -> uint40: 1714521600
 segmentos -> tupla[]: [
 ["0", "100000000000000000", "1746057600"],
 ["5000000000000000000000", "100000000000000000", "1746057601"],
 ["49500000000000000000000", "300000000000000000", "1872288000"]
 ]
 corrector -> tupla [endereço, uint256]: [0x0000000000000000000000000000000000000, 0]
```

#### Detalhes do segmento

```json
[
 ["0", "100000000000000000", "1746057600"],
 ["5000000000000000000000", "100000000000000000", "1746057601"],
 ["49500000000000000000000", "300000000000000000", "1872288000"]
]
```

- segmento 0: ["0", "100000000000000000", "1746057600"]
- segmento 1: ["50000000000000000000000", "100000000000000000", "1746057601"]
- segmento 2: ["4950000000000000000000000", "300000000000000000", "1872288000"]

##### Explicações:

- segmento 0:

 - 0 = valor distribuído durante o segmento
 - 100000000000000000 = expoente do segmento = 1 em decimal (1000000000000000000 = 10^18)
 - 1746057600 = carimbo de data/hora final do segmento (1 de maio de 2025 00:00:00 UTC)

- segmento 1:

 - 5000000000000000000000 = valor distribuído durante o segmento (500k ^18)
 - 100000000000000000 = expoente do segmento = 1 em decimal (1000000000000000000 = 10 ^ 18)
 - 1746057601 = carimbo de data/hora do fim do segmento (1 de maio de 2025 00:00:01 UTC)

- segmento 2:

 - 495000000000000000000000 = valor distribuído durante o segmento (49,5 milhões ^18)
 - 300000000000000000 = expoente do segmento (300000000000000000 = 10^18)
 - 1872288000 = data/hora do fim do segmento (1 de maio de 2029 00:00:00 UTC)

**Segmento 1**, define uma data de fim do segmento com uma distribuição de 0 do parâmetro `startTime`, o que cria um período de bloqueio rígido sem distribuição.

**Segmento 2** define uma data de fim do segmento com uma distribuição de 500 mil tokens, que liberta uma soma definida em valor disponível instantaneamente na data de fim do segmento.

**Segmento 3**, define uma data final do segmento com uma distribuição de 49,5 milhões de tokens, que cria a curva exponencial desde o final do segmento 2 até à data final do segmento 3, irá distribuir o valor do valor ao longo do período com um expoente de 3
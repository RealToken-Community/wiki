---
title: Reivindicação REG
description: 
published: true
date: 2025-03-20T13:57:57.333Z
tags: 
editor: markdown
dateCreated: 2025-03-19T09:02:40.132Z
---

# Preâmbulo: Porque é que os REG lhe são atribuídos ⭐


Com cada tokenização de um ativo emitido no ecossistema, o DAO permite que os seus parceiros de tokenização aloquem um montante de USDREG (US$ convertível apenas em REG) aos clientes que compraram e mantiveram os tokens por uma duração variável, dependendo dos ativos. Estes USDREG são convertíveis em REG a qualquer momento ao preço de mercado.

Atualmente, o valor do USDREG corresponde às taxas cobradas pela RealT pelo trabalho de procura de imóveis e é alocado no momento da primeira reavaliação do imóvel.
A RealT anunciou a sua intenção de desenvolver os critérios para a concessão de direitos no USDREG, para serem aplicáveis ​​a todos os tipos de tokenizações que a RealT oferece e não apenas aos imóveis tradicionais (tokenização de empréstimos, por exemplo, que não tem reavaliação).

Antes da criação do REG, este valor era pago em tokens SOON, cujo valor era de 1 USDREG.

Assim que o pedido de reclamação do REG for implementado, o SOON desaparecerá e a alocação durante a primeira reavaliação será feita diretamente no USDREG (consulte RIP00009).

![avaliação.png](/imag-en/regconvertor/avaliação.png){.align-right .img40}

As reavaliações de imóveis são feitas por empresas independentes da RealT.
O custo desta ação está a tornar-se elevado (principalmente para os multifamiliares) e está a levar a RealT a adiar estas reavaliações, que originalmente deveriam ser anuais.

No futuro, a distribuição do USDREG já não deverá estar ligada à reavaliação, mas sim a um atraso após a tokenização.

# A aplicação para reivindicar o seu REG ⭐

[https://claim.realtoken.network/](https://claim.realtoken.network/)
![claim1.png](/imag-en/regconvertor/claim1.png){.align-right .img35}

1. A aplicação é executada no blockchain Gnosis,
2.º Precisa de ligar uma das seguintes carteiras:
- o declarado como receptor dos Realtokens, no [Realt.co](https://realt.co/) (no momento do lançamento da conversão): para reclamar os USDREG ($ convertíveis em REG) que correspondem aos Soon recebidos durante vários anos,
 - aquele que possui os Realtokens, que serão reavaliados posteriormente (assim que a distribuição de Soon for interrompida).
3.º Escolha o seu modo de apresentação (língua, fundo),
4.º Verifique se está em “REG Claim” e não em “Genesis”.

Nota: os utilizadores *sem carteira* terão de migrar para a *Realtoken Wallet ou outra carteira compatível* para fazer login na aplicação e, em seguida, pedir ao suporte para fazer o pedido na nova carteira dos tokens alocados à Walletless.
[Link de ajuda](https://community-realt.gitbook.io/tuto-community/site-realt/option-realtoken-wallet-account-abstraction)

![claim2.png](/imag-en/regconvertor/claim2.png){.align-right .img30}
Após efetuar login, verá:
1. O valor em USDREG que lhe está alocado (20 na imagem),
2.º A paridade do REG em $ no momento (1,93),
3.º O número correspondente do REG (10.34), que pode reclamar (na totalidade),
4. O montante de USDREG que já reivindicou (100), antes de uma nova atribuição,
5. A opção de delegação (ver próximo capítulo),
6. A opção de reclamação automática
 Pode autorizar a execução automática das reivindicações à sua atenção (por um robô), assim que os USDREGs lhe forem alocados (atraso máximo de 24 horas).
 Poderá ser cobrada uma taxa por este serviço (atualmente 0%).
<br>

## Reivindicar delegado

![delegateclaim.png](/imag-en/regconvertor/delegateclaim.png){.align-right .img45}

 É possível delegar outro endereço para reclamar e recolher o seu REG (por exemplo, no caso de uma carteira corrompida):
1. Ligação à aplicação com a carteira inicial (0x69…94a na imagem),
2.º Verifique a reclamação com outro endereço e indique o endereço delegado (0x…C08 na imagem). Em seguida, assine esta autorização de delegação com o endereço inicial.
3. A aplicação indica que deve alterar o seu endereço de login para reivindicar,
4. Ligue com o endereço delegado (0x…C08 na imagem),
5.º Inicie a reclamação,
6.º A reclamação enviará os REG, alocados ao endereço inicial, para o endereço delegado.
<br>

# Como funciona ⭐⭐

## Antes da criação do contrato de sinistro

Durante o período Soon (2021 a março de 2025) e antes da criação do contrato de sinistro REG (Vault):


1. No final de cada reavaliação de um Realtoken, a RealT lançava a Mint do Soon correspondente,
![ccm1.png](/imag-en/regconvertor/ccm1.png){.align-right .img50}
2.º Estes Soon foram alocados nas carteiras dos proprietários do Realtoken no momento da reavaliação.

Para inicializar o contrato de reclamação, os Soons são convertidos para USDREG e depois destruídos:
3.º O RealT lança a conversão de Soon,
4.º Recolha de todas as carteiras que possuam em breve com a sua quantidade,
5.º Em breve serão convertidos para USDREG,
Este valor para cada carteira é registado num ficheiro off-chain ([Merkel Tree)](https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json), os Soon convertidos são alocados a cada endereço no último endereço de entrega Realtoken conhecido no site realt.co do utilizador,
6.º Logo os tokens são destruídos (queimados).

Todas as ações acima referidas são executadas apenas uma vez, quando o contrato de sinistro é criado.

## Reclamação do utilizador

Depois de o ficheiro "Merkle Tree" ser inicializado (a primeira vez a partir dos Soons acumulados):

1.º O pedido de REG pode ser requerido:
 \- do requerimento de reclamação que exibe o número do USDREG,
 \- ou directamente no contrato da carteira do utilizador (solução mais complexa).
![ccm2.png](/imag-en/regconvertor/ccm2.png){.align-right .img50}
2. A reclamação é executada pelo contrato inteligente de conversão do Vault,
3.º Verifica a partir da raiz da Árvore Merkle, que lhe foi transmitida, se o utilizador tem os direitos correspondentes,
4.º Calcula o número de USDREG a partir dos dados da Árvore Merkle e do USDREG já reclamado (cujo valor armazena) para determinar o número de USDREG que pode ser reclamado, este valor é então convertido no número de REG do preço do Oracle,
5.º Solicita ao REG o contrato para criar (cunhar) os REG,
6.º Novos REG são transferidos para a carteira do utilizador.
7.As reavaliações/distribuições que se seguirão (após o desaparecimento do Soon) serão feitas diretamente no USDREG, através da atualização da Árvore Merkel. Esta atualização trará novos USDREGs para a aplicação reivindicar.
 O novo USDREG será alocado ao endereço que contém os Realtokens no momento do snapshot (diferentemente do direito ao USDREG da conversão Soon, que é alocado ao último endereço de entrega do RealToken registado na sua conta realt.co)

Se o endereço com USDREG tiver sido corrompido, é possível simplesmente assinar com o endereço corrompido delegando a reclamação noutro endereço. Este outro endereço poderá então solicitar os REG e recebê-los.

## Alegação “automática”

A execução da reclamação pelo contrato inteligente do Vault pode ser acionada:
- manualmente, em qualquer momento, pelo utilizador (sendo então escolhida a paridade),
- automaticamente por um autómato (bot) após a alocação do novo USDREG (a paridade não é então escolhida). Este último método requer autorização prévia.

1. A máquina monitorizará as atualizações da Árvore Merkel para utilizadores registados e, em seguida, desencadeará uma reivindicação REG para os utilizadores afetados,
![ccm3.png](/imag-en/regconvertor/ccm3.png){.align-right .img50}
2.º Dependendo da configuração do contrato inteligente de conversão do Vault, poderão ser aplicadas taxas de reclamação automáticas (0% inicialmente), a fim de incentivar a criação de tal autómato e cobrir as taxas de transação associadas (a ativação das taxas terá de ser objeto de uma votação de governação)
3. O Vault solicitará a cunhagem de duas quantias de REG: uma para a taxa e outra para o utilizador,
4. O contrato REG cunha as taxas REG na carteira ATM,
5. O contrato REG cunha o REG do utilizador na carteira do utilizador.

Nota: esta função não pode ser combinada com uma reivindicação delegada (que é semelhante a uma reivindicação do utilizador).

## Endereço

Contrato inteligente:

- Brevemente: [0xaa2c0cf54cb418eb24e7e09053b82c875c68bb88](https://gnosisscan.io/token/0xaa2c0cf54cb418eb24e7e09053b82c875c68bb88)
- REG: [0x0aa1e96d2a46ec6beb2923de1e61addf5f5f1dce](https://gnosisscan.io/token/0x0aa1e96d2a46ec6beb2923de1e61addf5f5f1dce)
- Cofre: [0x71eb2b2518556700f5b778b9cc313bfe8de5086e](https://gnosisscan.io/address/0x71eb2b2518556700f5b778b9cc313bfe8de5086e)
- Oracle: [0x86339b40e588f774bd766eB70D47bEFBe68B6F64](https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/avançado)

Árvore Merkel (OffChain): [https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json](https://github.com/real-token/vault-merkle-data/blob/main/dao/usdreg_convertion/current.json)

Este ficheiro pode ser guardado pela comunidade para permanecer disponível em todas as situações. A versão mais recente, cuja raiz está registada no contrato de reclamação, permanece válida mesmo que a interface ou o ficheiro de origem já não existam.

# Detalhes da operação do contrato do Vault ⭐⭐⭐

O endereço do contrato inteligente acima é o do Proxy (invariável) que aponta para a seguinte implementação (que pode variar, dependendo das atualizações): [https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1 d02cc1839031b6d2#code)

O programa gere a conversão e distribuição de tokens REG, com base nos valores de USDREG registados numa Árvore Merkel e no valor de REG fornecido pelo Oracle.

## Principais características

1. **Funções e permissões**: O contrato utiliza o sistema de controlo de acessos do OpenZeppelin para gerir diferentes funções (administrador, operador, pausador e atualizador) para proteger operações críticas.
2. **Gestão de tokens**: Interage com o token REG, permitindo aos utilizadores reivindicar tokens REG com base em valores USDREG, enquanto verifica as provas Merkle para garantir a elegibilidade.
3. **Reivindicar recursos**: os utilizadores podem reivindicar tokens diretamente ou através de um delegado. O contrato também gere recursos de reclamações automáticas. A reivindicação Walletless existe, mas não é implementada offChain: as contas Walletless terão de migrar para uma nova carteira.
4. **Gestão do registo de data e hora**: O contrato inclui mecanismos para verificar se as reclamações ocorrem dentro de um período de tempo definido, evitando reclamações fora desse período.
5. **Ponte e Transferências**: Permite também transferir tokens para outras cadeias (ponte) e gerir as taxas associadas a estas transferências.
6. **Atualizações**: O contrato foi concebido para ser atualizado através de um mecanismo de atualização, permitindo que novas funcionalidades sejam adicionadas ou bugs sejam corrigidos sem perder o estado do contrato.
7. **Eventos**: Emite eventos para rastrear ações importantes, como reclamações de tokens, atualizações de parâmetros e erros.

## Validação com prova de Merkel

A validação de prova de Merkle funciona verificando se a informação de um utilizador (endereço e valor) está incluída numa estrutura de dados chamada árvore de Merkle.

Execução no programa:

1. **Geração de folhas**: Para cada reivindicação, é gerada uma “folha” através do hash dos dados do utilizador (endereço e valor) com a função keccak256. Isto cria um hash único que representa essa afirmação.
![](/imag-en/regconvertor/rc2.png){.align-right .img50}

<br>

2. **Verificação de Prova**: A função *\_validateMerkleProof* recebe como entrada o endereço do utilizador, o valor, a raiz de Merkle esperada e uma matriz de provas de Merkle. Verifica se a prova Merkle é válida para o utilizador e o valor especificados.
![](/imag-en/regconvertor/rc1.png){.align-right .img50}
<br>
[Link para o código correspondente](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L669)
 <br>
 <br>

3. **Verificação da Árvore Merkle**: A função *\_verifyAsm* utiliza uma abordagem de montagem para percorrer provas Merkle. Pega em cada nó da prova e combina-o com a folha para reconstruir o hash até atingir a raiz de Merkle. Se a raiz reconstruída corresponder à raiz de Merkle esperada, significa que a afirmação é válida.
![](/imag-en/regconvertor/rc3.png){.align-right .img50}
<br>
[Link para o código correspondente](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L719)
<br>

4. **Rejeição de reclamações inválidas**: Se a verificação falhar, o contrato rejeita a reclamação lançando um erro, garantindo assim que apenas as reclamações válidas, que correspondem à estrutura da árvore Merkle, são aceites.


## MÃE

### Reclamação do utilizador (linhas 176 a 200)

- Função: *reivindicar*
- Descrição: Este modo permite a um utilizador reivindicar tokens REG com base num valor em USDREG.
- Processo :

 1. O utilizador chama a função de reivindicação com o valor que pretende reivindicar e uma prova de Merkle.
 2. A função verifica se o contrato não está em pausa e se os carimbos de data/hora são válidos.
 3.º Verifica se a prova Merkle é válida para o utilizador e o valor especificados.
 4.º Verifica o valor já reclamado pelo utilizador para evitar reivindicações duplas.
 5.Se todas as verificações forem aprovadas, o contrato calcula a quantidade de REG a emitir e realiza a cunhagem de tokens REG para o utilizador.

![](/imag-en/regconvertor/rc4.png){.align-right .img50}

[Link para o código correspondente](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L176)
<br>
<br>

### Reivindicação de um delegado (linhas 302 a 336)

- Função: *claimByDelegate*
- Descrição: Este modo permite que um utilizador designe um delegado para reivindicar tokens em seu nome.
- Processo :

 1. O utilizador fornece informações de reclamação (endereço da conta, valor, destinatário) e assinatura para comprovar a autorização.
 2. A função verifica se o contrato não está em pausa e se os registos de data e hora são válidos.
 3.º Valida a assinatura para garantir que o delegado está autorizado a agir em nome do utilizador.
 4.º Tal como no modo de reivindicação do utilizador, a prova Merkle é validada e o valor já reivindicado é verificado.
 5.Se tudo for válido, o contrato cunha os tokens REG para o destinatário.

> A interface para esta função envia os REG para o endereço delegado.
Numa futura atualização da interface: será possível delegar a reclamação, sem enviar os tokens para o delegado, mas para um endereço determinado no momento da assinatura.
{.is-warning}

![](/imag-en/regconvertor/rc5.png){.align-right .img50}
<br>
[Link para o código correspondente](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L302)
<br>
<br>


### Reclamação automática (linhas 552 a 640)

- Função: claimByAutoClaim
- Descrição: Este modo permite aos utilizadores reivindicar automaticamente os tokens REG sem ter de iniciar a reivindicação manualmente.
- Processo :

 1. O utilizador chama a função com uma matriz de estruturas de reivindicações.
 2. A função verifica se o contrato não está em pausa e se os carimbos de data/hora são válidos.
 3.º Para cada reivindicação, verifica se a auto-reivindicação está habilitada para o destinatário.
 4.º Valida a prova Merkle e verifica o valor já reclamado.
 5. Os tokens REG são cunhados para o destinatário e podem ser aplicadas taxas se configuradas.

![](/imag-en/regconvertor/rc6.png){.align-right .img50}

[Link para o código correspondente](https://gnosisscan.io/address/0x94223f067dbf9b43ed3bfea1d02cc1839031b6d2#code#F1#L552)
<br>
<br>

## REG Preço Oracle

O contrato REG Price Oracle é baseado no código Chainlink e inclui:

- o contrato inteligente [https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/advanced#code](https://gnosisscan.io/address/0x86339b40e588f774bd766eB70D47bEFBe68B6F64/ad vanced#code)
- uma infraestrutura Chainlink para automatizar pesquisas de preços on-chain e off-chain em diversas fontes e realizar um cálculo que visa evitar a manipulação de preços.
 O cálculo tem em conta os volumes de negociação, o tempo gasto num preço, as diferenças entre fontes de preço, cria uma ponderação para evitar manipulações, a taxa de atualização com o valor calculado é atualizada na cadeia a cada 24 horas por um autómato Chainlink.
 No gráfico abaixo pode ver o Preço Médio Ponderado pelo Tempo (TWAP) calculado pela Oracle em comparação com o preço spot do REG

![oracle_vs_spot.png](/imag-en/regconvertor/oracle_vs_spot.png)

### Porquê utilizar este método de cálculo para a conversão REG?

- **Atenuar a manipulação do mercado**: o TWAP calcula o preço ao longo de 24 horas, reduzindo o risco de manipulação em mercados ilíquidos como o REG, onde os preços spot podem ser facilmente distorcidos por pequenas transações.

- **Evitar espirais descendentes**: Ao não reagir imediatamente às quedas de preços, o TWAP estabiliza o mercado e evita as vendas por pânico, o que pode levar a uma espiral descendente de tokens ilíquidos.

- **Garantir a estabilidade de preços**: O TWAP oferece uma taxa de conversão constante com base em médias diárias, protegendo o REG da volatilidade do preço spot, especialmente em ambientes de baixa liquidez. Garante justiça ao assegurar que todos os utilizadores podem, ao longo de um período de tempo, obter a mesma quantidade de REG para um determinado crédito.

- **Apoio ao auto-balanceamento para proteger o REG**: o TWAP ajusta a emissão do REG com base nos movimentos de preços; os utilizadores recebem menos REG quando os preços descem e mais REG quando os preços sobem.

- **Desenhado para conversão**: O motor foi concebido para permitir que os utilizadores escolham quando converter, em vez de servir como uma ferramenta de negociação em tempo real.
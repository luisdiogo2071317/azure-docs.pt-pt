---
title: Modelo de solução Ethereum consortium prova de trabalho
description: Utilize o modelo de solução Consortium Etherereum prova de trabalho para implementar e configurar uma rede de Ethereum consortium com vários membros
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/21/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 0ea9bd2c7d23aa227e62858983e7170b771751da
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655472"
---
# <a name="ethereum-proof-of-work-consortium-solution-template"></a>Modelo de solução Ethereum consortium prova de trabalho

O modelo de solução Consortium Ethereum prova de trabalho foi concebido para torna mais fácil e mais rápida implementar e configurar uma rede de Ethereum consortium com vários membros com o mínimo de conhecimento do Azure e Ethereum.

Com alguns a entradas do utilizador e uma implementação de clique único através do portal do Azure, cada membro pode aprovisionar os seus requisitos de espaço de rede, utilizando o Microsoft Azure de computação, redes e serviços de armazenamento em todo o mundo. Requisitos de espaço de rede de cada membro é composto por um conjunto de nós de transação com balanceamento de carga com que uma aplicação ou o utilizador pode interagir para submeter transações, um conjunto de nós de extração para transações de registos e um gateway de VPN. Um passo de ligação subsequentes liga os gateways para criar uma rede totalmente configurado com vários membros blockchain.

## <a name="about-blockchain"></a>Sobre blockchain

Os dos que são novos para a Comunidade blockchain, a versão desta solução é uma excelente oportunidade para saber mais sobre a tecnologia de forma fácil e configurável no Azure. No entanto, para começar, é recomendável implementar mais simples autónomo Ethereum topologia da rede com estas instruções orientadas, antes de conceção de redes com vários membros consortium.

### <a name="mining-node-details"></a>Detalhes do nó de extração

Nós mine transações são separados de nós que aceitam transações para se certificar de que as duas ações não a competir pelos mesmos recursos.

Um membro de consortium pode aprovisionar até cinco regiões que contém um ou mais nós de extração, associadas a um disco gerido. Um ou mais nós na região estão configurados como um nó de arranque para suportar a capacidade de deteção dinâmica de nós na rede. Nós de extração comunicam com outros nós de extração fique para consenso sobre o estado de ledger distribuída subjacente. Não é necessário para a sua aplicação ter em consideração ou comunicam com estes nós. Ao concentrar-se em redes privadas, os nós de extração estão isolados do tráfego de internet públicas entrada para adicionar uma camada de proteção secundária. Tráfego de saída for permitido, mas não a porta de deteção Ethereum.

Todos os nós têm uma versão estável do cliente aceda Ethereum (Geth) e estão configurados para serem nós de extração. Se não forneceu um bloco de genesis personalizado, todos os nós de utilizam o mesmo endereço de Ethereum e o par de chaves que está protegido por palavra-passe a Ethereum conta. A frase de acesso de Ethereum fornecida é utilizado para gerar a conta predefinida (coinbase) para cada nó de extração. Como nós de extração, mine, recolher taxas que são adicionadas a esta conta.

O número de nós de extração por cada membro de consortium depende o tamanho geral à rede pretendida e a quantidade de energia hash dedicada a cada membro. Quanto maior for a rede, os mais nós que precisam de ser comprometido para obter uma vantagem desleal. O modelo suporta até 15 extração nós por região aprovisionados utilizando conjuntos de dimensionamento de máquina virtual.

### <a name="transaction-node-details"></a>Detalhes do nó de transação

Um membro de consortium tem também um conjunto de nós de transação com balanceamento de carga. Estes nós são acessíveis a partir de fora da rede virtual, para que as aplicações podem utilizar estes nós para submeter transações ou executar contratos inteligentes dentro da rede blockchain. Todos os nós têm uma versão estável do cliente aceda Ethereum (Geth) e estão configurados para manter uma cópia completa de ledger de distribuída. Se não for fornecido um bloco de genesis personalizado, estes nós utilizam a mesma conta de Ethereum, protegida pela palavra-passe da conta de Ethereum.

Nós de transação estão com balanceamento de carga dentro de um conjunto de disponibilidade para manter a elevada disponibilidade. O modelo suporta até cinco nós de transação aprovisionados utilizando conjuntos de dimensionamento de máquina virtual.

### <a name="log-analytics-details"></a>Detalhes de análise do registo

Cada implementação também cria uma nova instância de análise de registos ou pode ser associada a uma instância existente. Isto permite a monitorização de várias métricas de desempenho de cada máquina virtual que constitui a rede implementada.

## <a name="deployment-architecture"></a>Arquitetura de implementação

### <a name="description"></a>Descrição

Este modelo de solução pode implementar única rede ou de várias transmissões com base na região membro Ethereum consortium. A rede virtual de cada região está ligada a outra região uma topologia de cadeia com os gateways VNET e recursos de ligação. É também Aprovisiona uma entidade de registo, que contém as informações necessárias de todos os nós de transação e Miner implementado em cada região.

### <a name="standalone-and-consortium-leader-overview"></a>Descrição geral da leader autónoma e consortium

![Autónoma e a descrição geral de Leader Consortium](./media/ethereum-deployment-guide/leader-overview.png)

### <a name="joining-consortium-member-overview"></a>Descrição geral de membro de consortium associar

![Descrição geral de membro de Consortium associar](./media/ethereum-deployment-guide/member-overview.png)

## <a name="getting-started"></a>Introdução

Este processo requer uma subscrição do Azure que suporta a implementação de vários conjuntos de dimensionamento de máquinas virtuais e discos geridos pelo. Se necessário, crie uma conta gratuita do Azure para iniciar.

Depois de uma subscrição está protegida, aceda ao portal do Azure. Selecione **+ criar um recurso**, Marketplace (ver todos os) e procure **Ethereum prova do trabalho Consortium**.

A implementação do modelo irá guiá-lo a configurar os requisitos de espaço do primeiro membro na rede. O fluxo de implementação está dividido nas cinco passos: Noções básicas, o Operations Management Suite, o regiões de implementação, o tamanho de rede e o desempenho, Ethereum definições.

### <a name="basics"></a>Noções básicas

Em **Noções básicas**, especifique os valores de parâmetros padrão para qualquer implementação, tais como a subscrição, o grupo de recursos e propriedades de máquinas virtuais básica.

![Noções básicas](./media/ethereum-deployment-guide/sample-deployment.png)

Nome do Parâmetro|Descrição| Valores Permitidos|Valores predefinidos
---|---|---|---
Criar uma nova rede ou rede existente associação?|Criar uma nova rede ou associar uma rede consortium já existente|Criar novo existente de associação|Criar Nova
Implementar uma rede que irá fazer parte de um consortium?|Permite a uma rede consortium implementações futuras associar esta rede (visível quando *criar novo* é selecionado acima)|Consortium autónomo|Autónomo
Prefixo de recursos |Cadeia utilizada como base para atribuição de nomes de recursos (2 a 4 apenas carateres alfanuméricos). Um valor hash exclusivo é prepended para a cadeia para alguns dos recursos, enquanto as informações específicas do recurso são acrescentadas.|Apenas carateres alfanuméricos com comprimento 2 a 4|ND
Nome de utilizador VM| Nome de utilizador de administrador de cada VM implementado (apenas carateres alfanuméricos)|1 - 64 carateres |gethadmin
Tipo de autenticação|O método de autenticação para a máquina virtual. |Chave pública de palavra-passe ou SSH|Palavra-passe
Palavra-passe (tipo de autenticação = a palavra-passe)|A palavra-passe da conta de administrador para cada uma das máquinas virtuais implementadas. A palavra-passe tem de conter 3 dos seguintes requisitos: 1 maiúscula, 1 minúscula, 1 número e 1 caráter especial. <br />Enquanto todas as VMs inicialmente têm a mesma palavra-passe, pode alterar a palavra-passe após o aprovisionamento.|12 - 72 carateres|ND
A chave SSH (tipo de autenticação = chave pública)|A chave de secure shell utilizada para início de sessão remoto.|| ND
Subscrição| A subscrição para o qual pretende implementar a rede consortium||ND
Grupo de Recursos| O grupo de recursos para o qual pretende implementar a rede consortium.||ND
Localização| A região do Azure para o grupo de recursos. ||ND



### <a name="operations-management-suite"></a>Operations Management Suite

O painel do Operations Management Suite (OMS) permite-lhe configurar um recurso do OMS para a sua rede. Irá recolher OMS e superfície métricas úteis e os registos da sua rede, que fornece a capacidade de verificar rapidamente o estado de funcionamento de rede ou depuração emite. A oferta livre do OMS falhará corretamente quando for atingida a capacidade.

![Criar novo OMS](./media/ethereum-deployment-guide/new-oms.png)

Nome do Parâmetro|Descrição| Valores Permitidos|Valores predefinidos
---|---|---|---
Ligar ao OMS existente|Criar uma nova instância de análise de registos ou adicionar uma instância existente|Criar novo existente de associação|Criar a nova localização de análise do registo|A região onde a análise de registos nova será implementado (ficará visível se for *criar nova* está selecionado)
Id de área de trabalho existente do OMS|ID da área de trabalho da instância existente (ficará visível se for *associar existente* está selecionado) camada de serviço do OMS|Escolha o escalão de preço para a nova instância. Obter mais informações em https://azure.microsoft.com/pricing/details/log-analytics/ (ficará visível se for *associar existente* está selecionado)|Autónomo livre por nó|Gratuito
Chave primária existente do OMS|A chave primária utilizada para ligar à instância do OMS existente (ficará visível se for *associar existente* está selecionado)

### <a name="deployment-regions"></a>Regiões de implementação

Em seguida, em **regiões de implementação**, especifique as entradas de **diversas nas regiões** para implementar a rede de consortium e a seleção das regiões do Azure com base no número de regiões fornecido. Utilizador pode implementar no máximo, cinco regiões.

![Regiões de implementação](./media/ethereum-deployment-guide/deployment-regions.png)

Nome do Parâmetro| Descrição| Valores Permitidos |Valores predefinidos
---|---|---|---
Número de nas regiões| Número de regiões para implementar a rede consortium|1, 2, 3, 4, 5| 2
Primeiro região| Região primeiro para implementar a rede consortium|Permitido todas as regiões do Azure| EUA Oeste
Segundo região |Segundo região para implementar a rede de consortium (Visible apenas quando o número de regiões é selecionado como 2)|Permitido todas as regiões do Azure| EUA Leste
Terceiro região| Terceiro região para implementar a rede de consortium (Visible apenas quando o número de regiões é selecionado como 3)|Permitido todas as regiões do Azure| EUA Central
Quarta região| Região quarta para implementar a rede de consortium (Visible apenas quando o número de regiões é selecionado como 4)|Permitido todas as regiões do Azure| EUA Leste 2
Região quinta| Região quinta para implementar a rede de consortium (Visible apenas quando o número de regiões é selecionado como 5)|Permitido todas as regiões do Azure| EUA Oeste 2

### <a name="network-size-and-performance"></a>Tamanho de rede e o desempenho

Em seguida, em **tamanho e o desempenho de rede** especifique entradas para o tamanho da rede consortium, tais como o número e tamanho de nós de extração e nós de transação.

![Tamanho de rede e o desempenho](./media/ethereum-deployment-guide/network-size-performance.png)

Nome do Parâmetro |Descrição |Valores Permitidos| Valores predefinidos
---|---|---|---
Número de nós de extração|O número de nós de extração implementado por região|2 - 15| 2
Desempenho de armazenamento do nó de extração|O tipo de disco gerido fazer uma cópia de cada um de nós extração implementado.|Standard ou Premium|Standard
Tamanho de máquina virtual do nó de extração|O tamanho de máquina virtual utilizado para nós de extração.|A padrão, <br />D padrão, <br />Standard D-v2, <br />Série de F padrão, <br />DS padrão, <br />e FS padrão|D1v2 padrão
Número de nós de transação com balanceamento de carga|O número de nós de transação para aprovisionar como parte da rede.|1 - 5| 2
Desempenho de armazenamento do nó de transação|O tipo de disco gerido fazer uma cópia de cada um de nós transação implementado.|Standard ou Premium|Standard
Tamanho de máquina virtual do nó de transação|O tamanho de máquina virtual utilizado para nós de transação.|A padrão, <br />D padrão, <br />Standard D-v2, <br />Série de F padrão, <br />DS padrão, <br />e FS padrão|D1v2 padrão

### <a name="ethereum-settings"></a>Definições de Ethereum

Em seguida, em **Ethereum definições**, especifique as definições de configuração relacionados com Ethereum, como a rede ID e Ethereum conta palavra-passe ou genesis bloco.

![Definições de Ethereum](./media/ethereum-deployment-guide/standalone-leader-deployment.png)

Nome do Parâmetro |Descrição |Valores Permitidos|Valores predefinidos
---|---|---|---
ID de ConsortiumMember|O ID associado a cada membro a participar numa rede consortium utilizada para configurar os espaços de endereços IP para evitar a colisão. <br /><br />ID de membro deve ser exclusivo em organizações diferentes na mesma rede. Um ID de membro exclusivo é necessário, mesmo quando a mesma organização implementa em várias regiões.<br /><br />Anote o valor deste parâmetro, uma vez que terá de partilhar com outros membros joining.|0 - 255
ID de rede Ethereum|O ID de rede para a rede de Ethereum consortium a ser implementado. Cada rede Ethereum tem as suas próprias ID de rede, com 1 a ser o ID para a rede pública. Enquanto o acesso à rede está restringido para nós de extração, ainda Recomendamos que utilize um grande número para evitar colisões.|5 - 999,999,999| 10101010
Bloco de genesis personalizado|Opção para automaticamente gerar um bloco de genesis ou forneça um personalizado.|Sim/Não| Não
Palavra-passe da conta de Ethereum (bloco de genesis personalizada = não)|A palavra-passe de administrador utilizada para proteger a conta de Ethereum importada para cada nó. A palavra-passe tem de conter o seguinte: 1 maiúscula, 1 minúscula e 1 número.|12 ou mais carateres|ND
Ethereum frase de acesso privado chave (bloco de genesis personalizada = não)|A frase de acesso utilizada para gerar a chave privada de ECC associada à conta Ethereum predefinido que é gerada. Uma chave privada pré-geradas não precisa de ser transmitida explicitamente.<br /><br />Considere uma frase de acesso com randomness suficiente para garantir uma chave privada forte e sem sobreposição com outros membros de consortium. A frase de acesso tem de conter o seguinte no mínimo: 1 maiúscula, 1 minúscula e 1 número.<br /><br />Tenha em atenção de que o se dois membros de utilizam a mesma frase de acesso as contas geradas será o mesmo. A mesma frase de acesso é útil se uma única organização está a tentar implementar em regiões e pretende partilhar uma única conta (coin base) em todos os nós.|12 ou mais carateres|ND
Bloco de Genesis (bloco de genesis personalizada = Yes)|Cadeia JSON que representa o bloco de genesis personalizado. Pode encontrar mais detalhes sobre o formato do bloco genesis aqui, em redes personalizada.<br /><br />Ainda é criada uma conta de Ethereum quando fornecer um bloco de genesis personalizado. Considere especificar uma conta de Ethereum prefunded no bloco de genesis não aguardar a extração.|JSON válido |ND
Chave para a ligação partilhada|Uma chave partilhada para a ligação entre os gateways VNET.| 12 ou mais carateres|ND
URL de dados de Consortium|O URL a apontar para os dados de configuração relevantes consortium fornecidos pela implementação de outro membro. <br /><br />Esta informação é fornecida por um membro já ligado que tenha uma implementação. Se tiver implementado o resto da rede, o URL é a saída de modelos de implementação, com o nome CONSORTIUM dados.||ND
Gateway de VNet para ligar a|O caminho do recurso do Gateway de VNet para o qual pretende ligar.<br />Esta informação é fornecida por um membro já ligado que tenha uma implementação. Se tiver implementado o resto da rede, o URL é no resultado de implementação do modelo, com o nome CONSORTIUM_MEMBER_GATEWAY_ID. Nota: Consortium Gateway de VNet e URL de recurso de dados o mesmo membro tem de ser utilizado.||ND
Ponto final de entidade de registo de informações de ponto a ponto|Ponto final de informações de elemento fornecido pela implementação de outro membro|Ponto final válido do primeiro membro no consortium|ND
Entidade de registo de informações de chave de ponto a ponto|Chave de principal de informações de elemento fornecido pela implementação de outro membro|Chave primária válida do primeiro membro no consortium|ND

### <a name="summary"></a>Resumo

Clicar no painel de resumo para rever as entradas especificadas e executa a validação de pré-implementação básica.

![Resumo](./media/ethereum-deployment-guide/summary.png)

Reveja os termos legais e privacidade e clique em **Compra** para implementar. Se a implementação tem mais de uma região, ou para uma rede consortium, em seguida, este modelo previamente implementa os gateways de VPN necessários para suportar a conetividade de rede com outros membros. Implementação do gateway pode demorar até 45 minutos 50.

## <a name="post-deployment-sanity-checks"></a>Após a implementação sanity verificações

### <a name="administrator-page"></a>Página de administrador

Depois de concluída a implementação com êxito e todos os recursos aprovisionados, pode aceder à página de administrador para obter uma vista da sua rede blockchain e sanity verificar o estado de implementação. O URL da página Administração é o nome DNS do Balanceador de carga; está presente na secção de saída da implementação do modelo com o nome como ADMIN_SITE.

Para localizar, selecione o grupo de recursos que foi implementado. Em seguida, selecione **descrição geral**e clique na ligação imediatamente em **implementações** que mostra o número que tiveram êxito.

![Descrição geral do grupo de recursos](./media/ethereum-deployment-guide/resource-overview.png)

Novo ecrã mostra o histórico de implementação. Selecione o primeiro recurso de implementação (por exemplo, microsoft-azure-blockchain.azure-blockchain-secundária do servi …) e procure o **saídas** secção na metade inferior da página. Verá o URL da página Administração listada no parâmetro de saída de implementação de modelo como ADMIN_SITE.

![Implementações de recursos](./media/ethereum-deployment-guide/resource-deployments.png)

![Saída de implementação](./media/ethereum-deployment-guide/deployment-output.png)

Para obter a página de administrador, copie o **SITE da administração** de saída e abri-lo noutro separador.

Na página Administração, pode obter uma descrição geral de alto nível da topologia que implementou, revendo a secção de estado do nó de Ethereum. Inclui todos os nomes de anfitrião do nó, maior número de elementos de rede e o bloco mais recente visto. A contagem de ponto a ponto para cada nó é entre o mínimo de um menor que o *total de contagem de nós* e a contagem de ponto a ponto máximo configurado. Tenha em atenção que a contagem de elemento de rede não restringe o número de nós que pode ser implementado na rede.
Ocasionalmente, poderá ver a contagem de ponto a ponto flutuam e ser inferior a (número total de nós - 1). A diferença na contagem de nem sempre é um sinal de que os nós estão danificados, uma vez que bifurcações na razão podem provocar a pequenas alterações em contagem de ponto a ponto. Por fim, pode inspecionar o bloco mais recente visualizado por cada nó na rede para determinar bifurcações ou lags no sistema.

![Estado do nó](./media/ethereum-deployment-guide/node-status.png)

O estado do nó é atualizado a cada 10 segundos. Recarregar a página através do browser ou **recarregar** botão para atualizar a vista.

### <a name="oms-portal"></a>Portal do OMS

Pode localizar o portal do OMS seguindo a hiperligação na saída de implementação (OMSPORTALURL) ou selecionando o recurso do OMS no seu grupo de recursos implementados.

![URL DO OMS](./media/ethereum-deployment-guide/oms-url.png)

![Recursos do OMS](./media/ethereum-deployment-guide/oms-resource.png)

O portal primeiro irá apresentar estatísticas de alto nível de rede como uma descrição geral.

![Descrição geral do OMS](./media/ethereum-deployment-guide/oms-overview.png)

Clicar na descrição geral irá direcioná-lo para um portal para ver as estatísticas de cada nó.

![Estatísticas de por nó](./media/ethereum-deployment-guide/per-node-stats.png)

### <a name="accessing-nodes"></a>Ao aceder a nós

Pode ligar remotamente para as máquinas virtuais para os nós de transação através de SSH com a chave de nome de utilizador e palavra-passe/SSH do administrador fornecido. Uma vez que o nó de transação VMs tem os seus próprios endereços IP públicos, terá de percorrer o Balanceador de carga e especifique o número de porta. O comando SSH para executar o acesso ao primeiro nó de transação está listado no parâmetro de saída de implementação de modelo como, **SSH_TO_FIRST_TX_NODE** (para a implementação de exemplo: ssh -p 4000 gethadmin@leader4vb.eastus.cloudapp.azure.com). Para obter nós de transações adicionais, incrementar o número de porta por um (por exemplo, o primeiro nó de transação está na porta 4000).

Uma vez que as máquinas virtuais nos quais os nós de extração executam não estão acessíveis externamente, tem de passar por um de nós de transação. Assim que tiver uma sessão SSH para um nó de transação, instale a chave privada no nó de transação ou utilizar a palavra-passe para iniciar uma sessão SSH em qualquer de nós de extração.

**Nota**

Os nomes de anfitriões podem ser obtidos a partir do Site de administração ou do portal do Azure. No portal do Azure, os nomes de anfitriões de nós presentes no recurso de (VMSS) do conjunto de dimensionamento de máquina virtual está listado em **instâncias**, que difere de acordo com os nomes de anfitrião real. Por exemplo, o nome de anfitrião no portal do Azure poderá aparentar ser **mn-asdfmv-reg1_0** , mas o nome do anfitrião real seria como **mn-asdfmv-reg**.

Por exemplo:

Nome de anfitrião de portal do Azure| Nome do anfitrião real
---|---
Mn-ethwvu-reg1_0| Mn-ethwvu-reg1000000
Mn-ethwvu-reg1_1 |Mn-ethwvu-reg1000001
Mn-ethwvu-reg1_2 |Mn-ethwvu-reg1000002

## <a name="adding-a-new-consortium-member"></a>Adicionar um novo membro de consortium

### <a name="sharing-data"></a>Partilha de dados

Como primeiro membro (ou um membro ligado) da consortium, tem de fornecer outros membros de alguns tipos de informações para que podem associar e estabelecer ligação. Especificamente:

1. **Partilhar dados de configuração Consortium**: há um conjunto de dados que são utilizados para orquestrar a ligação de Ethereum entre dois membros. As informações necessárias, incluindo o bloco de genesis, ID de rede consortium e nós de arranque, são escritas num ficheiro em nós de transação do leader ou outro membro implementado. A localização deste ficheiro é listada no parâmetro de saída de implementação de modelo com o nome **CONSORTIUM dados**.
2. **Elemento de ponto final de informações**: ponto final de entidade de registo de informações do elemento de rede para obter informações de todos os nós já ligado à rede Ethereum os líderes das ou implementação de outro membro. Os arquivos de base de dados de um conjunto de informações sobre cada nó ligado na rede, informações como o nome de anfitrião do nó, privada endereço IP etc. Este é o parâmetro de saída de implementação de modelo com o nome **PEER_INFO_ENDPOINT**.
3. **Chave primária da informação de elemento**: chave primária do ponto a ponto informações entidade de registo é utilizada para aceder à chave do informações de elemento principal do leader ou outro membro. Este é o parâmetro de saída de implementação de modelo com o nome **PEER_INFO_PRIMARY_KEY**.


4. **Gateway de VNET**: cada membro estabelece uma ligação à rede blockchain completa através de um membro existente. Para ligar a VNET, terá do caminho do recurso para o Gateway de VNET do membro ao qual está a ligar. Este é o parâmetro de saída de implementação de modelo com o nome **CONSORTIUM_MEMBER_GATEWAY_ID**.
5. **Chave partilhada**: A pré-estabelecida secreta entre dois membros da rede consortium for estabelecer uma ligação. Esta é uma cadeia alfanumérica (entre 1 e 128 carateres) que é acordada fora do contexto da implementação. (Por exemplo, **MySharedKeyAbc123**)

### <a name="acceptance-of-new-member"></a>Aceitação dos novo membro

Este passo deve ser feito depois do membro joining foi implementado com êxito respetiva rede. Antes de um membro pode associar a rede e ver o tráfego de transação, um membro existente tem de efetuar uma configuração final no seu Gateway de VPN para aceitar a ligação. Isto significa que os nós de Ethereum do membro joining não serão executado até é estabelecida uma ligação. Esta configuração pode ser efetuada através do PowerShell ou xPlat CLI. Um script CLI do PowerShell módulo e xPlat também são armazenadas no nó de transação, juntamente com os dados de consortium. Os parâmetros de saída de implementação com o nome de é a localização do script **par-GATEWAY-PS-MODULE** e **par-GATEWAY-AZURE-CLISCRIPT**, respetivamente.

**Configuração da CLI do PowerShell**

Informações adicionais sobre como começar com os cmdlets do PowerShell do Azure e xPlat do Azure CLI pode ser encontrado na documentação do Azure.

Terá da versão mais recente dos cmdlets do Azure instalados localmente e uma sessão aberta. Certifique-se iniciar sessão para a sessão com as suas credenciais de subscrição do Azure.

**PowerShell: Estabelecer a ligação**

Transferir o módulo do PowerShell e guarde-o localmente. A localização do módulo do PowerShell está especificada como a **par-GATEWAY-PS-MODULE** parâmetro de saída de implementação do modelo.

Se ainda não estiver ativada, utilize o **Set-ExecutionPolicy** cmdlet na sessão local permitir a execução de um módulo não assinado.

**Set-ExecutionPolicy irrestrito CurrentUser**

Início de sessão seguinte, a sua subscrição do Azure no qual implementou a implementação de Leader utilizando

**Login-AzureRmAccount**

Em seguida, importe o módulo de:

**Import-Module <filepath to downloaded file>**

Por fim, execute a função com a entrada adequada:

- **MyGatewayResourceId** : caminho do recurso do seu Gateway. Este é o parâmetro de saída de implementação de modelo com o nome **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId** : caminho do recurso do gateway do membro joining. Isto é fornecido pelo joining membro e é o parâmetro de saída de implementação do modelo de também denominado **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **ConnectionName** : um nome para identificar esta ligação de Gateway.
- **Chave partilhada** : O segredo previamente estabelecido entre os dois membros de rede consortium, que são estabelecer uma ligação.

**CreateConnection** -MyGatewayResourceId <resource path of your Gateway> - OtherGatewayResourceId < caminho do recurso do gateway do membro joining > - ConnectionName Aminhaligação - SharedKey "MySharedKeyAbc123"

**xPlat CLI: estabelecer ligação**

Transferir o script da CLI do Azure e armazene-o localmente. A localização do script CLI do Azure está especificada no parâmetro de implementação de modelo com o nome **par-GATEWAY-AZURE-CLI-SCRIPT**.

Execute o script com a entrada adequada:

- **MyGatewayResourceId** : caminho do recurso do seu Gateway. Este é o parâmetro de saída de implementação de modelo com o nome **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId** : caminho do recurso do gateway do membro joining. Isto é fornecido pelo joining membro e é o parâmetro de modelo de implementação da implementação deles também denominada **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **ConnectionName** : um nome para identificar esta ligação de Gateway.
- **Chave partilhada** : O segredo previamente estabelecido entre os dois membros de rede consortium, que são estabelecer uma ligação.
- **Localização** : região do Azure em que o recurso do gateway é implementado.

``` powershell
az network vpn-connection create --name $ConnectionName --resource-group
$MyResourceGroup --vnet-gateway1 $MyGatewayResourceId --shared-key $SharedKey --vnet-
gateway2 $OtherGatewayResourceId --enable-bgp
```

A arquitetura será o seguinte após ter configurado com êxito a ligação entre **leader** e **membro** implementações.

![Arquitetura LEADER e membro](./media/ethereum-deployment-guide/leader-member.png)

## <a name="fund-new-member-account"></a>Nova conta de membro de fundo

### <a name="generated-genesis-block"></a>Bloco de genesis gerado

Como primeiro membro, a sua conta Ethereum é financiada com um trillion ether se a implementação gera o bloco de genesis (personalizada Genesis bloco = não). Outros membros terão uma conta que não é previamente financiada e tem de aguardar a acumular Ether como os respetivos nós de extração começam a mine blocos. Para evitar que os novos membros aguardar Ether, terá de explicitamente fund contas de Ethereum membros associar.

Para tal, os membros de associação tem de fornecer, com a conta de Ethereum que é apresentada no respetivo site de administração. Em seguida, pode utilizar o seu site de administração para transferir Ether da sua conta para a respetiva conta ao introduzir a conta fornecida.

### <a name="custom-genesis-block"></a>Bloco de genesis personalizado

Se não for fornecido um bloco de genesis personalizado com uma conta de Ethereum especificada, pode utilizar MetaMask especificados, ou outra ferramenta de transferência ether da conta para a conta de Ethereum pré-geradas visível no Web site de administração. Para obter instruções sobre como utilizar MetaMask, consulte [criar conta de Ethereum](#create-ethereum-account).

Se um bloco de genesis personalizado é fornecido uma conta ou não tem acesso a quaisquer contas pré-alocados, terá de aguardar até que os nós de extração começam a mine para gerar Ether na sua conta (coin base). Como rapidamente as funds são gerados dependem o nível de dificuldade em que especificar no bloco de genesis personalizado.

## <a name="create-ethereum-account"></a>Criar conta Ethereum

Para criar uma conta adicional, pode utilizar uma variedade de soluções. Um essa solução é MetaMask, uma extensão do Chrome que fornece um cofre de identidade e ligação a uma rede de Ethereum pública, teste ou personalizado. MetaMask formulates uma transação para registar a conta na rede.

Esta transação, tal como qualquer outra transação, passará para um de nós de transação e, eventualmente, mined para um bloco conforme ilustrado abaixo.

![Nó de transação](./media/ethereum-deployment-guide/transaction-node.png)

Para instalar a extensão no Chrome, aceda a personalizar e controlar o Google Chrome (botão de sobrecarga) > ferramentas mais > extensões > obter mais as extensões e procure MetaMask.

![Extensão de MetaMask](./media/ethereum-deployment-guide/metamask-extension.png)

Depois de instalado, abra MetaMask e crie um novo cofre. Por predefinição, o Cofre será ligado à rede de teste Morden. Terá de alterar esta opção para ligar à rede consortium privada implementado, especificamente ao balanceador de carga à frente de nós de transação. O resultado de modelo, obter o expostas Ethereum RPC ponto final em porta 8545, com o nome como `ETHEREUM-RPC-ENDPOINT`e introduza-o no RPC personalizado conforme mostrado abaixo.

![Definições de MetaMask](./media/ethereum-deployment-guide/metamask-settings.png)

Ao criar o cofre, crie um carteira quando que contém uma conta. Para criar contas adicionais, selecione **comutador contas** e, em seguida, o **+** botão.

![MetaMask criar conta](./media/ethereum-deployment-guide/metamask-create-account.png)

### <a name="initiate-initial-ether-allocation"></a>Iniciar a alocação de Ether inicial

Através da página de administrador, pode formular uma transação para transferir Ether da conta do pré-alocados para outra conta Ethereum. Esta transferência Ether é uma transação que é enviada para o nó de transação e mined para um bloco, conforme ilustrado abaixo.

![Nó de transação](./media/ethereum-deployment-guide/transaction-node-mined.png)

Através do ícone da área de transferência de carteira MetaMask quando, copie o endereço da conta Ethereum ao qual pretende transferir ether e voltar para a página de administrador. Cole o campo de entrada para transferir 1000 ether da conta do Ethereum previamente atribuída à sua conta recentemente criada a conta copiada. Clique em **submeter** e aguarde que a transação para mined para um bloco.

![Estado do nó](./media/ethereum-deployment-guide/node-status2.png)

Depois da transação será aprovada para um bloco mined, o saldo da conta na MetaMask para a sua conta refletirá a transferência de 1000 Ether.

![Transferência de MetaMask](./media/ethereum-deployment-guide/metamask-transfer.png)

### <a name="transfer-of-ether-between-accounts"></a>Transferência de Ether entre contas

Neste momento, está pronto para executar transações dentro da sua rede privada consortium. A transação mais simples é transferência Ether a partir de uma conta para outra. Para formular essa uma transação, pode utilizar MetaMask novamente, transferir o dinheiro da conta do primeiro utilizada acima para uma segunda conta.

De **carteira quando 1** no MetaMask, clique em enviar. Copie o endereço de carteira segundo quando criado para **endereço do destinatário** entrada campo e a quantidade de Ether para transferência no **quantidade** campo de entrada. Clique em **enviar** e aceite a transação.

![Transação de envio de MetaMask](./media/ethereum-deployment-guide/metamask-send.png)

Novamente, quando a transação é mined e consolidada para um bloco, os saldos de conta são refletidos em conformidade. Nota, **carteira quando 1**do saldo é deducted Ether mais do que 15, uma vez que tinha aos quais deve prestar uma taxa de extração para processar a transação.

![Carteira quando 1](./media/ethereum-deployment-guide/wallet1.png)

![Carteira quando 2](./media/ethereum-deployment-guide/wallet2.png)

## <a name="next-steps"></a>Passos Seguintes

Agora está pronto para se focarem em aplicações e desenvolvimento de contrato inteligente contra a sua rede de blockchain consortium privada.

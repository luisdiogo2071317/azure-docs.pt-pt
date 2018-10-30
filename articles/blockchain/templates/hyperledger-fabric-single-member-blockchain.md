---
title: Consórcio de Recursos de Infraestrutura do Hyperledger
description: Utilizar o modelo de solução Hyperledger Consortium de recursos de infraestrutura para implementar e configurar uma rede de membro único
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/29/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: coborn
manager: femila
ms.openlocfilehash: c08557156848d4e7fcf0b1adbe6c8faa4ee00c82
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50231377"
---
# <a name="hyperledger-fabric-single-member-network"></a>Rede de membro único Hyperledger recursos de infraestrutura

Pode utilizar o modelo de solução Hyperledger Consortium de recursos de infraestrutura para implementar e configurar uma rede do Hyperledger Fabric membro único (com vários nó).

Depois de ler este artigo, irá:

- Obter o conhecimento de trabalho de blockchain, Hyperledger recursos de infraestrutura e mais complicado consortium arquiteturas de rede
- Saiba como implementar e configurar uma rede de consórcio da infraestrutura de Hyperledger único membro, no portal do Azure

## <a name="about-blockchain"></a>Sobre blockchain

Se estiver familiarizado com a Comunidade de blockchain, este modelo de solução é uma ótima oportunidade para saber mais sobre a tecnologia de maneira fácil e configurável no Azure. Blockchain é a tecnologia subjacente Bitcoins; No entanto, é muito mais do que apenas um ativador para uma moeda virtual. É uma composição de base de dados existente, o sistema distribuído e tecnologias de criptografia, que permite que o cálculo de múltiplos intervenientes seguro com garantias para imutabilidade, capacidade de verificação, auditability e resiliência a ataques. Protocolos diferentes empregam mecanismos diferentes para fornecer esses atributos. [Recursos de infraestrutura Hyperledger](https://github.com/hyperledger/fabric) é um protocolo desse tipo.

## <a name="consortium-architecture-on-azure"></a>Arquitetura de Consortium no Azure

Este modelo implementa uma topologia para ajudar a testar e simular produção para os utilizadores dentro de uma única organização (membro único). Esta implementação é composto por uma rede de vários nó numa única região, em breve para ser expandida para várias regiões.

A rede é composta por três tipos de nós:

1. **Nó de membro**: um nó a executar o serviço de associação de recursos de infraestrutura que registra e gere os membros da rede. Este nó pode ser em cluster para escalabilidade e elevada disponibilidade; No entanto neste laboratório, será utilizado um nó de membro único.
2. **Nós orderer**: transações atômicas ou difusão de encomenda de um nó a executar o serviço de comunicação implementando uma garantia de entrega, como o total.
3. **Configurar o peering entre nós**: um nó em que consolida as transações e mantém o estado e uma cópia do livro razão distribuído.

## <a name="getting-started"></a>Introdução

Para começar, terá uma subscrição do Azure que suporta a implementação de várias máquinas virtuais e contas de armazenamento standard. Se não tiver uma subscrição do Azure, pode [criar uma conta gratuita do Azure](https://azure.microsoft.com/free/).

Por predefinição, a maioria dos tipos de subscrição suportam para uma topologia de implementação pequenos sem a necessidade de aumentar a quota. Terá da implementação mais pequena possível para um membro:

- 5 de máquinas virtuais (5 núcleos)
- 1 VNet
- Balanceador de 1 carga
- 1 endereço IP público

Assim que tiver uma subscrição, vá para o [portal do Azure](https://portal.azure.com). Selecione **+**, selecione **Blockchain**e selecione **Hyperledger Fabric único membro Blockchain**.

![Modelo do mercado de Blockchain de membro único do Hyperledger recursos de infraestrutura](./media/hyperledger-fabric-single-member-blockchain/marketplace-template.png)

## <a name="deployment"></a>Implementação

Para começar, selecione o **Hyperledger Fabric único membro Blockchain** e clique em **Create** para abrir o **Noções básicas** painel no assistente.

A implementação do modelo explica como configurar a rede de vários nó. O fluxo de implementação é dividido em três passos: Noções básicas, configuração de rede e a configuração de recursos de infraestrutura.

### <a name="basics"></a>Noções básicas

Na **Noções básicas** painel, especifique os valores de parâmetros padrão para qualquer implementação. Como, por exemplo de subscrição, grupo de recursos e basic virtual machine propriedades.

![Noções básicas](./media/hyperledger-fabric-single-member-blockchain/basics.png)

Nome do Parâmetro| Descrição| Valores Permitidos|Valor Predefinido
---|---|---|---
**Prefixo de recursos**| Uma cadeia de caracteres usada como base para atribuir nomes os recursos implementados.|6 carateres ou menos|ND
**Nome de utilizador VM**| O nome de utilizador do administrador para cada uma das máquinas virtuais implementadas para este membro.|1 - 64 carateres|azureuser
**Tipo de autenticação**| O método para autenticar para a máquina virtual.|Chave pública de palavra-passe ou SSH|Palavra-passe
**Palavra-passe (tipo de autenticação = a palavra-passe)**|A palavra-passe da conta de administrador para cada uma das máquinas virtuais implementadas. A palavra-passe tem de conter três dos seguintes tipos de carateres: 1 caráter em maiúsculas, 1 carater em minúsculas, 1 número e 1 caráter especial.<br /><br />Embora todas as VMs tenham inicialmente a mesma palavra-passe, pode alterar a palavra-passe após o aprovisionamento.|12 - 72 carateres|ND
**Chave SSH (tipo de autenticação = a chave pública)**|A chave secure shell utilizada para início de sessão remoto.||ND
**Restringir o acesso por endereço IP**|Definição para determinar se o acesso para cliente do ponto final é restrito ou não o tipo.|Sim/Não| Não
**Permitido o endereço IP ou sub-rede (restringir o acesso por endereço IP = Yes)**|O endereço IP ou o conjunto de endereços IP que está autorizado a aceder ao ponto de final do cliente quando o controlo de acesso está ativado.||ND
**Subscrição** |A subscrição para a implementação.
**Grupo de Recursos** |O grupo de recursos ao qual pretende implementar a rede de consórcio.||ND
**Localização** |A região do Azure ao qual pretende implementar o primeiro membro * * os requisitos de espaço de rede de s.

### <a name="network-size-and-performance"></a>Tamanho de rede e o desempenho

Em seguida, no **tamanho e o desempenho, de rede** especificar entradas para o tamanho da rede de consórcio. Por exemplo, o número de associação, orderer e nós de ponto a ponto. Escolha as opções de infraestrutura e o tamanho da máquina virtual.

![Tamanho de rede e o desempenho](./media/hyperledger-fabric-single-member-blockchain/network-size-performance.png)

Nome do Parâmetro| Descrição| Valores Permitidos|Valor Predefinido
---|---|---|---
**Número de nós de associação**|O número de nós que executam o serviço de associação. Para obter detalhes adicionais sobre o serviço de associação, examinar de segurança e serviços de associação sob o Hyperledger [documentação](https://media.readthedocs.org/pdf/hyperledger-fabric/latest/hyperledger-fabric.pdf).<br /><br />Este valor está atualmente limitado a 1 nó, mas pretendemos suportar o aumento horizontal através de clustering na revisão da seguinte.|1| 1
**Número de nós de Orderer** |O número de nós que solicite (organizar) transações num bloco.--> Esta instrução é prolixo e confuso. Para obter detalhes adicionais sobre o serviço de ordenação, visite o Hyperledger [documentação](https://hyperledger-fabric.readthedocs.io/en/release-1.1/ordering-service-faq.html).<br /><br />Este valor está atualmente limitado a 1 nó. |1 |1
**Número de nós de ponto a ponto**| Nós que pertencem os membros de consórcio que executar transações e mantêm o estado e uma cópia do razão.<br /><br />Para obter detalhes adicionais sobre o serviço de ordenação, visite o Hyperledger [documentação](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html).|3| 3 - 9
**Desempenho de armazenamento**|O tipo de armazenamento de segurança cada um de nós implementados. Para saber mais sobre o armazenamento, visite [introdução ao armazenamento do Microsoft Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction) e [o armazenamento Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage).|Standard ou Premium|Standard
**Tamanho da máquina virtual** |O tamanho de máquina virtual utilizado para todos os nós na rede|Standard A,<br />Standard D,<br />D Standard-v2,<br />Série F Standard,<br />Standard DS,<br />e o Standard FS|D1_v2 padrão

### <a name="fabric-specific-settings"></a>Definições específicas de recursos de infraestrutura

Por fim, em **definições de recursos de infraestrutura**, especifique as definições de configuração relacionadas com recursos de infraestrutura.

![Definições de recursos de infraestrutura](./media/hyperledger-fabric-single-member-blockchain/fabric-settings.png)

Nome do Parâmetro| Descrição| Valores Permitidos|Valor Predefinido
---|---|---|---
**Nome de utilizador de arranque de configuração**| O utilizador autorizado inicial que será registado com os serviços de membro da rede implementada.|9 ou menos carateres|administrador
**Palavra-passe de utilizador de bootstrap para recursos de infraestrutura de AC**|A palavra-passe de administrador utilizada para proteger a conta de recursos de infraestrutura de AC importada para o nó de associação.<br /><br />A palavra-passe tem de conter uma maiúscula, um caráter em minúsculas e um número.|12 ou mais carateres|ND

### <a name="deploy"></a>Implementação

Na **resumo**, reveja as entradas especificadas e para executar a validação de pré-implementação básica.

![Resumo](./media/hyperledger-fabric-single-member-blockchain/summary.png)

Reveja os termos legais e privacidade e clique em **Compra** para implementar. Dependendo do número de VMs a ser aprovisionado, o tempo de implantação pode variar de alguns minutos para dezenas de minutos.

### <a name="accessing-nodes"></a>Aceder a nós

Assim que a implementação estiver concluída, uma **descrição geral** é apresentado.

![Implementações](./media/hyperledger-fabric-single-member-blockchain/deployments.png)

Se o ecrã não aparecer automaticamente (talvez porque mover em torno do portal de gestão durante a implementação em execução), sempre pode encontrá-lo no separador grupos de recursos na barra de navegação esquerda. Clique no nome do grupo de recursos que introduziu no passo 1 para ir para o **descrição geral** página.

Descrição geral apresenta todos os recursos que foram implementados pelo modelo de solução. Pode explorá-los à vontade, mas este ecrã também pode aceder a _parâmetros de saída_ gerado pelo modelo. Esses parâmetros de saída fornecerá informações úteis ao ligar à sua rede de recursos de infraestrutura Hyperledger.

Para acessar os parâmetros de saída, primeiro clique nas **implementações** separador no painel do grupo de recursos. O histórico de implementação é apresentado.

![Histórico de implementações](./media/hyperledger-fabric-single-member-blockchain/deployment-history.png)

A partir do histórico de implementação, clique na primeira implementação na lista para ver os detalhes.

![Detalhes de implementação](./media/hyperledger-fabric-single-member-blockchain/deployment-details.png)

O ecrã de detalhes mostrará a um resumo da implementação, seguido de três parâmetros de saída úteis:

- O _ponto final de API_ pode ser utilizado depois de implementar uma aplicação na rede.
- O _PREFIXO_ , também conhecido como _prefixo de implementação_ , exclusivamente identifica os recursos e a implementação. Será utilizado ao utilizar as ferramentas com base da linha de comandos.
- O _SSH-para-FIRST-VM_ dá a um previamente montado comando ssh com todos os parâmetros corretos necessários para ligar a primeira VM na sua rede; Para recursos de infraestrutura Hyperledger, é o nó do Fabric-CA.

Pode ligar remotamente para as máquinas virtuais para cada nó através de SSH com a sua chave de nome de utilizador e palavra-passe/SSH de administrador fornecida. Uma vez que as VMs de nó não tem seus próprios endereços IP públicos, terá de passar pelo balanceador de carga e especifique o número de porta. O comando SSH para aceder ao primeiro nó de transação é o terceiro resultado de modelo, * * SSH-para-FIRST-VM (para a implementação de exemplo: `sh -p 3000 azureuser@hlf2racpt.northeurope.cloudapp.azure.com`). Para aceder a nós de transações adicionais, incrementar o número de porta por um (por exemplo, o primeiro nó de transação é na porta 3000, a segunda é no 3001, o terceiro é no 3002, etc.).

## <a name="next-steps"></a>Passos Seguintes

Agora está pronto para se concentrar na aplicação e desenvolvimento de chaincode em relação a sua rede de blockchain Hyperledger consortium.

---
title: Consortium Hyperledger recursos de infraestrutura
description: Utilize o modelo de solução Hyperledger Consortium de recursos de infraestrutura para implementar e configurar uma rede de membro único
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/21/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 7b60c086896506e5883607db48a64d2a2efbd967
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655449"
---
# <a name="hyperledger-fabric-single-member-network"></a>Rede de membro único Hyperledger recursos de infraestrutura

Pode utilizar o modelo de solução Hyperledger Consortium de recursos de infraestrutura para implementar e configurar uma rede de (com vários nós) de membro único Hyperledger recursos de infraestrutura.

Depois de ler este artigo, irá:

- Obter conhecimento prático das blockchain, Hyperledger recursos de infraestrutura e consortium mais complicada arquiteturas de rede
- Saiba como implementar e configurar uma único membro Hyperledger recursos de infraestrutura consortium rede a partir no portal do Azure

## <a name="about-blockchain"></a>Sobre blockchain

Se estiver familiarizado com a Comunidade blockchain, esta é uma excelente oportunidade para saber mais sobre a tecnologia de forma fácil e configurável no Azure. Blockchain é a tecnologia subjacente atrás Bitcoin; No entanto, é muito mais do que apenas um ativador para uma moeda virtual. É um compostos de base de dados existente, distribuído do sistema e as tecnologias de criptografia que ativa o cálculo de múltiplos intervenientes seguro com garantias em torno immutability, verifiability, auditability e resiliência a ataques. Protocolos diferentes utilizam diferentes mecanismos para fornecer estes atributos. [Recursos de infraestrutura Hyperledger](https://github.com/hyperledger/fabric) é um protocolo desses.

## <a name="consortium-architecture-on-azure"></a>Arquitetura de Consortium no Azure

Este modelo implementa uma topologia para ajudar a testar e simular a produção para os utilizadores dentro de uma única organização (único membro). Compreende esta implementação de uma rede de vários nós numa única região, em breve para expandir de forma a várias regiões.

A rede é composta por três tipos de nós:

1. **Nó de membro**: um nó executa o serviço de associação de recursos de infraestrutura que regista e gere os membros da rede. Este nó, eventualmente, pode ser agrupado para escalabilidade e elevada disponibilidade, no entanto, neste laboratório, será utilizado um nó único do membro.
2. **Nós orderer**: transações de difusão ou atómicas de ordem de um nó que executa o serviço de comunicação implementar uma garantia de entrega, tais como total.
3. **Elemento nós**: um nó que consolida transações e mantém o estado e de uma cópia de ledger distribuída.

## <a name="getting-started"></a>Introdução

Para começar, terá de uma subscrição do Azure que suporta a implementação de várias máquinas virtuais e contas de armazenamento standard. Se não tiver uma subscrição do Azure, pode [criar uma conta do Azure gratuita](https://azure.microsoft.com/free/).

Por predefinição, a maioria dos tipos de subscrição suporta uma topologia de implementação pequenos sem necessidade de aumentar a quota. Terá da implementação mais pequena possível para um membro:

- 5 máquinas virtuais (5 núcleos)
- 1 VNet
- Balanceador de 1 carga
- 1 endereço IP público

Assim que tiver uma subscrição, vá para o [portal do Azure](https://portal.azure.com). Selecione **+**, selecione **Blockchain**e selecione **Hyperledger recursos de infraestrutura único membro Blockchain**.

![Modelo de Hyperledger recursos de infraestrutura único membro Blockchain Marketplace](./media/hyperledger-fabric-single-member-blockchain/marketplace-template.png)

## <a name="deployment"></a>Implementação

Para começar, selecione o **Hyperledger recursos de infraestrutura único membro Blockchain** e clique em **criar**. Esta ação irá abrir o **Noções básicas** painel no assistente.

A implementação do modelo irá guiá-lo a configurar a rede de vários nós. O fluxo de implementação está dividido em três passos: Noções básicas, configuração de rede e a configuração de recursos de infraestrutura.

### <a name="basics"></a>Noções básicas

Sob o **Noções básicas** painel, especifique os valores de parâmetros padrão para qualquer implementação, tais como a subscrição, o grupo de recursos e propriedades de máquinas virtuais básica.

![Noções básicas](./media/hyperledger-fabric-single-member-blockchain/basics.png)

Nome do Parâmetro| Descrição| Valores Permitidos|Valor Predefinido
---|---|---|---
**Prefixo de recursos**| Uma cadeia utilizada como base para atribuir nomes aos recursos implementados.|6 carateres ou menos|ND
**Nome de utilizador VM**| O nome de utilizador do administrador para cada uma das máquinas virtuais implementadas para este membro.|1 - 64 carateres|azureuser
**Tipo de autenticação**| O método de autenticação para a máquina virtual.|Chave pública de palavra-passe ou SSH|Palavra-passe
**Palavra-passe (tipo de autenticação = a palavra-passe)**|A palavra-passe da conta de administrador para cada uma das máquinas virtuais implementadas. A palavra-passe tem de conter 3 dos seguintes: 1 maiúscula, 1 minúscula, 1 número e 1 caráter especial.<br /><br />Enquanto todas as VMs inicialmente têm a mesma palavra-passe, pode alterar a palavra-passe após o aprovisionamento.|12 - 72 carateres|ND
**A chave SSH (tipo de autenticação = chave pública)**|A chave de secure shell utilizada para início de sessão remoto.||ND
**Restringir o acesso ao endereço IP**|Definição para determinar o tipo se o acesso de cliente do endpoint é restrito ou não.|Sim/Não| Não
**Permitido endereço IP ou sub-rede (restringir o acesso ao endereço IP = Yes)**|O endereço IP ou o conjunto de endereços IP que está autorizado a aceder ao ponto de final do cliente quando o controlo de acesso está ativado.||ND
**Subscrição** |A subscrição para o qual pretende implementar.
**Grupo de Recursos** |O grupo de recursos para o qual pretende implementar a rede consortium.||ND
**Localização** |A região do Azure para o qual pretende implementar o primeiro membro * * s rede os requisitos de espaço.

### <a name="network-size-and-performance"></a>Tamanho de rede e o desempenho

Em seguida, em **tamanho e o desempenho,** especifique entradas para o tamanho da rede consortium, como o número de associação, orderer e nós de ponto a ponto. Escolha as opções de infraestrutura e o tamanho da máquina virtual.

![Tamanho de rede e o desempenho](./media/hyperledger-fabric-single-member-blockchain/network-size-performance.png)

Nome do Parâmetro| Descrição| Valores Permitidos|Valor Predefinido
---|---|---|---
**Número de nós de associação**|O número de nós que executam o serviço de associação. Para obter detalhes adicionais sobre o serviço de associação, observe segurança & associações sob o Hyperledger [documentação](https://media.readthedocs.org/pdf/hyperledger-fabric/latest/hyperledger-fabric.pdf).<br /><br />Este valor é atualmente restrito para 1 nó, mas planeamos suportar de escalamento horizontal através de clustering na revisão seguinte.|1| 1
**Número de nós de Orderer** |O número de nós de ordem (organizar) transações num bloco.--> Esta instrução é palavras e confuso. Para obter detalhes adicionais sobre o serviço de ordenação, visite o Hyperledger [documentação](http://hyperledger-fabric.readthedocs.io/en/latest/orderingservice.html).<br /><br />Este valor é atualmente restrita para 1 nó. |1 |1
**Número de nós de ponto a ponto**| Nós que pertencem ao membros consortium que executar transações e mantém o estado e de uma cópia de ledger.<br /><br />Para obter detalhes adicionais sobre o serviço de ordenação, visite o Hyperledger [documentação](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html).|3| 3 - 9
**Desempenho de armazenamento**|O tipo de armazenamento de cópia de cada um de nós implementado. Para mais informações sobre o armazenamento, visite [introdução ao Storage do Microsoft Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction) e [armazenamento Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage).|Standard ou Premium|Standard
**Tamanho da máquina virtual** |O tamanho de máquina virtual utilizado para todos os nós na rede|A padrão,<br />D padrão,<br />Standard D-v2,<br />Série de F padrão,<br />DS padrão,<br />e FS padrão|D1_v2 padrão

### <a name="fabric-specific-settings"></a>Definições específicas de recursos de infraestrutura

Por fim, em **definições de recursos de infraestrutura**, especifique as definições de configuração relacionados com recursos de infraestrutura.

![Definições de recursos de infraestrutura](./media/hyperledger-fabric-single-member-blockchain/fabric-settings.png)

Nome do Parâmetro| Descrição| Valores Permitidos|Valor Predefinido
---|---|---|---
**Nome de utilizador de arranque de configuração**| O utilizador autorizado inicial que será registado com os serviços do membro na rede implementado.|9 ou menos carateres|Admin
**Palavra-passe de utilizador de arranque de configuração para recursos de infraestrutura de AC**|A palavra-passe de administrador utilizada para proteger a conta de recursos de infraestrutura de AC importada para o nó de associação.<br /><br />A palavra-passe tem de conter uma maiúscula, um caráter em minúsculas e um número.|12 ou mais carateres|ND

### <a name="deploy"></a>Implementação

No **resumo**, reveja as entradas especificadas e executa a validação de pré-implementação básica.

![Resumo](./media/hyperledger-fabric-single-member-blockchain/summary.png)

Reveja os termos legais e privacidade e clique em **Compra** para implementar. Dependendo do número de VMs que está a ser aprovisionados, tempo de implementação pode variar entre alguns minutos para dezenas de minutos.

### <a name="accessing-nodes"></a>Ao aceder a nós

Assim que a implementação estiver concluída, um **descrição geral** é apresentado.

![Implementações](./media/hyperledger-fabric-single-member-blockchain/deployments.png)

Se o ecrã não aparecer automaticamente (talvez porque moveu em torno do portal de gestão durante a implementação estava em execução), pode encontrá-lo sempre no separador de grupos de recursos na barra de navegação esquerda. Clique no nome do grupo de recursos que introduziu no passo 1 para ir para o **descrição geral** página.

Descrição geral apresenta todos os recursos que foram implementados pelo modelo de solução. Pode explorá-los à vontade, mas neste ecrã também pode aceder a _parâmetros de saída_ gerado pelo modelo. Estes parâmetros de saída irão dar-lhe informações úteis quando ligar à sua rede de recursos de infraestrutura Hyperledger.

Para os parâmetros de saída de acesso, primeiro clique no **implementações** separador no painel do grupo de recursos. O histórico de implementação é apresentado.

![Histórico de implementações](./media/hyperledger-fabric-single-member-blockchain/deployment-history.png)

No histórico de implementação, clique na primeira implementação, na lista para ver os detalhes.

![Detalhes da implementação](./media/hyperledger-fabric-single-member-blockchain/deployment-details.png)

O ecrã de detalhes mostrará um resumo da implementação, seguido de três parâmetros de saída úteis:

- O _ponto final de API_ pode ser utilizado depois de implementar uma aplicação na rede.
- O _PREFIXO_ , também chamado _prefixo implementação_ , exclusivamente identifica os recursos e a implementação. Será utilizado ao utilizar as ferramentas de com base da linha de comandos.
- O _SSH a primeira VM_ fornecem que previamente assembled comando ssh com todos os parâmetros corretos necessárias para ligar a primeira VM na sua rede; no caso de recursos de infraestrutura Hyperledger, será o nó de AC de recursos de infraestrutura.

Pode ligar remotamente para as máquinas virtuais para cada nó através de SSH com a chave de nome de utilizador e palavra-passe/SSH do administrador fornecido. Uma vez que o nó de VMs não têm os seus próprios endereços IP públicos, terá de percorrer o Balanceador de carga e especifique o número de porta. O comando SSH para o primeiro nó de transação de acesso é o terceiro resultado de modelo, * * SSH a primeira VM (para a implementação de exemplo: `sh -p 3000 azureuser@hlf2racpt.northeurope.cloudapp.azure.com`). Para obter nós de transações adicionais, incrementar o número de porta por um (por exemplo, o primeiro nó de transação está na porta 3000, o segundo é no 3001, é o terceiro no 3002, etc.).

## <a name="next-steps"></a>Passos Seguintes

Agora está pronto para focar-se no desenvolvimento de chaincode contra a sua rede de blockchain consortium Hyperledger e aplicações.

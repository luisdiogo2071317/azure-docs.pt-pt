---
title: Modelo de solução Ethereum de pilha do Azure
description: Utilizar modelos de solução personalizada para implementar e configurar uma rede de Ethereum consortium na pilha do Azure
services: azure-stack
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 6/28/2018
ms.topic: article
ms.service: azure-stack
ms.reviewer: coborn
manager: femila
ms.openlocfilehash: 4c2b0cda2d4144cde733f7f57ac6311e1a69f547
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114736"
---
# <a name="azure-stack-ethereum-solution-templates"></a>Modelos de solução Ethereum de pilha do Azure

O modelo de solução Ethereum foi concebido para torna mais fácil e mais rápida implementar e configurar uma rede de Ethereum consortium com vários membros com o mínimo de conhecimento do Azure e Ethereum.

Com alguns a entradas do utilizador e uma implementação de clique único através do portal de administrador de pilha do Azure, cada membro pode aprovisionar os seus requisitos de espaço de rede. Requisitos de espaço de rede de cada membro é composto por um conjunto de nós de transação com balanceamento de carga com que uma aplicação ou o utilizador pode interagir para submeter transações, um conjunto de nós de extração para transações de registos e um dispositivo de rede Virtual (NVA). Um passo de ligação subsequentes liga NVAs para criar uma rede totalmente configurado com vários membros blockchain.

## <a name="prerequisites"></a>Pré-requisitos

Transferir o seguinte [do Marketplace](azure-stack-download-azure-marketplace-item.md):

* Ubuntu Server 16.04 LTS versão 16.04.201802220
* Windows Server 2016 
* Script personalizado para o Linux 2.0 
* Extensão de Script Personalizado 

Para obter mais informações sobre os cenários de blockchain no Azure, consulte [modelo de solução de prova do trabalho consortium Ethereum](../blockchain-workbench/ethereum-deployment-guide.md).

Não é necessária uma subscrição do Azure que pode suportar a implementação de várias máquinas virtuais. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

## <a name="deployment-architecture"></a>Arquitetura de implementação

Este modelo de solução pode implementar única ou várias rede de consortium Ethereum membro. A rede virtual está ligada uma topologia de cadeia com recursos de aplicação Virtual de rede e a ligação. 

## <a name="deployment-use-cases"></a>Casos de utilização de implementação

O modelo pode implementar Ethereum consortium para leader e associação de membro de diversas formas, seguem-se ter testámos as:
- Numa pilha do Azure com vários nós, com o Azure AD ou AD FS, implementar oportunidades potenciais e o membro com a mesma subscrição ou com subscrições diferentes.
- Na pilha de Azure nó único (com o Azure AD) implemente oportunidades potenciais e o membro com a mesma subscrição.

### <a name="standalone-and-consortium-leader-deployment"></a>Implementação de leader autónomo e consortium

O modelo de leader consortium configura os requisitos de espaço do primeiro membro na rede. 

1. Transferir o [modelo leader a partir do GitHub](https://raw.githubusercontent.com/seyadava/AzureStack-QuickStart-Templates-1/blockchain_nva/eth/marketplace/ConsortiumLeader/mainTemplate.json)
2. No portal de administração de pilha do Azure, selecione **novo > implementação do modelo** para implementar a partir de um modelo personalizado.
3. Selecione **Editar modelo** para editar o novo modelo personalizado.
4. No painel de edição no lado direito, copie e cole o modelo de leader JSON que transferiu anteriormente.
    
    ![Editar modelo leader](media/azure-stack-ethereum/edit-leader-template.png)

5. Selecione **Guardar**.
6. Selecione **Editar parâmetros** e conclua os parâmetros de modelo para a sua implementação.
    
    ![Editar os parâmetros do modelo leader](media/azure-stack-ethereum/edit-leader-parameters.png)

    Nome do Parâmetro | Descrição | Valores Permitidos | Valor da amostra
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | Cadeia utilizada como base para atribuir nomes aos recursos implementados. | Apenas carateres alfanuméricos com comprimento de 1 a 6 | Eth
    AUTHTYPE | O método de autenticação para a máquina virtual. | Chave pública de palavra-passe ou SSH | Palavra-passe
    ADMINUSERNAME | Nome de utilizador de administrador de cada VM implementada | 1 - 64 carateres | gethadmin
    ADMINPASSWORD (tipo de autenticação = a palavra-passe)| A palavra-passe da conta de administrador para cada uma das máquinas virtuais implementadas. A palavra-passe tem de conter 3 dos seguintes requisitos: 1 maiúscula, 1 minúscula, 1 número e 1 caráter especial. <br />Enquanto todas as VMs inicialmente têm a mesma palavra-passe, pode alterar a palavra-passe após o aprovisionamento.|12 - 72 carateres|
    ADMINSSHKEY (tipo de autenticação = parâmetros sshPublicKey) | A chave de secure shell utilizada para início de sessão remoto. | |
    GENESISBLOCK | Cadeia JSON que representa o bloco de genesis personalizado. | |
    ETHEREUMACCOUNTPSSWD | A palavra-passe de administrador utilizada para proteger a conta de Ethereum. | |
    ETHEREUMACCOUNTPASSPHRASE | A frase de acesso utilizada para gerar a chave privada associada à conta Ethereum. | |
    ETHEREUMNETWORKID | O ID de rede do consortium. | Utilizar qualquer valor entre 5 e 999,999,999 | 72
    CONSORTIUMMEMBERID | O ID associado a cada membro da rede consortium.   | Este ID deve ser exclusivo na rede. | 0
    NUMMININGNODES | Número de nós de extração. | Entre 2 e 15. | 2
    MNNODEVMSIZE | Tamanho da VM de nós de extração. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | Desempenho de armazenamento de nós de extração. | | Standard_LRS
    NUMTXNODES | Número de nós de transação. | Entre 1 e 5. | 1
    TXNODEVMSIZE | Tamanho da VM de nós de transação. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | Desempenho de armazenamento de nós de transação. | | Standard_LRS
    BASEURL | URL de base para obter os modelos consoante a partir de. | Utilize o valor predefinido, exceto se pretender personalizar os modelos de implementação. | 

7. Selecione **OK**.
8. No **implementação personalizada**, especifique **subscrição**, **grupo de recursos**, e **localização do grupo de recursos**.
    
    ![Parâmetros de implementação LEADER](media/azure-stack-ethereum/leader-deployment-parameters.png)

    Nome do Parâmetro | Descrição | Valores Permitidos | Valor da amostra
    ---------------|-------------|----------------|-------------
    Subscrição | A subscrição para o qual pretende implementar a rede consortium | | Subscrição de consumo
    Grupo de Recursos | O grupo de recursos para o qual pretende implementar a rede consortium. | | EthereumResources
    Localização | A região do Azure para o grupo de recursos. | | local

8. Selecione **Criar**.

Implementação pode demorar 20 minutos ou mais a concluir.

Após a conclusão da implementação, pode rever a implementação de resumo para **Microsoft. Modelo** na secção de implementação do grupo de recursos. O resumo contém valores de saída que podem ser utilizados para associar membros consortium.

Para verificar a implementação do leader, procure o site de administração do leader. Pode encontrar o endereço do site de administração na secção saída **Template** implementação.  

![Resumo de implementação LEADER](media/azure-stack-ethereum/ethereum-node-status.png)

### <a name="joining-consortium-member-deployment"></a>A associar a implementação de membro consortium

1. Transferir o [modelo de membro consortium a partir do GitHub](https://raw.githubusercontent.com/seyadava/AzureStack-QuickStart-Templates-1/blockchain_nva/eth/marketplace/JoiningMember/mainTemplate.json)
2. No portal de administração de pilha do Azure, selecione **novo > implementação do modelo** para implementar a partir de um modelo personalizado.
3. Selecione **Editar modelo** para editar o novo modelo personalizado.
4. No painel de edição no lado direito, copie e cole o modelo de leader JSON que transferiu anteriormente.
5. Selecione **Guardar**.
6. Selecione **Editar parâmetros** e conclua os parâmetros de modelo para a sua implementação.

    Nome do Parâmetro | Descrição | Valores Permitidos | Valor da amostra
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | Cadeia utilizada como base para atribuir nomes aos recursos implementados. | Apenas carateres alfanuméricos com comprimento de 1 a 6 | Eth
    AUTHTYPE | O método de autenticação para a máquina virtual. | Chave pública de palavra-passe ou SSH | Palavra-passe
    ADMINUSERNAME | Nome de utilizador de administrador de cada VM implementada | 1 - 64 carateres | gethadmin
    ADMINPASSWORD (tipo de autenticação = a palavra-passe)| A palavra-passe da conta de administrador para cada uma das máquinas virtuais implementadas. A palavra-passe tem de conter 3 dos seguintes requisitos: 1 maiúscula, 1 minúscula, 1 número e 1 caráter especial. <br />Enquanto todas as VMs inicialmente têm a mesma palavra-passe, pode alterar a palavra-passe após o aprovisionamento.|12 - 72 carateres|
    ADMINSSHKEY (tipo de autenticação = parâmetros sshPublicKey) | A chave de secure shell utilizada para início de sessão remoto. | |
    CONSORTIUMMEMBERID | O ID associado a cada membro da rede consortium.   | Este ID deve ser exclusivo na rede. | 0
    NUMMININGNODES | Número de nós de extração. | Entre 2 e 15. | 2
    MNNODEVMSIZE | Tamanho da VM de nós de extração. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | Desempenho de armazenamento de nós de extração. | | Standard_LRS
    NUMTXNODES | Número de nós de transação. | Entre 1 e 5. | 1
    TXNODEVMSIZE | Tamanho da VM de nós de transação. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | Desempenho de armazenamento de nós de transação. | | Standard_LRS
    CONSORTIUMDATA | O URL a apontar para os dados de configuração relevantes consortium fornecidos pela implementação de outro membro. Este valor pode ser encontrado no resultado de implementação do Leader. | |
    REMOTEMEMBERVNETADDRESSSPACE | O endereço de NVA IP do leader. Este valor pode ser encontrado no resultado de implementação do leader. | | 
    REMOTEMEMBERNVAPUBLICIP | O endereço de NVA IP do leader. Este valor pode ser encontrado no resultado de implementação do leader. | | 
    CONNECTIONSHAREDKEY | Um segredo previamente estabelecido entre os membros da rede consortium que estiver a estabelecer uma ligação. | |
    BASEURL | URL de base para o modelo. | Utilize o valor predefinido, exceto se pretender personalizar os modelos de implementação. | 

7. Selecione **OK**.
8. No **implementação personalizada**, especifique **subscrição**, **grupo de recursos**, e **localização do grupo de recursos**.

    Nome do Parâmetro | Descrição | Valores Permitidos | Valor da amostra
    ---------------|-------------|----------------|-------------
    Subscrição | A subscrição para o qual pretende implementar a rede consortium | | Subscrição de consumo
    Grupo de Recursos | O grupo de recursos para o qual pretende implementar a rede consortium. | | MemberResources
    Localização | A região do Azure para o grupo de recursos. | | local

8. Selecione **Criar**.

Implementação pode demorar 20 minutos ou mais a concluir.

Após a conclusão da implementação, pode rever a implementação de resumo para **Template** na secção de implementação do grupo de recursos. O resumo contém valores de saída que podem ser utilizados para ligar os membros de consortium.

Para verificar a implementação do membro, procure o site de administração do membro. Pode encontrar o endereço do site de administração na secção de resultados de implementação de Template.

![Resumo de implementação de membro](media/azure-stack-ethereum/ethereum-node-status-2.png)

Como é mostrado na imagem, o estado de nós do membro é **não está em execução**. Isto acontece porque a ligação entre o membro e leader não for estabelecida. A ligação entre o membro e leader é uma ligação bidirecional. Quando implementa o membro, o modelo cria automaticamente a ligação de membro para o leader. Para criar a ligação de leader ao membro vá para o passo seguinte.

### <a name="connect-member-and-leader"></a>Ligar o membro e leader

Este modelo cria uma ligação do leader para um membro remoto. 

1. Transferir o [ligar membro e leader modelo a partir do GitHub](https://raw.githubusercontent.com/seyadava/AzureStack-QuickStart-Templates-1/blockchain_nva/eth/marketplace/Connection/mainTemplate.json)
2. No portal de administração de pilha do Azure, selecione **novo > implementação do modelo** para implementar a partir de um modelo personalizado.
3. Selecione **Editar modelo** para editar o novo modelo personalizado.
4. No painel de edição no lado direito, copie e cole o modelo de leader JSON que transferiu anteriormente.
    
    ![Editar modelo de ligar](media/azure-stack-ethereum/edit-connect-template.png)

5. Selecione **Guardar**.
6. Selecione **Editar parâmetros** e conclua os parâmetros de modelo para a sua implementação.
    
    ![Editar ligar os parâmetros do modelo](media/azure-stack-ethereum/edit-connect-parameters.png)

    Nome do Parâmetro | Descrição | Valores Permitidos | Valor da amostra
    ---------------|-------------|----------------|-------------
    MEMBERNAMEPREFIX | Prefixo do nome do LEADER. Este valor pode ser encontrado no resultado de implementação do leader.  | Apenas carateres alfanuméricos com comprimento de 1 a 6 | |
    MEMBERROUTETABLENAME | Nome da tabela de rotas a leader. Este valor pode ser encontrado no resultado de implementação do leader. |  | 
    REMOTEMEMBERVNETADDRESSSPACE | O espaço de endereços do membro. Este valor pode ser encontrado no resultado de implementação do membro. | |
    CONNECTIONSHAREDKEY | Um segredo previamente estabelecido entre os membros da rede consortium que estiver a estabelecer uma ligação.  | |
    REMOTEMEMBERNVAPUBLICIP | O endereço IP de NVA do membro. Este valor pode ser encontrado no resultado de implementação do membro. | |
    MEMBERNVAPRIVATEIP | Privado NVA endereço IP do LEADER. Este valor pode ser encontrado no resultado de implementação do leader. | |
    LOCALIZAÇÃO | Localização do seu ambiente de pilha do Azure. | | local
    BASEURL | URL de base para o modelo. | Utilize o valor predefinido, exceto se pretender personalizar os modelos de implementação. | 

7. Selecione **OK**.
8. No **implementação personalizada**, especifique **subscrição**, **grupo de recursos**, e **localização do grupo de recursos**.
    
    ![Ligar os parâmetros de implementação](media/azure-stack-ethereum/connect-deployment-parameters.png)

    Nome do Parâmetro | Descrição | Valores Permitidos | Valor da amostra
    ---------------|-------------|----------------|-------------
    Subscrição | Subscrição do leader. | | Subscrição de consumo
    Grupo de Recursos | Grupo de recursos do leader. | | EthereumResources
    Localização | A região do Azure para o grupo de recursos. | | local

8. Selecione **Criar**.

Depois de concluída a implementação, que demora alguns minutos para leader e membros iniciar a comunicação. Para verificar a implementação, atualize o site de administração do membro. Estado de nós do membro deve estar em execução. 

![Verificar a implementação](media/azure-stack-ethereum/ethererum-node-status-3.png)

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre Ethereum e o Azure, consulte o artigo [Blockchain tecnologia e aplicações | Microsoft Azure](https://azure.microsoft.com/solutions/blockchain/).
- Para obter mais informações sobre os cenários de blockchain no Azure, consulte [modelo de solução de prova do trabalho consortium Ethereum](../blockchain-workbench/ethereum-deployment-guide.md).
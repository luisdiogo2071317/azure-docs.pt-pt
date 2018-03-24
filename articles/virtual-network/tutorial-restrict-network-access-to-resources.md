---
title: Restringir o acesso de rede para recursos de PaaS - portal do Azure | Microsoft Docs
description: Saiba como limitar e restringir o acesso de rede para recursos do Azure, tais como o Storage do Azure e SQL Database do Azure, com pontos finais de serviço de rede virtual com o portal do Azure.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 9a64a5c1f63dc05cba6fdfa310b694e34bdba7d1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-portal"></a>Restringir o acesso de rede para recursos de PaaS com pontos finais de serviço de rede virtual com o portal do Azure

Pontos finais do serviço de rede virtual permitem-lhe limitar o acesso de rede a alguns recursos do serviço do Azure para uma sub-rede de rede virtual. Também pode remover o acesso à internet para os recursos. Pontos finais de serviço fornecem ligação direta a partir da sua rede virtual para os serviços do Azure suportadas, permitindo-lhe utilizar o espaço de endereços privados da sua rede virtual para aceder aos serviços do Azure. O tráfego destinado aos recursos do Azure através de pontos finais de serviço sempre permanece da rede principal do Microsoft Azure. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar uma rede virtual com uma sub-rede
> * Adicionar uma sub-rede e ativar um ponto final de serviço
> * Criar um recurso do Azure e permitir o acesso de rede ao mesmo de apenas uma sub-rede
> * Implementar uma máquina virtual (VM) em cada sub-rede
> * Confirmar o acesso a um recurso de sub-rede
> * Confirmar o acesso é negado a um recurso de uma sub-rede e a internet

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **rede Virtual**.
3. Introduza, ou selecione as seguintes informações e, em seguida, selecione **criar**:

    |Definição|Valor|
    |----|----|
    |Nome| myVirtualNetwork |
    |Espaço de endereços| 10.0.0.0/16|
    |Subscrição| Selecione a sua subscrição|
    |Grupo de recursos | Selecione **criar nova** e introduza *myResourceGroup*.|
    |Localização| Selecione **EUA leste** |
    |Nome da Sub-rede| Público|
    |Intervalo de endereços de sub-rede| 10.0.0.0/24|
    |pontos finais de serviço| Desativado|

    ![Introduza as informações básicas sobre a sua rede virtual](./media/tutorial-restrict-network-access-to-resources/create-virtual-network.png)


## <a name="enable-a-service-endpoint"></a>Ativar um ponto final de serviço

1. No **procurar recursos, serviços e docs** na parte superior do portal, introduza *myVirtualNetwork.* Quando **myVirtualNetwork** aparece nos resultados da pesquisa, selecionados-lo.
2. Adicione uma sub-rede para a rede virtual. Em **definições**, selecione **sub-redes**e, em seguida, selecione **+ sub-rede**, conforme mostrado na imagem seguinte:

    ![Adicionar sub-rede](./media/tutorial-restrict-network-access-to-resources/add-subnet.png) 

3. Em **adicionar sub-rede**, selecione ou introduza as seguintes informações e, em seguida, selecione **OK**:

    |Definição|Valor|
    |----|----|
    |Nome| Privado |
    |Intervalo de endereços| 10.0.1.0/24|
    |pontos finais de serviço| Selecione **Microsoft** em **serviços**|

## <a name="restrict-network-access-to-and-from-a-subnet"></a>Restringir o acesso de rede de e para uma sub-rede

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **grupo de segurança de rede**.
Em **criar um grupo de segurança de rede**, introduza, ou selecione as seguintes informações e, em seguida, selecione **criar**:

    |Definição|Valor|
    |----|----|
    |Nome| myNsgPrivate |
    |Subscrição| Selecione a sua subscrição|
    |Grupo de recursos | Selecione **utilizar existente** e selecione *myResourceGroup*.|
    |Localização| Selecione **EUA leste** |

4. Depois de criar o grupo de segurança de rede, introduzir *myNsgPrivate*, no **procurar recursos, serviços e docs** caixa na parte superior do portal. Quando **myNsgPrivate** aparece nos resultados da pesquisa, selecionados-lo.
5. Em **definições**, selecione **regras de segurança de saída**.
6. Selecione **+ adicionar**.
7. Crie uma regra que permite acesso de saída para os endereços IP públicos atribuída para este serviço de armazenamento do Azure. Introduza, ou selecione as seguintes informações e, em seguida, selecione **OK**:

    |Definição|Valor|
    |----|----|
    |Origem| Selecione **VirtualNetwork** |
    |Intervalos de portas de origem| * |
    |Destino | Selecione **etiquetas de serviço**|
    |Etiqueta do serviço de destino | Selecione **armazenamento**|
    |Intervalos de portas de destino| * |
    |Protocolo|Qualquer|
    |Ação|Permitir|
    |Prioridade|100|
    |Nome|Allow-Storage-All|
8. Crie uma regra que substitui uma regra de segurança predefinida que permite acesso de saída para todos os endereços IP públicos. Conclua os passos 6 e 7, com os seguintes valores:

    |Definição|Valor|
    |----|----|
    |Origem| Selecione **VirtualNetwork** |
    |Intervalos de portas de origem| * |
    |Destino | Selecione **etiquetas de serviço**|
    |Etiqueta do serviço de destino| Selecione **Internet**|
    |Intervalos de portas de destino| * |
    |Protocolo|Qualquer|
    |Ação|Negar|
    |Prioridade|110|
    |Nome|Deny-Internet-All|

9. Em **definições**, selecione **regras de segurança de entrada**.
10. Selecione **+ adicionar**.
11. Criar uma regra que permite que o tráfego de protocolo RDP (Remote Desktop Protocol) de entrada para a sub-rede partir de qualquer lugar. A regra substitui uma regra de segurança predefinida que nega todo o tráfego de entrada a partir da internet. Ligações de ambiente de trabalho remotas são permitidas para a sub-rede para que a conectividade pode ser testada num passo posterior. Conclua os passos 6 e 7, com os seguintes valores:

    |Definição|Valor|
    |----|----|
    |Origem| Qualquer |
    |Intervalos de portas de origem| * |
    |Destino | Selecione **etiquetas de serviço**|
    |Etiqueta do serviço de destino| Selecione **VirtualNetwork**|
    |Intervalos de portas de destino| 3389 |
    |Protocolo|Qualquer|
    |Ação|Permitir|
    |Prioridade|120|
    |Nome|Allow-RDP-All|

12. Em **definições**, selecione **sub-redes**.
13. Selecione **+ associar**
14. Em **associar sub-rede**, selecione **rede Virtual** e, em seguida, selecione **myVirtualNetwork** em **escolha uma rede virtual**.
15. Em **escolha subrede**, selecione **privada**e, em seguida, selecione **OK**.

## <a name="restrict-network-access-to-a-resource"></a>Restringir o acesso de rede a um recurso

Os passos necessários para restringir o acesso de rede para recursos criados através de serviços do Azure ativados para pontos finais de serviço varia em serviços. Consulte a documentação para serviços individuais para obter passos específicos para cada serviço. O resto deste artigo inclui os passos para restringir o acesso de rede para uma conta de armazenamento do Azure, como um exemplo.

### <a name="create-a-storage-account"></a>Criar uma conta do Storage

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **armazenamento**e, em seguida, selecione **conta de armazenamento - BLOBs, ficheiro, tabela, fila**.
3. Introduza, ou selecione as seguintes informações, aceite as restantes predefinições e, em seguida, selecione **criar**:

    |Definição|Valor|
    |----|----|
    |Nome| Introduza um nome que seja exclusivo em todas as localizações do Azure, entre 3 e 24 carateres de comprimento, utilizando apenas números e letras minúsculas.|
    |Tipo de conta|StorageV2 (fins gerais v2)|
    |Replicação| Armazenamento localmente redundante (LRS)|
    |Subscrição| Selecione a sua subscrição|
    |Grupo de recursos | Selecione **utilizar existente** e selecione *myResourceGroup*.|
    |Localização| Selecione **EUA leste** |

### <a name="create-a-file-share-in-the-storage-account"></a>Criar uma partilha de ficheiros na conta de armazenamento

1. Depois de criar a conta de armazenamento, introduza o nome da conta de armazenamento na **procurar recursos, serviços e docs** caixa, na parte superior do portal. Quando o nome da sua conta de armazenamento for apresentada nos resultados da pesquisa, selecione-a.
2. Selecione **ficheiros**, conforme mostrado na imagem seguinte:

    ![Conta de armazenamento](./media/tutorial-restrict-network-access-to-resources/storage-account.png) 
3. Selecione **+ partilha de ficheiros**, em **serviço ficheiro**.
4. Introduza *partilha de ficheiros my* em **nome**e, em seguida, selecione **OK**.
5. Fechar o **serviço ficheiro** caixa.

### <a name="enable-network-access-from-a-subnet"></a>Ativar o acesso de rede de sub-rede

Por predefinição, as contas do storage aceitam ligações de rede de clientes em qualquer rede. Para autorizar o acesso apenas uma sub-rede específica e negar o acesso de rede a partir de todas as outras redes, conclua os seguintes passos:

1. Em **definições** para a conta de armazenamento, selecione **Firewalls e redes virtuais**.
2. Em **redes virtuais**, selecione **selecionado redes**.
3. Selecione **adicionar rede virtual existente**.
4. Em **adicionar redes**, selecione os seguintes valores e, em seguida, selecione **adicionar**:

    |Definição|Valor|
    |----|----|
    |Subscrição| Selecione a sua subscrição.|
    |Redes virtuais|Selecione **myVirtualNetwork**, em **redes virtuais**|
    |Sub-redes| Selecione **privada**, em **sub-redes**|

    ![As firewalls e redes virtuais](./media/tutorial-restrict-network-access-to-resources/storage-firewalls-and-virtual-networks.png) 

5. Selecione **Guardar**.
6. Fechar o **Firewalls e redes virtuais** caixa.
7. Em **definições** para a conta de armazenamento, selecione **chaves de acesso**, conforme mostrado na imagem seguinte:

      ![As firewalls e redes virtuais](./media/tutorial-restrict-network-access-to-resources/storage-access-key.png)

8. Tenha em atenção o **chave** valor, que terá de introduzi-la manualmente num passo posterior ao mapear a partilha de ficheiros para uma letra de unidade na VM.

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Para testar o acesso de rede a uma conta de armazenamento, implemente uma VM para cada sub-rede.

### <a name="create-the-first-virtual-machine"></a>Criar primeira máquina virtual

1. Selecione **+ criar um recurso** encontrado no canto superior, à esquerda do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**.
3. Introduza, ou selecione as seguintes informações e, em seguida, selecione **OK**:

    |Definição|Valor|
    |----|----|
    |Nome| myVmPublic|
    |Nome de utilizador|Introduza um nome de utilizador à sua escolha.|
    |Palavra-passe| Introduza uma palavra-passe da sua escolha. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Subscrição| Selecione a sua subscrição.|
    |Grupo de recursos| Selecione **utilizar existente** e selecione **myResourceGroup**.|
    |Localização| Selecione **EUA Leste**.|

    ![Introduza as informações básicas sobre uma máquina virtual](./media/tutorial-restrict-network-access-to-resources/virtual-machine-basics.png)
4. Selecione um tamanho da máquina virtual e, em seguida, selecione **selecione**.
5. Em **definições**, selecione **rede** e, em seguida, selecione **myVirtualNetwork**. Em seguida, selecione **sub-rede**e selecione **pública**, conforme mostrado na imagem seguinte:

    ![Selecione uma rede virtual](./media/tutorial-restrict-network-access-to-resources/virtual-machine-settings.png)
6. No **resumo** página, selecione **criar** para iniciar a implementação da máquina virtual. A VM demora alguns minutos a implementar, mas pode continuar para o passo seguinte enquanto a VM está a criar.

### <a name="create-the-second-virtual-machine"></a>Criar a máquina virtual segundo

Concluir os passos 1-6 novamente, mas no passo 3, nome da máquina virtual *myVmPrivate* e no passo 5, selecione o **privada** sub-rede.

A VM demora alguns minutos a implementar. Continue para o passo seguinte até que acaba de criar e abrir as respetivas definições no portal.

## <a name="confirm-access-to-storage-account"></a>Confirmar o acesso à conta de armazenamento

1. Uma vez a *myVmPrivate* VM acaba de criar, Azure abre as definições para o mesmo. Ligar à VM, selecionando o **Connect** botão, conforme mostrado na imagem seguinte:

    ![Ligar a uma máquina virtual](./media/tutorial-restrict-network-access-to-resources/connect-to-virtual-machine.png)

2. Depois de selecionar o **Connect** botão, um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado e será transferido para o seu computador.  
3. Abra o ficheiro rdp transferido. Se lhe for pedido, selecione **Connect**. Introduza o nome de utilizador e palavra-passe que especificou ao criar a VM. Poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais que introduziu quando criou a VM. 
4. Selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Se receber o aviso, selecione **Sim** ou **continuar**, prossiga com a ligação.
6. No *myVmPrivate* VM, a partilha de ficheiros do Azure para a unidade Z através do PowerShell do mapa. Antes de executar os comandos que se seguem, substitua `<storage-account-key>` e `<storage-account-name>` com valores fornecidos e obtidos nas [criar uma conta de armazenamento](#create-a-storage-account).

    ```powershell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
    ```
    
    PowerShell devolve o resultado semelhante ao seguinte exemplo de saída:

    ```powershell
    Name           Used (GB)     Free (GB) Provider      Root
    ----           ---------     --------- --------      ----
    Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
    ```

    A partilha de ficheiros do Azure mapeada com êxito para a unidade Z.

7. Confirme que a VM possui conectividade de saída para outros endereços IP públicos numa linha de comandos:

    ```
    ping bing.com
    ```
    
    Receberá não existem respostas, porque o grupo de segurança de rede associados para o *privada* sub-rede não permite o acesso de saída para endereços IP públicos que não sejam endereços atribuídos para este serviço de armazenamento do Azure.

8. Fechar a sessão de ambiente de trabalho remoto para o *myVmPrivate* VM.

## <a name="confirm-access-is-denied-to-storage-account"></a>Confirmar o acesso foi negado à conta de armazenamento

1. Introduza *myVmPublic* no **procurar recursos, serviços e docs** caixa na parte superior do portal.
2. Quando **myVmPublic** aparece nos resultados da pesquisa, selecionados-lo.
3. Conclua os passos 1-6 em [confirmar o acesso à conta de armazenamento](#confirm-access-to-storage-account) para o *myVmPublic* VM.

    Acesso negado e recebe um `New-PSDrive : Access is denied` erro. O acesso é negado porque o *myVmPublic* VM for implementada no *pública* sub-rede. O *pública* sub-rede não tem um ponto final de serviço ativado para o Storage do Azure e a conta de armazenamento só permite o acesso de rede do *privada* sub-rede, não o *público*sub-rede.

4. Fechar a sessão de ambiente de trabalho remoto para o *myVmPublic* VM.

5. Do seu computador, navegue para o Azure [portal](https://portal.azure.com).
6. Introduza o nome da conta de armazenamento que criou no **procurar recursos, serviços e docs** caixa. Quando o nome da sua conta de armazenamento for apresentada nos resultados da pesquisa, selecione-a.
7. Selecione **Ficheiros**.
8. Recebe o erro mostrado na imagem seguinte:

    ![Acesso negado erro](./media/tutorial-restrict-network-access-to-resources/access-denied-error.png)

    O acesso é negado porque o computador não está a ser o *privada* sub-rede do *MyVirtualNetwork* rede virtual.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos e todos os recursos que contém:

1. Introduza *myResourceGroup* no **pesquisa** caixa na parte superior do portal. Quando vir **myResourceGroup** nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Introduza *myResourceGroup* para **tipo o nome de grupo de recursos:** e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ativar um ponto final de serviço para uma sub-rede de rede virtual. Aprendeu a que os pontos finais de serviço podem ser ativados para recursos implementados com vários serviços do Azure. Criou uma conta de armazenamento do Azure e o acesso limitado à rede para a conta de armazenamento para apenas os recursos dentro de uma sub-rede de rede virtual. Antes de criar pontos finais de serviço na produção redes virtuais, é recomendado que lhe exaustivamente familiarizar-se com [pontos finais de serviço](virtual-network-service-endpoints-overview.md).

Se tiver várias redes virtuais na sua conta, poderá pretender ligar duas redes virtuais em conjunto para que recursos dentro de cada rede virtual podem comunicar entre si. Avançar para o próximo tutorial para saber como ligar redes virtuais.

> [!div class="nextstepaction"]
> [Ligar redes virtuais](./tutorial-connect-virtual-networks-portal.md)

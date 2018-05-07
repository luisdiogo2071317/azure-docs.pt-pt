---
title: Criar um gateway de aplicação com o URL com base no caminho regras de encaminhamento - portal do Azure
description: Saiba como criar URL caminho com base em regras de encaminhamento para um gateway de aplicação e o dimensionamento da máquina virtual definido no portal do Azure.
services: application-gateway
author: vhorne
manager: jpconnock
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 3/26/2018
ms.author: victorh
ms.openlocfilehash: 554618b055ce5afcc67f95afa0242d36e74fabc0
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Criar um gateway de aplicação com o caminho com base em regras de encaminhamento utilizando o portal do Azure

Pode utilizar o portal do Azure para configurar [regras de encaminhamento com base no caminho de URL](application-gateway-url-route-overview.md) quando cria um [gateway de aplicação](application-gateway-introduction.md). Neste tutorial, vai criar conjuntos de back-end a utilizar máquinas virtuais. Em seguida, crie as regras de encaminhamento que certifique-se de que o tráfego web chega os servidores adequados nos agrupamentos de.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Criar um gateway de aplicação
> * Criar máquinas virtuais para servidores back-end
> * Criar conjuntos de back-end com os servidores de back-end
> * Criar um serviço de escuta de back-end
> * Criar uma regra de encaminhamento com base no caminho

![Exemplo de encaminhamento de URL](./media/application-gateway-create-url-route-portal/scenario.png)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Criar um gateway de aplicação

É necessária uma rede virtual para a comunicação entre os recursos que criar. Duas sub-redes são criadas neste exemplo: um para o gateway de aplicação e outra para servidores de back-end. Pode criar uma rede virtual ao mesmo tempo que criar o gateway de aplicação.

1. Clique em **novo** encontrado no canto superior esquerdo do portal do Azure.
2. Selecione **redes** e, em seguida, selecione **Gateway de aplicação** na lista em destaque.
3. Introduza estes valores para o gateway de aplicação:

    - *myAppGateway* - para que o nome do gateway de aplicação.
    - *myResourceGroupAG* – para o novo grupo de recursos.

    ![Criar o novo gateway de aplicação](./media/application-gateway-create-url-route-portal/application-gateway-create.png)

4. Aceite os valores predefinidos para as outras definições e, em seguida, clique em **OK**.
5. Clique em **escolha uma rede virtual**, clique em **criar nova**e, em seguida, introduza estes valores para a rede virtual:

    - *myVNet* - para que o nome da rede virtual.
    - *10.0.0.0/16* - para que o espaço de endereços de rede virtual.
    - *myAGSubnet* - para que o nome de sub-rede.
    - *10.0.0.0/24* - para que o espaço de endereços de sub-rede.

    ![Criar a rede virtual](./media/application-gateway-create-url-route-portal/application-gateway-vnet.png)

6. Clique em **OK** para criar a rede virtual e a sub-rede.
7. Clique em **escolher um endereço IP público**, clique em **criar nova**e, em seguida, introduza o nome do endereço IP público. Neste exemplo, o endereço IP público é denominado *myAGPublicIPAddress*. Aceite os valores predefinidos para as outras definições e, em seguida, clique em **OK**.
8. Aceite os valores predefinidos para a configuração do serviço de escuta, deixe a firewall de aplicações Web desativada e, em seguida, clique em **OK**.
9. Reveja as definições na página de resumo e, em seguida, clique em **OK** para criar os recursos de rede e o gateway de aplicação. Pode demorar alguns minutos até o gateway de aplicação ser criado, aguarde a conclusão da implementação com êxito antes de continuar para a secção seguinte.

### <a name="add-a-subnet"></a>Adicionar uma sub-rede

1. Clique em **todos os recursos** no menu da esquerda e, em seguida, clique em **myVNet** na lista de recursos.
2. Clique em **sub-redes**e, em seguida, clique em **sub-rede**.

    ![Criar sub-rede](./media/application-gateway-create-url-route-portal/application-gateway-subnet.png)

3. Introduza *myBackendSubnet* para o nome da sub-rede e, em seguida, clique em **OK**.

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Neste exemplo, crie três máquinas virtuais a ser utilizada como servidores de back-end para o gateway de aplicação. Também pode instala o IIS nas máquinas virtuais para verificar se o gateway de aplicação foi criado com êxito.

1. Clique em **Novo**.
2. Clique em **computação** e, em seguida, selecione **Datacenter do Windows Server 2016** na lista em destaque.
3. Introduza estes valores para a máquina virtual:

    - *myVM1* - no nome da máquina virtual.
    - *azureuser* - no nome de utilizador do administrador.
    - *Azure123456!* a palavra-passe.
    - Selecione **utilizar existente**e, em seguida, selecione *myResourceGroupAG*.

4. Clique em **OK**.
5. Selecione **DS1_V2** para o tamanho da máquina virtual e clique em **Selecionar**.
6. Certifique-se de que **myVNet** está selecionado para a rede virtual e a sub-rede é **myBackendSubnet**. 
7. Clique em **Desativado** para desativar o diagnóstico de arranque.
8. Clique em **OK**, reveja as definições na página de resumo e, em seguida, clique em **Criar**.

### <a name="install-iis"></a>Instalar o IIS

1. Abra a shell interativa e certifique-se de que está definido como **PowerShell**.

    ![Instalar a extensão personalizado](./media/application-gateway-create-url-route-portal/application-gateway-extension.png)

2. Execute o seguinte comando para instalar o IIS na máquina virtual: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/vhorne/samplescripts/master/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName myVM1 `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. Crie duas máquinas virtuais mais e instalar o IIS utilizando os passos que acabou de concluir. Introduza os nomes dos *myVM2* e *myVM3* para os nomes de e para os valores de VMName no conjunto AzureRmVMExtension.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Criar conjuntos de back-end com as máquinas virtuais

1. Clique em **todos os recursos** e, em seguida, clique em **myAppGateway**.
2. Clique em **conjuntos back-end**. Um conjunto predefinido foi criado automaticamente com o gateway de aplicação. Clique em **appGatewayBackendPool**.
3. Clique em **adicionar destino** adicionar *myVM1* para appGatewayBackendPool.

    ![Adicionar servidores de back-end](./media/application-gateway-create-url-route-portal/application-gateway-backend.png)

4. Clique em **Guardar**.
5. Clique em **conjuntos back-end** e, em seguida, clique em **adicionar**.
6. Introduza um nome de *imagesBackendPool* e adicione *myVM2* utilizando **adicionar destino**.
7. Clique em **OK**.
8. Clique em **adicionar** novamente para adicionar outro conjunto de back-end com um nome de *videoBackendPool* e adicione *myVM3* ao mesmo.

## <a name="create-a-backend-listener"></a>Criar um serviço de escuta de back-end

1. Clique em **os serviços de escuta** e clique em **básico**.
2. Introduza *myBackendListener* para o nome, *myFrontendPort* para o nome da porta de front-end e, em seguida, *8080* como a porta para o serviço de escuta.
3. Clique em **OK**.

## <a name="create-a-path-based-routing-rule"></a>Criar uma regra de encaminhamento com base no caminho

1. Clique em **regras** e, em seguida, clique em **com base no caminho**.
2. Introduza *rule2* para o nome.
3. Introduza *imagens* para o nome do caminho primeiro. Introduza */images/** para o caminho. Selecione **imagesBackendPool** para o conjunto de back-end.
4. Introduza *vídeo* para o nome do caminho segundo. Introduza */video/** para o caminho. Selecione **videoBackendPool** para o conjunto de back-end.

    ![Criar uma regra de caminho](./media/application-gateway-create-url-route-portal/application-gateway-route-rule.png)

5. Clique em **OK**.

## <a name="test-the-application-gateway"></a>O gateway de aplicação de teste

1. Clique em **todos os recursos**e, em seguida, clique em **myAGPublicIPAddress**.

    ![Registar o endereço IP público do application gateway](./media/application-gateway-create-url-route-portal/application-gateway-record-ag-address.png)

2. Copie o endereço IP público e cole-o na barra de endereço do browser. Tal como http://http://40.121.222.19.

    ![URL de base de teste no gateway de aplicação](./media/application-gateway-create-url-route-portal/application-gateway-iistest.png)

3. Altere o URL para http://&lt;endereço ip&gt;: 8080/images/test.htm, substituindo &lt;endereço ip&gt; endereço e com o IP deverá ver algo semelhante ao seguinte exemplo:

    ![Testar o URL de imagens no gateway de aplicação](./media/application-gateway-create-url-route-portal/application-gateway-iistest-images.png)

4. Altere o URL para http://&lt;endereço ip&gt;: 8080/video/test.htm, substituindo &lt;endereço ip&gt; endereço e com o IP deverá ver algo semelhante ao seguinte exemplo:

    ![URL de vídeo de teste no gateway de aplicação](./media/application-gateway-create-url-route-portal/application-gateway-iistest-video.png)

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como

> [!div class="checklist"]
> * Criar um gateway de aplicação
> * Criar máquinas virtuais para servidores back-end
> * Criar conjuntos de back-end com os servidores de back-end
> * Criar um serviço de escuta de back-end
> * Criar uma regra de encaminhamento com base no caminho

Para obter mais informações sobre gateways de aplicação e os recursos associados, avance para os artigos de procedimentos.

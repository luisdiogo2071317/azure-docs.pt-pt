---
title: Criar um gateway de aplicação com vários sites que aloja - portal do Azure | Microsoft Docs
description: Saiba como criar um gateway de aplicação que aloja vários sites no portal do Azure.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: victorh
ms.openlocfilehash: f3dd092b2298bfc97cac30b8706e0588a466e1e0
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-with-multiple-site-hosting-using-the-azure-portal"></a>Criar um gateway de aplicação com vários sites que aloja a utilizar o portal do Azure

Pode utilizar o portal do Azure para configurar [alojar vários web sites](application-gateway-multi-site-overview.md) quando cria um [gateway de aplicação](application-gateway-introduction.md). Neste tutorial, vai criar conjuntos de back-end utilizando conjuntos de dimensionamento de máquinas virtuais. Em seguida, configure os serviços de escuta e as regras com base em domínios que possui para se certificar de que o tráfego web chega os servidores adequados nos agrupamentos de. Este tutorial parte do princípio de que possui vários domínios e utiliza exemplos *www.contoso.com* e *www.fabrikam.com*.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Criar um gateway de aplicação
> * Criar máquinas virtuais para servidores back-end
> * Criar conjuntos de back-end com os servidores de back-end
> * Criar serviços de escuta e regras de encaminhamento
> * Crie um registo CNAME no seu domínio

![Exemplo de encaminhamento de vários site](./media/application-gateway-create-multisite-portal/scenario.png)

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

    ![Criar o novo gateway de aplicação](./media/application-gateway-create-multisite-portal/application-gateway-create.png)

4. Aceite os valores predefinidos para as outras definições e, em seguida, clique em **OK**.
5. Clique em **escolha uma rede virtual**, clique em **criar nova**e, em seguida, introduza estes valores para a rede virtual:

    - *myVNet* - para que o nome da rede virtual.
    - *10.0.0.0/16* - para que o espaço de endereços de rede virtual.
    - *myAGSubnet* - para que o nome de sub-rede.
    - *10.0.0.0/24* - para que o espaço de endereços de sub-rede.

    ![Criar a rede virtual](./media/application-gateway-create-multisite-portal/application-gateway-vnet.png)

6. Clique em **OK** para criar a rede virtual e a sub-rede.
7. Clique em **escolher um endereço IP público**, clique em **criar nova**e, em seguida, introduza o nome do endereço IP público. Neste exemplo, o endereço IP público é denominado *myAGPublicIPAddress*. Aceite os valores predefinidos para as outras definições e, em seguida, clique em **OK**.
8. Aceite os valores predefinidos para a configuração do serviço de escuta, deixe a firewall de aplicações Web desativada e, em seguida, clique em **OK**.
9. Reveja as definições na página de resumo e, em seguida, clique em **OK** para criar os recursos de rede e o gateway de aplicação. Pode demorar alguns minutos até o gateway de aplicação ser criado, aguarde a conclusão da implementação com êxito antes de continuar para a secção seguinte.

### <a name="add-a-subnet"></a>Adicionar uma sub-rede

1. Clique em **todos os recursos** no menu da esquerda e, em seguida, clique em **myVNet** na lista de recursos.
2. Clique em **sub-redes**e, em seguida, clique em **sub-rede**.

    ![Criar sub-rede](./media/application-gateway-create-multisite-portal/application-gateway-subnet.png)

3. Introduza *myBackendSubnet* para o nome da sub-rede e, em seguida, clique em **OK**.

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Neste exemplo, crie duas máquinas virtuais a ser utilizada como servidores de back-end para o gateway de aplicação. Também pode instala o IIS nas máquinas virtuais para verificar que é o encaminhamento de tráfego corretamente.

1. Clique em **Novo**.
2. Clique em **computação** e, em seguida, selecione **Datacenter do Windows Server 2016** na lista em destaque.
3. Introduza estes valores para a máquina virtual:

    - *contosoVM* - para que o nome da máquina virtual.
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

    ![Instalar a extensão personalizado](./media/application-gateway-create-multisite-portal/application-gateway-extension.png)

2. Execute o seguinte comando para instalar o IIS na máquina virtual: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/vhorne/samplescripts/master/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName contosoVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. Criar segunda máquina virtual e instale o IIS utilizando os passos que acabou de concluir. Introduza os nomes dos *fabrikamVM* para o nome e o valor de VMName no conjunto AzureRmVMExtension.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Criar conjuntos de back-end com as máquinas virtuais

1. Clique em **todos os recursos** e, em seguida, clique em **myAppGateway**.
2. Clique em **conjuntos back-end**e, em seguida, clique em **adicionar**.
3. Introduza um nome de *contosoPool* e adicione *contosoVM* utilizando **adicionar destino**.

    ![Adicionar servidores de back-end](./media/application-gateway-create-multisite-portal/application-gateway-multisite-backendpool.png)

4. Clique em **OK**.
5. Clique em **conjuntos back-end** e, em seguida, clique em **adicionar**.
6. Criar o *fabrikamPool* com o *fabrikamVM* utilizando os passos que acabou de concluir.

## <a name="create-listeners-and-routing-rules"></a>Criar serviços de escuta e regras de encaminhamento

1. Clique em **os serviços de escuta** e, em seguida, clique em **multilocal**.
2. Introduza estes valores para o serviço de escuta:
    
    - *contosoListener* - para que o nome do serviço de escuta.
    - *www.contoso.com* -substitua este exemplo de nome de anfitrião com o nome de domínio.

3. Clique em **OK**.
4. Criar um serviço de escuta segundo utilizando o nome do *fabrikamListener* e utilizar o seu nome de domínio de segundo. Neste exemplo, *www.fabrikam.com* é utilizado.

As regras são processadas pela ordem listadas e o tráfego é direcionado através da primeira regra que corresponde ao independentemente especificidade. Por exemplo, se tiver uma regra com um serviço de escuta básico e uma regra com uma escuta de vários site ambas na mesma porta, a regra com o serviço de escuta de vários site têm de estar listada antes da regra com o serviço de escuta básico por ordem para a regra de vários site a funcionar conforme esperado. 

Neste exemplo, pode criar duas novas regras e eliminar a regra predefinida que foi criada quando criou o gateway de aplicação. 

1. Clique em **regras** e, em seguida, clique em **básico**.
2. Introduza *contosoRule* para o nome.
3. Selecione *contosoListener* para o serviço de escuta.
4. Selecione *contosoPool* para o conjunto de back-end.

    ![Criar uma regra de caminho](./media/application-gateway-create-multisite-portal/application-gateway-multisite-rule.png)

5. Clique em **OK**.
6. Crie uma segunda regra com os nomes dos *fabrikamRule*, *fabrikamListener*, e *fabrikamPool*.
7. Eliminar a regra predefinida, denominada *rule1* , clicar no mesmo e, em seguida, clicando **eliminar**.

## <a name="create-a-cname-record-in-your-domain"></a>Crie um registo CNAME no seu domínio

Depois de criado o gateway de aplicação com o respetivo endereço IP público, pode obter o endereço DNS e utilizá-la para criar um registo CNAME no seu domínio. A utilização de registos não é recomendada porque o VIP podem ser alterados quando o gateway de aplicação é reiniciado.

1. Clique em **todos os recursos**e, em seguida, clique em **myAGPublicIPAddress**.

    ![Gateway de aplicação de registo endereço DNS](./media/application-gateway-create-multisite-portal/application-gateway-multisite-dns.png)

2. Copie o endereço DNS e utilizá-lo como o valor para um novo registo CNAME no seu domínio.

## <a name="test-the-application-gateway"></a>O gateway de aplicação de teste

1. Introduza o nome de domínio na barra de endereço do seu browser. Tal como http://www.contoso.com.

    ![Testar o site da contoso no gateway de aplicação](./media/application-gateway-create-multisite-portal/application-gateway-iistest.png)

2. Altere o endereço para o outro domínio e deverá ver algo semelhante ao seguinte exemplo:

    ![Testar o site da fabrikam no gateway de aplicação](./media/application-gateway-create-multisite-portal/application-gateway-iistest2.png)

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como:

> [!div class="checklist"]
> * Criar um gateway de aplicação
> * Criar máquinas virtuais para servidores back-end
> * Criar conjuntos de back-end com os servidores de back-end
> * Criar serviços de escuta e regras de encaminhamento
> * Crie um registo CNAME no seu domínio

> [!div class="nextstepaction"]
> [Saiba mais sobre o que pode fazer com o gateway de aplicação](application-gateway-introduction.md)
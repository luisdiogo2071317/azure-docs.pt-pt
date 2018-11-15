---
title: Criar um Gateway de aplicação - Portal do Azure
description: Saiba como criar um Gateway de aplicação com o portal do Azure.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2018
ms.author: victorh
ms.openlocfilehash: d8ab49bc988060533bc5ff65d414dcba6245be48
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51631914"
---
# <a name="create-an-application-gateway-using-the-azure-portal"></a>Criar um gateway de aplicação do portal do Azure

Pode utilizar o portal do Azure para criar ou gerir gateways de aplicação. Este artigo mostra-lhe como criar recursos de rede, servidores de back-end e um gateway de aplicação.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Criar um gateway de aplicação

Uma rede virtual é necessária para a comunicação entre os recursos que criar. Neste exemplo, são criadas duas sub-redes: uma para o gateway de aplicação e a outra para os servidores de back-end. Pode criar uma rede virtual ao mesmo tempo que cria o gateway de aplicação.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Clique em **Networking** e, em seguida, clique em **Gateway de aplicação** na lista em destaque.

### <a name="basics"></a>Noções básicas

1. Introduza estes valores para o gateway de aplicação:

    - *myAppGateway* - para o nome do gateway de aplicação.
    - *myResourceGroupAG* - para o novo grupo de recursos.

    ![Criar um novo gateway de aplicação](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

2. Aceite os valores predefinidos para as outras definições e, em seguida, clique em **OK**.

### <a name="settings"></a>Definições

1. Clique em **escolher uma rede virtual**, clique em **criar nova**e, em seguida, introduza estes valores para a rede virtual:

    - *myVNet* - para o nome da rede virtual.
    - *10.0.0.0/16* - para o espaço de endereços de rede virtual.
    - *myAGSubnet* - para o nome da sub-rede.
    - *10.0.0.0/24* – para o intervalo de endereços da sub-rede.

    ![Criar a rede virtual](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

6. Clique em **OK** voltar para a página de definições.
7. Sob **a configuração de IP de front-end** Certifique-se **tipo de endereço IP** está definida como **pública**e, em **endereço IP público**, certifique-se de **Criar novo** está selecionada. Tipo *myAGPublicIPAddress* para o nome do endereço IP público. Aceite os valores predefinidos para as outras definições e, em seguida, clique em **OK**.

### <a name="summary"></a>Resumo

Reveja as definições na página de resumo e, em seguida, clique em **OK** para criar a rede virtual, o endereço IP público e o gateway de aplicação. A criação do gateway de aplicação pode demorar vários minutos. Aguarde até a conclusão da implementação com êxito antes de passar para a secção seguinte.

## <a name="add-a-subnet"></a>Adicionar uma sub-rede

1. Clique em **Todos os recursos** no menu da esquerda e, em seguida, clique em **myVNet** na lista de recursos.
2. Clique em **sub-redes**e, em seguida, clique em **+ sub-rede**.

    ![Criar sub-rede](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Introduza *myBackendSubnet* para o nome da sub-rede e clique em **OK**.

## <a name="create-backend-servers"></a>Criar servidores de back-end

Neste exemplo, vai criar duas máquinas virtuais que são utilizadas como servidores de back-end para o gateway de aplicação. Vai também instalar o IIS nas máquinas virtuais para verificar se o gateway de aplicação foi criado com êxito.

### <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

1. No portal do Azure, clique em **Criar um recurso**.
2. Clique em **computação** e, em seguida, selecione **do Windows Server 2016 Datacenter** na lista em destaque.
3. Introduza estes valores para a máquina virtual:

    - *myResourceGroupAG* para o grupo de recursos.
    - *myVM* - para o nome da máquina virtual.
    - *azureuser* - no nome de utilizador do administrador.
    - *Azure123456!* para a palavra-passe.

   Aceite as outras predefinições e clique em **seguinte: discos**.
4. Aceite as predefinições de disco e clique em **seguinte: funcionamento em rede**.
5. Certifique-se de que **myVNet** está selecionado para a rede virtual e que a sub-rede é **myBackendSubnet**.
6. Aceite as outras predefinições e clique em **seguinte: gestão**.
7. Clique em **desativar** para desativar o diagnóstico de arranque. Aceite as outras predefinições e clique em **rever + criar**.
8. Reveja as definições na página de resumo e, em seguida, clique em **criar**.
9. Aguarde até a criação da máquina virtual concluir antes de continuar.

### <a name="install-iis"></a>Instalar o IIS

1. Abra a shell interativa e certifique-se de que está definido como **PowerShell**.

    ![Instalar uma extensão personalizada](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

2. Execute o comando seguinte para instalar o IIS na máquina virtual: 

    ```azurepowershell-interactive
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. Crie uma segunda máquina virtual e instale o IIS com os passos que acabou de concluir. Introduza *myVM2* para o respetivo nome e para VMName em Set-AzureRmVMExtension.

### <a name="add-backend-servers"></a>Adicionar servidores back-end

1. Clique em **todos os recursos**e, em seguida, clique em **myAppGateway**.
4. Clique em **Conjuntos de back-end**. Um conjunto predefinido foi criado automaticamente com o gateway de aplicação. Clique em **appGatewayBackendPool**.
5. Sob **destinos**, clique em **IP ou FQDN do endereço** selecionar **Máquina Virtual**.
6. Sob **Máquina Virtual**, adicionar máquinas virtuais myVM e myVM2 e suas interfaces de rede associado.

    ![Adicionar servidores back-end](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. Clique em **Guardar**.

## <a name="test-the-application-gateway"></a>Testar o gateway de aplicação

1. Localize o endereço IP público do gateway de aplicação no ecrã Descrição geral. Clique em **todos os recursos** e, em seguida, clique em **myAGPublicIPAddress**.

    ![Registar o endereço IP público do gateway de aplicação](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

2. Copie o endereço IP público e cole-o na barra de endereço do browser.

    ![Testar o gateway de aplicação](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos, o gateway de aplicação e todos os recursos relacionados. Para tal, selecione o grupo de recursos que contém o gateway de aplicação e clique em **Eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou um grupo de recursos, recursos de rede e servidores de back-end. Em seguida, utilizou esses recursos para criar um gateway de aplicação. Para saber mais sobre os gateways de aplicação e os recursos associados, consulte [o que é o Gateway de aplicação do Azure?](overview.md)

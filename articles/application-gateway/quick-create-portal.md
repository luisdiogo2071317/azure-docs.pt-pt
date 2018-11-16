---
title: Início Rápido - Direcionar tráfego da Web com o Gateway de Aplicação do Azure - portal do Azure | Microsoft Docs
description: Saiba como utilizar o portal do Azure para criar um Gateway de Aplicação do Azure que direciona o tráfego da Web para máquinas virtuais num conjunto de back-end.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 11/15/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 6ad839b9cf1179e282b9163df5a38e13417408e2
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706233"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-portal"></a>Início Rápido: Direcionar tráfego da Web com o Gateway de Aplicação do Azure - portal do Azure

Com o Gateway de Aplicação do Azure, pode direcionar o tráfego da Web de aplicações para recursos específicos ao atribuir serviços de escuta a portas, criar regras e adicionar recursos a um conjunto de back-end.

Este início rápido mostra como utilizar o portal do Azure para criar rapidamente o gateway de aplicação com duas máquinas virtuais no respetivo conjunto de back-end. Em seguida, vai testá-lo para se certificar de que está a funcionar corretamente.

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

> [!div class="nextstepaction"]
> [Gerir o tráfego da Web com um gateway de aplicação com a CLI do Azure](./tutorial-manage-web-traffic-cli.md)

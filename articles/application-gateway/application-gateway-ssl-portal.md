---
title: Criar um gateway de aplicação com a terminação de SSL - portal do Azure | Microsoft Docs
description: Saiba como criar um gateway de aplicação e adicionar um certificado para a terminação de SSL com o portal do Azure.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: victorh
ms.openlocfilehash: 10796000f913428e39a0ffbd0aa2cbe0c515eb7a
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-with-ssl-termination-using-the-azure-portal"></a>Criar um gateway de aplicação com a terminação de SSL com o portal do Azure

Pode utilizar o portal do Azure para criar um [gateway de aplicação](application-gateway-introduction.md) com um certificado para a terminação de SSL, que utiliza máquinas virtuais para servidores back-end.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Criar um certificado autoassinado
> * Criar um gateway de aplicação com o certificado
> * Criar as máquinas virtuais utilizadas como servidores de back-end

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em [http://portal.azure.com](http://portal.azure.com)

## <a name="create-a-self-signed-certificate"></a>Criar um certificado autoassinado

Esta secção, irá utilizar [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) para criar um certificado autoassinado que carregar para o portal do Azure ao criar o serviço de escuta para o gateway de aplicação.

No computador local, abra uma janela do Windows PowerShell como administrador. Execute o seguinte comando para criar o certificado:

```powershell
New-SelfSignedCertificate \
  -certstorelocation cert:\localmachine\my \
  -dnsname www.contoso.com
```

Deverá ver algo semelhante a esta resposta:

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com

Use [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) with the Thumbprint that was returned to export a pfx file from the certificate:
```

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate \
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 \
  -FilePath c:\appgwcert.pfx \
  -Password $pwd
```

## <a name="create-an-application-gateway"></a>Criar um gateway de aplicação

É necessária uma rede virtual para a comunicação entre os recursos que criar. Duas sub-redes são criadas neste exemplo: um para o gateway de aplicação e outra para servidores de back-end. Pode criar uma rede virtual ao mesmo tempo que criar o gateway de aplicação.

1. Clique em **novo** encontrado no canto superior esquerdo do portal do Azure.
2. Selecione **redes** e, em seguida, selecione **Gateway de aplicação** na lista em destaque.
3. Introduza *myAppGateway* para o nome do gateway de aplicação e *myResourceGroupAG* para o novo grupo de recursos.
4. Aceite os valores predefinidos para as outras definições e, em seguida, clique em **OK**.
5. Clique em **escolha uma rede virtual**, clique em **criar nova**e, em seguida, introduza estes valores para a rede virtual:

    - *myVNet* - para que o nome da rede virtual.
    - *10.0.0.0/16* - para que o espaço de endereços de rede virtual.
    - *myAGSubnet* - para que o nome de sub-rede.
    - *10.0.0.0/24* - para que o espaço de endereços de sub-rede.

    ![Criar a rede virtual](./media/application-gateway-ssl-portal/application-gateway-vnet.png)

6. Clique em **OK** para criar a rede virtual e a sub-rede.
7. Clique em **escolher um endereço IP público**, clique em **criar nova**e, em seguida, introduza o nome do endereço IP público. Neste exemplo, o endereço IP público é denominado *myAGPublicIPAddress*. Aceite os valores predefinidos para as outras definições e, em seguida, clique em **OK**.
8. Clique em **HTTPS** para o protocolo do serviço de escuta e certifique-se de que a porta está definida como **443**.
9. Clique no ícone de pasta e navegue para o *appgwcert.pfx* certificado que criou anteriormente para carregá-la.
10. Introduza *mycert1* para o nome do certificado e *Azure123456!* a palavra-passe e, em seguida, clique em **OK**.

    ![Criar o novo gateway de aplicação](./media/application-gateway-ssl-portal/application-gateway-create.png)

11. Reveja as definições na página de resumo e, em seguida, clique em **OK** para criar os recursos de rede e o gateway de aplicação. Pode demorar alguns minutos até o gateway de aplicação ser criado, aguarde a conclusão da implementação com êxito antes de continuar para a secção seguinte.

### <a name="add-a-subnet"></a>Adicionar uma sub-rede

1. Clique em **todos os recursos** no menu da esquerda e, em seguida, clique em **myVNet** na lista de recursos.
2. Clique em **sub-redes**e, em seguida, clique em **sub-rede**.

    ![Criar sub-rede](./media/application-gateway-ssl-portal/application-gateway-subnet.png)

3. Introduza *myBackendSubnet* para o nome da sub-rede e, em seguida, clique em **OK**.

## <a name="create-backend-servers"></a>Criar servidores de back-end

Neste exemplo, crie duas máquinas virtuais a ser utilizada como servidores de back-end para o gateway de aplicação. Também pode instala o IIS nas máquinas virtuais para verificar se o gateway de aplicação foi criado com êxito.

### <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

1. Clique em **Novo**.
2. Clique em **computação** e, em seguida, selecione **Datacenter do Windows Server 2016** na lista em destaque.
3. Introduza estes valores para a máquina virtual:

    - *myVM* - para que o nome da máquina virtual.
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

    ![Instalar a extensão personalizado](./media/application-gateway-ssl-portal/application-gateway-extension.png)

2. Execute o seguinte comando para instalar o IIS na máquina virtual: 

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

3. Criar uma segunda máquina virtual e instale o IIS utilizando os passos que acabou de concluir. Introduza *myVM2* para o respetivo nome e para VMName no conjunto AzureRmVMExtension.

### <a name="add-backend-servers"></a>Adicionar servidores de back-end

3. Clique em **todos os recursos**e, em seguida, clique em **myAppGateway**.
4. Clique em **conjuntos back-end**. Um conjunto predefinido foi criado automaticamente com o gateway de aplicação. Clique em **appGateayBackendPool**.
5. Clique em **adicionar destino** para adicionar cada máquina virtual que criou para o conjunto de back-end.

    ![Adicionar servidores de back-end](./media/application-gateway-ssl-portal/application-gateway-backend.png)

6. Clique em **Guardar**.

## <a name="test-the-application-gateway"></a>O gateway de aplicação de teste

1. Clique em **todos os recursos**e, em seguida, clique em **myAGPublicIPAddress**.

    ![Registar o endereço IP público do application gateway](./media/application-gateway-ssl-portal/application-gateway-ag-address.png)

2. Copie o endereço IP público e cole-o na barra de endereço do browser. Para aceitar o aviso de segurança se utilizou um certificado autoassinado, selecione os detalhes e, em seguida, avance para a página Web:

    ![Aviso seguro](./media/application-gateway-ssl-portal/application-gateway-secure.png)

    O site IIS protegido é apresentado como no exemplo seguinte:

    ![URL de base de teste no gateway de aplicação](./media/application-gateway-ssl-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar um certificado autoassinado
> * Criar um gateway de aplicação com o certificado
> * Criar as máquinas virtuais utilizadas como servidores de back-end

Para obter mais informações sobre gateways de aplicação e os recursos associados, avance para os artigos de procedimentos.
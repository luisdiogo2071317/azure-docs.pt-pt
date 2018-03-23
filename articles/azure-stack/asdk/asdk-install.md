---
title: Instalar o Kit de desenvolvimento de pilha do Azure (ASDK) | Microsoft Docs
description: Descreve como instalar o Kit de desenvolvimento de pilha do Azure (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 7b8fe61731a9412c61152bc58e55deebb611d011
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="install-the-azure-stack-development-kit-asdk"></a>Instalar o Kit de desenvolvimento de pilha do Azure (ASDK)
Depois de [preparar o computador de anfitrião ASDK](asdk-prepare-host.md), o ASDK pode ser implementado para a imagem de CloudBuilder.vhdx utilizando os seguintes passos neste artigo.

## <a name="install-the-asdk"></a>Instalar o ASDK
Os passos neste artigo mostram como implementar o ASDK utilizando uma interface de utilizador gráfica (GUI) fornecida ao descarregar e executar o **asdk installer.ps1** script do PowerShell.

> [!NOTE]
> A interface de utilizador do instalador para o Kit de desenvolvimento de pilha do Azure é um script tenham origem aberto com base em WCF e PowerShell.


1. Depois do computador anfitrião arranca com êxito na imagem do CloudBuilder.vhdx, inicie sessão utilizando as credenciais de administrador especificadas quando a [preparar o computador de anfitrião do kit de desenvolvimento](asdk-prepare-host.md) para instalação ASDK. Deve ser o mesmo que as credenciais de administrador local de anfitrião de kit de desenvolvimento.
2. Abra uma consola elevada do PowerShell e execute o  **&lt;letra de unidade > \AzureStack_Installer\asdk-installer.ps1** script (o que pode agora ser numa unidade diferente que C:\ na imagem CloudBuilder.vhdx). Clique em **Instalar**.

    ![](media/asdk-install/1.PNG) 

3. No fornecedor de identidade **tipo** caixa pendente, selecione **nuvem do Azure** ou **do AD FS**. Em **palavra-passe de administrador Local** escreva a palavra-passe de administrador local (que tem de coincidir com a palavra-passe de administrador local configurado atual) a **palavra-passe** caixa e, em seguida, clique em  **Seguinte**.
    - **Nuvem do Azure**: configura o Azure Active Directory (Azure AD) como o fornecedor de identidade. Para utilizar esta opção, terá uma ligação à internet, o nome completo de um Azure AD inquilino de diretório no formato *domainname*. onmicrosoft.com ou um Azure AD verificar o nome de domínio personalizado bem como credenciais de administrador global para especificado diretório. 
    - **AD FS**: O serviço de diretório de carimbo de data / predefinido é utilizado como o fornecedor de identidade. A conta predefinida para iniciar sessão com é azurestackadmin@azurestack.local, e a palavra-passe é fornecido como parte da configuração.

    ![](media/asdk-install/2.PNG) 
    
    > [!NOTE]
    > Para obter os melhores resultados, mesmo se pretender utilizar um ambiente desligado de pilha do Azure através do AD FS como o fornecedor de identidade, é melhor instalar o ASDK enquanto estiver ligado à internet. Dessa forma, a versão de avaliação do Windows Server 2016 incluída com a instalação do kit de desenvolvimento, pode ser ativada no momento da implementação.
4. Selecione um adaptador de rede a utilizar para o kit de desenvolvimento e, em seguida, clique em **seguinte**.

    ![](media/asdk-install/3.PNG)

5. Selecione o DHCP ou a configuração de rede estático para a máquina virtual de BGPNAT01.
    > [!TIP]
    > A VM BGPNAT01 é o router de limite que fornece capacidades NAT e VPN para a pilha do Azure.

    - **DHCP** (predefinição): A máquina virtual obtém a configuração de rede IP do servidor DHCP.
    - **Estático**: Utilize esta opção apenas se o DHCP não é possível atribuir um endereço IP válido para a pilha do Azure aceder à Internet. **Tem de ser especificado um endereço IP estático com o comprimento de subnetmask no formato CIDR (por exemplo, 10.0.0.5/24)**.
    - Escreva um **tempo IP do servidor** endereço. Este requisito necessário campo define o servidor de tempo a ser utilizado pelo kit de desenvolvimento. Este parâmetro tem de ser fornecido como um endereço IP do servidor de hora válido. Os nomes dos servidores não são suportadas.

      > [!TIP]
      > Para localizar um servidor de tempo de endereço IP, visite [pool.ntp.org](http:\\pool.ntp.org) ou ping time.windows.com. 

    - **Opcionalmente,**, defina os seguintes valores:
        - **ID de VLAN**: define o ID de VLAN. Utilize esta opção apenas se o anfitrião e AzS BGPNAT01 tem de configurar o ID de VLAN para aceder à rede física (e a internet). 
        - **Reencaminhador DNS**: um servidor DNS é criado como parte da implementação da pilha do Azure. Para permitir que os computadores dentro da solução para resolver nomes fora de carimbo, forneça o seu servidor DNS da infraestrutura existente. O servidor DNS no carimbo reencaminha os pedidos de resolução de nome desconhecido para este servidor.

    ![](media/asdk-install/4.PNG)

6. No **verificar as propriedades de cartão de interface de rede** página, verá uma barra de progresso. Quando a verificação estiver concluída, clique em **seguinte**.

    ![](media/asdk-install/5.PNG)

9. No **resumo** página, clique em **implementar** para iniciar a instalação de ASDK no computador de anfitrião do kit de desenvolvimento.

    ![](media/asdk-install/6.PNG)

    > [!TIP]
    > Aqui, também pode copiar os comandos de configuração do PowerShell que serão utilizados para instalar o kit de desenvolvimento. Esta ação é útil se alguma vez precisar de [Reimplementar ASDK no computador anfitrião através do PowerShell](asdk-deploy-powershell.md).

10. Se estiver a efetuar uma implementação do Azure AD, ser-lhe-emos pedido para introduzir as credenciais de conta de administrador global do Azure AD alguns minutos depois de iniciado o programa de configuração.

    ![](media/asdk-install/7.PNG)

11. O processo de implementação irá demorar algumas horas, durante o período de tempo do computador anfitrião reiniciará automaticamente uma vez. Se pretender monitorizar o progresso da implementação, inicie sessão como azurestack\AzureStackAdmin depois de reinicia o anfitrião do kit de desenvolvimento. Quando a implementação for bem sucedida, será apresentada a consola do PowerShell: **concluída: a ação 'Implementação'**. 
    > [!IMPORTANT]
    > Se iniciar sessão como um administrador local depois do computador está associado ao domínio, não verá o progresso da implementação. Não execute novamente a implementação, em vez disso, inicie sessão como azurestack\AzureStackAdmin validar que está em execução.

    ![](media/asdk-install/8.PNG)

Parabéns, instalou com êxito o ASDK!

Se a implementação falhar por algum motivo, pode [Reimplementar](asdk-redeploy.md) do zero ou utilize o PowerShell seguinte comandos, do mesma janela elevada do PowerShell, reiniciar a implementação do último passo com êxito:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

## <a name="next-steps"></a>Passos Seguintes
[Configuração de implementação de POST](asdk-post-deploy.md)
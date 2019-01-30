---
title: Instalar o Azure Stack Development Kit (ASDK) | Documentos da Microsoft
description: Descreve como instalar o Azure Stack Development Kit (ASDK).
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
ms.date: 09/10/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 09/10/2018
ms.openlocfilehash: 363e0868542f56df8c37639b2af7ac295be97da2
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55249910"
---
# <a name="install-the-azure-stack-development-kit-asdk"></a>Instalar o Azure Stack Development Kit (ASDK)
Após [preparar o computador do anfitrião ASDK](asdk-prepare-host.md), o ASDK pode ser implementado na imagem de CloudBuilder.vhdx através dos seguintes passos neste artigo.

## <a name="install-the-asdk"></a>Instalar o ASDK
Os passos neste artigo mostram como implementar o ASDK através de uma interface gráfica do usuário (GUI) fornecida ao transferir e executar o **asdk installer.ps1** script do PowerShell.

> [!NOTE]
> A interface de utilizador do instalador para o Development Kit do Azure Stack é um script aberto, com base no WCF e o PowerShell.


1. Depois do computador anfitrião é inicializado com êxito na imagem CloudBuilder.vhdx, início de sessão com as credenciais de administrador especificada quando [preparado o computador de anfitrião do kit de desenvolvimento](asdk-prepare-host.md) para a instalação de ASDK. Isso deve ser o mesmo que as credenciais de administrador local de anfitrião de kit de desenvolvimento.
2. Abra uma consola elevada do PowerShell e execute o  **&lt;letra de unidade > \AzureStack_Installer\asdk-installer.ps1** script (que agora pode ser numa unidade diferente que C:\ na imagem CloudBuilder.vhdx). Clique em **Instalar**.

    ![](media/asdk-install/1.PNG) 

3. No fornecedor de identidade **tipo** caixa de lista pendente, selecione **na Cloud do Azure** ou **do AD FS**. Sob **palavra-passe de administrador Local** escreva a palavra-passe de administrador local (que tem de coincidir com a palavra-passe de administrador local configurado atual) a **palavra-passe** caixa e, em seguida, clique em  **Próxima**.
    - **Cloud do Azure**: Configura o Azure Active Directory (Azure AD) como o fornecedor de identidade. Para utilizar esta opção, terá de uma ligação à internet, o nome completo de um Azure AD inquilino do diretório na forma de *domainname*. onmicrosoft.com ou um Azure AD verificar o nome de domínio personalizado e credenciais de administrador global do especificado diretório. Após a implementação, a permissão de administrador global do Azure Active Directory não é necessário. No entanto, algumas operações podem exigir a credencial de administrador global. Por exemplo, um script de instalador de fornecedor de recursos ou um novo recurso que requerem uma permissão para ser concedida. Pode temporariamente instate novamente as permissões de administrador global da conta ou utilizar uma conta de administrador global separado que é proprietária dos *predefinido da subscrição do fornecedor*.
    - **AD FS**: O serviço de diretório de carimbo de data / padrão é utilizado como o fornecedor de identidade. A conta predefinida para iniciar sessão com é azurestackadmin@azurestack.local, e a palavra-passe a utilizar é fornecido como parte da configuração.

    ![](media/asdk-install/2.PNG) 
    
    > [!NOTE]
    > Para obter melhores resultados, mesmo se desejar usar um ambiente desligado do Azure Stack através do AD FS como o fornecedor de identidade, é melhor instalar o ASDK enquanto estiver ligado à internet. Dessa forma, a versão de avaliação do Windows Server 2016 incluída com a instalação do kit de desenvolvimento pode ser ativada no momento da implementação.
4. Selecione um adaptador de rede a utilizar para o kit de desenvolvimento e, em seguida, clique em **seguinte**.

    ![](media/asdk-install/3.PNG)

5. Selecione o DHCP ou a configuração de rede estático para a máquina de virtual BGPNAT01.
    > [!TIP]
    > A VM BGPNAT01 é o router de limite que fornece capacidades VPN e de NAT para o Azure Stack.

    - **DHCP** (predefinição): A máquina virtual obtém a configuração de rede IP do servidor DHCP.
    - **Estático**: Utilize esta opção apenas se o DHCP não é possível atribuir um endereço IP válido para o Azure Stack aceder à Internet. **Tem de ser especificado um endereço IP estático com o comprimento de subnetmask no formato CIDR (por exemplo, 10.0.0.5/24)**.
    - Tipo na platnou **IP do servidor de tempo** endereço. Esta ação necessária campo define o servidor de horas para ser utilizado pelo kit de desenvolvimento. Este parâmetro tem de ser fornecido como um endereço IP do servidor de tempo válida. Não são suportados nomes de servidor.

      > [!TIP]
      > Para localizar um servidor de horas de endereço IP, visite [pool.ntp.org](http://pool.ntp.org) ou fazer ping time.windows.com. 

    - **Opcionalmente,**, defina os seguintes valores:
        - **ID DE VLAN**: Define o ID de VLAN. Utilize esta opção apenas se o anfitrião e AzS-BGPNAT01 tem de configurar o ID de VLAN para aceder à rede física (e internet). 
        - **Reencaminhador DNS**: Um servidor DNS é criado como parte da implementação do Azure Stack. Para permitir que os computadores dentro da solução para resolver nomes fora o carimbo de data /, fornece o servidor DNS de infraestrutura existente. O servidor DNS no carimbo reencaminha os pedidos de resolução de nome desconhecido para este servidor.

    ![](media/asdk-install/4.PNG)

6. Sobre o **verificar as propriedades de cartão de interface de rede** página, verá uma barra de progresso. Quando a verificação estiver concluída, clique em **seguinte**.

    ![](media/asdk-install/5.PNG)

9. No **resumo** página, clique em **implementar** para iniciar a instalação de ASDK no computador de anfitrião do kit de desenvolvimento.

    ![](media/asdk-install/6.PNG)

    > [!TIP]
    > Aqui também pode copiar os comandos de instalação do PowerShell que serão utilizados para instalar o kit de desenvolvimento. Isso é útil se alguma vez precisar [Reimplementar ASDK no computador anfitrião com o PowerShell](asdk-deploy-powershell.md).

10. Se estiver a efetuar uma implementação do Azure AD, será solicitado que introduza as credenciais de conta de administrador global do Azure AD alguns minutos após a instalação seja iniciada.

    ![](media/asdk-install/7.PNG)

11. O processo de implementação demorará algumas horas, período durante o qual o computador anfitrião será reinicializado automaticamente uma vez. Se quiser monitorizar o progresso da implementação, inicie sessão como azurestack\AzureStackAdmin depois de reinicia o anfitrião do kit de desenvolvimento. Quando a implementação com êxito, exibe a consola do PowerShell: **CONCLUA: A ação "Implementação"**. 
    > [!IMPORTANT]
    > Se iniciar sessão como um administrador local depois da máquina está associada ao domínio, não verá o progresso da implementação. Não execute novamente a implementação, em vez disso, inicie sessão como azurestack\AzureStackAdmin validar que está em execução.

    ![](media/asdk-install/8.PNG)

Parabéns, instalou com êxito o ASDK!

Se a implementação falhar por algum motivo, pode [Reimplementar](asdk-redeploy.md) partir do zero ou utilize o seguinte comandos do PowerShell, na mesma janela elevada do PowerShell, para reiniciar a implementação do último passo com êxito:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

## <a name="next-steps"></a>Passos Seguintes
[Configuração pós-implementação](asdk-post-deploy.md)

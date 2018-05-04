---
title: Volte a implementar o Kit de desenvolvimento de pilha do Azure (ASDK) | Microsoft Docs
description: Neste tutorial, irá aprender a reinstalar o ASDK.
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
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: fcf1abfe574dd3067f00df7c5ff2632b9cc2ec4f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-redeploy-the-asdk"></a>Tutorial: Reimplementar a ASDK
Neste tutorial, irá aprender a voltar a implementar o Azure pilha Development Kit (ASDK) num ambiente de não produção. Como atualizar o ASDK não é suportada, tem de completamente Reimplementar a mover para uma versão mais recente. Também pode implementar novamente o ASDK em qualquer altura em que pretende começar do zero.

> [!IMPORTANT]
> Atualizar o ASDK para uma nova versão não é suportada. Terá de voltar a implementar o ASDK no computador de anfitrião do kit de desenvolvimento sempre que pretende avaliar uma versão mais recente da pilha do Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Remover o registo do Azure 
> * Volte a implementar o ASDK

## <a name="remove-azure-registration"></a>Remover o registo do Azure 
Se tiver registado anteriormente a instalação de ASDK com o Azure, deve remover o recurso de registo antes de implementar novamente o ASDK. Volte a registar o ASDK para ativar sindicação marketplace quando implementar novamente o ASDK. Se não tiver registado anteriormente o ASDK com a sua subscrição do Azure, pode ignorar esta secção.

Para remover o recurso de registo, utilize o **remover AzsRegistration** cmdlet para anular o registo de pilha do Azure. Em seguida, utilize o **remover AzureRMRsourceGroup** cmdlet para eliminar o grupo de recursos de pilha do Azure da sua subscrição do Azure:

1. Abra uma consola do PowerShell como administrador num computador que tenha acesso ao ponto final com privilégios. Para ASDK, que é o computador de anfitrião do kit de desenvolvimento.

2. Execute os seguintes comandos do PowerShell para anular o registo da instalação do ASDK e eliminar o **azurestack** grupo de recursos da sua subscrição do Azure:

  ```Powershell    
  #Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Provide Azure subscription admin credentials
  Add-AzureRmAccount

  # Provide ASDK admin credentials
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"

  # Unregister Azure Stack
  Remove-AzsRegistration `
      -CloudAdminCredential $YourCloudAdminCredential `
      -PrivilegedEndpoint AzS-ERCS01

  # Remove the Azure Stack resource group
  Remove-AzureRmResourceGroup -Name azurestack -Force
  ```

3. Lhe for pedido para iniciar sessão na sua subscrição do Azure e a instalação de ASDK local quando o script é executado.
4. O script estiver concluído, deverá ver as mensagens semelhantes aos exemplos seguintes:

    ` De-Activating Azure Stack (this may take up to 10 minutes to complete).` ` Your environment is now unable to syndicate items and is no longer reporting usage data.` ` Remove registration resource from Azure...` ` "Deleting the resource..." on target "/subscriptions/<subscription information>"` ` ********** End Log: Remove-AzsRegistration ********* `



Pilha do Azure agora deve ser anulada a sua subscrição do Azure com êxito. Além disso, o grupo de recursos azurestack, criado quando se registou a ASDK com o Azure, também deve ser eliminado.

## <a name="redeploy-the-asdk"></a>Volte a implementar o ASDK
Para voltar a pilha do Azure, tem de iniciar através do zero conforme descrito abaixo. Os passos são diferentes consoante esteja ou não utilizou o script de instalador (asdk installer.ps1) de pilha do Azure para instalar o ASDK.

### <a name="redeploy-the-asdk-using-the-installer-script"></a>Reimplementar ASDK utilizando o script de instalador
1. No computador ASDK, abra uma consola do PowerShell elevada e navegue para o script de asdk installer.ps1 no **AzureStack_Installer** diretório localizado numa unidade não pertencente ao sistema. Execute o script e clique em **reiniciar**.

   ![Execute o script de asdk installer.ps1](media/asdk-redeploy/1.png)

2. Selecione o sistema operativo base (não **Azure pilha**) e clique em **seguinte**.

   ![Reiniciar para o sistema operativo anfitrião](media/asdk-redeploy/2.png)

3. Depois de reiniciado o anfitrião do kit de desenvolvimento para o sistema operativo base, inicie sessão como um administrador local. Localize e eliminar o **C:\CloudBuilder.vhdx** ficheiro que foi utilizado como parte da implementação anterior. 

4. Repita os mesmos passos, demorou a primeira [implementar o ASDK](asdk-deploy.md).

### <a name="redeploy-the-asdk-without-using-the-installer"></a>Volte a implementar o ASDK sem utilizar o instalador
Se não utilizou o script de asdk installer.ps1 para instalar o ASDK, tem de reconfigurar manualmente o computador de anfitrião do kit de desenvolvimento antes de implementar novamente o ASDK.

1. Inicie o utilitário de configuração do sistema executando **msconfig.exe** no computador ASDK. No **arranque** separador, selecione o sistema operativo do computador anfitrião (não pilha do Azure), clique em **configurado como predefinido**e, em seguida, clique em **OK**. Clique em **reiniciar** quando lhe for pedido.

      ![Definir a configuração de arranque](media/asdk-redeploy/4.png)

2. Depois de reiniciado o anfitrião do kit de desenvolvimento para o sistema operativo base, inicie sessão como um administrador local para o computador de anfitrião do kit de desenvolvimento. Localize e eliminar o **C:\CloudBuilder.vhdx** ficheiro que foi utilizado como parte da implementação anterior. 

3. Repita os mesmos passos, demorou a primeira [implementar ASDK através do PowerShell](asdk-deploy-powershell.md).


## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Remover o registo do Azure 
> * Volte a implementar o ASDK

Avançar para o próximo tutorial para saber como adicionar um item do marketplace pilha do Azure.

> [!div class="nextstepaction"]
> [Adicionar um item do marketplace pilha do Azure](asdk-marketplace-item.md)





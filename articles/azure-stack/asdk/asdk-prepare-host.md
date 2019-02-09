---
title: Preparar o computador de anfitrião do Azure Stack Development Kit (ASDK) | Documentos da Microsoft
description: Descreve como preparar o computador de anfitrião do Azure Stack Development Kit (ASDK) para a instalação de ASDK.
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
ms.date: 01/21/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 10/22/2018
ms.openlocfilehash: 36012c023025b8304dfaf9cc63997f600ef6cbe8
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55962728"
---
# <a name="prepare-the-asdk-host-computer"></a>Preparar o computador do anfitrião ASDK
Antes de poder instalar o ASDK no computador anfitrião, o anfitrião ASDK deve estar preparado para a instalação. Quando o computador de anfitrião do kit de desenvolvimento foi preparado, ele será inicializado do disco de rígido da máquina virtual CloudBuilder.vhdx para iniciar a implantação de ASDK.

## <a name="prepare-the-development-kit-host-computer"></a>Preparar o computador de anfitrião do kit de desenvolvimento
Antes de poder instalar o ASDK no computador anfitrião, o ambiente de computador do anfitrião ASDK deve estar preparado.
1. Inicie sessão como um administrador Local para o seu computador de anfitrião do kit de desenvolvimento.
2. Certifique-se de que o ficheiro de CloudBuilder.vhdx foi movido para a raiz da unidade C:\ (C:\CloudBuilder.vhdx).
3. Execute o seguinte script para transferir o ficheiro de instalador do development kit (asdk installer.ps1) a partir do [repositório de ferramentas do GitHub do Azure Stack](https://github.com/Azure/AzureStack-Tools) para o **C:\AzureStack_Installer** pasta no seu computador do anfitrião de kit de desenvolvimento:

   > [!IMPORTANT]
   > Certifique-se de que transfira o ficheiro de asdk installer.ps1 sempre que instala o ASDK. São feitas alterações frequentes este script e a versão mais atual deve ser utilizada para cada implementação ASDK. As versões mais antigas do script podem não funcionar com a versão mais atual.

   ```powershell
   # Variables
   $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
   $LocalPath = 'C:\AzureStack_Installer'
   # Create folder
   New-Item $LocalPath -Type directory
   # Enforce usage of TLSv1.2 to download from GitHub
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
   # Download file
   Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
   ```

4. A partir de uma consola elevada do PowerShell, inicie o **C:\AzureStack_Installer\asdk-installer.ps1** script e, em seguida, clique em **Preparar ambiente**.

    ![](media/asdk-prepare-host/1.PNG) 

5. Sobre o **selecione Cloudbuilder vhdx** página do instalador, procure e selecione o **cloudbuilder.vhdx** ficheiro que transferiu e extraiu no [os passos anteriores](asdk-download.md). Nesta página, também pode, opcionalmente, ativar a **adicionar controladores** caixa de verificação se precisar de adicionar controladores adicionais para o computador de anfitrião do kit de desenvolvimento. Clique em **Seguinte**.  

    ![](media/asdk-prepare-host/2.PNG)

6. Sobre o **definições opcionais** , indique o administrador local informações para o computador de anfitrião do kit de desenvolvimento de conta e, em seguida, clique em **próxima**.<br><br>Se não fornecer as credenciais de administrador local neste passo, precisará direct ou acesso KVM ao anfitrião depois do computador é reiniciado como parte de como configurar o kit de desenvolvimento.

   ![](media/asdk-prepare-host/3.PNG)

    Também pode fornecer valores para as seguintes definições opcionais:
    - **ComputerName**: Esta opção define o nome para o anfitrião do kit de desenvolvimento. O nome tem de cumprir os requisitos de FQDN e tem de ser 15 caracteres ou menos. A predefinição é um nome de computador aleatório gerado pelo Windows.
    - **Configuração de IP estático**: Define a sua implementação utilizar um endereço IP estático. Caso contrário, quando o instalador reinicia no cloudbuilder.vhdx, as interfaces de rede são configuradas com DHCP. Se optar por utilizar uma configuração de IP estático, opções adicionais são apresentadas em que deve também:
      - Selecione um adaptador de rede. Certifique-se de que pode ligar para o adaptador antes de clicar em **seguinte**.
      - Certifique-se de que o apresentado **endereço IP**, **Gateway**, e **DNS** valores estão corretos e, em seguida, clique em **seguinte**.
13. Clique em **seguinte** para iniciar o processo de preparação.
14. Quando a preparação indica **concluído**, clique em **próxima**.

    ![](media/asdk-prepare-host/4.PNG)

15. Clique em **reiniciar agora** para inicializar o computador de anfitrião do kit de desenvolvimento no cloudbuilder.vhdx e [continuar o processo de implantação](asdk-install.md).

    ![](media/asdk-prepare-host/5.PNG)


## <a name="next-steps"></a>Passos Seguintes
[Instalar o ASDK](asdk-install.md)

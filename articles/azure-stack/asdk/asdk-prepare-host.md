---
title: Preparar o computador de anfitrião do Kit de desenvolvimento de pilha do Azure (ASDK) | Microsoft Docs
description: Descreve como preparar o computador de anfitrião do Kit de desenvolvimento de pilha do Azure (ASDK) para a instalação de ASDK.
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
ms.openlocfilehash: 5de25f574cb876701ffce74f1dca8c4bb9764157
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30165675"
---
# <a name="prepare-the-asdk-host-computer"></a>Preparar o computador de anfitrião ASDK
Antes de poder instalar o ASDK no computador anfitrião, o ambiente de ASDK deve estar preparado para a instalação. Quando o computador de anfitrião do kit de desenvolvimento tenha sido preparado, arrancará da unidade de disco rígida da máquina virtual CloudBuilder.vhdx para começar a implementação de ASDK.

## <a name="prepare-the-development-kit-host-computer"></a>Preparar o computador de anfitrião do kit de desenvolvimento
Antes de poder instalar o ASDK no computador anfitrião, o ambiente de computador do anfitrião ASDK tem de ser preparado.
1. Inicie sessão como um administrador Local para o seu computador de anfitrião do kit de desenvolvimento.
2. Certifique-se de que o ficheiro CloudBuilder.vhdx foi movido para a raiz da unidade C:\ (C:\CloudBuilder.vhdx).
3. Execute o seguinte script para transferir o ficheiro de instalador de kit de desenvolvimento (asdk installer.ps1) da [repositório do GitHub de pilha do Azure ferramentas](https://github.com/Azure/AzureStack-Tools) para o **C:\AzureStack_Installer** pasta no seu computador do anfitrião de kit de desenvolvimento:

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

4. A partir de uma consola elevada do PowerShell, inicie o **C:\AzureStack_Installer\asdk-installer.ps1** script e, em seguida, clique em **preparar o ambiente**.

    ![](media/asdk-prepare-host/1.PNG) 

5. No **selecione Cloudbuilder vhdx** página do instalador, procure e selecione o **cloudbuilder.vhdx** ficheiro que transferiu e extraiu no [os passos anteriores](asdk-download.md). Nesta página, também pode, opcionalmente, ativar o **adicionar controladores** caixa de verificação se precisar de adicionar controladores ao computador de anfitrião do kit de desenvolvimento. Clique em **Seguinte**.  

    ![](media/asdk-prepare-host/2.PNG)

6. No **definições opcionais** , indique o administrador local informações para o computador de anfitrião do kit de desenvolvimento de conta e, em seguida, clique em **seguinte**. Também pode fornecer valores para as seguintes definições opcionais:
  - **ComputerName**: esta opção define o nome para o anfitrião do kit de desenvolvimento. O nome tem de cumprir os requisitos de FQDN e tem de ser 15 carateres ou menos. A predefinição é um nome de computador aleatório gerado pelo Windows.
  - **Fuso horário**: define o fuso horário para o anfitrião do kit de desenvolvimento. A predefinição é (UTC-8:00) hora do Pacífico (E.U.A. e Canadá).
  - **Configuração de IP estático**: define a sua implementação utilizar um endereço IP estático. Caso contrário, quando o instalador reinicia com o cloudbuilder.vhx, as interfaces de rede são configuradas com DHCP.

    ![](media/asdk-prepare-host/3.PNG)

  > [!IMPORTANT]
  > Se não fornecer as credenciais de administrador local neste passo, terá de direta ou de KVM acesso ao anfitrião depois de reiniciar o computador como parte de como configurar o kit de desenvolvimento.

7. Se optou por uma configuração de IP estático no passo anterior, tem de agora:
    - Selecione um adaptador de rede. Certifique-se de que pode ligar ao adaptador antes de clicar em **seguinte**.
    - Certifique-se de que o **endereço IP**, **Gateway**, e **DNS** valores estão corretos e, em seguida, clique em **seguinte**.
13. Clique em **seguinte** para iniciar o processo de preparação.
14. Quando a preparação indica **concluído**, clique em **seguinte**.
15. Clique em **reiniciar agora** para arrancar o computador de anfitrião do kit de desenvolvimento de cloudbuilder.vhdx e [continuar o processo de implementação](asdk-install.md).

    ![](media/asdk-prepare-host/4.PNG)


## <a name="next-steps"></a>Passos Seguintes
[Instalar o ASDK](asdk-install.md)
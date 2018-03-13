---
title: "Ativar a ligação de ambiente de trabalho remoto para uma função nos serviços em nuvem do Azure | Microsoft Docs"
description: "Como configurar a aplicação do serviço em nuvem do Azure para permitir ligações de ambiente de trabalho remotas"
services: cloud-services
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2018
ms.author: kraigb
ms.openlocfilehash: bcb7d24a302b57a1739c9c98763cd658c29b17bd
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-visual-studio"></a>Ativar a ligação de ambiente de trabalho remoto para uma função nos serviços de nuvem do Azure com o Visual Studio

> [!div class="op_single_selector"]
> * [Portal do Azure](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Ambiente de trabalho remoto permite-lhe aceder ao ambiente de trabalho de uma função em execução no Azure. Pode utilizar uma ligação de ambiente de trabalho remoto para resolver problemas e diagnosticar problemas com a sua aplicação enquanto estiver em execução.

O Assistente de publicar o Visual Studio fornece para serviços em nuvem inclui uma opção para ativar o ambiente de trabalho remoto durante o processo de publicação, utilizando as credenciais indicadas por si. Utilizar esta opção é adequado ao utilizar o Visual Studio 2017 versão 15.4 e anterior.

No entanto, com o Visual Studio 2017 versão 15.5 e posterior, é recomendado que evite ativar o ambiente de trabalho remoto através do assistente publicar, se estiver a trabalhar apenas como um programador único. Para qualquer situação em que o projeto poderá ser aberto por outros programadores, ativar em vez disso, ambiente de trabalho remoto através do portal do Azure, através do PowerShell ou a partir de uma definição de versão num fluxo de trabalho a implementação contínua. Esta recomendação é devido a uma alteração na forma como Visual Studio comunica com o ambiente de trabalho remoto no serviço em nuvem, VM, conforme é explicado neste artigo.

## <a name="configure-remote-desktop-through-visual-studio-2017-version-154-and-earlier"></a>Configurar o ambiente de trabalho remoto através do Visual Studio 2017 versão 15.4 e anterior

Quando utilizar o Visual Studio 2017 versão 15.4 e anterior, pode utilizar o **ativar o ambiente de trabalho remoto para todas as funções** opção no Assistente de publicação. Pode continuar a utilizar o assistente com o Visual Studio 2017 versão 15.5 e posterior, mas não utilize a opção de ambiente de trabalho remoto.

1. No Visual Studio, inicie o Assistente de publicação ao clicar projeto de serviço em nuvem no Explorador de soluções e escolher **publicar**.

2. Iniciar sessão na sua subscrição do Azure, se for necessário e selecione **seguinte**.

3. No **definições** página, selecione **ativar o ambiente de trabalho remoto para todas as funções**, em seguida, selecione o **definições...**  ligação abra o **configuração do ambiente de trabalho remoto** caixa de diálogo.

4. Na parte inferior da caixa de diálogo, selecione **mais opções**. Este comando apresenta uma lista pendente, na qual criar ou escolha um certificado para que possam encriptar as informações de credenciais quando a ligação através do ambiente de trabalho remoto.

   > [!Note]
   > Os certificados que sejam necessárias para uma ligação de ambiente de trabalho remoto são diferentes dos certificados que utiliza para outras operações do Azure. O certificado de acesso remoto tem de ter uma chave privada.

5. Selecione um certificado da lista ou escolha  **&lt;criar... &gt;**. Se criar um novo certificado, forneça um nome amigável para o novo certificado quando lhe for pedido e selecione **OK**. O novo certificado é apresentado na caixa de lista pendente.

6. Forneça um nome de utilizador e uma palavra-passe. Não é possível utilizar uma conta existente. Não utilize "Administrador" como nome de utilizador para a nova conta.

7. Selecione uma data em que a conta irá expirar e depois das ligações de ambiente de trabalho remoto serão bloqueadas.

8. Depois de que forneceu as informações necessárias, selecione **OK**. Visual Studio adiciona as definições de ambiente de trabalho remoto ao seu projeto `.cscfg` e `.csdef` ficheiros, incluindo a palavra-passe é encriptada utilizando o certificado que escolheu.

9. Concluir os passos restantes utilizando o **seguinte** botão, em seguida, selecione **publicar** quando estiver pronto para publicar o seu serviço em nuvem. Se não estiver pronto para publicar, selecione **Cancelar** e resposta **Sim** quando lhe for pedido para guardar as alterações. Pode publicar o serviço em nuvem mais tarde com estas definições.

## <a name="configure-remote-desktop-when-using-visual-studio-2017-version-155-and-later"></a>Configurar o ambiente de trabalho remoto quando utilizar o Visual Studio 2017 versão 15.5 e posterior

Com o Visual Studio 2017 versão 15.5 e posterior, pode continuar a utilizar o Assistente de publicação com um projeto de serviço em nuvem. Também pode utilizar o **ativar o ambiente de trabalho remoto para todas as funções** opção se estiver a trabalhar apenas como um programador único.

Se estiver a funcionar como parte de uma equipa, em vez disso, deve ativar o ambiente de trabalho remoto no serviço em nuvem do Azure utilizando o [portal do Azure](cloud-services-role-enable-remote-desktop-new-portal.md) ou [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md).

Esta recomendação é devido a uma alteração na como o Visual Studio 2017 versão 15.5 e posterior comunica com o serviço em nuvem VM. Ao ativar o ambiente de trabalho remoto através do assistente publicar, versões anteriores do Visual Studio comunicam com a VM através do qual foi chamado o "RDP Plug-in." Visual Studio 2017 versão 15.5 e posterior comunica utilizando em vez disso, a extensão"RDP" que é mais flexível e mais segura. Esta alteração também está alinhada com o facto de que o portal do Azure e métodos do PowerShell para ativar o ambiente de trabalho remoto também utilizam a extensão RDP.

Quando o Visual Studio comunica com a extensão RDP, transmitir uma palavra-passe de texto simples através de SSL. No entanto, os ficheiros de configuração do projeto armazenam apenas uma palavra-passe encriptada, que pode ser desencriptada em texto simples, apenas com o certificado local que foi utilizado originalmente para o encriptar.

Se implementar o projeto de serviço em nuvem no mesmo computador de desenvolvimento cada hora, esse certificado local está disponível. Neste caso, pode continuar a utilizar o **ativar o ambiente de trabalho remoto para todas as funções** opção no Assistente de publicação.

Se pretendem que implementar o projeto de serviço de nuvem a partir de computadores diferentes, ou outros programadores, no entanto, em seguida, os outros computadores não terá do certificado necessário para desencriptar a palavra-passe. Como resultado, consulte a seguinte mensagem de erro:

```output
Applying remote desktop protocol (RDP) extension.
Certificate with thumbprint [thumbprint] doesn't exist.
```

Pode alterar a palavra-passe sempre que implementar o serviço em nuvem, mas essa ação torna-se inconveniente para todas as pessoas que necessita de utilizar o ambiente de trabalho remoto.

Se estiver a partilhar o projeto com um agrupamento, em seguida, é melhor desmarque a opção do Assistente Publicar e em vez disso, ative o ambiente de trabalho remoto diretamente através do [portal do Azure](cloud-services-role-enable-remote-desktop-new-portal.md) ou utilizando [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md).

### <a name="deploying-from-a-build-server-with-visual-studio-2017-version-155-and-later"></a>Implementação a partir de um servidor de compilação com o Visual Studio 2017 versão 15.5 e posterior

Pode implementar um projeto de serviço em nuvem de um servidor de compilação (por exemplo, com o Visual Studio Team Services) no qual Visual Studio de 2017 versão 15.5 ou posterior está instalado no agente de compilação. Com esta organização, a implementação acontece no mesmo computador no qual o certificado de encriptação está disponível.

Para utilizar a extensão RDP a partir do Visual Studio Team Services, incluem os seguintes detalhes na sua definição de compilação:

1. Incluir `/p:ForceRDPExtensionOverPlugin=true` nos seus argumentos de MSBuild para se certificar de que a implementação funciona com a extensão RDP em vez do plug-in RDP. Por exemplo:

    ```
    msbuild AzureCloudService5.ccproj /t:Publish /p:TargetProfile=Cloud /p:DebugType=None
        /p:SkipInvalidConfigurations=true /p:ForceRDPExtensionOverPlugin=true
    ```

1. Depois dos passos da compilação, adicione o **implementação de serviço de nuvem do Azure** passo e definir as respetivas propriedades.

1. Após o passo de implementação, adicione um **Azure Powershell** passo, defina o respetivo **nome a apresentar** propriedade como "Do Azure: Ativar RDP extensão de implementação" (ou outro nome adequado) e selecione o seu Azure adequado subscrição.

1. Definir **tipo de Script** para "Inline" e cole o código abaixo para o **o Inline Script de** campo. (Também pode criar um `.ps1` de ficheiros no seu projeto com este script, defina **tipo de Script** para "Caminho do ficheiro de Script" e defina **caminho do Script** para apontar para o ficheiro.)

    ```ps
    Param(
        [Parameter(Mandatory=$True)]
        [string]$username,

        [Parameter(Mandatory=$True)]
        [string]$password,

        [Parameter(Mandatory=$True)]
        [string]$serviceName,

        [Datetime]$expiry = ($(Get-Date).AddYears(1))
    )

    Write-Host "Service Name: $serviceName"
    Write-Host "User Name: $username"
    Write-Host "Expiry: $expiry"

    $securepassword = ConvertTo-SecureString -String $password -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential $username,$securepassword

    # Try to remote existing RDP Extensions
    try
    {
        $existingRDPExtension = Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
        if ($existingRDPExtension -ne $null)
        {
            Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
        }
    }
    catch
    {
    }

    Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry -Verbose
    ```

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Ligar a uma função do Azure, utilizando o ambiente de trabalho remoto

Depois de publicar o serviço de nuvem no Azure e tiver ativado o ambiente de trabalho remoto, pode utilizar o Explorador de servidores do Visual Studio para iniciar sessão na VM de serviço de nuvem:

1. No Explorador de servidores, expanda o **Azure** nó e, em seguida, expanda o nó para um serviço em nuvem e uma das respetivas funções para apresentar uma lista de instâncias.

2. Um nó de instância com o botão direito e selecione **ligar utilizando o ambiente de trabalho remoto**.

3. Introduza o nome de utilizador e palavra-passe que criou anteriormente. Tem agora sessão iniciada na sua sessão remota.

## <a name="additional-resources"></a>Recursos adicionais

[Como configurar um Serviços Cloud](cloud-services-how-to-configure-portal.md)
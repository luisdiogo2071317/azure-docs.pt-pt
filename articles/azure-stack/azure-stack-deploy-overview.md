---
title: Avaliar o Kit de desenvolvimento do Azure Stack | Documentos da Microsoft
description: Saiba como implementar o Development Kit do Azure Stack para fins de avaliação.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/05/2018
ms.author: jeffgilb
ms.custom: mvc
ms.reviewer: misainat
ms.openlocfilehash: 8b33937a4da807578d371a95f2c06de451404036
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037469"
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Início rápido: avaliar o Development Kit do Azure Stack

O [do Azure Stack Development Kit (ASDK)](.\asdk\asdk-what-is.md) é um ambiente de teste e desenvolvimento que pode implementar para avaliar e demonstrar funcionalidades do Azure Stack e serviços. Para começar a utilizar com o ASDK, terá de preparar o anfitrião de hardware do computador e, em seguida, executar alguns scripts (instalação demora várias horas). Depois disso, pode iniciar sessão nos portais de administrador ou usuário para começar a utilizar o Azure Stack.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="asdk-host-computer-requirements"></a>Requisitos de computador do anfitrião ASDK

Antes de instalar o ASDK, terá de preparar o computador que irá alojar o kit de desenvolvimento. O computador de anfitrião do kit de desenvolvimento têm de cumprir o hardware, software, e os requisitos de rede descrita no  **[reveja as considerações de planeamento de implementação de ASDK](.\asdk\asdk-deploy-considerations.md)**.

> [!TIP]
> Pode utilizar o [ferramenta de verificação de requisitos de implementação do Azure Stack](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) depois de instalar o sistema operativo no computador do anfitrião de kit de desenvolvimento para confirmar que o hardware cumpre todos os requisitos.

### <a name="account-requirements"></a>Requisitos de conta

Também precisa escolher entre a utilização do Azure Active Directory (Azure AD) ou serviços de Federação do Active Directory (AD FS) como a solução de identidade para a sua implementação. Reveja os requisitos de conta no  **[considerações de planeamento de implementação](.\asdk\asdk-deploy-considerations.md#account-requirements)**

## <a name="download-and-extract-the-deployment-package"></a>Baixe e extraia o pacote de implementação

Depois de preparar o seu computador de anfitrião do kit de desenvolvimento, baixe e extraia o pacote de implementação de ASDK. O pacote de implementação inclui o arquivo de Cloudbuilder.vhdx, que é uma unidade de disco rígida virtual (VHD) com um sistema operativo de arranque, e os ficheiros de instalação do Azure Stack.

Pode transferir o pacote de implementação para o anfitrião do kit de desenvolvimento ou para outro computador. Os arquivos de implantação extraídos demorar até 60 GB de espaço livre em disco, pelo que utilizar outro computador pode ajudar a reduzir os requisitos de armazenamento no anfitrião do kit de desenvolvimento.

**[Baixe e extraia o Azure Stack Development Kit (ASDK)](.\asdk\asdk-download.md)**

## <a name="prepare-the-host-computer"></a>Preparar o computador anfitrião

Antes de instalar o ASDK, tem de ser preparado o ambiente de host e o sistema configurado para efetuar o arranque do kit de desenvolvimento VHD. Quando reiniciar o anfitrião, efetua o arranque de CloudBuilder.vhdx e pode começar a implementar o ASDK.

**[Preparar o computador do anfitrião ASDK](.\asdk\asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>Instalar o ASDK no computador anfitrião

Depois do computador anfitrião arranca a partir do VHD, pode implementar o development kit para o ambiente virtual Cloudbuilder. Pode implementar o ASDK usando a interface gráfica do usuário (GUI), fornecida ao executar o script do PowerShell asdk installer.ps1 ou a partir de [da linha de comandos do PowerShell](.\asdk\asdk-deploy-powershell.md)

> [!NOTE]
> Depois do anfitrião arranca a partir da imagem de Cloudbuilder.vhdx, tem a opção de configuração [definições de telemetria do Azure Stack](.\asdk\asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *antes de* o ASDK a instalar.

**[Instalar o Azure Stack Development Kit (ASDK)](.\asdk\asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Efetuar configurações de pós-implementação

Depois de instalar o ASDK, existem algumas verificações de pós-instalação recomendadas e alterações de configuração que devem ser feitas.

**Ferramentas**

Instalar as ferramentas do Azure Stack do PowerShell e do GitHub e verificar o sucesso da sua instalação com o cmdlet test-AzureStack.

**Solução de identidade**

Para uma implementação que utiliza o Azure AD, tem de ativar ambas do Azure Stack administrador portais e do inquilino. Esta ativação dá consentimento a que as permissões corretas (listadas na página de consentimento) para todos os utilizadores do diretório são o portal do Azure Stack e o Azure Resource Manager.

**Expiração de palavra-passe**

Deve redefinir a política de expiração de palavra-passe para se certificar de que a palavra-passe para o anfitrião do kit de desenvolvimento não expira antes de terminar o período de avaliação.

> [!NOTE]
> Também tem a opção de configuração [definições de telemetria do Azure Stack](.\asdk\asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *depois* o ASDK a instalar.

**[Tarefas de implementação de POST-ASDK](.\asdk\asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Registe-se com o Azure

Tem de registar Azure Stack com o Azure para que possa [transferir itens do marketplace do Azure](.\asdk\asdk-marketplace-item.md) ao Azure Stack.

**[Registar o Azure Stack com o Azure](.\asdk\asdk-register.md)**

## <a name="next-steps"></a>Passos Seguintes

Parabéns! Ao concluir os passos neste guia de introdução tiver um ambiente de ASDK com um [administrador](https://adminportal.local.azurestack.external) portal e um [utilizador](https://portal.local.azurestack.external) portal.

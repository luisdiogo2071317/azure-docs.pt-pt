---
title: Avaliar o Kit de desenvolvimento de pilha do Azure | Microsoft Docs
description: Saiba como implementar o Kit de desenvolvimento de pilha do Azure para fins de avaliação.
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
ms.date: 06/04/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: a0e742ab3ac43cc7977761dd94c9689e3a7c2e0b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235190"
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Início rápido: avaliar o Kit de desenvolvimento de pilha do Azure

O [Kit de desenvolvimento de pilha do Azure (ASDK)](.\asdk\asdk-what-is.md) é um ambiente de teste e desenvolvimento que pode implementar para avaliar e demonstrar serviços e funcionalidades de pilha do Azure. Para começar a utilizar com o ASDK, terá de preparar o anfitrião de hardware do computador e, em seguida, executar alguns scripts (a instalação demora várias horas). Depois disso, pode iniciar sessão para os portais de administrador ou utilizador começar a utilizar a pilha do Azure.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="asdk-host-computer-requirements"></a>Requisitos de computador do anfitrião ASDK

Antes de instalar o ASDK, terá de preparar o computador que alojará o kit de desenvolvimento. O computador de anfitrião do kit de desenvolvimento têm de cumprir o hardware, software e requisitos de rede descrita em  **[rever a considerações de planeamento de implementação de ASDK](.\asdk\asdk-deploy-considerations.md)**.

> [!TIP]
> Pode utilizar o [ferramenta de verificação de requisitos de implementação do Azure pilha](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) depois de instalar o sistema operativo do computador de anfitrião do kit de desenvolvimento para confirmar que o seu hardware cumpre todos os requisitos.

### <a name="account-requirements"></a>Requisitos de conta

Também tem de escolher entre a utilização do Azure Active Directory (Azure AD) ou os serviços de Federação do Active Directory (AD FS) como a solução de identidade para a sua implementação. Reveja os requisitos de conta no  **[considerações de planeamento de implementação](.\asdk\asdk-deploy-considerations.md#account-requirements)**

## <a name="download-and-extract-the-deployment-package"></a>Transferir e extraia o pacote de implementação

Depois de preparar o seu computador de anfitrião do kit de desenvolvimento, transferência e extraiu o pacote de implementação ASDK. O pacote de implementação inclui o ficheiro Cloudbuilder.vhdx, que é uma unidade de disco rígida virtual (VHD) com um sistema operativo de arranque, e os ficheiros de instalação de pilha do Azure.

Pode transferir o pacote de implementação para o anfitrião do kit de desenvolvimento ou para outro computador. Os ficheiros extraídos implementação demorar até 60 GB de espaço livre em disco, pelo que utilizar outro computador pode ajudar a reduzir os requisitos de armazenamento no anfitrião do kit de desenvolvimento.

**[Transferir e extrair o Kit de desenvolvimento de pilha do Azure (ASDK)](.\asdk\asdk-download.md)**

## <a name="prepare-the-host-computer"></a>Preparar o computador anfitrião

Antes de poder instalar o ASDK, tem de ser preparado o ambiente de anfitrião e o sistema configurado para arranque do kit de desenvolvimento VHD. Quando reiniciar o anfitrião, iniciam a partir de CloudBuilder.vhdx e pode começar a implementar o ASDK.

**[Preparar o computador de anfitrião ASDK](.\asdk\asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>Instalar o ASDK no computador anfitrião

Depois do computador anfitrião efetua o arranque do VHD, pode implementar o kit de desenvolvimento para o ambiente virtual Cloudbuilder. Pode implementar ASDK utilizando a interface de utilizador gráfica (GUI) fornecida ao executar o script do PowerShell asdk installer.ps1 ou a partir de [a linha de comandos do PowerShell](.\asdk\asdk-deploy-powershell.md)

> [!NOTE]
> Depois do anfitrião arranca a partir da imagem Cloudbuilder.vhdx, tem a opção de configuração [definições de telemetria de pilha do Azure](.\asdk\asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *antes* o ASDK a instalar.

**[Instalar o Kit de desenvolvimento de pilha do Azure (ASDK)](.\asdk\asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Efetuar configurações de pós-implementação

Depois de instalar o ASDK, existem algumas verificações de pós-instalação recomendadas e alterações de configuração que devem ser efetuadas.

**Ferramentas**

Instale as ferramentas do PowerShell de pilha do Azure e do GitHub e verificar o sucesso da sua instalação utilizando o cmdlet de teste AzureStack.

**Solução de identidade**

Para uma implementação que utiliza o Azure AD, terá de ativar ambas as os pilha do Azure administrador portais e de inquilino. Esta ativação permitir para dar o portal de pilha do Azure e o Azure Resource Manager, as permissões corretas (listadas na página de consentimento) para todos os utilizadores do diretório.

**Expiração da palavra-passe**

Deve repor a política de expiração de palavra-passe para se certificar de que a palavra-passe para o anfitrião do kit de desenvolvimento não expira antes de terminar o período de avaliação.

> [!NOTE]
> Também tem a opção de configuração [definições de telemetria de pilha do Azure](.\asdk\asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *depois* o ASDK a instalar.

**[Tarefas de implementação POST ASDK](.\asdk\asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Registar com o Azure

Tem de registar pilha do Azure com o Azure, para que possa [transferir itens do Azure marketplace](.\asdk\asdk-marketplace-item.md) à pilha do Azure.

**[Registar a pilha do Azure com o Azure](.\asdk\asdk-register.md)**

## <a name="next-steps"></a>Passos Seguintes

Parabéns! Ao concluir os passos neste guia de introdução tiver um ambiente de ASDK com um [administrador](https://adminportal.local.azurestack.external) portal e um [utilizador](https://portal.local.azurestack.external) portal.

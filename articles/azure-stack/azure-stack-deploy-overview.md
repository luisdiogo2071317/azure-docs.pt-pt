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
ms.date: 03/22/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: 1deabcf64b3fbf3cbc89232c340a8882cd2591e8
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Início rápido: Avaliar o Kit de desenvolvimento de pilha do Azure
O [Kit de desenvolvimento de pilha do Azure (ASDK)](.\asdk\asdk-what-is.md) é um ambiente de teste e desenvolvimento que pode implementar para avaliar e demonstrar serviços e funcionalidades de pilha do Azure. Para começar a utilizar com o ASDK, terá de preparar o anfitrião de hardware do computador e, em seguida, executar alguns scripts (a instalação demora várias horas). Depois disso, pode inscrever-para os portais de administrador e utilizador para começar a utilizar a pilha do Azure.

## <a name="prerequisites"></a>Pré-requisitos 
Antes de instalar o ASDK, terá de preparar o computador que alojará o kit de desenvolvimento (o anfitrião do kit de desenvolvimento). O computador de anfitrião do kit de desenvolvimento têm de cumprir mínimos de hardware, software e requisitos de rede. 

Também tem de escolher entre a utilização do Azure Active Directory (Azure AD) ou os serviços de Federação do Active Directory (AD FS) como a solução de identidade para a sua implementação. 

Lembre-se de que o anfitrião do kit de desenvolvimento cumpre os requisitos mínimos de hardware e que efetuou a decisão de solução de identidade efetuada antes de iniciar a implementação para que o processo de instalação seja executado facilmente. 

**[Reveja a considerações de planeamento de implementação de ASDK](.\asdk\asdk-deploy-considerations.md)**

> [!TIP]
> Pode utilizar o [ferramenta de verificação de requisitos de implementação do Azure pilha](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) depois de instalar o sistema operativo do computador de anfitrião do kit de desenvolvimento para confirmar que o seu hardware cumpre todos os requisitos.

## <a name="download-and-extract-the-deployment-package"></a>Transferir e extraia o pacote de implementação
Depois de se certificar de que o computador de anfitrião de kit de desenvolvimento cumpre os requisitos básicos para instalar o ASDK, o passo seguinte é a mesma e extraiu o pacote de implementação ASDK. O pacote de implementação inclui o ficheiro de Cloudbuilder.vhdx, que é uma unidade de disco rígida virtual que inclui um sistema operativo de arranque e os ficheiros de instalação de pilha do Azure.

Pode transferir o pacote de implementação para o anfitrião do kit de desenvolvimento ou para outro computador. Os ficheiros extraídos implementação demorar até 60 GB de espaço livre em disco, pelo que utilizar outro computador pode ajudar a reduzir os requisitos de hardware para o anfitrião do kit de desenvolvimento.

**[Transferir e extrair o Kit de desenvolvimento de pilha do Azure (ASDK)](.\asdk\asdk-download.md)**

## <a name="prepare-the-development-kit-host-computer"></a>Preparar o computador de anfitrião do kit de desenvolvimento
Antes de poder instalar o ASDK no computador anfitrião, o ambiente tem de ser preparado e o sistema configurado para arranque de VHD. Depois do computador de anfitrião do kit de desenvolvimento tenha sido preparado,-arranca a partir do disco rígido da máquina virtual de CloudBuilder.vhdx para que pode iniciar a implementação de ASDK.

**[Preparar o computador de anfitrião ASDK](.\asdk\asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>Instalar o ASDK no computador anfitrião
Depois de preparar o computador de anfitrião do kit de desenvolvimento, a ASDK pode ser implementado na imagem do CloudBuilder.vhdx. O ASDK pode ser implementado com uma interface gráfica (GUI) fornecida ao descarregar e executar o script de PowerShell asdk installer.ps1 ou completamente a partir de [a linha de comandos](.\asdk\asdk-deploy-powershell.md). 

> [!NOTE]
> Opcionalmente, após o computador anfitrião ter arrancado para o CloudBuilder.vhdx, pode configurar [definições de telemetria de pilha do Azure](.\asdk\asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *antes* o ASDK a instalar.


**[Instalar o Kit de desenvolvimento de pilha do Azure (ASDK)](.\asdk\asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Efetuar configurações de pós-implementação
Depois de instalar o ASDK, existem algumas verificações de pós-instalação recomendadas e alterações de configuração que devem ser efetuadas. Pode validar a instalação foi instalado com êxito utilizando o cmdlet de teste AzureStack e instalar as ferramentas do PowerShell de pilha do Azure e do GitHub. 

Depois de implementações que utilizam o Azure AD, tem de ativar ambas as os pilha do Azure administrador portais e de inquilino. Esta ativação permitir para dar o portal de pilha do Azure e o Azure Resource Manager, as permissões corretas (listadas na página de consentimento) para todos os utilizadores do diretório.

Deve também repor a política de expiração de palavra-passe para se certificar de que a palavra-passe para o anfitrião do kit de desenvolvimento não expira antes de terminar o período de avaliação.

> [!NOTE]
> Opcionalmente, também pode configurar [definições de telemetria de pilha do Azure](.\asdk\asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *depois* o ASDK a instalar.

**[Tarefas de implementação POST ASDK](.\asdk\asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Registar com o Azure
Tem de registar pilha do Azure com o Azure, para que possa [transferir itens do Azure marketplace](.\asdk\asdk-marketplace-item.md) à pilha do Azure.

**[Registar a pilha do Azure com o Azure](.\asdk\asdk-register.md)**

## <a name="next-steps"></a>Passos Seguintes
Parabéns! Depois de concluir estes passos, terá de um ambiente do kit de desenvolvimento com o [administrador](https://adminportal.local.azurestack.external) e [utilizador](https://portal.local.azurestack.external) portais. 

---
title: Resolver problemas de erros de cliente de Docker no Windows com o Visual Studio | Documentos da Microsoft
description: Resolução de problemas que encontrar ao utilizar o Visual Studio para criar e implementar aplicações web no Docker no Windows com o Visual Studio 2017.
services: azure-container-service
documentationcenter: na
author: devinb
manager: douge
editor: ''
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 10/13/2017
ms.author: devinb
ms.openlocfilehash: cd88dec2ad79ad9f4b4c004866060be86b777cd9
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/13/2018
ms.locfileid: "49311189"
---
# <a name="troubleshoot-visual-studio-2017-development-with-docker"></a>Resolver problemas de desenvolvimento do Visual Studio 2017 com o Docker

Quando está trabalhando com o Visual Studio Tools for Docker, poderá encontrar problemas ao criar ou a depurar seu aplicativo. Abaixo está alguns comum passos resolução de problemas.

## <a name="volume-sharing-is-not-enabled-enable-volume-sharing-in-the-docker-ce-for-windows-settings--linux-containers-only"></a>Partilha de volume não está ativada. Ativar o volume, partilha nas definições do Docker CE para Windows (apenas para contentores do Linux)

Para resolver este problema:

1. Com o botão direito **Docker para Windows** na área de notificação e, em seguida, selecione **definições**.
1. Selecione **partilhados unidades** e partilhar a unidade do sistema, juntamente com a unidade onde reside o projeto.

> [!NOTE]
> Se os ficheiros são apresentados partilhados, ainda poderá ter de clicar na ligação "Repor as credenciais..." na parte inferior da caixa de diálogo para voltar a ativar a partilha de volume. Para continuar depois de repor as credenciais, poderá ter de reiniciar o Visual Studio.

![unidades partilhadas](./media/vs-azure-tools-docker-troubleshooting-docker-errors/shareddrives.png)

## <a name="mounts-denied"></a>Monta negada

Ao utilizar o Docker para macOS, poderá encontrar um erro fazendo referência a /usr/local/share/dotnet/sdk/NuGetFallbackFolder de pasta. Adicione a pasta para o separador de partilha de ficheiros de mensagens em fila no Docker.

## <a name="unable-to-start-debugging"></a>Não é possível iniciar a depuração

Uma razão pode estar relacionado com a ter de componentes de depuração obsoletos na sua pasta de perfil do usuário. Execute os seguintes comandos para remover essas pastas para que os componentes de depuração mais recente são transferidos na próxima sessão de depuração.

- del %userprofile%\vsdbg
- del %userprofile%\onecoremsvsmon

## <a name="errors-specific-to-networking-when-debugging-your-application"></a>Erros específicos de sistema de rede ao depurar a sua aplicação

Tente executar o script que pode ser baixado da [rede de anfitrião de contentor de limpeza](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/master/windows-server-container-tools/CleanupContainerHostNetworking), que irá atualizar os componentes relacionados com a rede em sua máquina host.


## <a name="microsoftdockertools-github-repo"></a>Repositório do GitHub do Microsoft/DockerTools

Para quaisquer outros problemas que encontrar, consulte [Microsoft/DockerTools](https://github.com/microsoft/dockertools/issues) problemas.
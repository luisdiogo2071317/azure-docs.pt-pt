---
title: Configurar um anfitrião do Docker com o VirtualBox | Documentos da Microsoft
description: Instruções passo a passo para configurar uma instância de Docker predefinida com máquina do Docker e do VirtualBox
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: ''
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: d7549ac9888e9214280e5311aa5ef2123d97fd47
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969238"
---
# <a name="configure-a-docker-host-with-virtualbox"></a>Configurar um anfitrião do Docker com o VirtualBox
## <a name="overview"></a>Descrição geral
Este artigo orienta-o ao longo da configuração de uma instância de Docker predefinida com máquina do Docker e do VirtualBox. Se estiver a utilizar o [Docker para Windows](https://www.docker.com/products/docker-desktop), esta configuração não é necessária.

## <a name="prerequisites"></a>Pré-requisitos
As seguintes ferramentas precisam ser instalados.

* [Caixa de ferramentas do docker](https://github.com/docker/toolbox/releases)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Configurando o cliente de Docker com o Windows PowerShell
Para configurar um cliente de Docker, basta abrir o Windows PowerShell e execute os seguintes passos:

1. Crie uma instância de anfitrião do docker predefinida.
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. Certifique-se de que a instância predefinida está configurado e em execução. (Neste caso, deverá ver uma instância com o nome 'default' em execução.
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![saída de uma máquina docker ls][0]
3. Definir padrão que o anfitrião atual e configure o seu shell.
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. Apresente os contentores de Docker Active Directory. A lista deve estar vazia.
   
    ```PowerShell
    docker ps
    ```
   
    ![saída do docker ps][1]

> [!NOTE]
> Sempre que reiniciar o computador de desenvolvimento, terá de reiniciar o anfitrião do local docker.
> Para fazê-lo, emita o seguinte comando no prompt de comando: `docker-machine start default`.
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png

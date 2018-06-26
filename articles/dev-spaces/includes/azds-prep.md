---
title: incluir ficheiro
description: incluir ficheiro
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: d44f33b0e9f71c1d8d6e2c9878b08f9fa0e1f8a1
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938177"
---
## <a name="preparing-code-for-docker-and-kubernetes-development"></a>Preparar o código para o desenvolvimento de Docker e Kubernetes
Até ao momento, terá uma aplicação web básica que pode ser executados localmente. Agora irá containerize através da criação de ativos que definem contentor da aplicação e a forma como irá implementar para Kubernetes. Esta tarefa é fácil fazer com os espaços de programador do Azure: 

1. Iniciar o VS Code e abra o `webfrontend` pasta. (Pode ignorar quaisquer avisos de predefinido para adicionar recursos de depuração ou restaurar o projeto).
1. Abra o Terminal integrada no VS Code (utilizando o **vista > Terminal integrada** menu).
1. Execute este comando (ser Certifique-se que **webfrontend** é a pasta atual):

    ```cmd
    azds prep --public
    ```

A CLI do Azure `azds prep` comandos gera Docker e Kubernetes ativos com as predefinições:
* `./Dockerfile` Descreve a aplicação a imagem de contentor, e como o código de origem é criado e é executado dentro do contentor.
* A [gráfico Helm](https://docs.helm.sh) em `./charts/webfrontend` descreve como implementar o contentor Kubernetes.

Por agora, não é necessário compreender o conteúdo completo destes ficheiros. É importante, mas a apontar, no entanto, que **o mesmo Kubernetes e Docker ativos de configuração como código podem ser utilizados de desenvolvimento através para produção, assim que fornece uma melhor consistência entre ambientes diferentes.**
 
Um ficheiro denominado `./azds.yaml` também é gerado pelo `prep` comando, que é o ficheiro de configuração para os espaços de programador do Azure. -Complementa os artefactos Docker e Kubernetes sem configuração adicional que permite uma experiência de desenvolvimento iterativo no Azure.

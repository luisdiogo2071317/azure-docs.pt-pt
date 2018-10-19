---
title: Criar o Cloud Foundry no Azure
description: Saiba como configurar os parâmetros necessários para aprovisionar um cluster de PCF do Cloud Foundry no Azure
services: Cloud Foundry
documentationcenter: CloudFoundry
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/13/2018
ms.devlang: ''
ms.service: Cloud Foundry
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: a0a3379a8a2579080d9b686917395feec9cf8f3d
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48250622"
---
# <a name="create-cloud-foundry-on-azure"></a>Criar o Cloud Foundry no Azure

Este tutorial disponibiliza passos rápidos para criar e gerar os parâmetros necessários para aprovisionar um cluster de PCF do Pivotal Cloud Foundry no Azure.  A solução Pivotal Cloud Foundry pode ser encontrada através de uma pesquisa no Azure [MarketPlace](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).

![Texto alternativo da imagem](media/deploy/pcf-marketplace.png "Pesquisar Pivotal Cloud Foundry no Azure")


## <a name="generate-an-ssh-public-key"></a>Gerar uma chave pública SSH

Existem várias formas de gerar uma chave SSH pública com o Windows, Mac ou Linux.

```Bash
ssh-keygen -t rsa -b 2048
```
- Clique aqui para ver [instruções]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) para o seu ambiente.

## <a name="create-a-service-principal"></a>Criar um Principal de Serviço

> [!NOTE]
>
> Criar um principal de serviço requer uma permissão de conta do proprietário.  Além disso, pode escrever um script para automatizar a criação do Principal de Serviço. Por exemplo, com a CLI do Azure [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest).

1. Inicie sessão na sua conta do Azure.

    `az login`

    ![Texto alternativo da imagem](media/deploy/az-login-output.png "Início de sessão da CLI do Azure")
 
    Copie o valor do "id" como o seu **ID de subscrição** e o valor **tenantId** para ser utilizado mais tarde.

2. Defina a sua subscrição predefinida para esta configuração.

    `az account set -s {id}`

3. Crie uma aplicação do AAD para o PCF e especifique uma palavra-passe alfanumérica exclusiva.  Armazene a palavra-passe como o seu **clientSecret** para ser utilizado mais tarde.

    `az ad app create --display-name "Svc Prinicipal for OpsManager" --password {enter-your-password} --homepage "{enter-your-homepage}" --identifier-uris {enter-your-homepage}`

    O valor de "appId" de cópia na saída como o seu **ClientID** para ser utilizado mais tarde.

    > [!NOTE]
    >
    > Escolha sua própria home page da aplicação e o URI identificador.  Por exemplo, http://www.contoso.com.

4. Crie um principal de serviço com o seu novo “appId”.

    `az ad sp create --id {appId}`

5. Defina a função de permissão do seu principal de serviço como um **Contribuinte**.

    `az role assignment create --assignee “{enter-your-homepage}” --role “Contributor” `

    Também pode utilizar…

    `az role assignment create --assignee {service-princ-name} --role “Contributor” `

    ![Texto alternativo da imagem](media/deploy/svc-princ.png "Atribuição de função Principal de Serviço")

6. Certifique-se de que consegue iniciar sessão com êxito no seu Principal de Serviço com o appId, a palavra-passe e o tenantId.

    `az login --service-principal -u {appId} -p {your-passward}  --tenant {tenantId}`

7. Crie um ficheiro . json no formato seguinte com todos os valores anteriores **ID de subscrição**, **tenantId**, **clientID** e o **clientSecret** que copiou acima.  Guarde o ficheiro.

    ```json
    {
        "subscriptionID": "{enter-your-subscription-Id-here}",
        "tenantID": "{enter-your-tenant-Id-here}",
        "clientID": "{enter-your-app-Id-here}",
        "clientSecret": "{enter-your-key-here}"
    }
    ```

## <a name="get-the-pivotal-network-token"></a>Obtenha o Token de Rede da Pivotal

1. Registe-se ou inicie sessão na sua conta da [Rede Pivotal](https://network.pivotal.io).
2. Clique no nome do perfil no canto superior direito da página e selecione **Editar Perfil”.
3. Desloque-se para a parte inferior da página e copie o valor **TOKEN de API de LEGADO**.  Este é o seu valor de **Token da Rede Pivotal** que irá ser utilizado mais tarde.

## <a name="provision-your-cloud-foundry-on-azure"></a>Aprovisionar o Cloud Foundry no Azure

1. Agora possui todos os parâmetros necessários para aprovisionar o seu cluster de [Pivotal Cloud Foundry no Azure](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).
2. Introduza os parâmetros e crie o cluster de PCF.

## <a name="verify-the-deployment-and-log-into-the-pivotal-ops-manager"></a>Verifique a implementação e registe-se no Pivotal Ops Manager

1. O seu cluster de PCF deve mostrar um estado de implementação.

    ![Texto alternativo da imagem](media/deploy/deployment.png "Estado de implementação do Azure")

2. Clique na ligação **Implementações** no painel de navegação esquerdo para obter as credenciais para o seu PCF Ops Manager e, em seguida, clique no **Nome da Implementação** na página seguinte.
3. No painel de navegação esquerdo, clique na ligação **Saídas** para apresentar o URL, o nome de utilizador e a palavra-passe do PCF Ops Manager.  O valor de "OPSMAN-FQDN" é o URL.
 
    ![Texto alternativo da imagem](media/deploy/deploy-outputs.png "Saída de implementação do Cloud Foundry")
 
4. Inicie o URL num browser e introduza as credenciais do passo anterior para iniciar sessão.

    ![Texto alternativo da imagem](media/deploy/pivotal-login.png "Página de Início de Sessão Pivotal")
         
    > [!NOTE]
    >
    > Se o browser Internet Explorer falhar devido a uma mensagem de aviso de site não seguro, clique em "Obter mais informações" e "Avançar para a página Web.  No Firefox, clique em Avançar e adicione a certificação para continuar.

5. O PCF Ops Manager deve apresentar as instâncias do Azure implementadas. Agora pode começar a implantar e a gerir as suas aplicações aqui!
               
    ![Texto alternativo da imagem](media/deploy/ops-mgr.png "Instância do Azure implementada na Pivotal")
 

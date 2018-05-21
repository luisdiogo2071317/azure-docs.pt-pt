---
title: Atualizar o App Service do Azure na pilha do Azure | Microsoft Docs
description: Orientação detalhada para atualizar o serviço de aplicações do Azure na pilha do Azure
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: anwestg
ms.openlocfilehash: c822f25e25953b8709f481e51d6a63e6a912a60a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="update-azure-app-service-on-azure-stack"></a>Atualizar o App Service do Azure na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

> [!IMPORTANT]
> Aplicar a atualização 1804 ao seu sistema de pilha do Azure integrado ou implementar o kit de desenvolvimento de pilha do Azure mais recente antes de implementar 1.2 de serviço de aplicações do Azure.
>
>

Ao seguir as instruções neste artigo, pode atualizar o [fornecedor de recursos do serviço de aplicações](azure-stack-app-service-overview.md) implementado num ambiente do Azure pilha que está ligado à Internet.

> [!IMPORTANT]
> Antes de executar a atualização, certifique-se de que já concluiu a [implementação do serviço de aplicações do Azure no fornecedor de recursos de pilha do Azure](azure-stack-app-service-deploy.md)
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>Execute o instalador de fornecedor de recursos do serviço de aplicações

Durante este processo, a atualização irá:

* Detetar a implementação anterior do App Service
* Preparar todos os pacotes de atualização e novas versões de todas as bibliotecas de OSS para ser implementada
* Carregar para o armazenamento
* Atualizar todas as funções do serviço de aplicações (controladores, gestão, front-end, publicador e trabalho funções)
* Atualizar definições do conjutno de dimensionamento do Serviço de Aplicações
* Atualize o manifesto de fornecedor de recursos do serviço de aplicações

> [!IMPORTANT]
> O instalador do serviço de aplicações tem de ser executado numa máquina que pode alcançar o ponto final do Azure pilha administrador do Azure Resource Manager.
>
>

Para atualizar a implementação do serviço de aplicações na pilha do Azure, siga estes passos:

1. Transferir o [instalador do serviço de aplicações](https://aka.ms/appsvcupdate2installer)

2. Executar appservice.exe como um administrador

    ![Instalador do serviço de aplicações][1]

3. Clique em **implementar o serviço de aplicações ou Atualize para a versão mais recente.**

4. Reveja e aceite os termos de licenciamento de Software Microsoft e, em seguida, clique em **seguinte**.

5. Reveja e aceite os termos de licença de terceiros e, em seguida, clique em **seguinte**.

6. Certifique-se de que o ponto final do Azure pilha do Azure Resource Manager e o inquilino do Active Directory informações estão corretas. Se utilizou as predefinições durante a implementação do Kit de desenvolvimento de pilha do Azure, pode aceitar os valores predefinidos aqui. No entanto, se as opções personalizado quando implementou a pilha do Azure, tem de editar os valores nesta janela para refletir que. Por exemplo, se utilizar o sufixo de domínio *mycloud.com*, tem de alterar o ponto final do Azure de pilha do Azure Resource Manager para *management.region.mycloud.com*. Depois de confirmar as suas informações, clique em **seguinte**.

    ![Informações da nuvem de pilha do Azure][2]

7. Na página seguinte:

   1. Clique em de **Connect** junto ao **subscrições de pilha do Azure** caixa.
        * Se estiver a utilizar o Azure Active Directory (Azure AD), introduza a conta de administrador do Azure AD e a palavra-passe que forneceu quando implementou a pilha do Azure. Clique em **sessão**.
        * Se estiver a utilizar serviços de Federação do Active Directory (AD FS), forneça a sua conta de administrador. Por exemplo, *cloudadmin@azurestack.local*. Introduza a palavra-passe e clique em **sessão**.
   2. No **subscrições de pilha do Azure** caixa, selecione o **subscrição do fornecedor predefinido**.
   3. No **localizações de pilha do Azure** caixa, selecione a localização que corresponde à região estiver a implementar. Por exemplo, seleccione **local** se a implementar o Kit de desenvolvimento de pilha do Azure.
   4. Se for detetada uma implementação existente do serviço de aplicações, em seguida, a conta de armazenamento e de grupo de recursos será preenchida e a cinzento.
   5. Clique em **seguinte** para rever o resumo da atualização.

    ![Foi detetada uma instalação do serviço de aplicações][3]

8. Na página de resumo:
   1. Certifique-se seleções que fez. Para efetuar alterações, utilize o **anterior** botões para visitar às páginas anteriores.
   2. Se as configurações estão corretas, selecione a caixa de verificação.
   3. Para iniciar a atualização, clique em **seguinte**.

       ![Resumo de atualização do serviço de aplicações][4]

9. Página do progresso da atualização:
    1. Controle o progresso da atualização. A duração da atualização do serviço de aplicações na pilha de Azure varia depende do número de instâncias de função implementadas.
    2. Após a atualização for concluída com êxito, clique em **saída**.

        ![Progresso da atualização do serviço de aplicações][5]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update/app-service-exe.png
[2]: ./media/azure-stack-app-service-update/app-service-azure-resource-manager-endpoints.png
[3]: ./media/azure-stack-app-service-update/app-service-installation-detected.png
[4]: ./media/azure-stack-app-service-update/app-service-upgrade-summary.png
[5]: ./media/azure-stack-app-service-update/app-service-upgrade-complete.png

## <a name="next-steps"></a>Passos Seguintes

Também pode experimentar o outro [plataforma como dos serviços de serviço (PaaS)](azure-stack-tools-paas-services.md).

* [Fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-deploy.md)
* [Fornecedor de recursos de MySQL](azure-stack-mysql-resource-provider-deploy.md)

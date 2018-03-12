---
title: Atualizar o App Service do Azure Offline | Microsoft Docs
description: "Orientação detalhada para atualizar o serviço de aplicações do Azure na pilha de Azure offline"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2018
ms.author: anwestg
ms.openlocfilehash: 61a3169229cc121c078a934f6b979bdaffafd565
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="offline-update-of-azure-app-service-on-azure-stack"></a>Atualização offline do serviço de aplicações do Azure na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

> [!IMPORTANT]
> Aplicar a atualização 1802 ao seu sistema de pilha do Azure integrado ou implementar o kit de desenvolvimento de pilha do Azure mais recente antes de implementar o serviço de aplicações do Azure.
>
>

Ao seguir as instruções neste artigo, pode atualizar o [fornecedor de recursos do serviço de aplicações](azure-stack-app-service-overview.md) implementado num ambiente do Azure pilha que é:

* não ligado à Internet
* protegidos por serviços de Federação do Active Directory (AD FS).

> [!IMPORTANT]
> Antes de executar a atualização, certifique-se de que já concluiu a [implementação do serviço de aplicações do Azure no fornecedor de recursos de pilha do Azure](azure-stack-app-service-deploy-offline.md)
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>Execute o instalador de fornecedor de recursos do serviço de aplicações

Para atualizar o fornecedor de recursos do serviço de aplicações num ambiente de pilha do Azure, tem de concluir estas tarefas:

1. Transferir o [instalador do serviço de aplicações](https://aka.ms/appsvcupdate1installer)
2. Crie um pacote de atualização offline.
3. Execute o instalador do serviço de aplicações (appservice.exe) e concluir a atualização.

Durante este processo, a atualização irá:

* Detetar a implementação anterior do App Service
* Carregar para o armazenamento
* Atualizar todas as funções do serviço de aplicações (controladores, gestão, front-end, publicador e trabalho funções)
* Atualizar as definições do conjunto de dimensionamento do serviço de aplicações
* Atualize o manifesto de fornecedor de recursos do serviço de aplicações

## <a name="create-an-offline-upgrade-package"></a>Criar um pacote de atualização offline

Para atualizar o serviço de aplicações num ambiente desligado, primeiro tem de criar um pacote de atualização offline num computador que está ligado à Internet.

1. Executar appservice.exe como um administrador

    ![Instalador do serviço de aplicações][1]

2. Clique em **avançadas** > **pacote offline criar**

    ![Instalador do serviço de aplicações avançada][2]

3. O instalador do serviço de aplicações, cria um pacote de atualização offline e apresenta o caminho para o mesmo.  Pode clicar em **Abrir pasta** para abrir a pasta no Explorador de ficheiros.

4. Copie o instalador (AppService.exe) e o pacote de atualização offline para a máquina de anfitrião de pilha do Azure.

## <a name="complete-the-upgrade-of-app-service-on-azure-stack"></a>Concluir a atualização do serviço de aplicações na pilha do Azure

> [!IMPORTANT]
> O instalador do serviço de aplicações tem de ser executado numa máquina que pode aceder a pilha de administrador do Azure Resource Manager ponto final do Azure.
>
>

1. Execute appservice.exe como administrador.  

    ![Instalador do serviço de aplicações][1]

2. Clique em **avançadas** > **concluir offline instalação ou atualização**.

    ![Instalador do serviço de aplicações avançada][2]

3. Navegue para a localização do pacote de atualização offline que criou anteriormente e, em seguida, clique em **seguinte**.

4. Reveja e aceite os termos de licenciamento de Software Microsoft e, em seguida, clique em **seguinte**.

5. Reveja e aceite os termos de licença de terceiros e, em seguida, clique em **seguinte**.

6. Certifique-se de que o ponto final do Azure pilha do Azure Resource Manager e o inquilino do Active Directory informações estão corretas. Se utilizou as predefinições durante a implementação do Kit de desenvolvimento de pilha do Azure, pode aceitar os valores predefinidos aqui. No entanto, se as opções personalizado quando implementou a pilha do Azure, tem de editar os valores nesta janela para refletir que. Por exemplo, se utilizar o sufixo de domínio *mycloud.com*, tem de alterar o ponto final do Azure de pilha do Azure Resource Manager para *management.region.mycloud.com*. Depois de confirmar as suas informações, clique em **seguinte**.

    ![Informações da nuvem de pilha do Azure][3]

7. Na página seguinte:

   1. Clique em de **Connect** junto ao **subscrições de pilha do Azure** caixa.
        * Se estiver a utilizar o Azure Active Directory (Azure AD), introduza a conta de administrador do Azure AD e a palavra-passe que forneceu quando implementou a pilha do Azure. Clique em **sessão**.
        * Se estiver a utilizar serviços de Federação do Active Directory (AD FS), forneça a sua conta de administrador. Por exemplo,  *cloudadmin@azurestack.local* . Introduza a palavra-passe e clique em **sessão**.
   2. No **subscrições de pilha do Azure** caixa, selecione a sua subscrição.
   3. No **localizações de pilha do Azure** caixa, selecione a localização que corresponde à região estiver a implementar. Por exemplo, seleccione **local** se a implementar o Kit de desenvolvimento de pilha do Azure.
   4. Se for detetada uma implementação existente do serviço de aplicações, em seguida, a conta de armazenamento e de grupo de recursos será preenchida e a cinzento.
   5. Clique em **seguinte** para rever o resumo da atualização.

    ![Foi detetada uma instalação do serviço de aplicações][4]

8. Na página de resumo:
   1. Certifique-se seleções que fez. Para efetuar alterações, utilize o **anterior** botões para visitar às páginas anteriores.
   2. Se as configurações estão corretas, selecione a caixa de verificação.
   3. Para iniciar a atualização, clique em **seguinte**.

       ![Resumo de atualização do serviço de aplicações][5]

9. Página do progresso da atualização:
    1. Controle o progresso da atualização. A duração da atualização do serviço de aplicações na pilha de Azure varia depende do número de instâncias de função implementadas.
    2. Após a atualização for concluída com êxito, clique em **saída**.

        ![Progresso da atualização do serviço de aplicações][6]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update-offline/app-service-exe.png
[2]: ./media/azure-stack-app-service-update-offline/app-service-exe-advanced.png
[3]: ./media/azure-stack-app-service-update-offline/app-service-azure-resource-manager-endpoints.png
[4]: ./media/azure-stack-app-service-update-offline/app-service-installation-detected.png
[5]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-summary.png
[6]: ./media/azure-stack-app-service-update-offline/app-service-upgrade-complete.png

## <a name="next-steps"></a>Passos Seguintes

Também pode experimentar o outro [plataforma como dos serviços de serviço (PaaS)](azure-stack-tools-paas-services.md).

* [Fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-deploy.md)
* [Fornecedor de recursos de MySQL](azure-stack-mysql-resource-provider-deploy.md)

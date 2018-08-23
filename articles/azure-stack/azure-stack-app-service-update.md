---
title: Atualizar o serviço de aplicações do Azure no Azure Stack | Documentos da Microsoft
description: Documentação de orientação detalhada para a atualização de serviço de aplicações do Azure no Azure Stack
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
ms.date: 08/15/2018
ms.author: anwestg
ms.openlocfilehash: f12eb7d74d9c47567c72cddda8d2813f394fb1de
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060250"
---
# <a name="update-azure-app-service-on-azure-stack"></a>Atualizar o serviço de aplicações do Azure no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

> [!IMPORTANT]
> Aplicar a atualização de 1807 seu sistema integrado do Azure Stack ou implementar o development kit do Azure Stack mais recentes antes de implementar 1.3 de serviço de aplicações do Azure.
>
>

Ao seguir as instruções neste artigo, pode atualizar o [fornecedor de recursos do serviço de aplicações](azure-stack-app-service-overview.md) implementado num ambiente do Azure Stack que está ligado à Internet.

> [!IMPORTANT]
> Antes de executar a atualização, certifique-se de que já tenha concluído a [implementação do serviço de aplicações do Azure no fornecedor de recursos do Azure Stack](azure-stack-app-service-deploy.md)
>
>

## <a name="run-the-app-service-resource-provider-installer"></a>Execute o instalador de fornecedor de recursos do serviço de aplicações

Durante este processo, a atualização irá:

* Detetar a implementação anterior do serviço de aplicações
* Preparar todos os pacotes de atualização e novas versões de todas as bibliotecas de sistemas operacionais a serem implantados
* Carregar para o armazenamento
* Atualizar todas as funções do serviço de aplicações (controladores, o gerenciamento, front-end, o publicador e o trabalho funções)
* Atualizar definições do conjutno de dimensionamento do Serviço de Aplicações
* Atualizar o manifesto de fornecedor de recursos do serviço de aplicações

> [!IMPORTANT]
> O instalador do serviço de aplicações tem de ser executado numa máquina que pode alcançar o ponto de final do Gestor de recursos do Azure de administrador do Azure Stack.
>
>

Para atualizar a sua implementação do serviço de aplicações no Azure Stack, siga estes passos:

1. Transferir o [instalador do serviço de aplicações](https://aka.ms/appsvcupdate3installer)

2. Executar appservice.exe como administrador

    ![Instalador do serviço de aplicações][1]

3. Clique em **implementar o serviço de aplicações ou Atualize para a versão mais recente.**

4. Reveja e aceite os termos de licença de Software do Microsoft e, em seguida, clique em **seguinte**.

5. Reveja e aceite os termos de licença de terceiros e, em seguida, clique em **seguinte**.

6. Certifique-se de que o ponto final do Azure Stack Azure Resource Manager e o inquilino do Active Directory informações estão corretas. Se usou as configurações padrão durante a implementação do Development Kit do Azure Stack, pode aceitar os valores predefinidos aqui. No entanto, se as opções personalizado quando implementou o Azure Stack, tem de editar os valores nesta janela para refletir a alteração. Por exemplo, se usar o sufixo de domínio *mycloud.com*, tem de alterar para o ponto de final do Azure Stack Azure Resource Manager *management.region.mycloud.com*. Depois de confirmar suas informações, clique em **seguinte**.

    ![Informações de nuvem do Azure Stack][2]

7. Na página seguinte:

   1. Clique nas **Connect** junto aos **subscrições do Azure Stack** caixa.
        * Se estiver a utilizar o Azure Active Directory (Azure AD), introduza a conta de administrador do Azure AD e a palavra-passe que forneceu quando implementou o Azure Stack. Clique em **iniciar sessão**.
        * Se estiver a utilizar os serviços de Federação do Active Directory (AD FS), forneça a sua conta de administrador. Por exemplo, *cloudadmin@azurestack.local*. Introduza a palavra-passe e clique em **sessão**.
   2. Na **subscrições do Azure Stack** caixa, selecione a **subscrição do fornecedor predefinido**.
   3. Na **localizações do Azure Stack** caixa, selecione a localização que corresponde à região que está a implementar. Por exemplo, seleccione **local** se sua a implementar o Development Kit do Azure Stack.
   4. Se uma implementação de serviço de aplicações existente é detetada, em seguida, a conta de armazenamento e de grupo de recursos será preenchida e a cinzento.
   5. Clique em **seguinte** para rever o resumo da atualização.

    ![Instalação do serviço de aplicações detetada][3]

8. Na página de resumo:
   1. Certifique-se seleções que fez. Para fazer alterações, utilize o **Previous** botões para visitar as páginas anteriores.
   2. Se as configurações estão corretas, selecione a caixa de verificação.
   3. Para iniciar a atualização, clique em **seguinte**.

       ![Resumo de atualização do serviço de aplicações][4]

9. Página de progresso da atualização:
    1. Controle o progresso da atualização. A duração da atualização do serviço de aplicações no Azure Stack varia depende do número de instâncias de função implementadas.
    2. Após a atualização for concluída com êxito, clique em **saída**.

        ![Progresso da atualização de serviço de aplicações][5]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update/app-service-exe.png
[2]: ./media/azure-stack-app-service-update/app-service-azure-resource-manager-endpoints.png
[3]: ./media/azure-stack-app-service-update/app-service-installation-detected.png
[4]: ./media/azure-stack-app-service-update/app-service-upgrade-summary.png
[5]: ./media/azure-stack-app-service-update/app-service-upgrade-complete.png

## <a name="next-steps"></a>Passos Seguintes

Também pode tentar outros [plataforma como um serviço de serviço (PaaS)](azure-stack-tools-paas-services.md).

* [Fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-deploy.md)
* [Fornecedor de recursos MySQL](azure-stack-mysql-resource-provider-deploy.md)

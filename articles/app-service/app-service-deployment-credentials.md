---
title: Credenciais de implementação do serviço de aplicações do Azure | Documentos da Microsoft
description: Saiba como utilizar as credenciais de implementação do serviço de aplicações do Azure.
services: app-service
documentationcenter: ''
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/05/2016
ms.author: dariagrigoriu
ms.openlocfilehash: a17260770f0b2e0a73585ce4108bd5625ac22229
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436153"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Configurar as credenciais de implementação para o serviço de aplicações do Azure
[Serviço de aplicações do Azure](http://go.microsoft.com/fwlink/?LinkId=529714) suporta dois tipos de credenciais para [implementação de Git local](app-service-deploy-local-git.md) e [implementação de FTP/S](app-service-deploy-ftp.md). Essas não são o mesmo que as suas credenciais do Azure Active Directory.

* **Credenciais de nível de usuário**: um conjunto de credenciais para toda a conta do Azure. Ele pode ser utilizado para implementar no serviço de aplicações para qualquer aplicação, em qualquer subscrição, o que a conta do Azure tem permissão para aceder. Estes são o conjunto de credenciais predefinida que configurou **dos serviços de aplicações** > **&lt;app_name >** > **ascredenciaisdeimplementação**. Esta é a predefinição conjunto que aparece no portal do GUI (como o **descrição geral** e **propriedades** da sua aplicação [página de recursos](../azure-resource-manager/resource-group-portal.md#manage-resources)).

    > [!NOTE]
    > Quando delegar o acesso aos recursos do Azure através do controlo de acesso baseado em ' (RBAC) da função ou permissões de coadministrador, cada utilizador do Azure que recebe acesso a uma aplicação pode utilizar seu credenciais de nível de usuário pessoais até que o acesso é revogado. Estas credenciais de implementação não devem ser compartilhados com outros utilizadores do Azure.
    >
    >

* **Credenciais ao nível da aplicação**: um conjunto de credenciais para cada aplicação. Ele pode ser usado para implementar essa aplicação apenas. As credenciais de cada aplicação é gerada automaticamente durante a criação de aplicações, e está disponível na aplicação do perfil de publicação. Não é possível configurar manualmente as credenciais, mas pode repô-los para uma aplicação em qualquer altura.

    > [!NOTE]
    > Para dar a alguém aceder a estas credenciais através de função com base em controlo de acesso (RBAC), precisa para torná-los Contribuidor ou superior na aplicação Web. Os leitores não são permitidos para publicar e, por conseguinte, não é possível aceder essas credenciais.
    >
    >

## <a name="userscope"></a>Definir e repor as credenciais de nível de usuário

Pode configurar as suas credenciais de nível de usuário em qualquer aplicação [página de recursos](../azure-resource-manager/resource-group-portal.md#manage-resources). Independentemente na aplicação que configurar estas credenciais, ele se aplica a todas as aplicações e para todas as subscrições na sua conta do Azure. 

Para configurar as suas credenciais de nível de usuário:

1. Na [portal do Azure](https://portal.azure.com), clique em serviço de aplicações >  **&lt;any_app >** > **credenciais de implementação**.

    > [!NOTE]
    > No portal, tem de ter pelo menos uma aplicação antes de poder aceder a página de credenciais de implementação. No entanto, com o [CLI do Azure](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set), pode configurar as credenciais de nível de usuário sem uma aplicação existente.

2. Configurar o nome de utilizador e palavra-passe e, em seguida, clique em **guardar**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Depois de definir as suas credenciais de implementação, é possível encontrar o *Git* nome de utilizador de implementação na sua aplicação **descrição geral**,

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

e *FTP* nome de utilizador de implementação na sua aplicação **propriedades**.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Azure não mostra a palavra-passe de implementação de nível de usuário. Se se esquecer da palavra-passe, não é possível recuperá-la. No entanto, pode repor as suas credenciais ao seguir os passos nesta secção.
>
>  

## <a name="appscope"></a>Obter e repor as credenciais de ao nível da aplicação
Para cada aplicação no serviço de aplicações, as suas credenciais ao nível da aplicação são armazenados no XML de perfil de publicação.

Para obter as credenciais ao nível da aplicação:

1. Na [portal do Azure](https://portal.azure.com), clique em serviço de aplicações >  **&lt;any_app >** > **descrição geral**.

2. Clique em **... Obter mais** > **obter perfil de publicação**, e começa a transferência para um. Ficheiro PublishSettings.

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. Abra o. PublishSettings de ficheiros e localize a `<publishProfile>` marca com o atributo `publishMethod="FTP"`. Em seguida, obter seu `userName` e `password` atributos.
Estas são as credenciais ao nível da aplicação.

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    Assim como as credenciais de nível de usuário, o nome de utilizador de implementação de FTP está no formato `<app_name>\<username>`, e o nome de utilizador de implementação de Git é simplesmente `<username>` sem anterior `<app_name>\`.

Para repor as credenciais ao nível da aplicação:

1. Na [portal do Azure](https://portal.azure.com), clique em serviço de aplicações >  **&lt;any_app >** > **descrição geral**.

2. Clique em **... Obter mais** > **repor perfil de publicação**. Clique em **Sim** para confirmar a reposição.

    A ação de reposição invalida qualquer anteriormente transferidos. Ficheiros PublishSettings.

## <a name="next-steps"></a>Passos Seguintes

Saiba como utilizar estas credenciais para implementar a aplicação a partir [local Git](app-service-deploy-local-git.md) ou a utilizar [FTP/S](app-service-deploy-ftp.md).

---
title: Aceder e autenticar sem iniciar sessão - Azure Logic Apps | Documentos da Microsoft
description: Criar uma identidade gerida para que a aplicação lógica pode autenticar e aceder a recursos em outros inquilinos do Azure Active Directory (Azure AD) sem as suas credenciais
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: fb1c31e6e7c075e20191a4e51d7b1a9323f3b979
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973971"
---
# <a name="access-resources-and-authenticate-as-managed-identities-in-azure-logic-apps"></a>Aceder aos recursos e efetuar a autenticação como identidades geridas no Azure Logic Apps

Para aceder aos recursos de outros inquilinos do Azure Active Directory (Azure AD) e autenticar a sua identidade sem iniciar sessão, pode criar uma [identidade gerida](../active-directory/managed-identities-azure-resources/overview.md) que a aplicação lógica utiliza, em vez das suas credenciais. O Azure gere esta identidade para si e ajuda a proteger as suas credenciais, porque não precisa fornecer ou girar segredos. Este artigo mostra como criar e utilizar uma identidade gerida para a aplicação lógica. Para obter mais informações, consulte [gerir identidades para recursos do Azure](../app-service/app-service-managed-service-identity.md).

> [!NOTE]
> Identidades de gerido para recursos do Azure é o nome de substituição para o serviço anteriormente conhecido como identidade de serviço gerida (MSI).

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure, ou se não tiver uma subscrição <a href="https://azure.microsoft.com/free/" target="_blank">Inscreva-se uma conta gratuita do Azure</a>.

* A aplicação lógica em que pretende utilizar a identidade gerida. Se não tiver uma aplicação lógica, consulte [criar seu primeiro fluxo de trabalho de aplicação de lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="create-identity"></a>

## <a name="create-managed-identity"></a>Criar a identidade gerida

Pode criar ou ativar uma identidade gerida para a sua aplicação lógica através do portal do Azure, modelos Azure Resource Manager ou do Azure PowerShell. 

### <a name="azure-portal"></a>Portal do Azure

Para criar uma identidade gerida para a sua aplicação lógica no portal do Azure, ative o **registar com o Azure Active Directory** definição nas definições de fluxo de trabalho da sua aplicação lógica.

1. Na [portal do Azure](https://portal.azure.com), abra a aplicação lógica no Estruturador da aplicação lógica.

1. Siga estes passos. 

   1. No menu da aplicação lógica, sob **configurações**, selecione **definições de fluxo de trabalho**. 

   1. Sob **identidade do serviço gerido** > 
    **registar com o Azure Active Directory**, escolha **no**.

   1. Quando tiver terminado, escolha **guardar** na barra de ferramentas.

      ![Ativar a definição de identidade gerida](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

      Azure agora mostra essas propriedades e valores para a identidade gerida da sua aplicação lógica:

      ![GUIDS para o ID de principal e o ID de inquilino](./media/create-managed-service-identity/principal-tenant-id.png)

      | Propriedade | Valor | Descrição | 
      |----------|-------|-------------| 
      | **ID de principal** | <*ID de principal-GUID*> | Um identificador exclusivo global (GUID) que representa a aplicação lógica no inquilino do Azure AD | 
      | **ID do inquilino** | <*Inquilino do Azure-AD-– ID do GUID*> | Um identificador exclusivo global (GUID) que representa o inquilino do Azure AD em que a sua aplicação lógica agora é um membro. Dentro de inquilino do Azure AD, o principal de serviço tem o mesmo nome que a instância da aplicação lógica. | 
      ||| 

### <a name="deployment-template"></a>Modelo de implementação

Para automatizar a criação e a implementar recursos do Azure como o logic apps, pode configurar modelos Azure Resource Manager. Para obter mais informações, consulte [criar e implementar aplicações lógicas com modelos Azure Resource Manager](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md). 

Para criar uma identidade gerida para a sua aplicação lógica com o modelo, adicione a **identidade** elemento e **tipo** propriedade para a definição de fluxo de trabalho de aplicação lógica no seu modelo de implementação. Estas definições indicam que o Azure cria e gere esta identidade para a aplicação lógica:

```json
"identity": {
    "type": "SystemAssigned"
}
```

Por exemplo, a sua aplicação lógica pode ser como esta versão:

```json
{
   "apiVersion": "2016-06-01", 
   "type": "Microsoft.logic/workflows", 
   "name": "[variables('logicappName')]", 
   "location": "[resourceGroup().location]", 
   "identity": { 
      "type": "SystemAssigned" 
   }, 
   "properties": { 
      "definition": { 
         "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#", 
          "actions": {}, 
          "parameters": {}, 
          "triggers": {}, 
          "contentVersion": "1.0.0.0", 
          "outputs": {} 
     }, 
     "parameters": {}, 
     "dependsOn": [] 
}
```

Quando o Azure cria a aplicação lógica, a definição de fluxo de trabalho essa aplicação lógica inclui estas propriedades adicionais:

```json
"identity": {
    "type": "SystemAssigned",
    "principalId": "<principal-ID-GUID>",
    "tenantId": "<Azure-AD-tenant-ID>-GUID"
}
```

| Propriedade | Valor | Descrição | 
|----------|-------|-------------|
| **principalId** | <*ID de principal-GUID*> | Um identificador exclusivo global (GUID) que representa a aplicação lógica no inquilino do Azure AD | 
| **tenantId** | <*Inquilino do Azure-AD-– ID do GUID*> | Um identificador exclusivo global (GUID) que representa o inquilino do Azure AD em que a aplicação lógica agora é um membro. Dentro de inquilino do Azure AD, o principal de serviço tem o mesmo nome que a instância da aplicação lógica. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Aceder a recursos com a identidade gerida

Depois de criar uma identidade gerida para a aplicação lógica, poderá [conceder esse acesso de identidade para outros recursos](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). Em seguida, pode usar essa identidade gerida para a autenticação, como qualquer outro [principal de serviço](../active-directory/develop/app-objects-and-service-principals.md). 

Por exemplo, suponha que já tiver configurado a uma aplicação lógica com uma identidade gerida que tenha acesso a outro recurso. Agora pode adicionar uma ação de HTTP para que a sua aplicação lógica pode enviar uma solicitação HTTP ou ligue a esse recurso. 

1. Na sua aplicação lógica, adicione a **HTTP** ação. 

1. Forneça os detalhes necessários para esta ação, por exemplo, a solicitação **método** e **URI** localização do recurso que pretende chamar.

1. Na ação de HTTP, escolha **Mostrar opções avançadas**. 

1. Do **autenticação** lista, selecione **identidade do serviço gerido**, que mostra, em seguida, o **público-alvo** propriedade para definir:

   ![Selecione "Identidade de serviço gerida"](./media/create-managed-service-identity/select-managed-service-identity.png)

1. Continue a criar a aplicação lógica à sua maneira.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Remover a identidade gerida

Para desativar uma identidade gerida na sua aplicação lógica, pode seguir os passos semelhantes à forma como criou a identidade através do portal do Azure, modelos de implementação Azure Resource Manager ou do Azure PowerShell. 

Ao eliminar a aplicação lógica, o Azure remove automaticamente a identidade de sistema atribuído da sua aplicação lógica do Azure AD.

### <a name="azure-portal"></a>Portal do Azure

1. No Estruturador da aplicação lógica, abra a aplicação lógica.

1. Siga estes passos. 

   1. No menu da aplicação lógica, sob **configurações**, selecione **definições de fluxo de trabalho**. 
   
   1. Sob **identidade do serviço gerido**, escolha **desativar** para o **registar com o Azure Active Directory** propriedade.

   1. Quando tiver terminado, escolha **guardar** na barra de ferramentas.

      ![Desativar a definição de identidade gerida](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Modelo de implementação

Se tiver criado a identidade gerida da aplicação lógica com um modelo de implementação Azure Resource Manager, defina o `"identity"` do elemento `"type"` propriedade `"None"`. Esta ação também elimina o ID de principal do Azure AD. 

```json
"identity": {
    "type": "None"
}
```

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

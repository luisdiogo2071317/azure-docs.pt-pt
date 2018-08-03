---
title: 'O Azure Databricks: As questões comuns e ajuda | Documentos da Microsoft'
description: Obtenha respostas para perguntas comuns e as informações de resolução de problemas sobre o Azure Databricks.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: c3ba235c60480c38a21ee3264c54b4a4dcdea340
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434606"
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Perguntas mais frequentes sobre o Azure Databricks

Este artigo lista as principais consultas podem ter relacionadas com ao Azure Databricks. Ele também apresenta alguns problemas comuns que poderá ter durante o uso do Databricks. Para obter mais informações, consulte [o que é o Azure Databricks](what-is-azure-databricks.md). 

## <a name="can-i-use-my-own-keys-for-local-encryption"></a>Pode utilizar minhas próprias chaves de encriptação local? 
Na versão atual, usar suas próprias chaves do Azure Key Vault não é suportada. 

## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Pode utilizar redes virtuais do Azure databricks?
Uma nova rede virtual é criada como parte do aprovisionamento do Databricks. Nesta versão, não é possível utilizar a sua própria rede virtual do Azure.

## <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Como posso aceder a partir de um bloco de notas do Azure Data Lake Store? 

Siga estes passos.
1. No Azure Active Directory (Azure AD), Aprovisione um principal de serviço e registar a respetiva chave.
1. Atribua as permissões necessárias para o principal de serviço no Data Lake Store.
1. Para aceder a um ficheiro no Data Lake Store, utilize as credenciais do principal de serviço no bloco de notas.

Para obter mais informações, consulte [Use Data Lake Store com o Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html).

## <a name="fix-common-problems"></a>Corrigir problemas comuns

Seguem-se alguns problemas que poderão surgir com o Databricks.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Problema: Esta subscrição não está registada para utilizar o espaço de nomes 'Microsoft.Databricks'

#### <a name="error-message"></a>Mensagem de erro

"Esta subscrição não está registada para utilizar o espaço de nomes 'Microsoft.Databricks'. Consulte https://aka.ms/rps-not-found para saber como registar subscrições. (Code: MissingSubscriptionRegistration) "

#### <a name="solution"></a>Solução

1. Aceda ao [Portal do Azure](https://portal.azure.com).
1. Selecione **subscrições**, a subscrição estiver a utilizar, e, em seguida **fornecedores de recursos**. 
1. Na lista de fornecedores de recursos, contra **Microsoft.Databricks**, selecione **registar**. Tem de ter a função de Contribuidor ou proprietário da subscrição para registar o fornecedor de recursos.


### <a name="issue-your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Problema: Sua conta {email} não tem a função de proprietário ou Contribuidor no recurso da área de trabalho do Databricks no portal do Azure

#### <a name="error-message"></a>Mensagem de erro

"O sua conta {email} não tem função proprietário ou Contribuidor no recurso da área de trabalho do Databricks no portal do Azure. Este erro também pode ocorrer se for um utilizador convidado no inquilino. Peça ao seu administrador para conceder acesso ou adicioná-lo como um utilizador diretamente na área de trabalho do Databricks." 

#### <a name="solution"></a>Solução

Seguem-se algumas soluções para este problema:

* Para inicializar o inquilino, deve estar conectado como um usuário normal do inquilino, não como um utilizador convidado. Também tem de ter uma função de contribuinte do recurso de área de trabalho do Databricks. Pode conceder um acesso de utilizador a partir da **controlo de acesso (IAM)** separador dentro de sua área de trabalho do Databricks no portal do Azure.

* Este erro também poderá ocorrer se o seu nome de domínio de e-mail é atribuído a vários diretórios no Azure AD. Para contornar este problema, crie um novo utilizador no diretório que contém a subscrição com a sua área de trabalho do Databricks.

    a. No portal do Azure, vá para o Azure AD. Selecione **utilizadores e grupos** > **adicionar um utilizador**.

    b. Adicionar um utilizador com uma `@<tenant_name>.onmicrosoft.com` de e-mail em vez de `@<your_domain>` e-mail. Pode encontrar esta opção na **domínios personalizados**, no Azure AD no portal do Azure.
    
    c. Conceder ao utilizador novo a **contribuinte** função no recurso da área de trabalho do Databricks.
    
    d. Inicie sessão no portal do Azure com o novo utilizador e localize a área de trabalho do Databricks.
    
    e. Inicie a área de trabalho do Databricks como esse usuário.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Problema: A conta de {email} não foi registada no Databricks 

#### <a name="solution"></a>Solução

Se não tiver criado a área de trabalho, e é adicionado como um utilizador, contacte a pessoa que criou a área de trabalho. Ter essa pessoa adicioná-lo utilizando a consola de administração do Azure Databricks. Para obter instruções, consulte [adicionar e gerir os utilizadores](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Se tiver criado a área de trabalho e ainda obtiver este erro, experimente selecionar **inicializar a área de trabalho** novamente a partir do portal do Azure.

### <a name="issue-cloud-provider-launch-failure-while-setting-up-the-cluster-publicipcountlimitreached"></a>Problema: Falha de inicialização do fornecedor da Cloud durante a configuração do cluster (PublicIPCountLimitReached)

#### <a name="error-message"></a>Mensagem de erro

"Falha de inicialização do fornecedor da cloud: foi encontrado um erro de fornecedor de cloud durante a configuração do cluster. Para obter mais informações, consulte o Guia do Databricks. Código de erro do Azure: PublicIPCountLimitReached. Mensagem de erro do Azure: não é possível criar mais de 60 endereços IP públicos para esta subscrição nesta região. "

#### <a name="solution"></a>Solução

Clusters de Databricks utilizam um endereço IP público por nó. Se sua assinatura já tiver utilizado a todos os IPs públicos, deve [pedido para aumentar a quota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Escolher **Quota** como o **tipo de problema**, e **de rede: ARM** como o **tipo de Quota**. Na **detalhes**, pedir um aumento de quota de endereço IP público. Por exemplo, se o limite atualmente é 60 e pretender criar um cluster de nó de 100, pedir um aumento de limite de 160.

### <a name="issue-a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster-missingsubscriptionregistration"></a>Problema: Um segundo tipo de falha de inicialização de fornecedor de cloud durante a configuração do cluster (MissingSubscriptionRegistration)

#### <a name="error-message"></a>Mensagem de erro

"Falha de inicialização do fornecedor da cloud: foi encontrado um erro de fornecedor de cloud durante a configuração do cluster. Para obter mais informações, consulte o Guia do Databricks.
Código de erro do Azure: mensagem de erro do MissingSubscriptionRegistration Azure: A subscrição não está registada para utilizar o espaço de nomes "Microsoft. Compute". Consulte https://aka.ms/rps-not-found para saber como registar subscrições. "

#### <a name="solution"></a>Solução

1. Aceda ao [Portal do Azure](https://portal.azure.com).
1. Selecione **subscrições**, a subscrição estiver a utilizar, e, em seguida **fornecedores de recursos**. 
1. Na lista de fornecedores de recursos, contra **Microsoft. Compute**, selecione **registar**. Tem de ter a função de Contribuidor ou proprietário da subscrição para registar o fornecedor de recursos.

Para obter instruções mais detalhadas, consulte [fornecedores de recursos e os tipos de](../azure-resource-manager/resource-manager-supported-services.md).

### <a name="issue-azure-databricks-needs-permissions-to-access-resources-in-your-organization-that-only-an-admin-can-grant"></a>Problema: O Azure Databricks necessita de permissões para aceder aos recursos da sua organização que apenas um administrador pode conceder.

#### <a name="background"></a>Segundo plano

O Azure Databricks está integrado com o Azure AD. Isto permite-lhe definir permissões no Azure Databricks (por exemplo, em blocos de notas ou clusters) ao especificar os utilizadores do Azure AD. Para o Azure Databricks poder listar os nomes dos utilizadores do seu Azure AD, ele requer permissão de leitura para essas informações. Isto requer um consentimento. Se o consentimento ainda não estiver disponível, verá o erro.

#### <a name="solution"></a>Solução

Inicie sessão como um administrador global para o portal do Azure. Para o Azure Active Directory, vá para o **definições de utilizador** separador e certifique-se **os utilizadores podem dar consentimento a aplicações acedam aos dados da empresa em nome deles** está definida como **Sim**.

## <a name="next-steps"></a>Passos Seguintes

- [Início rápido: Introdução ao Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [O que é o Azure Databricks?](what-is-azure-databricks.md)


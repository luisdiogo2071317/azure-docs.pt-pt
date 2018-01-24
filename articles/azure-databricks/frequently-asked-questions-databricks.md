---
title: 'Databricks do Azure: Perguntas comuns e ajuda | Microsoft Docs'
description: "Obtenha respostas às perguntas comuns e informações de resolução de problemas sobre Databricks do Azure."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: nitinme
ms.openlocfilehash: b56253e7b9c9ab144ebc4006511631756de4f89b
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Perguntas mais frequentes sobre Databricks do Azure

Este artigo apresenta uma lista de consultas superiores, poderá ter relacionados com o Azure Databricks. Também apresenta uma lista alguns problemas comuns que poderá ter ao utilizar Databricks. Para obter mais informações, consulte [que é o Azure Databricks](what-is-azure-databricks.md). 

## <a name="can-i-use-my-own-keys-for-local-encryption"></a>Pode utilizar as minhas próprias chaves de encriptação local? 
Na versão atual, utilizando as suas próprias chaves a partir do Cofre de chaves do Azure não é suportada. 

## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Pode utilizar redes virtuais do Azure com Databricks?
É criada uma nova rede virtual como parte do aprovisionamento Databricks. Nesta versão, não é possível utilizar a sua própria rede virtual do Azure.

## <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Como aceder Azure Data Lake Store a partir de um bloco de notas? 

Siga estes passos.
1. No Azure Active Directory (Azure AD), aprovisiona um principal de serviço e a chave de registo.
2. Atribua as permissões necessárias para o principal de serviço no Data Lake Store.
3. Para aceder a um ficheiro no Data Lake Store, utilize as credenciais de principal de serviço no bloco de notas.

Para obter mais informações, consulte [utilize Data Lake Store com o Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html#azure-data-lake-store).

## <a name="fix-common-problems"></a>Corrija os problemas comuns

Seguem-se alguns problemas que poderá encontrar com Databricks.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Problema: Esta subscrição não está registada para utilizar o espaço de nomes 'Microsoft.Databricks'

#### <a name="error-message"></a>Mensagem de erro

"Esta subscrição não está registada para utilizar o espaço de nomes 'Microsoft.Databricks'. Consulte https://aka.ms/rps-not-found sobre como registar subscrições. (Code: MissingSubscriptionRegistration) "

#### <a name="solution"></a>Solução

1. Aceda ao [Portal do Azure](https://portal.azure.com).
2. Selecione **subscrições**, a subscrição que está a utilizar, e, em seguida, **fornecedores de recursos**. 
3. Na lista de fornecedores de recursos, contra **Microsoft.Databricks**, selecione **registar**. Tem de ter a função de contribuinte ou o proprietário da subscrição para registar o fornecedor de recursos.


### <a name="issue-your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Problema: A sua conta {e-mail} não ter a função de proprietário ou contribuinte no recurso de área de trabalho Databricks no portal do Azure

#### <a name="error-message"></a>Mensagem de erro

"A conta {e-mail} não tem função de proprietário ou contribuinte no recurso de área de trabalho Databricks no portal do Azure. Este erro também pode ocorrer se for um utilizador convidado no inquilino. Peça ao seu administrador para conceder acesso ou adicioná-o como um utilizador diretamente na área de trabalho de Databricks." 

#### <a name="solution"></a>Solução

Seguem-se algumas soluções para este problema:

* Para inicializar o inquilino, tem de iniciar sessão como um utilizador normal do inquilino, não como um utilizador convidado. Também tem de ter uma função de contribuinte no recurso Databricks área de trabalho. Pode garantir um acesso de utilizador do **(IAM) do controlo de acesso** separador dentro da sua área de trabalho Databricks no portal do Azure.

* Este erro também poderá ocorrer se o seu nome de domínio de e-mail está atribuído a vários diretórios no Azure AD. Para contornar este problema, crie um novo utilizador no diretório que contém a subscrição com a sua área de trabalho Databricks.

    a. No portal do Azure, avance para o Azure AD. Selecione **utilizadores e grupos** > **adicionar um utilizador**.

    b. Adicionar um utilizador com um `@<tenant_name>.onmicrosoft.com` de e-mail em vez de `@<your_domain>` correio eletrónico. Pode encontrar esta opção no **domínios personalizados**, no Azure AD no portal do Azure.
    
    c. Conceder este novo utilizador o **contribuinte** função no recurso Databricks área de trabalho.
    
    d. Inicie sessão no portal do Azure com o novo utilizador e, localize a área de trabalho Databricks.
    
    e. Inicie a área de trabalho Databricks como este utilizador.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Problema: A conta {e-mail} não foi registada no Databricks 

#### <a name="solution"></a>Solução

Se não tiver criado a área de trabalho e são adicionados como um utilizador, contacte a pessoa que criou a área de trabalho. Ter essa pessoa adicioná-utilizando a consola de administração do Azure Databricks. Para obter instruções, consulte [adicionar e gerir utilizadores](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Se tiver criado a área de trabalho e ainda obtiver este erro, tente selecionar **inicializar a área de trabalho** novamente a partir do portal do Azure.

### <a name="issue-cloud-provider-launch-failure-while-setting-up-the-cluster-publicipcountlimitreached"></a>Problema: Falha ao iniciar o fornecedor de nuvem ao configurar o cluster (PublicIPCountLimitReached)

#### <a name="error-message"></a>Mensagem de erro

"Falha ao iniciar o fornecedor de nuvem: foi encontrado um erro do fornecedor de nuvem ao configurar o cluster. Para obter mais informações, consulte o guia de Databricks. Código de erro do Azure: PublicIPCountLimitReached. Mensagem de erro do Azure: não é possível criar mais de 60 endereços IP públicos para esta subscrição nesta região. "

#### <a name="solution"></a>Solução

Databricks clusters utilizam um endereço IP público por nó. Se a sua subscrição já tiver utilizado a todos os IPs públicos, deve [pedido para aumentar a quota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Escolha **Quota** como o **emitir tipo**, e **redes: ARM** como o **tipo de Quota**. No **detalhes**, pedir um aumento de quota de endereço IP público. Por exemplo, se o limite atualmente é 60 e pretender criar um cluster de nó de 100, pedir um aumento de limite a 160.

### <a name="issue-a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster-missingsubscriptionregistration"></a>Problema: Um segundo tipo de falha no início do fornecedor de nuvem ao configurar o cluster (MissingSubscriptionRegistration)

#### <a name="error-message"></a>Mensagem de erro

"Falha ao iniciar o fornecedor de nuvem: foi encontrado um erro do fornecedor de nuvem ao configurar o cluster. Para obter mais informações, consulte o guia de Databricks.
Código de erro do Azure: mensagem de erro do MissingSubscriptionRegistration Azure: A subscrição não está registada para utilizar o espaço de nomes 'Microsoft. Compute'. Consulte https://aka.ms/rps-not-found sobre como registar subscrições".

#### <a name="solution"></a>Solução

1. Aceda ao [Portal do Azure](https://portal.azure.com).
2. Selecione **subscrições**, a subscrição que está a utilizar, e, em seguida, **fornecedores de recursos**. 
3. Na lista de fornecedores de recursos, contra **Microsoft. Compute**, selecione **registar**. Tem de ter a função de contribuinte ou o proprietário da subscrição para registar o fornecedor de recursos.

Para obter instruções mais detalhadas, consulte [fornecedores de recursos e tipos](../azure-resource-manager/resource-manager-supported-services.md).

### <a name="issue-azure-databricks-needs-permissions-to-access-resources-in-your-organization-that-only-an-admin-can-grant"></a>Problema: Databricks do Azure necessita de permissões para aceder a recursos na sua organização que apenas um administrador pode conceder.

#### <a name="background"></a>Em segundo plano

Databricks do Azure está integrado com o Azure AD. Isto permite-lhe definir permissões na Databricks do Azure (por exemplo, em blocos de notas ou clusters) ao especificar os utilizadores do Azure AD. Para o Azure Databricks conseguir lista os nomes dos utilizadores do seu Azure AD, requer permissão de leitura para essas informações. Isto requer um consentimento. Se o consentimento do utilizador já não estiver disponível, consulte o erro.

#### <a name="solution"></a>Solução

Inicie sessão como um administrador global para o portal do Azure. Para o Azure Active Directory, vá para o **as definições de utilizador** separador e certifique-se **os utilizadores podem autorizar aplicações acedem aos dados da empresa em nome daqueles** está definido como **Sim**.

## <a name="next-steps"></a>Passos Seguintes

- [Início rápido: Introdução ao Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [O que é o Azure Databricks?](what-is-azure-databricks.md)


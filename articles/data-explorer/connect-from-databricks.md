---
title: Ligar ao Explorador de dados do Azure a partir do Azure Databricks com o Python
description: Este tópico mostra como utilizar a biblioteca de Python no Azure Databricks para aceder a dados do Explorador de dados do Azure (ADX) utilizando um dos dois métodos de autenticação.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 53f51db8d1b495f9a6faec86450d2b4e08a4fb72
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52428523"
---
# <a name="connect-to-azure-data-explorer-from-azure-databricks-using-python"></a>Ligar ao Explorador de dados do Azure a partir do Azure Databricks com o Python

O [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks) é uma plataforma de análise baseada no Apache Spark e otimizada para a plataforma de serviços cloud Microsoft Azure. Este artigo mostra-lhe como utilizar a biblioteca de Python no Azure Databricks para aceder a dados do Explorador de dados do Azure (ADX). Existem várias formas de autenticar com ADX, incluindo o início de sessão do dispositivo e aplicação do Azure Active Directory (Azure AD).

## <a name="prerequisites"></a>Pré-requisitos

- [Criar um cluster do Explorador de dados do Azure e a base de dados](/azure/data-explorer/create-cluster-database-portal)
- [Criar uma área de trabalho do Azure Databricks](/azure/azure-databricks/quickstart-create-databricks-workspace-portal#create-an-azure-databricks-workspace)

    Sob **serviço Azure Databricks**, na **escalão de preço** menu pendente, selecione **Premium**. Isto permite-lhe utilizar o Azure Databricks segredos para armazenar as suas credenciais e referenciá-los em blocos de notas e tarefas.

- [Criar um cluster](https://docs.azuredatabricks.net/user-guide/clusters/create.html) no Azure Databricks com as seguintes especificações (configurações mínimas necessárias para executar os blocos de notas do exemplo):

![Criar cluster](media/connect-from-databricks/databricks-create-cluster.png)

## <a name="install-python-library-on-your-azure-databricks-cluster"></a>Instalar a biblioteca de Python no seu cluster do Azure Databricks

Para instalar [biblioteca de Python](/azure/kusto/api/python/kusto-python-client-library) no seu cluster do Azure Databricks:

1. Aceda à sua área de trabalho do Azure Databricks e [criar uma biblioteca](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)
2. [Carregar um pacote do Python PyPI ou ovo de Python](https://docs.azuredatabricks.net/user-guide/libraries.html#upload-a-python-pypi-package-or-python-egg)
    - Carregar, instalar e anexar biblioteca ao seu cluster do Databricks.
    - Introduza o nome de PyPi: *dados do azure-kusto*

## <a name="connect-to-adx-using-device-login"></a>Ligar ao ADX com início de sessão do dispositivo

[Importar um bloco de notas](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) utilizando o [consulta-ADX-dispositivo-início de sessão](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-device-login.ipynb) bloco de notas para ligar ao ADX com as suas credenciais.

## <a name="connect-to-adx-using-azure-ad-app"></a>Ligar ao ADX com aplicações do Azure AD

1. Criar aplicações do Azure AD por [aprovisionamento de uma aplicação AAD](/azure/kusto/management/access-control/how-to-provision-aad-app).
1. Conceder acesso à aplicação do Azure AD na sua base de dados do Explorador de dados do Azure da seguinte forma:

    ```kusto
    .set database <DB Name> users ('aadapp=<AAD App ID>;<AAD Tenant ID>') 'AAD App to connect Spark to ADX
    ```
    |   |   |
    | - | - |
    | ```DB Name``` | nome da sua base de dados |
    | ```AAD App ID``` | o ID da aplicação do Azure AD |
    | ```AAD Tenant ID``` | o ID de inquilino do Azure AD |

### <a name="find-your-azure-ad-tenant-id"></a>Encontrar o seu ID de inquilino do Azure AD

Para autenticar uma aplicação, o Explorador de dados do Azure utiliza o ID de inquilino do Azure AD. Para localizar o ID de inquilino, utilize o seguinte URL, substituindo o domínio pelo *SeuDomínio*.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Por exemplo, se o seu domínio for *contoso.com*, o URL é: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Clique neste URL para ver os resultados; a primeira linha é igual à seguinte. 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

O inquilino ID é `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. 

### <a name="store-and-secure-your-azure-ad-app-id-and-key"></a>Store e proteger o seu ID de aplicação do Azure AD e a chave 

Store e proteger o seu ID de aplicação do Azure AD e a chave com o Azure Databricks [segredos](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) da seguinte forma:
1. [Configurar a CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-the-cli)
1. [Instalar a CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli) 
1. [Configurar a autenticação](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-authentication).
1. Configurar o [segredos](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) com os comandos de exemplo seguintes:

    ```databricks secrets create-scope --scope adx```

    ```databricks secrets put --scope adx --key myaadappid```

    ```databricks secrets put --scope adx --key myaadappkey```

    ```databricks secrets list --scope adx```

### <a name="import-a-notebook"></a>Importar um bloco de notas
[Importar um bloco de notas](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) utilizando o [consulta-ADX-AAD-App](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-AAD-App.ipynb) bloco de notas para ligar ao ADX. Atualize os valores de marcador de posição com seu nome do cluster, o nome de base de dados e o ID de inquilino do Azure AD.
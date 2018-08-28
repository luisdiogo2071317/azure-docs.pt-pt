---
title: Início Rápido - Definir e obter um segredo do Azure Key Vault com uma aplicação Web do Node | Microsoft Docs
description: Início Rápido - Definir e obter um segredo do Azure Key Vault com uma aplicação Web do Node
services: key-vault
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 0188d06e5c58287e1040f6a15456d3ffe291b04a
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42022624"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-a-net-web-app"></a>Início Rápido: Definir e obter um segredo do Azure Key Vault com uma aplicação Web .NET

Neste início rápido, irá conhecer os passos necessários para que uma aplicação Web do Azure leia as informações do Cofre de chaves com identidades de serviço geridas. Saiba como:

> [!div class="checklist"]
> * Criar um Key Vault.
> * Armazene um segredo no Key Vault.
> * Obter um segredo do Cofre de Chaves.
> * Criar uma aplicação Web do Azure.
> * [Ativar as identidades de serviço geridas](../active-directory/managed-service-identity/overview.md).
> * Conceda as permissões exigidas para a aplicação ler dados do Cofre de chaves.

Antes de continuarmos, leia os [conceitos básicos](key-vault-whatis.md#basic-concepts).

>[!NOTE]
Para compreender o motivo do tutorial abaixo, precisamos de compreender alguns conceitos. O Key Vault é um repositório central para armazenar segredos de forma programática. Mas para tal, primeiro as aplicações/utilizadores têm de autenticar para o Key Vault, ou seja, apresentar um segredo. Para seguir as melhores práticas de segurança, este primeiro segredo também tem de ser trocado periodicamente. Mas com aplicações de [Identidade de Serviço Gerida](../active-directory/managed-service-identity/overview.md) executadas no Azure, é dada uma identidade que é gerida automaticamente pelo Azure. Isto ajuda a resolver o **Problema de Introdução do Segredo**, onde os utilizadores/aplicações podem seguir as melhores práticas e não precisa de se preocupar sobre trocar o primeiro segredo

## <a name="prerequisites"></a>Pré-requisitos

* No Windows:
  * [Visual Studio 2017 versão 15.7.3 ou posterior](https://www.microsoft.com/net/download/windows) com as seguintes cargas de trabalho:
    * Desenvolvimento ASP.NET e Web
    * Desenvolvimento de várias plataformas .NET core
  * [SDK .NET Core 2.1 ou posterior](https://www.microsoft.com/net/download/windows)

* No Mac:
  * https://visualstudio.microsoft.com/vs/mac/

* Todas as plataformas:
  * Transfira o GIT a partir de [aqui](https://git-scm.com/downloads).
  * Uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
  * [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Precisa da versão 2.0.4 ou posterior da CLI do Azure. Está disponível para Windows, Mac e Linux

## <a name="login-to-azure"></a>Iniciar sessão no Azure

   Para iniciar sessão no Azure através da CLI, pode escrever:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az-group-create). Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos.

Selecione um nome do Grupo de Recursos e preencha o marcador de posição.
O exemplo seguinte cria um grupo de recursos com o nome *<YourResourceGroupName>* na localização *eastus*.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

O grupo de recursos que acabou de criar é utilizado ao longo deste artigo.

## <a name="create-an-azure-key-vault"></a>Criar um Azure Key Vault

A seguir, vai criar um Key Vault no grupo de recursos criado no passo anterior. Forneça as seguintes informações:

* Nome do cofre - **Selecione um Nome de Cofre de Chaves aqui**. O nome do Cofre de Chaves tem de ser uma cadeia com 3 a 24 carateres de comprimento que contenha apenas (0-9, a-z, A-Z e -).
* Nome do grupo de recursos - **Selecione um Nome do Grupo do Recursos aqui**.
* A localização - **E.U.A. Leste**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

Nesta altura, a sua conta do Azure é a única autorizada a realizar quaisquer operações neste novo cofre.

## <a name="add-a-secret-to-key-vault"></a>Adicionar um segredo ao Key Vault

Estamos a adicionar um segredo para ajudar a ilustrar a forma como isto funciona. Pode armazenar uma cadeia de ligação do SQL ou outras informações que precise de manter em segurança, mas mantenha disponível para a sua aplicação. Neste tutorial, a palavra-passe será chamada de **AppSecret** e irá armazenar o valor de **MySecret** no mesmo.

Escreva os comandos abaixo para criar um segredo no Key Vault denominado **AppSecret** que irá armazenar o valor **MySecret**:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Para ver o valor contido no segredo como texto simples:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Este comando apresenta informações do segredo, incluindo o URI. Depois de concluir estes passos, deve ter um URI para um segredo num Azure Key Vault. Tome nota destas informações. Irá precisar delas noutro passo.

## <a name="clone-the-repo"></a>Clonar o repositório

Clone o repositório para fazer uma cópia local, para editar a origem ao executar o seguinte comando:

```
git clone https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart.git
```

## <a name="open-and-edit-the-solution"></a>Abrir e editar a solução

Edite o ficheiro program.cs para executar o exemplo com o nome do cofre de chaves específico.

1. Navegue para a pasta key-vault-dotnet-core-quickstart
2. Abra o ficheiro key-vault-dotnet-core-quickstart.sln no Visual Studio 2017
3. Abra o ficheiro Program.cs e atualize o marcador de posição <YourKeyVaultName> com o nome do seu Cofre de Chaves que criou anteriormente.

Esta solução utiliza as bibliotecas NuGet [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) e [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)

## <a name="run-the-app"></a>Executar a aplicação

No menu principal do Visual Studio 2017, escolha Depurar > Iniciar sem Depurar. Quando o browser é apresentado, navegue para a página Sobre. O valor para o AppSecret é apresentado.

## <a name="publish-the-web-application-to-azure"></a>Publicar a aplicação Web no Azure

Estamos a publicar esta aplicação no Azure para vê-la em direto como uma aplicação Web e ver também que vamos obter o valor secreto

1. No Visual Studio, selecione o Projeto **key-vault-dotnet-core-quickstart**.
2. Selecione **Publicar** e, em seguida, **Iniciar**.
3. Crie um novo **Serviço de Aplicações**, selecione **Publicar**.
4. Altere o Nome da Aplicação para "keyvaultdotnetcorequickstart"
5. Selecione **Criar**.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-managed-service-identities-msi"></a>Ativar Identidades de Serviço Geridas (MSI)

O Azure Key Vault oferece uma forma de armazenar credenciais e outras chaves e segredos em segurança, mas o código tem de se autenticar no Azure Key Vault para poder obtê-los. A Identidade de Serviço Gerida (MSI) simplifica isto ao dar aos serviços do Azure uma identidade gerida automaticamente no Azure Active Directory (Azure AD). Pode utilizar esta identidade para autenticar em qualquer serviço que suporte a autenticação do Azure AD, incluindo o Key Vault, sem ser necessário ter credenciais no seu código.

1. Voltar à CLI do Azure
2. Execute o comando assign-identity para criar a identidade para esta aplicação:

```azurecli
az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
```

>[!NOTE]
>Este comando é o equivalente a ir para o portal e mudar a **Identidade do serviço gerido** para **Ativada** nas propriedades da aplicação Web.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Atribuir permissões à sua aplicação para ler segredos do Cofre de Chaves

Tome nota da saída quando [publicar a aplicação no Azure][]. Deverá estar no formato:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Em seguida, execute este comando com o nome do seu Cofre de Chaves e o valor de PrincipalId copiado acima:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get

```

**Agora quando executar a aplicação deverá ver o seu valor secreto obtido**

## <a name="next-steps"></a>Passos seguintes

* [Home Page do Azure Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Documentação do Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)
* [SDK do Azure Para .NET](https://github.com/Azure/azure-sdk-for-net)
* [Referência à API REST do Azure](https://docs.microsoft.com/rest/api/keyvault/)

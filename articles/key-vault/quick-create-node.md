---
title: Início Rápido - Definir e obter um segredo do Azure Key Vault com uma aplicação Web do Node | Microsoft Docs
description: Início Rápido - Definir e obter um segredo do Azure Key Vault com uma aplicação Web do Node
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.workload: identity
ms.topic: quickstart
ms.date: 09/05/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 2b114a4aed812a91a9f6c4ed43f57411e47ea677
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54260033"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-a-node-web-app"></a>Início rápido: Definir e obter um segredo a partir do Azure Key Vault com uma aplicação Web Node 

Este início rápido mostra-lhe como armazenar um segredo num cofre de chaves e como utilizar uma aplicação Web para obtê-lo. Para ver o valor secreto, teria de executar isto no Azure. O início rápido utiliza o Node.js e identidades geridas para os recursos do Azure.

> [!div class="checklist"]
> * Criar um Key Vault.
> * Armazene um segredo no Key Vault.
> * Obter um segredo do Cofre de Chaves.
> * Criar uma aplicação Web do Azure.
> * Ative uma [identidade gerida](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) para a aplicação Web.
> * Conceda as permissões exigidas para a aplicação ler dados do Cofre de chaves.

Antes de avançar, confirme que está familiarizado com os [conceitos básicos](key-vault-whatis.md#basic-concepts).

>[!NOTE]
Para compreender o motivo do tutorial abaixo, precisamos de compreender alguns conceitos. O Key Vault é um repositório central para armazenar segredos de forma programática. Mas para tal, primeiro as aplicações/utilizadores têm de autenticar para o Key Vault, ou seja, apresentar um segredo. Para seguir as melhores práticas de segurança, este primeiro segredo também tem de ser trocado periodicamente. Mas com [geridos identidades para recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md) aplicações executadas no Azure recebem uma identidade que é gerida automaticamente pelo Azure. Isto ajuda a resolver o **Problema de Introdução do Segredo**, onde os utilizadores/aplicações podem seguir as melhores práticas e não precisa de se preocupar sobre trocar o primeiro segredo

## <a name="prerequisites"></a>Pré-requisitos

* [Node JS](https://nodejs.org/en/)
* [Git](https://www.git-scm.com/)
* [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 ou posterior
* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="login-to-azure"></a>Iniciar sessão no Azure

Para iniciar sessão no Azure através da CLI, pode escrever:

```azurecli
az login
```

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az-group-create). Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos.

Selecione um nome do Grupo de Recursos e preencha o marcador de posição.
O exemplo seguinte cria um grupo de recursos com o nome *<YourResourceGroupName>* na localização *eastus*.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

O grupo de recursos que acabou de criar é utilizado ao longo deste tutorial.

## <a name="create-an-azure-key-vault"></a>Criar um Azure Key Vault

A seguir, vai utilizar o grupo de recursos criado no passo anterior para criar um cofre de chaves. Embora "ContosoKeyVault" seja utilizado como o nome do cofre de chaves ao longo deste artigo, tem de utilizar um nome exclusivo. Forneça as seguintes informações:

* Nome do cofre - **selecione o nome de um cofre de chaves aqui**.
* Nome do grupo de recursos - **selecione o nome de um grupo de recursos aqui**.
* A localização - **E.U.A. Leste**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

Nesta altura, a sua conta do Azure é a única autorizada a realizar quaisquer operações neste novo cofre.

## <a name="add-a-secret-to-key-vault"></a>Adicionar um segredo ao Cofre de chaves

Estamos a adicionar um segredo para ajudar a ilustrar a forma como isto funciona. Pode armazenar uma cadeia de ligação do SQL ou outras informações que precise de manter em segurança, mas mantenha disponível para a sua aplicação. Neste tutorial, a palavra-passe será chamada de **AppSecret** e irá armazenar o valor de **MySecret** no mesmo.

Escreva os comandos abaixo para criar um segredo no Key Vault denominado **AppSecret** que irá armazenar o valor **MySecret**:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Para ver o valor contido no segredo como texto simples:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Este comando apresenta informações do segredo, incluindo o URI. Depois de concluir estes passos, deve ter um URI para um segredo num Azure Key Vault. Aponte estas informações. Irá precisar delas noutro passo.

## <a name="clone-the-repo"></a>Clonar o Repositório

Clone o repositório para fazer uma cópia local, para editar a origem ao executar o seguinte comando:

```
git clone https://github.com/Azure-Samples/key-vault-node-quickstart.git
```

## <a name="install-dependencies"></a>Instalar dependências

Aqui, instalamos as dependências. Execute os seguintes comandos cd key-vault-node-quickstart npm install

Este projeto utilizou módulos de dois nós:

* [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure) 
* [azure-keyvault](https://www.npmjs.com/package/azure-keyvault)

## <a name="publish-the-web-application-to-azure"></a>Publicar a aplicação Web no Azure

Os passos que temos de seguir estão abaixo

- O primeiro passo é criar um Plano do [Serviço de Aplicações do Azure](https://azure.microsoft.com/services/app-service/). Pode armazenar várias aplicações Web nesse plano.

    ```
    az appservice plan create --name myAppServicePlan --resource-group myResourceGroup
    ```
- De seguida, criamos uma aplicação Web. No exemplo a seguir, substitua <app_name> com um nome de aplicação globalmente exclusivo (os carateres válidos são a-z, 0-9 e -). O runtime está definido como NODE|6.9. Para ver todos os tempos de execução suportados, execute az webapp list-runtimes
    ```
    # Bash
    az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9" --deployment-local-git
    ```
    Quando a aplicação Web tiver sido criada, a CLI do Azure mostra informações semelhantes ao seguinte exemplo:
    ```
    {
      "availabilityState": "Normal",
      "clientAffinityEnabled": true,
      "clientCertEnabled": false,
      "cloningInfo": null,
      "containerSize": 0,
      "dailyMemoryTimeQuota": 0,
      "defaultHostName": "<app_name>.azurewebsites.net",
      "enabled": true,
      "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git"
      < JSON data removed for brevity. >
    }
    ```
    Navegue para a aplicação Web que acabou de criar e deverá vê-la a funcionar. Substitua <app_name> por um nome de aplicação exclusivo.

    ```
    http://<app name>.azurewebsites.net
    ```
    O comando anterior também cria uma aplicação ativada para Git, o que lhe permite implementar no Azure a partir do seu git local. 
    O git local está configurado com o URL “https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git”

- Crie um utilizador de implementação. Após a conclusão do comando anterior, pode adicionar um Azure remoto ao seu repositório de Git local. Substitua <url> pelo URL do Git remoto que obteve em Ativar Git para a sua aplicação.

    ```
    git remote add azure <url>
    ```

## <a name="enable-a-managed-identity-for-the-web-app"></a>Ativar uma identidade gerida para a aplicação Web

O Azure Key Vault oferece uma forma de armazenar credenciais e outras chaves e segredos em segurança, mas o código tem de se autenticar no Key Vault para poder obtê-los. A [descrição geral das identidades geridas para os recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md) simplifica a resolução deste problema ao conferir aos serviços do Azure uma identidade gerida automaticamente no Microsoft Azure Active Directory (Microsoft Azure AD). Pode utilizar esta identidade para autenticar em qualquer serviço que suporte a autenticação do Azure AD, incluindo o Key Vault, sem ser necessário ter credenciais no seu código.

Execute o comando assign-identity para criar a identidade para esta aplicação:

```azurecli
az webapp identity assign --name <app_name> --resource-group "<YourResourceGroupName>"
```

Este comando é o equivalente a aceder ao portal e mudar a definição **Identidade/Sistema atribuído** para **Ativado** nas propriedades da aplicação Web.

### <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Atribuir permissões à sua aplicação para ler segredos do Cofre de Chaves

Aponte ou copie o resultado do comando anterior. Deverá estar no formato:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Em seguida, execute este comando com o nome do seu Cofre de Chaves e o valor de PrincipalId copiado acima:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get
```

## <a name="deploy-the-node-app-to-azure-and-retrieve-the-secret-value"></a>Implementar a Aplicação Node no Azure e obter o valor do segredo

Agora que já está tudo pronto, execute o seguinte comando para implementar a aplicação no Azure

```
git push azure master
```

Depois disso, quando navegar para https://<app_name>.azurewebsites.net, poderá ver o valor do segredo.
Confirme que substituiu o nome <YourKeyVaultName> pelo nome do seu cofre

## <a name="next-steps"></a>Passos Seguintes

* [Home Page do Azure Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Documentação do Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)
* [Azure SDK For Node](https://docs.microsoft.com/javascript/api/overview/azure/key-vault) (SDK do Azure para Node)
* [Referência à API REST do Azure](https://docs.microsoft.com/rest/api/keyvault/)

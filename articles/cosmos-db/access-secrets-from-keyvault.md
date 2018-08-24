---
title: Utilizar o Key Vault para armazenar e aceder a chaves do Azure Cosmos DB | Documentos da Microsoft
description: Utilize o Azure Key Vault para armazenar e aceder a Azure Cosmos DB ligação da cadeia de caracteres, chaves, URI.
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: rafats
ms.openlocfilehash: 11aac0ab4476494f74453ca64a1b77964197adaa
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/23/2018
ms.locfileid: "42748051"
---
# <a name="use-key-vault-to-store-and-access-azure-cosmos-db-keys"></a>Utilizar o Key Vault para armazenar e aceder a chaves do Azure Cosmos DB

Ao utilizar o Azure Cosmos DB para as suas aplicações, pode acessar o banco de dados, coleções, documentos, utilizando o URI do ponto final e a chave no ficheiro de configuração da aplicação.  No entanto, não é seguro colocar chaves e o URL diretamente no código da aplicação, porque eles estão disponíveis no formato de texto não encriptado para todos os utilizadores. Pretende certificar-se de que o URI e as chaves são disponíveis, mas através de um mecanismo seguro. Isso é que o Azure Key Vault pode ajudar-lhe armazenar e gerir segredos da aplicação de forma segura.

Os seguintes passos são necessários para armazenar e ler as chaves de acesso do Azure Cosmos DB a partir do Key Vault:

* Criar um Key Vault  
* Adicionar chaves de acesso do Azure Cosmos DB para o Key Vault  
* Criar uma aplicação web do Azure  
* Registar a aplicação & conceder permissões para ler o Key Vault  


## <a name="create-a-key-vault"></a>Criar um Key Vault

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).  
2. Selecione **criar um recurso > Segurança > Key Vault**.  
3. Na secção **Criar cofre de chaves**, forneça as seguintes informações:  
   * **Nome:** forneça um nome exclusivo para o seu Cofre de chaves.  
   * **Subscrição:** escolha a subscrição que irá utilizar.  
   * Em **Grupo de Recursos**, selecione **Criar novo** e introduza um nome de grupo de recursos.  
   * No menu pendente de localização, escolha uma localização.  
   * Deixe as outras opções com os valores predefinidos.  
4. Depois de fornecer as informações acima, selecione **Criar**.  

## <a name="add-azure-cosmos-db-access-keys-to-the-key-vault"></a>Adicione chaves de acesso do Azure Cosmos DB para o Key Vault.
1. Navegue para o Cofre de chaves que criou no passo anterior, abra a **segredos** separador.  
2. Selecione **+ gerar/importar**, 

   * Selecione **Manual** para **opções de carregamento**.
   * Fornecer um **nome** para seu segredo
   * Fornecer a cadeia de ligação da sua conta do Cosmos DB para o **valor** campo. E, em seguida, selecione **criar**.

   ![Criar um segredo](./media/access-secrets-from-keyvault/create-a-secret.png)

4. Depois do segredo é criado, abra-o e copie o * * identificador de segredo que é o seguinte formato. Irá utilizar este identificador na próxima seção. 

   `https://<Key_Vault_Name>.vault.azure.net/secrets/<Secret _Name>/<ID>`

## <a name="create-an-azure-web-application"></a>Criar uma aplicação web do Azure

1. Criar uma aplicação web do Azure ou pode transferir a aplicação a partir da [repositório do GitHub](https://github.com/rsarosh/CosmosDB-KeyVault). É uma simple aplicação de MVC.  

2. Deszipe o aplicativo baixado e abra o **HomeController.cs** ficheiro. Atualize o ID secreto na linha a seguir:

   `var secret = await keyVaultClient.GetSecretAsync("<Your Key Vault’s secret identifier>")`

3. **Guarde** o arquivo **criar** a solução.  
4. Em seguida, implemente a aplicação no Azure. Clique com o botão direito do rato no projeto e escolha **publicar**. Criar um novo perfil de serviço de aplicações (pode nomear a aplicação WebAppKeyVault1) e selecione **publicar**.   

5. Assim que a aplicação é implementada. A partir do portal do Azure, navegue para a aplicação web que implementou e ativar a **identidade do serviço gerido** desta aplicação.  

   ![Identidade de serviço gerida](./media/access-secrets-from-keyvault/turn-on-managed-service-identity.png)

Se quiser executar o aplicativo agora, verá o seguinte erro, como qualquer permissão para esta aplicação não tiver dado no Cofre de chaves.

![Aplicação implementada sem acesso](./media/access-secrets-from-keyvault/app-deployed-without-access.png)

## <a name="register-the-application--grant-permissions-to-read-the-key-vault"></a>Registar a aplicação & conceder permissões para ler o Key Vault

Nesta secção, registar a aplicação no Azure Active Directory e conceder permissões para a aplicação leia o Key Vault. 

1. Navegue para o portal do Azure, abra a **Key Vault** que criou na secção anterior.  

2. Open **políticas de acesso**, selecione **+ adicionar novo** encontrar a aplicação web que implementou, selecione as permissões e selecione **OK**.  

   ![Adicionar política de acesso](./media/access-secrets-from-keyvault/add-access-policy.png)

Agora, se executar o aplicativo, é possível ler o segredo do Key Vault.

![Aplicação implementada com o segredo](./media/access-secrets-from-keyvault/app-deployed-with-access.png)
 
Da mesma forma, pode adicionar um utilizador aceder à chave do cofre. Terá de adicionar a próprio para o Key Vault ao clicar em "Políticas de acesso" e, em seguida, atribua a permissão all que terá de executar a aplicação a partir do Visual studio. Quando esta aplicação está em execução no ambiente de trabalho, que demora a sua identidade.

## <a name="next-steps"></a>Passos Seguintes

* Configurar uma firewall para Azure Cosmos DB, veja [suporte de firewall](firewall-support.md) artigo.
* Para configurar o ponto final de serviço de rede virtual, consulte [proteger o acesso ao utilizar o ponto final de serviço de VNet](vnet-service-endpoint.md) artigo.



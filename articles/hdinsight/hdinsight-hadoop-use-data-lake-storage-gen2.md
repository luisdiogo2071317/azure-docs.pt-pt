---
title: Utilize a geração 2 de armazenamento do Azure Data Lake com clusters do HDInsight do Azure
description: Saiba como criar clusters Gen2 de armazenamento do Azure Data Lake com o Azure HDInsight, utilize.
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: howto
ms.date: 01/10/2019
ms.author: hrasheed
ms.openlocfilehash: a44e53d7a32ab151fa951d1bc89b741390a70dfb
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55464794"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Utilize a geração 2 de armazenamento do Azure Data Lake com clusters do HDInsight do Azure

Geração 2 de armazenamento do Data Lake (armazenamento do Data Lake) do Azure é um conjunto de recursos dedicado a análise de macrodados, criado no armazenamento de Blobs do Azure. Geração 2 de armazenamento do Data Lake é o resultado da combinação de recursos de armazenamento de Blobs do Azure e de geração 1 de armazenamento do Azure Data Lake. O resultado é um serviço que oferece recursos de geração 1 armazenamento do Azure Data Lake, como semântica do sistema de arquivo, diretório e segurança ao nível do ficheiro e dimensionamento, juntamente com o armazenamento em camadas e de baixo custo, capacidades de recuperação de alta disponibilidade/desastre do Blob do Azure armazenamento.

## <a name="data-lake-storage-gen2-availability"></a>Disponibilidade de Lake Gen2 de armazenamento de dados

Geração de armazenamento 2 do Azure Data Lake está disponível como uma opção de armazenamento para quase todos os tipos de cluster do Azure HDInsight como um padrão e uma conta de armazenamento adicional. HBase, no entanto, pode ter apenas uma conta de geração 2 de armazenamento do Data Lake.

> [!Note] 
> Depois de selecionar geração 2 de armazenamento do Data Lake como sua **tipo de armazenamento primário**, não é possível selecionar uma conta de geração 1 de armazenamento do Data Lake como armazenamento adicional.

## <a name="creating-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Criar um cluster do HDInsight com geração 2 de armazenamento do Data Lake

## <a name="using-the-azure-portal"></a>Utilizar o portal do Azure

Para criar um cluster do HDInsight, que usa a geração 2 de armazenamento do Data Lake para o armazenamento, utilize os seguintes passos para criar uma conta de geração 2 de armazenamento do Data Lake está configurada corretamente.

1. Crie uma identidade gerida atribuído ao utilizador, se ainda não tiver uma. Ver [Create, lista, delete ou o atribuir uma função para uma identidade gerida atribuído ao utilizador com o portal do Azure](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

    ![Criar uma identidade gerida atribuída pelo utilizador](./media/hdinsight-hadoop-data-lake-storage-gen2/create-user-assigned-managed-identity-portal.png)

1. Crie uma conta de armazenamento de geração 2 de armazenamento do Azure Data Lake. Certifique-se de que o **sistema de ficheiros hierárquico** opção está ativada. Consulte [início rápido: Criar uma conta de armazenamento de geração 2 de armazenamento do Azure Data Lake](../storage/blobs/data-lake-storage-quickstart-create-account.md) para obter mais detalhes.

    ![Captura de ecrã que mostra a criação da conta de armazenamento no portal do Azure](./media/hdinsight-hadoop-data-lake-storage-gen2/azure-data-lake-storage-account-create-advanced.png)
 
1. Atribuir a identidade gerida para o **contribuinte de dados de Blob de armazenamento (pré-visualização)** função na conta de armazenamento. Consulte [gerir direitos de acesso aos dados de Blobs do Azure e a fila com o RBAC (pré-visualização)](../storage/common/storage-auth-aad-rbac.md#assign-a-role-scoped-to-the-storage-account-in-the-azure-portal)

    1. Na [portal do Azure](https://portal.azure.com), navegue até à sua conta de armazenamento.
    1. Selecione a sua conta de armazenamento, em seguida, selecione **controlo de acesso (IAM)** para apresentar as definições de controlo de acesso para a conta. Selecione o **atribuições de funções** separador para ver a lista de atribuições de funções.
    
        ![Captura de ecrã que mostra as definições de controlo de acesso de armazenamento](./media/hdinsight-hadoop-data-lake-storage-gen2/portal-access-control.png)
    
    1. Clique nas **adicionar atribuição de função** botão para adicionar uma nova função.
    1. Na **adicionar atribuição de função** janela, selecione a **contribuinte de dados de Blob de armazenamento (pré-visualização)** função. Em seguida, selecione a subscrição que tem a conta de armazenamento e de identidade gerida. Em seguida, procure para localizar a identidade de gerido atribuído ao utilizador que criou anteriormente. Por fim, selecione a identidade gerida e serão listada sob **membros selecionados**.
    
        ![Captura de ecrã que mostra como atribuir uma função RBAC](./media/hdinsight-hadoop-data-lake-storage-gen2/add-rbac-role2.png)
    
    1. Clique em **Guardar**. A identidade de utilizador atribuída que selecionou é agora listada sob a **contribuinte** função.

    1. Depois de concluída a configuração inicial, pode criar um cluster através do portal. O cluster tem de estar na mesma região do Azure como a conta de armazenamento. Na **armazenamento** secção do menu de criação do cluster, selecione as seguintes opções:
        
        * Para **tipo de armazenamento primário**, clique em **Gen2 de armazenamento do Azure Data Lake**.
        * Sob **Selecione uma conta de armazenamento**, procure e selecione a conta de armazenamento de geração 2 de armazenamento do Data Lake recém-criado.
        
            ![Definições de armazenamento para a utilização de geração 2 de armazenamento do Data Lake com o Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/primary-storage-type-adls-gen2.png)
        
        * Sob **identidade** selecione a subscrição correta e o recém-criado atribuído ao utilizador identidade gerida.
        
            ![Definições de identidade para a utilização de geração 2 de armazenamento do Data Lake com o Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/managed-identity-cluster-creation.png)

### <a name="using-a-resource-manager-template-deployed-with-azure-cli"></a>Com um modelo de Gestor de recursos implementado com a CLI do Azure

Pode baixar um exemplo [aqui o ficheiro de modelo](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/hdinsight-adls-gen2-template.json) e uma [aqui o ficheiro de parâmetros de exemplo](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/parameters.json). Antes de utilizar o modelo, substitua o ID de subscrição do Azure real para a cadeia de caracteres `<SUBSCRIPTION_ID>`. Além disso, substitua a palavra-passe escolhida para a cadeia de caracteres `<PASSWORD>` para definir a senha de logon que irá utilizar para iniciar sessão no seu cluster, bem como a palavra-passe SSH.

O trecho de código abaixo executa as seguintes etapas iniciais:

1. Início de sessão sua conta do Azure.
1. Defina a subscrição ativa em que serão executadas as operações de criar.
1. Criar um novo grupo de recursos para as atividades de implementação novo `hdinsight-deployment-rg`.
1. Criar um utilizador Managed Service Identity (MSI) `test-hdinsight-msi`.
1. Adicione uma extensão a CLI do Azure para utilizar as funcionalidades de geração 2 de armazenamento do Data Lake.
1. Criar uma nova conta de geração 2 de armazenamento do Data Lake `hdinsightadlsgen2`, utilizando o `--hierarchical-namespace true` sinalizador.

```azurecli
az login
az account set --subscription <subscription_id>

#create resource group
az group create --name hdinsight-deployment-rg --location eastus

# Create managed identity
az identity create -g hdinsight-deployment-rg -n test-hdinsight-msi

az extension add --name storage-preview

az storage account create --name hdinsightadlsgen2 \
    --resource-group hdinsight-deployment-rg \
    --location eastus --sku Standard_LRS \
    --kind StorageV2 --hierarchical-namespace true
```

Em seguida, inicie sessão no portal e adicione o novo MSI para o **contribuinte de dados de Blob de armazenamento (pré-visualização)** função na conta de armazenamento, conforme descrito no passo 3 acima sob [com o portal do Azure](hdinsight-hadoop-use-data-lake-storage-gen2.md#using-the-azure-portal).

Depois de concluir a atribuição de função no portal do MSI, avance para implementar o modelo com o trecho de código abaixo.

```azurecli
az group deployment create --name HDInsightADLSGen2Deployment \
    --resource-group hdinsight-deployment-rg \
    --template-file hdinsight-adls-gen2-template.json \
    --parameters parameters.json
```

## <a name="access-control-for-data-lake-storage-gen2-in-hdinsight"></a>Controlo de acesso para a geração 2 de Lake armazenamento de dados no HDInsight

### <a name="what-kinds-of-permissions-does-data-lake-storage-gen2-support"></a>Os tipos de permissão suporta a geração 2 de armazenamento do Data Lake?

Geração de armazenamento 2 do Azure Data Lake implementa um modelo de controle de acesso que suporte o controlo de acesso com base do Azure funções (RBAC) e listas de controlo de acesso POSIX semelhante ao (ACLs). Apresenta uma lista de controlo de acesso do Data Lake Gen1 de armazenamento apenas suportado para controlar o acesso aos dados.

Do Azure com base em função de controlo de acesso (RBAC) utiliza as atribuições de funções para aplicar efetivamente os conjuntos de permissões a utilizadores, grupos e principais de serviço para recursos do Azure. Normalmente, esses recursos do Azure estão restritos a recursos de nível superior (por exemplo, contas de armazenamento do Azure). Para armazenamento do Azure e também de geração 2 de armazenamento do Azure Data Lake, esse mecanismo foi expandido para o recurso de sistema de ficheiros.

 Para obter mais informações sobre as permissões de arquivo com o RBAC, veja [controlo de acesso baseado em funções do Azure (RBAC)](../storage/blobs/data-lake-storage-access-control.md#azure-role-based-access-control-rbac).

Para obter mais informações sobre as permissões de arquivo com ACLs, consulte [apresenta uma lista de controlo de acesso em arquivos e diretórios](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).


### <a name="how-do-i-control-access-to-my-data-in-gen2"></a>Como controlar o acesso aos meus dados na geração 2?

A capacidade para o seu cluster do HDInsight aceder a ficheiros na geração 2 de armazenamento do Data Lake é controlada através de identidades geridas. Uma identidade gerida é uma identidade registrada no Azure AD cujas credenciais são geridos pelo Azure. Não precisa de registar os principais de serviço no Azure AD e manter as credenciais, tais como certificados.

Existem dois tipos de identidades de geridos para serviços do Azure: atribuído de sistema e atribuído ao utilizador. O Azure HDInsight utiliza identidades geridas atribuído ao utilizador para acesso de geração 2 de armazenamento do Azure Data Lake. Uma identidade gerida atribuído ao utilizador é criada como uma autónoma recursos do Azure. Através de um processo de criação, o Azure cria uma identidade no inquilino do Azure AD no qual a subscrição que está a ser utilizada confia. Depois de criada, a identidade pode ser atribuída a uma ou mais instâncias do serviço do Azure. O ciclo de vida das identidades atribuídas pelo utilizador é gerido separadamente do ciclo de vida das instâncias do serviço do Azure ao qual estão atribuídas. Para obter mais informações sobre identidades geridas, consulte [como é que as identidades geridas para o trabalho de recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).

### <a name="how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-using-hive-or-other-services"></a>Como posso definir permissões para utilizadores do Azure AD para consultar dados na geração 2 de armazenamento do Data Lake com Hive ou outro serviço?

Utilize grupos de segurança do Azure AD como o principal atribuído nas ACLs. Não atribua diretamente a utilizadores individuais ou principais de serviço com permissões de acesso de arquivo. Quando utiliza grupos de segurança do AD para controlar o fluxo de permissões, pode adicionar e remover utilizadores ou principais de serviço sem reaplicando ACLs para uma estrutura de diretório inteiro. Só tem de adicionar ou remover os utilizadores do apropriado grupo de segurança do Azure AD. ACLs não são herdadas e, então, reaplicando ACLs requer a atualização a ACL em todos os ficheiros e o subdiretório.

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar a pré-visualização do Azure Data Lake Storage Gen2 com clusters do HDInsight do Azure](../storage/blobs/data-lake-storage-use-hdi-cluster.md)
* [Atualizar a integração do Azure HDInsight com a pré-visualização de geração 2 de armazenamento do Data Lake - ACL e segurança](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/)
* [Introdução à pré-visualização do Azure Data Lake Storage geração 2](../storage/blobs/data-lake-storage-introduction.md)

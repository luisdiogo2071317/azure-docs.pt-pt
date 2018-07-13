---
title: Criar e gerir a base de dados para pontos finais de serviço de MySQL VNet e regras com a CLI do Azure | Documentos da Microsoft
description: Este artigo descreve como criar e gerir a base de dados para pontos finais de serviço de MySQL VNet e regras utilizando a linha de comandos da CLI do Azure.
services: mysql
author: mbolz
ms.author: mbolz
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: fd8b21d1273b1bd02b0a949894be53cdc4a5c3c0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38597885"
---
# <a name="create-and-manage-azure-database-for-mysql-vnet-service-endpoints-using-azure-cli"></a>Criar e gerir a base de dados para pontos finais de serviço de VNet do MySQL com a CLI do Azure
Pontos finais de serviços de rede (VNet) virtual e regras de estendem o espaço de endereços privados de uma rede Virtual à sua base de dados do Azure para o servidor MySQL. Utilizar comandos de Interface de linha de comandos (CLI do Azure) conveniente, pode criar, atualizar, eliminar, listar e Mostrar pontos finais de serviço de VNet e regras para gerir o seu servidor. Para uma visão geral da base de dados do Azure para MySQL VNet pontos finais de serviço, incluindo as limitações, consulte [base de dados do Azure para pontos finais de serviço do MySQL Server VNet](concepts-data-access-and-security-vnet.md). Pontos finais de serviço de VNet estão disponíveis em pré-visualização pública em todas as regiões suportadas para a base de dados do Azure para MySQL.

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de procedimentos, terá de:
- Instale [CLI 2.0 do Azure](/cli/azure/install-azure-cli) utilitário da linha de comandos ou utilize o Azure Cloud Shell no browser.
- Uma [base de dados do Azure para servidor MySQL e base de dados](quickstart-create-mysql-server-database-using-azure-cli.md).

> [!NOTE]
> Suporte para pontos finais de serviço da VNet é apenas para fins gerais e memória otimizada de servidores.

## <a name="configure-vnet-service-endpoints-for-azure-database-for-mysql"></a>Configurar pontos finais de serviço de Vnet para a base de dados do Azure para MySQL
O [vnet de rede de az](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest) comandos são utilizados para configurar redes virtuais.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Para ver a versão instalada, execute o comando `az --version`. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli). 

Se estiver a executar a CLI localmente, tem de iniciar sessão na sua conta através do comando [az login](/cli/azure/authenticate-azure-cli?view=interactive-log-in). Anote a propriedade **id** da saída de comando para o nome de subscrição correspondente.
```azurecli-interactive
az login
```

Instalar a extensão da CLI para base de dados para pontos finais de serviço de VNet do MySQL com o `az extension add --name rdbms-vnet` comando. 
```azurecli-interactive
az extension add --name rdbms-vnet
```

Execute o `az extension list` comando para verificar a instalação da extensão da CLI.
```azurecli-interactive
az extension list
```
A saída do comando lista todas as extensões instaladas. A base de dados do Azure para a extensão da CLI do MySQL é:

 {"extensionType": "whl", "nome": "rdbms-vnet", "versão": "10.0.0"}

> [!NOTE]
> Para desinstalar a extensão da CLI, execute o `az extension remove -n rdbms-vnet` comando. 

Se tiver várias subscrições, escolha a subscrição adequada na qual o recurso deve ser cobrado. Selecione o ID da subscrição específica na sua conta com o comando [az account set](/cli/azure/account#az_account_set). Substitua a propriedade **id** da saída **az login** da sua subscrição no marcador de posição de id de subscrição.

- A conta tem de ter as permissões necessárias para criar uma rede virtual e o ponto final de serviço.

Pontos finais de serviço podem ser configurados em redes virtuais de forma independente por um utilizador com acesso de escrita para a rede virtual.

Para obter os recursos de serviço do Azure para uma VNet, o utilizador tem de ter permissão para "Microsoft.Network/JoinServicetoaSubnet" para as sub-redes que estão a ser adicionadas. Esta permissão está incluída por predefinição nas funções incorporadas de administrador de serviço e podem ser modificadas mediante a criação de funções personalizadas.

Saiba mais sobre [funções incorporadas](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) e a atribuição de permissões específicas a [funções personalizadas](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

As VNets e os recursos de serviço do Azure podem pertencer às mesmas subscrições ou a subscrições diferentes. Se os recursos de serviço de VNet e o Azure estão em subscrições diferentes, os recursos devem existir abaixo do mesmo inquilino do Active Directory (AD) aquando desta pré-visualização.

> [!IMPORTANT]
> É altamente recomendável para ler este artigo sobre considerações e configurações de ponto final de serviço antes de executar o script de exemplo abaixo, ou a configuração de pontos finais de serviço. **Ponto final de serviço de rede virtual:** uma [ponto final de serviço de rede Virtual](../virtual-network/virtual-network-service-endpoints-overview.md) é uma sub-rede cujos valores de propriedade incluem um ou mais nomes de tipo de serviço do Azure formal. Pontos de extremidade de serviços de VNet utilizam o nome do tipo de serviço **Microsoft. SQL**, que faz referência ao serviço do Azure com o nome da base de dados SQL. Esta etiqueta de serviço também se aplica a SQL Database do Azure, base de dados do Azure para PostgreSQL e MySQL serviços. É importante ter em conta ao aplicar a **Microsoft. SQL** etiqueta de serviço para um ponto de extremidade do serviço de VNet configura o tráfego de ponto final de serviço para todos os serviços de base de dados do Azure, incluindo o SQL Database do Azure, base de dados do Azure para PostgreSQL e Base de dados do Azure para MySQL servidores na sub-rede. 
> 

### <a name="sample-script-to-create-an-azure-database-for-mysql-database-create-a-vnet-vnet-service-endpoint-and-secure-the-server-to-the-subnet-with-a-vnet-rule"></a>Exemplo de script para criar uma base de dados do Azure para MySQL, criar uma VNet, o ponto final de serviço de VNet e proteger o servidor para a sub-rede com uma regra de VNet
Neste script de exemplo, altere as linhas realçadas para personalizar o nome de utilizador administrador e a palavra-passe. Substitua o SubscriptionID utilizado na `az account set --subscription` comando com o seu próprio identificador de subscrição.
[!code-azurecli-interactive[main](../../cli_scripts/mysql/create-mysql-server-vnet/create-mysql-server.sh?highlight=5,20 "Create an Azure Database for MySQL, VNet, VNet service endpoint, and VNet rule.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.
[!code-azurecli-interactive[main](../../cli_scripts/mysql/create-mysql-server-vnet/delete-mysql.sh "Delete the resource group.")]

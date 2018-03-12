---
title: Utilizar a CLI de Databricks a partir da Shell de nuvem do Azure | Microsoft Docs
description: Saiba como utilizar a CLI Databricks a partir da Shell de nuvem do Azure.
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
ms.date: 03/02/2018
ms.author: nitinme
ms.openlocfilehash: 8e4213813ff23586ac683556d4a3c0c587edea58
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="use-databricks-cli-from-azure-cloud-shell"></a>Utilizar a CLI do Databricks a partir do Azure Cloud Shell

Saiba como utilizar a CLI Databricks a partir da Shell de nuvem do Azure para executar operações no Databricks.

## <a name="prerequisites"></a>Pré-requisitos

* Uma área de trabalho do Azure Databricks e o cluster. Para obter instruções, consulte [introdução ao Azure Databricks](quickstart-create-databricks-workspace-portal.md). 

* Configure um token de acesso pessoal Databricks. Para obter instruções, consulte [Token gestão](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).

## <a name="use-the-azure-cloud-shell"></a>Utilizar a Shell de nuvem do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
 
2. No canto superior direito, clique em de **nuvem Shell** ícone.

   ![Inicie o Shell de nuvem](./media/databricks-cli-from-azure-cloud-shell/launch-azure-cloud-shell.png "iniciar ODBC a partir do Excel")

3. Certifique-se de que seleciona **Bash** para o ambiente de nuvem Shell. Pode selecionar da opção de menu pendente, conforme mostrado na captura de ecrã seguinte.

   ![Inicie o Shell de nuvem](./media/databricks-cli-from-azure-cloud-shell/select-bash-for-shell.png "iniciar ODBC a partir do Excel") 

4. Crie um ambiente virtual que pode instalar a CLI Databtricks. O fragmento abaixo, vai criar um ambiente virtual chamado `databrickscli`.

       virtualenv -p /usr/bin/python2.7 databrickscli

5. Mudar para o ambiente virtual que criou.

       source databrickscli/bin/activate

6. Instale a CLI de Databricks.

       pip install databricks-cli

7. Configure a autenticação com Databricks utilizando o token de acesso que tem de criar, listado como parte dos pré-requisitos. Utilize o seguinte comando:

       databricks configure --token

    Receber os seguintes avisos:

    * Lhe for pedido que introduza o anfitrião Databricks. O valor no formato de Ente `https://eastus2.azuredatabricks.net`. Aqui, **EUA Leste 2** é a região do Azure onde criou a sua área de trabalho Databricks do Azure.

    * Lhe for pedido que introduza um nome de utilizador. Introduza **token**.

    * Por fim, lhe for pedido que introduza a palavra-passe. Introduza o token que criou anteriormente.

Depois de concluir estes passos, pode começar a utilizar a CLI de Databricks a partir da Shell de nuvem do Azure.

## <a name="use-databricks-cli"></a>Utilizar a CLI de Databricks

Agora pode começar a utilizar a CLI Databricks. Por exemplo, execute o seguinte comando para listar todos os clusters de Databricks tem na sua área de trabalho.

    databricks clusters list

Também pode utilizar o seguinte comando para aceder ao sistema de ficheiros de Databricks (DBFS).

    databricks fs ls


Para uma referência completa de comandos, consulte [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html).


## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre a CLI do Azure, consulte [descrição geral da CLI do Azure](../cloud-shell/overview.md)
* Para ver uma lista de comandos para a CLI do Azure, consulte [referência da CLI do Azure](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
* Para ver uma lista de comandos para a CLI de Databricks, consulte [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html)



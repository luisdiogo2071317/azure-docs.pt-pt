---
title: Adicionar utilizadores a uma conta do Azure Data Lake Analytics
description: Saiba como corretamente adicionar utilizadores à sua conta do Data Lake Analytics
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: db35f16e-1565-4873-a851-bd987accdc58
ms.topic: conceptual
ms.date: 05/24/2018
ms.openlocfilehash: 58b78c7d9769069f36c9f01c2a7650878a6c5ec9
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34717233"
---
# <a name="adding-a-user-in-the-azure-portal"></a>A adição de um utilizador no portal do Azure

## <a name="start-the-add-user-wizard"></a>Iniciar o Assistente de utilizador para adicionar
1. Abra o Azure Data Lake Analytics através de https://portal.azure.com.
2. Clique em **adicionar utilizador assistente**.
3. No **selecionar utilizador** passo, localize o utilizador que pretende adicionar. Clique em **Selecionar**.
4. o **função selecione** passo, escolha **para programadores do Data Lake Analytics**. Esta função tem o conjunto mínimo de permissões necessárias para submeter/monitorizar/gerir tarefas de U-SQL. Se o grupo não se destina a gestão de serviços do Azure, atribua a esta função.
5. No **selecione catálogo permissões** passo, selecione quaisquer bases de dados adicionais esse utilizador terá acesso a. Leitura e escrita à base de dados mestra é necessário para submeter tarefas. Quando tiver terminado, clique em **OK**.
6. No passo final chamado **atribuir as permissões seleccionadas** rever as alterações que faz com que o assistente. Clique em **OK**.


## <a name="configure-acls-for-data-folders"></a>Configurar ACLs para pastas de dados
Conceder "R-X" ou "RWX", conforme necessário, em pastas que contêm dados de entrada e saída de dados.


## <a name="optionally-add-the-user-to-the-azure-data-lake-store-role-reader-role"></a>Opcionalmente, adicione o utilizador à função do Azure Data Lake Store **leitor** função.
1.  Localize a sua conta do Azure Data Lake Store.
2.  Clique em **utilizadores**.
3. Clique em **Adicionar**.
4.  Selecione uma função de RBAC do Azure para atribuir este grupo.
5.  Atribua a função de leitor. Esta função tem o conjunto mínimo de permissões necessárias para procurar/gerir dados armazenados no ADLS. Se o grupo não se destina a gestão de serviços do Azure, atribua a esta função.
6.  Escreva o nome do grupo.
7.  Clique em **OK**.

## <a name="adding-a-user-using-powershell"></a>Adição de um utilizador com o PowerShell

1. Siga as instruções neste guia: [como instalar e configurar o Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Transferir o [adicionar AdlaJobUser.ps1](https://github.com/Azure/AzureDataLake/blob/master/Samples/PowerShell/ADLAUsers/Add-AdlaJobUser.ps1) script do PowerShell.
3. Execute o script do PowerShell. 

O comando de exemplo para conceder acesso de utilizador para submeter tarefas, ver novos metadados de tarefa e a vista de metadados antigo são:

`Add-AdlaJobUser.ps1 -Account myadlsaccount -EntityToAdd 546e153e-0ecf-417b-ab7f-aa01ce4a7bff -EntityType User -FullReplication`


## <a name="next-steps"></a>Passos Seguintes

* [Descrição geral do Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Introdução ao Data Lake Analytics utilizando o portal do Azure](data-lake-analytics-get-started-portal.md)
* [Gerir o Azure Data Lake Analytics utilizando o Azure PowerShell](data-lake-analytics-manage-use-powershell.md)


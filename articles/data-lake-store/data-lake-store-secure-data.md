---
title: Proteger os dados armazenados no Azure Data Lake Store | Microsoft Docs
description: Saiba como proteger dados no Azure Data Lake Store utilizando grupos e listas de controlo de acesso
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 5b83f02c55d0aa7b2e122d7fc8c9ef5734cdd924
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="securing-data-stored-in-azure-data-lake-store"></a>Proteger os dados armazenados no Azure Data Lake Store
Proteger dados no Azure Data Lake Store é uma abordagem de três passos.  Ambos baseada em funções (RBAC) de controlo de acesso e listas de controlo de acesso (ACLs) tem de ser definidas para permitir totalmente o acesso aos dados para os utilizadores e grupos de segurança.

1. Comece por criar grupos de segurança no Azure Active Directory (AAD). Estes grupos de segurança são utilizados para implementar o controlo de acesso baseado em funções (RBAC) no portal do Azure. Para obter mais informações, consulte [controlo de acesso baseado em funções no Microsoft Azure](../role-based-access-control/role-assignments-portal.md).
2. Atribua os grupos de segurança do AAD para a conta do Azure Data Lake Store. Controla o acesso à conta do Data Lake Store das operações de gestão de portal e do portal ou de APIs.
3. Atribua os grupos de segurança do AAD como acesso (ACL) de listas de controlo no sistema de ficheiros de Data Lake Store.
4. Além disso, também pode definir um intervalo de endereços IP para clientes que podem aceder aos dados no Data Lake Store.

Este artigo fornece instruções sobre como utilizar o portal do Azure para realizar as tarefas acima. Para obter informações aprofundadas sobre como o Data Lake Store implementa a segurança ao nível da conta e de dados, consulte [segurança no Azure Data Lake Store](data-lake-store-security-overview.md). Para informações de descrição profunda sobre como as ACLs são implementadas no Azure Data Lake Store, consulte [descrição geral do controlo de acesso no Data Lake Store](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar este tutorial, tem de ter o seguinte:

* **Uma subscrição do Azure**. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Uma conta do Azure Data Lake Store**. Para obter instruções sobre como criar um, consulte [introdução ao Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Criar grupos de segurança no Azure Active Directory
Para obter instruções sobre como criar grupos de segurança do AAD e como adicionar utilizadores ao grupo, consulte [gerir grupos de segurança no Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

> [!NOTE] 
> Pode adicionar utilizadores e outros grupos a um grupo no Azure AD através do portal do Azure. No entanto, para poder adicionar um principal de serviço a um grupo, utilize [módulo do PowerShell do Azure AD](../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).
> 
> ```powershell
> # Get the desired group and service principal and identify the correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add the service principal to the group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Atribuir utilizadores ou grupos de segurança para contas de Azure Data Lake Store
Quando atribui os utilizadores ou grupos de segurança para contas de Azure Data Lake Store, controlar o acesso às operações de gestão da conta a utilizar o portal do Azure e as APIs do Azure Resource Manager. 

1. Abra uma conta do Azure Data Lake Store. No painel esquerdo, clique em **todos os recursos**e, em seguida, no painel todos os recursos, clique no nome de conta para o qual pretende atribuir um utilizador ou grupo de segurança.

2. No painel de conta do Data Lake Store, clique em **controlo de acesso (IAM)**. O painel por predefinição apresenta os proprietários da subscrição como proprietário.
   
    ![Atribua o grupo de segurança à conta do Azure Data Lake Store](./media/data-lake-store-secure-data/adl.select.user.icon.png "atribuir o grupo de segurança à conta do Azure Data Lake Store")

3. No **controlo de acesso (IAM)** painel, clique em **adicionar** para abrir o **adicionar permissões** painel. No **adicionar permissões** painel, selecione um **função** para o utilizador/grupo. Procure o grupo de segurança que criou anteriormente no Azure Active Directory e selecione-o. Se tiver um grande número de utilizadores e grupos a pesquisar a partir do, utilize o **selecione** caixa de texto para filtrar o nome do grupo. 
   
    ![Adicionar uma função para o utilizador](./media/data-lake-store-secure-data/adl.add.user.1.png "adicionar uma função de utilizador")
   
    O **proprietário** e **contribuinte** função fornecer acesso a uma variedade de funções de administração sobre a conta do data lake. Para os utilizadores que irão interagir com os dados na lake de dados, mas ainda é necessário para ver informações de gestão de conta, pode adicioná-los para o **leitor** função. O âmbito destas funções está limitado para as operações de gestão relacionados com a conta do Azure Data Lake Store.
   
    Para operações de dados, as permissões do sistema de ficheiros individuais definem que os utilizadores podem fazer. Por conseguinte, um utilizador ter uma função de leitor pode ver apenas as definições administrativas associadas à conta, mas pode potencialmente ler e escrever dados com base nas permissões do sistema de ficheiros atribuídas. Permissões de sistema de ficheiros do Data Lake Store são descritas em [atribuir grupo de segurança como ACLs o sistema de ficheiros do Azure Data Lake Store](#filepermissions).

    > [!IMPORTANT]
    > Apenas o **proprietário** função ativa automaticamente ao sistema de ficheiros. O **contribuinte**, **leitor**, e todas as outras funções requerem ACLs permitir qualquer nível de acesso a ficheiros e pastas.  O **proprietário** função fornece permissões de pasta que não podem ser substituídas através de ACLs de ficheiros de utilizador Super-obrigatórios. e. Para obter mais informações sobre como políticas RBAC mapeiam para acesso a dados, consulte [RBAC para a gestão de conta](data-lake-store-security-overview.md#rbac-for-account-management).

4. Se pretender adicionar um utilizador/grupo não está listado no **adicionar permissões** painel, pode convidá-los, escrevendo o respetivo endereço de e-mail no **selecione** caixa de texto e, em seguida, selecionando-los a partir da lista.
   
    ![Adicionar um grupo de segurança](./media/data-lake-store-secure-data/adl.add.user.2.png "adicionar um grupo de segurança")
   
5. Clique em **Guardar**. Deverá ver o grupo de segurança adicionado conforme mostrado abaixo.
   
    ![Grupo de segurança adicionado](./media/data-lake-store-secure-data/adl.add.user.3.png "grupo de segurança adicionado")

6. O grupo de segurança/utilizador tem agora acesso à conta do Azure Data Lake Store. Se pretender fornecer acesso a utilizadores específicos, pode adicioná-los ao grupo de segurança. Da mesma forma, se de que pretende revogar o acesso de um utilizador, pode removê-los do grupo de segurança. Também pode atribuir vários grupos de segurança para uma conta. 

## <a name="filepermissions"></a>Atribuir utilizadores ou grupos de segurança como ACLs para o sistema de ficheiros do Azure Data Lake Store
Através da atribuição de grupos de segurança/utilizador para o sistema de ficheiros do Azure Data Lake, definir o controlo de acesso em dados armazenados no Azure Data Lake Store.

1. No painel da conta do Data Lake Store, clique em **Explorador de Dados**.
   
    ![Ver dados através do Explorador de dados](./media/data-lake-store-secure-data/adl.start.data.explorer.png "ver dados através do Explorador de dados")
2. No **Explorador de dados** painel, clique na pasta para o qual pretende configurar a ACL e, em seguida, clique em **acesso**. Para atribuir ACLs para um ficheiro, tem primeiro de clicar em ficheiro para pré-visualizá-lo e, em seguida, clique em **acesso** do **pré-visualização do ficheiro** painel.
   
    ![Definir ACLs no sistema de ficheiros do Data Lake](./media/data-lake-store-secure-data/adl.acl.1.png "definir ACLs no sistema de ficheiros do Data Lake")
3. O **acesso** painel lista os proprietários e atribuir permissões já atribuídas para a raiz. Clique em de **adicionar** ícone para adicionar a ACL de acesso adicionais.
    > [!IMPORTANT]
    > Definir permissões de acesso para um ficheiro único não necessariamente conceder um acesso de utilizador/grupo a que o ficheiro. O caminho para o ficheiro tem de estar acessível para o utilizador/grupo atribuído. Para obter mais informações e exemplos, consulte [cenários comuns relacionadas com permissões](data-lake-store-access-control.md#common-scenarios-related-to-permissions).
   
    ![Lista de acesso padrão e personalizado](./media/data-lake-store-secure-data/adl.acl.2.png "lista acesso padrão e personalizado")
   
   * O **proprietários** e **todos pessoa** fornecer acesso de UNIX estilo, onde pode Especifica ler, escrever, executar (rwx) para três classes de utilizador distintos: proprietário, grupo e outros.
   * **Atribuir permissões** corresponde às ACLs de POSIX permitem-lhe definir as permissões para utilizadores nomeados específicos ou grupos para além do ficheiro proprietário ou grupo. 
     
     Para obter mais informações, consulte [HDFS ACLs](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Para obter mais informações sobre como as ACLs são implementadas no Data Lake Store, consulte [controlo de acesso no Data Lake Store](data-lake-store-access-control.md).
4. Clique em de **adicionar** ícone para abrir o **atribuir permissões** painel. Neste painel, clique em **selecionar utilizador ou grupo**e, em seguida, no **selecionar utilizador ou grupo** painel, procure o grupo de segurança que criou anteriormente no Azure Active Directory. Se tiver muitos grupos a pesquisar a partir do, utilize a caixa de texto na parte superior para filtrar o nome do grupo. Clique no grupo que pretende adicionar e, em seguida, clique em **selecione**.
   
    ![Adicione um grupo](./media/data-lake-store-secure-data/adl.acl.3.png "adicionar um grupo")
5. Clique em **selecionar permissões**, selecione as permissões, se as permissões devem ser aplicadas para recursivamente e se pretende atribuir as permissões como acesso ACL, predefinido ACL, ou ambos. Clique em **OK**.
   
    ![Atribuir permissões para agrupar](./media/data-lake-store-secure-data/adl.acl.4.png "atribuir permissões de grupo")
   
    Para obter mais informações sobre as permissões no Data Lake Store e predefinido/acesso ACLs, consulte [controlo de acesso no Data Lake Store](data-lake-store-access-control.md).
6. Depois de clicar em **Ok** no **selecionar permissões** painel, o grupo recentemente adicionado e permissões associadas serão agora listadas no **acesso** painel.
   
    ![Atribuir permissões para agrupar](./media/data-lake-store-secure-data/adl.acl.5.png "atribuir permissões de grupo")
   
   > [!IMPORTANT]
   > Na versão atual, pode ter até 28 entradas em **atribuir permissões**. Se pretender adicionar mais do que 28 utilizadores, deve criar grupos de segurança, adicionar utilizadores para grupos de segurança, adicione fornecer acesso aos grupos de segurança para a conta de Data Lake Store.
   > 
   > 
7. Se necessário, também pode modificar as permissões de acesso, depois de ter adicionado o grupo. Desmarque ou selecione a caixa de verificação para cada tipo de permissão (leitura, escrita, execução) com base em se pretende remover ou atribuir essa permissão ao grupo de segurança. Clique em **guardar** para guardar as alterações ou **rejeitar** de anular as alterações.

## <a name="set-ip-address-range-for-data-access"></a>Definir o intervalo de endereços IP para acesso a dados
O Azure Data Lake Store permite-lhe bloqueio mais baixo de acesso ao seu arquivo de dados ao nível da rede. Pode ativar a firewall, especifique um endereço IP ou definir um intervalo de endereços IP para os seus clientes fidedignos. Uma vez ativada, apenas os clientes com endereços IP dentro do intervalo definido podem ligar para o arquivo.

![As definições da firewall e IP acesso](./media/data-lake-store-secure-data/firewall-ip-access.png "endereço IP e definições da Firewall")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Remover grupos de segurança para uma conta do Azure Data Lake Store
Quando remover grupos de segurança das contas do Azure Data Lake Store, está a alterar apenas acesso para as operações de gestão da conta a utilizar o Portal do Azure e as APIs do Azure Resource Manager.  

Acesso aos dados é inalterado e ainda é gerido pelo ACL de acesso.  A exceção são os utilizadores/grupos na função de proprietários.  Os utilizadores/grupos removidos da função de proprietários deixaram de ser superutilizadores e o respetivo acesso retrocede para as definições de ACL de acesso. 

1. No painel de conta do Data Lake Store, clique em **controlo de acesso (IAM)**. 
   
    ![Atribua o grupo de segurança à conta do Azure Data Lake](./media/data-lake-store-secure-data/adl.select.user.icon.png "atribuir o grupo de segurança para a conta do Azure Data Lake")
2. No **controlo de acesso (IAM)** painel, clique nos grupos de segurança que pretende remover. Clique em **remover**.
   
    ![Grupo de segurança removido](./media/data-lake-store-secure-data/adl.remove.group.png "removido de grupo de segurança")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Remova as ACLs do grupo de segurança do sistema de ficheiros do Azure Data Lake Store
Quando remover o grupo de segurança ACLs do sistema de ficheiros do Azure Data Lake Store, alterar o acesso aos dados do Data Lake Store.

1. No painel da conta do Data Lake Store, clique em **Explorador de Dados**.
   
    ![Criar diretórios na conta do Data Lake](./media/data-lake-store-secure-data/adl.start.data.explorer.png "criar diretórios na conta do Data Lake")
2. No **Explorador de dados** painel, clique na pasta para o qual pretende remover a ACL e, em seguida, clique em **acesso**. Para remover ACLs para um ficheiro, tem primeiro de clicar em ficheiro para pré-visualizá-lo e, em seguida, clique em **acesso** do **pré-visualização do ficheiro** painel. 
   
    ![Definir ACLs no sistema de ficheiros do Data Lake](./media/data-lake-store-secure-data/adl.acl.1.png "definir ACLs no sistema de ficheiros do Data Lake")
3. No **acesso** painel, clique no grupo de segurança que pretende remover. No **aceder detalhes** painel, clique em **remover**.
   
    ![Atribuir permissões para agrupar](./media/data-lake-store-secure-data/adl.remove.acl.png "atribuir permissões de grupo")

## <a name="see-also"></a>Consulte também
* [Descrição geral do Azure Data Lake Store](data-lake-store-overview.md)
* [Copiar dados de Blobs de armazenamento do Azure para o Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
* [Utilizar o Azure Data Lake Analytics com o Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Use Azure HDInsight with Data Lake Store (Utilizar o Azure HDInsight com o Data Lake Store)](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Introdução ao Data Lake Store através do PowerShell](data-lake-store-get-started-powershell.md)
* [Introdução ao Data Lake Store com .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Access diagnostic logs for Data Lake Store (Registos de diagnóstico de acesso do Data Lake Store)](data-lake-store-diagnostic-logs.md)


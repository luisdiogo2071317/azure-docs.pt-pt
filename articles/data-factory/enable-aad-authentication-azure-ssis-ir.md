---
title: Ativar autenticação do Active Directory do Azure para o runtime de integração de SSIS do Azure | Documentos da Microsoft
description: Este artigo descreve como configurar o runtime de integração Azure-SSIS para ativar as ligações que utilizam a autenticação do Azure Active Directory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: douglasl
ms.openlocfilehash: d2000e626166304e92556e3c965df175a27046ad
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53321072"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Ativar a autenticação do Azure Active Directory para o runtime de integração Azure-SSIS

Este artigo mostra-lhe como criar um IR Azure-SSIS com a identidade de serviço do Azure Data Factory. Pode utilizar a autenticação do Azure Active Directory (Azure AD) com a identidade gerida pela sua fábrica de dados do Azure em vez da autenticação do SQL para criar um runtime de integração Azure-SSIS.

Para mais informações sobre a identidade gerida pela sua fábrica de dados, veja [identidade do serviço do Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Se já tiver criado um runtime de integração Azure-SSIS com a autenticação de SQL, não é possível reconfigurar o runtime de integração para utilizar autenticação do Azure AD com o PowerShell neste momento.

## <a name="enable-azure-ad-on-azure-sql-database"></a>Ativar o Azure AD na base de dados SQL do Azure

Base de dados SQL do Azure suporta a criação de uma base de dados com um utilizador do Azure AD. Como resultado, pode definir um utilizador do Azure AD como o administrador do Active Directory e, em seguida, inicie sessão para SQL Server Management Studio (SSMS) com o utilizador do Azure AD. Em seguida, pode criar um usuário independente para o grupo do Azure AD ativar o runtime de integração criar o catálogo do SQL Server Integration Services (SSIS) no servidor.

### <a name="create-a-group-in-azure-ad-and-make-the-managed-identity-for-your-data-factory-a-member-of-the-group"></a>Criar um grupo no Azure AD e tornar a identidade gerida pela sua fábrica de dados num membro do grupo

Pode utilizar um grupo do Azure AD existente ou criar um novo com o Azure AD PowerShell.

1.  Instalar o [do Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) módulo.

2.  Inicie sessão com `Connect-AzureAD`, execute o seguinte comando para criar o grupo e guarde-o numa variável:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    A saída é semelhante ao exemplo seguinte, que também examina o valor da variável:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Adicione a identidade gerida pela sua fábrica de dados ao grupo. Pode seguir [identidade do serviço do Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) para obter o ID de identidade de serviço principal (por exemplo, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, mas não utilizar o ID de aplicação de identidade de serviço para esta finalidade).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Também pode examinar a associação ao grupo posteriormente.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Ativar a autenticação do Azure AD para a base de dados do SQL do Azure

Pode [configurar a autenticação do Azure AD para a base de dados do SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) através dos seguintes passos:

1.  No portal do Azure, selecione **todos os serviços** -> **servidores SQL** no painel de navegação esquerda.

2.  Selecione a base de dados SQL para ser ativada para autenticação do Azure AD.

3.  Na **configurações** seção do painel, selecione **administrador do Active Directory**.

4.  Na barra de comandos, selecione **definir administrador**.

5.  Selecione uma conta de utilizador do Azure AD que se tornarão um administrador do servidor e, em seguida, selecione **selecionar.**

6.  Na barra de comandos, selecione **guardar.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Criar um utilizador contido na base de dados que representa o grupo do Azure AD

Esta próxima etapa, precisa [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  Inicie o SQL Server Management Studio.

2.  Na **ligar ao servidor** caixa de diálogo, introduza o nome do servidor SQL no **nome do servidor** campo.

3.  Na **autenticação** campo, selecione **Active Directory - Universal com o suporte MFA**. (Também pode utilizar outros dois tipos de autenticação do Active Directory. Ver [configurar e gerir a autenticação do Azure Active Directory com a base de dados do SQL, instância gerida](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure).)

4.  Na **nome de utilizador** , insira o nome da conta do Azure AD que definiu como administrador do servidor - por exemplo, testuser@xxxonline.com.

5.  Selecione **Connect**. Conclua o processo de início de sessão.

6.  Na **Object Explorer**, expanda o **bases de dados** -> pasta de bases de dados do sistema.

7.  Direito, selecione-ativado **mestre** da base de dados e selecione **nova consulta**.

8.  Na janela da consulta, introduza a seguinte linha e selecione **Execute** na barra de ferramentas:

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    O comando deve ser concluído com êxito, criando o utilizador contido para o grupo.

9.  Limpar a janela de consulta, introduza a seguinte linha e selecione **Execute** na barra de ferramentas:

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    O comando deve ser concluído com êxito, o usuário independente a conceder a capacidade de criar a base de dados.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Ativar o Azure AD na instância gerida de base de dados SQL do Azure

Instância de gerida de base de dados de SQL do Azure suporta a criação de uma base de dados com o MSI diretamente. Não precisa de associar o MSI da fábrica de dados a um grupo do AD ou criar o usuário independente de MI.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database-managed-instance"></a>Ativar a autenticação do Azure AD para o Azure SQL Database Managed Instance

1.   No portal do Azure, selecione **todos os serviços** -> **servidores SQL** no painel de navegação esquerda.

1.   Selecione o SQL server para ser ativada para autenticação do Azure AD.

1.   Na **configurações** seção do painel, selecione **administrador do Active Directory**.

1.   Na barra de comandos, selecione **definir administrador**.

1.   Selecione uma conta de utilizador do Azure AD que se tornarão um administrador do servidor e, em seguida, selecione **selecione**.

1.   Na barra de comandos, selecione **guardar**.

### <a name="add-data-factory-msi-as-a-user-to-the-azure-sql-database-managed-instance"></a>Adicionar o MSI da fábrica de dados como um utilizador para o Azure SQL Database Managed Instance

1.  Inicie o SQL Server Management Studio.

2.  Inicie sessão com uma conta de administrador SQL ou a conta de administrador do Active Directory.

3.  No Object Explorer, expanda as bases de dados -> pasta de bases de dados do sistema.

4.  Com o botão direito na base de dados mestra e selecione **nova consulta**.

5.  Pode seguir o artigo [identidade do serviço do Azure Data Factory](data-factory-service-identity.md) para obter o principal serviço identidade ID da aplicação. (Não utilize o ID de identidade de serviço para esta finalidade.)

6.  Na janela da consulta, execute o seguinte script para converter o ID de aplicação de identidade de serviço para o tipo binário:

    ```sql
    DECLARE @applicationId uniqueidentifier = {your service identity application id}
    select CAST(@applicationId AS varbinary)
    ```

7.  Pode obter o valor a partir da janela de resultados.

8.  Limpar a janela de consulta e execute o seguinte script:

    ```sql
    CREATE LOGIN [{MSI name}] FROM EXTERNAL PROVIDER with SID ={your service identity application id in binary type}, TYPE = E
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{MSI name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{MSI name}]
    ```

9.  O comando for concluído com êxito.

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Aprovisionar o IR Azure-SSIS no portal

Quando aprovisionar o runtime de integração Azure-SSIS com o portal do Azure, no **definições do SQL** página, verifique a "utilização AAD autenticação com a identidade gerida para o ADF" opção. (A captura de ecrã seguinte mostra as definições para o runtime de integração com a base de dados do Azure SQL. Para o runtime de integração com a instância gerida, a propriedade de "Camada de serviço de base de dados do catálogo" não está disponível; outras configurações são iguais.)

Para mais informações sobre como criar um runtime de integração Azure-SSIS, veja [criar um runtime de integração Azure-SSIS no Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Definições para o runtime de integração Azure-SSIS](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>Aprovisionar o IR Azure-SSIS com o PowerShell

Para aprovisionar o runtime de integração Azure-SSIS com o PowerShell, efetue os seguintes procedimentos:

1.  Instalar o [do Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) módulo.

2.  No seu script, não defina a *CatalogAdminCredential* parâmetro. Por exemplo:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Description $AzureSSISDescription `
                                               -Type Managed `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -Edition $AzureSSISEdition `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```

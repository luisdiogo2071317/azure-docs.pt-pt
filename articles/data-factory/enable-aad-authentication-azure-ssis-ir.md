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
ms.date: 06/21/2018
ms.author: douglasl
ms.openlocfilehash: aa06110a6f6fe668388c6aecd98c1ddeeae37edd
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576634"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Ativar a autenticação do Azure Active Directory para o runtime de integração Azure-SSIS

Este artigo mostra-lhe como criar um IR Azure-SSIS com a identidade de serviço do Azure Data Factory. Autenticação do Active Directory (Azure AD) do Azure com o Managed Service Identity (MSI) para o runtime de integração Azure-SSIS permite-lhe utilizar o MSI da fábrica de dados em vez da autenticação de SQL para criar um runtime de integração Azure-SSIS.

Para mais informações sobre o MSI da fábrica de dados, veja [identidade do serviço do Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Se já tiver criado um runtime de integração Azure-SSIS com a autenticação de SQL, não é possível reconfigurar o runtime de integração para utilizar autenticação do Azure AD com o PowerShell neste momento.

## <a name="create-a-group-in-azure-ad-and-make-the-data-factory-msi-a-member-of-the-group"></a>Criar um grupo no Azure AD e tornar o MSI da fábrica de dados de um membro do grupo

Pode utilizar um grupo do Azure AD existente ou criar um novo com o Azure AD PowerShell.

1.  Instalar o [do Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) módulo.

2.  Inicie sessão com `Connect-AzureAD`, execute o seguinte comando para criar o grupo e guarde-o numa variável:

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

3.  Adicione o MSI da fábrica de dados ao grupo. Pode seguir [identidade do serviço do Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) para obter o ID de identidade de serviço principal (por exemplo, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, mas não utilizar o ID de aplicação de identidade de serviço para esta finalidade).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Também pode examinar a associação ao grupo posteriormente.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

## <a name="enable-azure-ad-on-azure-sql-database"></a>Ativar o Azure AD na base de dados SQL do Azure

Base de dados SQL do Azure suporta a criação de uma base de dados com um utilizador do Azure AD. Como resultado, pode definir um utilizador do Azure AD como o administrador do Active Directory e, em seguida, inicie sessão no SSMS, com o utilizador do Azure AD. Em seguida, pode criar um usuário independente para o grupo do Azure AD ativar o runtime de integração criar o catálogo do SQL Server Integration Services (SSIS) no servidor.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Ativar a autenticação do Azure AD para a base de dados do SQL do Azure

Pode [configurar a autenticação do Azure AD para a base de dados do SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) através dos seguintes passos:

1.  No portal do Azure, selecione **todos os serviços** -> **servidores SQL** no painel de navegação esquerda.

2.  Selecione a base de dados SQL para ser ativada para autenticação do Azure AD.

3.  Na **configurações** seção do painel, selecione **administrador do Active Directory**.

4.  Na barra de comandos, selecione **definir administrador**.

5.  Selecione uma conta de utilizador do Azure AD que se tornarão um administrador do servidor e, em seguida, selecione **selecionar.**

6.  Na barra de comandos, selecione **guardar.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Criar um utilizador contido na base de dados que representa o grupo do Azure AD

Esta próxima etapa, precisa [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  Inicie o SQL Server Management Studio.

2.  Na **ligar ao servidor** caixa de diálogo, introduza o nome do servidor SQL no **nome do servidor** campo.

3.  No campo **Autenticação**, selecione **Active Directory - Universal com suporte de MFA**. (Também pode utilizar outros dois tipos de autenticação do Active Directory. Ver [configurar e gerir a autenticação do Azure Active Directory com a base de dados do SQL, instância gerida](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure).)

4.  Na **nome de utilizador** campo, introduza o nome da conta do Azure AD que definiu como administrador do servidor - por exemplo, testuser@xxxonline.com.

5.  Selecione **ligar**. Conclua o processo de início de sessão.

6.  Na **Object Explorer**, expanda o **bases de dados** -> pasta de bases de dados do sistema.

7.  Direito, selecione-ativado **mestre** da base de dados e selecione **nova consulta**.

8.  Na janela da consulta, introduza a seguinte linha e selecione **Execute** na barra de ferramentas:

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

Instância de gerida de base de dados de SQL do Azure não suporta a criação de uma base de dados com qualquer utilizador do Azure AD que não seja o administrador do AD. Como resultado, é necessário definir o grupo do Azure AD como o administrador do Active Directory. Não precisa de criar o usuário independente.

Pode [configurar a autenticação do Azure AD para o servidor da SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) através dos seguintes passos:

7.  No portal do Azure, selecione **todos os serviços** -> **servidores SQL** no painel de navegação esquerda.

8.  Selecione o SQL server para ser ativada para autenticação do Azure AD.

9.  Na **configurações** seção do painel, selecione **administrador do Active Directory**.

10. Na barra de comandos, selecione **definir administrador**.

11. Procure e selecione o grupo do Azure AD (por exemplo, SSISIrGroup) e selecione **selecionar.**

12. Na barra de comandos, selecione **guardar.**

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Aprovisionar o IR Azure-SSIS no portal

Quando aprovisionar o runtime de integração Azure-SSIS com o portal do Azure, no **definições do SQL** página, verifique a "utilização AAD autenticação com o MSI do ADF" opção. (A captura de ecrã seguinte mostra as definições para o runtime de integração com a base de dados do Azure SQL. Para o runtime de integração com a instância gerida, a propriedade de "Camada de serviço de base de dados do catálogo" não está disponível; outras configurações são iguais.)

Para mais informações sobre como criar um runtime de integração Azure-SSIS, veja [criar um runtime de integração Azure-SSIS no Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Definições para o runtime de integração Azure-SSIS](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>Aprovisionar o IR Azure-SSIS com o PowerShell

Para aprovisionar o runtime de integração Azure-SSIS com o PowerShell, efetue os seguintes procedimentos:

1.  Instalar o [do Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) módulo.

2.  No seu script, não defina a *CatalogAdminCredential* parâmetro. Por exemplo:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Type Managed `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier `
                                               -Description $AzureSSISDescription `
                                               -Edition $AzureSSISEdition `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```

---
title: Ativar a autenticação do Active Directory do Azure para o tempo de execução de integração do Azure-SSIS | Microsoft Docs
description: Este artigo descreve como configurar o tempo de execução de integração de SSIS do Azure para ativar as ligações que utilizam a autenticação do Azure Active Directory.
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
ms.openlocfilehash: 93d3e25957fb1f04400fa78423a5658d32f7d5fd
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36749723"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Ativar a autenticação do Azure Active Directory para o tempo de execução de integração do Azure-SSIS

Este artigo mostra como criar uma resposta a incidentes SSIS do Azure com a identidade de serviço do Azure Data Factory. A autenticação do Active Directory (Azure AD) do Azure com o geridos serviço de identidade (MSI) para o tempo de execução de integração de SSIS do Azure permite-lhe utilizar o MSI da fábrica de dados em vez de autenticação do SQL Server para criar um tempo de execução de integração de SSIS do Azure.

Para obter mais informações sobre o MSI da fábrica de dados, consulte [identidade de serviço do Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Se já tiver criado um tempo de execução de integração de SSIS do Azure com a autenticação do SQL Server, não é possível reconfigurar a IR para utilizar autenticação do Azure AD com o PowerShell neste momento.

## <a name="create-a-group-in-azure-ad-and-make-the-data-factory-msi-a-member-of-the-group"></a>Crie um grupo no Azure AD e tornar o MSI da fábrica de dados de um membro do grupo

Pode utilizar um grupo do Azure AD existente ou crie uma nova com o Azure AD PowerShell.

1.  Instalar o [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) módulo.

2.  Inicie sessão com `Connect-AzureAD`, execute o seguinte comando para criar o grupo e guardá-lo numa variável:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    Procura o resultado semelhante ao seguinte exemplo, o que também examina o valor da variável:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Adicione o MSI da fábrica de dados ao grupo. Pode seguir [identidade de serviço do Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity) obter o ID de identidade de serviço principal (por exemplo, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, mas não utilizar o ID de aplicação do serviço de identidade para esta finalidade).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Também pode examinar a associação ao grupo posteriormente.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

## <a name="enable-azure-ad-on-azure-sql-database"></a>Ativar o Azure AD na base de dados SQL do Azure

Base de dados SQL do Azure suporta a criação de uma base de dados com um utilizador do Azure AD. Como resultado, pode definir um utilizador do Azure AD como o administrador do Active Directory e, em seguida, inicie sessão no SSMS utilizando o utilizador do Azure AD. Em seguida, pode criar um utilizador contido para o grupo do Azure AD ativar a IR criar o catálogo do SQL Server Integration Services (SSIS) no servidor.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Ativar a autenticação do Azure AD para a base de dados do SQL do Azure

Pode [configurar a autenticação do Azure AD para a base de dados do SQL Server](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure) utilizando os seguintes passos:

1.  No portal do Azure, selecione **todos os serviços** -> **servidores SQL** do painel de navegação esquerdo.

2.  Selecione a base de dados do SQL Server esteja ativado para a autenticação do Azure AD.

3.  No **definições** secção do painel, selecione **administrador do Active Directory**.

4.  Na barra de comandos, selecione **definir administrador**.

5.  Selecione uma conta de utilizador do Azure AD para tornar um administrador do servidor e, em seguida, selecione **selecionar.**

6.  Na barra de comandos, selecione **guardar.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Criar um utilizador contido na base de dados que representa o grupo do Azure AD

Para este passo seguinte, terá de [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  Inicie o SQL Server Management Studio.

2.  No **ligar ao servidor** caixa de diálogo, introduza o nome do servidor SQL no **nome do servidor** campo.

3.  No **autenticação** campo, selecione **do Active Directory - Universal com suporte MFA**. (Também pode utilizar outros dois tipos de autenticação do Active Directory. Consulte [configurar e gerir a autenticação do Azure Active Directory com a base de dados do SQL Server, instância geridos](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure).)

4.  No **nome de utilizador** campo, introduza o nome da conta do Azure AD que definir como o administrador do servidor - por exemplo, testuser@xxxonline.com.

5.  Selecione **Connect**. Conclua o processo de início de sessão.

6.  No **Object Explorer**, expanda o **bases de dados** -> pasta de bases de dados do sistema.

7.  Direito-selecione no **mestre** da base de dados e selecione **nova consulta**.

8.  Na janela da consulta, introduza a seguinte linha e selecione **executar** na barra de ferramentas:

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    O comando deve ser concluído com êxito, criar o utilizador contido para o grupo.

9.  Limpar a janela de consulta, introduza a seguinte linha e selecione **executar** na barra de ferramentas:

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    O comando deve ser concluído com êxito, conceder ao utilizador contido a capacidade de criar a base de dados.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Ativar o Azure AD numa instância gerido de base de dados SQL do Azure

Instância de geridos de base de dados de SQL do Azure não suporta a criação de uma base de dados com qualquer utilizador do Azure AD que não seja de administrador do AD. Como resultado, tem de definir o grupo do AD do Azure como o administrador do Active Directory. Não precisa de criar o utilizador nele contidos.

Pode [configurar a autenticação do Azure AD para o servidor de SQL da base de dados geridos instância](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure) utilizando os seguintes passos:

7.  No portal do Azure, selecione **todos os serviços** -> **servidores SQL** do painel de navegação esquerdo.

8.  Selecione o SQL server esteja ativado para a autenticação do Azure AD.

9.  No **definições** secção do painel, selecione **administrador do Active Directory**.

10. Na barra de comandos, selecione **definir administrador**.

11. Procure e selecione o grupo do AD do Azure (por exemplo, SSISIrGroup) e selecione **selecionar.**

12. Na barra de comandos, selecione **guardar.**

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Aprovisionar a resposta a incidentes SSIS do Azure no portal

Quando aprovisionar o IR SSIS do Azure com o portal do Azure, no **as definições de SQL** página, consulte o "utilização AAD autenticação com o MSI do ADF" opção. (A seguinte captura de ecrã mostra as definições para IR com a SQL Database do Azure. Para a resposta a incidentes com instância geridos, a propriedade "Camada de serviço de base de dados de catálogo" não está disponível; outras definições são os mesmos.)

Para obter mais informações sobre como criar um tempo de execução de integração de SSIS do Azure, consulte [criar um tempo de execução de integração do Azure-SSIS no Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime).

![Definições para o tempo de execução de integração do Azure-SSIS](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>Aprovisionar a resposta a incidentes SSIS do Azure com o PowerShell

Para aprovisionar a resposta a incidentes SSIS do Azure com o PowerShell, efetue os seguintes procedimentos:

1.  Instalar o [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) módulo.

2.  No script, não defina a *CatalogAdminCredential* parâmetro. Por exemplo:

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

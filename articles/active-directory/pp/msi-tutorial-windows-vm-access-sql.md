---
title: Utilizar um MSI de VM do Windows para aceder ao SQL do Azure
description: Um tutorial que explica o processo de uso de um Windows VM Managed Service Identity (MSI) para aceder ao SQL do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 682998bb979c9b155b7b1389d8f605018ae135b6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610937"
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-sql"></a>Utilizar um Windows VM Managed Service Identity (MSI) para aceder ao SQL do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial mostra-lhe como utilizar uma identidade de serviço gerida (MSI) para uma máquina virtual de Windows (VM) para aceder a um servidor SQL do Azure. Identidades de serviço geridas são geridas automaticamente pelo Azure e ativar a autenticação no serviços que suportam a autenticação do Azure AD, sem a necessidade de inserir as credenciais no seu código. Saiba como:

> [!div class="checklist"]
> * Ativar o MSI num VM do Windows 
> * Conceder o acesso VM a um servidor SQL do Azure
> * Obter um token de acesso com a identidade da VM e utilizá-la para consultar um Azure SQL server

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Para este tutorial, vamos criar uma nova VM do Windows.  Também pode ativar o MSI numa VM existente.

1.  Clique em **criar um recurso** no canto superior esquerdo do portal do Azure.
2.  Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3.  Introduza as informações da máquina virtual. O **nome de utilizador** e **palavra-passe** criado, Eis aqui as credenciais que utiliza para início de sessão para a máquina virtual.
4.  Selecione o elemento adequado **subscrição** para a máquina virtual na lista pendente.
5.  Para selecionar um novo **grupo de recursos** no qual criar a sua máquina virtual, escolha **criar nova**. Quando terminar, clique em **OK**.
6.  Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. Na página Definições, mantenha as predefinições e clique em **OK**.

    ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Ativar o MSI na sua VM 

Um MSI de VM permite-lhe obter os tokens de acesso do Azure AD sem a necessidade de colocar as credenciais em seu código. Ativar o MSI informa o Azure para criar uma identidade gerida para a sua VM. Nos bastidores, ativar o MSI faz duas coisas: instala a extensão de VM de MSI na sua VM e permite a MSI no Azure Resource Manager.

1.  Selecione o **Máquina Virtual** que pretende ativar o MSI em.  
2.  Na barra de navegação esquerdo, clique em **configuração**. 
3.  Verá **identidade do serviço gerido**. Para registar e ativar o MSI, selecione **Sim**, se desejar para desabilitá-lo, selecione não. 
4.  Certifique-se de que clica **guardar** para guardar a configuração.  
    ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Se desejar verificar e certifique-se de que extensões estão nesta VM, clique em **extensões**. Se estiver ativado MSI, em seguida, **ManagedIdentityExtensionforWindows** aparece na lista.

    ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Conceder o acesso VM para uma base de dados num servidor SQL do Azure

Agora pode conceder o acesso VM para uma base de dados num servidor SQL do Azure.  Para este passo, pode utilizar um SQL server existente ou crie um novo.  Para criar um novo servidor e base de dados com o portal do Azure, siga isso [guia de início rápido do Azure SQL](~/articles/sql-database/sql-database-get-started-portal.md). Também existem inícios rápidos que utilizam a CLI do Azure e o Azure PowerShell no [documentação do Azure SQL](~/articles/sql-database/index.yml).

Existem três passos para conceder o acesso VM para uma base de dados:
1.  Criar um grupo no Azure AD e tornar o MSI de VM num membro do grupo.
2.  Ative a autenticação do Azure AD para o SQL server.
3.  Criar uma **usuário independente** na base de dados que representa o grupo do Azure AD.

> [!NOTE]
> Normalmente criaria um usuário independente, que é mapeado diretamente para o MSI da VM.  Atualmente, o SQL do Azure não permite o Principal de serviço do Azure AD que representa o MSI de VM seja mapeado para um usuário independente.  Como solução suportada, tornar o MSI de VM num membro de um grupo do Azure AD, em seguida, crie um usuário independente na base de dados que representa o grupo.


### <a name="create-a-group-in-azure-ad-and-make-the-vm-msi-a-member-of-the-group"></a>Criar um grupo no Azure AD e tornar o MSI de VM num membro do grupo

Pode utilizar um grupo do Azure AD existente ou crie um novo com o Azure AD PowerShell.  

Primeiro, instale o [do Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) módulo. Em seguida, inicie sessão com `Connect-AzureAD`, execute o seguinte comando para criar o grupo e guarde-o numa variável:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

A saída é semelhante ao seguinte, que também examina o valor da variável:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM MSI access to SQL
```

Em seguida, adicione o MSI da VM para o grupo.  É necessário o MSI **ObjectId**, que pode obter com o Azure PowerShell.  Em primeiro lugar, transfira [do Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Em seguida, inicie sessão com `Connect-AzureRmAccount`, e execute os seguintes comandos para:
- Certifique-se de que contexto de sua sessão está definido para a subscrição do Azure desejada, se tiver várias.
- Listar os recursos disponíveis na sua subscrição do Azure, no, certifique-se os nomes VM e o grupo de recursos correto.
- Obter propriedades da VM de MSI, usando os valores adequados para `<RESOURCE-GROUP>` e `<VM-NAME>`.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

A saída é semelhante ao seguinte, que também examina o ID de objeto principal do serviço do MSI da VM:
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Agora, adicione o MSI de VM para o grupo.  Só pode adicionar um principal de serviço a um grupo com o Azure AD PowerShell.  Execute este comando:
```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
```

Se também examinar a associação ao grupo posteriormente, o resultado será semelhante ao seguinte:

```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
Get-AzureAdGroupMember -ObjectId $Group.ObjectId

ObjectId                             AppId                                DisplayName
--------                             -----                                -----------
b83305de-f496-49ca-9427-e77512f6cc64 0b67a6d6-6090-4ab4-b423-d6edda8e5d9f DevTestWinVM
```

### <a name="enable-azure-ad-authentication-for-the-sql-server"></a>Ativar a autenticação do Azure AD para o SQL server

Agora que criou o grupo e adicionado o MSI de VM para a associação, pode [configurar a autenticação do Azure AD para o SQL server](~/articles/sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance) através dos seguintes passos:

1.  No portal do Azure, selecione **servidores SQL** no painel de navegação esquerda.
2.  Clique no SQL server para ser ativada para autenticação do Azure AD.
3.  Na **configurações** seção do painel, clique em **administrador do Active Directory**.
4.  Na barra de comandos, clique em **definir administrador**.
5.  Selecione uma conta de utilizador do Azure AD que se tornarão um administrador do servidor e clique em **selecionar.**
6.  Na barra de comandos, clique em **guardar.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Criar um usuário independente na base de dados que representa o grupo do Azure AD

Esta próxima etapa, precisará [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Antes de começar, também pode ser útil rever os artigos seguintes para obter informações sobre a integração do Azure AD:

- [Autenticação universal com a base de dados SQL e SQL Data Warehouse (suporte do SSMS para a MFA)](~/articles/sql-database/sql-database-ssms-mfa-authentication.md)
- [Configurar e gerir a autenticação do Azure Active Directory com a base de dados SQL ou SQL Data Warehouse](~/articles/sql-database/sql-database-aad-authentication-configure.md)

1.  Inicie o SQL Server Management Studio.
2.  Na **ligar ao servidor** caixa de diálogo, introduza o nome do seu servidor SQL no **nome do servidor** campo.
3.  Na **autenticação** campo, selecione **Active Directory - Universal com o suporte MFA**.
4.  Na **nome de utilizador** , insira o nome da conta do Azure AD que definiu como administrador do servidor, por exemplo, helen@woodgroveonline.com
5.  Clique em **Opções**.
6.  Na **ligar à base de dados** , insira o nome da base de dados não pertencente ao sistema que pretende configurar.
7.  Clique em **Ligar**.  Conclua o processo de início de sessão.
8.  Na **Object Explorer**, expanda o **bases de dados** pasta.
9.  Faça duplo clique numa base de dados de utilizador e clique em **nova consulta**.
10.  Na janela de consulta, introduza a seguinte linha e clique em **Execute** na barra de ferramentas:
    
     ```
     CREATE USER [VM MSI access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     O comando deve ser concluído com êxito, criar o usuário independente para o grupo.
11.  Limpar a janela de consulta, introduza a seguinte linha e clique em **Execute** na barra de ferramentas:
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM MSI access to SQL]
     ```

     O comando deve ser concluído com êxito, o usuário independente a conceder a capacidade de ler a base de dados.

Código em execução na VM agora pode obter um token da MSI e utilizar o token para autenticar para o SQL server.

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-sql"></a>Obter um token de acesso com a identidade da VM e utilizá-la para chamar o SQL do Azure 

SQL do Azure nativo do Azure AD suporta a autenticação, para que possa aceitar tokens de acesso diretamente obtido com o MSI.  Utilizar o **token de acesso** método de criação de uma ligação para o SQL.  Isso faz parte da integração do SQL do Azure com o Azure AD e é diferente de fornecer as credenciais na cadeia de ligação.

Eis um exemplo de código do .net de abrir uma ligação para o SQL com um token de acesso.  Esse código deve ser executado na VM para poder aceder ao ponto final de MSI de VM.  **.NET framework 4.6** ou superior, é necessário usar o método de token de acesso.  Substitua os valores do AZURE-SQL-SERVERNAME e base de dados em conformidade.  Tenha em atenção o ID de recurso para o SQL do Azure é "https://database.windows.net/".

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://localhost:50342/oauth2/token?resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call MSI endpoint.
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader and extract access token.
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

//
// Open a connection to the SQL server using the access token.
//
if (accessToken != null) {
    string connectionString = "Data Source=<AZURE-SQL-SERVERNAME>; Initial Catalog=<DATABASE>;";
    SqlConnection conn = new SqlConnection(connectionString);
    conn.AccessToken = accessToken;
    conn.Open();
}
```

Em alternativa, uma forma rápida de teste a configuração de ponto a ponto sem ter de escrever e implementar uma aplicação na VM é usando o PowerShell.

1.  No portal, navegue para **máquinas virtuais** e aceda à sua máquina virtual do Windows e na **descrição geral**, clique em **Connect**. 
2.  Introduza no seu **nome de utilizador** e **palavra-passe** para que adicionou ao criar a VM do Windows. 
3.  Agora que já criou um **conexão de área de trabalho remoto** com a máquina virtual, abra **PowerShell** na sessão remota. 
4.  Através do PowerShell `Invoke-WebRequest`, fazer um pedido para o ponto de final de MSI local para obter um acesso de token para o SQL do Azure.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://database.windows.net/"} -Headers @{Metadata="true"}
    ```
    
    Converta a resposta de um objeto JSON para um objeto do PowerShell. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Extraia o token de acesso da resposta.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5.  Abra uma ligação para o SQL server. Lembre-se substituir os valores para o AZURE-SQL-SERVERNAME e base de dados.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Em seguida, criar e enviar uma consulta para o servidor.  Lembre-se substituir o valor para a tabela.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Examinar o valor de `$DataSet.Tables[0]` para ver os resultados da consulta.  Parabéns, consultar a base de dados com um MSI de VM e sem a necessidade de fornecer credenciais!

## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](msi-overview.md).
- Saiba mais sobre [suporte de SQL do Azure para a autenticação do Azure AD](~/articles/sql-database/sql-database-aad-authentication.md).
- Saiba mais sobre [configurar o suporte de SQL do Azure para a autenticação do Azure AD](~/articles/sql-database/sql-database-aad-authentication-configure.md).
- Saiba mais sobre [autenticação e acesso no SQL server](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions).

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.

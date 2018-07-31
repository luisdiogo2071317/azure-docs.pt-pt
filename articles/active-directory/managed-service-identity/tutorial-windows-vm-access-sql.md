---
title: Utilizar uma VM do Windows para aceder ao SQL do Azure
description: Um tutorial que explica o processo de utilização de uma Identidade de Serviço Gerida de VM do Windows para aceder ao SQL do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: bryanla
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: ace7f11eeea081077855a409824272b4b55f3c33
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247232"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-to-access-azure-sql"></a>Tutorial: Utilizar uma Identidade de Serviço Gerida de VM do Windows para aceder ao SQL do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Este tutorial mostra como utilizar uma Identidade de Serviço Gerida para que uma máquina virtual (VM) do Windows aceda a um SQL Server do Azure. As Identidades de Serviço Geridas são geridas automaticamente pelo Azure e permitem-lhe fazer a autenticação em serviços que suportam a autenticação do Azure AD, sem ser necessário inserir as credenciais no seu código. Saiba como:

> [!div class="checklist"]
> * Ativar a Identidade de Serviço Gerida numa VM do Windows 
> * Conceder à sua VM acesso a um SQL Server do Azure
> * Obter um token de acesso com a identidade da VM e utilizá-la para consultar um SQL Server do Azure

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Neste tutorial, vamos criar uma nova VM do Windows.  Também pode ativar a Identidade de Serviço Gerida numa VM existente.

1.  Clique no botão **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2.  Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3.  Introduza as informações da máquina virtual. O **Nome de Utilizador** e a **Palavra-passe** aqui criados são as credenciais a utilizar para iniciar sessão na máquina virtual.
4.  Escolha uma **Subscrição** para a máquina virtual na lista pendente.
5.  Para selecionar um novo **Grupo de Recursos** no qual irá criar a sua máquina virtual, selecione **Criar Novo**. Quando terminar, clique em **OK**.
6.  Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. Na página Definições, mantenha as predefinições e clique em **OK**.

    ![Texto alternativo da imagem](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-service-identity-on-your-vm"></a>Ativar a Identidade de Serviço Gerida na sua VM 

Uma Identidade de Serviço Gerida de VM permite-lhe obter os tokens de acesso do Azure AD, sem ter de colocar as credenciais no código. A ativação da Identidade de Serviço Gerida informa o Azure para criar uma identidade gerida para a sua VM. Em segundo plano, a ativação da Identidade de Serviço Gerida faz duas coisas: regista a sua VM no Azure Active Directory para criar a respetiva identidade gerida e configura a identidade na VM.

1.  Selecione a **Máquina Virtual** na qual quer ativar a Identidade de Serviço Gerida.  
2.  Na barra de navegação esquerda, clique em **Configuração**. 
3.  Vai ver a **Identidade de Serviço Gerida**. Para registar e ativar a Identidade de Serviço Gerida, selecione **Sim**; se desejar desativá-la, selecione Não. 
4.  Certifique-se de que clica em **Guardar** para guardar a configuração.  
    ![Texto alternativo da imagem](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Conceder à sua VM acesso a uma base de dados num SQL Server do Azure

Agora, pode conceder à sua VM acesso a uma base de dados num SQL Server do Azure.  Para este passo, pode utilizar um SQL Server existente ou criar um novo.  Para criar um novo servidor e base de dados com o portal do Azure, siga este [início rápido do SQL do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal). Também existem inícios rápidos que utilizam a CLI do Azure e o Azure PowerShell na [documentação do SQL do Azure](https://docs.microsoft.com/azure/sql-database/).

Existem três passos para conceder à sua VM acesso a uma base de dados:
1.  Criar um grupo no Azure AD e tornar a Identidade de Serviço Gerida de VM num membro do grupo.
2.  Ative a autenticação do Azure AD para o SQL Server.
3.  Crie um **utilizador contido** na base de dados que represente o grupo do Azure AD.

> [!NOTE]
> Normalmente, criaria um utilizador contido que mapeasse diretamente para a Identidade de Serviço Gerida da VM.  Atualmente, o SQL do Azure não permite que o Principal de Serviço do Azure AD que representa a Identidade de Serviço Gerida de VM seja mapeado para um utilizador contido.  Uma solução suportada consiste em tornar a Identidade de Serviço Gerida de VM num membro de um grupo do Azure AD e, em seguida, criar um utilizador contido na base de dados que representa o grupo.


### <a name="create-a-group-in-azure-ad-and-make-the-vm-managed-service-identity-a-member-of-the-group"></a>Criar um grupo no Azure AD e tornar a Identidade de Serviço Gerida de VM num membro do grupo

Pode utilizar um grupo do Azure AD existente ou criar um novo com o Azure AD PowerShell.  

Em primeiro lugar, instale o módulo [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2). Em seguida, inicie sessão com `Connect-AzureAD`, execute o seguinte comando para criar o grupo e guarde-o numa variável:

```powershell
$Group = New-AzureADGroup -DisplayName "VM Managed Service Identity access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

A saída é semelhante à seguinte, que também examina o valor da variável:

```powershell
$Group = New-AzureADGroup -DisplayName "VM Managed Service Identity access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM Managed Service Identity access to SQL
```

Em seguida, adicione a Identidade de Serviço Gerida da VM ao grupo.  Precisa do **ObjectId** da Identidade de Serviço Gerida, que poderá obter com o Azure PowerShell.  Em primeiro lugar, transfira o [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Em seguida, inicie sessão com `Connect-AzureRmAccount` e execute os seguintes comandos para:
- Garantir que o contexto da sessão está definido para a subscrição do Azure pretendida, caso tenha várias.
- Listar os recursos disponíveis na sua subscrição do Azure e verificar os nomes corretos do grupo de recursos e da VM.
- Obter as propriedades da VM da Identidade de Serviço Gerida, com os valores adequados para `<RESOURCE-GROUP>` e `<VM-NAME>`.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

A saída é semelhante à seguinte, que também examina o ID de Objeto do principal de serviço da Identidade de Serviço Gerida da VM:
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Agora, adicione a Identidade de Serviço Gerida da VM ao grupo.  Só pode adicionar um principal de serviço a um grupo com o Azure AD PowerShell.  Execute este comando:
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

### <a name="enable-azure-ad-authentication-for-the-sql-server"></a>Ativar a autenticação do Azure AD para o SQL Server

Agora que criou o grupo e adicionou a Identidade de Serviço Gerida da VM à associação, pode [configurar a autenticação do Azure AD para o SQL Server](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-azure-sql-server) através dos seguintes passos:

1.  No portal do Azure, selecione **SQL Servers** no painel de navegação esquerdo.
2.  Clique no SQL Server para ser ativado para autenticação do Azure AD.
3.  Na secção **Definições** do painel, clique em **Administrador do Active Directory**.
4.  Na barra de comandos, clique em **Definir administrador**.
5.  Selecione uma conta de utilizador do Azure AD para se tornar num administrador do servidor e clique em **Selecionar.**
6.  Na barra de comandos, clique em **Guardar.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Criar um utilizador contido na base de dados que representa o grupo do Azure AD

Para este próximo passo, vai precisar do [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Antes de começar, também pode ser útil rever os artigos seguintes para obter informações sobre a integração do Azure AD:

- [Autenticação Universal com a Base de Dados SQL e o SQL Data Warehouse (suporte do SSMS para MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication.md)
- [Configurar e gerir a autenticação do Azure Active Directory com a Base de Dados SQL ou o SQL Data Warehouse](/azure/sql-database/sql-database-aad-authentication-configure.md)

1.  Inicie o SQL Server Management Studio.
2.  Na caixa de diálogo **Ligar ao Servidor**, introduza o nome do seu SQL Server no campo **Nome do servidor**.
3.  No campo **Autenticação**, selecione **Active Directory - Universal com suporte de MFA**.
4.  No campo **Nome de utilizador**, introduza o nome da conta do Azure AD que definiu como administrador do servidor, por exemplo helen@woodgroveonline.com
5.  Clique em **Opções**.
6.  No campo **Ligar à base de dados**, introduza o nome da base de dados não pertencente ao sistema que pretende configurar.
7.  Clique em **Ligar**.  Conclua o processo de início de sessão.
8.  No **Object Explorer**, expanda a pasta **Databases**.
9.  Faça duplo clique numa base de dados de utilizador e clique em **Nova consulta**.
10.  Na janela de consulta, introduza a seguinte linha e clique em **Executar** na barra de ferramentas:
    
     ```
     CREATE USER [VM Managed Service Identity access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     O comando deve ser concluído com êxito, criando o utilizador contido para o grupo.
11.  Limpe a janela de consulta, introduza a seguinte linha e clique em **Executar** na barra de ferramentas:
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM Managed Service Identity access to SQL]
     ```

     O comando deve ser concluído com êxito, concedendo ao utilizador contido a capacidade de ler a base de dados completa.

O código em execução na VM pode agora obter um token da Identidade de Serviço Gerida e utilizar o token para fazer a autenticação no SQL Server.

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-sql"></a>Obter um token de acesso com a identidade da VM e utilizá-la para chamar o SQL do Azure 

O SQL do Azure suporta nativamente a autenticação do Azure AD, para que possa aceitar diretamente tokens de acesso obtidos com a Identidade de Serviço Gerida.  Vai utilizar o método de **token de acesso** de criação de uma ligação para o SQL.  Isto faz parte da integração do SQL do Azure no Azure AD e é diferente de fornecer as credenciais na cadeia de ligação.

Eis um exemplo de código .Net para abrir uma ligação para o SQL com um token de acesso.  Este código deve ser executado na VM para poder aceder ao ponto final da Identidade de Serviço Gerida da VM.  É necessário o **.NET Framework 4.6** ou superior para utilizar o método de token de acesso.  Substitua os valores de AZURE-SQL-SERVERNAME e DATABASE em conformidade.  Tenha em atenção que o ID de recurso para o SQL do Azure é "https://database.windows.net/".

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call Managed Service Identity endpoint.
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

Em alternativa, uma forma rápida de testar a configuração ponto a ponto sem ter de escrever e implementar uma aplicação na VM é utilizando o PowerShell.

1.  No portal, navegue para **Máquinas Virtuais**, aceda à sua máquina virtual do Windows e, em **Descrição Geral**, clique em **Ligar**. 
2.  Introduza o seu **Nome de Utilizador** e a **Palavra-passe** que adicionou quando criou a VM do Windows. 
3.  Agora que já criou uma **Ligação ao Ambiente de Trabalho Remoto** com a máquina virtual, abra o **PowerShell** na sessão remota. 
4.  Através de `Invoke-WebRequest` do PowerShell, faça um pedido ao ponto final da Identidade de Serviço Gerida local para obter um token de acesso para o SQL do Azure.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fdatabase.windows.net%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    Converta a resposta de um objeto JSON num objeto do PowerShell. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Extraia o token de acesso da resposta.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5.  Abra uma ligação para o SQL Server. Lembre-se de substituir os valores de AZURE-SQL-SERVERNAME e DATABASE.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Em seguida, crie e envie uma consulta para o servidor.  Lembre-se de substituir o valor de TABLE.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Examine o valor de `$DataSet.Tables[0]` para ver os resultados da consulta.  Parabéns! Consultou a base de dados com uma Identidade de Serviço Gerida de VM e sem precisar de fornecer credenciais!

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a criar uma Identidade de Serviço Gerida para aceder ao SQL Server do Azure.  Para saber mais sobre o SQL Server do Azure, veja:

> [!div class="nextstepaction"]
>[Serviço Base de Dados SQL do Azure](/azure/sql-database/sql-database-technical-overview)

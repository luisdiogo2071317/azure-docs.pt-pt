---
title: Automatizar tarefas de serviços de análise do Azure com os principais de serviço | Microsoft Docs
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 931a45fdbb04d15f3080ee0a2c0546ab6e924b59
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34595721"
---
# <a name="automation-with-service-principals"></a>Automatização com principais de serviço

Principais de serviço são um recurso de aplicação do Azure Active Directory cria no seu inquilino para o recurso autónomo e operações de nível de serviço. Se estiver a um único tipo de *a identidade do utilizador* com um ID de aplicação e a palavra-passe ou o certificado. Um principal de serviço tem apenas essas permissões necessárias para efetuar tarefas definidas pelas funções e permissões para o qual é atribuído. 

No Analysis Services, os principais de serviço são utilizadas com a automatização do Azure, modo automático do PowerShell, aplicações de cliente personalizadas e aplicações web para automatizar tarefas comuns. Por exemplo, para cima servidores aprovisionamento, implementação de modelos, a atualização de dados, a escala/para baixo e Pausa/Retomar podem todos ser automatizada através da utilização de principais de serviço. Permissões são atribuídas a principais de serviço através da associação de função, semelhante a contas do Azure AD UPN regulares.

## <a name="create-service-principals"></a>Criar principais de serviço
 
Principais de serviço podem ser criadas no portal do Azure ou através do PowerShell. Para saber mais, consulte:

[Criar serviço principal - portal do Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Criar principal de serviço - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="store-credential-and-certificate-assets-in-azure-automation"></a>Armazenar elementos de certificado e da credencial na automatização do Azure

Credenciais de principais de serviço e os certificados podem ser armazenados em segurança na automatização do Azure para operações do runbook. Para saber mais, consulte:

[Recursos de credencial na automatização do Azure](../automation/automation-credentials.md)   
[Ativos de certificado na automatização do Azure](../automation/automation-certificates.md)

## <a name="add-service-principals-to-server-admin-role"></a>Adicionar principais de serviço à função de administrador do servidor

Antes de poder utilizar um principal de serviço para operações de gestão do servidor de Analysis Services, tem de o adicionar à função de administradores do servidor. Para obter mais informações, consulte [adicionar um principal de serviço à função de administrador do servidor](analysis-services-addservprinc-admins.md).

## <a name="service-principals-in-connection-strings"></a>Principais de serviço nas cadeias de ligação

AppID principal de serviço e a palavra-passe ou certificado pode ser utilizado em cadeias de ligação muito igual um UPN.

### <a name="powershell"></a>PowerShell

Quando utilizar um principal de serviço para operações de gestão de recursos com o [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) módulo, utilize `Login-AzureRmAccount` cmdlet. Quando utilizar um principal de serviço para operações de servidor com o [SQLServer](https://www.powershellgallery.com/packages/SqlServer) módulo, utilize `Add-AzureAnalysisServicesAccount` cmdlet. 

No exemplo seguinte, o appID e uma palavra-passe são utilizados para efetuar uma operação de atualização de base de dados do modelo:

```PowerShell
Param (

        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force

$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

Add-AzureAnalysisServicesAccount -Credential $Credential -ServicePrincipal -TenantId $TenantId -RolloutEnvironment "westcentralus.asazure.windows.net"

Invoke-ProcessTable -Server "asazure://westcentralus.asazure.windows.net/myserver" -TableName "MyTable" -Database "MyDb" -RefreshType "Full"
```

### <a name="amo-and-adomd"></a>AMO e ADOMD 

Ao ligar-se com aplicações de cliente e as aplicações web, [AMO e ADOMD bibliotecas de cliente](analysis-services-data-providers.md) versão 15.0.2 e pacotes instaláveis superiores do NuGet suportam principais de serviço nas cadeias de ligação utilizando a seguinte sintaxe: `app:AppID` e palavra-passe ou `cert:thumbprint`. 

No exemplo seguinte, `appID` e um `password` são utilizados para efetuar uma operação de atualização de base de dados do modelo:

```C#
string appId = "xxx";
string authKey = "yyy";
string connString = $"Provider=MSOLAP;Data Source=asazure://westus.asazure.windows.net/<servername>;User ID=app:{appId};Password={authKey};";
Server server = new Server();
server.Connect(connString);
Database db = server.Databases.FindByName("adventureworks");
Table tbl = db.Model.Tables.Find("DimDate");
tbl.RequestRefresh(RefreshType.Full);
db.Model.SaveChanges();
```

## <a name="next-steps"></a>Passos Seguintes
[Iniciar sessão com o Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps)   
[Adicionar um principal de serviço à função de administrador do servidor](analysis-services-addservprinc-admins.md)   
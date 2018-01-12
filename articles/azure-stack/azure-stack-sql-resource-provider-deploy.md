---
title: Bases de dados SQL a utilizar na pilha do Azure | Microsoft Docs
description: "Saiba como pode implementar bases de dados do SQL Server como um serviço na pilha do Azure e os passos rápidos para implementar o adaptador de fornecedor de recursos do SQL Server"
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: JeffGo
ms.openlocfilehash: e2f29a1686d0f4baa88b9d37bac0261952f4caa6
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados do SQL Server na pilha do Microsoft Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Utilizar a placa de fornecedor de recursos do SQL Server para expor bases de dados do SQL Server como um serviço de [Azure pilha](azure-stack-poc.md). Depois de instalar o fornecedor de recursos e ligá-lo para um ou mais instâncias do SQL Server, podem criar e os seus utilizadores:
- bases de dados para aplicações em nuvem-nativo
- Web sites baseados em SQL
- cargas de trabalho que se baseiam em SQL Server que não têm de aprovisionar uma máquina virtual (VM) que aloja o servidor de SQL cada vez.

O fornecedor de recursos não suporta todas as capacidades de gestão de base de dados do [SQL Database do Azure](https://azure.microsoft.com/services/sql-database/). Por exemplo, conjuntos de base de dados elástica e a capacidade de aumentar automaticamente o desempenho de base de dados acima e abaixo não estão disponíveis. No entanto, o suporte de does de fornecedor de recursos semelhante criar, ler, atualizar e eliminar as operações (CRUD). A API não é compatível com a base de dados SQL.

## <a name="sql-resource-provider-adapter-architecture"></a>Arquitetura de adaptador de fornecedor de recursos SQL
O fornecedor de recursos é constituído por três componentes:

- **O adaptador de fornecedor de recursos do SQL Server VM**, que é uma máquina virtual do Windows em execução os serviços do fornecedor.
- **O fornecedor de recursos próprio**, que processa os pedidos de aprovisionamento e expõe recursos de base de dados.
- **Servidores que alojam o SQL Server**, que fornecem a capacidade para bases de dados, denominados servidores de alojamento.

Deve criar (ou mais) servidores SQL e/ou fornecer acesso a instâncias externas do SQL Server.

## <a name="deploy-the-resource-provider"></a>Implementar o fornecedor de recursos

1. Se ainda não o tiver feito, registe o kit de desenvolvimento e transferir a imagem do Windows Server 2016 Datacenter Core transferível através da gestão do Marketplace. Tem de utilizar uma imagem do Windows Server 2016 Core. Também pode utilizar um script para criar um [imagem do Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) -Certifique-se selecionar a opção de núcleos. O tempo de execução de .NET 3.5 já não é necessário.

2. A iniciar sessão para um anfitrião que pode aceder a VM de ponto final com privilégios.

    a. Nas instalações do Kit de desenvolvimento de pilha do Azure (ASDK), inicie sessão no anfitrião físico.

    b. Em sistemas com vários nós, o anfitrião tem de ser um sistema que pode aceder ao ponto de final com privilégios. 
    
    >[!NOTE]
    > O sistema onde está a ser executado o script *tem* ser um sistema Windows 10 ou Windows Server 2016 com a versão mais recente do tempo de execução de .NET instalada. A instalação falhará em contrário. O anfitrião ASDK cumpre estes critérios.


3. Transferir o fornecedor de recursos SQL binário e executar o Self-extractor para extrair os conteúdos num diretório temporário.

    >[!NOTE] 
    > Corresponde a compilação de fornecedor de recursos para compilações de pilha do Azure. Tem de transferir o binário correto para a versão da pilha do Azure que está em execução.

    | Compilação de pilha do Azure | Instalador do SQL Server RP |
    | --- | --- |
    |1.0.180102.3 ou 1.0.180106.1 (com vários nós) | [SQL Server RP versão 1.1.14.0](https://aka.ms/azurestacksqlrp1712) |
    | 1.0.171122.1 | [SQL Server RP versão 1.1.12.0](https://aka.ms/azurestacksqlrp1711) |
    | 1.0.171028.1 | [SQL Server RP versão 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
  

4. O certificado de raiz de pilha do Azure é obtido a partir do ponto final com privilégios. Para ASDK, é criado um certificado autoassinado como parte deste processo. Em vários nós, tem de fornecer um certificado adequado.

    Se tiver de fornecer o seu próprio certificado, terá de um ficheiro PFX colocado no **DependencyFilesLocalPath** (ver abaixo) da seguinte forma:

    - Um certificado de caráter universal para \*.dbadapter.\< região\>.\< fqdn externo\> ou um certificado de site único com um nome comum de sqladapter.dbadapter.\< região\>.\< fqdn externo\>
    - Este certificado tem de ser fidedigno, tal como faria ser emitido por uma autoridade de certificação. Ou seja, a cadeia de confiança tem de existir sem exigir certificados intermédios.
    - Existe apenas um ficheiro de certificado único no DependencyFilesLocalPath.
    - O nome de ficheiro não pode conter os caracteres especiais.


5. Abra um **novo** elevada consola do PowerShell (administrativa) e alteração para o diretório onde extraiu os ficheiros. Utilize uma nova janela para evitar problemas que podem surgir na incorretos módulos do PowerShell já carregados no sistema.

6. [Instalar o Azure PowerShell versão 1.2.11](azure-stack-powershell-install.md).

7. Execute o script de DeploySqlProvider.ps1 com os parâmetros indicados abaixo.

O script efetua estes passos:

- Carregar os certificados e outros artefactos para uma conta de armazenamento na pilha do Azure.
- Publica pacotes de galeria que pode implementar as bases de dados do SQL Server através da galeria.
- Publicar um pacote de galeria para implementar servidores de alojamento
- Implementar uma VM com a imagem do Windows Server 2016 criada no passo 1 e instale o fornecedor de recursos.
- Registe um registo DNS local, que mapeia para o fornecedor de recursos VM.
- Registe o fornecedor de recursos com o local do Azure Resource Manager (utilizador e administrador).

> [!NOTE]
> Se a instalação demora mais de 90 minutos, poderá falhar e verá uma mensagem de falha no ecrã e no ficheiro de registo, mas a implementação é repetida do passo falhar. Sistemas que não cumprem as especificações recomendadas de memória e vCPU poderão não conseguir implementar o SQL Server RP.
>

Eis um exemplo que pode executar a partir do PowerShell solicitar (mas alterar as informações de conta e as palavras-passe, conforme necessário):

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack and the default prefix is AzS
# For integrated systems, the domain and the prefix will be the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>Parâmetros de DeploySqlProvider.ps1
Pode especificar estes parâmetros na linha de comandos. Se não o fizer, ou qualquer parâmetro validação falhar, são-lhe pedido fornecer as necessárias.

| Nome do Parâmetro | Descrição | Comentário ou o valor predefinido |
| --- | --- | --- |
| **CloudAdminCredential** | A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios. | _necessário_ |
| **AzCredential** | Forneça as credenciais da conta de administrador de serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure). | _necessário_ |
| **VMLocalCredential** | Defina as credenciais para a conta de administrador local do fornecedor de recursos VM do SQL Server. | _necessário_ |
| **PrivilegedEndpoint** | Forneça o endereço IP ou nome de DNS do ponto final Privleged. |  _necessário_ |
| **DependencyFilesLocalPath** | O ficheiro PFX de certificado tem de ser colocado bem neste diretório. | _opcional_ (_obrigatório_ para vários nós) |
| **DefaultSSLCertificatePassword** | A palavra-passe para o certificado. pfx | _necessário_ |
| **MaxRetryCount** | Defina o número de vezes que pretende repetir a cada operação, se existir uma falha.| 2 |
| **RetryDuration** | Defina o limite de tempo entre tentativas, em segundos. | 120 |
| **Desinstalar** | Remover o fornecedor de recursos e recursos de todos os associados (ver notas abaixo) | Não |
| **DebugMode** | Impede a limpeza automática em caso de falha | Não |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Verificar a implementação através do portal de pilha do Azure

> [!NOTE]
>  Quando o script de instalação estiver concluída, terá de atualizar o portal para ver o painel de administração.


1. Inicie sessão no portal de administração como o administrador de serviço.

2. Certifique-se de que a implementação concluída com êxito. Procurar **grupos de recursos** &gt;, clique em de **system.\< localização\>.sqladapter** recursos de grupo e certifique-se de que todas as implementações de quatro teve êxito.

      ![Verificar a implementação de RP o SQL Server](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Atualizar o adaptador de fornecedor de recursos do SQL Server (com vários nós apenas, compilações 1710 e posteriores)
Sempre que é atualizada a compilação de pilha do Azure, será lançado um novo adaptador do fornecedor de recursos do SQL Server. Enquanto o adaptador existente poderão continuar a funcionar, é recomendado para atualizar para a compilação mais recente logo que possível após a pilha do Azure é atualizada. O processo de atualização é muito semelhante ao processo de instalação descrito acima. Será criada uma nova VM com o código RP mais recente e as definições serão migradas para esta nova instância, incluindo a base de dados e alojamento de informações do servidor, bem como o registo DNS necessário.

Utilize o script de UpdateSQLProvider.ps1 com os mesmos argumentos acima. Também tem de fornecer o certificado aqui.

> [!NOTE]
> A atualização só é suportada em sistemas com vários nós.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack and the default prefix is AzS
# For integrated systems, the domain and the prefix will be the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="updatesqlproviderps1-parameters"></a>Parâmetros de UpdateSQLProvider.ps1
Pode especificar estes parâmetros na linha de comandos. Se não o fizer, ou qualquer parâmetro validação falhar, são-lhe pedido fornecer as necessárias.

| Nome do Parâmetro | Descrição | Comentário ou o valor predefinido |
| --- | --- | --- |
| **CloudAdminCredential** | A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios. | _necessário_ |
| **AzCredential** | Forneça as credenciais da conta de administrador de serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure). | _necessário_ |
| **VMLocalCredential** | Defina as credenciais para a conta de administrador local do fornecedor de recursos VM do SQL Server. | _necessário_ |
| **PrivilegedEndpoint** | Forneça o endereço IP ou nome de DNS do ponto final Privleged. |  _necessário_ |
| **DependencyFilesLocalPath** | O ficheiro PFX de certificado tem de ser colocado bem neste diretório. | _opcional_ (_obrigatório_ para vários nós) |
| **DefaultSSLCertificatePassword** | A palavra-passe para o certificado. pfx | _necessário_ |
| **MaxRetryCount** | Defina o número de vezes que pretende repetir a cada operação, se existir uma falha.| 2 |
| **RetryDuration** | Defina o limite de tempo entre tentativas, em segundos. | 120 |
| **Desinstalar** | Remover o fornecedor de recursos e recursos de todos os associados (ver notas abaixo) | Não |
| **DebugMode** | Impede a limpeza automática em caso de falha | Não |



## <a name="remove-the-sql-resource-provider-adapter"></a>Remova o adaptador de fornecedor de recursos SQL

Para remover o fornecedor de recursos, é essencial primeiro de remover as dependências.

1. Certifique-se de que tem o pacote de implementação original que transferiu para esta versão do adaptador de fornecedor de recursos do SQL.

2. Devem ser apagadas todas as bases de dados de utilizador do fornecedor de recursos (não elimina os dados). Isto deve ser efetuado pelos utilizadores pessoalmente.

3. Administrador terá de eliminar os servidores de alojamento do adaptador de fornecedor de recursos SQL

4. Administrador tem de eliminar quaisquer planos que referenciam o adaptador de fornecedor de recursos do SQL Server.

5. Administrador tem de eliminar todos os SKUs e quotas associadas ao adaptador de fornecedor de recursos do SQL Server.

6. Volte a executar o script de implementação-desinstalar parâmetro, pontos finais do Azure Resource Manager, DirectoryTenantID e as credenciais da conta de administrador de serviço.


## <a name="next-steps"></a>Passos Seguintes

[Adicionar servidores que alojam](azure-stack-sql-resource-provider-hosting-servers.md) e [criar bases de dados](azure-stack-sql-resource-provider-databases.md).

Tente outro [PaaS serviços](azure-stack-tools-paas-services.md) como o [fornecedor de recursos do servidor de MySQL](azure-stack-mysql-resource-provider-deploy.md) e [fornecedor de recursos de serviços aplicacionais](azure-stack-app-service-overview.md).

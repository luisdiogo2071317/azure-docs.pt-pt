---
title: Bases de dados SQL a utilizar na pilha do Azure | Microsoft Docs
description: "Saiba como pode implementar bases de dados do SQL Server como um serviço na pilha do Azure e os passos rápidos para implementar o adaptador de fornecedor de recursos do SQL Server."
services: azure-stack
documentationCenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: mabrigg
ms.reviewer: jeffgo
ms.openlocfilehash: 4d2a00f04e5b07aeb3585fb3ab6c8966e0de7e19
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados do SQL Server na pilha do Microsoft Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Utilizar a placa de fornecedor de recursos do SQL Server para expor bases de dados do SQL Server como um serviço de [Azure pilha](azure-stack-poc.md). Depois de instalar o fornecedor de recursos e ligá-lo para um ou mais instâncias do SQL Server, podem criar e os seus utilizadores:
- Bases de dados para aplicações em nuvem nativo.
- Web sites baseados em SQL.
- Cargas de trabalho que se baseiam em SQL.
Não tem de aprovisionar uma máquina virtual (VM) que aloja o servidor de SQL cada vez.

O fornecedor de recursos não suporta todas as capacidades de gestão de base de dados do [SQL Database do Azure](https://azure.microsoft.com/services/sql-database/). Por exemplo, conjuntos de base de dados elástica e a capacidade de aumentar e reduzir verticalmente o desempenho de base de dados automaticamente não estão disponíveis. No entanto, o suporte de does de fornecedor de recursos semelhante criar, ler, atualizar e eliminar as operações (CRUD). A API não é compatível com a base de dados SQL.

## <a name="sql-resource-provider-adapter-architecture"></a>Arquitetura de adaptador do fornecedor de recursos do SQL Server
O fornecedor de recursos é composto por três componentes:

- **O adaptador de fornecedor de recursos do SQL Server VM**, que é uma máquina virtual do Windows que executa os serviços do fornecedor.
- **O fornecedor de recursos próprio**, que processa os pedidos de aprovisionamento e expõe recursos de base de dados.
- **Servidores que alojam o SQL Server**, que fornecem a capacidade para bases de dados denominados servidores de alojamento.

Deve criar uma (ou mais) instâncias do SQL Server e/ou fornecer acesso a instâncias externas do SQL Server.

> [!NOTE]
> Alojamento de servidores que estejam instalados na pilha do Azure integrados sistemas tem de ser criados de uma subscrição de inquilino. Estes não podem ser criados da subscrição de fornecedor predefinido. Deve ser criadas no portal de inquilinos ou de uma sessão do PowerShell com um adequado início de sessão. Todos os servidores de alojamento são cobráveis VMs e tem de ter licenças adequadas. O administrador de serviço pode ser o proprietário da subscrição de inquilino.

## <a name="deploy-the-resource-provider"></a>Implementar o fornecedor de recursos

1. Se ainda não o tiver feito, registe o kit de desenvolvimento e transferir a imagem do Windows Server 2016 Datacenter Core transferível através da gestão do Marketplace. Tem de utilizar uma imagem do Windows Server 2016 Core. Também pode utilizar um script para criar um [imagem do Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Lembre-se de que seleciona a opção de núcleos.)

2. A iniciar sessão para um anfitrião que pode aceder ao ponto final com privilégios VM.

    - Nas instalações do Kit de desenvolvimento de pilha do Azure, inicie sessão no anfitrião físico.

    - Em sistemas com vários nós, o anfitrião tem de ser um sistema que pode aceder ao ponto de final com privilégios.
    
    >[!NOTE]
    > O sistema onde está a ser executado o script *tem* ser um sistema Windows 10 ou Windows Server 2016 com a versão mais recente do tempo de execução de .NET instalada. Caso contrário, a instalação falha. O anfitrião do Azure SDK de pilha cumpre neste critério.


3. Transfira o fornecedor de recursos SQL binário. Em seguida, execute o Self-extractor para extrair os conteúdos num diretório temporário.

    >[!NOTE] 
    > O fornecedor de recursos tem uma pilha do Azure correspondente mínimo, compilação. Não se esqueça de transferir o binário correto para a versão da pilha do Azure que está em execução.

    | Compilação de pilha do Azure | Instalador de fornecedor de recursos SQL |
    | --- | --- |
    | 1802: 1.0.180302.1 | [SQL Server RP versão 1.1.18.0](https://aka.ms/azurestacksqlrp1802) |
    | 1712: 1.0.180102.3, 1.0.180103.2 ou 1.0.180106.1 (com vários nós) | [SQL Server RP versão 1.1.14.0](https://aka.ms/azurestacksqlrp1712) |
    | 1711: 1.0.171122.1 | [SQL Server RP versão 1.1.12.0](https://aka.ms/azurestacksqlrp1711) |
    | 1710: 1.0.171028.1 | [SQL Server RP versão 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
  

4. O certificado de raiz de pilha do Azure é obtido a partir do ponto final com privilégios. Para o SDK de pilha do Azure, é criado um certificado autoassinado como parte deste processo. Para sistemas integrados, tem de fornecer um certificado adequado.

   Para fornecer o seu próprio certificado, colocar um ficheiro. pfx a **DependencyFilesLocalPath** da seguinte forma:

    - Um certificado de caráter universal para \*.dbadapter.\< região\>.\< fqdn externo\> ou um certificado de site único com um nome comum de sqladapter.dbadapter.\< região\>.\< fqdn externo\>.

    - Este certificado tem de ser fidedigno. Ou seja, a cadeia de confiança tem de existir sem exigir certificados intermédios.

    - Pode existir apenas um ficheiro de certificado único no diretório indicado pelo parâmetro DependencyFilesLocalPath.

    - O nome de ficheiro não pode conter os caracteres especiais.


5. Abra um **novo** elevada consola do PowerShell (administrativa) e alteração para o diretório onde extraiu os ficheiros. Utilize uma nova janela para evitar problemas que possam surgir de incorretos módulos do PowerShell que já estão carregados no sistema.

6. [Instalar o Azure PowerShell versão 1.2.11](azure-stack-powershell-install.md).

7. Execute o script de DeploySqlProvider.ps1 que efetua estes passos:

    - Carrega os certificados e outros artefactos para uma conta de armazenamento na pilha do Azure.
    - Publica os pacotes de galeria que pode implementar as bases de dados do SQL Server através da galeria.
    - Publica um pacote de galeria para implementar servidores de alojamento.
    - Implementa uma VM utilizando a imagem do Windows Server 2016 que foi criada no passo 1 e, em seguida, instala o fornecedor de recursos.
    - Regista um registo DNS local, que mapeia para o fornecedor de recursos VM.
    - Regista o fornecedor de recursos com o local do Azure Resource Manager (utilizador e administrador).
    - Opcionalmente, instala uma única atualização do Windows durante a instalação de RP

8. Recomendamos que transfira a imagem do Windows Server 2016 Core mais recente da gestão do Marketplace. Se precisar de instalar uma atualização, é possível colocar um único. Pacote MSU no caminho local da dependência. Se mais do que um. Ficheiro MSU for encontrado, o script irá falhar.


Eis um exemplo que pode executar a partir da linha de comandos do PowerShell. (Lembre-se de que alterar as informações de conta e as palavras-passe conforme necessário.)

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parameters
Pode especificar estes parâmetros na linha de comandos. Se não o fizer, ou se falhar qualquer validação dos parâmetros, lhe for pedido para fornecer os parâmetros necessários.

| Nome do parâmetro | Descrição | Comentário ou o valor predefinido |
| --- | --- | --- |
| **CloudAdminCredential** | A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios. | _Necessário_ |
| **AzCredential** | As credenciais para a conta de administrador do serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure. | _Necessário_ |
| **VMLocalCredential** | As credenciais da conta de administrador local do fornecedor de recursos VM do SQL Server. | _Necessário_ |
| **PrivilegedEndpoint** | O endereço IP ou nome DNS do ponto final com privilégios. |  _Necessário_ |
| **DependencyFilesLocalPath** | O ficheiro de. pfx do certificado deve ser colocado neste diretório bem. | _Opcional_ (_obrigatório_ para vários nós) |
| **DefaultSSLCertificatePassword** | A palavra-passe para o certificado. pfx. | _Necessário_ |
| **MaxRetryCount** | O número de vezes que pretende repetir a cada operação, se ocorrer uma falha.| 2 |
| **RetryDuration** | O intervalo de tempo limite entre tentativas, em segundos. | 120 |
| **Desinstalar** | Remove o fornecedor de recursos e recursos de todos os associados (consulte as seguintes notas). | Não |
| **DebugMode** | Impede a limpeza automática em caso de falha. | Não |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Verificar a implementação através do portal de pilha do Azure

> [!NOTE]
>  Após o script de instalação em execução, terá de atualizar o portal para ver o painel de administração.


1. Inicie sessão no portal de administração como o administrador de serviço.

2. Certifique-se de que a implementação concluída com êxito. Aceda a **grupos de recursos**. Em seguida, selecione o **system.\< localização\>.sqladapter** grupo de recursos. Certifique-se de que todas as implementações de quatro teve êxito.

      ![Verificar a implementação do fornecedor de recursos SQL](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Atualizar o adaptador de fornecedor de recursos do SQL Server (com vários nós apenas, compilações 1710 e posteriores)
Um novo adaptador do fornecedor de recursos do SQL Server poderá ser lançado quando compilações de pilha do Azure são atualizadas. Enquanto o adaptador existente continua a funcionar, recomendamos que a atualização para a compilação mais recente logo que possível. Atualizações tem de ser instaladas por ordem: não é possível ignorar versões (consulte a tabela no passo 3 do [implementar o fornecedor de recursos](#deploy-the-resource-provider)).

Para atualizar do fornecedor de recursos que utiliza o *UpdateSQLProvider.ps1* script. O processo é semelhante ao processo utilizado para instalar um fornecedor de recursos, conforme descrito no [implementar o fornecedor de recursos](#deploy-the-resource-provider) secção deste artigo. O script está incluído com a transferência do fornecedor de recursos.

O *UpdateSQLProvider.ps1* script cria uma nova VM com o código mais recente do fornecedor de recursos e migra as definições da VM antiga para a nova VM. As definições de migração incluem a base de dados e as informações do servidor de alojamento e o necessário registo DNS.

O script requer a utilização dos mesmos argumentos que são descritos para o script de DeploySqlProvider.ps1. Forneça o certificado aqui, bem como. 

Recomendamos que transfira a imagem do Windows Server 2016 Core mais recente da gestão do Marketplace. Se precisar de instalar uma atualização, é possível colocar um único. Pacote MSU no caminho local da dependência. Se mais do que um. Ficheiro MSU for encontrado, o script irá falhar.

Segue-se um exemplo do *UpdateSQLProvider.ps1* script que pode executar a partir da linha de comandos do PowerShell. É necessário alterar as palavras-passe conforme necessário e informações da conta: 

> [!NOTE]
> O processo de atualização aplica-se apenas a sistemas integrados.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="updatesqlproviderps1-parameters"></a>Parâmetros de UpdateSQLProvider.ps1
Pode especificar estes parâmetros na linha de comandos. Se não o fizer, ou se falhar qualquer validação dos parâmetros, lhe for pedido para fornecer os parâmetros necessários.

| Nome do parâmetro | Descrição | Comentário ou o valor predefinido |
| --- | --- | --- |
| **CloudAdminCredential** | A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios. | _Necessário_ |
| **AzCredential** | As credenciais para a conta de administrador do serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure. | _Necessário_ |
| **VMLocalCredential** | As credenciais da conta de administrador local do fornecedor de recursos VM do SQL Server. | _Necessário_ |
| **PrivilegedEndpoint** | O endereço IP ou nome DNS do ponto final com privilégios. |  _Necessário_ |
| **DependencyFilesLocalPath** | O ficheiro de. pfx do certificado deve ser colocado neste diretório bem. | _Opcional_ (_obrigatório_ para vários nós) |
| **DefaultSSLCertificatePassword** | A palavra-passe para o certificado. pfx. | _required_ |
| **MaxRetryCount** | O número de vezes que pretende repetir a cada operação, se ocorrer uma falha.| 2 |
| **RetryDuration** |O intervalo de tempo limite entre tentativas, em segundos. | 120 |
| **Desinstalar** | Remove o fornecedor de recursos e recursos de todos os associados (consulte as seguintes notas). | Não |
| **DebugMode** | Impede a limpeza automática em caso de falha. | Não |


## <a name="collect-diagnostic-logs"></a>Recolher registos de diagnóstico
O fornecedor de recursos do SQL Server é um bloqueado para baixo de máquina virtual. Se for necessário recolher registos de máquina virtual, um ponto final de PowerShell apenas suficiente administração (JEA) _DBAdapterDiagnostics_ é fornecido para o efeito. Existem dois comandos estão disponíveis através deste ponto final:

* Get-AzsDBAdapterLog - prepara um pacote zip que contém os registos de diagnóstico RP e coloca-o na unidade de utilizador de sessão. O comando pode ser chamado sem parâmetros e irá recolher as últimos quatro horas de registos.
* Remove-AzsDBAdapterLog - limpa os pacotes de registo existentes no fornecedor de recursos VM

Uma conta de utilizador denominada _dbadapterdiag_ é criada durante a implementação de RP ou atualização para ligar ao ponto final de diagnóstico para extrair os registos RP. A palavra-passe desta conta é o mesmo que a palavra-passe fornecida para a conta de administrador local durante a implementação/atualização.

Para utilizar estes comandos, terá de criar uma sessão remota do PowerShell para a máquina virtual do fornecedor de recursos e invocar o comando. Opcionalmente, pode fornecer parâmetros FromDate e ToDate. Se não especificar um ou ambos, a FromDate quatro horas antes da hora atual, e o ToDate será a hora atual.

Este script de exemplo demonstra a utilização destes comandos:

```
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<see above>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, $diagnosticsUserPassword)
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds `
        -ConfigurationName DBAdapterDiagnostics

# Sample captures logs from the previous one hour
$fromDate = (Get-Date).AddHours(-1)
$dateNow = Get-Date
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2}
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow

# Copy the logs
$sourcePath = "User:\{0}" -f $logs
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage

# Cleanup logs
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove- AzsDBAdapterLog }
# Close the session
$session | Remove-PSSession
```

## <a name="maintenance-operations-integrated-systems"></a>Operações de manutenção (sistemas integradas)
O fornecedor de recursos do SQL Server é um bloqueado para baixo de máquina virtual. A atualização de segurança de recursos fornecedor da máquina virtual pode ser feita o ponto final do PowerShell apenas suficiente administração (JEA) _DBAdapterMaintenance_.

Um script é fornecido com o pacote de instalação no RP para facilitar a estas operações.

### <a name="update-the-virtual-machine-operating-system"></a>Atualize o sistema operativo da máquina virtual
Existem várias formas de atualizar a VM do Windows Server:
* Instalar o pacote mais recente do fornecedor de recursos utilizando uma imagem do Windows Server 2016 Core atualmente patched
* Instalar um pacote do Windows Update durante a instalação ou atualização no RP


### <a name="update-the-virtual-machine-windows-defender-definitions"></a>Atualizar as definições do Windows Defender de máquina virtual

Siga estes passos para atualizar as definições de Defender:

1. As definições do Windows Defender update a partir de transferência [definição do Windows Defender](https://www.microsoft.com/en-us/wdsi/definitions)

    Nessa página, em "Manualmente, transfira e instale as definições" transferir "Windows Defender antivírus do Windows 10 e Windows 8.1" ficheiro de 64 bits. 
    
    Ligação direta: https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64

2. Criar uma sessão do PowerShell para o ponto final de manutenção do SQL Server RP adaptador da máquina virtual
3. Copie o ficheiro de atualização de definições para a máquina de placa de base de dados utilizando a sessão de ponto final de manutenção
4. Sobre a manutenção do PowerShell sessão invocar o _atualização DBAdapterWindowsDefenderDefinitions_ comando
5. Após a instalação, é recomendado para remover o ficheiro de atualização de definições utilizadas. Podem ser removido a sessão de manutenção utilizando o _remover ItemOnUserDrive)_ comando.


Eis um exemplo de script para atualizar as definições de Defender (substituir o endereço ou o nome da máquina virtual com o valor real):

```
# Set credentials for the diagnostic user
$diagPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$diagCreds = New-Object System.Management.Automation.PSCredential `
    ("dbadapterdiag", $vmLocalAdminPass)$diagCreds = Get-Credential

# Public IP Address of the DB adapter machine
$databaseRPMachine  = "XX.XX.XX.XX"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"
 
# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions. 
Invoke-WebRequest -Uri https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64 `
    -Outfile $localPathToDefenderUpdate 

# Create session to the maintenance endpoint
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $diagCreds -ConfigurationName DBAdapterMaintenance
# Copy defender update file to the db adapter machine
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\mpam-fe.exe"
# Install the update file
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinitions -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}
# Cleanup the definitions package file and session
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession
```

## <a name="remove-the-sql-resource-provider-adapter"></a>Remova o adaptador de fornecedor de recursos SQL

Para remover o fornecedor de recursos, é essencial primeiro de remover as dependências.

1. Certifique-se de que tem o pacote de implementação original que transferiu para esta versão do adaptador de fornecedor de recursos do SQL Server.

2. Devem ser apagadas todas as bases de dados de utilizador do fornecedor de recursos. (A eliminar as bases de dados do utilizador não elimina os dados.) Esta tarefa deve ser efetuada pelos utilizadores pessoalmente.

3. O administrador terá de eliminar os servidores de alojamento do adaptador de fornecedor de recursos do SQL Server.

4. O administrador tem de eliminar quaisquer planos que referenciam o adaptador de fornecedor de recursos do SQL Server.

5. O administrador tem de eliminar todos os SKUs e quotas que estão associadas a placa de fornecedor de recursos do SQL Server.

6. Volte a executar o script de implementação com os seguintes elementos:
    - -Desinstalar parâmetro
    - Os pontos finais do Azure Resource Manager
    - O DirectoryTenantID
    - As credenciais da conta de administrador de serviço


## <a name="next-steps"></a>Passos Seguintes

[Adicionar servidores de alojamento](azure-stack-sql-resource-provider-hosting-servers.md) e [criar bases de dados](azure-stack-sql-resource-provider-databases.md).

Tente outro [PaaS serviços](azure-stack-tools-paas-services.md) como o [fornecedor de recursos do servidor de MySQL](azure-stack-mysql-resource-provider-deploy.md) e [fornecedor de recursos de serviços aplicacionais](azure-stack-app-service-overview.md).

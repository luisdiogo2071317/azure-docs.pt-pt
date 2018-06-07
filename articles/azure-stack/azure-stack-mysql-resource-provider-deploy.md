---
title: Utilizar bases de dados MySQL como PaaS na pilha do Azure | Microsoft Docs
description: Saiba como pode implementar o fornecedor de recursos de MySQL e fornecer bases de dados MySQL como um serviço na pilha do Azure.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: ef4d34870cce7d2a149b2592e341956419272c92
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604204"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados MySQL na pilha do Microsoft Azure 
Pode implementar um fornecedor de recursos de MySQL na pilha do Azure. Depois de implementar o fornecedor de recursos, pode criar servidores MySQL e bases de dados através de modelos de implementação Azure Resource Manager. Também pode fornecer bases de dados MySQL como um serviço.  

Bases de dados MySQL, que são comuns em web sites, suportam várias plataformas de Web site. Por exemplo, depois de implementar o fornecedor de recursos, pode criar sites WordPress da plataforma de aplicações Web como um suplemento de serviço (PaaS) para a pilha do Azure. 
 
Para implementar o fornecedor de MySQL num sistema que não tem acesso à Internet, copie o ficheiro [mysql conector-net 6.10.5.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) para uma partilha local. Em seguida, fornece esse nome de partilha quando lhe for pedido para o mesmo. Tem de instalar os módulos do Azure e o Azure PowerShell da pilha. 

## <a name="mysql-server-resource-provider-adapter-architecture"></a>Arquitetura de adaptador do fornecedor de recursos do servidor de MySQL 
O fornecedor de recursos é constituído por três componentes: 
- **O adaptador de fornecedor de recursos de MySQL VM**, que é uma máquina virtual do Windows que está a executar os serviços do fornecedor. 
- **O fornecedor de recursos próprio**, que processa os pedidos de aprovisionamento e expõe recursos de base de dados. 
- **Servidores que alojam o servidor de MySQL**, que fornecem a capacidade para bases de dados que são denominados servidores de alojamento. Terá de criar instâncias de MySQL por si ou fornecer acesso a instâncias externas do SQL Server. Visite o [Galeria de início rápido do Azure pilha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) para um modelo de exemplo que podem: 
    - Crie um servidor de MySQL para si. 
    - Transferir e implementar um servidor de MySQL do Azure Marketplace. 


> [!NOTE] 
> Alojamento de servidores que estejam instalados na pilha do Azure integrados sistemas tem de ser criados de uma subscrição de inquilino. Estes não podem ser criados da subscrição de fornecedor predefinido. Deve ser criadas no portal de inquilinos ou de uma sessão do PowerShell com um adequado início de sessão. Todos os servidores de alojamento são cobráveis VMs e tem de ter licenças adequadas. O administrador de serviço pode ser o proprietário da subscrição de inquilino. 

### <a name="required-privileges"></a>Privilégios necessários 
A conta do sistema tem de ter os seguintes privilégios: 
- Base de dados: Criar, remover 
- Início de sessão: Criar, definir, remover, conceder, revogar 
 
## <a name="deploy-the-resource-provider"></a>Implementar o fornecedor de recursos 
1. Se ainda não o tiver feito, registe o kit de desenvolvimento e transferir a imagem do Windows Server 2016 Datacenter Core transferível através da gestão do Marketplace. Tem de utilizar uma imagem do Windows Server 2016 Core. Também pode utilizar um script para criar um [imagem do Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Lembre-se de que seleciona a opção de núcleos.) 

2. A iniciar sessão para um anfitrião que pode aceder ao ponto final com privilégios VM:
    - Nas instalações do SDK do Azure, inicie sessão no anfitrião físico.  
    - Em sistemas integrados, o anfitrião tem de ser um sistema que pode aceder ao ponto de final com privilégios. 
    >[!NOTE] 
    > O sistema em que o script está a ser executado *tem* ser um sistema Windows 10 ou Windows Server 2016 com a versão mais recente do tempo de execução de .NET instalada. Caso contrário, a instalação falha. O anfitrião do Azure pilha ASDK cumpre neste critério. 
3. Transfira o fornecedor de recursos de MySQL binário. Em seguida, execute o Self-extractor para extrair os conteúdos num diretório temporário. 
    >[!NOTE]  
    > O fornecedor de recursos tem uma pilha do Azure correspondente mínimo, compilação. Não se esqueça de transferir o binário correto para a versão da pilha do Azure que está em execução.

    | Versão de pilha do Azure | Versão MySQL RP| 
    | --- | --- | 
    | Versão 1804 (1.0.180513.1)|[MySQL RP versão 1.1.24.0](https://aka.ms/azurestackmysqlrp1804) | 
    | Versão 1802 (1.0.180302.1) | [MySQL RP versão 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) | 
    | Versão 1712 (1.0.180102.3 ou 1.0.180106.1 (sistemas integrados)) | [MySQL RP versão 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) | 

4.  Para ASDK, é criado um certificado autoassinado como parte deste processo. Para sistemas integrados, tem de fornecer um certificado adequado. Se tiver de fornecer o seu próprio certificado, colocar um ficheiro. pfx a **DependencyFilesLocalPath** que cumpra os seguintes critérios: 
    - Um certificado de caráter universal para \*.dbadapter.\< região\>.\< fqdn externo\> ou um certificado de site único com um nome comum de mysqladapter.dbadapter.\< região\>.\< fqdn externo\>. 
    - Este certificado tem de ser fidedigno. Ou seja, a cadeia de confiança tem de existir sem exigir certificados intermédios. 
    - Existe apenas um ficheiro de certificado único no DependencyFilesLocalPath. 
    - O nome de ficheiro não pode conter quaisquer carateres especiais ou espaços. 

5. Abra um **novo** elevada consola do PowerShell (administrativa). Em seguida, altere o diretório onde extraiu os ficheiros. Utilize uma nova janela para evitar problemas que possam surgir de incorretos módulos do PowerShell que já estão carregados no sistema. 

6. Execute o **DeployMySqlProvider.ps1** script. O script efetua estes passos: 
    - Transfere o binário de conector MySQL (Isto pode ser fornecido offline). 
    - Carrega os certificados e outros artefactos para uma conta de armazenamento na pilha do Azure. 
    - Publica os pacotes de galeria que pode implementar as bases de dados do SQL Server através da galeria. 
    - Publica um pacote de galeria para implementar servidores de alojamento. 
    - Implementa uma VM usin uma imagem do marketplace pilha do Windows Server 2016 do Azure e instala o fornecedor de recursos. 
    - Regista um registo DNS local, que mapeia para o fornecedor de recursos VM. 
    - Regista o fornecedor de recursos com o local do Azure Resource Manager (inquilino e administrador). 

    Pode especificar os parâmetros necessários na linha de comandos ou execute o script sem quaisquer parâmetros e, em seguida, introduza-las quando lhe for pedido. 

    Eis um exemplo que pode executar a partir da linha de comandos do PowerShell. É necessário alterar as palavras-passe conforme necessário e informações da conta: 

    ```powershell 
    # Install the AzureRM.Bootstrapper module and set the profile. 
    Install-Module -Name AzureRm.BootStrapper -Force 
    Use-AzureRmProfile -Profile 2017-03-09-profile 

    # Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time. 
    $domain = "AzureStack"  

    # For integrated systems, use the IP address of one of the ERCS virtual machines 
    $privilegedEndpoint = "AzS-ERCS01" 

    # Point to the directory where the resource provider installation files were extracted. 
    $tempDir = 'C:\TEMP\MYSQLRP' 

    # The service admin account (can be Azure Active Directory or Active Directory Federation Services). 
    $serviceAdmin = "admin@mydomain.onmicrosoft.com" 
    $AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
    $AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass) 

    # Set the credentials for the new resource provider VM local administrator account 
    $vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
    $vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass) 

    # And the cloudadmin credential required for privileged endpoint access. 
    $CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
    $CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass) 

    # Change the following as appropriate. 
    $PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 

    # Run the installation script from the folder where you extracted the installation files. 
    # Find the ERCS01 IP address first, and make sure the certificate 
    # file is in the specified directory. 
    $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds ` 
    VMLocalCredential $vmLocalAdminCreds ` 
    CloudAdminCredential $cloudAdminCreds ` 
    PrivilegedEndpoint $privilegedEndpoint ` 
    DefaultSSLCertificatePassword $PfxPass ` 
    DependencyFilesLocalPath $tempDir\cert ` 
    AcceptLicense 
    ``` 

### <a name="deploymysqlproviderps1-parameters"></a>Parâmetros de DeployMySqlProvider.ps1 
Pode especificar estes parâmetros na linha de comandos. Se não o fizer, ou se falhar qualquer validação dos parâmetros, lhe for pedido para fornecer os parâmetros necessários. 

| Nome do parâmetro | Descrição | Comentário ou o valor predefinido | 
| --- | --- | --- | 
| **CloudAdminCredential** | A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios. | _Necessário_ | 
| **AzCredential** | As credenciais para a conta de administrador do serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure. | _Necessário_ | 
| **VMLocalCredential** | As credenciais para a conta de administrador local do fornecedor de recursos de MySQL VM. | _Necessário_ | 
| **PrivilegedEndpoint** | O endereço IP ou nome DNS do ponto final com privilégios. |  _Necessário_ | 
| **DependencyFilesLocalPath** | Caminho para uma partilha local que contém [mysql conector-net 6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi). Se fornecer um destes caminhos, o ficheiro de certificado tem de ser colocado bem neste diretório. | _Opcional_ (_obrigatório_ para vários nós) | 
| **DefaultSSLCertificatePassword** | A palavra-passe para o certificado. pfx. | _Necessário_ | 
| **MaxRetryCount** | O número de vezes que pretende repetir a cada operação, se ocorrer uma falha.| 2 | 
| **RetryDuration** | O intervalo de tempo limite entre tentativas, em segundos. | 120 | 
| **Desinstalar** | Remove o fornecedor de recursos e recursos de todos os associados (consulte as seguintes notas). | Não | 
| **DebugMode** | Impede a limpeza automática em caso de falha. | Não | 
| **AcceptLicense** | Ignora o pedido para aceitar o GPL de licenciamento.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | | 

> [!NOTE] 
> SKUs podem demorar até uma hora ser visível no portal. Não é possível criar uma base de dados até que seja criada a SKU. 

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Verificar a implementação através do portal de pilha do Azure 
> [!NOTE] 
>  Após o script de instalação em execução, terá de atualizar o portal para ver o painel de administração. 

1. Inicie sessão no portal de administração como o administrador de serviço. 
2. Certifique-se de que a implementação concluída com êxito. Aceda a **grupos de recursos**e, em seguida, selecione o **system.\< localização\>.mysqladapter** grupo de recursos. Certifique-se de que todas as implementações de quatro teve êxito:

      ![Verificar a implementação de RP o MySQL](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png) 

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Fornecem a capacidade de ligar a um servidor de alojamento MySQL 
1. Inicie sessão no portal do Azure pilha como um administrador de serviço. 
2. Selecione **recursos administrativos** > **MySQL servidores de alojamento** > **+ adicionar**. No **servidores de alojamento MySQL** painel, pode ligar o fornecedor de recursos do servidor de MySQL para instâncias reais do servidor de MySQL que servem de back-end o fornecedor de recursos. 

![Servidores de alojamento](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png) 

3. Forneça os detalhes de ligação da sua instância do servidor de MySQL. Lembre-se de que forneça o nome de domínio completamente qualificado (FQDN) ou um endereço IPv4 válido e não o nome abreviado de VM. Esta instalação já não fornece uma instância de MySQL predefinida. O tamanho que é fornecido ajuda-o o gerir a capacidade de base de dados do fornecedor de recursos. Deve estar perto a capacidade do servidor da base de dados física. 

    > [!NOTE] 
    > Se a instância de MySQL pode ser acedida pelo inquilino e administrador do Azure Resource Manager, pode ser colocado sob o controlo do fornecedor de recursos. A instância de MySQL *tem* atribuída exclusivamente para o fornecedor de recursos. 

4. Como adicionar servidores, tem de atribuir-lhes um SKU de novo ou existente para permitir a diferenciação de ofertas de serviço. Por exemplo, pode ter uma instância de enterprise que fornecer: 
    - capacidade de base de dados
    - Cópia de segurança automática
    - servidores de elevado desempenho para departamentos individuais de reserva 

    > [!IMPORTANT] 
    > Não é possível misturar servidores autónomos com Always On instâncias o SKU do mesmo. A tentar combinar tipos após a adição de resultados do servidor de alojamento primeiro num erro. 

    O nome do SKU deve refletir as propriedades, para que os inquilinos podem colocar as bases de dados adequadamente. Todos os servidores de alojamento de um SKU devem ter as mesmas capacidades. 

    ![Criar um SKU de MySQL](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png) 

## <a name="test-your-deployment-by-creating-your-first-mysql-database"></a>Testar a implementação ao criar a sua primeira base de dados MySQL 
1. Inicie sessão no portal do Azure pilha como um administrador de serviço. 
2. Selecione **+ nova** > **dados + armazenamento** > **base de dados MySQL**. 
3. Forneça os detalhes da base de dados. 

    ![Criar um teste de base de dados MySQL](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)
 
4. Selecione um SKU. 

    ![Selecione um SKU](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png) 

5. Crie uma definição de início de sessão. Pode reutilizar uma definição de início de sessão existente ou crie um novo. Esta definição contém o nome de utilizador e palavra-passe para a base de dados. 

    ![Criar um novo início de sessão de base de dados](./media/azure-stack-mysql-rp-deploy/create-new-login.png) 

    A cadeia de ligações inclui o nome do servidor de base de dados real. Copie-a partir do portal. 

    ![Obter a cadeia de ligação para a base de dados MySQL](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png) 

    > [!NOTE] 
    > O comprimento dos nomes de utilizador não pode exceder 32 carateres no MySQL 5.7. Nas edições anteriores, este não pode exceder os 16 carateres. 

## <a name="add-capacity"></a>Adicionar capacidade 
Adicione capacidade adicionando servidores adicionais do MySQL no portal do Azure pilha. Servidores adicionais podem ser adicionados a um SKU de novo ou existente. Certifique-se que as características de servidor são os mesmos. 
 
## <a name="make-mysql-databases-available-to-tenants"></a>Disponibilizar bases de dados MySQL aos inquilinos 
Crie planos e as ofertas para disponibilizar as bases de dados MySQL para os inquilinos. Por exemplo, adicione o serviço de Microsoft.MySqlAdapter, adicione uma quota e assim sucessivamente. 

![Criar planos e as ofertas para incluir as bases de dados](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png) 

## <a name="update-the-administrative-password"></a>Atualizar a palavra-passe administrativa 
Pode modificar a palavra-passe pelo primeiro alterá-lo na instância de servidor MySQL. Selecione **recursos administrativos** > **MySQL servidores de alojamento**. Em seguida, selecione o servidor de alojamento. No **definições** painel, selecione **palavra-passe**. 

![Atualizar a palavra-passe de administrador](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png) 

## <a name="update-the-mysql-resource-provider-adapter-integrated-systems-only"></a>Atualizar o adaptador de fornecedor de recursos de MySQL (apenas a sistemas integrados)
Um novo adaptador do fornecedor de recursos do SQL Server poderá ser lançado quando compilações de pilha do Azure são atualizadas. Enquanto o adaptador existente continua a funcionar, recomendamos que a atualização para a compilação mais recente logo que possível.  
 
Para atualizar do fornecedor de recursos que utiliza o **UpdateMySQLProvider.ps1** script. O processo é semelhante ao processo utilizado para instalar um fornecedor de recursos, conforme descrito no [implementar o fornecedor de recursos](#deploy-the-resource-provider) secção deste artigo. O script está incluído com a transferência do fornecedor de recursos. 

O **UpdateMySQLProvider.ps1** script cria uma nova VM com o código mais recente do fornecedor de recursos e migra as definições da VM antiga para a nova VM. As definições de migração incluem a base de dados e as informações do servidor de alojamento e o necessário registo DNS. 

O script requer a utilização dos mesmos argumentos que são descritos para o script de DeployMySqlProvider.ps1. Forneça o certificado aqui, bem como.  

Segue-se um exemplo do *UpdateMySQLProvider.ps1* script que pode executar a partir da linha de comandos do PowerShell. É necessário alterar as palavras-passe conforme necessário e informações da conta:  

> [!NOTE] 
> O processo de atualização aplica-se apenas a sistemas integrados. 

```powershell 
# Install the AzureRM.Bootstrapper module and set the profile. 
Install-Module -Name AzureRm.BootStrapper -Force 
Use-AzureRmProfile -Profile 2017-03-09-profile 

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time. 
$domain = "AzureStack" 

# For integrated systems, use the IP address of one of the ERCS virtual machines 
$privilegedEndpoint = "AzS-ERCS01" 

# Point to the directory where the resource provider installation files were extracted. 
$tempDir = 'C:\TEMP\MYSQLRP' 

# The service admin account (can be Azure Active Directory or Active Directory Federation Services). 
$serviceAdmin = "admin@mydomain.onmicrosoft.com" 
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass) 
 
# Set credentials for the new resource provider VM. 
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass) 
 
# And the cloudadmin credential required for privileged endpoint access. 
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass) 

# Change the following as appropriate. 
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
 
# Change directory to the folder where you extracted the installation files. 
# Then adjust the endpoints. 
$tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds ` 
-VMLocalCredential $vmLocalAdminCreds ` 
-CloudAdminCredential $cloudAdminCreds ` 
-PrivilegedEndpoint $privilegedEndpoint ` 
-DefaultSSLCertificatePassword $PfxPass ` 
-DependencyFilesLocalPath $tempDir\cert ` 
-AcceptLicense 
``` 
 
### <a name="updatemysqlproviderps1-parameters"></a>Parâmetros de UpdateMySQLProvider.ps1 
Pode especificar estes parâmetros na linha de comandos. Se não, ou se falhar qualquer validação dos parâmetros, lhe for pedido para fornecer os parâmetros necessários. 

| Nome do Parâmetro | Descrição | Comentário ou o valor predefinido | 
| --- | --- | --- | 
| **CloudAdminCredential** | A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios. | _Necessário_ | 
| **AzCredential** | As credenciais para a conta de administrador do serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure. | _Necessário_ | 
| **VMLocalCredential** |As credenciais da conta de administrador local do fornecedor de recursos VM do SQL Server. | _Necessário_ | 
| **PrivilegedEndpoint** | O endereço IP ou nome DNS do ponto final com privilégios. |  _Necessário_ | 
| **DependencyFilesLocalPath** | O ficheiro de. pfx do certificado deve ser colocado neste diretório bem. | _Opcional_ (_obrigatório_ para vários nós) | 
| **DefaultSSLCertificatePassword** | A palavra-passe para o certificado. pfx. | _Necessário_ | 
| **MaxRetryCount** | O número de vezes que pretende repetir a cada operação, se ocorrer uma falha.| 2 | 
| **RetryDuration** | O intervalo de tempo limite entre tentativas, em segundos. | 120 | 
| **Desinstalar** | Remova o fornecedor de recursos e recursos de todos os associados (consulte as seguintes notas). | Não | 
| **DebugMode** | Impede a limpeza automática em caso de falha. | Não | 
| **AcceptLicense** | Ignora o pedido para aceitar o GPL de licenciamento.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | | 
 
## <a name="collect-diagnostic-logs"></a>Recolher registos de diagnóstico 
O fornecedor de recursos de MySQL é um bloqueado para baixo de máquina virtual. Se for necessário recolher registos de máquina virtual, um ponto final de PowerShell apenas suficiente administração (JEA) _DBAdapterDiagnostics_ é fornecido para o efeito. Existem dois comandos estão disponíveis através deste ponto final: 

- **Get-AzsDBAdapterLog**. Prepara um pacote zip que contém os registos de diagnóstico RP e coloca-o na unidade de utilizador de sessão. O comando pode ser chamado sem parâmetros e irá recolher as últimos quatro horas de registos. 

- **Remover AzsDBAdapterLog**. Limpa os pacotes de registo existentes no fornecedor de recursos VM 

Uma conta de utilizador denominada _dbadapterdiag_ é criada durante a implementação de RP ou atualização para ligar ao ponto final de diagnóstico para extrair os registos RP. A palavra-passe desta conta é o mesmo que a palavra-passe fornecida para a conta de administrador local durante a implementação/atualização. 

Para utilizar estes comandos, terá de criar uma sessão remota do PowerShell para a máquina virtual do fornecedor de recursos e invocar o comando. Opcionalmente, pode fornecer parâmetros FromDate e ToDate. Se não especificar um ou ambos, a FromDate quatro horas antes da hora atual, e o ToDate será a hora atual. 

Este script de exemplo demonstra a utilização destes comandos: 

```powershell 
# Create a new diagnostics endpoint session. 
$databaseRPMachineIP = '<RP VM IP address>' 
$diagnosticsUserName = 'dbadapterdiag' 
$diagnosticsUserPassword = '<Enter Diagnostic password>' 
$diagCreds = New-Object System.Management.Automation.PSCredential ` 
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force)) 
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds 
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
O fornecedor de recursos de MySQL é um bloqueado para baixo de máquina virtual. A atualização de segurança de recursos fornecedor da máquina virtual pode ser feita o ponto final do PowerShell apenas suficiente administração (JEA) _DBAdapterMaintenance_. Um script é fornecido com o pacote de instalação no RP para facilitar a estas operações. 

### <a name="update-the-virtual-machine-operating-system"></a>Atualize o sistema operativo da máquina virtual 
Existem várias formas de atualizar a VM do Windows Server: 
- Instalar o pacote mais recente do fornecedor de recursos utilizando uma imagem do Windows Server 2016 Core atualmente patched 
- Instalar um pacote do Windows Update durante a instalação ou atualização no RP 

### <a name="update-the-virtual-machine-windows-defender-definitions"></a>Atualizar as definições do Windows Defender de máquina virtual 
Siga estes passos para atualizar as definições de Defender: 
1. As definições do Windows Defender update a partir de transferência [Windows Defender definição](https://www.microsoft.com/en-us/wdsi/definitions).

    Nessa página, em "Manualmente, transfira e instale as definições" transferir "Windows Defender antivírus do Windows 10 e Windows 8.1" ficheiro de 64 bits.
    
    Ligação direta: https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64. 

2. Crie uma sessão do PowerShell para o ponto final de manutenção de MySQL RP adaptador da máquina virtual. 

3. Copie o ficheiro de atualização de definições para a máquina de placa de base de dados utilizando a sessão de ponto final de manutenção. 

4. Sobre a manutenção do PowerShell sessão invocar o _atualização DBAdapterWindowsDefenderDefinitions_ comando. 

5. Após a instalação, é recomendado para remover o ficheiro de atualização de definições utilizadas. Podem ser removido a sessão de manutenção utilizando o _remover ItemOnUserDrive)_ comando. 

Eis um exemplo de script para atualizar as definições de Defender (substituir o endereço ou o nome da máquina virtual com o valor real): 

```powershell 
# Set credentials for the RP VM local admin user 
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force 
$vmLocalAdminUser = "<local admin user name>" 
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ` 
    ($vmLocalAdminUser, $vmLocalAdminPass) 

# Public IP Address of the DB adapter machine 
$databaseRPMachine  = "<RP VM IP address>" 
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe" 
 
# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions.  
Invoke-WebRequest -Uri 'https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64' ` 
    -Outfile $localPathToDefenderUpdate  

# Create session to the maintenance endpoint 
$session = New-PSSession -ComputerName $databaseRPMachine ` 
    -Credential $vmLocalAdminCreds -ConfigurationName DBAdapterMaintenance 

# Copy defender update file to the db adapter machine 
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate ` 
     -Destination "User:\" 

# Install the update file 
Invoke-Command -Session $session -ScriptBlock ` 
    {Update-AzSDBAdapterWindowsDefenderDefinition -DefinitionsUpdatePackageFile "User:\"} 

# Cleanup the definitions package file and session 
Invoke-Command -Session $session -ScriptBlock ` 
    {Remove-AzSItemOnUserDrive -ItemPath "User:\"} 
$session | Remove-PSSession  
``` 
### <a name="secrets-rotation"></a>Rotação de segredos  
*Estas instruções aplicam-se apenas ao Azure pilha integrada sistemas versão 1804 e mais tarde. Não tente rotação secreta em versões de pilha do pre-1804 do Azure.* 
 
Quando utilizar os fornecedores de recursos do SQL Server e o MySQL com pilha do Azure integrado sistemas, pode rodar os seguintes segredos de infraestrutura (implementação): 
- Certificado SSL externo [fornecido durante a implementação](azure-stack-pki-certs.md). 
- O recurso fornecedor VM administrador local conta palavra-passe fornecida durante a implementação. 
- Palavra-passe da utilizador diagnóstico (dbadapterdiag) do fornecedor recursos. 
#### <a name="powershell-examples-for-rotating-secrets"></a>Exemplos do PowerShell para rodar segredos 
 
**Altere todos os segredos em simultâneo** 
```powershell 
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd `  
    -VMLocalCredential $localCreds 
``` 
**Alterar utilizador diagnóstico palavra-passe apenas** 
```powershell 
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd  
``` 

**Alterar a palavra-passe de conta de administrador local VM** 
```powershell 
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds 
``` 
**Certificado SSL de alteração** 
```powershell 
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd  
``` 

### <a name="secretrotationmysqlproviderps1-parameters"></a>Parâmetros de SecretRotationMySQLProvider.ps1 
|Parâmetro|Descrição| 
|-----|-----| 
|AzCredential|Credencial de conta de administrador de serviços de pilha do Azure.| 
|CloudAdminCredential|Azure pilha nuvem domínio conta credencial de administrador.| 
|PrivilegedEndpoint|Ponto final com privilégios para aceder à Get AzureStackStampInformation.| 
|DiagnosticsUserPassword|Palavra-passe de utilizador de diagnóstico.| 
|VMLocalCredential|A conta de administrador local da MySQLAdapter VM.| 
|DefaultSSLCertificatePassword|Predefinição certificado SSL (* pfx) palavra-passe.| 
|DependencyFilesLocalPath|Caminho Local do ficheiros de dependência.| 
|     |     | 

### <a name="known-issues"></a>Problemas conhecidos 
Problema: Os registos para rotação de segredos não são automaticamente recolhidos se o script falhar quando for executada. 
 
Solução: Utilize o cmdlet Get-AzsDBAdapterLogs para recolher todos os registos do fornecedor de recursos, incluindo AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log, sob C:\Logs. 

## <a name="remove-the-mysql-resource-provider-adapter"></a>Remova o adaptador de fornecedor de recursos de MySQL 
Para remover o fornecedor de recursos, é essencial primeiro de remover as dependências. 
1. Certifique-se de que tem o pacote de implementação original que transferiu para esta versão do fornecedor de recursos. 

2. Devem ser apagadas todas as bases de dados do inquilino do fornecedor de recursos. (A eliminar as bases de dados do inquilino não elimina os dados.) Esta tarefa deve ser efetuada por si próprios inquilinos. 

3. Os inquilinos tem de anular o registo do espaço de nomes. 

4. O administrador tem de eliminar os servidores de alojamento da placa de MySQL. 

5. O administrador tem de eliminar quaisquer planos que referenciam o adaptador de MySQL. 

6. O administrador tem de eliminar quaisquer quotas que estão associados a placa de MySQL. 

7. Volte a executar o script de implementação utilizando os seguintes parâmetros: 
    - -Desinstalar parâmetro 
    - Os pontos finais do Azure Resource Manager 
    - O DirectoryTenantID 
    - As credenciais da conta de administrador de serviço 

### <a name="next-steps"></a>Passos Seguintes
[Oferecer serviços aplicacionais como PaaS](azure-stack-app-service-overview.md)

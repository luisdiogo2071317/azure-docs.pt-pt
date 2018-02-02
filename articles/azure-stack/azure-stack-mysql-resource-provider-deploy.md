---
title: Utilizar bases de dados MySQL como PaaS na pilha do Azure | Microsoft Docs
description: "Saiba como pode implementar o fornecedor de recursos de MySQL e fornecer bases de dados MySQL como um serviço na pilha do Azure."
services: azure-stack
documentationCenter: 
author: mattbriggs
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: mabrigg
ms.openlocfilehash: db7daf61fa80854c17b58252d7d6cb30c329dfb1
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados MySQL na pilha do Microsoft Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode implementar um fornecedor de recursos de MySQL na pilha do Azure. Depois de implementar o fornecedor de recursos, pode criar servidores MySQL e bases de dados através de modelos de implementação Azure Resource Manager. Também pode fornecer bases de dados MySQL como um serviço. 

Bases de dados MySQL, que são comuns em web sites, suportam várias plataformas de Web site. Por exemplo, depois de implementar o fornecedor de recursos, pode criar sites WordPress da plataforma de aplicações Web como um suplemento de serviço (PaaS) para a pilha do Azure.

Para implementar o fornecedor de MySQL num sistema que não tem acesso à Internet, copie o ficheiro [mysql conector-net 6.10.5.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) para uma partilha local. Em seguida, fornece esse nome de partilha quando lhe for pedido para o mesmo. Tem de instalar os módulos do Azure e o Azure PowerShell da pilha.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>Arquitetura de adaptador do fornecedor de recursos do servidor de MySQL

O fornecedor de recursos é constituído por três componentes:

- **O adaptador de fornecedor de recursos de MySQL VM**, que é uma máquina virtual do Windows que está a executar os serviços do fornecedor.

- **O fornecedor de recursos próprio**, que processa os pedidos de aprovisionamento e expõe recursos de base de dados.

- **Servidores que alojam o servidor de MySQL**, que fornecem a capacidade para bases de dados que são denominados servidores de alojamento.

Esta versão já não cria instâncias MySQL. Isto significa que terá de criá-los por si e/ou fornecer acesso a instâncias externas do SQL Server. Visite o [Galeria de início rápido do Azure pilha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) para um modelo de exemplo que podem:
- Crie um servidor de MySQL para si.
- Transferir e implementar um servidor de MySQL do Azure Marketplace.

> [!NOTE]
> Servidores que estão instaladas numa implementação de pilha do Azure com vários nós de alojamento têm de ser criado a partir de uma subscrição de inquilino. Estes não podem ser criados da subscrição de fornecedor predefinido. Deve ser criadas no portal de inquilinos ou de uma sessão do PowerShell com um adequado início de sessão. Todos os servidores de alojamento são cobráveis VMs e tem de ter licenças adequadas. O administrador de serviço pode ser o proprietário da subscrição de inquilino.

### <a name="required-privileges"></a>Privilégios necessários
A conta do sistema tem de ter os seguintes privilégios:

1.  Base de dados: Criar, remover
2.  Início de sessão: Criar, definir, remover, conceder, revogar

## <a name="deploy-the-resource-provider"></a>Implementar o fornecedor de recursos

1. Se ainda não o tiver feito, registe o kit de desenvolvimento e transferir a imagem do Windows Server 2016 Datacenter Core transferível através da gestão do Marketplace. Tem de utilizar uma imagem do Windows Server 2016 Core. Também pode utilizar um script para criar um [imagem do Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Lembre-se de que seleciona a opção de núcleos.) O tempo de execução de .NET 3.5 já não é necessário.


2. A iniciar sessão para um anfitrião que pode aceder ao ponto final com privilégios VM.

    - Nas instalações do SDK do Azure, inicie sessão no anfitrião físico. 
    - Em sistemas com vários nós, o anfitrião tem de ser um sistema que pode aceder ao ponto de final com privilégios.
    
    >[!NOTE]
    > O sistema em que o script está a ser executado *tem* ser um sistema Windows 10 ou Windows Server 2016 com a versão mais recente do tempo de execução de .NET instalada. Caso contrário, a instalação falha. O anfitrião do Azure SDK cumpre estes critérios.
    

3. Transfira o fornecedor de recursos de MySQL binário. Em seguida, execute o Self-extractor para extrair os conteúdos num diretório temporário.

    >[!NOTE] 
    > Corresponde a compilação de fornecedor de recursos para as compilações de pilha do Azure. Não se esqueça de transferir o binário correto para a versão da pilha do Azure que está em execução.

    | Compilação de pilha do Azure | Instalador de MySQL RP |
    | --- | --- |
    | 1.0.180102.3 ou 1.0.180106.1 (com vários nós) | [MySQL RP versão 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) |
    | 1.0.171122.1 | [MySQL RP versão 1.1.12.0](https://aka.ms/azurestackmysqlrp1711) |
    | 1.0.171028.1 | [MySQL RP versão 1.1.8.0](https://aka.ms/azurestackmysqlrp1710) |

4.  O certificado de raiz de pilha do Azure é obtido a partir do ponto final com privilégios. Para o SDK do Azure, é criado um certificado autoassinado como parte deste processo. Em vários nós, tem de fornecer um certificado adequado.

    Se tiver de fornecer o seu próprio certificado, colocar um ficheiro. pfx a **DependencyFilesLocalPath** que cumpra os seguintes critérios:

    - Um certificado de caráter universal para \*.dbadapter.\< região\>.\< fqdn externo\> ou um certificado de site único com um nome comum de mysqladapter.dbadapter.\< região\>.\< fqdn externo\>.

    - Este certificado tem de ser fidedigno. Ou seja, a cadeia de confiança tem de existir sem exigir certificados intermédios.

    - Existe apenas um ficheiro de certificado único no DependencyFilesLocalPath.
    
    - O nome de ficheiro não pode conter os caracteres especiais.


5. Abra um **novo** elevada consola do PowerShell (administrativa). Em seguida, altere o diretório onde extraiu os ficheiros. Utilize uma nova janela para evitar problemas que possam surgir de incorretos módulos do PowerShell que já estão carregados no sistema.

6. [Instalar o Azure PowerShell versão 1.2.11](azure-stack-powershell-install.md).

7. Execute o script `DeployMySqlProvider.ps1`.

O script efetua estes passos:

* Transfere o binário de conector MySQL (Isto pode ser fornecido offline).
* Carrega os certificados e outros artefactos para uma conta de armazenamento na pilha do Azure.
* Publica os pacotes de galeria que pode implementar as bases de dados do SQL Server através da galeria.
* Publica um pacote de galeria para implementar servidores de alojamento.
* Implementa uma VM utilizando a imagem do Windows Server 2016 que foi criada no passo 1. Também instala o fornecedor de recursos.
* Regista um registo DNS local, que mapeia para o fornecedor de recursos VM.
* Regista o fornecedor de recursos com o local do Azure Resource Manager (inquilino e administrador).


Pode:
- Especifique os parâmetros necessários na linha de comandos.
- Execute sem quaisquer parâmetros e, em seguida, introduza-las quando lhe for pedido.

Eis um exemplo que pode executar a partir da linha de comandos do PowerShell. É necessário alterar as palavras-passe conforme necessário e informações da conta:


```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure SDK, the default is AzureStack, and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM.
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
. $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense

 ```


### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parameters
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



Consoante as velocidades de desempenho e a transferência de sistema, instalação pode demorar tão pequeno como 20 minutos ou longa como várias horas. Se o **MySQLAdapter** painel não está disponível, atualize o portal de administração.

> [!NOTE]
> Se a instalação demora mais de 90 minutos, poderá falhar. Se existir, verá uma mensagem de falha no ecrã e no ficheiro de registo. A implementação é repetida do passo falhar. Sistemas que não cumprem as especificações de núcleo e de memória recomendadas poderão não conseguir implementar o MySQL RP.



## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Verificar a implementação através do portal de pilha do Azure

> [!NOTE]
>  Após o script de instalação em execução, terá de atualizar o portal para ver o painel de administração.


1. Inicie sessão no portal de administração como o administrador de serviço.

2. Certifique-se de que a implementação concluída com êxito. Aceda a **grupos de recursos**e, em seguida, selecione o **system.\< localização\>.mysqladapter** grupo de recursos. Certifique-se de que todas as implementações de quatro teve êxito.

      ![Verificar a implementação de RP o MySQL](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png)

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Fornecem a capacidade de ligar a um servidor de alojamento MySQL

1. Inicie sessão no portal do Azure pilha como um administrador de serviço.

2. Selecione **recursos administrativos** > **MySQL servidores de alojamento** > **+ adicionar**.

    No **servidores de alojamento MySQL** painel, pode ligar o fornecedor de recursos do servidor de MySQL para instâncias reais do servidor de MySQL que servem de back-end o fornecedor de recursos.

    ![Servidores de alojamento](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. Forneça os detalhes de ligação da sua instância do servidor de MySQL. Lembre-se de que forneça o nome de domínio completamente qualificado (FQDN) ou um endereço IPv4 válido e não o nome abreviado de VM. Esta instalação já não fornece uma instância de MySQL predefinida. O tamanho que é fornecido ajuda-o o gerir a capacidade de base de dados do fornecedor de recursos. Deve estar perto a capacidade do servidor da base de dados física.

    > [!NOTE]
    >Se a instância de MySQL pode ser acedida pelo inquilino e administrador do Azure Resource Manager, pode ser colocado sob o controlo do fornecedor de recursos. A instância de MySQL *tem* atribuída exclusivamente para o fornecedor de recursos.

4. Como adicionar servidores, tem de atribuir-lhes um SKU de novo ou existente para permitir a diferenciação de ofertas de serviço.
  Por exemplo, pode ter uma instância de enterprise que fornecer:
    - capacidade de base de dados
    - Cópia de segurança automática
    - servidores de elevado desempenho para departamentos individuais de reserva
 

O nome do SKU deve refletir as propriedades, para que os inquilinos podem colocar as bases de dados adequadamente. Todos os servidores de alojamento de um SKU devem ter as mesmas capacidades.

![Criar um SKU de MySQL](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
> SKUs podem demorar até uma hora ser visível no portal. Não é possível criar uma base de dados até que seja criada a SKU.


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

## <a name="update-the-mysql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Atualizar o adaptador de fornecedor de recursos de MySQL (com vários nós apenas, compilações 1710 e posteriores)
Sempre que é atualizada a compilação de pilha do Azure, é lançada uma nova placa de fornecedor de recursos de MySQL. O adaptador existente poderá continuar a trabalhar. No entanto, recomendamos a atualizar para a compilação mais recente logo que possível após a pilha do Azure é atualizada. 

O processo de atualização é semelhante ao processo de instalação que foi descrito anteriormente. Criar uma nova VM com o código mais recente do fornecedor de recursos. Em seguida, migrar as definições para esta nova instância, incluindo a base de dados e as informações do servidor de alojamento. Também migrar o registo DNS necessário.

Utilize o script de UpdateMySQLProvider.ps1 com os mesmos argumentos que foram descritos anteriormente. Forneça o certificado aqui, bem como.

> [!NOTE]
> A atualização só é suportada em sistemas com vários nós.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

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
. $tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds `
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
| **DefaultSSLCertificatePassword** | A palavra-passe para o certificado. pfx | _Necessário_ |
| **MaxRetryCount** | O número de vezes que pretende repetir a cada operação, se ocorrer uma falha.| 2 |
| **RetryDuration** | O intervalo de tempo limite entre tentativas, em segundos. | 120 |
| **Desinstalar** | Remova o fornecedor de recursos e recursos de todos os associados (consulte as seguintes notas). | Não |
| **DebugMode** | Impede a limpeza automática em caso de falha. | Não |
| **AcceptLicense** | Ignora o pedido para aceitar o GPL de licenciamento.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |

## <a name="remove-the-mysql-resource-provider-adapter"></a>Remova o adaptador de fornecedor de recursos de MySQL

Para remover o fornecedor de recursos, é essencial primeiro de remover as dependências.

1. Certifique-se de que tem o pacote de implementação original que transferiu para esta versão do fornecedor de recursos.

2. Devem ser apagadas todas as bases de dados do inquilino do fornecedor de recursos. (A eliminar as bases de dados do inquilino não elimina os dados.) Esta tarefa deve ser efetuada por si próprios inquilinos.

3. Os inquilinos tem de anular o registo do espaço de nomes.

4. O administrador tem de eliminar os servidores de alojamento da placa de MySQL.

5. O administrador tem de eliminar quaisquer planos que referenciam o adaptador de MySQL.

6. O administrador tem de eliminar quaisquer quotas que estão associados a placa de MySQL.

7. Volte a executar o script de implementação com os seguintes elementos:
    - -Desinstalar parâmetro
    - Os pontos finais do Azure Resource Manager
    - O DirectoryTenantID
    - As credenciais da conta de administrador de serviço

---
title: Utilizar bases de dados MySQL no Azure pilha | Microsoft Docs
description: Saiba como pode implementar bases de dados MySQL como um serviço na pilha do Azure e os passos rápidos para implementar o adaptador de fornecedor de recursos do servidor MySQL.
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
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 061d9431890622dafafa8c180027b8df735c100e
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295759"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados MySQL na pilha do Microsoft Azure
Utilize o fornecedor de recursos do servidor de MySQL de pilha do Azure para expor bases de dados MySQL como um serviço da pilha do Azure. O serviço de fornecedor de recursos de MySQL é executado no fornecedor de recursos de MySQL VM, que é uma máquina de virtual do Windows Server core.

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
    - Implementa uma VM com uma imagem de mercado de pilha do Windows Server 2016 do Azure e instala o fornecedor de recursos. 
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
        -VMLocalCredential $vmLocalAdminCreds ` 
        -CloudAdminCredential $cloudAdminCreds ` 
        -PrivilegedEndpoint $privilegedEndpoint ` 
        -DefaultSSLCertificatePassword $PfxPass ` 
        -DependencyFilesLocalPath $tempDir\cert ` 
        -AcceptLicense 
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


## <a name="next-steps"></a>Passos Seguintes
[Adicionar servidores de alojamento](azure-stack-mysql-resource-provider-hosting-servers.md)

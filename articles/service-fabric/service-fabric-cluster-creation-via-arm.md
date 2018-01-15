---
title: Criar um cluster do Service Fabric do Azure a partir de um modelo | Microsoft Docs
description: "Este artigo descreve como configurar um cluster do Service Fabric seguro no Azure utilizando o Azure Resource Manager, o Cofre de chaves do Azure e o Azure Active Directory (Azure AD) para autenticação de cliente."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: chackdan
ms.openlocfilehash: e5dd1ebd290c950c7f2bda3dae088f3ee7f836fd
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Criar um cluster do Service Fabric com o Azure Resource Manager 
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Portal do Azure](service-fabric-cluster-creation-via-portal.md)
>
>

Este guia passo a passo explica como configurar um cluster do Azure Service Fabric seguro no Azure utilizando o Gestor de recursos do Azure. Iremos confirmar que o artigo é longo. Contudo, a menos que já estejam exaustivamente familiarizado com o conteúdo, lembre-se de que seguir os passos cuidadosamente.

O guia abrange os procedimentos seguintes:

* Conceitos chave que precisa de ter em consideração desativada antes de implementar um cluster do service fabric.
* Criar um cluster no Azure através da utilização de módulos do serviço Gestor de recursos de infraestrutura.
* Configurar o Azure Active Directory (Azure AD) para autenticar utilizadores efetuar as operações de gestão no cluster.
* Criação de um modelo Azure Resource Manager personalizado para o cluster e implementar a política.

## <a name="key-concepts-to-be-aware-of"></a>Conceitos chave a ter em consideração
No Azure, o Service fabric exige deste que utilizar um x509 certificado para proteger o seu cluster e os respetivos pontos finais. Os certificados são utilizados no Service Fabric para fornecer autenticação e encriptação para proteger diferentes aspetos de um cluster e as respetivas aplicações. Para o cliente acesso/efetuar operações de gestão no cluster, incluindo a implementar, atualizar e eliminar aplicações, serviços e os dados que contêm, pode utilizar certificados ou as credenciais do Azure Active Directory. A utilização do Azure Active Directory é vivamente recomendada, uma vez que é a única forma de impedir a partilha de certificados nos seus clientes.  Para obter mais informações sobre como os certificados são utilizados no Service Fabric, consulte [cenários de segurança de cluster do Service Fabric][service-fabric-cluster-security].

O Service Fabric utiliza certificados x. 509 para proteger um cluster e fornecer funcionalidades de segurança da aplicação. Utilizar [Cofre de chaves] [ key-vault-get-started] para gerir os certificados para clusters de Service Fabric no Azure. 


### <a name="cluster-and-server-certificate-required"></a>Cluster e servidor de certificado (obrigatório)
Estes certificados (um site primário e, opcionalmente, uma secundária) são necessários para proteger um cluster e impedir o acesso não autorizado ao mesmo. Fornece segurança de cluster de duas formas:

* **Autenticação do cluster:** autentica o nó de nó de comunicação para a Federação de cluster. Apenas nós que podem provar a sua identidade com este certificado podem aderirem ao cluster.
* **Autenticação de servidor:** autentica pontos finais de gestão de cluster para um cliente de gestão, para que o cliente de gestão sabe que o cluster real e não um 'man no meio' é falar com. Este certificado fornece também um SSL para a API de gestão HTTPS e para o Service Fabric Explorer através de HTTPS.

Para efetuar estes fins, o certificado tem de cumprir os seguintes requisitos:

* O certificado tem de conter uma chave privada. Estes certificados têm normalmente. pfx de extensões ou. pem  
* O certificado tem de ser criado para a troca de chaves, que é exportável para um ficheiro Personal Information Exchange (. pfx).
* O **nome do requerente do certificado tem de corresponder ao domínio que utilizar para aceder ao cluster do Service Fabric**. Esta correspondência é necessário para fornecer um SSL para o cluster ponto final de gestão HTTPS e Service Fabric Explorer. Não é possível obter um certificado SSL de uma autoridade de certificação (AC) para o *. cloudapp.azure.com domínio. Tem de obter um nome de domínio personalizado para o cluster. Quando solicitar um certificado a partir de uma AC, o nome de requerente do certificado tem de corresponder ao nome de domínio personalizado que utilizar para o cluster.

### <a name="set-up-azure-active-directory-for-client-authentication-optional-but-recommended"></a>Configurar o Azure Active Directory para autenticação de cliente (opcional, mas recomendado)

Azure AD permite às organizações (conhecidas como inquilinos) para gerir o acesso de utilizador para aplicações. As aplicações são divididas aqueles com uma conta baseada na web IU de início de sessão e as pessoas com uma experiência de cliente nativo. Neste artigo, partimos do pressuposto que já criou um inquilino. Se não tiver, comece por ler [como obter um inquilino do Azure Active Directory][active-directory-howto-tenant].

Um cluster do Service Fabric oferece vários pontos de entrada para a funcionalidade de gestão, incluindo baseada na web [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] e [Visual Studio][service-fabric-manage-application-in-visual-studio]. Como resultado, crie duas aplicações do Azure AD para controlar o acesso ao cluster, uma aplicação web e uma aplicação nativa.

Mais informações sobre como configurá-lo mais tarde no documento.

### <a name="application-certificates-optional"></a>Certificados de aplicação (opcionais)
Qualquer número de certificados adicionais pode ser instalado num cluster por motivos de segurança da aplicação. Antes de criar o cluster, considere os cenários de segurança de aplicações que necessitam de um certificado para ser instalada em nós, tais como:

* A encriptação e desencriptação de valores de configuração de aplicação.
* Encriptação dos dados em nós durante a replicação.

O conceito de criação de clusters seguros é os mesmos, quer sejam Linux ou clusters do Windows. 

### <a name="client-authentication-certificates-optional"></a>Certificados de autenticação de cliente (opcional)
Qualquer número de certificados adicionais pode ser especificado para operações de cliente de administrador ou utilizador. Por predefinição, o certificado de cluster tem privilégios de cliente de administrador. Estes certificados de cliente adicionais não devem ser instalados num cluster, apenas tem de ser especificado como permitidas na configuração de cluster, no entanto, têm de ser instalado nos computadores cliente para ligar ao cluster e executar qualquer gestão operações.


## <a name="prerequisites"></a>Pré-requisitos 
O conceito de criação de clusters seguros é os mesmos, quer sejam Linux ou clusters do Windows. Este guia abrange a utilização do azure powershell ou a CLI do azure para criar novos clusters. Os pré-requisitos são 

-  [O Azure PowerShell 4.1 e acima] [ azure-powershell] ou [CLI do Azure 2.0 e acima][azure-CLI].
-  Pode encontrar detalhes sobre os fabic módulos do serviço de aqui - [AzureRM.ServiceFabric](https://docs.microsoft.com/powershell/module/azurerm.servicefabric) e [módulo do az SF CLI](https://docs.microsoft.com/cli/azure/sf?view=azure-cli-latest)


## <a name="use-service-fabric-rm-module-to-deploy-a-cluster"></a>Utilize o módulo do serviço de recursos de infraestrutura RM para implementar um cluster

Neste documento, teria de utilizar o powershell de service fabric RM e módulo CLI para implementar um cluster, o powershell ou o comando de módulo CLI permite para vários cenários. Informe-nos percorra cada um da. Escolher o cenário sentir melhor satisfaz as suas necessidades. 

- Criar um novo cluster – a utilizar um sistema a gerar o certificado autoassinado
    - Utilizar um modelo de cluster predefinido
    - Utilize um modelo que já tenha
- Criar um novo cluster – a utilizar um certificado que já possui
    - Utilizar um modelo de cluster predefinido
    - Utilize um modelo que já tenha

### <a name="create-new-cluster----using-a-system-generated-self-signed-certificate"></a>Criar novo cluster – a utilizar um sistema a gerar o certificado autoassinado

Utilize o seguinte comando para criar o cluster, se tiver pretende que o sistema para gerar um certificado autoassinado e utilizá-la para proteger o seu cluster. Este comando configura um certificado de cluster primário que é utilizado para a segurança do cluster e para configurar o acesso de administrador para efetuar operações de gestão a utilizar esse certificado.

### <a name="login-in-to-azure"></a>Inicie sessão no Azure.

```Powershell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```
#### <a name="use-the-default-5-node-1-nodetype-template-that-ships-in-the-module-to-set-up-the-cluster"></a>Utilize o modelo de nodetype do predefinida de 5 1 nó que é fornecido no módulo para configurar o cluster

Utilize o seguinte comando para criar um cluster rapidamente, especificando os parâmetros mínimos

O modelo que é utilizado está disponível na [amostras de modelo de recursos de infraestrutura de serviço do azure: modelo de windows](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) e [Ubuntu modelo](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

Os comandos abaixo funciona para criação de clusters do Windows e Linux, apenas tem de especificar o SO em conformidade. O powershell / comandos da CLI produz também o certificado do certificado no theCertificateOutputFolder especificado no. O comando aceita outros parâmetros, bem como o SKU de VM.

```Powershell

$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"
$certOutputFolder="c:\certificates"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -OS $os -VmPassword $vmpassword -VmUserName $vmuser 

```

```CLI

declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare CertSubjectName="mylinux.westus.cloudapp.azure.com"
declare vmpassword="Password!1"
declare certpassword="Password!1"
declare vmuser="myadmin"
declare vmOs="UbuntuServer1604"
declare certOutputFolder="c:\certificates"



az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certpassword  \
    --vault-name $vaultName --vault-resource-group $resourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath --vm-os $vmOs  \
    --vm-password $vmpassword --vm-user-name $vmuser

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Utilizar o modelo personalizado que já tenha 

Se precisar de criar um modelo personalizado para se adequarem às suas necessidades, recomenda-se vivamente que comece com um dos modelos que estão disponíveis no [amostras de modelo de recursos de infraestrutura de serviço do azure](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Siga as orientações e explicações para [personalizar o modelo de cluster] [ customize-your-cluster-template] secção abaixo.

Se já tiver um modelo personalizado, certifique-se de que a verificação de double, que o certificado três relacionados com os parâmetros no modelo e o ficheiro de parâmetros são denominados de forma e os valores são nulas da seguinte forma.

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```Powershell


$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```

Eis o comando da CLI equivalente para o fazer. Altere os valores nas instruções ' declare para os valores adequados. CLI suporta todos os outros parâmetros que suporta o comando do powershell acima.

```CLI

declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath

```


### <a name="create-new-cluster---using-the-certificate-you-bought-from-a-ca-or-you-already-have"></a>Criar novo cluster - utilizando o certificado comprou a partir de uma AC ou que já tem.

Utilize o seguinte comando para criar o cluster, se tiver um certificado que pretende utilizar para proteger o cluster com o.

Se se tratar de um certificado assinado pela AC que irá surgir a utilizar para outros fins, bem como, em seguida, é recomendado que forneça um grupo de recursos distintos especificamente para o seu Cofre de chaves. Recomendamos que colocar o Cofre de chaves no seu próprio grupo de recursos. Esta ação permite-lhe remover os grupos de recursos de armazenamento e computação, incluindo o grupo de recursos que contém o cluster do Service Fabric, sem perder as chaves e segredos. **O grupo de recursos que contém o seu Cofre de chaves _tem de estar na mesma região_ como o cluster que está a ser utilizado.**


#### <a name="use-the-default-5-node-1-nodetype-template-that-ships-in-the-module"></a>Utilize o modelo de nodetype do predefinida de 5 1 nó que é fornecido no módulo
O modelo que é utilizado está disponível na [exemplos do azure: modelo de windows](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) e [Ubuntu modelo](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

```Powershell
$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword=("Password!1" | ConvertTo-SecureString -AsPlainText -Force) 
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"

New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile C:\MyCertificates\chackocertificate3.pfx -CertificatePassword $certPassword -OS $os -VmPassword $vmpassword -VmUserName $vmuser 

```

```CLI

declare vmPassword="Password!1"
declare certPassword="Password!1"
declare vmUser="myadmin"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare certificate-file="c:\certificates\mycert.pem"
declare vmOs="UbuntuServer1604"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $certPassword  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --vm-os vmOs \
    --vm-password $vmPassword --vm-user-name $vmUser

```

#### <a name="use-the-custom-template-that-you-have"></a>Utilizar o modelo personalizado que tiver 
Se precisar de criar um modelo personalizado para se adequarem às suas necessidades, recomenda-se vivamente que comece com um dos modelos que estão disponíveis no [amostras de modelo de recursos de infraestrutura de serviço do azure](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Siga as orientações e explicações para [personalizar o modelo de cluster] [ customize-your-cluster-template] secção abaixo.

Se já tiver um modelo personalizado, certifique-se de que a verificação de double, que o certificado três relacionados com os parâmetros no modelo e o ficheiro de parâmetros são denominados de forma e os valores são nulas da seguinte forma.

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```Powershell

$resourceGroupLocation="westus"
$resourceGroupName="mylinux"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$os="WindowsServer2016DatacenterwithContainers"
$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"
$certificateFile="C:\MyCertificates\chackonewcertificate3.pem"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath -KeyVaultResouceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile $certificateFile -CertificatePassword #certPassword

```

Eis o comando da CLI equivalente para o fazer. Altere os valores nas instruções ' declare para os valores adequados.

```CLI

declare certPassword="Password!1"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $password  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 
```

#### <a name="use-a-pointer-to-the-secret-you-already-have-uploaded-into-the-keyvault"></a>Utilize um ponteiro para o segredo que já carregou para o keyvault

Para utilizar um cofre de chaves, _necessário ativá-la para implementação_ para permitir que o fornecedor de recursos de computação obter certificados a partir do mesmo e a instalar em nós de cluster:

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment


$parameterFilePath="c:\mytemplates\mytemplate.json"
$templateFilePath="c:\mytemplates\mytemplateparm.json"
$secertId="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroup -SecretIdentifier $secretID -TemplateFile $templateFile -ParameterFile $templateParmfile 

```
Eis o comando da CLI equivalente para o fazer. Altere os valores nas instruções ' declare para os valores adequados.

```cli

declare $parameterFilePath="c:\mytemplates\mytemplate.json"
declare $templateFilePath="c:\mytemplates\mytemplateparm.json"
declare $secertId="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --secret-identifieraz $secretID  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 

```

<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Configurar o Azure Active Directory para autenticação de cliente

Azure AD permite às organizações (conhecidas como inquilinos) para gerir o acesso de utilizador para aplicações. As aplicações são divididas aqueles com uma conta baseada na web IU de início de sessão e as pessoas com uma experiência de cliente nativo. Neste artigo, partimos do pressuposto que já criou um inquilino. Se não tiver, comece por ler [como obter um inquilino do Azure Active Directory][active-directory-howto-tenant].

Um cluster do Service Fabric oferece vários pontos de entrada para a funcionalidade de gestão, incluindo baseada na web [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] e [Visual Studio][service-fabric-manage-application-in-visual-studio]. Como resultado, crie duas aplicações do Azure AD para controlar o acesso ao cluster, uma aplicação web e uma aplicação nativa.

Para simplificar a alguns dos passos envolvidos na configuração do Azure AD com um cluster do Service Fabric, foi criado um conjunto de scripts do Windows PowerShell.

> [!NOTE]
> Tem de concluir os passos seguintes antes de criar o cluster. Uma vez que os scripts de esperam os nomes de clusters e os pontos finais, os valores deve ser planeadas e não os valores que já criou.

1. [Transfira os scripts] [ sf-aad-ps-script-download] para o seu computador.
2. Clique no ficheiro zip, selecione **propriedades**, selecione o **desbloqueio** caixa de verificação e, em seguida, clique em **aplicar**.
3. Extraia o ficheiro zip.
4. Executar `SetupApplications.ps1`e forneça o TenantId, ClusterName e WebApplicationReplyUrl como parâmetros. Por exemplo:

```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'

```

Pode encontrar o TenantId executando o comando do PowerShell `Get-AzureSubscription`. Executar este comando apresenta o TenantId para cada subscrição.

ClusterName é utilizado para o prefixo de aplicações do Azure AD que são criadas pelo script. Não é necessário o nome do cluster real de corresponder exatamente. Destina-se apenas a tornar mais fácil mapear artefactos do Azure AD para o cluster do Service Fabric que está a ser utilizados com.

WebApplicationReplyUrl é o ponto final predefinido, que devolve do Azure AD para os seus utilizadores, depois de concluir a iniciar sessão. Defina este ponto final como o ponto final do Service Fabric Explorer para o cluster, o que, por predefinição, está:

https://&lt;cluster_domain&gt;: 19080/Explorer

Lhe for pedido para iniciar sessão para uma conta que tenha privilégios administrativos para o inquilino do Azure AD. Depois de iniciar sessão, o script cria o web e aplicações nativas para representar o cluster do Service Fabric. Se observar aplicações do inquilino no [portal do Azure][azure-portal], deverá ver duas novas entradas:

   * *ClusterName*\_Cluster
   * *ClusterName*\_cliente

O script imprime o JSON exigido pelo modelo Azure Resource Manager, ao criar o cluster na secção seguinte, pelo que é uma boa ideia mantenha a janela do PowerShell aberta.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

<a id="customize-arm-template" ></a>

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Criar um modelo de Gestor de recursos de cluster do Service Fabric
Esta secção é para os utilizadores que pretendem personalizada criar um modelo de Gestor de recursos de cluster do Service Fabric. Depois de ter um modelo, ainda pode voltar atrás e utilizar o powershell ou os módulos do CLI para implementá-la. 

Modelos de Gestor de recursos de exemplo estão disponíveis no [exemplos do Azure no GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). Estes modelos, podem ser utilizados como um ponto de partida para o modelo de cluster.

### <a name="create-the-resource-manager-template"></a>Criar o modelo do Resource Manager
Este guia utiliza o [5-nó cluster segura] [ service-fabric-secure-cluster-5-node-1-nodetype] modelo de exemplo e os parâmetros do modelo. Transferir `azuredeploy.json` e `azuredeploy.parameters.json` para o seu computador e abra ambos os ficheiros no seu editor de texto favorito.

### <a name="add-certificates"></a>Adicione certificados
Adicione certificados para um modelo de Gestor de recursos do cluster ao consultar o Cofre de chaves que contém as chaves de certificado. Adicione esses parâmetros de Cofre de chaves e valores de um ficheiro de parâmetros de modelo do Resource Manager (azuredeploy.parameters.json). 

#### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Adicionar todos os certificados para o osProfile de conjunto de dimensionamento de máquina virtual
Cada certificado que está instalado no cluster deve ser configurado na secção osProfile o recurso de conjunto de dimensionamento (Microsoft.Compute/virtualMachineScaleSets). Esta ação instrui o fornecedor de recursos para instalar o certificado nas VMs. Esta instalação inclui o certificado de cluster e quaisquer certificados de segurança de aplicação que pretende utilizar para as suas aplicações:

```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-the-service-fabric-cluster-certificate"></a>Configurar o certificado de cluster do Service Fabric
O certificado de autenticação do cluster tem de ser configurado em ambos os o Service Fabric recurso de cluster (Microsoft.ServiceFabric/clusters) e a extensão de recursos de infraestrutura do serviço de dimensionamento da máquina virtual define no recurso de conjunto de dimensionamento de máquina virtual. Esta disposição permite que o fornecedor de recursos do Service Fabric configurá-la para utilização para a autenticação do cluster e a autenticação de servidor para pontos finais de gestão.

##### <a name="add-the-certificate-information-the-virtual-machine-scale-set-resource"></a>Adicione o que recurso de definir as informações do certificado de dimensionamento da Máquina Virtual:
```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="add-the-certificate-information-to-the-service-fabric-cluster-resource"></a>Adicione as informações do certificado para o recurso de cluster do Service Fabric:
```json
{
  "apiVersion": "[variables('sfrpApiVersion')]",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>Adicionar a configuração do AD do Azure para utilizar o Azure AD para acesso de cliente

Adicionar a configuração do Azure AD s para um modelo de Gestor de recursos do cluster ao consultar o Cofre de chaves que contém as chaves de certificado. Adicione esses valores de parâmetros do Azure AD e um ficheiro de parâmetros de modelo do Resource Manager (azuredeploy.parameters.json).

```json
{
  "apiVersion": "[variables('sfrpApiVersion')]",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### <a name="populate-the-parameter-file-with-the-values"></a>Preencha o ficheiro de parâmetros com os valores.
Por fim, utilize os valores de saída do Cofre de chaves e os comandos do powershell do Azure AD para preencher o ficheiro de parâmetros:

Se planeia utilizar os módulos do powershell do Azure service fabric RM, em seguida, não terá de preencher as informações do certificado de cluster, se de que pretende que o sistema para gerar o self assinado certificado de segurança do cluster, basta mantê-las como null. 

> [!NOTE]
> Para os módulos RM recolher e preencher estes valores de parâmetro vazio, os nomes de parâmetros muito corresponder aos nomes abaixo
>

```json
        "clusterCertificateThumbprint": {
            "value": ""
        },
        "clusterCertificateUrlValue": {
            "value": ""
        },
        "sourceVaultvalue": {
            "value": ""
        },
```

Se estiver a utilizar certificados de aplicação ou estiver a utilizar um cluster existente que carregou para o keyvault, terá de obter estas informações e preenchê-lo 

Os módulos RM não tem a capacidade de geneate a configuração do Azure AD para si. por isso, se planeia utilizar o Azure AD para acesso de cliente, terá de preenchê-lo.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```

### <a name="test-your-template"></a>Testar o modelo  
Utilize o seguinte comando do PowerShell para testar o modelo do Resource Manager com um ficheiro de parâmetros:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

No caso de depare com problemas e receber mensagens crípticas, em seguida, utilize "-Debug" como uma opção.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json -Debug
```

O diagrama seguinte ilustra onde ajustam o seu Cofre de chaves e a configuração do AD do Azure ao seu modelo do Resource Manager.

![Mapa de dependência do Gestor de recursos][cluster-security-arm-dependency-map]

## <a name="create-the-cluster-using-azure-resource-template"></a>Criar o cluster utilizando o modelo de recursos do Azure 

Agora pode implementar clusters utilizando os passos descritos anteriormente no documento ou, se tiver os valores no ficheiro de parâmetros, preenchido, em seguida, agora está pronto para criar o cluster utilizando [implementação do modelo de recursos do Azure] [ resource-group-template-deploy] diretamente.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

No caso de depare com problemas e receber mensagens crípticas, em seguida, utilize "-Debug" como uma opção.


<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Atribuir utilizadores a funções
Depois de criar as aplicações para representar o cluster, atribuir os seus utilizadores a funções suportadas pelo Service Fabric: só de leitura e administrador. Pode atribuir as funções utilizando o [portal do Azure][azure-portal].

1. No portal do Azure, selecione o seu inquilino no canto superior direito.

    ![Selecione o botão de inquilino][select-tenant-button]
2. Selecione **do Azure Active Directory** no separador à esquerda e, em seguida, selecionadas "aplicações da empresa".
3. Selecione "Todas as aplicações" e, em seguida, localize e selecione a aplicação web, que tem um nome como `myTestCluster_Cluster`.
4. Clique em de **utilizadores e grupos** separador.

    ![Separador utilizadores e grupos][users-and-groups-tab]
5. Clique em de **adicionar utilizador** botão na nova página, selecione um utilizador e a função e, em seguida, clique em de **selecione** na parte inferior da página.

    ![Atribuir utilizadores a página de funções][assign-users-to-roles-page]
6. Clique em de **atribuir** na parte inferior da página.

    ![Adicionar a confirmação de atribuição][assign-users-to-roles-confirm]

> [!NOTE]
> Para obter mais informações sobre as funções no Service Fabric, consulte [controlo de acesso baseado em funções para clientes de Service Fabric](service-fabric-cluster-security-roles.md).
>
>


## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Resolução de problemas de ajuda na configuração do Azure Active Directory
Configurar o Azure AD e utilizá-lo, pode ser um desafio, pelo que seguem-se algumas indicações sobre o que pode fazer para depurar o problema.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer pede-lhe para selecionar um certificado
#### <a name="problem"></a>Problema
Depois de iniciar sessão com êxito para o Azure AD no Service Fabric Explorer, o browser devolve à home page, mas uma mensagem pede-lhe para selecionar um certificado.

![Caixa de diálogo do SFX certificado][sfx-select-certificate-dialog]

#### <a name="reason"></a>Razão
O utilizador não está atribuído uma função na aplicação de cluster do Azure AD. Assim, a autenticação do Azure AD falha no cluster do Service Fabric. Service Fabric Explorer retrocede para autenticação de certificados.

#### <a name="solution"></a>Solução
Siga as instruções para configurar o Azure AD e atribuir funções de utilizador. Além disso, recomendamos que ative "Atribuição do utilizador necessária para aceder à aplicação," como `SetupApplications.ps1` does.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Falha da ligação com o PowerShell com um erro: "as credenciais especificadas são inválidas"
#### <a name="problem"></a>Problema
Quando utilizar o PowerShell para ligar ao cluster utilizando o modo de segurança "AzureActiveDirectory", depois de iniciar sessão com êxito para o Azure AD, a ligação falhar com um erro: "as credenciais especificadas são inválidas."

#### <a name="solution"></a>Solução
Esta solução é idêntica à anterior.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer devolve uma falha ao iniciar sessão: "AADSTS50011"
#### <a name="problem"></a>Problema
Ao tentar iniciar sessão Azure AD no Service Fabric Explorer, a página devolve uma falha: "AADSTS50011: O endereço de resposta &lt;url&gt; não coincide com os endereços de resposta configurados para a aplicação: &lt;guid&gt;."

![Não corresponde ao endereço de resposta SFX][sfx-reply-address-not-match]

#### <a name="reason"></a>Razão
A aplicação de cluster (web) que representa o Service Fabric Explorer tenta a autenticação no Azure AD e como parte do pedido fornece o URL de retorno de redirecionamento. Mas o URL não está listado na aplicação do Azure AD **URL de resposta** lista.

#### <a name="solution"></a>Solução
Selecione "Registos de aplicação" na página do AAD, selecione a aplicação de cluster e, em seguida, selecione o **URLs de resposta** botão. Na página "URLs de resposta", adicione o URL do Service Fabric Explorer à lista ou substituir um dos itens na lista. Quando tiver terminado, guarde as alterações.

![Url de resposta de aplicação Web][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Ligue o cluster utilizando a autenticação do Azure AD através do PowerShell
Para ligar o cluster do Service Fabric, utilize o seguinte exemplo de comando do PowerShell:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Para saber mais sobre o cmdlet do Connect-ServiceFabricCluster, consulte [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Pode reutilizar o mesmo inquilino do Azure AD em vários clusters?
Sim. Mas não se esqueça de adicionar o URL do Service Fabric Explorer à sua aplicação de cluster (web). Caso contrário, o Service Fabric Explorer não funciona.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Por que motivo é ainda necessário um certificado de servidor ao Azure AD está ativado?
FabricClient e FabricGateway executará uma autenticação mútua. Durante a autenticação do Azure AD, a integração do Azure AD fornece uma identidade do cliente para o servidor e o certificado de servidor é utilizado para verificar a identidade do servidor. Para obter mais informações sobre certificados de Service Fabric, consulte [certificados x. 509 e o Service Fabric][x509-certificates-and-service-fabric].

## <a name="next-steps"></a>Passos Seguintes
Neste momento, tiver um cluster seguro com fornecer autenticação de gestão do Azure Active Directory. Em seguida, [ligar ao cluster](service-fabric-connect-to-secure-cluster.md) e saber como [gerir segredos de aplicação](service-fabric-application-secret-management.md).


<!-- Links -->
[azure-powershell]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric
[customize-your-cluster-template]: service-fabric-cluster-creation-via-arm.md#create-a-service-fabric-cluster-resource-manager-template

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[select-tenant-button]: ./media/service-fabric-cluster-creation-via-arm/select-tenant-button.png
[users-and-groups-tab]: ./media/service-fabric-cluster-creation-via-arm/users-and-groups-tab.png
[assign-users-to-roles-page]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-page.png
[assign-users-to-roles-confirm]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-confirm.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png


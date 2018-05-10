---
title: Implementar um cluster do Service Fabric protegido na pilha do Azure | Microsoft Docs
description: Saiba como implementar um cluster do Service Fabric protegido na pilha do Azure
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/08/2018
ms.author: mattbriggs
ms.reviewer: shnatara
ms.openlocfilehash: a88d8dd2af94ac796a3b2e3c667fd40a308f02a1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="deploy-a-service-fabric-cluster-in-azure-stack"></a>Implementar um cluster do Service Fabric na pilha do Azure

Utilize o **Cluster do Service Fabric** item no Azure Marketplace para implementar um cluster do Service Fabric protegido na pilha do Azure. 

Para obter mais informações sobre como trabalhar com recursos de infraestrutura de serviço, consulte [descrição geral do Azure Service Frabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) e [cenários de segurança de cluster do Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security), na documentação do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Os seguintes são necessários para implementar o cluster do Service Fabric:
1. **Certificado de cluster**  
   Este é o certificado de servidor de x. 509 que adiciona ao KeyVault quando implementar o Service Fabric. 
   - O **CN** neste certificado tem de coincidir com o nome de domínio completamente qualificado (FQDN) de criar o cluster do Service Fabric. 
   - O formato de certificado tem de ser PFX, como as chaves públicas e privadas são necessárias. 
   Consulte [requisitos](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) para criar este certificado do lado do servidor.

    > [!NOTE]  
    > Pode utilizar inplace um certificado autoassinado do certificado x. 509 servidor para fins de teste. Certificados autoassinados não é necessário corresponder ao FQDN do cluster.

2.  **Certificado de cliente de Admin** este é o certificado que o cliente irá utilizar para autenticação no cluster de Service Fabric, o que pode ser autoassinado. Consulte [requisitos](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) para criar este certificado de cliente.

3.  **Os seguintes itens tem de estar disponíveis no mercado de pilha do Azure:**
     - **Windows Server 2016** – o modelo utiliza a imagem do Windows Server 2016 para criar o cluster.  
     - **A extensão de Script do cliente** -extensão da Máquina Virtual da Microsoft.  
     - **Configuração da fase de pretendido do PowerShell** -extensão da Máquina Virtual da Microsoft.


## <a name="add-a-secret-to-key-vault"></a>Adicionar um segredo ao Key Vault
Para implementar um cluster do Service Fabric, terá de especificar o KeyVault correto *segredo identificador* ou URL para o cluster do Service Fabric. O modelo Azure Resource Manager assume um KeyVault como entrada e, em seguida, obtém o certificado de Cluster durante a instalação do cluster Service Fabric. 

> [!IMPORTANT]  
> Tem de utilizar o PowerShell para adicionar um segredo ao KeyVault para utilização com o Service Fabric. Não utilize o portal.  

Utilize o seguinte script para criar o KeyVault e adicionar o *certificado de cluster* ao mesmo. (Consulte o [pré-requisitos](#prerequisites).) Antes de executar o script, reveja o script de exemplo e atualize os parâmetros indicados para corresponder ao seu ambiente. Este script irão também saída os valores que tem de fornecer para o modelo Azure Resource Manager. 

> [!TIP]  
> Antes do script pode falhar, tem de existir uma oferta pública, que incluem os serviços de computação, rede, armazenamento e de KeyVault. 

  ```PowerShell
    function Get-ThumbprintFromPfx($PfxFilePath, $Password) 
        {
            return New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($PfxFilePath, $Password)
        }
    
    function Publish-SecretToKeyVault ($PfxFilePath, $Password, $KeyVaultName)
       {
            $keyVaultSecretName = "ClusterCertificate"
            $certContentInBytes = [io.file]::ReadAllBytes($PfxFilePath)
            $pfxAsBase64EncodedString = [System.Convert]::ToBase64String($certContentInBytes)
    
            $jsonObject = ConvertTo-Json -Depth 10 ([pscustomobject]@{
                data     = $pfxAsBase64EncodedString
                dataType = 'pfx'
                password = $Password
            })
    
            $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
            $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
            $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
            $keyVaultSecret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $keyVaultSecretName -SecretValue $secret
            
            $pfxCertObject = Get-ThumbprintFromPfx -PfxFilePath $PfxFilePath -Password $Password
    
            Write-Host "KeyVault id: " -ForegroundColor Green
            (Get-AzureRmKeyVault -VaultName $KeyVaultName).ResourceId
            
            Write-Host "Secret Id: " -ForegroundColor Green
            (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $keyVaultSecretName).id
    
            Write-Host "Cluster Certificate Thumbprint: " -ForegroundColor Green
            $pfxCertObject.Thumbprint
       }
    
    #========================== CHANGE THESE VALUES ===============================
    $armEndpoint = "https://management.local.azurestack.external"
    $tenantId = "your_tenant_ID"
    $location = "local"
    $clusterCertPfxPath = "Your_path_to_ClusterCert.pfx"
    $clusterCertPfxPassword = "Your_password_for_ClusterCert.pfx"
    #==============================================================================
    
    Add-AzureRmEnvironment -Name AzureStack -ARMEndpoint $armEndpoint
    Login-AzureRmAccount -Environment AzureStack -TenantId $tenantId
    
    $rgName = "sfvaultrg"
    Write-Host "Creating Resource Group..." -ForegroundColor Yellow
    New-AzureRmResourceGroup -Name $rgName -Location $location
    
    Write-Host "Creating Key Vault..." -ForegroundColor Yellow
    $Vault = New-AzureRmKeyVault -VaultName sfvault -ResourceGroupName $rgName -Location $location -EnabledForTemplateDeployment -EnabledForDeployment -EnabledForDiskEncryption
    
    Write-Host "Publishing certificate to Vault..." -ForegroundColor Yellow
    Publish-SecretToKeyVault -PfxFilePath $clusterCertPfxPath -Password $clusterCertPfxPassword -KeyVaultName $vault.VaultName
   ``` 


Para obter mais informações, consulte [gerir KeyVault na pilha do Azure com o PowerShell](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-kv-manage-powershell).

## <a name="deploy-the-marketplace-item"></a>Implementar o item do Marketplace

1. No portal de utilizador, aceda a **novo** > **computação** > **Cluster do Service Fabric**. 

   ![Selecione o Cluster do Service Fabric](./media/azure-stack-solution-template-service-fabric-cluster/image2.png)

2. Para cada página, tal como *Noções básicas*, preencha o formulário de implementação. Se não tiver a certeza de um valor, utilize as predefinições. Selecione **OK** para avançar para a página seguinte:

   ![Noções básicas](media/azure-stack-solution-template-service-fabric-cluster/image3.png)

3. No *as definições de rede* página, pode especificar portas específicas para abrir as suas aplicações:

   ![Definições de rede](media/azure-stack-solution-template-service-fabric-cluster/image4.png)

4. No *segurança* página, adicione os valores que recebeu do [criar o KeyVault Azure](#add-a-secret-to-key-vault) e carregar o segredo.

   Para o *Thumbprint do certificado de cliente de Admin*, introduza o thumbprint do *certificado de cliente de Admin*. (Consulte o [pré-requisitos](#prerequisites).)
   
   - O Cofre de chaves de origem: Especificar todo *keyVault id* cadeia dos resultados de script. 
   - URL de certificado de cluster: Especifique o URL completo do *segredo Id* dos resultados de script. 
   - Thumbprint do certificado de cluster: Especifique o *Thumbprint do certificado de Cluster* dos resultados de script.
   - Thumbprints de certificado de cliente de administrador: Especifique o *Thumbprint do certificado de cliente de Admin* que criou os pré-requisitos. 

   ![Resultado do script](media/azure-stack-solution-template-service-fabric-cluster/image5.png)

   ![Segurança](media/azure-stack-solution-template-service-fabric-cluster/image6.png)

5. Conclua o assistente e, em seguida, selecione **criar** para implementar o Cluster do Service Fabric.



## <a name="access-the-service-fabric-cluster"></a>Acesso de Cluster do Service Fabric
Pode aceder ao cluster de Service Fabric, utilizando o Service Fabric Explorer ou o PowerShell de Service Fabric.


### <a name="use-service-fabric-explorer"></a>Utilize o Explorador de recursos de infraestrutura de serviço
1.  Confirme que o browser tem acesso ao seu certificado de cliente de administrador e pode autenticar-se ao cluster do Service Fabric.  

    a. Abra o Internet Explorer e aceda a **opções da Internet** > **conteúdo** > **certificados**.
  
    b. Em certificados, selecione **importação** para iniciar o *Assistente para importar certificados*e, em seguida, clique em **seguinte**. No *ficheiro a importar* página clique **procurar**e selecione o **certificado de cliente de Admin** fornecido para o modelo Azure Resource Manager.
        
       > [!NOTE]  
       > Este certificado não é o certificado de Cluster que foi previamente adicionado ao KeyVault.  

    c. Certifique-se de que tem "Personal Information Exchange" selecionado na lista pendente de extensão da janela do Explorador de ficheiros.  

       ![Troca de informações pessoais](media/azure-stack-solution-template-service-fabric-cluster/image8.png)  

    d. No *arquivo de certificados* página, selecione **pessoais**e, em seguida, conclua o assistente.  
       ![Arquivo de certificados](media/azure-stack-solution-template-service-fabric-cluster/image9.png)  
2. Para localizar o FQDN do cluster do Service Fabric:  

    a. Vá para o grupo de recursos que está associado a sua infraestrutura de serviço de cluster e localize o *endereço IP público* recursos. Selecione o objeto associado com o endereço IP público para abrir o *endereço IP público* painel.  

      ![Endereço IP público](media/azure-stack-solution-template-service-fabric-cluster/image10.png)   

    b. No painel de endereço IP público, o FQDN indicará *nome DNS*.  

      ![Nome DNS](media/azure-stack-solution-template-service-fabric-cluster/image11.png)  

3. Para encontrar o URL para o Service Fabric Explorer e o ponto final de ligação de cliente, reveja os resultados da implementação do modelo.

4. No seu browser, aceda a https://*FQDN*: 19080. Substitua *FQDN* com o FQDN do cluster do Service Fabric do passo 2.   
   Se utilizou um certificado autoassinado, obterá um aviso que a ligação não está protegida. Para continuar para o web site, selecione **informações mais**e, em seguida, **avance para a página Web**. 

5. Para autenticar para o site tem de selecionar um certificado a utilizar. Selecione **mais opções**, escolha o certificado adequado e, em seguida, clique em **OK** para ligar ao Service Fabric Explorer. 

   ![Autenticar](media/azure-stack-solution-template-service-fabric-cluster/image14.png)



## <a name="use-service-fabric-powershell"></a>Utilizar o PowerShell de Service Fabric

1. Instalar o *SDK do Microsoft Azure Service Fabric* de [preparar o ambiente de desenvolvimento Windows](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started#install-the-sdk-and-tools) na documentação do Azure Service Fabric.  

2. Depois de concluída a instalação, configure as variáveis de ambiente de sistema para se certificar de que os cmdlets do Service Fabric estão acessíveis a partir do PowerShell.  
    
    a. Aceda a **painel de controlo** > **sistema e segurança** > **sistema**e, em seguida, selecione **definições de sistemaAvançadas**.  
    
      ![Painel de controlo](media/azure-stack-solution-template-service-fabric-cluster/image15.png) 

    b. No **avançadas** separador de *propriedades do sistema*, selecione **variáveis de ambiente**.  

    c. Para *variáveis do sistema*, editar **caminho** e certifique-se de que **c:\\os ficheiros de programa\\Microsoft Service Fabric\\bin\\recursos de infraestrutura \\Fabric.Code** está no topo da lista de variáveis de ambiente.  

      ![Lista de variáveis de ambiente](media/azure-stack-solution-template-service-fabric-cluster/image16.png)

3. Depois de alterar a ordem das variáveis de ambiente, reinicie o PowerShell e, em seguida, execute o seguinte script de PowerShell para obterem acesso ao cluster do Service Fabric:

   ````PowerShell  
    Connect-ServiceFabricCluster -ConnectionEndpoint "\[Service Fabric
    CLUSTER FQDN\]:19000" \`

    -X509Credential -ServerCertThumbprint
    761A0D17B030723A37AA2E08225CD7EA8BE9F86A \`

    -FindType FindByThumbprint -FindValue
    0272251171BA32CEC7938A65B8A6A553AA2D3283 \`

    -StoreLocation CurrentUser -StoreName My -Verbose
   ````
   
   > [!NOTE]  
   > Não existe nenhum *https://* antes do nome do cluster no script. É necessária a porta 19000.
 

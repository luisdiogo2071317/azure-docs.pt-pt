---
title: Ligar ao Azure Stack com a CLI | Documentos da Microsoft
description: Saiba como utilizar a interface de linha de comandos para várias plataformas (CLI) para gerir e implementar recursos no Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 6042aa4dd8b26a0986737edc3c89b8e165ae970a
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067708"
---
# <a name="use-api-version-profiles-with-azure-cli-in-azure-stack"></a>Utilizar perfis de versão de API com a CLI do Azure no Azure Stack

Pode seguir os passos neste artigo para definir a Interface de linha de comandos do Azure (CLI) para gerir recursos de Kit de desenvolvimento do Azure Stack a partir de plataformas de cliente de Linux, Mac e Windows.

## <a name="install-cli"></a>Instalar CLI

Inicie sessão na sua estação de trabalho de desenvolvimento e instalar a CLI. O Azure Stack requer a versão 2.0 ou posterior da CLI do Azure. Pode instalá-lo ao utilizar os passos descritos na [instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) artigo. Para verificar se a instalação foi concluída com êxito, abra um terminal ou uma janela de linha de comandos e execute o seguinte comando:

```azurecli
az --version
```

Deverá ver a versão da CLI do Azure e outras bibliotecas dependentes instaladas no seu computador.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Confiar no certificado de raiz de AC do Azure Stack

1. Obter o certificado de raiz da AC do Azure Stack [seu operador do Azure Stack](..\azure-stack-cli-admin.md#export-the-azure-stack-ca-root-certificate) e confiar nele. Para confiar no certificado de raiz de AC do Azure Stack, anexe-o para o certificado de Python existente.

1. Encontre a localização do certificado no seu computador. A localização pode variar dependendo de onde instalou o Python. Precisará ter [pip](https://pip.pypa.io) e o [certifi](https://pypi.org/project/certifi/) módulo instalado. Pode utilizar o seguinte comando de Python de linha de comandos do bash:

  ```bash  
    python -c "import certifi; print(certifi.where())"
  ```

  Tome nota da localização do certificado. Por exemplo, `~/lib/python3.5/site-packages/certifi/cacert.pem`. O caminho específico dependerá seu sistema operacional e a versão do Python que tenha instalado.

### <a name="set-the-path-for-a-development-machine-inside-the-cloud"></a>Definir o caminho para uma máquina de desenvolvimento na cloud

Se estiver a executar o CLI de uma máquina Linux que é criada dentro do ambiente do Azure Stack, execute o seguinte comando de bash com o caminho para o certificado.

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
```

### <a name="set-the-path-for-a-development-machine-outside-the-cloud"></a>Definir o caminho para uma máquina de desenvolvimento fora da nuvem

Se estiver a executar o CLI de uma máquina **fora** o ambiente do Azure Stack:  

1. Tem de configurar [conectividade VPN para o Azure Stack](azure-stack-connect-azure-stack.md).

1. Copie o certificado PEM que recebeu do operador do Azure Stack e tome nota da localização do ficheiro (PATH_TO_PEM_FILE).

1. Execute os comandos seguintes, consoante final no sistema operacional de seu desenvolvimento da estação de trabalho.

#### <a name="linux"></a>Linux

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="macos"></a>macOS

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="windows"></a>Windows

```powershell
$pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

$root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$root.Import($pemFile)

Write-Host "Extracting needed information from the cert file"
$md5Hash    = (Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
$sha1Hash   = (Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
$sha256Hash = (Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

$issuerEntry  = [string]::Format("# Issuer: {0}", $root.Issuer)
$subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
$labelEntry   = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
$serialEntry  = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
$md5Entry     = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
$sha1Entry    = [string]::Format("# SHA1 Fingerprint: {0}", $sha1Hash)
$sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
$certText = (Get-Content -Path $pemFile -Raw).ToString().Replace("`r`n","`n")

$rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
$serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

Write-Host "Adding the certificate content to Python Cert store"
Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

Write-Host "Python Cert store was updated for allowing the azure stack CA root certificate"
```

## <a name="get-the-virtual-machine-aliases-endpoint"></a>Obter o ponto final de aliases de máquina virtual

Antes dos utilizadores podem criar máquinas virtuais com a CLI, tem de contactar o operador do Azure Stack e obter o URI do ponto de final aliases de máquina virtual. Por exemplo, o Azure utiliza o URI seguinte: https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json. O administrador da nuvem deve configurar um ponto de extremidade semelhante para o Azure Stack com as imagens que estão disponíveis no mercado do Azure Stack. Os utilizadores tem de passar o URI do ponto final para o `endpoint-vm-image-alias-doc` parâmetro para o `az cloud register` comando conforme mostrado na próxima seção. 
   

## <a name="connect-to-azure-stack"></a>Ligar ao Azure Stack

Utilize os seguintes passos para ligar ao Azure Stack:

1. Registe o seu ambiente do Azure Stack executando o `az cloud register` comando.
   
   a. Para registar o *cloud administrativa* ambiente, utilize:

      ```azurecli
      az cloud register \ 
        -n AzureStackAdmin \ 
        --endpoint-resource-manager "https://adminmanagement.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".adminvault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

   b. Para registar o *utilizador* ambiente, utilize:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

1. Defina o ambiente do Active Directory com os seguintes comandos.

   a. Para o *cloud administrativa* ambiente, utilize:

      ```azurecli
      az cloud set \
        -n AzureStackAdmin
      ```

   b. Para o *utilizador* ambiente, utilize:

      ```azurecli
      az cloud set \
        -n AzureStackUser
      ```

1. Atualize a configuração do seu ambiente para utilizar o perfil de versão de API específico do Azure Stack. Para atualizar a configuração, execute o seguinte comando:

   ```azurecli
   az cloud update \
     --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Se estiver a executar uma versão do Azure Stack, antes da compilação de 1808, irá precisar usar o perfil da versão de API **2017-03-09-perfil** em vez do perfil da versão de API **2018-03-01-híbrida**.

1. Inicie sessão no seu ambiente do Azure Stack, utilizando o `az login` comando. Pode iniciar sessão no ambiente do Azure Stack como um utilizador ou como um [principal de serviço](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects). 

    * Ambientes de AAD
      * Inicie sessão como um *utilizador*: pode especificar o nome de utilizador e palavra-passe diretamente dentro do `az login` comando ou autenticar com um browser. Precisa fazer a última opção se a sua conta tem a autenticação multifator ativada.

      ```azurecli
      az login \
        -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
      ```

      > [!NOTE]
      > Se a sua conta de utilizador tiver a autenticação multifator ativada, pode utilizar o `az login command` sem fornecer o `-u` parâmetro. Executar o comando dá-lhe um URL e um código que tem de utilizar a autenticação.
   
      * Inicie sessão como um *principal de serviço*: antes de iniciar sessão, [criar um principal de serviço através do portal do Azure](azure-stack-create-service-principals.md) ou a CLI e atribuí-lo uma função. Agora, inicie sessão com o seguinte comando:

      ```azurecli
      az login \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
        --service-principal \
        -u <Application Id of the Service Principal> \
        -p <Key generated for the Service Principal>
      ```
    * Ambientes do AD FS

        * Inicie sessão como um *principal de serviço*: 
          1.    Prepare o ficheiro. pem a ser utilizado para início de sessão principal de serviço.
                * No computador cliente em que o principal foi criado, exporte o certificado de principal de serviço como um pfx com a chave privada (localizado em cert: \CurrentUser\My; o nome do certificado tem o mesmo nome que o principal).

                *   Converta o pfx em pem (utilitário de OpenSSL do uso).

          1.    Início de sessão para a CLI. :
                ```azurecli
                az login --service-principal \
                 -u <Client ID from the Service Principal details> \
                 -p <Certificate's fully qualified name. Eg. C:\certs\spn.pem>
                 --tenant <Tenant ID> \
                 --debug 
                ```

## <a name="test-the-connectivity"></a>Testar a conectividade

Agora que já temos tudo o que configurar, vamos utilizar a CLI para criar recursos no Azure Stack. Por exemplo, pode criar um grupo de recursos para uma aplicação e adicionar uma máquina virtual. Utilize o seguinte comando para criar um grupo de recursos com o nome "MyResourceGroup":

```azurecli
az group create \
  -n MyResourceGroup -l local
```

Se o grupo de recursos é criado com êxito, o comando anterior devolve as seguintes propriedades do recurso recentemente criado:

![Saída de criar grupo de recursos](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Problemas conhecidos
Existem alguns problemas conhecidos que deve ficar atento ao utilizar a CLI no Azure Stack:

 - Como de modo interativo da CLI o `az interactive` comando ainda não é suportado no Azure Stack.
 - Para obter a lista de imagens de máquinas virtuais disponíveis no Azure Stack, utilize o `az vm images list --all` comando, em vez do `az vm image list` comando. Especificar o `--all` opção certifica-se de que a resposta retorna apenas as imagens que estão disponíveis no seu ambiente do Azure Stack.
 - Aliases de imagem de máquina virtual que estão disponíveis no Azure podem não ser aplicáveis para o Azure Stack. Quando utilizar imagens de máquinas virtuais, tem de utilizar o parâmetro URN inteiro (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) em vez do alias da imagem. Este URN tem de corresponder as especificações de imagem como deriva a `az vm images list` comando.

## <a name="next-steps"></a>Passos Seguintes

[Implementar modelos com a CLI do Azure](azure-stack-deploy-template-command-line.md)

[Ativar a CLI do Azure para utilizadores do Azure Stack (operador)](..\azure-stack-cli-admin.md)

[Gerir permissões de utilizador](azure-stack-manage-permissions.md)

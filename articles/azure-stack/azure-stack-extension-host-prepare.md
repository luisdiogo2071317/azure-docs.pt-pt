---
title: Preparar para o anfitrião de extensão para o Azure Stack | Documentos da Microsoft
description: Aprenda a preparar para o anfitrião de extensão que é ativada automaticamente por meio de um pacote de atualização de pilha do Azure de futuro.
services: azure-stack
keywords: ''
author: mattbriggs
ms.author: mabrigg
ms.date: 08/29/2018
ms.topic: article
ms.service: azure-stack
ms.reviewer: thoroet
manager: femila
ms.openlocfilehash: c6d4c5402474023ffc9eb9a5d9016fe8f56928f4
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42889578"
---
# <a name="prepare-for-extension-host-for-azure-stack"></a>Preparar para o anfitrião de extensão para o Azure Stack

Pode utilizar o anfitrião de extensão para o ajudar a proteger do Azure Stack, reduzindo o número de portas de TCP/IP necessárias. Este artigo analisa a preparação do Azure Stack para o anfitrião de extensão, o que é ativado automaticamente por meio de um pacote de atualização de pilha do Azure após a atualização 1808.

## <a name="certificate-requirements"></a>Requisitos de certificado

O anfitrião de extensão implementa dois novos namespaces de domínios para garantir entradas de anfitrião exclusivo para cada extensão do portal. Os espaços de nomes de domínio novo exigem dois certificados de caráter universal adicionais para garantir uma comunicação segura.

A tabela mostra os novos espaços de nomes e os certificados associados:

| Pasta de implementação | Assunto do certificado necessário e os nomes alternativos do requerente (SAN) | Âmbito (por região) | Espaço de nomes do subdomínio |
|-----------------------|------------------------------------------------------------------|-----------------------|------------------------------|
| Anfitrião de extensão de administração | *.adminhosting. \<região >. \<fqdn > (certificados de SSL de caráter universal) | Anfitrião de extensão de administração | adminhosting. \<região >. \<fqdn > |
| Anfitrião de pública de extensão | * .hosting. \<região >. \<fqdn > (certificados de SSL de caráter universal) | Anfitrião de pública de extensão | hospedando. \<região >. \<fqdn > |

Os requisitos de certificado detalhadas podem ser encontrados no [requisitos de certificados de infraestrutura de chaves públicas do Azure Stack](azure-stack-pki-certs.md) artigo.

## <a name="create-certificate-signing-request"></a>Criar pedido de assinatura de certificado

A ferramenta de verificação da preparação do Azure Stack fornece a capacidade de criar um pedido para os dois certificados SSL de novo, obrigatórios de assinatura de certificado. Siga os passos no artigo [geração de pedido de assinatura de certificados do Azure Stack](azure-stack-get-pki-certs.md).

> [!Note]  
> Pode ignorar este passo consoante o seu pedido os certificados SSL.

## <a name="validate-new-certificates"></a>Validar novos certificados

1. Abra o PowerShell com a permissão elevada no anfitrião de ciclo de vida de hardware ou a estação de trabalho de gestão do Azure Stack.
2. Execute o cmdlet seguinte para instalar a ferramenta de verificação de preparação do Azure Stack.
    ```PowerShell  
    Install-Module -Name Microsoft.AzureStack.ReadinessChecker
    ```
3. Execute o seguinte script para criar a estrutura de pasta necessária:

    ```PowerShell  
    New-Item C:\Certificates -ItemType Directory

    $directories = 'ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal', 'Admin extension host', 'Public extension host'

    $destination = 'c:\certificates'

    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```

    > [!Note]  
    > Se implementar com o Azure Active Directory Federated Services (AD FS) os seguintes diretórios tem de ser adicionados ao **$directories** no script: `ADFS`, `Graph`.

4. Execute os seguintes cmdlets para iniciar a verificação de certificado:

    ```PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD -ExtensionHostFeature $true
    ```

5. Coloque o seu certificado (s) nos diretórios apropriados.

6. Verificar a saída e todos os certificados passam todos os testes.


## <a name="import-extension-host-certificates"></a>Importar certificados de anfitrião de extensão

Utilize um computador que pode ligar-se ao ponto final do Azure Stack com privilégios para os passos seguintes. Certifique-se de que tem acesso a novos ficheiros de certificado a partir desse computador.

1. Utilize um computador que pode ligar-se ao ponto final do Azure Stack com privilégios para os passos seguintes. Certifique-se de que aceder para os novos ficheiros de certificado a partir desse computador.
2. Abra o ISE do PowerShell para executar o seguinte script permanece bloqueado
3. Importe o certificado para o alojamento de ponto final. Ajuste o script para corresponder ao seu ambiente.

    ```PowerShell  
    $CertPassword = ConvertTo-SecureString "***" -AsPlainText -Force

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]] $AdminHostingCertContent = [Byte[]](Get-Content c:\certificate\myadminhostingcertificate.pfx -Encoding Byte)

    Invoke-Command -ComputeName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($AdminHostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($AdminHostingCertContent, $CertPassword)
            Import-AdminHostingServiceCert $AdminHostingCertContent $certPassword
    }
    ```

5. Importe o certificado para a ponto final de alojamento de administração.

    ```PowerShell  
    [Byte[]] $HostingCertContent = [Byte[]](Get-Content c:\certificate\myadminhostingcertificate.pfx  -Encoding Byte)

    Invoke-Command -ComputeName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($HostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($HostingCertContent, $CertPassword)
            Import-UserHostingServiceCert $HostingCertContent $certPassword
    }
    ```

### <a name="update-dns-configuration"></a>Atualizar a configuração de DNS

> [!Note]  
> Este passo não é necessário se tiver utilizado a delegação de zona de DNS para a integração do DNS.
Se os registos anfitriões individuais tenham sido configurados para publicar os pontos finais do Azure Stack, terá de criar dois registos de um anfitrião adicional:

| IP | Nome de anfitrião | Tipo |
|----|------------------------------|------|
| \<IP &GT; | Adminhosting. <Region>.<FQDN> | A |
| \<IP &GT; | Hospedando. <Region>.<FQDN> | A |

IPs alocado pode ser obtido com o ponto final com privilégios ao executar o cmdlet **Get-AzureStackStampInformation**.

### <a name="ports-and-protocols"></a>Portas e protocolos

O artigo [integração no datacenter do Azure Stack - publicar pontos de extremidade](azure-stack-integrate-endpoints.md), abrange as portas e protocolos que necessitam de comunicação de entrada para publicar o Azure Stack anteriores à implementação de anfitrião de extensão.

### <a name="publish-new-endpoints"></a>Publicar novos pontos de extremidade

Existem dois novos pontos de extremidade devem ser publicado através da firewall. Os IPs alocado do conjunto VIP público pode ser obtido com o cmdlet **Get-AzureStackStampInformation**.

> [!Note]  
> Efetue esta alteração antes de ativar o anfitrião de extensão. Isso permite que os portais do Azure Stack a estar acessíveis de forma contínua.

| Ponto final (VIP) | Protocolo | Portas |
|----------------|----------|-------|
| AdminHosting | HTTPS | 443 |
| Hospedagem | HTTPS | 443 |

### <a name="update-existing-publishing-rules-post-enablement-of-extension-host"></a>Atualizar as regras de publicação existentes (ativação de postagem de anfitrião de extensão)

> [!Note]  
> O 1808 faz de pacote de atualização do Azure Stack **não** ativar ainda o anfitrião de extensão. Ele permite que a preparar para o anfitrião de extensão ao importar os certificados necessários. Não feche as portas antes de anfitrião de extensão é ativada automaticamente por meio de um pacote de atualização de pilha do Azure após a atualização 1808.

As seguintes portas de ponto final existente tem de ser fechadas nas suas regras de firewall existentes.

> [!Note]  
> Recomenda-se para fechar as portas após a validação com êxito.

| Ponto final (VIP) | Protocolo | Portas |
|----------------------------------------|----------|-------------------------------------------------------------------------------------------------------------------------------------|
| Portal (administrador) | HTTPS | 12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015 |
| Portal (utilizador) | HTTPS | 12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003 |
| O Azure Resource Manager (administrador) | HTTPS | 30024 |
| O Azure Resource Manager (utilizador) | HTTPS | 30024 |

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [integração da Firewall](azure-stack-firewall.md).
- Saiba mais sobre [geração de pedido de assinatura de certificados do Azure Stack](azure-stack-get-pki-certs.md)
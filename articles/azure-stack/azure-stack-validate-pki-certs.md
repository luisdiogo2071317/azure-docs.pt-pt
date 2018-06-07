---
title: Validar os certificados de infraestrutura de chave pública do Azure pilha para a implementação de sistemas de pilha do Azure integrado | Microsoft Docs
description: Descreve como validar os certificados PKI de pilha do Azure para sistemas de pilha do Azure integrado. Abrange utilizando a ferramenta do Verificador de certificado de pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: e381d2ed3c6a972d776dd31f311fcebe2e35823a
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605615"
---
# <a name="validate-azure-stack-pki-certificates"></a>Validar os certificados PKI de pilha do Azure

A ferramenta do Verificador de preparação de pilha do Azure descrita neste artigo está disponível [da galeria do PowerShell](https://aka.ms/AzsReadinessChecker). Pode utilizar a ferramenta para validar o [gerados certificados PKI](azure-stack-get-pki-certs.md) adequadas para a pré-implementação. Deve validar o certificado com a opção tempo suficiente para testar e volte a emitir certificados, se necessário.

A ferramenta de preparação verificador efetua as seguintes validações de certificado:

- **Ler PFX**  
    Verifica a existência do ficheiro PFX válido, palavra-passe correta e avisa se as informações públicas não estão protegidas pela palavra-passe. 
- **Algoritmo de assinatura**  
    Verifica se o algoritmo de assinatura não é SHA1.
- **Chave privada**  
    Verifica se a chave privada está presente e é exportada com o atributo de computador local. 
- **Cadeia de certificados**  
    Verificações de cadeia de certificados está intacta, incluindo uma verificação de certificados autoassinados.
- **Nomes DNS**  
    Verifica a SAN contém nomes DNS relevantes para cada ponto final ou um suporte de caráter universal estiver presente.
- **Utilização de chave**  
    Verifica se a utilização de chave contém a assinatura digital e cifragem de chaves e utilização de chave avançada contém a autenticação de servidor e autenticação de cliente.
- **Tamanho da chave**  
    Verifica se o tamanho da chave de 2048 ou superior.
- **Ordem de cadeia**  
    Verifica a ordem dos outros certificados validar que a ordem está correta.
- **Outros certificados**  
    Certifique-se de que não existem outros certificados tem sido empacotados num PFX que não seja o certificado de folha relevantes e cadeia.
- **Nenhum perfil**  
    Verifica se um novo utilizador pode carregar os dados PFX sem um perfil de utilizador carregado, mimicking o comportamento de gMSA contas durante a manutenção do certificado.

> [!IMPORTANT]  
> O certificado PKI é um ficheiro PFX e a palavra-passe deve ser tratado como informações confidenciais.

## <a name="prerequisites"></a>Pré-requisitos

O sistema deve cumprir os seguintes pré-requisitos antes de a validar os certificados PKI para uma implementação de pilha do Azure:

- Verificador de preparação de pilha do Microsoft Azure
- Emissor de SSL exportado seguir o [instruções de preparação](azure-stack-prepare-pki-certs.md)
- DeploymentData.json
- Windows 10 ou Windows Server 2016

## <a name="perform-core-services-certificate-validation"></a>Efetuar a validação de certificados de serviços de núcleo

Utilize estes passos para preparar e para validar os certificados PKI de pilha do Azure para implementação e a rotação secreta:

1. Instalar **AzsReadinessChecker** uma linha do PowerShell (5.1 ou acima), executando o seguinte cmdlet:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker -force 
    ````

2. Crie a estrutura de diretórios do certificado. No exemplo abaixo, pode alterar `<c:\certificates>` para um novo caminho de diretório da sua preferência.

    ````PowerShell  
    New-Item C:\Certificates -ItemType Directory
    
    $directories = 'ACSBlob','ACSQueue','ACSTable','ADFS','Admin Portal','ARM Admin','ARM Public','Graph','KeyVault','KeyVaultInternal','Public Portal'
    
    $destination = 'c:\certificates'
    
    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ````
    
    > [!Note]  
    > AD FS e do Graph são necessários se estiver a utilizar o AD FS como o sistema de identidade.
    
     - Coloque o emissor nos diretórios adequados criados no passo anterior. Por exemplo:  
        - `c:\certificates\ACSBlob\CustomerCertificate.pfx`
        - `c:\certificates\Certs\Admin Portal\CustomerCertificate.pfx`
        - `c:\certificates\Certs\ARM Admin\CustomerCertificate.pfx`

3. Na janela do PowerShell, alterar os valores dos **RegionName** e **FQDN** apropriado para o ambiente de pilha do Azure e execute o seguinte:

    ````PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD 

    ````

4. A saída e todos os certificados passaram todos os testes de verificação. Por exemplo:

    ````PowerShell
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Validation

    Starting Azure Stack Certificate Validation 1.1803.405.3
    Testing: ARM Public\ssl.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: ACSBlob\ssl.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Finished Certificate Validation

    AzsReadinessChecker Log location: C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Report location: 
    C:\AzsReadinessChecker\AzsReadinessReport.json
    AzsReadinessChecker Completed
    ````

### <a name="known-issues"></a>Problemas conhecidos

**Sintoma**: testes são ignorados

**Causa**: AzsReadinessChecker ignora são executados determinados testes se não está cumprida uma dependência:

 - Outros certificados são ignorados se falhar de cadeia de certificados.

    ````PowerShell  
    Testing: ACSBlob\singlewildcard.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: Fail
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: Skipped
    Details:
    The certificate records '*.east.azurestack.contoso.com' do not contain a record that is valid for '*.blob.east.azurestack.contoso.com'. Please refer to the documentation for how to create the required certificate file.
    The Other Certificates check was skipped because Cert Chain and/or DNS Names failed. Follow the guidance to remediate those issues and recheck. 
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Finished Certificate Validation

    AzsReadinessChecker Log location: C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Report location (for OEM): C:\AzsReadinessChecker\AzsReadinessChecker.log
    AzsReadinessChecker Completed
    ````

**Resolução**: Siga as orientações a ferramenta na secção detalhes em cada conjunto de testes de cada certificado.

## <a name="perform-platform-as-a-service-certificate-validation"></a>Efetuar plataforma como uma validação de certificado de serviço

Utilize estes passos para preparar e validar os certificados PKI de pilha do Azure para a plataforma como certificados de serviço (PaaS), se estão a ser planeadas implementações SQL/MySQL ou serviços de aplicações.

1.  Instalar **AzsReadinessChecker** uma linha do PowerShell (5.1 ou acima), executando o seguinte cmdlet:

    ````PowerShell  
      Install-Module Microsoft.AzureStack.ReadinessChecker -force
    ````

2.  Crie uma hashtable aninhada que contenha os caminhos e a palavra-passe para cada certificado PaaS necessitar de validação. Na janela do PowerShell a executar:

    ```PowerShell
        $PaaSCertificates = @{
        'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        }
    ```

3.  Alterar os valores dos **RegionName** e **FQDN** para corresponder ao seu ambiente de pilha do Azure para iniciar a validação. Em seguida, execute:

    ```PowerShell
    Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com 
    ```
4.  Certifique-se de que a saída e de que todos os certificados passaram todos os testes.

    ```PowerShell
    AzsReadinessChecker v1.1805.425.2 started
    Starting PaaS Certificate Validation
    
    Starting Azure Stack Certificate Validation 1.0 
    Testing: PaaSCerts\wildcard.appservice.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: PaaSCerts\api.appservice.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: PaaSCerts\wildcard.dbadapter.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: PaaSCerts\sso.appservice.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
    ```

## <a name="using-validated-certificates"></a>Utilização de certificados validados

Depois dos certificados tem foram validados pelo AzsReadinessChecker, está pronto para utilizá-los na sua implementação de pilha do Azure ou para rotação secreta pilha do Azure. 

 - Para a implementação, transferir de forma segura os certificados para o seu engenheiro de implementação para que pode copiá-los para o anfitrião de implementação, conforme especificado no [documentação de requisitos do Azure pilha PKI](azure-stack-pki-certs.md).
 - Para rotação secreta, pode utilizar os certificados para atualizar certificados antigos para pontos finais de infraestrutura público do seu ambiente de pilha do Azure, seguindo o [documentação do Azure pilha segredo rotação](azure-stack-rotate-secrets.md).
 - Para os serviços de PaaS, pode utilizar os certificados para instalar o SQL Server, MySQL e fornecedores de recursos de serviços de aplicação na pilha do Azure, seguindo o [descrição geral da oferta de serviços na documentação do Azure pilha](azure-stack-offer-services-overview.md).

## <a name="next-steps"></a>Passos Seguintes

[Integração de identidade do Centro de dados](azure-stack-integrate-identity.md)
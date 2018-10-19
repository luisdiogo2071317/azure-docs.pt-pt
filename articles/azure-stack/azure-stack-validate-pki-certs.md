---
title: Validar certificados de infraestrutura de chave pública do Azure Stack para a implementação de sistemas integrados do Azure Stack | Documentos da Microsoft
description: Descreve como validar os certificados PKI de pilha do Azure para os sistemas integrados do Azure Stack. Explica como utilizar a ferramenta de verificação de certificado do Azure Stack.
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
ms.date: 09/26/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 8c89c49c1ea88937ba2494a4d9ee7adfa38ea1c1
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408970"
---
# <a name="validate-azure-stack-pki-certificates"></a>Validar certificados PKI de pilha do Azure

A ferramenta do Verificador de preparação do Azure Stack descrita neste artigo está disponível [da galeria do PowerShell](https://aka.ms/AzsReadinessChecker). Pode usar a ferramenta para validar que o [gerados certificados PKI](azure-stack-get-pki-certs.md) são adequadas para pré-implementação. Deve validar certificados deixando tempo suficiente para testar e volte a emitir certificados, se necessário.

A ferramenta de verificação de preparação da realiza as seguintes validações de certificado:

- **Leia o PFX**  
    Verifica a existência de ficheiro PFX válido, palavra-passe correta e avisa se informações públicas não estiver protegidas por palavra-passe. 
- **Algoritmo de assinatura**  
    Verifica se o algoritmo de assinatura não é SHA1.
- **Chave privada**  
    Verifica se a chave privada está presente e é exportada com o atributo de computador local. 
- **Cadeia de certificado**  
    Verificações de cadeia de certificados é intacta, incluindo uma verificação de certificados autoassinados.
- **Nomes DNS**  
    Verifica a SAN contém nomes DNS relevantes para cada ponto de extremidade ou se um suporte de caráter universal está presente.
- **Utilização de chave**  
    Verifica se a utilização da chave contém uma assinatura digital e a codificação de chave e utilização de chave avançada contém a autenticação de servidor e autenticação de cliente.
- **Tamanho da chave**  
    Verifica se o tamanho da chave é 2048 ou superior.
- **Ordem da cadeia**  
    Verifica a ordem dos outros certificados a validar que a ordem é correta.
- **Outros certificados**  
    Certifique-se de que não existem outros certificados tem sido empacotados no PFX que não seja o certificado de folha relevantes e sua cadeia.
- **Nenhum perfil**  
    Verifica-se de que um novo utilizador pode carregar os dados PFX sem um perfil de usuário carregado, imitando o comportamento de gMSA contas durante a manutenção de certificado.

> [!IMPORTANT]  
> O certificado PKI é um ficheiro PFX e palavras-passe deve ser tratado como informações confidenciais.

## <a name="prerequisites"></a>Pré-requisitos

O sistema deve cumprir os seguintes pré-requisitos antes de validação de certificados PKI para uma implementação do Azure Stack:

- Verificador de preparação do Microsoft Azure Stack
- Certificados SSL exportados seguindo o [instruções de preparação](azure-stack-prepare-pki-certs.md)
- DeploymentData.json
- Windows 10 ou Windows Server 2016

## <a name="perform-core-services-certificate-validation"></a>Executar a validação de certificado de serviços de núcleo

Utilize estes passos para preparar e para validar os certificados de PKI do Azure Stack para implementação e a rotação secreta:

1. Instale **AzsReadinessChecker** uma linha do PowerShell (5.1 ou acima), ao executar o seguinte cmdlet:

    ```PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker -force 
    ```

2. Crie a estrutura de diretórios do certificado. No exemplo abaixo, pode alterar `<c:\certificates>` para um novo caminho de diretório da sua preferência.
    ```PowerShell  
    New-Item C:\Certificates -ItemType Directory
    
    $directories = 'ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal'
    
    $destination = 'c:\certificates'
    
    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```
    
    > [!Note]  
    > AD FS e o gráfico são necessários se estiver a utilizar o AD FS como seu sistema de identidade.
    
     - Coloque o seu certificado (s) nos diretórios apropriados criados no passo anterior. Por exemplo:  
        - `c:\certificates\ACSBlob\CustomerCertificate.pfx`
        - `c:\certificates\Certs\Admin Portal\CustomerCertificate.pfx`
        - `c:\certificates\Certs\ARM Admin\CustomerCertificate.pfx`

3. Na janela do PowerShell, alterar os valores dos **RegionName** e **FQDN** adequados para o ambiente do Azure Stack e execute o seguinte:

    ```PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Start-AzsReadinessChecker  -extensionhostfeature -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD 
    ```

4. Verificar a saída e todos os certificados passam todos os testes. Por exemplo:

    ```PowerShell  
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
    ```

### <a name="known-issues"></a>Problemas conhecidos

**Sintoma**: testes são ignorados

**Causa**: AzsReadinessChecker ignora determinados testes se não for cumprida uma dependência:

 - Outros certificados são ignorados se falhar de cadeia de certificados.

    ```PowerShell  
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
    ```

**Resolução**: Siga a orientação da ferramenta na secção de detalhes em cada conjunto de testes para cada certificado.

## <a name="perform-platform-as-a-service-certificate-validation"></a>Efetuar a plataforma como uma validação de certificado de serviço

Utilize estes passos para preparar e validar os certificados PKI de pilha do Azure para a plataforma como certificados de serviço (PaaS), se as implementações de SQL/MySQL ou dos serviços de aplicações estão a ser planeadas.

1.  Instale **AzsReadinessChecker** uma linha do PowerShell (5.1 ou acima), ao executar o seguinte cmdlet:

    ```PowerShell  
      Install-Module Microsoft.AzureStack.ReadinessChecker -force
    ```

2.  Crie uma tabela de hash aninhada que contém os caminhos e a palavra-passe para cada certificado de PaaS que precisam de validação. Na janela do PowerShell a executar:

    ```PowerShell  
        $PaaSCertificates = @{
        'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        }
    ```

3.  Os valores dos **RegionName** e **FQDN** para corresponder ao seu ambiente do Azure Stack para iniciar a validação. Em seguida, execute:

    ```PowerShell  
    Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com 
    ```
4.  Verifique a saída e que todos os certificados passam todos os testes.

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

Assim que os certificados foram validados pelo AzsReadinessChecker, está pronto para usá-los na sua implementação do Azure Stack ou para a rotação de segredo do Azure Stack. 

 - Para a implementação, transfere com segurança os certificados para o engenheiro de implementação para que eles podem copiá-los em anfitrião conforme especificado na implementação do [documentação de requisitos do Azure Stack PKI](azure-stack-pki-certs.md).
 - Para a rotação de segredo, pode utilizar os certificados para atualizar certificados antigos para pontos finais de infra-estrutura público do seu ambiente do Azure Stack ao seguir a [documentação de rotação de segredo do Azure Stack](azure-stack-rotate-secrets.md).
 - Para os serviços de PaaS, pode utilizar os certificados para instalar o SQL, MySQL e fornecedores de recursos de serviços de aplicação no Azure Stack ao seguir a [descrição geral da oferta de serviços na documentação do Azure Stack](azure-stack-offer-services-overview.md).

## <a name="next-steps"></a>Passos Seguintes

[Integração de identidade do Centro de dados](azure-stack-integrate-identity.md)
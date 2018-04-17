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
ms.date: 04/11/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: cd917165804314f6ee4ee006e3f29263d8d4b4c5
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
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

> [!IMPORTANT]  
> O certificado PKI é um ficheiro PFX e a palavra-passe deve ser tratado como informações confidenciais.

## <a name="prerequisites"></a>Pré-requisitos

O sistema deve cumprir os seguintes pré-requisitos antes de a validar os certificados PKI para uma implementação de pilha do Azure:

- Verificador de preparação de pilha do Microsoft Azure
- Emissor de SSL exportado seguir o [instruções de preparação](azure-stack-prepare-pki-certs.md)
- DeploymentData.json
- Windows 10 ou Windows Server 2016

## <a name="perform-certificate-validation"></a>Efetuar a validação de certificado

Utilize estes passos para preparar e para validar os certificados PKI de pilha do Azure:

1. Instale AzsReadinessChecker a partir de uma linha de comandos PowerShell (5.1 ou acima), executando o seguinte cmdlet:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker 
    ````

2. Crie a estrutura de diretórios do certificado. No exemplo abaixo, pode alterar `<c:\certificates>` para um novo caminho de diretório da sua preferência.

    ````PowerShell  
    New-Item C:\Certificates -ItemType Directory

    $directories = 'ACSBlob','ACSQueue','ACSTable','ADFS','Admin Portal','ARM Admin','ARM Public','Graph','KeyVault','KeyVaultInternal','Public Portal' 

    $destination = 'c:\certificates' 

    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}  
    ````

 - Coloque o emissor nos diretórios adequados criados no passo anterior. Por exemplo:  
    - c:\certificates\ACSBlob\CustomerCertificate.pfx 
    - c:\certificates\Certs\Admin Portal\CustomerCertificate.pfx 
    - c:\certificates\Certs\ARM Admin\CustomerCertificate.pfx 
    - e assim sucessivamente... 

3. Na janela do PowerShell a executar:

    ````PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
    ````

4. Reveja o resultado para Certifique-se de que todos os certificados passaram os testes. Por exemplo:

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
    AzsReadinessChecker Report location (for OEM): C:\AzsReadinessChecker\AzsReadinessReport.json
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

## <a name="using-validated-certificates"></a>Utilização de certificados validados

Depois dos certificados tem foram validados pelo AzsReadinessChecker, está pronto para utilizá-los na sua implementação de pilha do Azure ou para rotação secreta pilha do Azure. 

 - Para a implementação, transferir de forma segura os certificados para o seu engenheiro de implementação para que pode copiá-los para o anfitrião de implementação, conforme especificado no [documentação de requisitos do Azure pilha PKI](azure-stack-pki-certs.md).
 - Para rotação secreta, pode utilizar os certificados para atualizar certificados antigos para pontos finais de infraestrutura público do seu ambiente de pilha do Azure, seguindo o [documentação do Azure pilha segredo rotação](azure-stack-rotate-secrets.md).

## <a name="next-steps"></a>Passos Seguintes

[Integração de identidade do Centro de dados](azure-stack-integrate-identity.md)

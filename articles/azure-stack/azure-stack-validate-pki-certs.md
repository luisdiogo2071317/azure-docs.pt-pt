---
title: Validar os certificados de infraestrutura de chave pública do Azure pilha para a implementação de sistemas de pilha do Azure integrado | Microsoft Docs
description: Descreve como validar os certificados PKI de pilha do Azure para sistemas de pilha do Azure integrado.
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
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 0bdadadb1f4ee5f76cde9d05b11e8d57b99ac191
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="validate-azure-stack-pki-certificates"></a>Validar os certificados PKI de pilha do Azure

A ferramenta Azure pilha certificado verificador descrita neste artigo é fornecida pelo OEM incluído com o ficheiro deploymentdata.json para validar que o [gerados certificados PKI](azure-stack-get-pki-certs.md) adequadas para a pré-implementação. Certificados devem ser validados com tempo suficiente para testar e obter certificados emitido novamente se necessário.

A ferramenta do Verificador de certificado (Certchecker) efetua as seguintes verificações:

- **Ler PFX**. Verifica a existência do ficheiro PFX válido, palavra-passe correta e avisa se as informações públicas não estão protegidas pela palavra-passe. 
- **Algoritmo de assinatura**. Verifica o algoritmo de assinatura não SHA1.
- **Chave privada**. Verifica a chave privada está presente e é exportada com o atributo de computador Local. 
- **Cadeia de certificados**. Verifica a cadeia de certificados está a ser tact incluindo para certificados autoassinados. 
- **Nomes DNS**. Verifica a SAN contém nomes DNS relevantes para cada ponto final ou um suporte de caráter universal estiver presente. 
- **Utilização da chave**. Verifica a utilização de chave contém a assinatura Digital e a encriptação da chave e utilização de chave avançada contém a autenticação de servidor e autenticação de cliente.
- **Tamanho da chave**. Verifica o tamanho de chave de 2048 ou superior.
- **Ordem de encadear**. Verifica a ordem dos outros certificados efetuar cadeia está correta.
- **Outros certificados**. Certifique-se de que não existem outros certificados tem sido empacotados num PFX que não seja o certificado de folha relevantes e cadeia.
- **Nenhum perfil**. Verifica que um novo utilizador pode carregar os dados PFX sem um perfil de utilizador carregado, mimicking o comportamento de gMSA contas durante a manutenção do certificado.

> [!IMPORTANT]  
> O ficheiro PFX de certificado PKI e a palavra-passe devem ser tratados como informações confidenciais.

## <a name="prerequisites"></a>Pré-requisitos
O sistema deve cumprir os seguintes pré-requisitos antes de a validar os certificados PKI para a implementação de pilha do Azure:
- CertChecker (no **PartnerToolKit** em **\utils\certchecker**)
- Emissor de SSL exportado seguir o [instruções de preparação](azure-stack-prepare-pki-certs.md)
- DeploymentData.json
- Windows 10 ou Windows Server 2016

## <a name="perform-certificate-validation"></a>Efetuar a validação de certificado

Utilize estes passos para preparar e validar os certificados PKI de pilha do Azure: 

1. Extraia os conteúdos do <partnerToolkit>\utils\certchecker para um novo diretório, por exemplo, **c:\certchecker**.

2. Abra o PowerShell como administrador e altere o diretório para a pasta de certchecker:

  ```powershell
  cd c:\certchecker
  ```
 
3. Crie uma estrutura de diretório para os certificados, executando os seguintes comandos do PowerShell:

  ```powershell 
  $directories = "ACS","ADFS","Admin Portal","ARM Admin","ARM Public","Graph","KeyVault","KeyVaultInternal","Public Portal" 
  $destination = '.\certs' 
  $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}  
  ```

  >  [!NOTE]
  >  Se o fornecedor de identidade para a implementação de pilha do Azure do Azure AD, remova o **ADFS** e **gráfico** diretórios. 

4. Coloque o emissor nos diretórios adequados criados no passo anterior, por exemplo: 
  - c:\certchecker\Certs\ACS\CustomerCertificate.pfx,  
  - c:\certchecker\Certs\Admin Portal\CustomerCertificate.pfx  
  - c:\certchecker\Certs\ARM Admin\CustomerCertificate.pfx  
  - e assim sucessivamente... 

5. Cópia **deploymentdata.json** para o **c:\certchecker** diretório.

6. Na janela do PowerShell, execute os seguintes comandos: 

  ```powershell
  $password = Read-Host -Prompt "Enter PFX Password" -AsSecureString 
  .\CertChecker.ps1 -CertificatePath .\Certs\ -pfxPassword $password -deploymentDataJSONPath .\DeploymentData.json  
  ```

7. O resultado deve conter OK para todos os certificados e todos os atributos marcados: 

  ```powershell
  Starting Azure Stack Certificate Validation 1.1802.221.1
  Testing: ADFS\ContosoSSL.pfx
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Testing: KeyVaultInternal\ContosoSSL.pfx
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Testing: ACS\ContosoSSL.pfx
  WARNING: Pre-1803 certificate structure. The folder structure for Azure Stack 1803 and above is: ACSBlob, ACSQueue, ACSTable instead of ACS folder. Refer to deployment documentation for further informat
  ion.
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Detailed log can be found C:\CertChecker\CertChecker.log 
  ```

### <a name="known-issues"></a>Problemas conhecidos 
**Sintoma**: Certchecker sai prematuramente e receber o erro seguinte: 
> Com Falhas

> Detalhes: Não é possível executar este comando devido ao erro: O nome de diretório é inválido. 

**Causa**: executar certchecker.ps1 a partir de uma pasta restritiva, por exemplo, c:\temp ou % temp % 

**Resolução**: mover a ferramenta de certchecker para o novo diretório, por exemplo, C:\CertChecker 


**Sintoma**: Certchecker fornece um aviso sobre a utilização de Pre-1803 (como no exemplo acima do passo 7):

> [!WARNING]
> Estrutura do certificado de Pre-1803. A pasta de estrutura para 1803 de pilha do Azure e posterior está: ACSTable ACSBlob, ACSQueue, em vez da pasta de ACS. Para obter mais informações, consulte a documentação de implementação.

**Causa**: CertChecker detetou a utilização de uma única pasta de ACS, isto é correto para implementações antes 1803. Para a versão de pilha do Azure 1803 e acima implementações, a estrutura da pasta alterações ACSTable, ACSQueue, ACSBlob. Certchecker já tem de ser atualizado para suportar esta funcionalidade.

**Resolução**: Se implementar 1802, é necessária nenhuma ação. Se implementar 1803 acima, substitua o ACS ACSTable, ACSQueue, ACSBlob e copie o emissor de ACS para essas pastas.

**Sintoma**: testes são ignorados

**Causa**: CertChecker ignora são executados determinados testes se não está cumprida uma dependência:
- Outros certificados são ignorados se falhar de cadeia de certificados.
- Nenhum perfil é ignorada se:
  - Há uma política de segurança restringir a capacidade de criar um utilizador temporário e executar o powershell como de que o utilizador.
  - Falha de verificação de chave privada.

**Resolução**: Siga as orientações de ferramentas na secção detalhes em conjunto de cada certificado de testes.


## <a name="prepare-deployment-script-certificates"></a>Preparar a implementação de certificados de script 
Como passo final, todos os certificados que já preparou tem de ser colocado nos diretórios no anfitrião de implementação adequados. No seu anfitrião de implementação, crie uma pasta designada. * * De certificados e coloque o certificado exportado ficheiros nas subpastas correspondentes especificadas no [certificados obrigatórios](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates) secção:

```
\Certificates
\ACS\ssl.pfx
\Admin Portal\ssl.pfx
\ARM Admin\ssl.pfx
\ARM Public\ssl.pfx
\KeyVault\ssl.pfx
\KeyVaultInternal\ssl.pfx
\Public Portal\ssl.pfx
\ADFS\ssl.pfx*
\Graph\ssl.pfx*
```

<sup>*</sup> Os certificados marcados com asterisco * apenas são necessárias quando o AD FS é utilizado como o arquivo de identidade.


## <a name="next-steps"></a>Passos Seguintes
[Integração de identidade do Centro de dados](azure-stack-integrate-identity.md)

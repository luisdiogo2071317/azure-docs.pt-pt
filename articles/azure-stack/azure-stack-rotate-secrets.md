---
title: Roda os segredos na pilha do Azure | Microsoft Docs
description: Saiba como rodar os segredos na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 49071044-6767-4041-9EDD-6132295FA551
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 509570dfe0e3d4be2e589ac1958dd377dc4e8e03
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>Roda os segredos na pilha do Azure

*Estas instruções aplicam-se apenas ao Azure pilha integrada sistemas versão 1803 e mais tarde. Não tente rotação secreta em versões de pilha do pre-1802 do Azure*

Pilha do Azure utiliza vários segredos para manter a comunicação segura entre os serviços e recursos de infraestrutura de pilha do Azure.

- **Segredos internos**  
Todos os os certificados, as palavras-passe, cadeias seguras e as chaves utilizadas pela infraestrutura do Azure pilha sem a intervenção do operador de pilha do Azure. 

- **Segredos externos**  
Certificados de infraestrutura de serviço para os serviços de acesso externo que são fornecidos pela operadora de rede de pilha do Azure. Isto inclui os certificados para os seguintes serviços: 
    - Portal de administrador 
    - Portal público 
    - Administrador do Azure Resource Manager 
    - Gestor de recursos global do Azure 
    - Administrador Keyvault 
    - Keyvault 
    - ACS (incluindo o blob, tabela e armazenamento de filas) 
    - ADFS<sup>*</sup>
    - Gráfico<sup>*</sup>

    > <sup>*</sup> Apenas aplicável se o fornecedor de identidade do ambiente Active Directory Federated Services (AD FS).

> [!NOTE]
> Todos os outros proteger chaves e cadeias, incluindo BMC e mudar as palavras-passe, utilizador e administrador as palavras-passe de conta são atualizadas manualmente pelo administrador. 

Para poder manter a integridade da infraestrutura de pilha do Azure, os operadores necessita da capacidade de periodicamente rodar os segredos da respetiva infraestrutura em frequências que são consistentes com os requisitos de segurança da respetiva organização.

## <a name="alert-remediation"></a>Remediação de alerta

Quando são segredos no prazo de 30 dias de expiração, os seguintes alertas são gerados no Portal do administrador: 

- Expiração de palavra-passe da conta de serviço pendente 
- Expiração do certificado interno pendente 
- Expiração do certificado externo pendente 

Executar rotação secreta utilizando as instruções abaixo irá remediar estes alertas.

## <a name="pre-steps-for-secret-rotation"></a>Pré-passos para rotação secreta

1.  Notifique os utilizadores de quaisquer operações de manutenção. Agende janelas de manutenção normal, quanto possíveis, durante o horário não comercial. Operações de manutenção podem afetar as cargas de trabalho de utilizador e operações portais.

    > [!note]  
    > Os passos seguintes aplicam-se apenas quando a rotação dos segredos externos pilha do Azure.

2.  Prepare um novo conjunto de substituição certificados externos. O novo conjunto corresponde as especificações de certificado descritas no [requisitos de certificado PKI de pilha do Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs).
3.  Armazene uma cópia de segurança nos certificados utilizados para rotação numa localização segura cópia de segurança. Se a rotação é executado e, em seguida, falha, substitua os certificados na partilha de ficheiros com as cópias de segurança antes de voltar a executar a rotação. Tenha em atenção, manter cópias de segurança na localização de cópia de segurança segura.
3.  Crie uma partilha de ficheiros que pode aceder a partir de VMs ERCS. A partilha de ficheiros tem de ser legível e gravável para o **CloudAdmin** identidade.
4.  Abra uma consola do ISE do PowerShell a partir de um computador onde tenha acesso à partilha de ficheiros. Navegue para a partilha de ficheiros. 
5.  Executar **[CertDirectoryMaker.ps1](http://www.aka.ms/azssecretrotationhelper)** para criar os diretórios necessários para os certificados externos.

## <a name="rotating-external-and-internal-secrets"></a>Rodar segredos internos e externos

Para rodar os dois externo um segredos interno:

1. Dentro do recém-criada **certificados** diretório que criou nos pré-passos de, coloque o novo conjunto de certificados externos de substituição na estrutura de diretório, de acordo com o formato descrito na secção certificados obrigatório do [requisitos de certificado PKI de pilha do Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates).
2. Criar uma sessão do PowerShell com o [ponto final com privilégios](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) utilizando o **CloudAdmin** conta e armazenar as sessões como uma variável. Irá utilizar esta variável como o parâmetro no próximo passo.

    > [!IMPORTANT]  
    > Não introduza a sessão, armazene a sessão como uma variável.
    
3. Executar  **[invoke-command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-5.1)**. Transmitir a variável de sessão do PowerShell com privilégios de ponto final como a **sessão** parâmetro. 
4. Executar **início SecretRotation** com os seguintes parâmetros:
    - **PfxFilesPath**  
    Especifique o caminho de rede para o diretório de certificados que criou anteriormente.  
    - **PathAccessCredential**  
    Um objeto PSCredential as credenciais para a partilha. 
    - **CertificatePassword**  
    Uma cadeia segura a palavra-passe utilizada para todos os ficheiros de certificado pfx criados.
4. Aguarde enquanto o rodar os segredos.  
Quando a rotação secreta é concluída com êxito, a consola irá apresentar **estado geral da ação: êxito**. 
5. Após a conclusão bem-sucedida da rotação secreta, remova os certificados da partilha criada no passo anterior ao e armazená-las numa localização de cópia de segurança segura. 

## <a name="walkthrough-of-secret-rotation"></a>Instruções de rotação secreta

O exemplo do PowerShell seguinte demonstra os cmdlets e parâmetros para executar para rodar os segredos.

```powershell
#Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IPofERCSMachine>"}'
$PEPCreds = Get-Credential 
$PEPsession = New-PSSession -computername <IPofERCSMachine> -Credential $PEPCreds -ConfigurationName PrivilegedEndpoint 

#Run Secret Rotation
$CertPassword = "CertPasswordHere" | ConvertTo-SecureString
$CertShareCred = Get-Credential 
$CertSharePath = <NetworkPathofCertShare>   
Invoke-Command -session $PEPsession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword }
Remove-PSSession -Session $PEPSession
```
## <a name="rotating-only-internal-secrets"></a>Rodar apenas segredos internos

Para rodar apenas do Azure pilha internos segredos:

1. Criar uma sessão do PowerShell com o [ponto final com privilégios](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).
2. Na sessão do ponto final com privilégios, execute **início SecretRotation** sem argumentos.

## <a name="start-secretrotation-reference"></a>Referência de início SecretRotation

Roda os segredos de um sistema de pilha do Azure. Só é executado contra o Endpoint com privilégios de pilha do Azure.

### <a name="syntax"></a>Sintaxe

Caminho (predefinição)

```powershell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential] <PSCredential> [-CertificatePassword <SecureString>]  
```

### <a name="description"></a>Descrição

O cmdlet Start-SecretRotation roda os segredos de infraestrutura de um sistema de pilha do Azure. Por predefinição, roda todos os segredos expostos à rede de infraestrutura interna, com a intervenção do utilizador, também roda os certificados de todos os pontos finais infraestrutura de rede externa. Quando a rotação dos pontos finais de infraestrutura de rede externa, SecretRotation de início deve ser executado através de um bloco de script de Invoke-Command com sessão de ponto final com privilégios o ambiente do Azure pilha transmitido como o parâmetro de sessão.
 
### <a name="parameters"></a>Parâmetros

| Parâmetro | Tipo | Necessário | Posição | Predefinição | Descrição |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | Cadeia  | Falso  | com o nome  | Nenhuma  | O caminho de partilha de ficheiros para o **\Certificates** certificados de ponto final de rede do diretório que contém todos os externo. Só é necessário quando a rotação dos segredos internos e externos. Diretório de fim tem de estar **\Certificates**. |
| CertificatePassword | SecureString | Falso  | com o nome  | Nenhuma  | A palavra-passe para todos os certificados fornecidas - PfXFilesPath. Valor necessário se PfxFilesPath é fornecida quando rodam segredos internos e externos. |
|

### <a name="examples"></a>Exemplos
 
**Roda apenas segredos de infra-estrutura interna**

```powershell  
PS C:\> Start-SecretRotation  
```

Este comando roda todos os segredos de infraestrutura expostos à rede interna de pilha do Azure. Início SecretRotation roda todos os segredos gerado de pilha, mas porque não existem nenhum certificados fornecidos, certificados de ponto final externo não define a rotação.  

**Roda os segredos de infraestrutura internos e externos**
  
```powershell
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword } 
Remove-PSSession -Session $PEPSession
```

Este comando roda todos os segredos de infraestrutura expostos à rede interna de pilha do Azure, bem como os certificados TLS utilizados para pontos finais de infraestrutura de rede externa da pilha do Azure. Início SecretRotation roda todos os segredos gerado de pilha e porque são fornecidos certificados, os certificados de ponto final externo também rotação.  


## <a name="update-the-baseboard-management-controller-bmc-password"></a>Atualizar a palavra-passe do bmc gestão controlador BMC

O controlador de gestão de placas base (BMC) monitoriza o estado dos seus servidores físico. As especificações e instruções sobre como atualizar a palavra-passe do BMC variam com base no seu fornecedor de hardware de fabricante de equipamento original (OEM). Deve ser atualizado as palavras-passe para os componentes de pilha do Azure a uma cadência regular.

1. Atualize o BMC em servidores físicos a pilha do Azure ao seguir as instruções do OEM. A palavra-passe para cada BMC no seu ambiente tem de ser o mesmo.
2. Abra um ponto final com privilégios em sessões de pilha do Azure. Para instruções, consulte [utilizando o ponto final com privilégios na pilha de Azure](azure-stack-privileged-endpoint.md).
3. Depois do PowerShell linha foi alterada para **[endereço IP ou ERCS VM nome]: PS >** ou **[azs ercs01]: PS >**, consoante o ambiente, execute `Set-BmcPassword` executando `invoke-command`. A variável de sessão do ponto final com privilégios de passar como parâmetro. Por exemplo:

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```
    
    Também pode utilizar a versão do PowerShell estática com as palavras-passe como linhas de código:
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for exmaple Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais sobre a segurança de pilha do Azure](azure-stack-security-foundations.md)

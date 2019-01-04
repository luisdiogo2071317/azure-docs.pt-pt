---
title: Rodar segredos no Azure Stack | Documentos da Microsoft
description: Saiba como rodar os segredos no Azure Stack.
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
ms.date: 12/18/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 27fd02503881ef1f0587ccb0301f8490101d5bcb
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53720650"
---
# <a name="rotate-secrets-in-azure-stack"></a>Rodar segredos no Azure Stack

*Estas instruções aplicam-se apenas ao Azure Stack integrada sistemas versão 1803 e mais tarde. Não tente secreta rotação em versões de pilha do Azure de pré-1802*

O Azure Stack utiliza várias segredos para manter uma comunicação segura entre os serviços e recursos de infraestrutura do Azure Stack.

- **Segredos internos**  
Todos os os certificados, palavras-passe, cadeias de caracteres seguras e as chaves utilizadas pela infraestrutura do Azure Stack sem a intervenção do operador do Azure Stack. 

- **Segredos externos**  
Certificados de infraestrutura de serviço para os serviços de face externa que são fornecidos pela operadora de rede do Azure Stack. Isto inclui os certificados para os seguintes serviços: 
  - Portal de administrador  
  - Portal público  
  - Administrador do Azure Resource Manager  
  - Global Azure Resource Manager  
  - Cofre de chaves de administrador  
  - Keyvault  
  - Anfitrião de extensão de administração  
  - ACS (incluindo o armazenamento de filas, tabelas e BLOBs)  
  - ADFS *  
  - Gráfico *  

\* Apenas aplicável se o fornecedor de identidade do ambiente é o Active Directory Federated Services (AD FS).

> [!Note]  
> Todos os outros proteger chaves e cadeias de caracteres, incluindo o BMC e alternar as palavras-passe, as senhas de contas de utilizador e administrador são atualizadas manualmente pelo administrador. 

> [!Note]  
> A partir 1811 versão do Azure Stack, rotação secreta foi separada para certificados internos e externos. 

Para manter a integridade da infraestrutura do Azure Stack, operadores tem a capacidade de rodar periodicamente os segredos de sua infra-estrutura em frequências são consistentes com os requisitos de segurança da sua organização.

### <a name="rotating-secrets-with-external-certificates-from-a-new-certificate-authority"></a>Girando segredos com certificados externos de uma nova autoridade de certificação

O Azure Stack suporta a rotação de secreta com certificados externos de um novo certificado de autoridade (AC) nos contextos seguintes:

|Instalado de certificado de AC|Autoridade de certificação para girar para|Suportadas|Versões do Azure Stack suportadas|
|-----|-----|-----|-----|
|De Autoassinado|Para a empresa|Não suportado||
|De Autoassinado|Para Autoassinado|Não suportado||
|De Autoassinado|Para o público<sup>*</sup>|Suportadas|1803 e posterior|
|Do Enterprise|Para a empresa|Suportado, desde que os clientes utilizam a mesma empresa AC como utilizado na implementação|1803 e posterior|
|Do Enterprise|Para Autoassinado|Não suportado||
|Do Enterprise|Para o público<sup>*</sup>|Suportadas|1803 e posterior|
|De público<sup>*</sup>|Para a empresa|Não suportado|1803 e posterior|
|De público<sup>*</sup>|Para Autoassinado|Não suportado||
|De público<sup>*</sup>|Para o público<sup>*</sup>|Suportadas|1803 e posterior|

<sup>*</sup>Indica que as autoridades de certificação pública são aqueles que fazem parte do programa de raiz confiável do Windows. Pode encontrar a lista completa no artigo [Microsoft Trusted Root Certificate Program: Os participantes (a partir de 27 de Junho de 2017)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).

## <a name="alert-remediation"></a>Remediação do alerta

Quando os segredos são dentro de 30 dias de expiração, os seguintes alertas são gerados no Portal do administrador: 

- Expiração de palavra-passe da conta de serviço pendente 
- Expiração do certificado interno pendente 
- Expiração do certificado externo pendente 

Executar a rotação secreta com as instruções abaixo irá remediar estes alertas.

> [!Note]  
> Ambientes de pilha do Azure em versões de pré-1811 poderão ver alertas para o certificado interno ou expirações secretas pendentes. Estes alertas são incorretos e devem ser ignorados sem executar a rotação secreta interna. Os alertas de expiração do segredo interno incorretos são um problema conhecido que é resolvido no 1811 – interno segredos não irão expirar, a menos que o ambiente tem estado ativo por dois anos. 

## <a name="pre-steps-for-secret-rotation"></a>Passos anteriores para a rotação de secreta

   > [!IMPORTANT]  
   > Se rotação segreda já foi executada em seu ambiente do Azure Stack, em seguida, tem de atualizar o sistema para a versão 1811 ou posterior antes de executar novamente a rotação secreta. Rotação secreta tem de ser executada por através da [ponto final com privilégios](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) e requer credenciais de operador do Azure Stack. Se o seu ambiente do Azure Stack Operator(s) não souber se rotação secreta tiver sido executada no seu ambiente, atualize para 1811 antes de executar novamente a rotação secreta.

1. É altamente recomendado que Atualize a instância do Azure Stack para a versão 1811.

    > [!Note] 
    > Para versões de pré-1811 não é necessário girar segredos para adicionar os certificados de anfitrião de extensão. Deve seguir as instruções no artigo [preparar para o anfitrião de extensão para o Azure Stack](azure-stack-extension-host-prepare.md) para adicionar os certificados de anfitrião de extensão.

2.  Operadores poderão reparar alertas abrir e fechar automaticamente durante a rotação de segredos do Azure Stack.  Este comportamento é esperado e os alertas podem ser ignorados.  Operadores podem verificar a validade destes alertas, executando **AzureStack teste**.  Para operadores usando o SCOM para monitorizar sistemas do Azure Stack, colocar um sistema no modo de manutenção impedirá que estes alertas de chegar aos seus sistemas ITSM, mas irão continuar para o alertar se o sistema do Azure Stack fique inacessível. 
3. Notificar os utilizadores de quaisquer operações de manutenção. Agende janelas de manutenção normal, tanto quanto possíveis, durante o horário não comercial. Operações de manutenção podem afetar as cargas de trabalho de utilizador e de operações do portal.

    > [!Note]  
    > Os passos seguintes aplicam-se apenas quando alternar segredos externos do Azure Stack.

4. Execute **[teste AzureStack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test)** e confirme a todos os resultados de teste estão em bom Estados antes de efetuar a rotação de segredos.
5. Prepare um novo conjunto de substituição de certificados externos. O novo conjunto corresponde as especificações do certificado descritas a [requisitos de certificado PKI do Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs). Pode gerar um certificado (CSR) do pedido de assinatura para compra ou a criação de novos certificados utilizando os passos descritos em [gerar certificados de PKI](https://docs.microsoft.com/azure/azure-stack/azure-stack-get-pki-certs) e prepará-los para utilização no seu ambiente do Azure Stack através dos passos [ Preparar os certificados PKI do Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-prepare-pki-certs). Certifique-se de que validar os certificados de preparar-se com os passos descritos em [validar certificados de PKI](https://docs.microsoft.com/azure/azure-stack/azure-stack-validate-pki-certs). 
6.  Store uma cópia de segurança nos certificados utilizados para a rotação num local de cópia de segurança seguro. Se a rotação é executado e, em seguida, falha, substitua os certificados na partilha de ficheiros com as cópias de segurança antes de voltar a executar a rotação. Tenha em atenção, manter cópias de segurança na localização de cópia de segurança segura.
7.  Crie uma partilha de ficheiros que pode aceder a partir de ERCS VMs. A partilha de ficheiros tem de ser legíveis e gravável para o **CloudAdmin** identidade.
8.  Abra uma consola do ISE do PowerShell a partir de um computador em que tem acesso à partilha de ficheiros. Navegue para a partilha de ficheiros. 
9.  Execute **[CertDirectoryMaker.ps1](https://www.aka.ms/azssecretrotationhelper)** para criar os diretórios necessários para os certificados externos.

## <a name="rotating-external-secrets"></a>Girando segredos externos

Para girar segredos externos:

1. Dentro do recém-criado **certificados** diretório criado nos passos anteriores, coloque o novo conjunto de certificados externos de substituição na estrutura de diretórios, de acordo com o formato descrito na secção certificados obrigatórios do [requisitos de certificado PKI do Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates).
2. Criar uma sessão do PowerShell com o [ponto final com privilégios](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) utilizando o **CloudAdmin** da conta e armazenar as sessões como uma variável. Irá utilizar esta variável como o parâmetro no próximo passo.

    > [!IMPORTANT]  
    > Não introduza a sessão, armazenar a sessão como uma variável.
    
3. Execute  **[invoke-command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-5.1)**. Passar a variável de sessão do PowerShell com privilégios de ponto final como a **sessão** parâmetro. 
4. Execute **Start-SecretRotation** com os seguintes parâmetros:
    - **PfxFilesPath**  
    Especifique o caminho de rede para o seu diretório de certificados que criou anteriormente.  
    - **PathAccessCredential**  
    Um objeto PSCredential as credenciais para a partilha. 
    - **CertificatePassword**  
    Uma cadeia segura da palavra-passe utilizada para todos os ficheiros de certificado pfx, criados.
4. Aguarde enquanto o girar seus segredos. Rotação de secreta externa demora, aproximadamente uma hora. Quando a rotação do segredo é concluída com êxito, a consola irá apresentar **estado geral da ação: Êxito**. 
    > [!Note]  
    > Se falhar a rotação secreta, siga as instruções na mensagem de erro e volte a executar **start-secretrotation** com o **-volte a executar** parâmetro. 

    ```PowerShell  
    Start-SecretRotation -Rerun
    ```
    Contacte o suporte se tiver repetidas falhas de rotação secreta.
5. Após a conclusão com êxito de rotação secreta, remova os certificados da partilha criada no passo anterior e armazená-las em seu local de cópia de segurança seguro. 

## <a name="walkthrough-of-secret-rotation"></a>Passo a passo de rotação secreta

O exemplo de PowerShell seguinte mostra os cmdlets e parâmetros de executar para girar seus segredos.

```powershell
#Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IPofERCSMachine>"}'
$PEPCreds = Get-Credential 
$PEPsession = New-PSSession -computername <IPofERCSMachine> -Credential $PEPCreds -ConfigurationName PrivilegedEndpoint 

#Run Secret Rotation
$CertPassword = ConvertTo-SecureString "Certpasswordhere" -Force
$CertShareCred = Get-Credential 
$CertSharePath = "<NetworkPathofCertShare>"
Invoke-Command -session $PEPsession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword }
Remove-PSSession -Session $PEPSession
```

## <a name="rotating-only-internal-secrets"></a>Girando apenas internos segredos

> [!Note]  
> Rotação de segredo interno só deve ser feita se suspeitar de que um segredo interno tem sido comprometido por uma entidade maliciosa, ou se receber um alerta (na compilação 1811 ou posterior), indicando certificados internos estão prestes a expirar. Ambientes de pilha do Azure em versões de pré-1811 poderão ver alertas para o certificado interno ou expirações secretas pendentes. Estes alertas são incorretos e devem ser ignorados sem executar a rotação secreta interna. Os alertas de expiração do segredo interno incorretos são um problema conhecido que é resolvido no 1811 – interno segredos não irão expirar, a menos que o ambiente tem estado ativo por dois anos.

Para girar apenas do Azure Stack internos segredos:

1. Criar uma sessão do PowerShell com o [ponto final com privilégios](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).
2. Na sessão do ponto final com privilégios, execute **Start-SecretRotation-interno**.

    > [!Note]  
    > Não precisará de ambientes de pilha do Azure em versões de pré-1811 a **-interno** sinalizador. **Start-SecretRotation** irá girar apenas internos segredos.

3. Aguarde enquanto o girar seus segredos.  
Quando a rotação do segredo é concluída com êxito, a consola irá apresentar **estado geral da ação: Êxito**. 
    > [!Note]  
    > Se falhar a rotação secreta, siga as instruções na mensagem de erro e volte a executar **start-secretrotation** com o **– interno** e **-volte a executar** parâmetros.  

    ```PowerShell  
    Start-SecretRotation -Internal -Rerun
    ```
    Contacte o suporte se tiver repetidas falhas de rotação secreta.

## <a name="start-secretrotation-reference"></a>Referência de SecretRotation de início

Gira os segredos de um sistema de pilha do Azure. Só é executado no ponto final do Azure com privilégios do Stack.

### <a name="syntax"></a>Sintaxe

#### <a name="for-external-secret-rotation"></a>Para a rotação de segredos externa

```Powershell  
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential] <PSCredential> [-CertificatePassword <SecureString>]  
```

#### <a name="for-internal-secret-rotation"></a>Para a rotação de secreta interna 

```Powershell  
Start-SecretRotation [-Internal]  
```

#### <a name="for-external-secret-rotation-rerun"></a>Para a rotação de secreta externa, volte a executar 

```Powershell  
Start-SecretRotation [-Rerun]
```

#### <a name="for-internal-secret-rotation-rerun"></a>Para a rotação de secreta interna, volte a executar 

```Powershell  
Start-SecretRotation [-Rerun] [-Internal]
```
### <a name="description"></a>Descrição

O **Start-SecretRotation** cmdlet gira os segredos de infraestrutura de um sistema de Azure Stack. Por padrão, ele gira apenas os certificados de todos os pontos de extremidade de infraestrutura de rede externa. Se for utilizado com o - sinalizador interno interno roda segredos de infraestrutura. Quando alternar pontos finais de infraestrutura de rede externa, **Start-SecretRotation** devem ser executados com um **Invoke-Command** bloco de script com o ambiente do Azure Stack do privilegiado a sessão de ponto final passado como o parâmetro de sessão.
 
### <a name="parameters"></a>Parâmetros

| Parâmetro | Tipo | Necessário | Posição | Predefinição | Descrição |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | Cadeia  | Falso  | com o nome  | Nenhuma  | O caminho de partilha de ficheiros para o **\Certificates** certificados de ponto final de rede do diretório que contém todos os externo. Apenas é necessário quando efetuar a rotação de segredos externos ou todos os segredos. Diretório de fim tem de ser **\Certificates**. |
| CertificatePassword | SecureString | Falso  | com o nome  | Nenhuma  | A palavra-passe para todos os certificados fornecido no - PfXFilesPath. Valor obrigatório se PfxFilesPath é fornecida quando são revezados de segredos internos e externos. |
| Interno | Cadeia | Falso | com o nome | Nenhuma | Sinalizador interno tem de ser utilizado sempre que um operador do Azure Stack deseja girar segredos de infra-estrutura interna. |
| PathAccessCredential | PSCredential | Falso  | com o nome  | Nenhuma  | A credencial do PowerShell para a partilha de ficheiros do **\Certificates** certificados de ponto final de rede do diretório que contém todos os externo. Apenas é necessário quando efetuar a rotação de segredos externos ou todos os segredos.  |
| Voltar a executar | SwitchParameter | Falso  | com o nome  | Nenhuma  | Tem de ser utilizada voltar a executar em qualquer altura rotação secreta está a tentar novamente após uma tentativa falhada. |

### <a name="examples"></a>Exemplos
 
#### <a name="rotate-only-internal-infrastructure-secrets"></a>Rodar apenas os segredos de infra-estrutura interna

Isso tem de ser executado através do Azure Stack [ambiente do privilegiado do ponto de extremidade](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).

```powershell  
PS C:\> Start-SecretRotation  -Internal
```

Este comando gira todos os segredos de infraestrutura expostos à rede interna do Azure Stack. 

#### <a name="rotate-only-external-infrastructure-secrets"></a>Rodar apenas os segredos de infraestrutura externo  

```powershell  
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock {  

Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword }  

Remove-PSSession -Session $PEPSession
```

Este comando gira os certificados TLS utilizados para pontos finais de infraestrutura de rede externa do Azure Stack.   

**Rodar segredos de infra-estrutura interna e externa**
  
```powershell
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword } 
Remove-PSSession -Session $PEPSession
```

Este comando gira todos os segredos de infraestrutura expostos à rede interna do Azure Stack, bem como os certificados TLS utilizados para pontos finais de infraestrutura de rede externa do Azure Stack. Início SecretRotation gira todos os segredos da pilha gerado e, uma vez que recebem certificados, os certificados de ponto final externo irão também precisar ser girados.  


## <a name="update-the-baseboard-management-controller-bmc-credential"></a>Atualizar as credenciais de controlador (BMC) de gestão da placa base

O controlador de gestão da placa base (BMC) monitoriza o estado físico dos seus servidores. As especificações e instruções sobre como atualizar o nome de conta de utilizador e palavra-passe do BMC variam consoante o fornecedor do hardware de fabricante de equipamento original (OEM). Atualize as palavras-passe para os componentes do Azure Stack regularmente.

1. Atualize o BMC em servidores físicos do Azure Stack com as instruções do seu OEM. O nome de conta de utilizador e palavra-passe para cada BMC no seu ambiente tem de ser o mesmo.
2. Abra um ponto final com privilégios em sessões do Azure Stack. Para obter instruções, consulte [utilizando o ponto final com privilégios no Azure Stack](azure-stack-privileged-endpoint.md).
3. Depois do PowerShell, linha foi alterado para **[VM de ERCS do endereço IP ou nome]: PS >** ou a **[azs-ercs01]: PS >**, dependendo do ambiente, execute `Set-BmcCredemtial` executando `invoke-command`. Passe a variável de sessão do ponto final com privilégios, como um parâmetro. Por exemplo:

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString
    $NewBMCuser = Read-Host -Prompt "Enter New BMC user name"

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        # Parameter BMCPassword is mandatory, while the BMCUser parameter is optional.
        Set-Bmccredential -bmcpassword $using:NewBMCpwd -bmcuser $using:NewBMCuser
    }
    Remove-PSSession -Session $PEPSession
    ```
    
    Também pode utilizar a versão estática do PowerShell com as palavras-passe como linhas de código:
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for example Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -Force
    $NewBMCuser = "<New BMC User name>" 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmccredential -bmcpassword $using:NewBMCpwd -bmcuser $using:NewBMCuser
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais sobre a segurança do Azure Stack](azure-stack-security-foundations.md)

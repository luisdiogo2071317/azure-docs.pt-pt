---
title: Utilize a ferramenta de validação do Azure Stack | Documentos da Microsoft
description: Como recolher ficheiros de registo para obter um diagnóstico no Azure Stack.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 12/03/2018
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: f754242d0cf7ee30572b21a3f4daf6fd2c0f63ff
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275912"
---
# <a name="validate-azure-stack-system-state"></a>Validar o estado do sistema do Azure Stack

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Como um operador do Azure Stack, é essencial ter a capacidade de saber o estado de funcionamento e o estado do seu sistema sob demanda. A ferramenta de validação do Azure Stack (**AzureStack teste**) é um cmdlet do PowerShell que lhe permite executar uma série de testes no seu sistema para identificar falhas, se estiver presente. Normalmente, será pedido para executar esta ferramenta do [ponto de final com privilégios (PEP)](azure-stack-privileged-endpoint.md) quando contactar o suporte de serviços ao cliente da Microsoft (CSS) com um problema. Com o estado de funcionamento de todo o sistema e informações de estado à mão, CSS pode recolher e analisar registos detalhados, concentrar-se na área em que ocorreu o erro e trabalhar para resolver o problema.

## <a name="running-the-validation-tool-and-accessing-results"></a>Executar a ferramenta de validação e aceder aos resultados

Conforme indicado anteriormente, a ferramenta de validação é executada através do PEP. Cada teste retorna um **aprovação/REPROVAÇÃO** estado na janela do PowerShell. Além disso, é criado um relatório detalhado do HTML que posteriormente pode ser acessada durante [recolha de registos](azure-stack-diagnostics.md). Eis um resumo da validação de ponto a ponto do processo de teste: 

1. Aceda ao ponto de final com privilégios (PEP). Execute os seguintes comandos para estabelecer uma sessão PEP:

   ```powershell
   Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
   ```

   > [!TIP]
   > Para aceder a PEP num computador anfitrião ASDK, utilize AzS-ERCS01 para - ComputerName.

2. Assim que estiver no PEP, execute: 

   ```powershell
   Test-AzureStack
   ```

   Consulte a [considerações sobre o parâmetro](azure-stack-diagnostic-test.md#parameter-considerations) e [utilize maiúsculas exemplos](azure-stack-diagnostic-test.md#use-case-examples) secções para obter mais informações.

3. Se qualquer um relatório de testes **FALHAR**, execute:

   ```powershell
   Get-AzureStackLog -FilterByRole SeedRing -OutputSharePath "<path>" -OutputShareCredential $cred
   ```

   O cmdlet reúne registos gerados pelo AzureStack de teste. Para obter mais informações sobre os registos de diagnóstico, consulte [ferramentas de diagnóstico do Azure Stack](azure-stack-diagnostics.md). Não deve recolher registos ou contacte o CSS se o relatório de testes **WARN**.

4. Se foram instruído para executar a ferramenta de validação, o CSS, o representante de CSS solicitará registos recolhidos para continuar a resolução de problemas.

## <a name="tests-available"></a>Testes de disponibilidade

A ferramenta de validação permite-lhe executar uma série de testes de nível de sistema e cenários de nuvem básico que fornecem a um informações aprofundadas sobre o atual estipulam e a determinar problemas no seu sistema.

### <a name="cloud-infrastructure-tests"></a>Testes de infraestrutura de nuvem

Esses testes de baixo impacto trabalham num nível de infra-estrutura e fornecem-lhe informações sobre os vários componentes do sistema e as funções. Atualmente, os testes são agrupados nas seguintes categorias:

| Categoria de teste                                        | O argumento-incluem e – ignorar |
| :--------------------------------------------------- | :-------------------------------- |
| Resumo de alertas do Azure Stack                            | AzsAlertSummary                   |
| Resumo de acessibilidade de partilha de cópia de segurança do Azure Stack       | AzsBackupShareAccessibility       |
| Resumo de plano de controlo do Azure Stack                    | AzsControlPlane                   |
| Resumo de Defender o Azure Stack                         | AzsDefenderSummary                |
| O Azure Stack resumo de Firmware de infraestrutura de alojamento  | AzsHostingInfraFWSummary          |
| Resumo de infraestrutura de alojamento de na Cloud do Azure Stack     | AzsHostingInfraSummary            |
| Utilização da infraestrutura de alojamento de na Cloud do Azure Stack | AzsHostingInfraUtilization        |
| Capacidade de infraestrutura do Azure Stack                  | AzsInfraCapacity                  |
| Desempenho de infraestrutura do Azure Stack               | AzsInfraPerformance               |
| Resumo de função de infraestrutura do Azure Stack              | AzsInfraRoleSummary               |
| Resumo de atualização do Azure Stack                           | AzsInfraUpdateSummary             |
| Portal do Azure Stack e o resumo de API                   | AzsPortalAPISummary               |
| Eventos de VMS de unidade de escala do Azure Stack                     | AzsScaleUnitEvents                |
| Recursos de VM de unidade de escala do Azure Stack                  | AzsScaleUnitResources             |
| Resumo de validação de SDN do Azure Stack                   | AzsSDNValidation                  |
| Resumo de função de recursos de infraestrutura de serviço do Azure Stack              | AzsSFRoleSummary                  |
| Resumo BMC do Azure Stack                              | AzsStampBMCSummary                |
| Resumo de serviços de armazenamento do Azure Stack                 | AzsStorageSvcsSummary             |
| Resumo de Store de SQL do Azure Stack                        | AzsStoreSummary                   |
| Resumo de colocação de VM do Azure Stack                     | AzsVmPlacement                    |

### <a name="cloud-scenario-tests"></a>Testes de cenário de nuvem

Além dos testes de infraestrutura acima, também tem a capacidade de executar testes de cenário de nuvem para verificar a funcionalidade em componentes de infraestrutura. As credenciais de administrador da cloud são necessários para executar estes testes, à medida que envolvem a implementação de recursos. 
    > [!NOTE]
    >
    > Currently you cannot run cloud scenario tests using Active Directory Federated Services (AD FS) credentials. 

Os seguintes cenários de cloud são testados pela ferramenta de validação:
- Criação do grupo de recursos   
- Criação de um plano              
- Criação de oferta            
- Criação da conta de armazenamento   
- Criação da máquina virtual 
- Operação de armazenamento de BLOBs   
- Operação de fila de armazenamento  
- Operação de armazenamento de tabela  

## <a name="parameter-considerations"></a>Considerações de parâmetro

- O parâmetro **lista** pode ser usado para exibir todas as categorias de testes disponíveis.

- Os parâmetros **inclusão** e **ignorar** pode ser utilizado para incluir ou excluir as categorias de teste. Consulte a [testes disponíveis](azure-stack-diagnostic-test.md#tests-available) secção para obter mais informações sobre um atalho para ser utilizado com estes argumentos.

  ```powershell
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
  ```

  ```powershell
  Test-AzureStack -Ignore AzsInfraPerformance
  ```

- Testes de um inquilino que VM é implementada como parte de um o cenário de nuvem. Pode usar **DoNotDeployTenantVm** para desativar isso. 

- Precisa fornecer o **ServiceAdminCredential** parâmetro para executar testes de cenário de nuvem, conforme descrito no [utilizar maiúsculas exemplos](azure-stack-diagnostic-test.md#use-case-examples) secção.

- **BackupSharePath** e **BackupShareCredential** são utilizados quando as definições de cópia de segurança de infra-estrutura de teste, como mostra a [utilizar maiúsculas exemplos](azure-stack-diagnostic-test.md#use-case-examples) secção.

- A ferramenta de validação também suporta parâmetros comuns do PowerShell: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable e OutVariable. Para obter mais informações, consulte [sobre parâmetros comuns](http://go.microsoft.com/fwlink/?LinkID=113216).  

## <a name="use-case-examples"></a>Exemplos de casos de utilização

### <a name="run-validation-without-cloud-scenarios"></a>Executar testes de validação sem cenários de nuvem

Execute a ferramenta de validação sem o **ServiceAdminCredential** parâmetro para ignorar os testes de cenário de nuvem em execução: 

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred
Test-AzureStack
```

### <a name="run-validation-with-cloud-scenarios"></a>Executar testes de validação com cenários de nuvem

Fornecendo a ferramenta de validação com o **ServiceAdminCredentials** parâmetro executa os testes de cenário de nuvem por predefinição: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" 
```

Se quiser executar cenários de nuvem apenas sem executar o restante dos testes, pode utilizar o **inclusão** parâmetro para o fazer: 

```powershell
Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -ServiceAdminCredential "<Cloud administrator user name>" -Include AzsScenarios   
```

O nome de utilizador de administrador de nuvem tem de ser introduzido no formato UPN: serviceadmin@contoso.onmicrosoft.com (Azure AD). Quando lhe for pedido, escreva a palavra-passe para a conta de administrador na cloud.

### <a name="run-validation-tool-to-test-system-readiness-before-installing-update-or-hotfix"></a>Executar a ferramenta de validação para testar a preparação do sistema antes de instalar a atualização ou correção

Antes de iniciar a instalação de uma atualização ou correção, deve executar a ferramenta de validação para verificar o estado do seu Azure Stack:

```powershell
New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
```

### <a name="run-validation-tool-to-test-infrastructure-backup-settings"></a>Executar a ferramenta de validação para testar as definições de cópia de segurança de infra-estrutura

*Antes de* configurar a cópia de segurança da infraestrutura, pode testar o caminho da partilha de cópia de segurança e de credenciais com o **AzsBackupShareAccessibility** de teste: 

  ```powershell
  New-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential <PSCredentials-for-backup-share>
  ```

*Após* configurar a cópia de segurança, pode executar **AzsBackupShareAccessibility** validar a partilha está acessível a partir do ERCS:

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility
  ```

Para testar novas credenciais com a partilha de cópia de segurança configurada, execute: 

  ```powershell
  Enter-PSSession -ComputerName "<ERCS VM-name/IP address>" -ConfigurationName PrivilegedEndpoint -Credential $localcred 
  Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential "<PSCredential for backup share>"
  ```



## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre ferramentas de diagnóstico do Azure Stack e o registo de problema, consulte [ferramentas de diagnóstico do Azure Stack](azure-stack-diagnostics.md).

Para saber mais sobre a resolução de problemas, consulte [resolução de problemas do Microsoft Azure Stack](azure-stack-troubleshooting.md).
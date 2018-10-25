---
title: Executar um teste de validação no Azure Stack | Documentos da Microsoft
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
ms.date: 10/24/2018
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: 4f95fb5f2199e8c276b78a83391f3814303a9470
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024624"
---
# <a name="run-a-validation-test-for-azure-stack"></a>Executar um teste de validação para o Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*
 
Pode validar o estado do seu Azure Stack. Quando tiver um problema, contacte o suporte ao cliente de serviços da Microsoft. Suporte lhe pedir para executar **AzureStack teste** a partir do seu nó de gestão. O teste de validação isola a falha. Suporte, em seguida, pode analisar os registos detalhados, concentrar-se na área onde ocorreu o erro e trabalhar consigo resolver o problema.

## <a name="run-test-azurestack"></a>Run Test-AzureStack

Quando tiver um problema, contacte o suporte ao cliente de serviços da Microsoft e, em seguida, execute **AzureStack de teste executar**.

1. Tem um problema.
2. Contacto de Microsoft de cliente dos serviços de suporte.
3. Execute **AzureStack teste** do ponto final com privilégios.
    1. O ponto final com privilégios de acesso. Para obter instruções, consulte [utilizando o ponto final com privilégios no Azure Stack](azure-stack-privileged-endpoint.md). 
    2. No ASDK, inicie sessão para o anfitrião de gestão como **AzureStack\CloudAdmin**.  
    Num sistema integrado, terá de utilizar o endereço IP para com privilégios--ponto final para o gerenciamento fornecido a pelo seu fornecedor de hardware de OEM.
    3. Abra o PowerShell como um Administrador.
    4. Execute: `Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint`
    5. Execute: `Test-AzureStack`
4. Se qualquer teste de relatório falhar, execute: `Get-AzureStackLog -FilterByRole SeedRing -OutputPath <Log output path>` o cmdlet reúne os registos de teste AzureStack. Para obter mais informações sobre os registos de diagnóstico, consulte [ferramentas de diagnóstico do Azure Stack](azure-stack-diagnostics.md). Não deve recolher registos ou contacte o suporte de serviços ao cliente da Microsoft (CSS) se o aviso de relatório de testes.
5. Enviar a **SeedRing** registos ao suporte ao cliente de serviços da Microsoft. Suporte ao cliente de serviços da Microsoft trabalha para resolver o problema.

## <a name="reference-for-test-azurestack"></a>Referência para o teste AzureStack

Esta secção contém uma descrição geral para o cmdlet Test-AzureStack e um resumo do relatório de validação.

### <a name="test-azurestack"></a>Test-AzureStack

Valida o estado do Azure Stack. O cmdlet comunica o estado do seu software e hardware do Azure Stack. Equipa de suporte técnico pode utilizar este relatório para reduzir o tempo para resolver incidentes de suporte do Azure Stack.

> [!Note]  
> **Teste AzureStack** pode detetar falhas que não são resultando em falhas de cloud, como uma única falha de disco ou uma falha de nó único host físico.

#### <a name="syntax"></a>Sintaxe

````PowerShell
  Test-AzureStack
````

#### <a name="parameters"></a>Parâmetros

| Parâmetro               | Valor           | Necessário | Predefinição |
| ---                     | ---             | ---      | ---     |
| ServiceAdminCredentials | Cadeia    | Não       | FALSO   |
| DoNotDeployTenantVm     | SwitchParameter | Não       | FALSO   |
| AdminCredential         | PSCredential    | Não       | ND      |
| Lista                    | SwitchParameter | Não       | FALSO   |
| Ignorar                  | Cadeia          | Não       | ND      |
| Incluir                 | Cadeia          | Não       | ND      |
| BackupSharePath         | Cadeia          | Não       | ND      |
| BackupShareCredential   | PSCredential    | Não       | ND      |


O cmdlet Test-AzureStack suporta os parâmetros comuns: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable e OutVariable. Para obter mais informações, consulte [sobre parâmetros comuns](http://go.microsoft.com/fwlink/?LinkID=113216). 

### <a name="examples-of-test-azurestack"></a>Exemplos de teste AzureStack

Os exemplos seguintes partem do princípio de que tem sessão iniciada como **CloudAdmin** e acessar o ponto de extremidade com privilégios (PEP). Para obter instruções, consulte [utilizando o ponto final com privilégios no Azure Stack](azure-stack-privileged-endpoint.md). 

#### <a name="run-test-azurestack-interactively-without-cloud-scenarios"></a>Executar teste AzureStack interativamente sem cenários de nuvem

Numa sessão PEP, execute:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack
````

#### <a name="run-test-azurestack-with-cloud-scenarios"></a>Executar teste AzureStack com cenários de nuvem

Pode usar **AzureStack teste** executar cenários de nuvem do Azure Stack. Os cenários incluem:

 - Criar grupos de recursos
 - Criação de planos
 - Criação de ofertas
 - A criação de contas de armazenamento
 - Criar uma máquina virtual
 - Efetuar operações de Blobs com a conta de armazenamento criada no cenário do teste
 - Efetuar operações de fila com a conta de armazenamento criada no cenário do teste
 - Efetuar operações de tabela com a conta de armazenamento criada no cenário do teste

Os cenários de nuvem necessitam de credenciais de administrador da cloud. 
> [!Note]  
> Não é possível executar os cenários de nuvem com as credenciais do Active Directory Federated Services (AD FS). O **AzureStack teste** cmdlet só é acessível via a PEP. No entanto, o PEP não suporta credenciais de AD FS.

Escreva o nome de utilizador de administrador de nuvem no formato UPN serviceadmin@contoso.onmicrosoft.com (Azure AD). Quando lhe for pedido, escreva a palavra-passe para a conta de administrador na cloud.

Numa sessão PEP, execute:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -ServiceAdminCredentials <Cloud administrator user name>
````

#### <a name="run-test-azurestack-without-cloud-scenarios"></a>Executar teste AzureStack sem cenários de nuvem

Numa sessão PEP, execute:

````PowerShell
  $session = New-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Invoke-Command -Session $session -ScriptBlock {Test-AzureStack}
````

#### <a name="list-available-test-scenarios"></a>Liste os cenários de teste disponíveis:

Numa sessão PEP, execute:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -List
````

#### <a name="run-a-specified-test"></a>Executar um teste especificado

Numa sessão PEP, execute:

````PowerShell
  Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
````

Para excluir os testes específicos:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint  -Credential $localcred
    Test-AzureStack -Ignore AzsInfraPerformance
````

### <a name="run-test-azurestack-to-test-infrastructure-backup-settings"></a>Executar AzureStack de teste para testar as definições de cópia de segurança de infra-estrutura

Antes de configurar a cópia de segurança da infraestrutura, pode testar o caminho da partilha de cópia de segurança e de credenciais com o **AzsBackupShareAccessibility** testar.

Numa sessão PEP, execute:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility -BackupSharePath "\\<fileserver>\<fileshare>" -BackupShareCredential <PSCredentials-for-backup-share>
````
Depois de configurar a cópia de segurança, pode executar AzsBackupShareAccessibility para validar a partilha está acessível a partir do ERCS, a partir de uma sessão PEP executar:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint  -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility
````

Para testar novas credenciais com a partilha de cópia de segurança configurada, a partir de uma sessão PEP execute:

````PowerShell
    Enter-PSSession -ComputerName <ERCS-VM-name> -ConfigurationName PrivilegedEndpoint -Credential $localcred
    Test-AzureStack -Include AzsBackupShareAccessibility -BackupShareCredential <PSCredential for backup share>
````

### <a name="validation-test"></a>Teste de validação

A tabela seguinte resume os testes de validação executados pelo **AzureStack teste**.

| Nome                                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Resumo de infraestrutura de alojamento de na Cloud do Azure Stack                                                                                  |
| Resumo de serviços de armazenamento do Azure Stack                                                                                              |
| Resumo de instância de função de infraestrutura do Azure Stack                                                                                  |
| Utilização da infraestrutura de alojamento de na Cloud do Azure Stack                                                                              |
| Capacidade de infraestrutura do Azure Stack                                                                                               |
| Portal do Azure Stack e o resumo de API                                                                                                |
| Resumo de certificados do Gestor de recursos do Azure de pilha do Azure                                                                                               |
| Controlador de gestão de infraestrutura, controlador de rede, serviços de armazenamento e funções de infraestrutura de ponto de extremidade com privilégios          |
| Controlador de gestão de infraestrutura, controlador de rede, serviços de armazenamento e ponto final com privilégios instâncias de função de infraestrutura |
| Resumo da função de infraestrutura de pilha do Azure                                                                                           |
| Serviços de recursos de infraestrutura de serviço de Cloud do Azure Stack                                                                                         |
| Desempenho de instâncias de função de infraestrutura do Azure Stack                                                                              |
| Resumo de desempenho de anfitrião de nuvem do Azure Stack                                                                                        |
| Resumo de consumo de recursos de serviço do Azure Stack                                                                                  |
| O Azure Stack dimensionamento unidade eventos críticos (últimas 8 horas)                                                                             |
| Resumo de discos físicos de serviços de armazenamento do Azure Stack                                                                               |
|Resumo de acessibilidade de partilha de cópia de segurança do Azure Stack                                                                                     |

## <a name="next-steps"></a>Passos Seguintes

 - Para saber mais sobre ferramentas de diagnóstico do Azure Stack e o registo de problema, consulte [ ferramentas de diagnóstico do Azure Stack](azure-stack-diagnostics.md).
 - Para saber mais sobre a resolução de problemas, consulte [resolução de problemas do Microsoft Azure Stack](azure-stack-troubleshooting.md)

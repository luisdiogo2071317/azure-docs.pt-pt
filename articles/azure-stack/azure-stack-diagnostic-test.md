---
title: "Execute um teste de validação na pilha do Azure | Microsoft Docs"
description: "Como recolher ficheiros de registo para obter um diagnóstico na pilha do Azure"
services: azure-stack
author: mattbriggs
manager: femila
cloud: azure-stack
ms.assetid: D44641CB-BF3C-46FE-BCF1-D7F7E1D01AFA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: mabrigg
ms.openlocfilehash: 53ef19628b40c4a008143c867c9e7867ac91854d
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="run-a-validation-test-for-azure-stack"></a>Execute um teste de validação de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*
 
Pode validar o estado da pilha do Azure. Quando tiver um problema, contacte o suporte ao cliente de serviços da Microsoft. Suporte pede-lhe para executar o teste AzureStack a partir do nó de gestão. O teste de validação isola a falha. Suporte, em seguida, pode analisar os registos de detalhado, focar-se na área de onde ocorreu o erro e trabalhar com a resolver o problema.

## <a name="run-test-azurestack"></a>Executar o teste AzureStack

Quando tiver um problema, contacte o suporte ao cliente de serviços da Microsoft e, em seguida, execute **executar teste-AzureStack**.

1. Tem um problema.
2. Contacto técnico da Microsoft dos serviços de suporte.
3. Executar **teste AzureStack** do ponto final com privilégios.
    1. O ponto final com privilégios de acesso. Para obter instruções, consulte [utilizando o ponto final com privilégios na pilha de Azure](azure-stack-privileged-endpoint.md). 
    2. Inicie sessão como **AzureStack\CloudAdmin** no anfitrião de gestão.
    3. Abra o PowerShell como um Administrador.
    4. Execute:`Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint`
    5. Execute:`Test-AzureStack`
4. Se os testes de relatório falhar, execute: `Get-AzureStackLog -FilterByRole SeedRing -OutputPath <Log output path>` o cmdlet recolhe os registos de AzureStack de teste. Para mais informações sobre os registos de diagnóstico, consulte [ferramentas de diagnóstico do Azure pilha](azure-stack-diagnostics.md).
5. Enviar o **SeedRing** registos para o suporte ao cliente de serviços da Microsoft. Suporte ao cliente de serviços da Microsoft funciona com a resolver o problema.

## <a name="reference-for-test-azurestack"></a>Referência para o teste AzureStack

Esta secção contém uma descrição geral para o cmdlet de teste AzureStack e um resumo do relatório de validação.

### <a name="test-azurestack"></a>Teste AzureStack

Valida o estado da pilha do Azure. O cmdlet reporta o estado do software e hardware de pilha do Azure. Equipa de suporte pode utilizar este relatório para reduzir o tempo para resolver casos do suporte de pilha do Azure.

> [!Note]  
> Teste AzureStack pode detetar falhas que não são resultando em falhas de nuvem, como falha de um único disco ou uma falha de nó único anfitrião físico.

#### <a name="syntax"></a>Sintaxe

````PowerShell
  Test-AzureStack
````

#### <a name="parameters"></a>Parâmetros

| Parâmetro               | Valor           | Necessário | Predefinição |
| ---                     | ---             | ---      | ---     |
| ServiceAdminCredentials | PSCredential    | Não       | FALSO   |
| DoNotDeployTenantVm     | SwitchParameter | Não       | FALSO   |
| AdminCredential         | PSCredential    | Não       | ND      |
| StorageConnectionString | Cadeia          | Não       | ND      |
| Lista                    | SwitchParameter | Não       | FALSO   |
| Ignorar                  | Cadeia          | Não       | ND      |
| Incluir                 | Cadeia          | Não       | ND      |

O cmdlet de teste AzureStack suporta os parâmetros comuns: verboso, depuração, ErrorAction, ErrorVariable, WarningAction, WarningVariable, OutBuffer, PipelineVariable e OutVariable. Para obter mais informações, consulte [sobre parâmetros comuns de](http://go.microsoft.com/fwlink/?LinkID=113216). 

### <a name="examples-of-test-azurestack"></a>Exemplos de teste AzureStack

Os exemplos seguintes partem do princípio de que iniciou sessão como **CloudAdmin** e aceder ao ponto final com privilégios (PEP). Para obter instruções, consulte [utilizando o ponto final com privilégios na pilha de Azure](azure-stack-privileged-endpoint.md). 

#### <a name="run-test-azurestack-interactively-without-cloud-scenarios"></a>Executar o teste AzureStack interativamente sem cenários de nuvem

Numa sessão PEP, execute:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
      Test-AzureStack
````

#### <a name="run-test-azurestack-with-cloud-scenarios"></a>Execute o teste AzureStack com cenários de nuvem

Pode utilizar AzureStack de teste para executar os cenários de nuvem no seu pilha do Azure. Os cenários incluem:

 - Criar grupos de recursos
 - Criar planos
 - Criar ofertas
 - Criar contas de armazenamento
 - Criar uma máquina virtual
 - Efetuar operações de Blobs com a conta de armazenamento criada no cenário de teste
 - Efetuar operações de fila utilizando a conta de armazenamento criada no cenário de teste
 - Efetuar operações de tabela com a conta de armazenamento criada no cenário de teste

Os cenários de nuvem necessitam de credenciais de administrador da nuvem. 
> [!Note]  
> Não é possível executar os cenários de nuvem com as credenciais de serviços de federado do Active Directory (ADFS). O **teste AzureStack** cmdlet só é acessível através de PEP. No entanto, o PEP não suporta credenciais ADFS.

Escreva o nome de utilizador de administrador de nuvem no formato UPN serviceadmin@contoso.onmicrosoft.com (AAD). Quando lhe for pedido, escreva a palavra-passe para a conta de administrador da nuvem.

Numa sessão PEP, execute:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -ServiceAdminCredentials <Cloud administrator user name>
````

#### <a name="run-test-azurestack-without-cloud-scenarios"></a>Executar o teste AzureStack sem cenários de nuvem

Numa sessão PEP, execute:

````PowerShell
  $session = New-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Invoke-Command -Session $session -ScriptBlock {Test-AzureStack}
````

#### <a name="list-available-test-scenarios"></a>Listar os cenários de teste disponíveis:

Numa sessão PEP, execute:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -List
````

#### <a name="run-a-specified-test"></a>Executar um teste especificado

Numa sessão PEP, execute:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -Include AzsSFRoleSummary, AzsInfraCapacity
````

Para excluir os testes específicos:

````PowerShell
  Enter-PSSession -ComputerName <ERCS VM name> -ConfigurationName PrivilegedEndpoint `
  Test-AzureStack -Ignore AzsInfraPerformance
````

### <a name="validation-test"></a>Teste de validação

A tabela seguinte resume os testes de validação executados por AzureStack de teste.

| Nome                                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| Resumo de infraestrutura de alojamento de nuvem de pilha do Azure                                                                                  |
| Resumo de serviços de armazenamento de pilha do Azure                                                                                              |
| Resumo de instância de função de infraestrutura de pilha do Azure                                                                                  |
| Utilização da infraestrutura de alojamento de nuvem de pilha do Azure                                                                              |
| Capacidade de infraestrutura de pilha do Azure                                                                                               |
| Portal de pilha do Azure e o resumo de API                                                                                                |
| Resumo de certificado do Gestor de recursos do Azure da pilha de Azure                                                                                               |
| Controlador de gestão de infraestrutura, controlador de rede, serviços de armazenamento e ponto final com privilégios funções de infraestrutura          |
| Controlador de gestão de infraestrutura, controlador de rede, serviços de armazenamento e ponto final com privilégios instâncias de função de infraestrutura |
| Resumo de função de infraestrutura de pilha do Azure                                                                                           |
| Serviços de recursos de infraestrutura de serviço de Cloud de pilha do Azure                                                                                         |
| Desempenho de instância da função de infraestrutura de pilha do Azure                                                                              |
| Resumo de desempenho de anfitrião de nuvem de pilha do Azure                                                                                        |
| Resumo de consumo de recursos de serviço de pilha do Azure                                                                                  |
| Azure pilha escala unidade eventos críticos (última 8 horas)                                                                             |
| Resumo de discos físicos de serviços de armazenamento de pilha do Azure                                                                               |

## <a name="next-steps"></a>Passos Seguintes

 - Para saber mais sobre as ferramentas de diagnóstico de pilha do Azure e o registo de problema, consulte o artigo [ ferramentas de diagnóstico do Azure pilha](azure-stack-diagnostics.md).
 - Para obter mais informações sobre resolução de problemas, consulte [resolução de problemas de pilha do Microsoft Azure](azure-stack-troubleshooting.md)
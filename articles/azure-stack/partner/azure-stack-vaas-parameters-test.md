---
title: Testar os parâmetros para a validação como um serviço do Azure Stack | Documentos da Microsoft
description: Tópico de referência sobre o ficheiro de configuração e o teste passar parâmetros para a validação como um serviço do Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 9f042779e3463f0d75dc6327b3553156edbf162a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162228"
---
# <a name="test-parameters-for-validation-as-a-service-azure-stack"></a>Parâmetros de teste para validação como um serviço do Azure Stack

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Antes de executar qualquer conjunto de teste de validações como um serviço (VaaS), selecione uma solução e crie uma aprovação de teste.

- Inicie sessão com a sua VaaS registados credenciais de inquilino.
- Criar uma nova solução (selecionando **Adicionar solução**) ou selecionar qualquer existente.
- Selecione o **começar** botão a **Passes de teste** mosaico de fluxo de trabalho.

> [!Note]  
> Crie uma nova entrada de solução para cada configuração de conjunto/hardware de computador exclusivo que está a validar e um novo teste passar para todas as implementações de compilação nesse conjunto de máquina.

Terá de introduzir os parâmetros necessários para executar os testes do **informações de passar de teste** página. Fornece estes parâmetros quando iniciar uma nova passagem de teste ou executar de validação. A maioria dos parâmetros têm os mesmos valores que forneceu quando implementou o Azure Stack.

Pode introduzir os valores apresentados manualmente na [tabela de parâmetros de teste](#test-parameters), ou carregar o ficheiro de configuração de implementação, que contém as informações de carimbo de data / completas do Azure Stack. Depois de carregados, o portal carrega os valores do arquivo.

> [!Note]  
> Pode procurar e escrever os valores de parâmetros de teste por encontrar e carregar o ficheiro de configuração para o portal.

## <a name="export-the-test-parameters-using-powershell"></a>Exportar os parâmetros de teste com o PowerShell

1. Inicie sessão na máquina do DVM ou de qualquer computador que tenha acesso ao ambiente do Azure Stack.
2. Abra uma janela elevada do PowerShell e execute:

    ````PowerShell  
        $params = Invoke-RestMethod -Method Get -Uri 'https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation'
        ConvertTo-Json $params > stampinfoproperties.json
    ````

3. Carregue **stampinfoproperties.json** para o portal de VaaS.

## <a name="find-the-test-parameters-in-the-configuration-file"></a>Encontrar os parâmetros de teste no ficheiro de configuração

Também pode encontrar o teste de valores de parâmetros, abrindo o ECE **ficheiro de configuração**. Pode encontrar o ficheiro no seguinte caminho na máquina do DVM:  
`C:\EceStore\403314e1-d945-9558-fad2-42ba21985248\80e0921f-56b5-17d3-29f5-cd41bf862787`

## <a name="test-parameters"></a>Parâmetros de teste 

| Parâmetro    | Descrição |
|-------------|-----------------|
| Compilação do Azure Stack                      | Número de versão ou criar a pilha do Azure que foi implementado por exemplo, 1.0.170330.0 | 
| ID do inquilino                              | Inquilino do Azure Active Directory especificado durante a implementação do Azure Stack. Procure **AADTenant** no ficheiro de configuração e selecione o **GUID** valor no `Id` marca. | 
| Região                                 | Região de implementação do Azure Stack. Procure **região** no ficheiro de configuração. | 
| Gestor de recursos de inquilino                | Por exemplo, o ponto final do Azure Resource Manager, de inquilino `https://management.<ExternalFqdn>` | 
| Gestor de recursos de administração                 | Ponto final de administração do Azure Resource Manager. Por exemplo, `https://adminmanagement.<ExternalFqdn>` | 
| FQDN externo                          | FQDN externa do ambiente. Procure **ExternalFqdn** no ficheiro de configuração. | 
| Utilizador do inquilino                            | Administrador de inquilino do Active Directory seja aprovisionada já ou tem de ser aprovisionadas pelo administrador de serviço no Azure AD Directory do Azure. Para obter detalhes sobre o aprovisionamento da conta de inquilino, consulte [adicionar uma nova conta de inquilino do Azure Stack no Azure Active Directory](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-new-user-aad). Este valor é utilizado pelo teste de execução de operações ao nível do inquilino como a implementação de modelos para aprovisionar os recursos (da VM, armazenamento contas etc.) e executar cargas de trabalho. | 
| Utilizador de administrador de serviços             | Administrador do Active Directory do Azure do inquilino do Azure AD Directory especificado durante a implementação do Azure Stack. Procure **AADTenant** na configuração do arquivo e selecione o valor no `UniqueName` marca no ficheiro de configuração. | 

## <a name="checks-before-starting-the-tests"></a>Verificações antes de iniciar os testes

Os testes realizam operações remotas. A máquina que executa os testes deve ter acesso aos pontos finais do Azure Stack. Caso contrário, o teste não funcionará. Se estiver a utilizar o VaaS no agente local, utilize a máquina em que o agente será executado. Pode verificar que o seu computador tem acesso aos pontos de pilha do Azure ao executar as seguintes verificações.

1. Verifique se o URI de Base pode ser contatado. Abra uma linha de comandos ou shell do bash e execute o seguinte comando:

    ```bash  
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Abra um browser e navegue para `https://adminportal.<EXTERNALFQDN>` para verificar que o Portal de MAS pode ser contactado.

3. Valores de nome e palavra-passe de administrador indicou quando criou a aprovação de teste do serviço de início de sessão com o Azure AD.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre [validação de Azure Stack como um serviço](https://docs.microsoft.com/azure/azure-stack/partner).

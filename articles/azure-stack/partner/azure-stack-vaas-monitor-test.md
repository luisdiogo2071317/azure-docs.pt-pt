---
title: Monitorizar um teste com a validação do Azure Stack como serviço | Documentos da Microsoft
description: Monitorize um teste com a validação do Azure Stack como um serviço.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 2dc4d3f2855864ff80648b5b9635ff28c0dacbb7
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163334"
---
# <a name="monitor-a-test-with-azure-stack-validation-as-a-service"></a>Monitorizar um teste com a validação do Azure Stack como serviço

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

A execução de um teste pode ser monitorizada, visualizando o **operações** página para conjuntos de testes que estão em curso ou foi concluída. Esta página apresenta o estado das suas operações e o teste.

## <a name="monitor-a-test"></a>Monitorizar um teste

1. Selecione uma solução.

2. Selecione **gerir** em qualquer mosaico de fluxo de trabalho.

3. Clique num fluxo de trabalho para abrir a página de resumo de teste.

4. Expanda o menu de contexto **[...]**  para qualquer instância de conjunto de teste.

5. Selecione **ver operações**

![Texto alternativo](media\image4.png)

Para testes que tem concluído a execução, registos podem ser transferidos a partir da página de resumo de teste ao clicar em **transferir registos** no menu de contexto de um teste **[...]** . Parceiros do Azure Stack podem utilizar estes registos para depuração dos problemas de testes com falhas.

## <a name="open-the-test-pass-summary"></a>Abra o resumo de passagem de teste

1. Abra o portal. 
2. Selecione o nome de uma solução existente que anteriormente contém testes de execução ou agendadas.

    ![Gerir as fases de teste](media/managetestpasses.png)

3. Selecione **Manage** no **Passes de teste** painel.
4. Selecione a aprovação de teste para abrir o resumo de passagem de teste. Pode rever o nome de teste, data de criação, executar, quanto tempo levou o teste e o resultado (com êxito ou falhadas).
5. Selecione [ **...  .** ].

### <a name="test-pass-summary"></a>Resumo de passagem de teste

| Coluna | Descrição |
| --- | --- |
| Nome do teste | O nome do teste. Referencia o número de validação. |
| Criado | Hora de que criação de aprovação de teste. |
| Iniciado | Tempo de que execução de teste nos últimos. |
| Duração | Período de tempo o tempo que demorou a aprovação de teste para ser executado. |
| Estado | O resultado (com êxito ou falha) para a passagem de rest. |
| Nome do Agente | O nome de domínio completamente qualificado do agente. |
| Total de operações | O número total de operações de tentativa de aprovação de teste. |
| Operações com êxito | O número de operações que passado a aprovação de teste. |
|  Operações com falhas | O número de operações que falharam. |

### <a name="group-columns-in-the-test-pass-summary"></a>Colunas de grupo no teste passam resumidas

Pode selecionar e arrastar uma coluna para o cabeçalho para criar um grupo no valor de coluna.

## <a name="reschedule-a-test"></a>Reagendar um teste

1. [Abra o resumo de passagem de teste](#open-the-test-pass-summary).
2. Selecione **reagendar** para reagendar a aprovação de teste.
3. Introduza a palavra-passe de administrador da nuvem para a sua instância do Azure Stack.
4. Introduza a cadeia de ligação de armazenamento de diagnóstico que definiu quando configurar a sua conta.
5. Reagende o teste.

## <a name="cancel-a-test"></a>Cancelar um teste

1. [Abra o resumo de passagem de teste](#open-the-test-pass-summary).
2. Selecione **Cancelar**.

## <a name="get-test-information"></a>Obtenha informações de teste

1. [Abra o resumo de passagem de teste](#open-the-test-pass-summary).
2. Selecione **ver informações** para reagendar a aprovação de teste.

**Informações de teste**

| Nome | Descrição |
| -- | -- |
| Nome do teste | O nome do teste, por exemplo, OEM atualização na validação do Azure Stack 1806 RC. |
| Versão de teste | A versão de teste, por exemplo, 5.1.4.0. |
| Publicador | O publicador de teste, como o Microsoft. |
| Categoria | A categoria de teste, tal como **funcional** ou **confiabilidade**. |
| Serviços de destino | Os serviços que está sendo testados, tais como VirtualMachines |
| Descrição | A descrição do teste. |
| Duração estimada (minutos) | O período de tempo em minutos, que o teste demorou a execução. |
| Ligações | Uma ligação para o controlador de problemas do GitHub. |

## <a name="get-test-parameters"></a>Obter parâmetros de teste

1. [Abra o resumo de passagem de teste](#open-the-test-pass-summary).
2. Selecione **ver parâmetros** para reagendar a aprovação de teste.

**Parâmetros**

| Nome | Descrição |
| -- | -- |
| Nome do teste | O nome do teste, por exemplo, oemupdate1806test. |
| Versão de teste | A versão do resto, por exemplo, 5.1.4.0. |
| ID de instância de teste | Um GUID a identificar a instância específica do teste, por exemplo, 20b20645-b400-4f0d-bf6f-1264d866ada9. |
| cloudAdminUser | O nome da conta utilizada como o administrador da nuvem, por exemplo, **cloudadmin**. |
| DiagnosticsContainerName | O ID do contentor diagnóstico, por exemplo, 04dd3815-5f35-4158-92ea-698027693080. |

## <a name="get-test-operations"></a>Obter operações de teste

1. [Abra o resumo de passagem de teste](#open-the-test-pass-summary).
2. Selecione **ver operações** para reagendar a aprovação de teste. É aberto o painel de resumo de operações.

## <a name="get-test-logs"></a>Obter registos de teste

1. [Abra o resumo de passagem de teste](#open-the-test-pass-summary).
2. Selecione **transferir registos** para reagendar a aprovação de teste.  
    Um ficheiro zip com o nome ReleaseYYYY-MM-DD.zip, que contém os downloads de registos.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre [validação de Azure Stack como um serviço](https://docs.microsoft.com/azure/azure-stack/partner).

---
title: Instalar componentes licenciados para o tempo de execução de integração do Azure-SSIS | Microsoft Docs
description: Saiba como pode desenvolver um ISV e instalar paga ou está licenciado componentes personalizados para o tempo de execução de integração do Azure-SSIS
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 146dc8c4475a041f28d7fe7ca464dfbc104258c7
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265959"
---
# <a name="install-paid-or-licensed-custom-components-for-the-azure-ssis-integration-runtime"></a>Instalar componentes personalizados pagos ou licenciados para o tempo de execução de integração do Azure-SSIS

Este artigo descreve como um ISV e estiver pode desenvolver e instalar pagos ou licenciados componentes personalizados para pacotes de SQL Server Integration Services (SSIS) que são executados no Azure em tempo de execução de integração SSIS do Azure.

## <a name="the-problem"></a>O problema

A natureza do tempo de execução de integração do Azure-SSIS apresenta várias desafios, que tornam os métodos de licenciamento típicos utilizados para a instalação local dos componentes personalizados inadequados. Como resultado, a resposta a incidentes SSIS do Azure requer uma abordagem diferente.

-   Os nós da resposta a incidentes SSIS do Azure são volátil e pode ser atribuído ou lançadas em qualquer altura. Por exemplo, pode iniciar ou parar nós para gerir o custo, ou aumentar vertical ou através de vários tamanhos de nó. Como resultado, vincula uma licença de componente de terceiros a um determinado nó através da utilização de informações específicas do computador, tais como o endereço MAC ou o ID de CPU já não é viável.

-   Também pode dimensionar a resposta a incidentes Azure SSIS entrada ou saída, para que o número de nós pode diminuir ou expanda em qualquer altura.

## <a name="the-solution"></a>A solução

Como resultado as limitações dos métodos de licenciamento tradicionais, descritos na secção anterior, a resposta a incidentes SSIS do Azure fornece uma nova solução. Esta solução utiliza variáveis de ambiente do Windows e variáveis de SSIS do sistema para o enlace de licenciamento e a validação de componentes de terceiros. Os ISVs podem utilizar estas variáveis para obter as informações exclusivas e persistente para uma resposta a incidentes SSIS do Azure, como o ID de Cluster e o número de nós de Cluster. Com esta informação, ISVs podem então vinculá a licença para os respetivos componentes para uma resposta a incidentes Azure SSIS *como um cluster*. Este enlace utiliza um ID que não alteram quando os clientes iniciar ou param, aumentar ou reduzir verticalmente a escala de entrada ou saída ou reconfigurar a resposta a incidentes SSIS do Azure de qualquer forma.

O diagrama seguinte mostra a instalação típica, a ativação e o enlace da licença e fluxos de validação de componentes de terceiros que utilizam estas novas variáveis:

![Instalação dos componentes licenciadas](media/how-to-configure-azure-ssis-ir-licensed-components/licensed-component-installation.png)

## <a name="instructions"></a>Instruções
1. Os ISVs podem oferecer os respetivos componentes licenciados em vários SKUs ou camadas (por exemplo, único nó, até 5 nós, até 10 nós e assim sucessivamente). O ISV fornece a chave de produto correspondente quando os clientes comprar um produto. O ISV também pode fornecer um contentor do blob Storage do Azure que contém um script de configuração de ISV e os ficheiros associados. Os clientes podem copiar estes ficheiros para os seus próprios contentor de armazenamento e modificá-las com a chave de produto (por exemplo, executando `IsvSetup.exe -pid xxxx-xxxx-xxxx`). Os clientes podem aprovisionar ou reconfigurar a resposta a incidentes SSIS do Azure com o URI de SAS do respetivo contentor como parâmetro. Para mais informações, veja [Configuração personalizada do runtime de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md).

2. Quando a resposta a incidentes SSIS do Azure é aprovisionada ou reconfigurada, a instalação de ISV é executada em cada nó para consultar as variáveis de ambiente do Windows, `SSIS_CLUSTERID` e `SSIS_CLUSTERNODECOUNT`. A resposta a incidentes Azure SSIS envia o ID de Cluster e a chave de produto para o produto licenciado para o servidor de ativação de ISV para gerar uma chave de ativação.

3. Depois de receber a chave de ativação, configuração de ISV pode armazenar a chave localmente em cada nó (por exemplo, no registo).

4. Quando os clientes executam um pacote que utiliza o componente licenciada do ISV num nó da resposta a incidentes SSIS do Azure, o pacote lê a chave de ativação armazenado localmente e validá-lo contra o ID de Cluster. do nó O pacote também, opcionalmente, pode reportar a contagem de nós de Cluster para o servidor de ativação de ISV.

    Eis um exemplo de código que valida a chave de ativação e reporta o número de nós de cluster:

    ```csharp
    public override DTSExecResult Validate(Connections, VariableDispenser, IDTSComponentEvents componentEvents, IDTSLogging log) 
                                                                                                                               
    {                                                                                                                             
                                                                                                                               
    Variables vars = null;                                                                                                        
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterID");                                                                           
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterNodeCount");                                                                    
                                                                                                                               
    variableDispenser.GetVariables(ref vars);                                                                                     
                                                                                                                               
    // Validate Activation Key with ClusterID                                                                                     
                                                                                                                               
    // Report on ClusterNodeCount                                                                                                 
                                                                                                                               
    vars.Unlock();                                                                                                                
                                                                                                                               
    return base.Validate(connections, variableDispenser, componentEvents, log);                                                   
                                                                                                                               
    }
    ```

## <a name="isv-partners"></a>Parceiros ISV

Pode encontrar uma lista de parceiros ISV que tem adaptadas os seus componentes e as extensões para a resposta a incidentes SSIS do Azure no fim desta mensagem de blogue - [Enterprise Edition, a configuração personalizada e 3rd extensibilidade de terceiros para SSIS no ADF](https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/).

## <a name="next-steps"></a>Passos Seguintes

-   [Configuração personalizada para o tempo de execução de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Edição Enterprise do tempo de execução da integração do Azure-SSIS](how-to-configure-azure-ssis-ir-enterprise-edition.md)

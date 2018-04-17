---
title: Desenvolver pagos ou licenciados componentes para o tempo de execução de integração do Azure-SSIS | Microsoft Docs
description: Este artigo descreve como pode desenvolver um ISV e instalar paga ou está licenciado componentes personalizados para o tempo de execução de integração do Azure-SSIS
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2018
ms.author: douglasl
ms.openlocfilehash: e22ca4bd5b749e8752f800590938199e06abbd34
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="develop-paid-or-licensed-custom-components-for-the-azure-ssis-integration-runtime"></a>Desenvolver pagos ou licenciados componentes personalizados para o tempo de execução de integração do Azure-SSIS

## <a name="problem---the-azure-ssis-ir-requires-a-different-approach"></a>Problema - a resposta a incidentes SSIS do Azure requer uma abordagem diferente

A natureza do tempo de execução de integração do Azure-SSIS apresenta várias desafios, que tornam os métodos de licenciamento típicos utilizados para a instalação local dos componentes personalizados inadequados.

-   Os nós da resposta a incidentes SSIS do Azure são volátil e pode ser atribuído ou lançadas em qualquer altura. Por exemplo, pode iniciar ou parar nós para gerir o custo, ou aumentar vertical ou através de vários tamanhos de nó. Como resultado, vincula uma licença de componente de terceiros a um determinado nó através da utilização de informações específicas do computador, tais como o endereço MAC ou o ID de CPU já não é viável.

-   Também pode dimensionar a resposta a incidentes Azure SSIS entrada ou saída, para que o número de nós pode diminuir ou expanda em qualquer altura.

## <a name="solution---windows-environment-variables-and-ssis-system-variables-for-license-binding-and-validation"></a>Solução - variáveis de ambiente do Windows e variáveis do sistema SSIS para enlace da licença e validação

Como resultado as limitações dos métodos de licenciamento tradicionais, descritos na secção anterior, a resposta a incidentes SSIS do Azure fornece as variáveis de ambiente do Windows e as variáveis do sistema SSIS para o enlace de licenciamento e a validação de componentes de terceiros. Os ISVs podem utilizar estas variáveis para obter as informações exclusivas e persistente para uma resposta a incidentes SSIS do Azure, como o ID de Cluster e o número de nós de Cluster. Com esta informação, ISVs podem vincular a licença para os respetivos componentes para uma resposta a incidentes Azure SSIS *como um cluster*, com um ID não altera quando os clientes, iniciar ou parar, aumentar verticalmente ou para baixo, aumentar ou reduzir ou reconfigurar a resposta a incidentes SSIS do Azure de qualquer forma.

O diagrama seguinte mostra a instalação típica, a ativação e o enlace da licença e fluxos de validação de componentes de terceiros que utilizam estas novas variáveis:

![Instalação dos componentes licenciadas](media/how-to-configure-azure-ssis-ir-licensed-components/licensed-component-installation.png)

## <a name="instructions"></a>Instruções
1. Os ISVs podem oferecer os respetivos componentes licenciados em vários SKUs ou camadas (por exemplo, único nó, até 5 nós, até 10 nós e assim sucessivamente). O ISV fornece a chave de produto correspondente quando os clientes comprar um produto. O ISV também pode fornecer um contentor do blob Storage do Azure que contém um script de configuração de ISV e os ficheiros associados. Os clientes podem copiar estes ficheiros para os seus próprios contentor de armazenamento e modificá-las com a chave de produto (por exemplo, executando `IsvSetup.exe -pid xxxx-xxxx-xxxx`). Os clientes podem aprovisionar ou reconfigurar a resposta a incidentes SSIS do Azure com o URI de SAS do respetivo contentor como parâmetro. Para obter mais informações, consulte [configuração personalizada para o tempo de execução de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md).

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

## <a name="next-steps"></a>Passos Seguintes

-   [Configuração personalizada para o tempo de execução de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Edição Enterprise do tempo de execução da integração do Azure-SSIS](how-to-configure-azure-ssis-ir-enterprise-edition.md)
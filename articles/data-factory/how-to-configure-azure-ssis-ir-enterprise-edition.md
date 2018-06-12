---
title: Aprovisionar a edição Enterprise para o tempo de execução de integração do Azure-SSIS | Microsoft Docs
description: Este artigo descreve as funcionalidades do Enterprise Edition para o tempo de execução de integração de SSIS do Azure e como aprovisionar
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: douglasl
ms.openlocfilehash: 55f4fd18dbebe8a4c666c5512b9cad46ddf9f7d7
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296861"
---
# <a name="enterprise-edition-of-the-azure-ssis-integration-runtime"></a>Edição Enterprise do tempo de execução da integração do Azure-SSIS

A edição Enterprise do tempo de execução da integração do Azure SSIS permite-lhe utilizar que o seguinte avançado e as funcionalidades premium:
-   Alterar os componentes de captura de dados (CDC)
-   Conectores do Oracle, Teradata e SAP BW
-   Os conectores do SQL Server Analysis Services (SSAS) e Azure Analysis Services (AAS) e transformações
-   Transformações de agrupamento e de referência difusa difusa
-   Transformações de extração termo e termo de pesquisa

Algumas destas funcionalidades exigem que instale os componentes adicionais para personalizar o IR. SSIS do Azure Para obter mais informações sobre como instalar os componentes adicionais, consulte [configuração personalizada para o tempo de execução de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="enterprise-features"></a>Funcionalidades empresariais

| **Funcionalidades empresariais** | **Descrições** |
|---|---|
| Componentes de CDC | A origem de CDC, a tarefa de controlo e a transformação de divisor são pré-instalado o Azure SSIS IR Enterprise Edition. Para ligar a Oracle, também terá de instalar o estruturador de CDC e o serviço noutro computador. |
| Conectores Oracle | O Gestor de ligações do Oracle, origem e destino estão pré-instalado o Azure SSIS IR Enterprise Edition. Também terá de instalar o controlador de Interface de chamada de Oracle (OCI) e, se necessário configurar o Oracle transporte rede Substrate (TNS) no IR. SSIS do Azure Para mais informações, veja [Configuração personalizada do runtime de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Conectores Teradata | Tem de instalar o Gestor de ligações Teradata, origem e destino, bem como o controlador da API Teradata paralelas Transporter (TPT) e Teradata ODBC, sobre o Azure SSIS IR Enterprise Edition. Para mais informações, veja [Configuração personalizada do runtime de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Conectores do SAP BW | O Gestor de ligações do SAP BW, origem e destino estão pré-instalado o Azure SSIS IR Enterprise Edition. Também terá de instalar o controlador de SAP BW no IR. SSIS do Azure Estes conectores suportam SAP BW 7.0 ou versões anteriores. Para ligar a versões posteriores do SAP BW ou outros produtos do SAP, pode comprar e instalar os conectores SAP da ISVs de terceiros no IR. SSIS do Azure Para obter mais informações sobre como instalar os componentes adicionais, consulte [configuração personalizada para o tempo de execução de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Componentes do Analysis Services               | Destino formação do modelo de extração de dados, o destino de processamento de dimensão e o destino de processamento de partição, bem como a transformação de consulta de extração de dados, são pré-instalado o Azure SSIS IR Enterprise Edition. Todos estes componentes suportam o SQL Server Analysis Services (SSAS), mas apenas a partição processamento destino suporta serviços de análise do Azure (AAS). Para ligar a SSAS, terá também de [configurar as credenciais de autenticação do Windows no SSISDB](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth). Para além destes componentes, a tarefa de DDL executar do Analysis Services, a tarefa de processamento do Analysis Services e a tarefa de consulta de extração de dados são também pré-instalado o Azure SSIS IR padrão/Enterprise Edition. |
| Transformações de agrupamento e de referência difusa difusa  | As transformações difusa agrupamento e de referência difusa são pré-instalado o Azure SSIS IR Enterprise Edition. Estes componentes suportam o SQL Server e SQL Database do Azure para armazenar dados de referência. |
| Transformações de extração termo e termo de pesquisa | São pré-instalado as transformações de extração termo e termo de pesquisa do Azure-SSIS IR Enterprise Edition. Estes componentes suportam o SQL Server e SQL Database do Azure para armazenar dados de referência. |

## <a name="instructions"></a>Instruções

1.  Transfira e instale [(versão 5.4 ou posterior) do Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018).

2.  Quando aprovisionar ou reconfigurar a resposta a incidentes SSIS do Azure com o PowerShell, execute `Set-AzureRmDataFactoryV2IntegrationRuntime` com **Enterprise** como o valor para o **edição** parâmetro antes de começar a IR. SSIS do Azure Eis um exemplo de script:

    ```powershell
    $MyAzureSsisIrEdition = "Enterprise"

    Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName
                                               -Name $MyAzureSsisIrName
                                               -ResourceGroupName $MyResourceGroupName
                                               -Edition $MyAzureSsisIrEdition

    Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName
                                                 -Name $MyAzureSsisIrName
                                                 -ResourceGroupName $MyResourceGroupName
    ```

## <a name="next-steps"></a>Passos Seguintes

-   [Configuração personalizada para o tempo de execução de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Como desenvolver paga ou está licenciado componentes personalizados para o tempo de execução de integração do Azure-SSIS](how-to-develop-azure-ssis-ir-licensed-components.md)

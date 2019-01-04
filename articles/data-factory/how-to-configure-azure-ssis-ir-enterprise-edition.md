---
title: Aprovisionar a Enterprise Edition para o Runtime de integração de SSIS do Azure | Documentos da Microsoft
description: Este artigo descreve as funcionalidades do Enterprise Edition para o Runtime de integração de SSIS do Azure e como aprovisioná-lo
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: c475a9c820e577a321491b87d806ed08ddff8693
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54013882"
---
# <a name="provision-enterprise-edition-for-the-azure-ssis-integration-runtime"></a>Aprovisionar a Enterprise Edition para o Runtime de integração Azure-SSIS

A edição Enterprise do Runtime de integração Azure-SSIS permite-lhe utilizar que Advanced o seguinte e as funcionalidades premium:
-   Alterar os componentes de captura de dados (CDC)
-   Conectores SAP BW, Oracle e Teradata
-   Conectores de SQL Server Analysis Services (SSAS) e o Azure Analysis Services (AAS) e transformações
-   Transformações de Grouping e Fuzzy Lookup difusas
-   Transformações de extração de prazo e o termo de pesquisa

Alguns desses recursos exigem a instalação de componentes adicionais para personalizar o ir Azure-SSIS. Para mais informações sobre como instalar componentes adicionais, veja [configuração personalizada para o runtime de integração Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="enterprise-features"></a>Funcionalidades empresariais

| **Funcionalidades empresariais** | **Descrições** |
|---|---|
| Componentes de CDC | O CDC origem, a tarefa de controlo e a transformação de Splitter são pré-instalado no runtime de integração Azure-SSIS Enterprise Edition. Para ligar a Oracle, também tem de instalar o Designer de CDC e o serviço noutro computador. |
| Conectores do Oracle | O Gestor de ligações do Oracle, a origem e destino estão pré-instalado no runtime de integração Azure-SSIS Enterprise Edition. Também terá de instalar o controlador de Interface de chamada de Oracle (OCI) e, se for necessário configurar o Oracle transporte de rede um substrato de (TNS) sobre o ir Azure-SSIS. Para mais informações, veja [Configuração personalizada do runtime de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Conectores de Teradata | Tem de instalar o Gestor de ligações de Teradata, origem e destino, bem como o API de Teradata paralela Transporter (TPT) e Teradata o controlador ODBC, sobre o runtime de integração Azure-SSIS Enterprise Edition. Para mais informações, veja [Configuração personalizada do runtime de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Conectores SAP BW | O Gestor de ligações do SAP BW, a origem e destino estão pré-instalado no runtime de integração Azure-SSIS Enterprise Edition. Também tem de instalar o controlador de SAP BW no ir Azure-SSIS. Estes conectores suportam SAP BW 7.0 ou versões anteriores. Para ligar a versões posteriores do SAP BW ou outros produtos SAP, pode comprar e instalar os conectores SAP de ISVs de terceiros no ir Azure-SSIS. Para mais informações sobre como instalar componentes adicionais, veja [configuração personalizada para o runtime de integração Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md). |
| Componentes de serviços de análise               | O destino de treinamento de modelo de extração da dados, o destino de processamento de dimensão e o destino de processamento de partição, bem como a transformação de consulta de extração de dados, são pré-instalado no runtime de integração Azure-SSIS Enterprise Edition. Todos estes componentes suportam SQL Server Analysis Services (SSAS), mas apenas a partição de processamento destino suporta o Azure Analysis Services (AAS). Para ligar ao SSAS, terá também de [configurar credenciais de autenticação do Windows no SSISDB](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth). Além desses componentes, a tarefa de DDL de execução do Analysis Services, a tarefa de processamento do Analysis Services e a tarefa de consulta de extração de dados são também pré-instalado no runtime de integração Azure-SSIS Standard/Enterprise Edition. |
| Transformações de Grouping e Fuzzy Lookup difusas  | As transformações Fuzzy Grouping e Fuzzy Lookup são pré-instalado no runtime de integração Azure-SSIS Enterprise Edition. Esses componentes suportam o SQL Server e SQL Database do Azure para armazenar dados de referência. |
| Transformações de extração de prazo e o termo de pesquisa | As transformações de extração de prazo e o termo de pesquisa são pré-instalado no runtime de integração Azure-SSIS Enterprise Edition. Esses componentes suportam o SQL Server e SQL Database do Azure para armazenar dados de referência. |

## <a name="instructions"></a>Instruções

1.  Transfira e instale [do Azure PowerShell (versão 5.4 ou posterior)](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018).

2.  Quando aprovisionar ou reconfigurar o IR Azure-SSIS com o PowerShell, execute `Set-AzureRmDataFactoryV2IntegrationRuntime` com **Enterprise** como o valor para o **Edition** parâmetro antes de iniciar o ir Azure-SSIS. Eis um script de exemplo:

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

-   [Configuração personalizada para o runtime de integração Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Como desenvolver pago ou licenciado componentes personalizados para o runtime de integração Azure-SSIS](how-to-develop-azure-ssis-ir-licensed-components.md)

---
title: "Configurar o tempo de execução de integração do Azure-SSIS para elevado desempenho | Microsoft Docs"
description: "Saiba como configurar as propriedades do tempo de execução de integração do Azure-SSIS para elevado desempenho"
services: data-factory
ms.date: 01/10/2018
ms.topic: article
ms.service: data-factory
ms.workload: data-services
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7d0e75ad85731b10f9a993c2fa62f30c0142ed05
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="configure-the-azure-ssis-integration-runtime-for-high-performance"></a>Configurar o tempo de execução de integração do Azure-SSIS para elevado desempenho

Este artigo descreve como configurar um tempo de execução de integração de SSIS do Azure (IR) de elevado desempenho. A resposta a incidentes SSIS do Azure permite-lhe implementar e executar os pacotes do SQL Server Integration Services (SSIS) no Azure. Para obter mais informações sobre IR SSIS do Azure, consulte [integração runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime) artigo. Para obter informações sobre como implementar e executar pacotes SSIS no Azure, consulte [comparação de precisão shift SQL Server Integration Services cargas de trabalho e para a nuvem](/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview).

> [!IMPORTANT]
> Este artigo contém resultados de desempenho e as observações testar internamente efetuada por membros da equipa de desenvolvimento de SSIS. Os resultados podem variar. Efetue os seus próprios testes antes de finalizar as definições de configuração que afetam o desempenho e custo.

## <a name="properties-to-configure"></a>Propriedades para configurar

A seguinte parte de um script de configuração mostra as propriedades que pode configurar quando criar um tempo de execução de integração de SSIS do Azure. Para o script do PowerShell concluída e a descrição, consulte [pacotes de implementar o SQL Server Integration Services para o Azure](tutorial-deploy-ssis-packages-azure.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS IR>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS, NorthEurope, and WestEurope are supported.
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_D3_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>"
```

## <a name="azuressislocation"></a>AzureSSISLocation
**AzureSSISLocation** é a localização para o nó de trabalho de runtime de integração. O nó de trabalho mantém uma constante a ligação ao catálogo de SSIS base de dados (SSISDB) uma base de dados SQL do Azure. Definir o **AzureSSISLocation** para a mesma localização que o servidor de base de dados do SQL Server que aloja SSISDB, que permite que o tempo de execução de integração para trabalharem eficientemente possível.

## <a name="azuressisnodesize"></a>AzureSSISNodeSize
A pré-visualização pública do Azure Data Factory v2, incluindo IR SSIS do Azure, suporta as seguintes opções:
-   Standard\_A4\_v2
-   Standard\_A8\_v2
-   Standard\_D1\_v2
-   Standard\_D2\_v2
-   Standard\_D3\_v2
-   Standard\_D4\_v2.

Nos internas testes de pela equipa de engenharia do SSIS, a série de D parece ser mais adequada para a execução de pacotes SSIS do que a série de um.

-   O rácio de desempenho/preços da série de D é superior da série de um.
-   O débito para a série de D é superior da série de um pelo mesmo preço.

### <a name="configure-for-execution-speed"></a>Configurar a velocidade de execução
Se não tiver o número de pacotes para executar e pretende que os pacotes para executar rapidamente, utilize as informações no gráfico seguinte para escolher um tipo de máquina virtual adequado para o seu cenário.

Estes dados representa uma execução de pacote único num nó único do worker. O pacote carrega 10 milhões de registos com o nome próprio e o último colunas de nome do Blob Storage do Azure, gera uma coluna de nome completo e escreve os registos com o nome completo mais de 20 carateres para o Blob Storage do Azure.

![Velocidade de execução do pacote de tempo de execução de integração de SSIS](media/configure-azure-ssis-integration-runtime-performance/ssisir-execution-speed.png)

### <a name="configure-for-overall-throughput"></a>Configurar o débito global

Se tiver muitos pacotes para executar e são mais importantes sobre o débito global, utilize as informações no gráfico seguinte para escolher um tipo de máquina virtual adequado para o seu cenário.

![Débito máximo global de tempo de execução de integração de SSIS](media/configure-azure-ssis-integration-runtime-performance/ssisir-overall-throughput.png)

## <a name="azuressisnodenumber"></a>AzureSSISNodeNumber

**AzureSSISNodeNumber** ajusta a escalabilidade do tempo de execução de integração. O débito do tempo de execução de integração é proporcional à **AzureSSISNodeNumber**. Definir o **AzureSSISNodeNumber** para um valor pequeno em primeiro lugar, monitorizar o débito do tempo de execução de integração, em seguida, ajuste o valor para o seu cenário. Para reconfigurar a contagem de nós de trabalho, consulte [gerir um tempo de execução de integração do Azure-SSIS](manage-azure-ssis-integration-runtime.md).

## <a name="azuressismaxparallelexecutionspernode"></a>AzureSSISMaxParallelExecutionsPerNode

Quando já estiver a utilizar um nó de trabalho de elevado desempenho para executar os pacotes, aumentando **AzureSSISMaxParallelExecutionsPerNode** pode aumentar o débito global do tempo de execução de integração. Standard_D1_v2 nós, de 1 a 4 execuções paralelas por nó são suportadas. Para todos os outros tipos de nós, são suportadas 1-8 execuções paralelas por nó.
Pode calcular o valor adequado baseado no custo do seu pacote e as seguintes configurações para nós de trabalho. Para obter mais informações, consulte [tamanhos de máquinas de virtuais para fins gerais](../virtual-machines/windows/sizes-general.md).

| Tamanho             | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | Débito do armazenamento temporário máximo: IOPS/MBps de Leitura/MBps de Escrita | Máximo do disco de dados/débito: IOPS | NICs. Máx. / Desempenho de rede esperado (Mbps) |
|------------------|------|-------------|------------------------|------------------------------------------------------------|-----------------------------------|------------------------------------------------|
| Standard\_D1\_v2 | 1    | 3.5         | 50                     | 3000 / 46 / 23                                             | 2 / 2x500                         | 2 / 750                                        |
| Standard\_D2\_v2 | 2    | 7           | 100                    | 6000 / 93 / 46                                             | 4 / 4x500                         | 2 / 1500                                       |
| Standard\_D3\_v2 | 4    | 14          | 200                    | 12000 / 187 / 93                                           | 8 / 8x500                         | 4 / 3000                                       |
| Standard\_D4\_v2 | 8    | 28          | 400                    | 24000 / 375 / 187                                          | 16 / 16x500                       | 8 / 6000                                       |
| Standard\_A4\_v2 | 4    | 8           | 40                     | 4000 / 80 / 40                                             | 8 / 8x500                         | 4 / 1000                                       |
| Standard\_A8\_v2 | 8    | 16          | 80                     | 8000 / 160 / 80                                            | 16 / 16x500                       | 8 / 2000                                       |

Seguem-se as diretrizes para definir o valor da direita para a **AzureSSISMaxParallelExecutionsPerNode** propriedade: 

1. Defina-o como um valor pequeno em primeiro lugar.
2. Aumente este uma pequena quantidade para verificar se o débito global é melhorado.
3. Pare a aumentar o valor quando o débito global atinge o valor máximo.

## <a name="ssisdbpricingtier"></a>SSISDBPricingTier

**SSISDBPricingTier** é o escalão de preço para o catálogo de SSIS base de dados (SSISDB) uma base de dados SQL do Azure. Esta definição afeta o número máximo dos funcionários da instância de resposta a incidentes, a velocidade em fila uma execução de pacote e a velocidade ao carregar o registo de execução.

-   Se não se preocupar velocidade para execução de pacotes de fila e para carregar o registo de execução, pode escolher o escalão de preço mais baixa da base de dados. SQL Database do Azure, com preços básico suporta 8 trabalhadores uma instância de tempo de execução de integração.

-   Escolha uma base de dados mais poderoso que básico se a contagem de trabalho é mais 8 ou o número de núcleos é mais de 50. Caso contrário, a base de dados torna-se o valor da instância do tempo de execução de integração e o desempenho global é afetado negativamente.

Pode também ajustar a base de dados com base do escalão de preço [unidade de transação de base de dados](../sql-database/sql-database-what-is-a-dtu.md) (DTU) informações de utilização disponíveis no portal do Azure.

## <a name="design-for-high-performance"></a>Conceber o elevado desempenho
Conceber um pacote SSIS para ser executada no Azure é diferente do conceber um pacote para execução no local. Em vez de combinar várias tarefas independentes no mesmo pacote, separá-los em pacotes de vários para execução mais eficiente no IR. SSIS do Azure Crie uma execução de pacotes para cada pacote, para que não têm de aguardar por si concluir. Esta abordagem beneficia de escalabilidade do tempo de execução de integração do Azure-SSIS e melhora o débito global.

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre o tempo de execução de integração de SSIS do Azure. Consulte [Runtime de integração do Azure-SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime).
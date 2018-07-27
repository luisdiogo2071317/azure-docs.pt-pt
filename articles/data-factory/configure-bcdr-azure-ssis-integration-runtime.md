---
title: Recomendações de recuperação (BCDR) de desastre e continuidade comercial para o Runtime de integração de SSIS do Azure | Documentos da Microsoft
description: Este artigo descreve as recomendações de recuperação de desastre e continuidade comercial para o Runtime de integração Azure-SSIS.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/26/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 37347df2d543116085f52fed76c692b60fac2ad6
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285863"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-recommendations-for-azure-ssis-integration-runtime"></a>Recomendações de recuperação (BCDR) de desastre e continuidade comercial para o Runtime de integração Azure-SSIS

Para fins de recuperação após desastre, pode parar o runtime de integração Azure-SSIS na região em que está em execução e mudar para outra região para iniciá-lo novamente. Recomendamos que utilize [regiões emparelhadas do Azure](../best-practices-availability-paired-regions.md) para esta finalidade.

## <a name="prerequisites"></a>Pré-requisitos

- Certifique-se de que ativou a recuperação após desastre para o seu servidor de base de dados do Azure SQL no caso do servidor tem um período de indisponibilidade, ao mesmo tempo. Para mais informações, veja [descrição geral da continuidade do negócio com a base de dados do Azure SQL](../sql-database/sql-database-business-continuity.md).

- Se estiver a utilizar uma rede virtual na região atual, terá de utilizar outra rede virtual na nova região para ligar o runtime de integração Azure-SSIS. Para mais informações, veja [associar um runtime de integração Azure-SSIS a uma rede virtual](join-azure-ssis-integration-runtime-virtual-network.md).

- Se estiver a utilizar uma configuração personalizada, terá de preparar noutro URI de SAS para o contentor de BLOBs que armazena o seu script de configuração personalizada e os ficheiros associados, para que ele continua a estar acessíveis durante um período de indisponibilidade. Para mais informações, veja [configurar uma configuração personalizada num runtime de integração Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="steps"></a>Passos

Siga estes passos para parar o runtime de integração Azure-SSIS, mude o IR para uma nova região e iniciá-lo novamente.

1. Pare o runtime de integração na região original.

2. Chamar o seguinte comando no PowerShell para atualizar o runtime de integração

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

    Para obter mais informações sobre este comando do PowerShell, consulte [criar o runtime de integração Azure-SSIS no Azure Data Factory](create-azure-ssis-integration-runtime.md)

3. Inicie novamente o runtime de integração.

## <a name="next-steps"></a>Passos Seguintes

Considere estas outras opções de configuração para o IR Azure-SSIS:

- [Configurar o Runtime de integração Azure-SSIS para elevado desempenho](configure-azure-ssis-integration-runtime-performance.md)

- [Personalizar a configuração para o runtime de integração Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Aprovisionar a Enterprise Edition para o Runtime de integração Azure-SSIS](how-to-configure-azure-ssis-ir-enterprise-edition.md)

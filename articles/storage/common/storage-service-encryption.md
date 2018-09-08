---
title: Encriptação do serviço de armazenamento do Azure para dados Inativos | Documentos da Microsoft
description: Utilize a funcionalidade de encriptação de serviço de armazenamento do Azure para encriptar os Managed Disks do Azure, o armazenamento de Blobs do Azure, o ficheiros do Azure, o armazenamento de filas do Azure e o armazenamento de tabelas do Azure no lado do serviço ao armazenar os dados e decifrá-la ao obter os dados.
services: storage
author: lakasa
ms.service: storage
ms.topic: article
ms.date: 08/01/2018
ms.author: lakasa
ms.component: common
ms.openlocfilehash: c6001b0c95186dff53f33c73833b4cea30688db2
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094897"
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Encriptação do serviço de armazenamento do Azure para dados Inativos
Encriptação do serviço de armazenamento do Azure para dados Inativos ajuda a proteger os seus dados para cumprir os seus compromissos de conformidade e segurança organizacionais. Com esta funcionalidade, a plataforma de armazenamento do Azure encripta automaticamente os dados antes de persisti-los para Managed Disks do Azure, armazenamento de Blobs do Azure, filas ou tabelas ou ficheiros do Azure e descriptografa os dados antes da obtenção. O processamento de encriptação, encriptação em repouso, desencriptação e gestão de chaves de encriptação do serviço de armazenamento é transparente para os utilizadores. Todos os dados escritos para a plataforma de armazenamento do Azure são criptografados por meio de 256 bits [encriptação AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), um bloco mais cifras disponíveis.

Encriptação do serviço de armazenamento está ativada para todas as contas de armazenamento de novas e existentes e não pode ser desativada. Porque os seus dados estão protegidos por predefinição, não terá de modificar as suas aplicações para tirar partido da encriptação do serviço de armazenamento ou de código.

A funcionalidade encripta automaticamente dados em:

- Serviços de armazenamento do Azure:
    - Managed Disks do Azure
    - Armazenamento de Blobs do Azure
    - Ficheiros do Azure
    - Armazenamento de filas do Azure
    - Armazenamento de tabelas do Azure.  
- Ambos os escalões de desempenho (Standard e Premium).
- Ambos os modelos de implementação (Azure Resource Manager e clássica).

Encriptação do serviço de armazenamento não afeta o desempenho dos serviços de armazenamento do Azure.

Pode utilizar chaves de encriptação gerida pela Microsoft com a encriptação do serviço de armazenamento, ou pode usar suas próprias chaves de encriptação. Para obter mais informações sobre como utilizar as suas próprias chaves, consulte [encriptação do serviço de armazenamento a utilizar chaves geridas pelo cliente no Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Ver definições de encriptação no portal do Azure
Para ver as definições de encriptação do serviço de armazenamento, inicie sessão para o [portal do Azure](https://portal.azure.com) e selecione uma conta de armazenamento. Na **configurações** painel, selecione a **encriptação** definição.

![Portal captura de ecrã que mostra a definição de encriptação](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>FAQ sobre a encriptação do serviço de armazenamento
**Como posso encriptar os dados numa conta de armazenamento do Resource Manager?**  
Encriptação do serviço de armazenamento está ativada para todas as contas de armazenamento – clássicas e Resource Manager, quaisquer ficheiros existentes na conta de armazenamento criada antes de ativar a encriptação serão retroativamente obtém encriptada por um processo de encriptação em segundo plano.

**É Storage Service Encryption habilitado por padrão ao criar uma conta de armazenamento?**  
Sim, o Storage Service Encryption está ativado para todas as contas de armazenamento e para todos os serviços de armazenamento do Azure.

**Tenho uma conta de armazenamento do Resource Manager. Pode ativar encriptação do serviço de armazenamento na mesma?**  
Encriptação do serviço de armazenamento é ativada por predefinição em todas as contas de armazenamento do Resource Manager existente. Isto é suportado para o armazenamento de filas do Azure de armazenamento, ficheiros do Azure, BLOBs do Azure, armazenamento de tabelas. 

**Pode desativar encriptação na minha conta de armazenamento?**  
Encriptação está ativada por predefinição e não existe nenhuma provisão para desativar a encriptação para a sua conta de armazenamento. 

**Quanto mais custa armazenamento do Azure se estiver ativada a encriptação do serviço de armazenamento?**  
Não existe nenhum custo adicional.

**Pode utilizar as minhas chaves de encriptação?**  
Sim, para armazenamento de Blobs do Azure e ficheiros do Azure, pode usar suas próprias chaves de encriptação. Chaves geridas pelo cliente não são atualmente suportadas pelo Azure Managed Disks. Para obter mais informações, consulte [encriptação do serviço de armazenamento a utilizar chaves geridas pelo cliente no Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

**Pode revogar o acesso às chaves de encriptação?**  
Sim, se [usar suas próprias chaves de encriptação](storage-service-encryption-customer-managed-keys.md) no Azure Key Vault.

**Como é diferente do Azure Disk Encryption Storage Service Encryption?**  
O Azure Disk Encryption fornece integração entre soluções baseadas no sistema operacional como o BitLocker e o DM-Crypt e o Cofre de chaves do Azure. Encriptação do serviço de armazenamento fornece encriptação nativamente na camada de plataforma de armazenamento do Azure, abaixo da máquina virtual.

**Tenho uma conta de armazenamento clássico. Pode ativar encriptação do serviço de armazenamento na mesma?**  
Encriptação do serviço de armazenamento está ativada para todas as contas de armazenamento (clássica e Resource Manager).

**Como posso encriptar dados na minha conta de armazenamento clássicas?**  
Com encriptação ativada por predefinição, todos os dados armazenados nos serviços de armazenamento do Azure são automaticamente encriptados. 

**Pode criar contas de armazenamento com encriptação do serviço de armazenamento ativada com o Azure PowerShell e CLI do Azure?**  
Encriptação do serviço de armazenamento é ativada por predefinição no momento da criação de qualquer conta de armazenamento (clássico ou Resource Manager). Pode verificar as propriedades da conta com o Azure PowerShell e CLI do Azure.

**Minha conta de armazenamento está configurada para ser replicada geo adquiria. Com encriptação do serviço de armazenamento, serão minha cópia georredundante também encriptada?**  
Sim, todas as cópias da conta de armazenamento são encriptadas. Redundância todas as opções são suportadas – armazenamento localmente redundante, o armazenamento com redundância de zona, o armazenamento georredundante e o armazenamento georredundante com acesso de leitura.

**Encriptação do serviço de armazenamento é permitida apenas em regiões específicas?**  
Encriptação do serviço de armazenamento está disponível em todas as regiões.

**Storage Service Encryption FIPS 140-2 está em conformidade?**  
Sim, o Storage Service Encryption é FIPS 140-2 em conformidade.

**Como posso contactar alguém se eu tiver quaisquer problemas ou pretender fornecer comentários?**  
Contacte [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) para quaisquer problemas ou feedback relacionado com a encriptação do serviço de armazenamento.

## <a name="next-steps"></a>Passos Seguintes
Armazenamento do Azure fornece que um conjunto abrangente de recursos de segurança que os desenvolvedores disso ajuda a criar aplicativos seguros. Para obter mais informações, consulte a [guia de segurança de armazenamento](../storage-security-guide.md).

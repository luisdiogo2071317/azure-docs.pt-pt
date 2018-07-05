---
title: Encriptação do serviço de armazenamento do Azure para dados Inativos | Documentos da Microsoft
description: Utilize a funcionalidade de encriptação de serviço de armazenamento do Azure para encriptar o armazenamento de Blobs do Azure no lado do serviço ao armazenar os dados e decifrá-la ao obter os dados.
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 06/12/2018
ms.author: lakasa
ms.openlocfilehash: d469dfb5092f1269a6600ee8ee2f81778fd83b96
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450091"
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Encriptação do Serviço de Armazenamento do Azure para Dados Inativos

Encriptação do serviço de armazenamento do Azure para dados Inativos ajuda a proteger os seus dados para cumprir os seus compromissos de conformidade e segurança organizacionais. Com esta funcionalidade, o armazenamento do Azure encripta automaticamente os dados antes de persisti-los ao armazenamento do Azure e descriptografa os dados antes da obtenção. O processamento de encriptação, encriptação em repouso, desencriptação e gestão de chaves de encriptação do serviço de armazenamento é transparente para os utilizadores. Todos os dados escritos no armazenamento do Azure são criptografados por meio de 256 bits [encriptação AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), um bloco mais cifras disponíveis.

Encriptação do serviço de armazenamento está ativada para todas as contas de armazenamento de novas e existentes e não pode ser desativada. Porque os seus dados estão protegidos por predefinição, não terá de modificar as suas aplicações para tirar partido da encriptação do serviço de armazenamento ou de código.

A funcionalidade encripta automaticamente dados em:

- Ambos os escalões de desempenho (Standard e Premium).
- Ambos os modelos de implementação (Azure Resource Manager e clássica).
- Serviços de todos os de armazenamento do Azure (armazenamento de BLOBs, armazenamento de filas, o armazenamento de tabelas e ficheiros do Azure). 

Encriptação do serviço de armazenamento não afeta o desempenho de armazenamento do Azure.

Pode utilizar chaves de encriptação gerida pela Microsoft com a encriptação do serviço de armazenamento, ou pode usar suas próprias chaves de encriptação. Para obter mais informações sobre como utilizar as suas próprias chaves, consulte [encriptação do serviço de armazenamento a utilizar chaves geridas pelo cliente no Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Ver definições de encriptação no portal do Azure

Para ver as definições de encriptação do serviço de armazenamento, inicie sessão para o [portal do Azure](https://portal.azure.com) e selecione uma conta de armazenamento. Na **configurações** painel, selecione a **encriptação** definição.

![Portal captura de ecrã que mostra a definição de encriptação](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>FAQ sobre a encriptação do serviço de armazenamento

**P: Tenho uma conta de armazenamento clássico. Pode ativar encriptação do serviço de armazenamento na mesma?**

R: Storage Service Encryption está ativado para todas as contas de armazenamento (clássica e Resource Manager).

**P: como pode criptografar dados em minha conta de armazenamento clássicas?**

R: com encriptação ativada por predefinição, o armazenamento do Azure encripta automaticamente os dados de novo. 

**P: Tenho uma conta de armazenamento do Resource Manager. Pode ativar encriptação do serviço de armazenamento na mesma?**

R: storage Service Encryption está ativada por predefinição em todas as contas de armazenamento do Resource Manager existente. Isto é suportado para o armazenamento de BLOBs, armazenamento de tabelas, armazenamento de filas e ficheiros do Azure. 

**P: como posso encriptar os dados numa conta de armazenamento do Resource Manager?**

R: Storage Service Encryption está ativado para todas as contas de armazenamento – clássicas e Resource Manager, quaisquer ficheiros existentes na conta de armazenamento criada antes de ativar a encriptação serão retroativamente obtém encriptada por um processo de encriptação em segundo plano.

**P: pode criar contas de armazenamento com encriptação do serviço de armazenamento ativada com o Azure PowerShell e CLI do Azure?**

R: Storage Service Encryption está ativada por predefinição no momento da criação de qualquer conta de armazenamento (clássico ou Resource Manager). Pode verificar as propriedades da conta com o Azure PowerShell e CLI do Azure.

**P: como muito mais custa armazenamento do Azure se estiver ativada a encriptação do serviço de armazenamento?**

R: não existe nenhum custo adicional.

**P: Posso utilizar minhas própria chaves de encriptação?**

R: Sim, pode usar suas próprias chaves de encriptação. Para obter mais informações, consulte [encriptação do serviço de armazenamento a utilizar chaves geridas pelo cliente no Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

**P: posso revogar o acesso às chaves de encriptação?**

R: Sim, se [usar suas próprias chaves de encriptação](storage-service-encryption-customer-managed-keys.md) no Azure Key Vault.

**P: está ativada por predefinição de encriptação do serviço de armazenamento ao criar uma conta de armazenamento?**

R: Sim, o Storage Service Encryption está ativado para todas as contas de armazenamento e para todos os serviços de armazenamento do Azure.

**P: como isso é diferente do Azure Disk Encryption?**

R: o azure Disk Encryption é utilizado para encriptar discos de SO e dados em VMs de IaaS. Para obter mais informações, consulte a [guia de segurança de armazenamento](../storage-security-guide.md).

**P: E se posso ativar o Azure Disk Encryption em meus discos de dados?**

R: Isto irá funcionar de forma totalmente integrada. Ambos os métodos irão criptografar seus dados.

**P: a minha conta de armazenamento está configurada para ser replicada geo adquiria. Com encriptação do serviço de armazenamento, serão minha cópia georredundante também encriptada?**

R: Sim, todas as cópias da conta de armazenamento são encriptadas. Redundância todas as opções são suportadas – armazenamento localmente redundante, o armazenamento com redundância de zona, o armazenamento georredundante e o armazenamento georredundante com acesso de leitura.

**P: posso desativar a encriptação na minha conta de armazenamento?**

R: encriptação está ativada por predefinição e não existe nenhuma provisão para desativar a encriptação para a sua conta de armazenamento. 

**P: é permitida a Storage Service Encryption apenas em regiões específicas?**

R: storage Service Encryption está disponível em todas as regiões para todos os serviços.

**P: está em conformidade com FIPS 140-2 de encriptação do serviço de armazenamento?**

R: Sim, o Storage Service Encryption é FIPS 140-2 em conformidade.

**P: como posso contactar alguém se eu tiver quaisquer problemas ou pretender fornecer comentários?**

R: contacto [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) para quaisquer problemas ou feedback relacionado com a encriptação do serviço de armazenamento.

## <a name="next-steps"></a>Passos Seguintes
Armazenamento do Azure fornece que um conjunto abrangente de recursos de segurança que os desenvolvedores disso ajuda a criar aplicativos seguros. Para obter mais informações, consulte a [guia de segurança de armazenamento](../storage-security-guide.md).

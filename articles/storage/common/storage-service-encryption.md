---
title: "Encriptação do serviço de armazenamento do Azure para dados Inativos | Microsoft Docs"
description: "Utilize a funcionalidade de encriptação do serviço do Storage do Azure para encriptar o armazenamento de Blobs do Azure no lado do serviço ao armazenar os dados e desencriptá-lo ao obter os dados."
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/06/2018
ms.author: lakasa
ms.openlocfilehash: 6b56cbb4220ce1c8767724938dd531b8ae5c3920
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Encriptação do Serviço de Armazenamento do Azure para Dados Inativos

Encriptação do serviço de armazenamento do Azure para dados Inativos ajuda a proteger os seus dados para satisfazer os seus compromissos de conformidade e segurança organizacional. Com esta funcionalidade, o Storage do Azure encripta automaticamente os dados antes a persistência ao Storage do Azure e desencripta os dados antes de obtenção. O processamento de encriptação, rest, desencriptação e gestão de chaves na encriptação do serviço de armazenamento a encriptação é transparente para os utilizadores. Todos os dados escritos para armazenamento do Azure são encriptados através de 256 bits [encriptação AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), um bloco de maior cifras disponíveis.

Encriptação do serviço de armazenamento está ativada para todas as contas de armazenamento novas e existentes e não pode ser desativada. Porque os dados estão protegidos por predefinição, não precisa de modificar o código ou aplicações para tirar partido da encriptação do serviço de armazenamento.

A funcionalidade encripta automaticamente dados em:

- Ambas as camadas de desempenho (Standard e Premium).
- Ambos os modelos de implementação (Azure Resource Manager e clássico).
- Serviços de todos os do Storage do Azure (armazenamento de BLOBs, armazenamento de filas, armazenamento de tabelas e ficheiros do Azure). 

Encriptação do serviço de armazenamento não afeta o desempenho de armazenamento do Azure.

Pode utilizar as chaves de encriptação gerida pela Microsoft através da encriptação do serviço de armazenamento ou pode utilizar as suas próprias chaves de encriptação. Para obter mais informações sobre como utilizar as suas próprias chaves, consulte [encriptação do serviço de armazenamento gerida pelo cliente chaves a utilizar no Cofre de chaves do Azure](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Ver definições de encriptação no portal do Azure

Para ver as definições de encriptação do serviço de armazenamento, inicie sessão no [portal do Azure](https://portal.azure.com) e selecione uma conta de armazenamento. No **definições** painel, selecione o **encriptação** definição.

![Portal captura de ecrã que mostra a definição de encriptação](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>FAQ acerca de encriptação do serviço de armazenamento

**P: Posso ter uma conta de armazenamento clássico. Pode ativar encriptação do serviço de armazenamento na mesma?**

R: encriptação do serviço de armazenamento está ativada por predefinição para todas as contas de armazenamento (clássica e Resource Manager).

**P: como pode encriptar dados na minha conta de armazenamento clássico?**

R: com encriptação ativada por predefinição, o Storage do Azure encripta automaticamente os dados de novo. 

**P: Posso ter uma conta de armazenamento do Resource Manager. Pode ativar encriptação do serviço de armazenamento na mesma?**

R: encriptação do serviço de armazenamento de está ativada por predefinição em todas as contas de armazenamento do Resource Manager existente. Isto é suportado para o Blob storage, o Table storage, armazenamento de filas e ficheiros do Azure. 

**P: como encriptam os dados numa conta de armazenamento do Resource Manager?**

R: encriptação do serviço de armazenamento está ativada por predefinição para todas as contas de armazenamento – clássicas e Resource Manager. No entanto, os dados existentes não são encriptados. Para encriptar os dados existentes, pode copiá-lo para outro nome ou outro contentor e, em seguida, remover as versões não encriptadas. 

**P: pode criar contas de armazenamento com a encriptação do serviço de armazenamento ativado ao utilizar o Azure PowerShell e da CLI do Azure?**

R: encriptação do serviço de armazenamento está ativada por predefinição no momento da criação de qualquer conta de armazenamento (clássico ou do Resource Manager). Pode verificar as propriedades da conta através da utilização do Azure PowerShell e CLI do Azure.

**P: quanto mais armazenamento do Azure custo se estiver ativada a encriptação do serviço de armazenamento?**

R: não há sem custos adicionais.

**P: quem gere as chaves de encriptação?**

R: Microsoft gere as chaves.

**P: Posso utilizar o meus próprio chaves de encriptação?**

R: neste momento, não.

**P: posso revogar o acesso às chaves de encriptação?**

R: neste momento, não. Microsoft totalmente gere as chaves.

**P: está ativada por predefinição de encriptação do serviço de armazenamento, quando criar uma conta de armazenamento?**

R: Sim, a encriptação do serviço de armazenamento (através de chaves gerida pela Microsoft) está ativada por predefinição para todas as contas de armazenamento – Azure Resource Manager e clássico. Foi ativada para todos os serviços, bem como – armazenamento de BLOBs, o Table storage, armazenamento de filas e ficheiros do Azure.

**P: como é diferente do Azure Disk Encryption?**

R: azure Disk Encryption é utilizado para encriptar o SO e discos de dados em VMs do IaaS. Para obter mais informações, consulte o [manual de segurança de armazenamento](../storage-security-guide.md).

**P: E se ativar Azure Disk Encryption no meu discos de dados?**

R: Isto irá funcionar de forma totalmente integrada. Ambos os métodos encripta os seus dados.

**P: minha conta de armazenamento é configurada para ser replicado georreplicação redundantly. Com a encriptação do serviço de armazenamento, será minha cópia redundante também será encriptada?**

R: Sim, todas as cópias da conta do storage são encriptadas. Redundância todas as opções são suportadas – armazenamento localmente redundante, o armazenamento com redundância de zona, o armazenamento georredundante e o armazenamento georredundante com acesso de leitura.

**P: pode desativar encriptação na minha conta de armazenamento?**

R: encriptação está ativada por predefinição e não há nenhum aprovisionar para desativar a encriptação para a sua conta de armazenamento. 

**P: é a encriptação do serviço de armazenamento permitida apenas em regiões específicas?**

R: encriptação do serviço de armazenamento de está disponível em todas as regiões para todos os serviços. 

**P: como posso o contacto certo se posso tiver algum problema ou quiser fornecer comentários?**

R: contacto [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) para quaisquer problemas ou comentários relacionados com a encriptação do serviço de armazenamento.

## <a name="next-steps"></a>Passos Seguintes
Storage do Azure fornece que um abrangentes conjunto de capacidades de segurança que os programadores de ajuda em conjunto compilação de aplicações seguras. Para obter mais informações, consulte o [manual de segurança de armazenamento](../storage-security-guide.md).

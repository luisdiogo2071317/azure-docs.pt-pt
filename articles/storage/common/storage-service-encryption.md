---
title: "Encriptação do serviço de armazenamento do Azure para dados Inativos | Microsoft Docs"
description: "Utilizar a funcionalidade de encriptação do serviço do Storage do Azure para encriptar o armazenamento de Blobs do Azure no lado do serviço ao armazenar os dados e desencriptá-lo ao obter os dados."
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/06/2018
ms.author: lakasa
ms.openlocfilehash: 29f6a38f7a7f0f107dab8c99e750a8dec803f6f0
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Encriptação do Serviço de Armazenamento do Azure para Dados Inativos

Azure armazenamento serviço encriptação (SSE) para dados Inativos ajuda a proteger e a salvaguardar os seus dados para satisfazer os seus compromissos de conformidade e segurança organizacional. Com esta funcionalidade, o Storage do Azure encripta automaticamente os dados antes a persistência ao Storage do Azure e desencripta a mesma antes da obtenção. O processamento de encriptação, a encriptação rest, desencriptação e gestão de chaves fornecida pelo SSE são transparente para os utilizadores. Todos os dados escritos para armazenamento do Azure são encriptados utilizando 256 bits [encriptação AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), um bloco de maior cifras disponíveis.

SSE está ativado para todas as contas de armazenamento novas e existentes e não pode ser desativado. Porque os dados estão protegidos por predefinição, não terá de modificar o código ou aplicações para tirar partido de SSE.

 SSE encripta automaticamente dados em todos os escalões de desempenho (Standard e Premium), todos os modelos de implementação (Azure Resource Manager e clássico) e todos os serviços de armazenamento do Azure (Blob, fila, tabela e ficheiro). 

Pode utilizar as chaves de encriptação gerida pela Microsoft com SSE ou pode utilizar as suas próprias chaves de encriptação. Para obter mais informações sobre como utilizar as suas próprias chaves, consulte [utilizando o cliente de encriptação do serviço de armazenamento de chaves geridas no Cofre de chaves do Azure](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Ver definições de encriptação no portal do Azure

Para ver as definições de encriptação do serviço de armazenamento, inicie sessão no [portal do Azure](https://portal.azure.com) e selecione uma conta de armazenamento. No **definições** painel, clique na definição de encriptação.

![Portal captura de ecrã com a opção de encriptação](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>FAQ acerca de encriptação do serviço de armazenamento

**P: Posso ter uma conta de armazenamento clássicas existente. Pode ativar SSE no mesmo?**

R: SSE está ativada por predefinição para todas as contas de armazenamento (contas de armazenamento clássica e Resource Manager).

**P: como pode encriptar dados na minha conta de armazenamento clássico?**

R: com encriptação ativada por predefinição, os novos dados são automaticamente encriptados pelo serviço de armazenamento. 

**P: Posso ter uma conta de armazenamento do Resource Manager existente. Pode ativar SSE no mesmo?**

R: SSE está ativada por predefinição em todas as contas de armazenamento do Resource Manager existente. Isto é suportado para Blobs, tabelas, ficheiros e fila de armazenamento. 

**P: gostaria encriptar os dados atuais numa conta de armazenamento do Resource Manager existente?**

R: com SSE ativada por predefinição para todas as contas de armazenamento - clássica e Resource Manager contas de armazenamento. No entanto, os dados que já foram presentes não serão encriptados. Para encriptar os dados existentes, pode copiá-los para outro nome ou outro contentor e, em seguida, remover as versões não encriptadas. 

**P: pode criar novas contas de armazenamento com SSE ativada com o Azure PowerShell e da CLI do Azure?**

R: SSE está ativada por predefinição no momento da criação de qualquer conta de armazenamento (contas clássica e Resource Manager). Pode verificar as propriedades da conta a utilizar o Azure PowerShell e a CLI do Azure.

**P: quanto mais armazenamento do Azure custo se SSE está ativado?**

R: não há sem custos adicionais.

**P: quem gere as chaves de encriptação?**

R: as chaves são geridas pela Microsoft.

**P: Posso utilizar o meus próprio chaves de encriptação?**

R: estamos a trabalhar para fornecer capacidades para os clientes movam as suas próprias chaves de encriptação.

**P: posso revogar o acesso às chaves de encriptação?**

R: não neste momento; as chaves são completamente geridas pela Microsoft.

**P: é SSE ativada por predefinição, quando criar uma nova conta de armazenamento?**

R: Sim, SSE utilizando chaves Microsoft Managed está ativada por predefinição para todas as contas de armazenamento - Azure Resource Manager e de contas do storage clássicas. Está ativada para todos os serviços, bem como - do Blob, tabela, fila e armazenamento de ficheiros.

**P: como é diferente do Azure Disk Encryption?**

R: azure Disk Encryption é utilizado para encriptar o SO e discos de dados em VMs do IaaS. Para obter mais detalhes, visite o nosso [manual de segurança de armazenamento](../storage-security-guide.md).

**P: E se ativar Azure Disk Encryption no meu discos de dados?**

R: Isto irá funcionar de forma totalmente integrada. Os dados serão encriptados por ambos os métodos.

**P: minha conta de armazenamento é configurada para ser replicado georreplicação redundantly. Com SSE, será minha cópia redundante também será encriptada?**

R: Sim, todas as cópias da conta do storage são encriptadas e todas as opções de redundância – localmente redundante armazenamento (LRS), armazenamento Redundant de zona (ZRS), armazenamento Georredundante (GRS) e o armazenamento Georredundante com acesso de leitura (RA-GRS) – são suportadas.

**P: Posso não é possível desativar a encriptação na minha conta de armazenamento.**

R: encriptação está ativada por predefinição e não há nenhum aprovisionar para desativar a encriptação para a sua conta de armazenamento. 

**P: SSE só é permitida em regiões específicas?**

R: o SSE está disponível em todas as regiões para todos os serviços. 

**P: como posso o contacto certo se posso ter algum problema ou quiser fornecer comentários?**

R: contacto [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) para quaisquer problemas relacionados com a encriptação do serviço de armazenamento.

## <a name="next-steps"></a>Próximos Passos
Storage do Azure fornece um conjunto completo de capacidades de segurança, que em conjunto permitem aos programadores criarem aplicações seguras. Para obter mais detalhes, visite o [manual de segurança de armazenamento](../storage-security-guide.md).

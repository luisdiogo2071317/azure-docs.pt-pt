---
title: Funcionalidades de segurança que podem ser utilizadas com o Storage do Azure | Microsoft Docs
description: Este artigo fornece uma descrição geral das principais funcionalidades de segurança do Azure que podem ser utilizadas com o Storage do Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 9558f1ec0d8ccd83da764a0967fa83d93e1e6a02
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34365383"
---
# <a name="azure-storage-security-overview"></a>Descrição geral de segurança de armazenamento do Azure
O Azure Storage é a solução de armazenamento na cloud para aplicações modernas que dependem da durabilidade, da disponibilidade e da escalabilidade para satisfazer as necessidades dos seus clientes. Storage do Azure fornece um conjunto completo de capacidades de segurança. Pode:

* Proteger a conta de armazenamento utilizando o controlo de acesso baseado em funções (RBAC) e o Azure Active Directory.
* Proteger dados em trânsito entre uma aplicação e o Azure utilizando a encriptação do lado do cliente, HTTPS ou SMB 3.0.
* Definir os dados sejam encriptados automaticamente quando está a ser escrito ao Storage do Azure através de encriptação do serviço de armazenamento.
* Conjunto de dados e SO discos utilizados por máquinas virtuais (VMs) sejam encriptados utilizando a Azure Disk Encryption.
* Conceda acesso delegado aos objetos de dados no armazenamento do Azure através da utilização de assinaturas de acesso partilhado (SASs).
* Utilize a análise para controlar o método de autenticação que alguém está a utilizar quando acedem ao armazenamento.

Para ver mais detalhada segurança no armazenamento do Azure, consulte o [manual de segurança de armazenamento do Azure](../storage/common/storage-security-guide.md). Este guia fornece uma descrição profunda para as funcionalidades de segurança do Storage do Azure. Estas funcionalidades incluem chaves de conta de armazenamento, a encriptação de dados em trânsito e rest e análise de armazenamento.

Este artigo fornece uma descrição geral das funcionalidades de segurança do Azure que pode utilizar com o Storage do Azure. Ligações para artigos dê detalhes de cada funcionalidade, pelo que pode saber mais.

## <a name="role-based-access-control"></a>Controlo de Acesso Baseado em Funções
Pode ajudar a proteger a sua conta de armazenamento utilizando o controlo de acesso baseado em funções. Restringir o acesso com base no [precisa de saber](https://en.wikipedia.org/wiki/Need_to_know) e [menor privilégio](https://en.wikipedia.org/wiki/Principle_of_least_privilege) princípios de segurança é imperativo para as organizações que pretendem aplicar políticas de segurança para acesso a dados. Estes direitos de acesso são concedidos ao atribuir a função RBAC adequada aos grupos e aplicações num determinado âmbito. Pode utilizar [funções incorporadas do RBAC](../role-based-access-control/built-in-roles.md), tais como contribuinte de conta de armazenamento, para atribuir os privilégios aos utilizadores.

Saiba mais:

* [Controlo de acesso baseado em funções do Azure Active Directory](../role-based-access-control/role-assignments-portal.md)

## <a name="delegated-access-to-storage-objects"></a>Acesso delegado a objetos de armazenamento
Uma assinatura de acesso partilhado fornece acesso delegado a recursos na sua conta de armazenamento. O SAS significa que pode conceder que um cliente limitada permissões para objetos na sua conta de armazenamento durante um período especificado e com um conjunto especificado de permissões. Pode conceder estas permissões limitadas sem ter de partilhar as chaves de acesso da conta. 

O SAS é um URI que abrange nos respetivos parâmetros de consulta todas as informações necessárias para acesso autenticado a um recurso de armazenamento. Para aceder aos recursos de armazenamento com a SAS, o cliente só tem de fornecer a SAS para o método ou construtor adequado.

Saiba mais:

* [Compreender o modelo SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [Criar e utilizar um SAS com o Blob storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Encriptação em trânsito
A encriptação em trânsito é um mecanismo de proteção de dados quando transmitido através de redes. Com o Storage do Azure, pode proteger dados utilizando:

* [Encriptação de nível de transporte](../storage/common/storage-security-guide.md#encryption-in-transit), por exemplo, HTTPS, quando a transferência de dados ou a sair do armazenamento do Azure.
* [Ligar a encriptação](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), tais como a encriptação SMB 3.0, para o Azure partilhas de ficheiros.
* [Encriptação do lado do cliente](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), para encriptar os dados antes de é transferido para o armazenamento e para desencriptar os dados depois de serem transferidos fora do armazenamento.

Saiba mais sobre a encriptação do lado do cliente:

* [Encriptação do lado do cliente para o armazenamento do Microsoft Azure](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [Série os controlos da segurança de nuvem: encriptação de dados em trânsito](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Encriptação inativa
Para muitas organizações, [encriptação de dados Inativos](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) é um passo obrigatório na direção soberania dos dados, privacidade e conformidade de dados. Três funcionalidades do Azure fornecem encriptação de dados Inativos:

* [Encriptação do serviço de armazenamento](../storage/common/storage-security-guide.md#encryption-at-rest) permite-lhe pedido que o serviço de armazenamento encriptar dados automaticamente quando escrita ao Storage do Azure.
* [Encriptação do lado do cliente](../storage/common/storage-security-guide.md#client-side-encryption) também fornece a funcionalidade de encriptação de inativos.
* [Azure Disk Encryption](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) permite encriptar os discos de SO e discos de dados que utiliza a máquina virtual IaaS.

Saiba mais sobre a encriptação do serviço de armazenamento:

* [Encriptação do serviço de armazenamento do Azure](https://azure.microsoft.com/services/storage/) está disponível para [Blob storage do Azure](https://azure.microsoft.com/services/storage/blobs/). Para obter detalhes sobre outros tipos de armazenamento do Azure, consulte [ficheiros do Azure](https://azure.microsoft.com/services/storage/files/), [disco (armazenamento Premium)](https://azure.microsoft.com/services/storage/premium-storage/), [tabela armazenamento](https://azure.microsoft.com/services/storage/tables/), e [fila armazenamento](https://azure.microsoft.com/services/storage/queues/).
* [Encriptação do serviço de armazenamento do Azure para dados Inativos](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Disk Encryption
Encriptação de disco do Azure para máquinas virtuais ajuda-o a requisitos de conformidade e segurança organizacional de endereço. Os discos da VM (incluindo o arranque e discos de dados), encripta através da utilização de chaves e as políticas que controla na [Cofre de chaves do Azure](https://azure.microsoft.com/services/key-vault/).

Encriptação de disco para as VMs funciona para os sistemas operativos Linux e Windows. Também utiliza o Cofre de chaves para o ajudar a salvaguardar, gerir e utilização das suas chaves de encriptação de disco de auditoria. Todos os dados em discos de VM são encriptados em pausa utilizando a tecnologia de encriptação de norma da indústria nas suas contas do storage do Azure. A solução de encriptação de disco para o Windows é baseada no [a encriptação de unidade BitLocker do Microsoft](https://technet.microsoft.com/library/cc732774.aspx), e a solução de Linux baseia-se no [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Saiba mais:

* [Encriptação de disco do Azure para o Windows e as máquinas virtuais de Linux IaaS](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure Key Vault
Utiliza a Azure Disk Encryption [Cofre de chaves do Azure](https://azure.microsoft.com/services/key-vault/) para o ajudar a controlar e gerir chaves de encriptação de disco e segredos na sua subscrição do Cofre de chaves. Também garante que todos os dados em discos de máquinas virtuais sejam encriptados Inativos no armazenamento do Azure. Deve utilizar o Cofre de chaves para chaves e a utilização da política de auditoria.

Saiba mais:

* [O que é o Cofre de chaves do Azure?](../key-vault/key-vault-whatis.md)
* [Introdução ao Cofre de chaves do Azure](../key-vault/key-vault-get-started.md)

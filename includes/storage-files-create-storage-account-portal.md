---
title: storage-files-create-storage-account-portal
description: Como criar uma conta de armazenamento para ficheiros do Azure.
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 03/28/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 82e141eb4941dbd58be95e19dae186acd18caf94
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
---
Uma conta de armazenamento é um conjunto partilhado de armazenamento no qual pode implementar uma partilha de ficheiros do Azure ou outros recursos de armazenamento, como blobs ou filas. Uma conta de armazenamento pode conter um número ilimitado de partilhas e uma partilha pode armazenar um número ilimitado de ficheiros, até aos limites de capacidade da conta de armazenamento.

Para criar uma conta de armazenamento:

1. No menu à esquerda, clique no **+** para criar um recurso.
2. Escreva **conta de armazenamento** na caixa de pesquisa (1), selecione **Conta de armazenamento - blob, ficheiro, tabela, fila** (2) e, em seguida, clique em **Criar**.
    ![Uma captura de ecrã do aspeto que a entrada da conta de armazenamento deve ter na caixa de diálogo de pesquisa de recursos](../articles/storage/files/media/storage-how-to-use-files-portal/create-storage-account-1.png)

3. Em **Nome**, escreva *mystorageacct* seguido de alguns números aleatórios até obter a marca de verificação verde a indicar que é um nome exclusivo. Um nome de conta de armazenamento tem de estar todo em minúsculas e ser globalmente exclusivo. Anote o nome da sua conta de armazenamento porque irá utilizá-lo mais tarde. 
4. Em **Modelo de implementação**, deixe o valor predefinido de **Resource manager**. Para saber mais sobre as diferenças entre o Azure Resource Manager e a implementação clássica, veja [Compreender os modelos de implementação e o estado dos seus recursos](../articles/azure-resource-manager/resource-manager-deployment-model.md).
5. Em **Tipo de conta**, selecione **StorageV2**. Para saber mais sobre os diferentes tipos de contas de armazenamento, veja [Compreender as contas de armazenamento do Azure](../articles/storage/common/storage-account-options.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
6. Em **Desempenho**, mantenha o valor predefinido de **Armazenamento Standard**. Atualmente, os Ficheiros do Azure só suportam o armazenamento standard; mesmo se selecionar o armazenamento premium, a partilha de ficheiros é armazenada no armazenamento standard.
7. Em **Replicação**, selecione *Armazenamento Localmente Redundante (LRS)*. 
8. Em **Transferência segura obrigatória**, recomendamos que selecione sempre **Ativada**. Para saber mais sobre esta opção, veja [Compreender a encriptação em trânsito](../articles/storage/common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
9. Em **Subscrição**, selecione a subscrição na qual quer criar a conta de armazenamento. Se tiver apenas uma subscrição, deve ser a predefinição.
10. Em **Grupo de recursos**, selecione **Criar novo** e escreva *myResourceGroup* como o nome.
11. Em **Localização**, selecione **E.U.A. Leste**.
12. Em **Redes virtuais**, deixe a opção predefinida como *Desativado*. 
13. Selecione **Afixar ao dashboard** para que seja mais fácil encontrar a conta de armazenamento.
14. Quanto terminar, clique em **Criar** para iniciar a implementação.
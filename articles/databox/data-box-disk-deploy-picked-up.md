---
title: Enviar o Microsoft Azure Data Box Disk de volta | Microsoft Docs
description: Utilize este tutorial para aprender enviar o Azure Data Box Disk para a Microsoft
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/10/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 783c837bbb9ce22e7354252523e6725753776836
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011063"
---
# <a name="tutorial-return-azure-data-box-disk-and-verify-data-upload-to-azure"></a>Tutorial: devolver o Azure Data Box Disk e verificar os dados carregados para o Azure

Este é o último tutorial da série: Implementar o Azure Data Box Disk. Neste tutorial, vai aprender a:

> [!div class="checklist"]
> * Enviar o Data Box Disk para a Microsoft
> * Verificar o carregamento de dados para o Azure
> * Eliminação de dados do Data Box Disk

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que concluiu o [Tutorial: Copiar dados para o Azure Data Box Disk e verificar](data-box-disk-deploy-copy-data.md).

## <a name="ship-data-box-disk-back"></a>Enviar o Data Box Disk de volta

1. Após a validação de dados ser concluída, desligue os discos. Remova os cabos de ligação.
2. Embrulhe todos os discos e os cabos de ligação em plástico-bolha e coloque-os na caixa de envio.
3. Utilize a etiqueta de envio para devolução, na capa de plástico afixada na caixa. Se a etiqueta estiver danificada ou perder-se, transfira uma nova etiqueta de envio do portal do Azure e coloque-a no dispositivo. Aceda a **Descrição geral > Transferir etiqueta de envio**. 

    ![Transferir etiqueta de envio](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

    Esta ação transfere uma etiqueta de envio para devolução, conforme mostrado abaixo.

    ![Examinar a etiqueta de envio](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)

4. Feche a caixa de envio e certifique-se de que a etiqueta para envio de devolução se encontra visível.
5. Se estiver a devolver o dispositivo nos EUA, contacte a UPS para agendar uma recolha. Se estiver a devolver o dispositivo na Europa com a DHL, solicite uma recolha à DHL acedendo ao respetivo site e especificando o número de carta de porte aéreo. Aceda ao site da DHL Express do seu país e selecione **Book a Courier Collection (Agendar uma Recolha por Estafeta) > eReturn Shipment (Envio eReturn)**.

    ![Envio eReturn da DHL](media/data-box-disk-deploy-picked-up/dhl-ship-1.png)
    
    Especifique o número de carta de porte aéreo e clique em **Schedule Pickup (Agendar Recolha)** para marcar uma recolha.

      ![Agendar recolha](media/data-box-disk-deploy-picked-up/dhl-ship-2.png)

7. Após a sua transportadora recolher os discos, o estado de encomenda no portal será atualizado para **Picked up (Recolhido)**. É também mostrado um ID de controlo.

    ![Discos recolhidos](media/data-box-disk-deploy-picked-up/data-box-portal-pickedup.png)

## <a name="verify-data-upload-to-azure"></a>Verificar o carregamento de dados para o Azure

Quando a Microsoft receber e analisar o disco, o estado da tarefa será alterado para **Received (Recebido)**. 

![Discos recebidos](media/data-box-disk-deploy-picked-up/data-box-portal-received.png)

Os dados são automaticamente copiados após os discos serem ligados a um servidor no datacenter do Azure. Consoante o tamanho dos dados, a operação de cópia poderá demorar algumas horas a dias para concluir. Pode monitorizar o progresso da tarefa de cópia no portal.

Quando a cópia for concluída, o estado da encomenda será atualizado para **Completed (Concluído)**.

![Cópia de dados concluída](media/data-box-disk-deploy-picked-up/data-box-portal-completed.png)

Certifique-se de que os dados estão na(s) conta(s) de armazenamento antes de eliminá-los da origem. Para garantir que os dados foram carregados para o Azure, execute os seguintes passos:

1. Aceda à conta de armazenamento associada à sua encomenda de disco.
2. Aceda a **Serviço Blob > Procurar blobs**. É apresentada a lista de contentores. De forma correspondente à subpasta que criou nas pastas *BlockBlob* e *PageBlob*, os contentores com o mesmo nome são criados na sua conta de armazenamento.
    Se os nomes de pastas não estiverem em conformidade com as convenções de nomenclatura do Azure, o carregamento de dados para o Azure irá falhar.

4. Para se certificar de que todo o conjunto de dados foi carregado, utilize o Explorador de Armazenamento do Microsoft Azure. Anexe a conta de armazenamento correspondente à encomenda de aluguer de discos e, em seguida, observe a lista de contentores de blobs. Selecione um contentor, clique em **…Mais** e, em seguida, clique em **Estatísticas de pasta**. No painel **Atividades**, a estatística dessa pasta, incluindo o número de blobs e o tamanho total de blobs, é apresentado. O tamanho total de blobs em bytes deve corresponder ao tamanho do conjunto de dados.

    ![Estatísticas de pastas no Explorador de Armazenamento do Azure](media/data-box-disk-deploy-picked-up/folder-statistics-storage-explorer.png)

## <a name="erasure-of-data-from-data-box-disk"></a>Eliminação de dados do Data Box Disk

Após a cópia ser concluída e se certificar de que os dados estão na conta de armazenamento do Azure, os discos são apagados de forma segura, segundo a norma NIST. 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a conhecer tópicos do Azure Data Box Disk, como:

> [!div class="checklist"]
> * Enviar o Data Box Disk para a Microsoft
> * Verificar o carregamento de dados para o Azure
> * Eliminação de dados do Data Box Disk


Avance para as próximas instruções para saber como gerir o Data Box Disk através do portal do Azure.

> [!div class="nextstepaction"]
> [Utilizar o portal do Azure para administrar o Azure Data Box Disk](./data-box-portal-ui-admin.md)



---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/07/2019
ms.author: alkohli
ms.openlocfilehash: c7e5231650ec1afb97a72ec0cf26cb8f80088b63
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54440459"
---
O passo final é preparar o dispositivo para envio. Neste passo, todas as partilhas do dispositivo são colocadas offline. Não não possível aceder as partilhas depois de iniciar este processo.

> [!IMPORTANT]
> Preparação para envio é necessária, como sinalizadores de dados que não estão em conformidade com as convenções de nomenclatura do Azure. A ignorar este passo foi resultem em dados de potenciais falhas de carregamento devido a não confirmar dados.

1. Aceda a **Preparar para enviar** e clique em **Iniciar preparação**. Por predefinição, as somas de verificação são calculadas inline durante a preparação para envio. O cálculo de soma de verificação pode demorar horas a dias dependendo do tamanho dos seus dados. 
   
    ![Preparar para enviar 1](media/data-box-prepare-to-ship/prepare-to-ship1.png)

    Se por algum motivo quiser interromper a preparação do dispositivo, clique em **parar preparação**. Pode retomar a preparação para envio posterior.
        
    ![Preparação para envio 2](media/data-box-prepare-to-ship/prepare-to-ship2.png)
    
2. A preparação para envio é iniciado e as partilhas de dispositivo offline. Verá um lembrete para transferir a etiqueta quando o dispositivo está pronto.

    ![Baixe o lembrete de etiqueta de envio](media/data-box-prepare-to-ship/download-shipping-label-reminder.png)

3. Atualizações do Estado do dispositivo *pronto para enviar* e o dispositivo está bloqueado depois de concluída a preparação do dispositivo.
        
    ![Preparação para envio 3](media/data-box-prepare-to-ship/prepare-to-ship3.png)

    Se desejar copiar mais dados para o dispositivo, pode desbloquear o dispositivo, copiar mais dados e execução de preparação para envio de novamente.

    Se houver erros neste passo, terá de transferir o registo de erros e resolva os erros. Depois dos erros são resolvidos, execute **preparação para envio**.

4. Após a preparação para envio concluída com sucesso (sem nenhum erro), baixe a lista de ficheiros (também conhecido como manifesto) copiada nesse processo. Mais tarde, pode utilizar esta lista para verificar os ficheiros carregados para o Azure.
        
    ![Preparar para enviar 1](media/data-box-prepare-to-ship/prepare-to-ship4.png)

5. Encerre o dispositivo. Aceda à página **Encerrar ou reiniciar** e clique em **Encerrar**. Quando lhe for pedida a confirmação, clique em **OK** para continuar.

6. Remova os cabos. O passo seguinte é enviar o dispositivo para a Microsoft.

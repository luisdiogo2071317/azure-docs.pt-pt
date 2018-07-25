---
title: Tutorial para encomendar o Microsoft Azure Data Box Disk | Microsoft Docs
description: Utilize este tutorial para saber como inscrever-se e encomendar um disco do Azure Data Box Disk para importar dados para o Azure.
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
ms.date: 07/17/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: f46a3ecb3c4ef6ace31b7010ba9068ab04a43315
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126546"
---
# <a name="tutorial-order-an-azure-data-box-disk-preview"></a>Tutorial: encomendar um disco do Azure Data Box Disk (Pré-visualização)

O Azure Data Box Disk é uma solução cloud que permite importar os dados no local para o Azure, duma forma rápida, fácil e fiável. Transfira os seus dados para discos de estado sólido (SSDs) fornecidos pela Microsoft e reenvie os discos. Estes dados são então carregados para o Azure. 

Este tutorial descreve como pode encomendar um disco do Azure Data Box Disk. Neste tutorial, ficará a saber mais sobre:

> [!div class="checklist"]
> * Inscrever-se no Data Box Disk
> * Encomendar um disco do Data Box Disk
> * Controlar a encomenda
> * Cancelar a encomenda

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

> [!IMPORTANT]
> - O Data Box Disk está em pré-visualização. Reveja os [Termos de serviço do Azure para pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de encomendar e implementar esta solução. 
> - Durante a pré-visualização, o Data Box Disk pode ser enviado aos clientes nos EUA e na União Europeia. Para obter mais informações, aceda a [Disponibilidade de região](data-box-disk-overview.md#region-availability).

## <a name="sign-up"></a>Inscrever-se 

O Data Box Disk está em pré-visualização e precisa de inscrever-se no serviço. Execute os seguintes passos para inscrever-se no serviço Data Box:

1. Inicie sessão no Portal do Azure em: [https://aka.ms/azuredataboxfromdiskdocs](https://aka.ms/azuredataboxfromdiskdocs).
2. Escolha a subscrição que pretende ativar para a pré-visualização. Responda às perguntas sobre o tamanho dos dados, o país de residência dos dados, o período de tempo e a frequência de transferência de dados. Clique em **Inscrever-me!**.
3. Assim que estiver inscrito e ativado para pré-visualização, pode encomendar um disco do Data Box Disk.

## <a name="order-data-box-disk"></a>Encomendar o Data Box Disk

Execute os seguintes passos no [portal do Azure](https://aka.ms/azuredataboxfromdiskdocs) para encomendar o Data Box Disk.

1. No canto superior esquerdo do portal, clique em **+ Criar um recurso**e procure *Azure Data Box*. Clique em **Azure Data Box**.
    
   ![Pesquisar o Azure Data Box 1](media/data-box-disk-deploy-ordered/search-data-box11.png)

2. Clique em **Criar**.

3. Verifique se o serviço Data Box está disponível na sua região. Introduza ou selecione as seguintes informações e clique em **Aplicar**.

    ![Selecionar a opção Data Box Disk](media/data-box-disk-deploy-ordered/select-data-box-sku-1.png)

    |Definição|Valor|
    |---|---|
    |Subscrição|Selecione a subscrição para o qual o serviço Data Box está ativado.<br> A subscrição está ligada à sua conta de faturação. |
    |Tipo de transferência| Importar para o Azure|
    |País de origem | Selecione o país onde os dados residem atualmente.|
    |Região do Azure de destino|Selecione a região do Azure para onde pretende transferir os dados.|

  
5.  Selecione **Data Box Disk**. A capacidade máxima da solução para uma encomenda única de 5 discos é 35 TB. Pode criar várias encomendas para tamanhos de dados maiores. 

     ![Selecionar a opção Data Box Disk](media/data-box-disk-deploy-ordered/select-data-box-sku-zoom.png)

6.  Em **Encomenda**, especifique os **Detalhes da encomenda**. Introduza ou selecione as seguintes informações.

    |Definição|Valor|
    |---|---|
    |Nome|Forneça um nome amigável para controlar a encomenda.<br> O nome pode ter entre 3 e 24 carateres que podem ser letras, números e hífenes. <br> O nome tem de começar e terminar com uma letra ou um número. |
    |Grupo de recursos| Utilize um grupo de recursos existente ou crie um novo. <br> Um grupo de recursos é um contentor lógico para os recursos que podem ser geridos ou implementados em conjunto. |
    |Região do Azure de destino| Selecione uma região para a sua conta de armazenamento.<br> Atualmente, são suportadas contas de armazenamento em todas as regiões dos EUA e da Europa Ocidental e Europa do Norte. |
    |Contas de armazenamento|Com base na região do Azure especificada, selecione na lista filtrada uma conta de armazenamento existente. <br>Também pode criar uma nova conta v1 para Fins gerais ou uma conta v2 para Fins Gerais. |
    |Tamanho de dados estimado em TB| Introduza uma estimativa em TB. <br>Com base no tamanho dos dados, a Microsoft envia um número adequado de SSDs de 8 TB (capacidade utilizável de 7 TB). <br>A capacidade máxima utilizável de 5 discos é 35 TB. |

13. Clique em **Seguinte**. 

    ![Fornecer os detalhes da encomenda](media/data-box-disk-deploy-ordered/data-box-order-details.png)

14. No separador **Endereço para envio**, forneça o seu nome próprio e apelido, o nome e o endereço postal da empresa e um número de telefone válido. Clique em **Validar endereço**. O serviço valida o endereço de envio relativamente à disponibilidade do serviço. Se o serviço estiver disponível para o endereço de envio especificado, receberá uma notificação para o efeito. 

    ![Fornecer o endereço de envio](media/data-box-disk-deploy-ordered/data-box-shipping-address.png)
15. Em **Detalhes de notificação**, especifique os endereços de e-mail. O serviço envia notificações por e-mail relativamente a todas as atualizações do estado da encomenda para os endereços de e-mail especificados. 

    Recomendamos que utilize um e-mail de grupo para continuar a receber notificações se um administrador sair do grupo.

16. Reveja o **Resumo** das informações relacionadas com os termos de encomenda, contacto, notificação e privacidade. Selecione a caixa correspondente ao contrato de termos de privacidade.

17. Clique em **Encomendar**. A encomenda demora alguns minutos a ser criada.

 
## <a name="track-the-order"></a>Controlar a encomenda

Depois que fazer a encomenda, pode controlar o estado da encomenda a partir do portal de pré-visualização do Azure. Vá para a sua encomenda e, em seguida, aceda a **Descrição Geral** para ver o estado. O portal mostra a tarefa no estado **Encomendado**. 

![Estado do disco do Data Box Disk encomendado](media/data-box-disk-deploy-ordered/data-box-portal-ordered.png) 

Se os discos não estiverem disponíveis, receberá uma notificação. Se os discos estiverem disponíveis, a Microsoft identifica os discos para envio e prepara o pacote de discos. Durante a preparação dos discos, as ações seguintes ocorrem:

- Os discos são encriptados com a encriptação AES-128 BitLocker.  
- Os discos são bloqueados para impedir qualquer acesso não autorizado aos mesmos.
- A chave de acesso que desbloqueia os discos é gerada durante este processo.

Quando a preparação dos discos estiver concluída, o portal mostra a encomenda no estado **Processado**.

A Microsoft prepara e expede então os seus discos através de uma operadora regional. Receberá um número de controlo assim que os discos forem enviados. O portal mostra a encomenda no estado **Expedido**.



## <a name="cancel-the-order"></a>Cancelar a encomenda

Para cancelar esta encomenda, no portal de pré-visualização do Azure, aceda a **Descrição Geral** e clique em **Cancelar** na barra de comandos. 

Só pode cancelar quando os discos tiverem sido encomendados e a encomenda estiver a ser processada para envio. Assim que a encomenda for processada, já não pode cancelar a encomenda. 

![Cancelar encomenda](media/data-box-disk-deploy-ordered/cancel-order1.png)

Para eliminar uma encomenda cancelada, aceda a **Descrição Geral** e clique em **Eliminar** na barra de comandos. 


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a conhecer tópicos do Azure Data Box, como:

> [!div class="checklist"]
> * Inscrever-se no Data Box Disk
> * Encomendar o Data Box Disk
> * Controlar a encomenda
> * Cancelar a encomenda

Avance para o próximo tutorial para saber como configurar o Data Box Disk.

> [!div class="nextstepaction"]
> [Configurar o seu disco do Azure Data Box Disk](./data-box-disk-deploy-set-up.md)



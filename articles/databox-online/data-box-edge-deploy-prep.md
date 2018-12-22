---
title: Tutorial sobre a preparação do portal do Azure para implementar o Data Box Edge | Microsoft Docs
description: O primeiro tutorial sobre como implementar o Edge de caixa de dados do Azure envolve a preparação do portal do Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 25f68b011d1fcba450903e9a691b98dfe9e87281
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53726124"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge"></a>Tutorial: Preparar a implementação de borda de caixa de dados do Azure  


Este é o primeiro tutorial da série de tutoriais de implementação que são necessários para implementar completamente o Edge de caixa de dados do Azure. Este tutorial descreve como preparar o portal do Azure para implementar um recurso de borda de caixa de dados. 

Necessita de privilégios de administrador para concluir o processo de instalação e configuração. A preparação do portal demora menos de 10 minutos.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um novo recurso
> * Obter a chave de ativação

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


> [!IMPORTANT]
> O Data Box Edge está em pré-visualização. Antes de pedir e implementar esta solução, reveja os [do Azure termos de serviço para pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).  

### <a name="get-started"></a>Introdução

Para implementar o Edge de caixa de dados, consulte os seguintes tutoriais na seqüência prescrita.

| **#** | **Neste passo** | **Utilizar estes documentos** |
| --- | --- | --- | 
| 1. |**[Preparar o portal do Azure para o Data Box Edge](data-box-edge-deploy-prep.md)** |Criar e configurar o seu recurso do Data Box Edge antes de instalar um dispositivo físico Data Box Edge. |
| 2. |**[Instalar o Data Box Edge](data-box-edge-deploy-install.md)**|Descompactar, montar e instalar o dispositivo físico do Data Box Edge.  |
| 3. |**[Ligar, configurar e ativar o limite da caixa de dados](data-box-edge-deploy-connect-setup-activate.md)** |Ligue IU Web local, conclua a configuração do dispositivo e ative o dispositivo. O dispositivo está pronto para configurar partilhas SMB ou NFS.  |
| 4. |**[Transferir dados com o Data Box Edge](data-box-edge-deploy-add-shares.md)** |Adicione partilhas e ligue-se a partilhas através de SMB ou NFS. |
| 5. |**[Transformar dados com o Data Box Edge](data-box-edge-deploy-configure-compute.md)** |Configure os módulos do Edge no dispositivo para transformar os dados durante a deslocação destes para o Azure. |

Agora, pode começar a configurar o portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Seguem-se os pré-requisitos de configuração para o seu recurso de borda de caixa de dados, o seu dispositivo Edge de caixa de dados e a rede de centro de dados.

### <a name="for-the-data-box-edge-resource"></a>Para o recurso do Data Box Edge

Antes de começar, certifique-se de que:

* Subscrição do Microsoft Azure está ativada para um recurso de borda de caixa de dados.
* Tem a conta do Storage do Microsoft Azure com credenciais de acesso.

### <a name="for-the-data-box-edge-device"></a>Para o dispositivo do Data Box Edge

Antes de implementar um dispositivo físico, certifique-se de que:
- Tem uma 1 ranhura de U disponível num bastidor padrão 19" no seu datacenter para rack montar o dispositivo. 
- Tem acesso para uma superfície plana, estável e nível de trabalho em que o dispositivo pode rest com segurança.
- O site onde pretende configurar o dispositivo tem alternada padrão de uma origem de independente ou uma unidade de distribuição de energia (PDU) de rack com uma alimentação ininterrupta (UPS).
- Tem acesso a um dispositivo físico.


### <a name="for-the-datacenter-network"></a>Para a rede do datacenter

Antes de começar, certifique-se de que:

* A rede no seu datacenter está configurada pelos requisitos de rede para o seu dispositivo do Edge de caixa de dados. Para obter mais informações, consulte [requisitos de sistema de borda de caixa de dados](data-box-gateway-system-requirements.md).

* Dados caixa Edge dedicou largura de banda de Internet de 20 Mbps (ou mais) disponível, o tempo todo. Esta largura de banda não deve ser partilhada com outras aplicações. Se estiver a utilizar a limitação de rede, em seguida, para limitação para trabalhar, recomendamos que utilização de largura de banda de Internet de 32 Mbps ou mais.

## <a name="create-a-new-resource"></a>Criar um novo recurso

Execute os passos seguintes para criar um novo recurso do Data Box Edge. 

Se tiver um recurso existente do Data Box Edge para gerir os dispositivos físicos, ignore este passo e vá para o passo [Obter a chave de ativação](#get-the-activation-key).

Para criar um recurso de borda de caixa de dados, siga os passos seguintes no portal do Azure.

1. Utilize as suas credenciais do Microsoft Azure para iniciar sessão no portal de pré-visualização do Azure neste URL: [ https://aka.ms/databox-edge ](https://aka.ms/databox-edge). 

2. Escolha a subscrição que pretende utilizar para a pré-visualização do Edge de caixa de dados. Selecione a região onde pretende implementar o recurso Data Box Edge. Na **dados de caixa de borda** opção, selecione **criar**.

    ![Pesquisar o serviço Data Box Edge](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Para o novo recurso, introduza ou selecione as seguintes informações.
    
    |Definição  |Valor  |
    |---------|---------|
    |Nome do recurso   | Um nome amigável com a qual identificar o recurso.<br>O nome do recurso tem entre 2 e 50 carateres que contém letras, números e hífenes.<br> O nome começa e termina com uma letra ou um número.        |
    |Subscrição    |A subscrição está associada à sua conta de faturação. |
    |Grupo de recursos  |Selecione um grupo existente ou crie um novo grupo.<br>Saiba mais sobre [grupos de recursos do Azure](../azure-resource-manager/resource-group-overview.md).     |
    |Localização     |Para esta versão, estão disponíveis as regiões E.U.A. Leste, E.U.A. Oeste 2, Sudeste Asiático e Europa Ocidental. <br> Escolha uma localização mais próxima da região geográfica onde pretende implementar o dispositivo.|
    
    ![Criar um recurso de borda de caixa de dados](media/data-box-edge-deploy-prep/data-box-edge-resource.png)
    
4. Selecione **OK**.
 
A criação do recurso demora alguns minutos. Depois do recurso é criado com êxito, receberá uma notificação adequadamente.


## <a name="get-the-activation-key"></a>Obter a chave de ativação

Depois do recurso de borda de caixa de dados está em execução, terá de obter a chave de ativação. Esta chave serve para ativar e ligar o seu dispositivo Data Box Edge ao recurso. Pode obter esta chave agora enquanto está no portal do Azure.

1. Selecione o recurso que criou e, em seguida, selecione **descrição geral**.

2. Selecione **gerar chave** para criar uma chave de ativação. Selecione o ícone de cópia para copiar a chave e guarde-o para utilização posterior.

    ![Obter a chave de ativação](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - A chave de ativação expira três dias após ser gerado. 
> - Se a chave tiver expirado, gere um nova chave. A chave mais antiga não é válida.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a conhecer tópicos do Data Box Edge, como:

> [!div class="checklist"]
> * Criar um novo recurso
> * Obtendo a chave de ativação

Avance para o próximo tutorial para saber como instalar o Edge de caixa de dados. 

> [!div class="nextstepaction"]
> [Instalar o Edge de caixa de dados](./data-box-edge-deploy-install.md)




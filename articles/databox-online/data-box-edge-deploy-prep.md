---
title: Tutorial sobre a preparação do portal do Azure para implementar o Data Box Edge | Microsoft Docs
description: O primeiro tutorial para implementar o Azure Data Box Edge envolve a preparação do portal do Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 35ac28d687c8bc6636a7d8e10f54ffb5b219a776
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167470"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge-preview"></a>Tutorial: Preparar a implementação do Azure Data Box Edge (Pré-visualização)


Este é o primeiro tutorial da série de tutoriais de implementação necessários para implementar completamente o Azure Data Box Edge. Este tutorial descreve como preparar o portal do Azure para implementar o recurso do Data Box Edge. 

Necessita de privilégios de administrador para concluir o processo de instalação e configuração. A preparação do portal demora menos de 10 minutos.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um novo recurso
> * Obter a chave de ativação

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


> [!IMPORTANT]
> O Data Box Edge está em pré-visualização. Reveja os [Termos de serviço do Azure para pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de encomendar e implementar esta solução. 

### <a name="get-started"></a>Introdução

Para implementar o Data Box Edge, consulte os tutoriais seguintes pela sequência indicada.

| **#** | **Neste passo** | **Utilizar estes documentos** |
| --- | --- | --- | 
| 1. |**[Preparar o portal do Azure para o Data Box Edge](data-box-edge-deploy-prep.md)** |Criar e configurar o seu recurso do Data Box Edge antes de instalar um dispositivo físico Data Box Edge. |
| 2. |**[Instalar o Data Box Edge](data-box-edge-deploy-install.md)**|Descompactar, montar e instalar o dispositivo físico do Data Box Edge.  |
| 3. |**[Ligar, configurar e ativar o Data Box Edge](data-box-edge-deploy-connect-setup-activate.md)** |Ligue IU Web local, conclua a configuração do dispositivo e ative o dispositivo. O dispositivo está pronto para configurar partilhas SMB ou NFS.  |
| 4. |**[Transferir dados com o Data Box Edge](data-box-edge-deploy-add-shares.md)** |Adicione partilhas e ligue-se a partilhas através de SMB ou NFS. |
| 5. |**[Transformar dados com o Data Box Edge](data-box-edge-deploy-configure-compute.md)** |Configure os módulos do Edge no dispositivo para transformar os dados durante a deslocação destes para o Azure. |

Agora, pode começar a configurar o portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Aqui, vai encontrar os pré-requisitos de configuração para o recurso do Data Box Edge, o dispositivo do Data Box Edge e a rede do datacenter.

### <a name="for-the-data-box-edge-resource"></a>Para o recurso do Data Box Edge

Antes de começar, certifique-se de que:

* A sua subscrição do Microsoft Azure deve estar ativada para o recurso do Data Box Edge.
* Tem a conta do Storage do Microsoft Azure com credenciais de acesso.

### <a name="for-the-data-box-edge-device"></a>Para o dispositivo do Data Box Edge

Antes de implementar um dispositivo físico, certifique-se de que:

- Tem uma 1 ranhura em U disponível num bastidor padrão de 19" no seu datacenter para montar o dispositivo num bastidor. 
- Certifique-se de que tem acesso a uma superfície de trabalho plana, estável e uniforme na qual o dispositivo pode estar apoiado de forma segura.
- Certifique-se de que o local onde pretende configurar o dispositivo dispõe de alimentação CA de uma origem independente ou de uma unidade de distribuição de energia (PDU) em rack com uma fonte de alimentação ininterrupta (UPS).
- Tem acesso a um dispositivo físico.


### <a name="for-the-datacenter-network"></a>Para a rede do datacenter

Antes de começar, certifique-se de que:

* A rede do seu datacenter está configurada de acordo com os requisitos de rede para o seu dispositivo do Data Box Edge. Para obter mais informações, consulte os [Requisitos de Sistema do Data Box Edge](data-box-gateway-system-requirements.md).

* O Data Box Edge tem uma largura de banda de Internet dedicada de 20 Mbps (ou superior) disponível em permanência. Esta largura de banda não deve ser partilhada com outras aplicações. Se utilizar limitação de rede, para que esta funcione, recomendamos que utilize uma largura de banda de Internet de 32 Mbps ou superior.

## <a name="create-a-new-resource"></a>Criar um novo recurso

Execute os passos seguintes para criar um novo recurso do Data Box Edge. 

Se tiver um recurso existente do Data Box Edge para gerir os dispositivos físicos, ignore este passo e vá para o passo [Obter a chave de ativação](#get-the-activation-key).

Execute os seguintes passos no portal do Azure para criar um recurso do Data Box.

1. Utilize as suas credenciais do Microsoft Azure para iniciar sessão no portal de pré-visualização do Azure neste URL: [https://aka.ms/databox-edge](https://aka.ms/databox-edge). 

2. Escolha a subscrição que pretende utilizar para a pré-visualização do Data Box Edge. Selecione a região onde pretende implementar o recurso Data Box Edge. Na opção **Data Box Edge**, clique em **Criar**.

    ![Pesquisar o serviço Data Box Edge](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Para o novo recurso, introduza ou selecione as seguintes informações.
    
    |Definição  |Valor  |
    |---------|---------|
    |Nome do recurso   | Um nome amigável para identificar o recurso.<br>O nome do recurso tem entre 2 e 50 carateres que contêm letras, números e hífenes.<br> O nome começa e termina com uma letra ou um número.        |
    |Subscrição    |A subscrição está associada à sua conta de faturação. |
    |Grupo de recursos  |Selecione um grupo existente ou crie um novo grupo.<br>Saiba mais sobre os [Grupos de Recursos do Azure](../azure-resource-manager/resource-group-overview.md).     |
    |Localização     |Para esta versão, estão disponíveis as regiões E.U.A. Leste, E.U.A. Oeste 2, Sudeste Asiático e Europa Ocidental. <br> Escolha uma localização mais próxima da região geográfica onde pretende implementar o dispositivo.|
    
    ![Criar o recurso do Data Box Edge](media/data-box-edge-deploy-prep/data-box-edge-resource.png)
    
4. Clique em **OK**.
 
A criação do recurso demora alguns minutos. Depois de o recurso ser criado com êxito, será notificado adequadamente.


## <a name="get-the-activation-key"></a>Obter a chave de ativação

Depois de o recurso do Data Box Edge estar operacional, terá de obter a chave de ativação. Esta chave serve para ativar e ligar o seu dispositivo Data Box Edge ao recurso. Pode obter esta chave agora enquanto está no portal do Azure.

1. Clique no recurso que criou e, em seguida, clique em **Descrição geral**.

2. Clique em **Gerar chave** para criar uma chave de ativação. Clique no ícone de cópia para copiar a chave e guarde-a para utilização posterior.

    ![Obter a chave de ativação](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - A chave de ativação expira 3 dias após ser gerada. 
> - Se a chave tiver expirado, gere um nova chave. A chave mais antiga não é válida.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a conhecer tópicos do Data Box Edge, como:

> [!div class="checklist"]
> * Criar um novo recurso
> * Obter a chave de ativação

Avance para o tutorial seguinte para saber como instalar o Data Box Edge. 

> [!div class="nextstepaction"]
> [Instalar um Data Box Edge](./data-box-edge-deploy-install.md)




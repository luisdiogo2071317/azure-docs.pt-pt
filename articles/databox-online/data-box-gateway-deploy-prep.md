---
title: Tutorial sobre a preparação do portal do Azure para implementar o Data Box Gateway | Microsoft Docs
description: O primeiro tutorial para implementar o Azure Data Box Gateway envolve a preparação do portal do Azure.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: 4c21245e05625f65b6b5dcf5f7081f5f5d1abb52
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030610"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-gateway-preview"></a>Tutorial: Preparar a implementação do Azure Data Box Gateway (Pré-visualização)


Este é o primeiro tutorial da série de tutoriais de implementação necessários para implementar completamente o Azure Data Box Gateway. Este tutorial descreve como preparar o portal do Azure para implementar o recurso do Data Box Gateway. 

Necessita de privilégios de administrador para concluir o processo de instalação e configuração. A preparação do portal demora menos de 10 minutos.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um novo recurso
> * Transferir a imagem do dispositivo virtual
> * Obter a chave de ativação

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


> [!IMPORTANT]
> - O Data Box Gateway está em pré-visualização. Reveja os [Termos de serviço do Azure para pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de encomendar e implementar esta solução. 

### <a name="get-started"></a>Introdução

Para implementar o Data Box Gateway, consulte os seguintes tutoriais na sequência indicada.

| **#** | **Neste passo** | **Utilizar estes documentos** |
| --- | --- | --- | 
| 1. |**[Preparar o portal do Azure para o Data Box Gateway](data-box-gateway-deploy-prep.md)** |Crie e configure o seu recurso do Data Box Gateway antes de aprovisionar um dispositivo virtual Data Box Gateway. |
| 2. |**[Aprovisionar o Data Box Gateway em Hyper-V](data-box-gateway-deploy-provision-hyperv.md)** <br><br><br>**[Aprovisionar o Data Box Gateway em VMware](data-box-gateway-deploy-provision-vmware.md)**|Para Hyper-V, aprovisione e ligue a um dispositivo virtual Data Box Gateway num sistema anfitrião com Hyper-V no Windows Server 2016 ou Windows Server 2012 R2. <br><br><br> Para VMware, aprovisione e ligue a um dispositivo virtual Data Box Gateway num sistema anfitrião com VMware ESXi 6.0 ou 6.5.<br></br> |
| 3. |**[Ligar, configurar, ativar o Data Box Gateway](data-box-gateway-deploy-connect-setup-activate.md)** |Ligue IU Web local, conclua a configuração do dispositivo e ative o dispositivo. Em seguida, pode aprovisionar partilhas SMB.  |
| 4. |**[Transferir dados com o Data Box Gateway](data-box-gateway-deploy-add-shares.md)** |Adicione partilhas, ligue-se a partilhas através de SMB ou NFS. |

Agora, pode começar a configurar o portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Aqui, vai encontrar os pré-requisitos de configuração para o recurso do Data Box Gateway, o dispositivo Data Box Gateway e a rede do datacenter.

### <a name="for-the-data-box-gateway-resource"></a>Para o recurso do Data Box Gateway

Antes de começar, certifique-se de que:

* A sua subscrição do Microsoft Azure está ativada para o recurso do Data Box Gateway.
* Tem a conta do Storage do Microsoft Azure com credenciais de acesso.

### <a name="for-the-data-box-gateway-device"></a>Para o dispositivo Data Box Gateway

Antes de implementar um dispositivo virtual, certifique-se de que:

* Tem acesso a um sistema anfitrião com Hyper-V no Windows Server 2012 R2 ou posterior, ou com VMware (ESXi 6.0 ou 6.5), que possa ser utilizado para aprovisionar um dispositivo.
* O sistema anfitrião tem capacidade para dedicar os recursos seguintes para aprovisionar o seu dispositivo virtual Data Box:
  
  * Um mínimo de 4 núcleos.
  * Pelos menos 8 GB de RAM. 
  * Uma interface de rede.
  * Um disco de SO de 250 GB.
  * Um disco virtual de 2 TB para dados do sistema.

### <a name="for-the-datacenter-network"></a>Para a rede do datacenter

Antes de começar, certifique-se de que:

* A rede no seu datacenter está configurada de acordo com os requisitos de rede para o seu dispositivo Data Box Gateway. Para obter mais informações, veja os [Requisitos de Sistema do Data Box Gateway](data-box-gateway-system-requirements.md).

* O Data Box Gateway tem uma largura de banda de Internet de 20 Mbps (ou mais) disponível sempre. Esta largura de banda não deve ser partilhada com outras aplicações. Se utilizar limitação de rede, para que a limitação funcione, recomendamos que utilize uma largura de banda de Internet de 32 Mbps ou mais.

## <a name="create-a-new-resource"></a>Criar um novo recurso

Execute os seguintes passos para criar um novo recurso do Data Box Gateway. 

Se tiver um recurso existente do Data Box Gateway para gerir os dispositivos virtuais, ignore este passo e vá para o passo [Obter a chave de ativação](#get-the-activation-key).

Execute os seguintes passos no portal do Azure para criar um recurso do Data Box.
1. Utilize as suas credenciais do Microsoft Azure para iniciar sessão no portal de pré-visualização do Azure neste URL: [https://aka.ms/databox-edge](https://aka.ms/databox-edge). 

2. Escolha a subscrição que pretende utilizar para a pré-visualização do Data Box Edge. Selecione a região onde pretende implementar o recurso do Data Box Edge. Na opção **Data Box Gateway**, clique em **Criar**.

    ![Pesquisar o serviço Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-edge-sku.png)

3. Para o novo recurso, introduza ou selecione as seguintes informações.
    
    |Definição  |Valor  |
    |---------|---------|
    |Nome do recurso   | Um nome amigável para identificar o recurso.<br>O nome tem entre 2 e 50 carateres que contêm letras, números e hífenes.<br> O nome começa e termina com uma letra ou um número.        |
    |Subscrição    |A subscrição está associada à sua conta de faturação. |
    |Grupo de recursos  |Selecione um grupo existente ou crie um novo grupo.<br>Saiba mais sobre os [Grupos de Recursos do Azure](../azure-resource-manager/resource-group-overview.md).     |
    |Localização     |Para esta versão, estão disponíveis as regiões E.U.A. Leste, E.U.A. Oeste 2, Sudeste Asiático e Europa Ocidental. <br> Escolha uma localização mais próxima da região geográfica onde pretende implementar o dispositivo.|
    
    ![Criar o recurso do Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-resource.png)
    
4. Clique em **OK**.
 
A criação do recurso demora alguns minutos. Depois de o recurso ser criado com êxito, será notificado adequadamente.


## <a name="download-the-virtual-device-image"></a>Transferir a imagem do dispositivo virtual

Depois de criar o recurso do Data Box Gateway, transfira a imagem do dispositivo virtual adequado para aprovisionar um dispositivo virtual no seu sistema anfitrião. As imagens de dispositivo virtual são específicas do sistema operativo e podem ser transferidas a partir do painel **Início Rápido** do seu recurso no portal do Azure.

> [!IMPORTANT]
> O software em execução no Data Box Gateway só pode ser utilizado com o recurso do Data Box Gateway.


Execute os seguintes passos no [portal do Azure](https://portal.azure.com/).

1. Clique no recurso que criou e, em seguida, clique em **Descrição geral**. Se tiver um recurso existente do Azure Data Box Gateway, clique no recurso e aceda a **Descrição geral**.

    ![Novo recurso do Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

4. No painel da direita do início rápido, clique na hiperligação correspondente para a imagem que pretende transferir. Os ficheiros de imagem têm, aproximadamente, 4,8 GB.
   
   * [VHDX para Hyper-V no Windows Server 2012 R2 e versões posteriores](https://aka.ms/dbe-vhdx-2012).
   * [VMDK para VMWare ESXi 6.0 ou 6.5](https://aka.ms/dbe-vmdk).

5. Transfira e deszipe o ficheiro numa unidade local, tomando nota da localização onde o ficheiro deszipado se encontra.


## <a name="get-the-activation-key"></a>Obter a chave de ativação

Depois de o recurso do Data Box Gateway estar operacional, terá de obter a chave de ativação. Esta chave serve para ativar e ligar o seu dispositivo Data Box Gateway ao recurso.

A chave de ativação serve para registar todos os dispositivos Data Box Gateway que precisa de ativar com o seu recurso do Data Box Gateway. Pode obter esta chave agora enquanto está no portal do Azure.

1. Clique no recurso que criou e, em seguida, clique em **Descrição geral**.

    ![Novo recurso do Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

2. Clique em **Gerar chave** para criar uma chave de ativação. Clique no ícone de cópia para copiar a chave e guarde-a para utilização posterior.

    ![Obter a chave de ativação](media/data-box-gateway-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - A chave de ativação expira 3 dias após ser gerada. 
> - Se a chave tiver expirado, gere um nova chave. A chave mais antiga não é válida.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a conhecer tópicos do Data Box Gateway, como:

> [!div class="checklist"]
> * Criar um novo recurso
> * Transferir a imagem do dispositivo virtual
> * Obter a chave de ativação

Avance para o próximo tutorial para saber como aprovisionar uma máquina virtual para o Data Box Gateway. Consoante o sistema operativo anfitrião, veja as instruções detalhadas em:

> [!div class="nextstepaction"]
> [Aprovisionar um Data Box Gateway em Hyper-V](./data-box-gateway-deploy-provision-hyperv.md)

OU

> [!div class="nextstepaction"]
> [Aprovisionar um Data Box Gateway em VMware](./data-box-gateway-deploy-provision-vmware.md)



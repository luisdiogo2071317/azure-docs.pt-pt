---
title: Transformar dados com o Azure Data Box Edge | Microsoft Docs
description: Saiba como configurar a função de computação no Data Box Edge e utilizá-la para transformar dados antes de os enviar para o Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 11/27/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: 77a2b8d2b5d3ac42dcbbe2db2b05d38657290073
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52443794"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-preview"></a>Tutorial: Transformar dados com o Azure Data Box Edge (Pré-visualização)

Este tutorial descreve como configurar a função de computação no Data Box Edge. Quando a função de computação estiver configurada, o Data Box Edge pode transformar dados antes de os enviar para o Azure.

Este procedimento pode demorar cerca de 30 a 45 minutos a concluir.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um recurso do Hub IoT
> * Configurar a função de computação
> * Adicionar um módulo de computação
> * Verificar a transformação e a transferência dos dados

> [!IMPORTANT]
> O Data Box Edge está em pré-visualização. Reveja os [Termos de serviço do Azure para pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de encomendar e implementar esta solução.
 
## <a name="prerequisites"></a>Pré-requisitos

Antes de configurar a computação no Data Box Edge, certifique-se de que:

* O seu dispositivo do Data Box Edge está ativado, conforme detalhado em [Connect and activate your Azure Data Box Edge](data-box-edge-deploy-connect-setup-activate.md) (Ligar e ativar o Azure Data Box Edge).


## <a name="create-an-iot-hub-resource"></a>Criar um recurso do Hub IoT

Antes de configurar a função de computação no Data Box Edge, tem de criar um recurso do Hub IoT.

Para obter instruções detalhadas, aceda a [Create an IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#create-an-iot-hub) (Criar um Hub IoT). Utilize a mesma subscrição e o mesmo grupo de recursos que utilizou para o recurso do Data Box Edge.

![Criar o recurso do Hub IoT](./media/data-box-edge-deploy-configure-compute/create-iothub-resource-1.png)

Se a função de computação Edge não estiver configurada, tenha em conta que:

- O recurso do Hub IoT não terá nenhum dispositivo IoT nem dispositivos IoT Edge.
- Não poderá criar partilhas edge locais. Quando adiciona uma partilha, a opção para criar uma partilha local para a computação edge não estará ativada.


## <a name="set-up-compute-role"></a>Configurar a função de computação

Quando a função de computação edge estiver configurada no dispositivo Edge, cria dois dispositivos – um dispositivo IoT e um dispositivo IoT Edge. Ambos os dispositivos podem ser vistos no recurso do Hub IoT.

Para configurar a função de computação no dispositivo, siga os passos abaixo.

1. Aceda ao recurso do Data Box Edge e, em **Overview** (Descrição Geral), clique em **Set up compute role** (Configurar função de computação). 

    ![Configurar a função de computação](./media/data-box-edge-deploy-configure-compute/setup-compute-1.png)
   
    Também pode aceder a **Modules** (Módulos) e clicar em **Configure compute** (Configurar computação).

    ![Configurar a função de computação](./media/data-box-edge-deploy-configure-compute/setup-compute-2.png)
 
2. Na lista pendente, selecione o **recurso do Hub IoT** que criou no passo anterior. Neste momento, apenas a plataforma Linux está disponível para o seu dispositivo IoT Edge. Clique em **Criar**.

    ![Configurar a função de computação](./media/data-box-edge-deploy-configure-compute/setup-compute-3.png)
 
3. A criação da função de computação demora alguns minutos. Devido a um erro nesta versão, mesmo após a criação da função, o ecrã não é atualizado. Aceda a **Modules** (Módulos) e poderá ver que a computação edge está configurada.  

    ![Configurar a função de computação](./media/data-box-edge-deploy-configure-compute/setup-compute-4.png)

4. Aceda novamente a **Overview** (Descrição Geral) e o ecrã estará atualizado e indicará que a função de computação está configurada.

    ![Configurar a função de computação](./media/data-box-edge-deploy-configure-compute/setup-compute-5.png)
 
5. Aceda ao Hub IoT que utilizou na criação da função de computação edge. Aceda a **IoT devices** (Dispositivos IoT). Pode ver que está agora ativado um dispositivo IoT. 

    ![Configurar a função de computação](./media/data-box-edge-deploy-configure-compute/setup-compute-6.png)

6. Aceda a **IoT Edge** e verá que também está ativado um dispositivo IoT Edge.

    ![Configurar a função de computação](./media/data-box-edge-deploy-configure-compute/setup-compute-7.png)
 
7. Selecione e clique no dispositivo IoT Edge. Está a ser executado um agente Edge neste dispositivo IoT Edge. 

    ![Configurar a função de computação](./media/data-box-edge-deploy-configure-compute/setup-compute-8.png) 

Contudo, não existem módulos personalizados neste dispositivo edge. Pode agora adicionar um módulo personalizado a este dispositivo. Para saber como criar um módulo personalizado, aceda a [Desenvolver um módulo do C# para o Data Box Edge](data-box-edge-create-iot-edge-module.md).


## <a name="add-a-custom-module"></a>Adicionar um módulo personalizado

Nesta secção, irá adicionar um módulo personalizado para o dispositivo do IoT Edge que criou em [Desenvolver um módulo do C# para o Data Box Edge](data-box-edge-create-iot-edge-module.md). 

Este procedimento recorre a um exemplo no qual o módulo personalizado utilizado recebe ficheiros de uma partilha local no dispositivo edge e os move para uma partilha na cloud no dispositivo. Em seguida, a partilha na cloud envia os ficheiros para a conta de armazenamento associada à mesma. 

1. O primeiro passo é adicionar uma partilha local no dispositivo edge. No seu recurso do Data Box Edge, aceda a **Shares** (Partilhas). Clique em **+ Add share** (+ Adicionar partilha). Indique o nome da partilha e selecione o tipo. Para criar uma partilha local, assinale a opção **Configure as Edge local share** (Configurar como partilha edge local). Selecione **Use existing** (Utilizador existente) ou **Create new** (Criar novo). Clique em **Criar**.

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-1.png) 

    Se tiver criado uma partilha NFS local, utilize a opção de comando rsync seguinte para copiar ficheiros para a partilha:

    `rsync --inplace <source file path> < destination file path>`

     Para obter mais informações sobre o comando rsync, aceda à [documentação sobre o Rsync](https://www.computerhope.com/unix/rsync.htm). 

 
2. Quando a partilha local estiver criada e depois de receber uma notificação de criação bem-sucedida (a lista de partilhas poderá ser atualizada antes, mas tem de aguardar pela conclusão da criação da partilha), aceda à lista de partilhas. 

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-2.png) 
 
3. Selecione e clique na partilha local acabada de criar e veja as propriedades da mesma. Copie e cole o **ponto de montagem local para os módulos edge** que correspondem a esta partilha.

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-3.png) 
 
    Aceda a uma partilha na cloud existente que tenha sido criada no seu dispositivo Data Box Edge. Novamente, copie e guarde o ponto de montagem local dos módulos de computação edge desta partilha na cloud. Estes pontos de montagem locais serão utilizados na implementação do módulo.

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-4.png)  

4. Para adicionar um módulo personalizado ao dispositivo IoT Edge, aceda ao recurso do Hub IoT e a **IoT Edge device** (Dispositivo IoT Edge). Selecione e clique no dispositivo. Em **Device details** (Detalhes do dispositivo), na barra de comandos na parte superior, clique em **Set Modules** (Definir Módulos). 

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-5.png) 

5. Em **Add Modules** (Adicionar Módulos), siga os passos abaixo:

    1. Indique o **nome**, a **morada**, o **nome de utilizador** e a **palavra-passe** nas **Definições do registo de contentores** relativas ao módulo personalizado. O nome, a morada e as credenciais indicados são utilizados para obter os módulos que tenham um URL correspondente. Para implementar este módulo, em **Deployment modules** (Módulos de implementação), selecione **IoT Edge module** (Módulo do IoT Edge). Este módulo do IoT Edge é um contentor do Docker que pode implementar no dispositivo IoT Edge associado ao seu dispositivo Data Box Edge.

        ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-6.png) 
 
    2. Especifique as definições do módulo personalizado do IoT Edge. Indique o **nome** do seu módulo e o **URI da imagem** para a imagem de contentor correspondente. 
    
        ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-7.png) 

    3. Em **Container create options** (Opções de criação de contentor), forneça os pontos de montagem locais dos módulos do Edge copiados nos passos anteriores relativos às partilhas na cloud e local (é importante utilizar estes caminhos em vez de criar outros novos). Os pontos de montagem locais estão mapeados nos **InputFolderPath** e **OutputFolderPath** correspondentes que especificou no módulo quando [atualizou o módulo com código personalizado](data-box-edge-create-iot-edge-module.md#update-the-module-with-custom-code). 
    
        Pode copiar e colar o exemplo mostrado abaixo nas suas **Opções de criação do contentor**: 
        
        ```
        {
         "HostConfig": {
          "Binds": [
           "/home/hcsshares/mysmblocalshare:/home/LocalShare",
           "/home/hcsshares/mysmbshare1:/home/CloudShare"
           ]
         }
        }
        ```

        Indique também eventuais variáveis ambientais do seu módulo aqui.

        ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-8.png) 
 
    4. Se necessário, **configure as definições avançadas do runtime do edge** e clique em **Next** (Seguinte).

        ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-9.png) 
 
6.  Em **Specify routes** (Especificar rotas), defina as rotas entre os módulos. Neste caso, indique o nome da partilha local que vai enviar os dados para a partilha na cloud. Clique em **Seguinte**.

    Pode substituir a rota com a seguinte cadeia de rota:       `"route": "FROM /* WHERE topic = 'mysmblocalshare' INTO BrokeredEndpoint(\"/modules/filemovemodule/inputs/input1\")"`

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-10.png) 
 
7.  Em **Review deployment** (Rever implementação), reveja todas as definições e, se estiver satisfeito, **submeta** o módulo para implementação.

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-11.png) 
 
Desta forma, a implementação do módulo começa, conforme é mostrado em **IoT Edge Custom module** (Módulo personalizado do IoT Edge), em **Modules** (Módulos).

![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-12.png) 

### <a name="verify-data-transform-and-transfer"></a>Verificar a transformação e a transferência dos dados

O último passo é confirmar que o módulo está ligado e a funcionar conforme esperado. Siga os passos abaixo para verificar se o módulo está a funcionar.

1. O estado do runtime do módulo deverá estar em execução no seu dispositivo IoT Edge no recurso do Hub IoT.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-1.png) 
 
2. Selecione e clique no módulo e veja **Module Identity Twin** (Identidade do Módulo Gémeo). O estado do cliente do dispositivo e do módulo edge deverá aparecer como **Connected** (Ligado).

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-2.png) 
 
3.  Quando o módulo estiver em execução, também é apresentado na lista de módulos do Edge no seu recurso do Data Box Edge. O **estado do runtime** do módulo que adicionou é **em execução**.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-3.png) 
 
4.  Ligue às partilhas local e na cloud que criou através do Explorador de Ficheiros.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-4.png) 
 
5.  Adicione os dados à partilha local.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-5.png) 
 
6.  Os dados são movidos para a partilha na cloud.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-6.png)  

7.  Os dados são, depois, enviados da partilha na cloud para a conta de armazenamento. Aceda ao Explorador de Armazenamento para ver os dados.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-7.png) 
 
Isto conclui o processo de validação.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a conhecer tópicos do Data Box Edge, como:

> [!div class="checklist"]
> * Criar um recurso do Hub IoT
> * Configurar a função de computação
> * Adicionar um módulo de computação
> * Verificar a transformação e a transferência dos dados

Avance para o próximo tutorial para saber como administrar o Data Box Edge.

> [!div class="nextstepaction"]
> [Use local web UI to administer a Data Box Edge](https://aka.ms/dbg-docs) (Utilizar a IU da Web local para administrar o Data Box Edge)



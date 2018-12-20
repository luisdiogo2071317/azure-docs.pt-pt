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
ms.openlocfilehash: c0901f22e4941fdfaa21138153a06e97c2d6095f
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630381"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-preview"></a>Tutorial: Transformar dados com o Edge de caixa de dados do Azure (pré-visualização)

Este tutorial descreve como configurar uma função de computação no seu dispositivo do Edge de caixa de dados do Azure. Depois de configurar a função de computação, dados de caixa de borda pode transformar os dados antes de os enviar para o Azure.

Este procedimento pode demorar cerca de 30 a 45 minutos a concluir.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um recurso do IoT Hub do Azure
> * Configurar a função de computação
> * Adicionar um módulo de computação
> * Verificar a transformação e a transferência dos dados

> [!IMPORTANT]
> O Data Box Edge está em pré-visualização. Antes de pedir e implementar esta solução, reveja os [do Azure termos de serviço para pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
## <a name="prerequisites"></a>Pré-requisitos

Antes de configurar uma função de computação no seu dispositivo do Edge de caixa de dados, certifique-se de que:

* Ativou o seu dispositivo Edge de caixa de dados conforme descrito em [ligue-se de que configurar e ative o Edge de caixa de dados do Azure](data-box-edge-deploy-connect-setup-activate.md).


## <a name="create-an-iot-hub-resource"></a>Criar um recurso do Hub IoT

Antes de configurar uma função de computação no limite da caixa de dados, tem de criar um recurso do IoT Hub.

Para obter instruções detalhadas, aceda a [Create an IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#create-an-iot-hub) (Criar um Hub IoT). Utilize o mesmo grupo de recursos e subscrição que utilizou para o seu recurso de borda de caixa de dados.

![Criar um recurso do Hub IoT](./media/data-box-edge-deploy-configure-compute/create-iothub-resource-1.png)

Se ainda não foi configurada uma função de computação de borda, aplicam-se os seguintes avisos:

- O recurso do IoT Hub não tem quaisquer dispositivos IoT do Azure ou os dispositivos do Azure IoT Edge.
- Não poderá criar partilhas edge locais. Quando adiciona uma partilha, a opção para criar uma partilha local para a computação edge não estará ativada.


## <a name="set-up-compute-role"></a>Configurar a função de computação

Quando a função de computação de borda é configurada o dispositivo de limite, ele cria dois dispositivos: um dispositivo de IoT e um dispositivo IoT Edge. Ambos os dispositivos podem ser visualizados no recurso da IoT Hub.

Para configurar a função de computação no dispositivo, faça o seguinte:

1. Vá para o recurso de borda de caixa de dados, selecione **descrição geral**e, em seguida, selecione **configurar a função de computação**. 

    ![A ligação de descrição geral do painel esquerdo](./media/data-box-edge-deploy-configure-compute/setup-compute-1.png)
   
    Opcionalmente, pode aceder à **módulos** e selecione **configurar computação**.

    ![O "Módulos" e "Configurar computação" ligações](./media/data-box-edge-deploy-configure-compute/setup-compute-2.png)
 
1. Na lista pendente, selecione o **recursos do IoT Hub** que criou no passo anterior.  
    Neste momento, apenas a plataforma de Linux está disponível para seu dispositivo IoT Edge. 
    
1. Clique em **Criar**.

    ![O botão Criar](./media/data-box-edge-deploy-configure-compute/setup-compute-3.png)
 
    A criação da função de computação demora alguns minutos. Devido a um erro nesta versão, mesmo após a criação da função, o ecrã não é atualizado. Para confirmar que a função de computação de borda foi configurada, aceda a **módulos**.  

    ![A lista de dispositivos de "Computação configurar Edge"](./media/data-box-edge-deploy-configure-compute/setup-compute-4.png)

1. Aceda a **descrição geral** novamente.  
    A tela é atualizada para indicar que a função de computação é configurada.

    ![Configurar uma função de computação](./media/data-box-edge-deploy-configure-compute/setup-compute-5.png)
 
1. No IoT Hub que utilizou quando criou a função de computação de borda, aceda a **dispositivos IoT**.  
    Um dispositivo IoT está agora ativado. 

    ![A página de "Dispositivos da IoT"](./media/data-box-edge-deploy-configure-compute/setup-compute-6.png)

1. No painel esquerdo, selecione **IoT Edge**.  
    Um dispositivo IoT Edge também está ativado.

    ![Configurar uma função de computação](./media/data-box-edge-deploy-configure-compute/setup-compute-7.png)
 
1. Selecione e clique no dispositivo IoT Edge.  
    Está a ser executado um agente Edge neste dispositivo IoT Edge. 

    ![A página de detalhes do dispositivo](./media/data-box-edge-deploy-configure-compute/setup-compute-8.png) 

    Não há nenhum módulos personalizados neste dispositivo do Edge, mas agora, pode adicionar um módulo personalizado. Para saber como criar um módulo personalizado, aceda à [desenvolver um C# módulo para o seu dispositivo Edge de caixa de dados](data-box-edge-create-iot-edge-module.md).


## <a name="add-a-custom-module"></a>Adicionar um módulo personalizado

Nesta secção, adicionar um módulo personalizado para o dispositivo do IoT Edge que criou no [desenvolver um C# módulo para a extremidade da caixa de dados](data-box-edge-create-iot-edge-module.md). 

O procedimento seguinte utiliza um exemplo em que o módulo personalizado utiliza ficheiros de uma partilha local no dispositivo de limite e movê-los para uma partilha de cloud no dispositivo. A partilha de cloud, em seguida, envia os ficheiros para a conta de armazenamento do Azure que está associada a partilha de cloud. 

1. Adicione uma partilha de local no dispositivo Edge efetuando o seguinte procedimento:

    a. No seu recurso do Data Box Edge, aceda a **Shares** (Partilhas). 
    
    b. Selecione **Adicionar partilha**e, em seguida, forneça o nome de partilha e selecione o tipo de partilha. 
    
    c. Para criar uma partilha local, selecione o **configurar como partilha local do Edge** caixa de verificação. 
    
    d. Selecione **criar novo** ou **utilizar existente**e, em seguida, selecione **criar**.

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-1.png) 

    Se tiver criado uma partilha NFS local, utilize a seguinte opção de comando de sincronização remoto (rsync) para copiar ficheiros para a partilha:

    `rsync --inplace <source file path> < destination file path>`

    Para obter mais informações sobre o comando rsync, aceda a [Rsync documentação](https://www.computerhope.com/unix/rsync.htm). 

    Criar a partilha local, e receberá uma notificação de criação com êxito. A lista de partilha pode ser atualizada, mas tem de aguardar a criação de partilha para serem concluídas.
    
1. Vá para a lista de partilhas. 

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-2.png) 
 
1. Para ver as propriedades da partilha local criada recentemente, selecioná-lo. 

1. Na **módulos de computação de ponto de montagem Local para o Edge** caixa, copie o valor correspondente a esta partilha.  
    Irá utilizar este ponto de montagem local quando implantar o módulo.

    ![A caixa "módulos de computação de ponto de montagem Local para o Edge"](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-3.png) 
 
1. Num compartilhamento de nuvem existente que foi criado no seu dispositivo do Edge de caixa de dados, no **módulos de computação de ponto de montagem Local para o Edge** caixa, copie o ponto de montagem local para os módulos de computação do Edge para esta partilha de cloud.  
    Irá utilizar este ponto de montagem local quando implantar o módulo.

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-4.png)  

1. Para adicionar um módulo personalizado para o dispositivo do IoT Edge, aceda ao seu recurso do IoT Hub e, em seguida, aceda a **dispositivo IoT Edge**. 

1. Selecione o dispositivo e, em **detalhes do dispositivo**, selecione **definir módulos**. 

    ![A ligação do conjunto de módulos](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-5.png) 

1. Sob **adicionar módulos**, efetue o seguinte procedimento:

    a. Introduza o nome, endereço, nome de utilizador e palavra-passe para as definições de registo de contentor para o módulo personalizado.  
    O nome, endereço e as credenciais indicadas são utilizadas para obter os módulos com um URL correspondente. Para implementar este módulo, em **Deployment modules** (Módulos de implementação), selecione **IoT Edge module** (Módulo do IoT Edge). Este módulo do IoT Edge é um contentor do docker que pode implementar para o dispositivo do IoT Edge associada ao seu dispositivo Edge de caixa de dados.

    ![A página Definir módulos](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-6.png) 
 
    b. Especifique as definições para o módulo personalizado do IoT Edge ao introduzir o nome do seu módulo e o URI da imagem para a imagem de contentor correspondente. 
    
    ![A página de módulos do IoT Edge personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-7.png) 

    c. Na **opções de criar contentor** , introduza os pontos de montagem local para os módulos de extremidade que copiou nos passos anteriores para a cloud e a partilha local.
    > [!IMPORTANT]
    > Utilize os caminhos copiados; Não crie novos caminhos. Os pontos de montagem local são mapeados para os correspondentes **InputFolderPath** e o **OutputFolderPath** que especificou no módulo quando [atualizado o módulo com código personalizado](data-box-edge-create-iot-edge-module.md#update-the-module-with-custom-code). 
    
    Na **opções de criar contentor** caixa, pode colar o exemplo a seguir: 
    
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

    Também fornecem quaisquer variáveis de ambiente aqui para seu módulo.

    ![A caixa de opções de criar contentor](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-8.png) 
 
    d. Se necessário, configure as definições avançadas de runtime Edge e, em seguida, clique em **seguinte**.

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-9.png) 
 
1.  Sob **especificar rotas**, definir rotas entre módulos.  
    Neste exemplo, introduza o nome da partilha local que emite dados para a partilha de cloud.

    Pode substituir *rota* com a seguinte cadeia de rota:       `"route": "FROM /* WHERE topic = 'mysmblocalshare' INTO BrokeredEndpoint(\"/modules/filemovemodule/inputs/input1\")"`

    ![A secção de especificar rotas](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-10.png) 

1. Selecione **Seguinte**. 

1.  Sob **rever implantação**, reveja todas as definições e, em seguida, selecione **submeter** para submeter o módulo para a implementação.

    ![A página Definir módulos](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-11.png) 
 
    Esta ação inicia a implementação do módulo, conforme mostrado na imagem seguinte:

    ![Adicionar módulo personalizado](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-12.png) 

### <a name="verify-data-transform-and-transfer"></a>Verificar a transformação e a transferência dos dados

A etapa final é garantir que o módulo está ligado e em execução conforme esperado. O estado de tempo de execução do módulo deve estar em execução para o seu dispositivo IoT Edge no recurso da IoT Hub.

![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-1.png) 
 
Para verificar que o módulo está em execução, faça o seguinte:

1. Selecione o módulo e, em seguida, visualize o duplo do módulo de identidade.  
    Estado do cliente para o dispositivo de limite e o módulo deve ser exibido como *ligado*.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-2.png) 
 
    Depois do módulo está em execução, também será apresentado na lista de módulos de extremidade no seu recurso de borda de caixa de dados. O estado de tempo de execução do módulo adicionado é *em execução*.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-3.png) 
 
1.  No Explorador de ficheiros, ligar a ambos os locais e partilhas de cloud criou anteriormente.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-4.png) 
 
1.  Adicione os dados à partilha local.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-5.png) 
 
    Os dados são movidos para a partilha na cloud.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-6.png)  

    Os dados, em seguida, são emitidos da partilha na cloud para a conta de armazenamento. Para ver os dados, vá para o Explorador de armazenamento para ver os dados.

    ![Verificar a transformação dos dados](./media/data-box-edge-deploy-configure-compute/verify-data-transform-7.png) 
 
Concluiu o processo de validação.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar um recurso do Hub IoT
> * Configurar a função de computação
> * Adicionar um módulo de computação
> * Verificar a transformação e a transferência dos dados

Para saber como administrar o seu dispositivo Edge de caixa de dados, veja:

> [!div class="nextstepaction"]
> [Use local web UI to administer a Data Box Edge](https://aka.ms/dbg-docs) (Utilizar a IU da Web local para administrar o Data Box Edge)



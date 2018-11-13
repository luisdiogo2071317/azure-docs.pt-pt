---
title: Integração contínua do IoT Edge do Azure e a implementação contínua | Documentos da Microsoft
description: Descrição geral da integração contínua e implementação contínua para o Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 11/12/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a110c0a938e56c8ac276e0efed22ea3af23f111a
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578540"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Integração contínua e implementação contínua para o Azure IoT Edge

Este artigo demonstra como pode utilizar a integração contínua e os recursos de implementação contínua do Microsoft Team Foundation Server (TFS) e serviços de DevOps do Azure para criar, testar e implementar aplicações rápida e eficiente para o Azure IoT Edge. 

Neste artigo, ficará a saber como:
* Criar e verificar num exemplo de solução de IoT Edge.
* Instale a extensão Azure IoT Edge para sua programação e operações do Azure.
* Configure a integração contínua (CI) para criar a solução.
* Configure a implementação contínua (CD) para implementar a solução e ver respostas.

Irá demorar 30 minutos para concluir os passos neste artigo.

![CI e CD](./media/how-to-ci-cd/cd.png)


## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Criar uma solução do Azure IoT Edge de exemplo com o Visual Studio Code

Nesta secção, irá criar um exemplo do IoT Edge solução que contém testes de unidade que poderá ser executado como parte do processo de compilação. Antes de seguir as orientações nesta secção, conclua os passos na [desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code](tutorial-multiple-modules-in-vscode.md).

1. Na paleta de comandos do VS Code, escreva e execute o comando **do Azure IoT Edge: solução nova do IoT Edge**. Em seguida, selecione a pasta de área de trabalho, forneça o nome da solução (o nome predefinido é **EdgeSolution**) e criar um módulo de c# (**FilterModule**) como o primeiro módulo do utilizador nesta solução. Também tem de especificar o repositório de imagens do Docker para o seu primeiro módulo. O repositório de imagens padrão baseia-se um registo do Docker local (`localhost:5000/filtermodule`). Terá de alterá-lo para o Azure Container Registry (`<your container registry address>/filtermodule`) ou o Hub do Docker para ainda mais a integração contínua.

    ![Configurar o ACR](./media/how-to-ci-cd/acr.png)

2. Janela do VS Code carregará a sua área de trabalho de solução de IoT Edge. Além disso, pode, opcionalmente, escreva e execute **do Azure IoT Edge: módulo do IoT Edge/adicionar** para adicionar mais módulos. Há uma `modules` pasta, um `.vscode` pasta e um arquivo de manifesto do modelo de implementação na pasta raiz. Todos os códigos de módulo de utilizador será subpastas na pasta `modules`. O `deployment.template.json` é o modelo de manifesto de implantação. Alguns dos parâmetros existentes neste ficheiro vão ser analisado a partir do `module.json`, que existe na pasta de cada módulo.

3. Agora seu exemplo de solução de IoT Edge está pronto. O módulo c# padrão funciona como um módulo de mensagem de pipe. Na `deployment.template.json`, verá essa solução contém dois módulos. A mensagem será gerada a partir da `tempSensor` módulo e será ser encaminhado diretamente através de `FilterModule`, em seguida, enviadas ao seu hub IoT.

4. Guardar esses projetos, em seguida, verificá-lo no seu repositório de DevOps do Azure ou do TFS.
    
> [!NOTE]
> Para obter mais informações sobre como utilizar o Azure repositórios, consulte [compartilhar seu código com o Visual Studio e Azure repositórios](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-azure-pipeline-for-continuous-integration"></a>Configurar o Pipeline do Azure para integração contínua
Nesta secção, irá criar um pipeline de compilação que está configurado para ser executada automaticamente quando fizer check-in todas as alterações para o exemplo de solução de IoT Edge e irá mostrar registos de compilação no Pipeline do Azure.

1. Inicie sessão na sua organização de DevOps do Azure (**https://**_sua conta_**. visualstudio.com**) e abra o projeto em que tiver selecionado na aplicação de exemplo.

    ![Código de verificação](./media/how-to-ci-cd/init-project.png)

1. Visite [do Azure IoT Edge, para o Pipeline do Azure](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) no mercado de DevOps do Azure. Clique em **obter gratuitamente** e siga o Assistente para instalar esta extensão para a sua organização de DevOps do Azure ou a transferência para o TFS.

    ![Instalar a extensão](./media/how-to-ci-cd/install-extension.png)

1. No seu Azure DevOps, abra a **criar e lançar** hub e, no **baseia-se** separador, escolha **+ novo pipeline**. Ou, se já tiver de criação de pipelines, escolha o **+ novo** botão.

    ![Novo pipeline](./media/how-to-ci-cd/add-new-build.png)

1. Se lhe for pedido, selecione o **Git de DevOps do Azure** tipo de origem. Em seguida, selecione o projeto, o repositório e o ramo em que o seu código está localizado. Escolher **continuar**.

    ![Selecione o git](./media/how-to-ci-cd/select-vsts-git.png)

    Na **selecionar um modelo** janela, escolha **começar com um processo vazio**.

    ![Selecione um modelo](./media/how-to-ci-cd/start-with-empty.png)

1. No editor do pipeline, selecione o conjunto de agentes. 
    
    * Se gostaria de criar os seus módulos na plataforma amd64 para contentores do Linux, escolha **alojado 1604 da Ubuntu**
    * Se gostaria de criar os seus módulos na plataforma amd64 para contentores do Windows, escolha **Hosted VS2017** 
    * Se gostaria de criar os seus módulos na plataforma arm32v7 para contentores do Linux, tem de conjunto se o seu próprio agente de compilação clicando a **gerir** botão.
    
    ![Configurar o agente de compilação](./media/how-to-ci-cd/configure-env.png)

1. Tarefa de agente, clique em "+" para adicionar duas tarefas no pipeline de compilação. A primeira é partir **do Azure IoT Edge**. E a segunda é de **publicar artefactos de compilação**
    
    ![Adicionar tarefas](./media/how-to-ci-cd/add-tasks.png)

1. Na primeira **do Azure IoT Edge** de tarefas, atualize o **nome a apresentar** para **Push e de criar o módulo**e, no **ação** lista pendente, selecione **Criar e enviar por Push**. Na **Module ficheiro** caixa de texto, adicionar caminho ao mesmo. Em seguida, escolha **o tipo de registo de contentor**, certifique-se de configurar e selecionar o mesmo registo no seu code(module.json). Esta tarefa criar e enviar por push todos os seus módulos na solução e publicar no registo de contentor especificado. Se seus módulos são enviados aos registos diferentes, pode ter vários **Push e de criar o módulo** tarefas. No caso de que a solução de IoT Edge não está sob a raiz do repositório de código, é possível especificar **raiz de solução do caminho do Edge** na definição de compilação.
    
    ![Criar e enviar por push](./media/how-to-ci-cd/build-and-push.png)

1. Na **publicar artefactos de compilação** tarefa, tem de especificar o ficheiro de implementação gerado pela tarefa de compilação. Definir o **caminho para publicar** para "config/deployment.json". Se definir **raiz de solução do caminho do Edge** na última tarefa, terá de associar o caminho da raiz aqui. Por exemplo, se o caminho da raiz da solução de borda é ". / edgesolution", em seguida, o **caminho para publicar** deve ser ". / edgesolution/config/deployment.json". O `deployment.json` ficheiro é gerado durante o tempo de compilação, portanto, é seguro ignorar as linhas de erro vermelha na caixa de texto. 

    ![Publicar artefactos](./media/how-to-ci-cd/publish-build-artifacts.png)

1. Abra o **Acionadores** separador e ativar o **integração contínua** acionador. Certifique-se de que o ramo que contém o código está incluído.

    ![Configurar o acionador](./media/how-to-ci-cd/configure-trigger.png)

    Guarde o novo pipeline de compilação. Clique no botão **Guardar**.


## <a name="configure-azure-pipeline-for-continuous-deployment"></a>Configurar o Pipeline do Azure para a implementação contínua
Nesta secção, irá criar um pipeline de versão que está configurado para ser executada automaticamente quando o pipeline de compilação cai artefactos e ele mostrará os registos de implementação no Pipeline do Azure.

1. Na **versões** separador, escolha **+ novo pipeline**. Ou, se já tiver pipelines de versão, escolha o **+ novo** botão.  

    ![Adicionar o pipeline de lançamento](./media/how-to-ci-cd/add-release-pipeline.png)

    No **selecionar um modelo** janela, selecione **começar com uma tarefa está vazia.**

    ![Começar com a tarefa está vazia](./media/how-to-ci-cd/start-with-empty-job.png)

2. Em seguida, o pipeline de lançamento deve inicializar com uma fase: **fase 1**. Mudar o nome da **fase 1** ao **controle de qualidade** e tratá-lo como um ambiente de teste. Num pipeline de implementação contínua típico, ela existe, normalmente, várias fases, pode criar mais com base nas suas práticas de DevOps.

    ![Criar estágio](./media/how-to-ci-cd/QA-env.png)

3. A versão de uma ligação para os artefactos de compilação. Clique em **adicionar** na área de artefactos.

    ![Adicione os artefactos](./media/how-to-ci-cd/add-artifacts.png)  
    
    Na **adicionar uma página de artefacto**, escolha o tipo de origem **criar**. Em seguida, selecione o projeto e o pipeline de compilação que criou. Em seguida, clique em **adicionar**.

    ![Adicione um artefacto](./media/how-to-ci-cd/add-an-artifact.png)

    Abra o acionador de implementação contínua para que a nova versão será criada sempre que uma nova compilação está disponível.

    ![Configurar o acionador](./media/how-to-ci-cd/add-a-trigger.png)

4. Navegue para **estágio de controle de qualidade** e configurar as tarefas neste estágio.

    ![Configurar tarefas de controle de qualidade](./media/how-to-ci-cd/view-stage-tasks.png)

   Tarefas de implementação são a plataforma de minúsculas, o que significa que pode escolher entre **Hosted VS2017** ou **alojados 1604 de Ubuntu** no **conjunto de agentes** (ou qualquer outro agente gerido pelo por conta própria). Clique em "+" e adicione uma tarefa.

    ![Adicionar tarefas para controle de qualidade](./media/how-to-ci-cd/add-task-qa.png)

5. A tarefa do Azure IoT Edge, navegue para o **ação** lista pendente, selecione **implementar no dispositivo IoT Edge**. Selecione seu **subscrição do Azure** e entrada sua **nome do IoT Hub**. Pode especificar um IoT Edge **ID de implementação** e a implementação **prioridade**. Também pode optar por implementar um ou vários dispositivos. Se estiver a implementar **vários dispositivos**, tem de especificar o dispositivo **condição de destino**. A condição de destino é um filtro para corresponder a um conjunto de dispositivos periféricos do IoT Hub. Se pretender utilizar etiquetas do dispositivo como a condição, terá de atualizar os dispositivos correspondentes etiquetas com twin de dispositivos do IoT Hub. Suponha que tem vários do IoT Edge que dispositivos foram marcados como "controle de qualidade,", em seguida, a configuração de tarefas deve ser como na captura de ecrã seguinte. 

    ![Implementar no controle de qualidade](./media/how-to-ci-cd/deploy-to-qa.png)

    Guarde o novo pipeline de lançamento. Clique no botão **Guardar**. E, em seguida, clique em **Pipeline** voltar para o pipeline.

6. A segunda fase é para o seu ambiente de produção. Para adicionar uma nova fase "PROD", pode simplesmente clonar o estágio "QA" e mude o nome palco clonado para **PROD**,

    ![Fase de clone](./media/how-to-ci-cd/clone-stage.png)

7. Configure as tarefas para o seu ambiente de produção. Suponha que tem vários do IoT Edge que dispositivos foram marcados como "prod", em configurações de tarefa, atualizar a condição de destino para "prod" e defina o id de implementação como "implementar-prod". Clique no botão **Guardar**. E, em seguida, clique em **Pipeline** voltar para o pipeline.
    
    ![Implementar em produção](./media/how-to-ci-cd/deploy-to-prod.png)

7. Atualmente, nossa artefactos de compilação serão possível acionar a continuamente **controle de qualidade** estágio e, em seguida **PROD** estágio. Mas a maioria das vezes que precisa integrar alguns casos de teste nos dispositivos de controle de qualidade e manualmente aprovar os bits. Mais tarde os bits serão implementados ao ambiente de produção. Configure uma aprovação na fase de produção de como o seguinte.

    1. Open **condições de pré-implantação** painel de definição.

        ![Condições de pré-implantação abertas](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Definir **Enabled** na **aprovações de pré-implantação**. E preencha a **aprovadores** entrada. Em seguida, clique em **Guardar**.
    
        ![Conjunto de condições](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


8. Agora seu pipeline de lançamento foi configurado como o seguinte.

    ![Pipeline de lançamento](./media/how-to-ci-cd/release-pipeline.png)

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Verifique se o IoT Edge CI/CD com a compilação e lançar pipelines

Nesta secção, irá acionar uma compilação para tornar o pipeline de CI/CD trabalhar. Em seguida, verifique se o resultado com o portal do Azure DevOps. 

1. Para acionar uma tarefa de compilação, pode emitir uma confirmação para o repositório de código fonte ou acionar manualmente. Pode acionar uma tarefa de compilação no seu pipeline de compilação clicando a **fila** botão como na seguinte captura de ecrã.

    ![Acionador manual](./media/how-to-ci-cd/manual-trigger.png)

2. Se o pipeline de compilação for concluído com êxito, irá acionar uma versão **controle de qualidade** estágio. Navegue para criar registos de pipeline e deverá ver o seguinte.

    ![Os registos de compilação](./media/how-to-ci-cd/build-logs.png)

3. A implementação bem-sucedida **controle de qualidade** estágio dispararia uma notificação para o aprovador. Navegue para o pipeline de versões, pode ver o seguinte. 

    ![Aprovação pendente](./media/how-to-ci-cd/pending-approval.png)


4. Depois do aprovador aprovar esta alteração, pode ser implementado para **PROD**.

    ![Implementar em produção](./media/how-to-ci-cd/approve-and-deploy-to-prod.png)


## <a name="next-steps"></a>Passos Seguintes

* Compreender a implementação de IoT Edge no [implementações de compreender o IoT Edge para dispositivos individuais ou em escala](module-deployment-monitoring.md)
* Siga os passos para criar, atualizar ou eliminar uma implementação no [implementar e monitorizar os módulos do IoT Edge em escala](how-to-deploy-monitor.md).

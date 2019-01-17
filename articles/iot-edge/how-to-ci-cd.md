---
title: Integração contínua e implementação contínua – Azure IoT Edge | Documentos da Microsoft
description: Configurar a integração contínua e implementação contínua – Azure IoT Edge com o Azure DevOps, Pipelines do Azure
author: shizn
manager: philmea
ms.author: xshi
ms.date: 12/12/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: aef88a4fbc7d71ee1438333afd9773d1aba3ed9c
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359158"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Integração contínua e implementação contínua para o Azure IoT Edge

Pode adotar facilmente DevOps com as suas aplicações do Azure IoT Edge com as tarefas incorporadas do Azure IoT Edge em Pipelines do Azure ou [Plug-in do Azure IoT Edge do Jenkins](https://plugins.jenkins.io/azure-iot-edge) no seu servidor do Jenkins. Este artigo demonstra como pode utilizar a integração contínua e os recursos de implementação contínua do Pipelines do Azure para criar, testar e implementar aplicações rápida e eficiente para o Azure IoT Edge. 

Neste artigo, ficará a saber como:
* Criar e verificar num exemplo de solução de IoT Edge.
* Configure a integração contínua (CI) para criar a solução.
* Configure a implementação contínua (CD) para implementar a solução e ver respostas.

![Diagrama - CI e CD ramos para desenvolvimento e produção](./media/how-to-ci-cd/cd.png)


## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Criar uma solução do Azure IoT Edge de exemplo com o Visual Studio Code

Nesta secção, irá criar um exemplo do IoT Edge solução que contém testes de unidade que poderá ser executado como parte do processo de compilação. Antes de seguir as orientações nesta secção, conclua os passos na [desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code](how-to-develop-multiple-modules-vscode.md).

1. Na paleta de comandos do VS Code, escreva e execute o comando **Azure IoT Edge: Nova solução de IoT Edge**. Em seguida, selecione a pasta de área de trabalho, forneça o nome da solução (o nome predefinido é **EdgeSolution**) e criar um módulo de c# (**FilterModule**) como o primeiro módulo do utilizador nesta solução. Também tem de especificar o repositório de imagens do Docker para o seu primeiro módulo. O repositório de imagens padrão baseia-se um registo do Docker local (`localhost:5000/filtermodule`). Altere-o para o Azure Container Registry (`<your container registry address>/filtermodule`) ou o Hub do Docker para ainda mais a integração contínua.

    ![Configurar o Azure Container Registry](./media/how-to-ci-cd/acr.png)

2. Janela do VS Code carregará a sua área de trabalho de solução de IoT Edge. Além disso, pode, opcionalmente, escreva e executar **Azure IoT Edge: Adicionar módulo do IoT Edge** para adicionar mais módulos. Há uma `modules` pasta, um `.vscode` pasta e um arquivo de manifesto do modelo de implementação na pasta raiz. Todos os códigos de módulo de utilizador será subpastas na pasta `modules`. O `deployment.template.json` é o modelo de manifesto de implantação. Alguns dos parâmetros existentes neste ficheiro vão ser analisado a partir do `module.json`, que existe na pasta de cada módulo.

3. Agora seu exemplo de solução de IoT Edge está pronto. O módulo c# padrão funciona como um módulo de mensagem de pipe. Na `deployment.template.json`, verá essa solução contém dois módulos. A mensagem será gerada a partir da `tempSensor` módulo e será ser encaminhado diretamente através de `FilterModule`, em seguida, enviadas ao seu hub IoT.

4. Guardar esses projetos, em seguida, consolidar no seu repositório de repositórios do Azure.
    
> [!NOTE]
> Para obter mais informações sobre como utilizar o Azure repositórios, consulte [compartilhar seu código com o Visual Studio e Azure repositórios](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-azure-pipelines-for-continuous-integration"></a>Configurar Pipelines do Azure para integração contínua
Nesta secção, irá criar um pipeline de compilação que está configurado para ser executada automaticamente quando fizer check-in todas as alterações para o exemplo de solução de IoT Edge e ele mostrará os registos de compilação em Pipelines do Azure.

1. Inicie sessão na sua organização de DevOps do Azure ( **https://dev.azure.com/{your organização} /**) e abra o projeto em que tiver selecionado na aplicação de exemplo.

    ![Código de verificação para os Pipelines do Azure](./media/how-to-ci-cd/init-project.png)

1. No seus Pipelines do Azure, abra a **baseia-se** separador, escolha **+ novo pipeline**. Ou, se já tiver de criação de pipelines, escolha o **+ novo** botão. Em seguida, selecione **criar novo pipeline**.

    ![Criar um novo pipeline de compilação](./media/how-to-ci-cd/add-new-build.png)

1. Se lhe for pedido, selecione repositórios do Azure para a sua origem. Em seguida, selecione o projeto, o repositório e o ramo em que o seu código está localizado. Escolher **continuar**.

    ![Selecione Azure repositórios Git](./media/how-to-ci-cd/select-vsts-git.png)

    Na **selecionar um modelo** janela, escolha **começar com um processo vazio**.

    ![Começar com um processo vazio](./media/how-to-ci-cd/start-with-empty.png)

1. No editor do pipeline, selecione o conjunto de agentes. 
    
    * Se gostaria de criar os seus módulos na plataforma amd64 para contentores do Linux, escolha **alojado 1604 da Ubuntu**
    * Se gostaria de criar os seus módulos na plataforma amd64 para contentores do Windows 1809, terá [configure autoalojado agente no Windows](https://docs.microsoft.com/azure/devops/pipelines/agents/v2-windows?view=vsts).
    * Se gostaria de criar os seus módulos na plataforma arm32v7 para contentores do Linux, tem de [configure autoalojado agente no Linux](https://blogs.msdn.microsoft.com/iotdev/2018/11/13/setup-azure-iot-edge-ci-cd-pipeline-with-arm-agent/).
    
    ![Configurar o conjunto de agentes de compilação](./media/how-to-ci-cd/configure-env.png)

1. Tarefa de agente, abra "+" para adicionar três tarefas no pipeline de compilação. As duas primeiras são partir **do Azure IoT Edge**. E o terceiro é de **publicar artefactos de compilação**
    
    ![Adicionar tarefas para o pipeline de compilação](./media/how-to-ci-cd/add-tasks.png)

1. Na primeira **do Azure IoT Edge** de tarefas, atualize o **nome a apresentar** para **Azure IoT Edge - imagens de módulo de compilação**e, no **ação** pendente lista, selecione **criar imagens de módulo**. Na **. ficheiro Template JSON** controlo, selecione a **deployment.template.json** arquivo, que descreve a sua solução de IoT Edge. Em seguida, escolha **plataforma padrão**, certifique-se de selecionar a mesma plataforma como o seu dispositivo IoT Edge. Esta tarefa será criar todos os módulos na solução com a plataforma de destino especificado. E também gerar os **deployment.json** ficheiro, pode encontrar o caminho do ficheiro em variáveis de saída. Defina o alias para `edge` desta variável.
    
    ![Configurar tarefas de imagens do módulo de compilação](./media/how-to-ci-cd/build-and-push.png)

1. Na segunda **do Azure IoT Edge** de tarefas, atualize o **nome a apresentar** para **Azure IoT Edge - imagens de módulo de Push**e, no **ação** pendente lista, selecione **enviar imagens de módulo**. Escolha o tipo de registo de contentor, certifique-se de configurar e selecionar o mesmo registo no seu code(module.json). Na **. ficheiro Template JSON** controlo, selecione a **deployment.template.json** arquivo, que descreve a sua solução de IoT Edge. Em seguida, escolha **plataforma padrão**, certifique-se de selecionar a mesma plataforma para as imagens de módulo criados. Esta tarefa será enviar todas as imagens de módulo para o registo de contentor que selecionou. E também adicionar credenciais de registo de contentor no **deployment.json** ficheiro, pode encontrar o caminho do ficheiro em variáveis de saída. Defina o alias para `edge` desta variável. Se tiver vários registos de contentores para alojar as suas imagens de módulo, precisa duplicar esta tarefa, selecione o registo de contentor diferente e utilizar **ignorar módulos** nas definições avançadas para ignorar as imagens que não são para isso Registro específicos.

    ![Configurar tarefas de imagens do módulo de push](./media/how-to-ci-cd/push.png)

1. Na **publicar artefactos de compilação** tarefa, tem de especificar o ficheiro de implementação gerado pela tarefa de compilação. Definir o **caminho para publicar** para `$(edge.DEPLOYMENT_FILE_PATH)`.

    ![Configurar tarefas de artefacto de publicar](./media/how-to-ci-cd/publish-build-artifacts.png)

1. Abra o **Acionadores** separador e ativar o **integração contínua** acionador. Certifique-se de que o ramo que contém o código está incluído.

    ![Ativar o acionador de integração contínua](./media/how-to-ci-cd/configure-trigger.png)

    Guardar o novo pipeline de compilação com **guardar** botão.


## <a name="configure-azure-pipelines-for-continuous-deployment"></a>Configurar Pipelines do Azure para a implementação contínua
Nesta secção, irá criar um pipeline de versão que está configurado para ser executada automaticamente quando o pipeline de compilação cai artefactos e ele mostrará os registos de implementação nos Pipelines do Azure.

1. Na **versões** separador, escolha **+ novo pipeline**. Ou, se já tiver pipelines de versão, escolha o **+ novo** e selecione **+ novo pipeline de versões**.  

    ![Adicionar o pipeline de lançamento](./media/how-to-ci-cd/add-release-pipeline.png)

    No **selecionar um modelo** janela, selecione **começar com uma tarefa está vazia.**

    ![Começar com uma tarefa vazia](./media/how-to-ci-cd/start-with-empty-job.png)

2. Em seguida, o pipeline de lançamento seria inicializar com uma fase: **Fase 1**. Mudar o nome da **fase 1** ao **controle de qualidade** e tratá-lo como um ambiente de teste. Num pipeline de implementação contínua típico, ela existe, normalmente, várias fases, pode criar mais com base nas suas práticas de DevOps.

    ![Criar o estágio de ambiente de teste](./media/how-to-ci-cd/QA-env.png)

3. A versão de uma ligação para os artefactos de compilação. Clique em **adicionar** na área de artefactos.

    ![Adicione os artefactos](./media/how-to-ci-cd/add-artifacts.png)  
    
    Na **adicionar uma página de artefacto**, escolha o tipo de origem **criar**. Em seguida, selecione o projeto e o pipeline de compilação que criou. Em seguida, selecione **Adicionar**.

    ![Adicione um artefacto de compilação](./media/how-to-ci-cd/add-an-artifact.png)

    Abra o acionador de implementação contínua para que a nova versão será criada sempre que uma nova compilação está disponível.

    ![Configurar o acionador de implementação contínua](./media/how-to-ci-cd/add-a-trigger.png)

4. Navegue para **estágio de controle de qualidade** e configurar as tarefas neste estágio.

    ![Configurar tarefas de controle de qualidade](./media/how-to-ci-cd/view-stage-tasks.png)

   Tarefas de implementação são a plataforma de minúsculas, o que significa que pode escolher entre **Hosted VS2017** ou **alojados 1604 de Ubuntu** no **conjunto de agentes** (ou qualquer outro agente gerido pelo por conta própria). Selecione "+" e adicione uma tarefa.

    ![Adicionar tarefas para controle de qualidade](./media/how-to-ci-cd/add-task-qa.png)

5. A tarefa do Azure IoT Edge, navegue para o **ação** lista pendente, selecione **implementar no dispositivo IoT Edge**. Selecione seu **subscrição do Azure** e entrada sua **nome do IoT Hub**. Pode optar por implementar um ou vários dispositivos. Se estiver a implementar **vários dispositivos**, tem de especificar o dispositivo **condição de destino**. A condição de destino é um filtro para corresponder a um conjunto de dispositivos periféricos do IoT Hub. Se pretender utilizar etiquetas do dispositivo como a condição, terá de atualizar os dispositivos correspondentes etiquetas com twin de dispositivos do IoT Hub. Atualização do **ID de implementação do IoT Edge** para "implementar-controle de qualidade" em definições avançadas. Suponha que tem vários do IoT Edge que dispositivos foram marcados como "controle de qualidade,", em seguida, a configuração de tarefas deve ser como na captura de ecrã seguinte. 

    ![Implementar no controle de qualidade](./media/how-to-ci-cd/deploy-to-qa.png)

    Guardar o novo pipeline de lançamento com o **guardar** botão. E, em seguida, selecione **Pipeline** voltar para o pipeline.

6. A segunda fase é para o seu ambiente de produção. Para adicionar uma nova fase "PROD", pode clonar o estágio "QA" e mudar o nome do estágio clonado para **PROD**,

    ![Fase de clone](./media/how-to-ci-cd/clone-stage.png)

7. Configure as tarefas para o seu ambiente de produção. Suponha que tem vários do IoT Edge que dispositivos foram marcados como "prod", em configurações de tarefas, a condição de destino para "prod" e defina o ID de implementação como "implementar-prod" nas definições avançadas de atualização. Salve-o com o **guardar** botão. E, em seguida, selecione **Pipeline** voltar para o pipeline.
    
    ![Implementar em produção](./media/how-to-ci-cd/deploy-to-prod.png)

7. Atualmente, nossa artefactos de compilação serão possível acionar a continuamente **controle de qualidade** estágio e, em seguida **PROD** estágio. Mas a maioria das vezes que precisa integrar alguns casos de teste nos dispositivos de controle de qualidade e manualmente aprovar os bits. Mais tarde os bits serão implementados ao ambiente de produção. Configure uma aprovação na fase de produção, como a captura de ecrã seguinte.

    1. Open **condições de pré-implantação** painel de definição.

        ![Condições de pré-implantação abertas](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Definir **Enabled** na **aprovações de pré-implantação**. E preencha a **aprovadores** entrada. Em seguida, guarde-o com **guardar** botão.
    
        ![Conjunto de condições](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


8. Agora seu pipeline de lançamento foi configurado como a captura de ecrã seguinte.

    ![Pipeline de lançamento](./media/how-to-ci-cd/release-pipeline.png)

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Verifique se o IoT Edge CI/CD com a compilação e lançar pipelines

Nesta secção, irá acionar uma compilação para tornar o pipeline de CI/CD trabalhar. Em seguida, verifique se que a implementação com êxito.

1. Para acionar uma tarefa de compilação, pode emitir uma confirmação para o repositório de código fonte ou acionar manualmente. Pode acionar uma tarefa de compilação no seu pipeline de compilação, selecionando o **fila** botão como na seguinte captura de ecrã.

    ![Acionador manual](./media/how-to-ci-cd/manual-trigger.png)

2. Se o pipeline de compilação for concluído com êxito, irá acionar uma versão **controle de qualidade** estágio. Navegue para criar registos de pipeline e verá a seguinte captura de ecrã.

    ![Criar registos](./media/how-to-ci-cd/build-logs.png)

3. A implementação bem-sucedida **controle de qualidade** estágio dispararia uma notificação para o aprovador. Navegue para o pipeline de versões, pode ver a seguinte captura de ecrã. 

    ![Aprovação pendente](./media/how-to-ci-cd/pending-approval.png)


4. Depois do aprovador aprovar esta alteração, pode ser implementado para **PROD**.

    ![Implementar em produção](./media/how-to-ci-cd/approve-and-deploy-to-prod.png)


## <a name="next-steps"></a>Passos Seguintes

* Compreender a implementação de IoT Edge no [implementações de compreender o IoT Edge para dispositivos individuais ou em escala](module-deployment-monitoring.md)
* Siga os passos para criar, atualizar ou eliminar uma implementação no [implementar e monitorizar os módulos do IoT Edge em escala](how-to-deploy-monitor.md).

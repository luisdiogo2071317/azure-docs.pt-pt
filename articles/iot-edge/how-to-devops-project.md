---
title: Pipeline de CI/CD com projetos de DevOps do Azure - Azure IoT Edge | Documentos da Microsoft
description: Projetos de DevOps do Azure torna mais fácil começar a utilizar no Azure. Ele ajuda-o a iniciar uma aplicação do Azure IoT Edge à sua escolha em alguns passos rápidos.
author: shizn
manager: ''
ms.author: xshi
ms.date: 12/04/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: ebb7e515f9d9205f364d50b3d686c68a2988f86a
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074219"
---
# <a name="create-a-cicd-pipeline-for-iot-edge-with-azure-devops-projects-preview"></a>Criar um pipeline CI/CD para o IoT Edge com projetos de DevOps do Azure (pré-visualização)

Configure integração contínua (CI) e a entrega contínua (CD) para a sua aplicação do IoT Edge com projetos de DevOps. Projetos de DevOps simplifica a configuração inicial de um pipeline de compilação e versão nos Pipelines do Azure.

Se não tiver uma subscrição ativa do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free) antes de começar.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Projetos de DevOps cria um pipeline CI/CD no Azure DevOps. Pode criar uma nova organização de DevOps do Azure ou utilizar uma organização existente. Projetos de DevOps também cria os recursos do Azure na subscrição do Azure à sua escolha.

1. Inicie sessão no [Portal do Microsoft Azure](https://portal.azure.com).

1. No painel esquerdo, selecione **criar um recurso** ícone na barra de navegação esquerda e, em seguida, procure **projetos de DevOps**.  

1.  Selecione **Criar**.

## <a name="select-a-sample-application-and-azure-service"></a>Selecione um exemplo de aplicação e serviço do Azure

1. Os módulos do Azure IoT Edge podem ser escritos em [ C# ](tutorial-csharp-module.md), [node. js](tutorial-node-module.md), [Python](tutorial-python-module.md), [C](tutorial-c-module.md) e [Java](tutorial-java-module.md). Selecione seu idioma preferencial para iniciar uma nova aplicação. Do mesmo modo, pode selecionar **.NET**, **node. js**, **Python**, **C**, ou **Java**e, em seguida, clique em **Seguinte**.

    ![Selecione o idioma para criar uma nova aplicação](./media/how-to-devops-project/select-language.png)

2. Selecione **simples IoT (pré-visualização)** e, em seguida, clique em **próxima**.

    ![Selecione o framework de IoT simples](media/how-to-devops-project/select-iot.png)

3. Selecione **IoT Edge**e, em seguida, clique em **próxima**.

    ![Selecione o serviço de IoT Edge](media/how-to-devops-project/select-iot-edge.png)

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Configurar o Azure DevOps e uma subscrição do Azure

1. Crie uma nova organização do Azure DevOps gratuita ou escolher uma organização existente.

    a. Escolha um nome para o seu projeto. 

    b. Selecione a subscrição do Azure e a localização, escolha um nome para a sua aplicação e, em seguida, selecione **feito**.  

    ![Dê um nome e a criar aplicação](media/how-to-devops-project/select-devops.png)

1. Após alguns minutos, é apresentado o dashboard de projetos de DevOps no portal do Azure. Um exemplo de aplicação do IoT Edge é configurar num repositório na sua organização de DevOps do Azure, uma compilação é executada e a aplicação é implementada para o dispositivo do IoT Edge. Este dashboard fornece visibilidade para o repositório de código, o pipeline de CI/CD e a sua aplicação no Azure.

    ![Aplicação de vista no portal de DevOps](./media/how-to-devops-project/devops-portal.png)


## <a name="commit-code-changes-and-execute-cicd"></a>Consolidar as alterações de código e executar o CI/CD

Projetos de DevOps criado um repositório de Git em repositórios do Azure ou do GitHub. Para ver o repositório e fazer alterações de código para a sua aplicação, siga os passos abaixo:

1. À esquerda do dashboard de projetos de DevOps, selecione a ligação para o seu **mestre** ramo.  
Esta ligação abre uma vista para o repositório Git recentemente criado.

1. Para ver o URL de clone do repositório, selecione **Clone** na parte superior direita do browser. Pode clonar o repositório de Git no VS Code ou outras ferramentas preferidas. Em alguns passos que se seguem, é usar o navegador da web para fazer e consolidação de código altera diretamente para o ramo principal.

    ![Repositório de git clone](media/how-to-devops-project/clone.png)

1. No lado esquerdo do browser, vá para o **modules/FilterModule/module.json** ficheiro.

1. Selecione **edite**e, em seguida, faça uma alteração ao `"version"` sob o `"tag"`. Por exemplo, pode atualizá-la para `"version": "${BUILD_BUILDID}"` utilizar [do Azure DevOps criar variáveis](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=vsts#build-variables) como parte do seu tag de imagem de módulo do Azure IoT Edge.

    ![Editar a versão para aceitar as variáveis de compilação](media/how-to-devops-project/update-module-json.png)

1. Selecione **consolidar**e, em seguida, guarde as alterações.

1. No seu browser, aceda ao dashboard do projeto de DevOps do Azure.  Deverá agora ver que está em curso uma compilação. As alterações que efetuou são automaticamente criadas e implementadas por meio de um pipeline CI/CD.

    ![Ver o estado em curso](media/how-to-devops-project/ci-cd-in-progress.png)

## <a name="examine-the-cicd-pipeline"></a>Examine o pipeline de CI/CD

No passo anterior, os projetos de DevOps do Azure configurado automaticamente um pipeline CI/CD completo para a sua aplicação do IoT Edge. Explore e personalize o pipeline, conforme necessário. Siga os passos seguintes para familiarizar-se com a compilação de DevOps do Azure e lançar pipelines.

1. Na parte superior do dashboard de projetos de DevOps, selecione **criar Pipelines**.  
Esse link abre um separador do browser e o Azure DevOps criar pipeline para o novo projeto.

1. Selecione **Editar**.

    ![Editar o pipeline de compilação](media/how-to-devops-project/click-edit-button.png)

1. Neste painel, pode examinar as várias tarefas para o seu pipeline de compilação. A compilação executa várias tarefas, tais como a obter origens de repositório de Git, criação de imagens do módulo de IoT Edge, envio de módulos do IoT Edge e publicação saídas utilizadas que são utilizadas para implementações. Para saber mais sobre as tarefas do Azure IoT Edge para CI, pode visitar [configurar Pipelines do Azure para integração contínua](https://docs.microsoft.com/azure/iot-edge/how-to-ci-cd#configure-azure-pipelines-for-continuous-integration).

    ![Tarefas da vista de integração contínua](media/how-to-devops-project/ci.png)

1. Na parte superior do pipeline de compilação, selecione o nome do pipeline de compilação.

1. Altere o nome do seu pipeline de compilação para algo mais descritivo, selecione **guardar e colocar em fila**e, em seguida, selecione **guardar**.

1. No nome do pipeline de compilação, selecione **Histórico**.   
Na **histórico** painel, verá uma trilha de auditoria das alterações recentes para a compilação.  Pipelines do Azure mantém um registro de quaisquer alterações efetuadas no pipeline de compilação e permite-lhe comparar versões.

1. Selecione **Acionadores**. Projetos de DevOps criado automaticamente um acionador de CI e cada consolidação no repositório inicia uma nova compilação.  Opcionalmente, pode optar por incluir ou excluir os ramos do processo de CI.

1. Selecione **Retenção**. Dependendo do seu cenário, pode especificar políticas de manter ou remover um determinado número de compilações.

1. Selecione **lançamento** sob **Pipelines**. Projetos de DevOps cria um pipeline de lançamento para gerir implementações do Azure IoT Edge.

    ![Pipeline de lançamento do Vista](media/how-to-devops-project/release-pipeline.png)

1. Selecione **Editar**. O pipeline de lançamento contém um pipeline, que define o processo de liberação.  

1. Em **Artefactos**, selecione **Remover**. O pipeline de compilação que examinou nos passos anteriores produz a saída utilizada para o artefacto. 

1. Junto a **Drop** ícone, selecione a **acionador de implementação contínua**.  
Esta versão de pipeline tem um acionador de CD ativado, o que é executada uma implantação sempre que houver um artefacto de compilação nova disponível. Opcionalmente, pode desativar o acionador para que as suas implementações exigem execução manual.  

1. No lado esquerdo, selecione **tarefas**. As tarefas são as atividades que executa o processo de implementação. Neste exemplo, foi criada uma tarefa para implementar as imagens de módulo do Azure IoT Edge. Para saber mais sobre as tarefas do Azure IoT Edge para CD, pode visitar [configurar Pipelines do Azure para a implementação contínua](https://docs.microsoft.com/azure/iot-edge/how-to-ci-cd#configure-azure-pipelines-for-continuous-deployment).

    ![Visualizar tarefas de implementação contínua](media/how-to-devops-project/dev-release.png)

1. À direita, selecione **ver versões**. Esta vista mostra um histórico das versões.

1. Selecione as reticências (...) junto a um dos seus lançamentos e, em seguida, selecione **aberto**.  
Existem vários menus para explorar, como um resumo de lançamento, itens de trabalho associados e testes.

1. Selecione **Consolidações**. Esta vista mostra as confirmações de código que estão associadas a implementação específica. 

1. Selecionar **Registos**. Os registos contêm informações úteis sobre o processo de implementação. Podem ser vistos durante e após as implementações.


## <a name="clean-up-resources"></a>Limpar recursos

É possível eliminar o serviço de aplicações do Azure e outros recursos relacionados que criou quando não precisar delas. Utilize o **eliminar** funcionalidade no dashboard de projetos de DevOps.

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre as tarefas do Azure IoT Edge no Azure DevOps no [integração contínua e implementação contínua para o Azure IoT Edge](how-to-ci-cd.md)
* Compreender a implementação de IoT Edge no [implementações de compreender o IoT Edge para dispositivos individuais ou em escala](module-deployment-monitoring.md)
* Siga os passos para criar, atualizar ou eliminar uma implementação no [implementar e monitorizar os módulos do IoT Edge em escala](how-to-deploy-monitor.md).

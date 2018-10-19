---
title: 'Início Rápido: Criar uma área de trabalho do serviço de aprendizagem automática no portal do Azure – Azure Machine Learning'
description: Utilize o portal do Azure para criar uma área de trabalho do serviço Azure Machine Learning. Esta área de trabalho é o pilar na cloud para experimentação, preparação e implementação de modelos de machine learning com o serviço do Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: rastala
ms.author: roastala
ms.date: 09/24/2018
ms.openlocfilehash: b6f0201a36a676e7647b9f5e60bc2df3415b9594
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831335"
---
# <a name="quickstart-use-the-azure-portal-to-get-started-with-azure-machine-learning-service"></a>Início Rápido: Utilizar o portal do Azure para começar a utilizar o serviço Azure Machine Learning

Neste início rápido, vai utilizar o portal do Azure para criar uma área de trabalho do serviço Azure Machine Learning. Esta área de trabalho é o pilar na cloud para experimentação, preparação e implementação de modelos de machine learning com o serviço do Machine Learning. 

Neste tutorial, irá:

* Criar uma área de trabalho na sua subscrição do Azure
* Faça um teste com o Python num Bloco de Notas do Azure e registe os valores em várias iterações
* Ver os valores registados na área de trabalho

Para sua comodidade, os seguintes recursos do Azure são adicionados automaticamente à sua área de trabalho quando estiverem disponíveis a nível regional: [registo de contentor](https://azure.microsoft.com/services/container-registry/), [armazenamento](https://azure.microsoft.com/services/storage/), [estatísticas das aplicações](https://azure.microsoft.com/services/application-insights/) e [cofre de chaves](https://azure.microsoft.com/services/key-vault/).

Os recursos que criar podem ser utilizados como pré-requisitos para outros tutoriais e artigos de procedimentos do serviço Azure Machine Learning. Tal como noutros serviços do Azure, existem limites em certos recursos (por exemplo, tamanho do cluster BatchAI) associados ao serviço do Azure Machine Learning. Leia [este](how-to-manage-quotas.md) artigo sobre os limites predefinidos e como pedir mais quota.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


## <a name="create-a-workspace"></a>Criar uma área de trabalho 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

Na página da área de trabalho, clique em `Explore your Azure Machine Learning service workspace`

 ![explorar a área de trabalho](./media/quickstart-get-started/explore_aml.png)


## <a name="use-the-workspace"></a>Utilizar a área de trabalho

Veja agora como uma área de trabalho ajuda-o a gerir os seus scripts de aprendizagem automática. Nesta secção, vai:

* Abrir um bloco de notas no Azure Notebooks
* Executar código que cria alguns valores registados
* Ver os valores registados na área de trabalho

Este é um exemplo de como a área de trabalho pode ajudá-lo a manter o controlo das informações geradas num script. 

### <a name="open-a-notebook"></a>Abrir um bloco de notas 

O Azure Notebooks fornece uma plataforma na cloud gratuita para blocos de notas do Jupyter, pré-configurada com tudo o que precisa para executar o serviço do Azure Machine Learning.  

Clique no botão `Open Azure Notebooks` para tentar a sua primeira experiência.

 ![Iniciar o Azure Notebooks](./media/quickstart-get-started/explore_ws.png)

Depois de iniciar sessão, é aberto um novo separador e um aviso `Clone Library` é apresentado.  Clique em `Clone`


### <a name="run-the-notebook"></a>Executar o bloco de notas

Juntamente com dois blocos de notas, verá um ficheiro `config.json`.  Este ficheiro de configuração contém informações sobre a área de trabalho que acabou de criar.  

Clique em `01.run-experiment.ipynb` para abrir o bloco de notas.

Pode executar as células uma de cada vez com `Shift`+`Enter`.  Ou utilize o menu `Cells` > `Run All` para executar o bloco de notas completo.  Quando vir [*] ao lado de uma célula, significa que está em execução.  Quando o código dessa célula for concluído, será apresentado um número.

Ser-lhe-á pedido que inicie sessão.  Copie o código na mensagem e, em seguida, clique na ligação e cole o código na nova janela.  Certifique-se de que não copia um espaço antes ou depois do código.  Inicie sessão com a mesma conta utilizada no portal do Azure.

 ![início de sessão](./media/quickstart-get-started/login.png)

No bloco de notas, a segunda célula lê a partir do `config.json` para ligar à sua área de trabalho.
```
ws = Workspace.from_config()
```

A terceira célula de código começa uma experimentação com o nome "my-first-experiment".  Irá utilizar este nome para procurar informações sobre a execução na sua área de trabalho.

```
experiment = Experiment(workspace_object=ws, name = "my-first-experiment")
```

Observe, na última célula do bloco de notas, os valores que foram escritos num ficheiro de registo.

```
# Log final results
run.log("Final estimate: ",pi_estimate)
run.log("Final error: ",math.pi-pi_estimate)
```

Pode ver estes valores na sua área de trabalho após a execução do código.

## <a name="view-logged-values"></a>Ver valores registados

Depois da conclusão de todas as células no bloco de notas, regresse à página do portal.  

Clique em `View Experiments`.

![ver experimentações](./media/quickstart-get-started/view_exp.png)

Feche o pop-up `Reports`.

Clique em `my-first-experiment`.

Veja as informações sobre a execução que acabou de efetuar.  Desloque para baixo na página, para localizar a tabela de execuções e clique no ligação do número de execuções.

 ![ligação do histórico de execuções](./media/quickstart-get-started/report.png)

Verá os gráficos que foram criados automaticamente dos valores registados:

   ![ver histórico](./media/quickstart-get-started/plots.png)

## <a name="clean-up-resources"></a>Limpar recursos 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Também pode manter o grupo de recursos mas eliminar uma única área de trabalho, ao apresentar as propriedades da área de trabalho e selecionar o botão Eliminar.

## <a name="next-steps"></a>Passos seguintes

Criou os recursos necessários para começar a experimentar e implementar modelos. Também executou algum código num bloco de notas e explorou o histórico de execuções desse código na sua área de trabalho na cloud.

Para obter uma experiência aprofundada de fluxo de trabalho, siga o tutorial do Azure Machine Learning para preparar e implementar um modelo.  

> [!div class="nextstepaction"]
> [Tutorial: Preparar um modelo de classificação de imagens](tutorial-train-models-with-aml.md)

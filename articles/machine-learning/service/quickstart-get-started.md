---
title: 'Início Rápido: Criar uma área de trabalho do serviço de aprendizagem automática no portal do Azure – Azure Machine Learning'
description: Utilize o portal do Azure para criar uma área de trabalho do Azure Machine Learning. Esta área de trabalho é o pilar na cloud que utiliza para experimentar, preparar e implementar modelos de machine learning com o Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: rastala
ms.author: roastala
ms.date: 09/24/2018
ms.openlocfilehash: b81e40298eae0f0b44f37e7f8f16beaddad999a5
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456818"
---
# <a name="quickstart-use-the-azure-portal-to-get-started-with-azure-machine-learning"></a>Início Rápido: Utilizar o portal do Azure para começar a trabalhar com o Azure Machine Learning

Neste início rápido, vai utilizar o portal do Azure para criar uma área de trabalho do Azure Machine Learning. Esta área de trabalho é o pilar na cloud que utiliza para experimentar, preparar e implementar modelos de machine learning com Machine Learning. 

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2F9Ad]

Neste início rápido, irá:

* Criar uma área de trabalho na sua subscrição do Azure.
* Faça um teste com o Python num bloco de notas do Azure e registe os valores em várias iterações.
* Ver os valores registados na área de trabalho.

Os seguintes recursos do Azure são adicionados automaticamente à sua área de trabalho quando estiverem disponíveis regionalmente:

  - [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
  - [Armazenamento do Azure](https://azure.microsoft.com/services/storage/)
  - [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
  - [Cofre de Chaves do Azure](https://azure.microsoft.com/services/key-vault/)

Os recursos que criar podem ser utilizados como pré-requisitos para outros tutoriais e artigos de procedimentos do serviço Machine Learning. Tal como noutros serviços do Azure, existem limites em certos recursos associados com Machine Learning. Um exemplo é o tamanho do cluster do Azure Batch AI. Para obter informações sobre os limites predefinidos e como aumentar a quota, veja [este artigo](how-to-manage-quotas.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


## <a name="create-a-workspace"></a>Criar uma área de trabalho 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

Na página da área de trabalho, selecione `Explore your Azure Machine Learning service workspace`.

 ![Explorar a área de trabalho](./media/quickstart-get-started/explore_aml.png)


## <a name="use-the-workspace"></a>Utilizar a área de trabalho

Veja agora como uma área de trabalho ajuda-o a gerir os seus scripts de aprendizagem automática. Nesta secção, pode:

* Abrir um bloco de notas no Azure Notebooks.
* Executar código que cria alguns valores registados.
* Ver os valores registados na área de trabalho.

Este exemplo mostra como a área de trabalho pode ajudá-lo a manter o controlo das informações geradas num script. 

### <a name="open-a-notebook"></a>Abrir um bloco de notas 

O Azure Notebooks oferece uma plataforma na cloud gratuita para blocos de notas do Jupyter que são pré-configurados com tudo o que precisa para executar o Machine Learning.  

Selecione `Open Azure Notebooks` para realizar a sua primeira experimentação.

 ![Abra o Azure Notebooks](./media/quickstart-get-started/explore_ws.png)

A sua organização poderá precisar de [consentimento de administrador](https://notebooks.azure.com/help/signing-up/work-or-school-account/admin-consent) antes de poder iniciar sessão.

Depois de iniciar sessão, é aberto um novo separador e um aviso `Clone Library` é apresentado. Selecione `Clone`


### <a name="run-the-notebook"></a>Executar o bloco de notas

Juntamente com dois blocos de notas, verá um ficheiro `config.json`. Este ficheiro de configuração contém informações sobre a área de trabalho que criou.  

Selecione `01.run-experiment.ipynb` para abrir o bloco de notas.

Para executar as células uma de cada vez, utilize `Shift`+`Enter`. Ou selecione `Cells` > `Run All` para executar o bloco de notas completo. Quando vir um asterisco [*] junto de uma célula, significa que está em execução. Após o código para essa célula terminar, é apresentado um número.

Pode ser pedido que inicie sessão. Copie o código na mensagem. Em seguida, selecione a ligação e cole o código na nova janela. Certifique-se de que não copia um espaço antes ou depois do código. Inicie sessão com a mesma conta utilizada no portal do Azure.

 ![Iniciar sessão](./media/quickstart-get-started/login.png)

No bloco de notas, a segunda célula lê a partir do `config.json` para ligar à sua área de trabalho.
```
ws = Workspace.from_config()
```

A terceira célula de código começa uma experimentação com o nome "my-first-experiment." Utilize este nome para procurar informações sobre a execução na sua área de trabalho.

```
experiment = Experiment(workspace_object=ws, name = "my-first-experiment")
```

Na última célula do bloco de notas, observe os valores que foram escritos num ficheiro de registo.

```
# Log final results
run.log("Final estimate: ",pi_estimate)
run.log("Final error: ",math.pi-pi_estimate)
```

Pode ver estes valores na sua área de trabalho após a execução do código.

## <a name="view-logged-values"></a>Ver valores registados

Depois da executar todas as células no bloco de notas, regresse à página do portal.  

Selecione `View Experiments`.

![Ver experimentações](./media/quickstart-get-started/view_exp.png)

Feche o pop-up `Reports`.

Selecione `my-first-experiment`.

Veja as informações sobre a execução que acabou de efetuar. Desloque a página para baixo para encontrar a tabela de execuções. Selecione a ligação do número de execução.

 ![Ligação do histórico de execuções](./media/quickstart-get-started/report.png)

Verá os gráficos que foram criados automaticamente dos valores registados.  

   ![Ver histórico](./media/quickstart-get-started/plots.png)

Uma vez que o código para obter uma estimativa do pi utiliza valores aleatórios, os seus gráficos irão mostrar valores diferentes.

## <a name="clean-up-resources"></a>Limpar recursos 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Também pode manter o grupo de recursos mas eliminar uma área de trabalho única. Apresente as propriedades de área de trabalho e selecione **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

Criou os recursos necessários para experimentar e implementar modelos. Também executou algum código num bloco de notas. E explorou o histórico de execuções desse código na sua área de trabalho na cloud.

Para obter uma experiência aprofundada de fluxo de trabalho, siga o tutorial de Machine Learning para preparar e implementar um modelo.  

> [!div class="nextstepaction"]
> [Tutorial: Preparar um modelo de classificação de imagens](tutorial-train-models-with-aml.md)

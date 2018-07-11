---
title: Usar o teste de batch para melhorar as previsões de LUIS | Documentos da Microsoft
titleSuffix: Azure
description: Lote de teste de carga, reveja os resultados e melhorar as previsões de LUIS com alterações.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/06/2018
ms.author: v-geberr
ms.openlocfilehash: 962f33a178048c459e8c6c2948eb17f0e78904ae
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930995"
---
# <a name="improve-app-with-batch-test"></a>Melhorar a aplicação com o teste de batch

Este tutorial demonstra como usar o teste de batch para localizar problemas de predição de expressão.  

Neste tutorial, ficará a saber como:

<!-- green checkmark -->
> [!div class="checklist"]
* Crie um ficheiro de teste do batch 
* Executar um teste de batch
* Rever os resultados de teste
* Corrija os erros para intenções
* Testar novamente o batch

Para este artigo, precisa de uma conta do [LUIS](luis-reference-regions.md#luis-website) gratuita para criar a sua aplicação LUIS.

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de recursos humanos do [rever expressões de ponto final](luis-tutorial-review-endpoint-utterances.md) tutorial, [importar](luis-how-to-start-new-app.md#import-new-app) o JSON para uma nova aplicação no [LUIS](luis-reference-regions.md#luis-website) Web site. A aplicação a importar está no repositório do Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-review-HumanResources.json).

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `batchtest`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 

## <a name="purpose-of-batch-testing"></a>Objetivo do teste de batch de
Teste de batch permite-lhe validar o estado de um modelo com um conjunto conhecido de expressões de teste e rotulado como entidades. No ficheiro batch formatada em JSON, adicionar as expressões e definir as etiquetas de entidade que tem de prever dentro da expressão. 

A estratégia de teste recomendados para LUIS utiliza três conjuntos separados de dados: discursos de exemplo fornecidos para o modelo, expressões de teste do batch e expressões de ponto final. Para este tutorial, certifique-se de que não estiver a utilizar o expressões a partir de qualquer uma das expressões de exemplo (adicionados numa intenção), ou expressões de ponto final. 

Para verificar sua expressões de teste de batch com a expressão de exemplo e expressões de ponto final, [exportar](luis-how-to-start-new-app.md#export-app) a aplicação e [transferir](luis-how-to-start-new-app.md#export-endpoint-logs) o log de consulta. Compare da expressão de exemplo de aplicação e as expressões de log de consulta para as expressões de teste do batch. 

Requisitos do teste de batch:

* expressões de 1000 por teste. 
* Não contém duplicados. 
* Tipos de entidade permitidos: simples e compostos.

## <a name="create-a-batch-file-with-utterances"></a>Criar um arquivo em lotes com expressões com
1. Crie `HumanResources-jobs-batch.json` num editor de texto, como [VSCode](https://code.visualstudio.com/). Ou transfira [o ficheiro](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorial-batch-testing/HumanResources-jobs-batch.json) partir do repositório Github de exemplos do LUIS.

2. No ficheiro batch formatada em JSON, adicionar expressões com o **intenção** desejar prevista no teste. 

    ```JSON
    [
        {
        "text": "Are there any janitorial jobs currently open?",
        "intent": "GetJobInformation",
        "entities": []
        },
        {
        "text": "I would like a fullstack typescript programming with azure job",
        "intent": "GetJobInformation",
        "entities": []
        },
        {
        "text": "Is there a database position open in Los Colinas?",
        "intent": "GetJobInformation",
        "entities": []
        },
        {
        "text": "Can I apply for any database jobs with this resume?",
        "intent": "GetJobInformation",
        "entities": []
        }
    ]
    ```

## <a name="run-the-batch"></a>A executar o batch

1. Selecione **teste** na barra de navegação superior. 

    [ ![Aplicação de captura de ecrã do LUIS com teste realçado na barra de navegação superior, certo](./media/luis-tutorial-batch-testing/hr-first-image.png)](./media/luis-tutorial-batch-testing/hr-first-image.png#lightbox)

2. Selecione **painel de teste do Batch** no painel do lado direito. 

    [ ![Aplicação de captura de ecrã do LUIS com painel de teste de Batch realçado](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png)](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png#lightbox)

3. Selecione **conjunto de dados de importação**.

    [ ![Aplicação de captura de ecrã do LUIS com o conjunto de dados de importação realçado](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png)](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png#lightbox)

4. Escolha a localização de sistema de ficheiros do `HumanResources-jobs-batch.json` ficheiro.

5. Nome do conjunto de dados `intents only` e selecione **feito**.

    ![Selecionar ficheiro](./media/luis-tutorial-batch-testing/hr-import-new-dataset-ddl.png)

6. Selecionar o botão **Executar**. Aguarde até que o teste é concluído.

    [ ![Aplicação de captura de ecrã do LUIS Run realçado](./media/luis-tutorial-batch-testing/hr-run-button.png)](./media/luis-tutorial-batch-testing/hr-run-button.png#lightbox)

7. Selecione **ver resultados**.

8. Reveja os resultados no gráfico e da legenda.

    [ ![Aplicação de captura de ecrã do LUIS com resultados de teste do batch](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png)](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png#lightbox)

## <a name="review-batch-results"></a>Rever os resultados de batch
O gráfico de batch apresenta quatro quadrantes de resultados. À direita do gráfico é um filtro. Por predefinição, o filtro é definido para o primeiro objetivo na lista. O filtro contém todas as intenções e apenas simples, hierárquica (só de principal) e entidades compostas. Quando seleciona uma seção de gráfico ou um ponto no gráfico, exibir o utterance(s) associado abaixo do gráfico. 

Ao passar o rato sobre o gráfico, a roda do rato pode aumentar ou reduzir a apresentar no gráfico. Isto é útil quando há muitos pontos no gráfico rigidamente agrupado. 

O gráfico está em quatro quadrantes, com duas das seções apresentadas a vermelho. **Estas são as secções concentrar-se no**. 

### <a name="applyforjob-test-results"></a>Resultados do teste ApplyForJob
O **ApplyForJob** resultados de teste apresentados no filtro de mostram de que 1 das predições a quatro foi concluída com êxito. Selecione o nome **positivo falso** acima Quadrante de superior direito para ver as expressões abaixo do gráfico. 

![Expressões de teste de batch de LUIS](./media/luis-tutorial-batch-testing/hr-applyforjobs-false-positive-results.png)

As três expressões tinham uma intenção superior **ApplyForJob**. A intenção indicada no ficheiro batch tinha uma pontuação inferior. Por que isso aconteceu? Dos dois objetivos estão intimamente em termos de escolha do word e a disposição do word. Além disso, há quase três vezes como muitos exemplos para **ApplyForJob** que **GetJobInformation**. Este irregularidade de expressões de exemplo avalia **ApplyForJob** favor da intenção. 

Tenha em atenção que ambos os objetivos tem a mesma contagem de erros: 

![Erros de filtro de teste de lote de LUIS](./media/luis-tutorial-batch-testing/hr-intent-error-count.png)

A expressão correspondente o ponto superior no **falso positivo** secção é `Can I apply for any database jobs with this resume?`. A palavra `resume` só foi utilizado na **ApplyForJob**. 

Os outros dois pontos no gráfico tinham muito inferiores pontuações para a intenção de errado, que significa que eles estão mais próximos da intenção correta. 

## <a name="fix-the-app-based-on-batch-results"></a>Corrigir a aplicação com base nos resultados de batch
O objetivo desta seção é fazer com que as três expressões que foram previstas incorretamente para **ApplyForJob** a ser corretamente prevista para **GetJobInformation**, depois da aplicação é resolvida. 

Uma correção rápida aparentemente seria adicionar estas expressões de ficheiro de batch para a intenção correta. Isso é que não o que deseja fazer, no entanto. Pretende que o LUIS para prever corretamente estas expressões sem adicionar-os como exemplos. 

Também deve estar imaginando sobre a remoção de expressões a partir **ApplyForJob** até que a quantidade de expressão é igual a **GetJobInformation**. Isso pode corrigir os resultados do teste, mas seria atrapalham LUIS dessa intenção prever com precisão próxima vez. 

A primeira correção é adicionar expressões com mais **GetJobInformation**. A segunda correção é reduzir o peso da palavra `resume` para o **ApplyForJob** intenção. 

### <a name="add-more-utterances-to-getjobinformation"></a>Adicionar expressões com mais **GetJobInformation**
1. Fechar o painel de teste do batch, selecionando o **testar** botão no painel de navegação superior. 

    [ ![Captura de ecrã do LUIS, com o botão de teste realçado](./media/luis-tutorial-batch-testing/hr-close-test-panel.png)](./media/luis-tutorial-batch-testing/hr-close-test-panel.png#lightbox)

2. Selecione **GetJobInformation** na lista de objetivos. 

    [ ![Captura de ecrã do LUIS, com o botão de teste realçado](./media/luis-tutorial-batch-testing/hr-select-intent-to-fix-1.png)](./media/luis-tutorial-batch-testing/hr-select-intent-to-fix-1.png#lightbox)

3. Adicionar mais expressões que são diversificados de comprimento, a escolha do word e a disposição do word, certificar-se de que incluem os termos `resume` e `c.v.`:

    ```JSON
    Is there a new job in the warehouse for a stocker?
    Where are the roofing jobs today?
    I heard there was a medical coding job that requires a resume.
    I would like a job helping college kids write their c.v.s. 
    Here is my resume, looking for a new post at the community college using computers.
    What positions are available in child and home care?
    Is there an intern desk at the newspaper?
    My C.v. shows I'm good at analyzing procurement, budgets, and lost money. Is there anything for this type of work?
    Where are the earth drilling jobs right now?
    I've worked 8 years as an EMS driver. Any new jobs?
    New food handling jobs?
    How many new yard work jobs are available?
    Is there a new HR post for labor relations and negotiations?
    I have a masters in library and archive management. Any new positions?
    Are there any babysitting jobs for 13 year olds in the city today?
    ```

4. Preparar a aplicação, selecionando **Train** na barra de navegação direita superior.

## <a name="verify-the-fix-worked"></a>Certifique-se de que a correção trabalhou
Para verificar que as expressões no teste de batch estão previstas corretamente, execute novamente o teste de batch.

1. Selecione **teste** na barra de navegação superior. Se os resultados do batch são ainda abertos, selecione **voltar à lista**.  

2. Selecione as reticências (***...*** ) à direita do nome do batch e selecione **executar o conjunto de dados**. Aguarde até que o teste de batch é concluído. Tenha em atenção que o **ver resultados** botão agora está verde. Isso significa que o lote inteiro foi executado com êxito.

3. Selecione **ver resultados**. Os objetivos devem todos ter ícones verde à esquerda dos nomes de intenção. 

    [ ![Captura de ecrã do LUIS, com o botão de resultados de batch realçado](./media/luis-tutorial-batch-testing/hr-batch-test-intents-no-errors.png)](./media/luis-tutorial-batch-testing/hr-batch-test-intents-no-errors.png#lightbox)


## <a name="what-has-this-tutorial-accomplished"></a>O que conseguiu neste tutorial?
Essa precisão de previsão de aplicação aumentou, encontrando erros no lote e corrigir o modelo ao adicionar mais expressões de exemplo para a intenção correta e o treinamento. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Selecione **as minhas aplicações** no menu à esquerda superior. Selecione as reticências **...**  à direita do nome da aplicação na lista de aplicações, selecione **eliminar**. Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.


## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre os padrões de](luis-tutorial-pattern.md)


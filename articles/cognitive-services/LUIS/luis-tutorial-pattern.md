---
title: Tutorial sobre como utilizar padrões para melhorar as previsões de LUIS – Azure | Documentos da Microsoft
titleSuffix: Azure
description: Neste tutorial, utilize o padrão para objetivos para melhorar as previsões de intenção e entidade do LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 9793b98c384346dc0de68061d42b4bcb3c513ed4
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866207"
---
# <a name="tutorial-use-patterns-to-improve-predictions"></a>Tutorial: Utilizar padrões para melhorar as previsões de indisponibilidade

Neste tutorial, utilize padrões para aumentar a predição de intenção e entidade.  

> [!div class="checklist"]
* Como identificar que um padrão ajudaria a sua aplicação
* Como criar um padrão 
* Como utilizar entidades pré-criados e personalizadas num padrão 
* Como verificar as melhorias de predição do padrão
* Como adicionar uma função a uma entidade para encontrar contextualmente baseada em entidades
* Como adicionar um Pattern.any para encontrar as entidades de forma livre

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="import-humanresources-app"></a>Aplicação de RecursosHumanos de importação
Este tutorial importa uma aplicação de RecursosHumanos. A aplicação tem três objetivos: nenhum, GetEmployeeOrgChart, GetEmployeeBenefits. A aplicação tem duas entidades: número pré-criados e funcionários. A entidade Employee é uma entidade para extrair o nome de um funcionário. 

1. Crie um novo ficheiro de aplicação do LUIS e nomeie- `HumanResources.json`. 

2. Copie a seguinte definição de aplicação para o ficheiro:

   [!code-json[Add the LUIS model](~/samples-luis/documentation-samples/tutorial-patterns/HumanResources.json?range=1-164 "Add the LUIS model")]

3. Sobre o LUIS **aplicações** página, selecione **importar a nova aplicação**. 

4. Na **importar a nova aplicação** caixa de diálogo, selecione o `HumanResources.json` ficheiro que criou no passo 1.

5. Selecione o **GetEmployeeOrgChart** intenção, em seguida, a alteração da **vista de entidades** para **Tokens vista**. São listadas várias expressões de exemplo. Cada expressão contém um nome, o que é uma entidade Employee. Tenha em atenção que cada nome é diferente e que a organização do texto é diferente para cada ocorrência de pronunciação. Essa diversidade ajuda LUIS saiba uma vasta gama de expressões.

    ![Página de captura de ecrã da intenção com a vista de entidades ativado/desativada](media/luis-tutorial-pattern/utterances-token-view.png)

6. Selecione **treinar** na barra de navegação superior para preparar a aplicação. Aguarde que a barra de êxito verde.

7. Selecione **teste** no painel superior. Introduza `Who does Patti Owens report to?` , em seguida, selecione introduzir. Selecione **Inspect** sob a expressão para obter mais informações sobre o teste.
    
    O nome do funcionário, Patti Owens, não foi utilizado numa expressão de exemplo ainda. Este é um teste para ver quão bem LUIS aprendidas esta expressão se destina a `GetEmployeeOrgChart` devem ser a intenção e a entidade Employee `Patti Owens`. O resultado deve ser inferior a 50% (. 50) para o `GetEmployeeOrgChart` intenção. Embora a intenção é correta, a pontuação é baixa. A entidade Employee também corretamente é identificada como `Patti Owens`. Padrões de aumentam esta pontuação de predição inicial. 

    ![Painel de captura de ecrã de teste](media/luis-tutorial-pattern/original-test.png)

8. Fechar o painel de teste, selecionando o **testar** botão no painel de navegação superior. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Padrões de ensinam LUIS expressões comuns com menos de exemplos
Devido à natureza do domínio de recursos humanos, existem algumas formas comuns de perguntar sobre relações de funcionário em organizações. Por exemplo:

```
Who does Mike Jones report to?
Who reports to Mike Jones? 
```

Estas expressões são demasiado fechar para determinar a exclusividade contextual de cada sem fornecer muitos exemplos de expressão. Ao adicionar um padrão para um objetivo, o LUIS aprende os padrões de expressão comuns para um objetivo sem fornecer muitos exemplos de expressão. 

Expressão de modelo de exemplo incluem intenção:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

O padrão é uma combinação de correspondência de expressões regulares e de machine learning. Em seguida, fornece algum modelo exemplos de expressão para o LUIS saber o padrão. Estes exemplos, juntamente com as expressões de intenção, dar LUIS uma melhor compreensão de quais expressões com ajustar a intenção e onde, dentro da expressão, a entidade existe. <!--A pattern is specific to an intent. You can't duplicate the same pattern on another intent. That would confuse LUIS, which lowers the prediction score. -->

## <a name="add-the-template-utterances"></a>Adicionar as expressões de modelo

1. No painel de navegação esquerdo, sob **melhorar o desempenho da aplicação**, selecione **padrões** no painel de navegação esquerda.

2. Selecione o **GetEmployeeOrgChart** intenção, em seguida, introduza as seguintes expressões de modelo, um de cada vez, selecione introduzir após cada ocorrência de pronunciação de modelo:

    ```
    Does {Employee} have {number} subordinates?
    Does {Employee} have {number} direct reports?
    Who does {Employee} report to?
    Who reports to {Employee}?
    Who is {Employee}'s manager?
    Who are {Employee}'s subordinates?
    ```

    O `{Employee}` sintaxe marca a localização de entidade dentro da expressão de modelo como bem como a entidade é. 

    ![Captura de ecrã da introdução de expressões de modelo de intenção](./media/luis-tutorial-pattern/enter-pattern.png)

3. Selecione **Train** na barra de navegação superior. Aguarde que a barra de êxito verde.

4. Selecione **teste** no painel superior. Introduza `Who does Patti Owens report to?` na caixa de texto. Selecione introduzir. Esta é a mesma expressão testado na secção anterior. O resultado deve ser superior para o `GetEmployeeOrgChart` intenção. 

    A classificação agora é muito melhor. LUIS aprendeu o padrão relevante para a intenção sem fornecer muitos exemplos.

    ![Resultado do painel de captura de ecrã de teste com score elevada](./media/luis-tutorial-pattern/high-score.png)

    A entidade é encontrada em primeiro lugar, em seguida, o padrão for encontrado, o que indica a intenção. Se tiver um resultado de teste onde a entidade não é detetada e, portanto, o padrão não for encontrado, terá de adicionar mais de exemplo de expressões na intenção (não o padrão). 

5. Fechar o painel de teste, selecionando o **testar** botão no painel de navegação superior.

## <a name="use-an-entity-with-a-role-in-a-pattern"></a>Utilizar uma entidade com uma função num padrão
A aplicação do LUIS é utilizada para o ajudar a mover os funcionários de uma localização para outra. Uma expressão de exemplo é `Move Bob Jones from Seattle to Los Colinas`. Cada local em que a expressão tem um significado diferente. Seattle é a localização de origem e Colinas de Los é a localização de destino para a mudança. Para diferenciar nessas localizações no padrão, nas seções a seguir é criar uma entidade para a localização com duas funções: origem e de destino. 

### <a name="create-a-new-intent-for-moving-people-and-assets"></a>Criar uma intenção de novo para mover as pessoas e ativos
Crie uma intenção de novo para qualquer expressões com que está prestes a mover recursos ou pessoas.

1. Selecione **intenções** no painel de navegação à esquerda
2. Selecione **criar intenção de novo**
3. Nome a intenção de novo `MoveAssetsOrPeople`
4. Adicione expressões de exemplo:

    ```
    Move Bob Jones from Seattle to Los Colinas
    Move Dave Cooper from Redmond to Seattle
    Move Jim Smith from Toronto to Vancouver
    Move Jill Benson from Boston to London
    Move Travis Hinton from Portland to Orlando
    ```
    ![Captura de ecrã da expressão de exemplo para MoveAssetsOrPeople intenção](./media/luis-tutorial-pattern/intent-moveasserts-example-utt.png)

    As expressões de exemplo o objetivo é dar suficiente exemplos. Se, mais tarde no teste, a entidade de localização não é detetada e, consequentemente, o padrão não é detetado, regresse a este passo e adicionar mais exemplos. Em seguida, dar formação e teste novamente. 

5. Marca as entidades nas expressões de exemplo com a entidade Employee ao selecionar o nome próprio, em seguida, o apelido numa expressão, em seguida, selecionando a entidade Employee na lista.

    ![Captura de ecrã de expressões no MoveAssetsOrPeople marcados com a entidade Employee](./media/luis-tutorial-pattern/intent-moveasserts-employee.png)

6. Selecione o texto `portland` em que a expressão `move travis hinton from portland to orlando`. Na caixa de diálogo pop-up, introduza o novo nome de entidade `Location`e selecione **criar nova entidade**. Escolha o **simples** tipo de entidade e selecione **feito**.

    ![Captura de ecrã da criação de nova entidade de localização](./media/luis-tutorial-pattern/create-new-location-entity.png)

    Marca o resto dos nomes de localização em que as expressões. 

    ![Captura de ecrã de todas as entidades marcadas](./media/luis-tutorial-pattern/moveasset-all-entities-labeled.png)

    O padrão de escolha do word e a ordem é óbvio na imagem anterior. Se fosse **não** usando padrões, e as expressões na intenção tem um padrão de óbvio, que é uma boa indicação que deve usar padrões. 

    Se esperar uma grande variedade de expressões, em vez de um padrão, estes seriam as expressões de exemplo errado. Nesse caso, desejaria expressões com todos os vários no termo ou escolha do word, comprimento da expressão e posicionamento de entidade. 

<!--TBD: what guidance to move from hier entities to patterns with roles -->
<!--    The [Hierarchical entity quickstart](luis-quickstart-intent-and-hier-entity.md) uses the  same idea of location but uses child entities to find origin and destination locations. 
-->
### <a name="add-role-to-location-entity"></a>Adicionar função a entidade de localização 
Funções só podem ser utilizadas para padrões. Adicione as funções de origem e destino para a entidade de localização. 

1. Selecione **entidades** no painel de navegação esquerdo, em seguida, **localização** na lista de entidades.

2. Adicione `Origin` e `Destination` funções para a entidade.

    ![Captura de ecrã da nova entidade com as funções](./media/luis-tutorial-pattern/location-entity.png)

    As funções não são marcadas na página de intenção MoveAssetsOrPeople porque funções não existem em expressões de intenção. Eles apenas existem em expressões de modelo padrão. 

### <a name="add-template-utterances-that-uses-location-and-destination-roles"></a>Adicionar expressões de modelo que utiliza funções de localização e de destino
Adicione expressões de modelo que utilizam a nova entidade.

1. Selecione **padrões** no painel de navegação esquerda.

2. Selecione o **MoveAssetsOrPeople** intenção.

3. Introduza uma expressão de modelo de novo utilizando a nova entidade `Move {Employee} from {Location:Origin} to {Location:Destination}`. A sintaxe para uma entidade e a função dentro de uma expressão de modelo é `{entity:role}`.

    ![Captura de ecrã da nova entidade com as funções](./media/luis-tutorial-pattern/pattern-moveassets.png)

4. Prepare a aplicação para o novo objetivo, a entidade e o padrão.

### <a name="test-the-new-pattern-for-role-data-extraction"></a>Testar o novo padrão para extração de dados de função
Valide o novo padrão com um teste.

1. Selecione **teste** no painel superior. 
2. Introduza a expressão `Move Tammi Carlson from Bellingham to Winthrop`.
3. Selecione **Inspect** sob o resultado para ver os resultados de teste para a entidade e propósito.

    ![Captura de ecrã da nova entidade com as funções](./media/luis-tutorial-pattern/test-with-roles.png)

    As entidades são encontradas em primeiro lugar, em seguida, o padrão for encontrado, o que indica a intenção. Se tiver um resultado de teste em que as entidades não são detetadas e, portanto, o padrão não for encontrado, terá de adicionar mais de exemplo de expressões na intenção (não o padrão). 

4. Fechar o painel de teste, selecionando o **testar** botão no painel de navegação superior.

## <a name="use-a-patternany-entity-to-find-free-form-entities-in-a-pattern"></a>Utilizar uma entidade de Pattern.any para localizar as entidades de forma livre num padrão
Esta aplicação RecursosHumanos também ajuda os funcionários a encontrar formulários de empresa. Muitos dos formulários tem títulos são variados de comprimento. O comprimento variado inclui expressões que podem confundir o LUIS sobre onde termina o nome do formulário. Utilizar um **Pattern.any** entidade num padrão permite-lhe especificar o início e fim do nome do formulário, para que o LUIS corretamente extrai o nome do formulário. 

### <a name="create-a-new-intent-for-the-form"></a>Criar uma intenção de novo para o formulário
Crie uma intenção de novo para expressões que estiver à procura de formulários.

1. Selecione **intenções** no painel de navegação esquerda.

2. Selecione **Criar nova intenção**.

3. Nomeie o novo objetivo `FindForm`.

4. Adicione uma expressão de exemplo.

    ```
    `Where is the form What to do when a fire breaks out in the Lab and who needs to sign it after I read it?`
    ```

    ![Captura de ecrã da nova entidade com as funções](./media/luis-tutorial-pattern/intent-findform.png)

    O título do formulário é `What to do when a fire breaks out in the Lab`. A expressão é pedir para a localização do formulário e também está perguntando quem precisa de iniciar sessão-lo a validar o funcionário lê-lo. Sem uma entidade de Pattern.any, seria difícil de entender onde o título do formulário termina e extrair o título do formulário como uma entidade da expressão.

### <a name="create-a-patternany-entity-for-the-form-title"></a>Criar uma entidade de Pattern.any para o título do formulário
Permite que a entidade de Pattern.any para entidades de comprimento variável. Ela só funciona num padrão porque o padrão de marca de início e no fim da entidade. Se achar que seu padrão, quando ele inclui um Pattern.any, incorretamente, utilizam entidades extrai uma [lista explícita](luis-concept-patterns.md#explicit-lists) para corrigir este problema. 

1. Selecione **entidades** na navegação à esquerda.

2. Selecione **Criar nova entidade**. 

3. Nome da entidade `FormName` com o tipo **Pattern.any**. Para este tutorial específico, não é necessário adicionar quaisquer funções à entidade.

    ![Imagem da caixa de diálogo para o nome da entidade e tipo de entidade](./media/luis-tutorial-pattern/create-entity-pattern-any.png)

### <a name="add-a-pattern-that-uses-the-patternany"></a>Adicionar um padrão que utiliza o Pattern.any

1. Selecione **padrões** no painel de navegação esquerda.

2. Selecione o **FindForm** intenção.

3. Introduza uma expressão de modelo com a nova entidade `Where is the form {FormName} and who needs to sign it after I read it?`

    ![Captura de ecrã da expressão de modelo usando pattern.any entidade](./media/luis-tutorial-pattern/pattern.any-template-utterance.png)

4. Prepare a aplicação para o novo objetivo, a entidade e o padrão.

### <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Testar o novo padrão para extração de dados de forma livre
1. Selecione **testar** a partir da barra superior para abrir o painel de teste. 

2. Introduza a expressão `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`.

3. Selecione **Inspect** sob o resultado para ver os resultados de teste para a entidade e propósito.

    ![Captura de ecrã da expressão de modelo usando pattern.any entidade](./media/luis-tutorial-pattern/test-pattern.any-results.png)

    A entidade é encontrada em primeiro lugar, em seguida, o padrão for encontrado, o que indica a intenção. Se tiver um resultado de teste em que as entidades não são detetadas e, portanto, o padrão não for encontrado, terá de adicionar mais de exemplo de expressões na intenção (não o padrão).

4. Fechar o painel de teste, selecionando o **testar** botão no painel de navegação superior.

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Para tal, selecione as reticências (***...*** ) à direita do nome da aplicação na lista de aplicações, selecione **eliminar**. Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Utilize a lista de frase para melhorar a previsão](luis-tutorial-interchangeable-phrase-list.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
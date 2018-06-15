---
title: Tutorial com padrões para melhorar as predições LUIS - Azure | Microsoft Docs
titleSuffix: Azure
description: Neste tutorial, utilize padrão para pendentes para melhorar predições LUIS de tentativa e de entidade.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: d2d680a1e2749c6e0a5b17876915c5c0d86eb420
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356088"
---
# <a name="tutorial-use-patterns-to-improve-predictions"></a>Tutorial: Utilizar padrões para melhorar as predições

Neste tutorial, utilize padrões para aumentar a predição de tentativa e de entidade.  

> [!div class="checklist"]
* Como identificar que um padrão iria ajudar a sua aplicação
* Como criar um padrão 
* Como utilizar entidades prebuilt e personalizadas num padrão 
* Como verificar melhoramentos de predição de padrão
* Como adicionar uma função a uma entidade para localizar o reconhecimento do contexto baseada em entidades
* Como adicionar um Pattern.any encontrar entidades de forma livre

Para este artigo, precisa de um livre [LUIS] [ LUIS] conta para criar a sua aplicação LUIS.

## <a name="import-humanresources-app"></a>Importar RecursosHumanos aplicação
Este tutorial importa uma aplicação RecursosHumanos. A aplicação tem três pendentes: None, GetEmployeeOrgChart, GetEmployeeBenefits. A aplicação tiver duas entidades: Prebuilt número e dos empregados. A entidade de empregado é uma entidade simple para extrair o nome de um funcionário. 

1. Criar um novo ficheiro de aplicação LUIS e dê-lhe nome `HumanResources.json`. 

2. Copie a seguinte definição de aplicação para o ficheiro:

   [!code-json[Add the LUIS model](~/samples-luis/documentation-samples/tutorial-patterns/HumanResources.json?range=1-164 "Add the LUIS model")]

3. No LUIS **aplicações** página, selecione **importação nova aplicação**. 

4. No **importação nova aplicação** caixa de diálogo, selecione o `HumanResources.json` ficheiro que criou no passo 1.

5. Selecione o **GetEmployeeOrgChart** intenção, em seguida, a alteração do **vista entidades** para **Tokens vista**. Vários utterances de exemplo são apresentados. Cada utterance contém um nome, que é uma entidade do empregado. Tenha em atenção que cada nome é diferente e que é diferente para cada utterance a disposição das palavras utilizadas. Este diversity ajuda LUIS saiba uma vasta gama de utterances.

    ![Página de captura de ecrã da intenção com vista entidades tivada/desativada](media/luis-tutorial-pattern/utterances-token-view.png)

6. Selecione **preparar** na barra de navegação superior para preparar a aplicação. Aguarde que a barra de êxito verde.

7. Selecione **teste** no painel superior. Introduza `Who does Patti Owens report to?` , em seguida, selecione introduzir. Selecione **inspecionar** em utterance para obter mais informações sobre o teste.
    
    O nome de empregado, Patti Owens, não foi utilizado no ainda um utterance de exemplo. Este é um teste para ver quão bem LUIS aprendeu neste utterance destina-se a `GetEmployeeOrgChart` intenção e a entidade de empregado devem ser `Patti Owens`. O resultado deve ser inferior a 50% (. 50) para o `GetEmployeeOrgChart` intenção. Embora a intenção é correta, a classificação é baixa. A entidade de empregado também corretamente é identificada como `Patti Owens`. Padrões de aumentam este modelo de pontuação de predição inicial. 

    ![Painel de captura de ecrã do teste](media/luis-tutorial-pattern/original-test.png)

8. Feche o painel de teste, selecionando o **testar** botão no painel de navegação superior. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Padrões ensinam LUIS utterances comuns com exemplos menos
Devido à natureza do domínio de recursos humanos, existem algumas formas comuns de perguntar sobre relações de empregado em organizações. Por exemplo:

```
Who does Mike Jones report to?
Who reports to Mike Jones? 
```

Estes utterances são demasiado fechar para determinar a nível contextual das exclusividade de cada sem fornecer exemplos de utterance muitos. Ao adicionar um padrão para um objetivo, LUIS aprende comuns utterance padrões de objetivo sem fornecer exemplos de utterance muitos. 

Utterances de modelo de exemplo intenção incluem:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

O padrão é uma combinação de correspondência de expressões regulares e de aprendizagem. Em seguida, forneça algumas modelo exemplos utterance LUIS saber o padrão. Estes exemplos, juntamente com utterances intenção, dê LUIS melhor compreender de que utterances ajustar a intenção e onde, dentro de utterance, a entidade existe. <!--A pattern is specific to an intent. You can't duplicate the same pattern on another intent. That would confuse LUIS, which lowers the prediction score. -->

## <a name="add-the-template-utterances"></a>Adicionar os utterances de modelo

1. No painel de navegação esquerdo, em **melhorar o desempenho da aplicação**, selecione **padrões** do painel de navegação esquerdo.

2. Selecione o **GetEmployeeOrgChart** intenção, em seguida, introduza os seguinte utterances de modelo, um de cada vez, selecionar introduza após cada utterance do modelo:

    ```
    Does {Employee} have {number} subordinates?
    Does {Employee} have {number} direct reports?
    Who does {Employee} report to?
    Who reports to {Employee}?
    Who is {Employee}'s manager?
    Who are {Employee}'s subordinates?
    ```

    O `{Employee}` sintaxe marca a localização de entidade dentro de utterance de modelo como bem como a que entidade é. 

    ![Captura de ecrã da introdução utterances de modelo de objetivo](./media/luis-tutorial-pattern/enter-pattern.png)

3. Selecione **formação** na barra de navegação superior. Aguarde que a barra de êxito verde.

4. Selecione **teste** no painel superior. Introduza `Who does Patti Owens report to?` na caixa de texto. Selecione introduzir. Este é o mesmo utterance testado na secção anterior. O resultado deve ser superior para o `GetEmployeeOrgChart` intenção. 

    A classificação agora é muito mais. LUIS aprendeu o padrão relevante para o objetivo sem fornecer muitos exemplos.

    ![Painel de captura de ecrã de teste com elevada pontuação resultar](./media/luis-tutorial-pattern/high-score.png)

    A entidade encontra-se em primeiro lugar, em seguida, o padrão for encontrado, o que indica a intenção. Se tiver um resultado de teste em que a entidade não é detetada, e, por conseguinte, não foi encontrado o padrão, terá de adicionar mais utterances de exemplo na intenção (não o padrão). 

5. Feche o painel de teste, selecionando o **testar** botão no painel de navegação superior.

## <a name="use-an-entity-with-a-role-in-a-pattern"></a>Utilizar uma entidade com uma função num padrão
A aplicação de LUIS é utilizada para ajudar a mover os funcionários de uma localização para outra. Utterance um exemplo é `Move Bob Jones from Seattle to Los Colinas`. Cada localização no utterance tem um significado diferente. Seattle é a localização de origem e Los Colinas é a localização de destino para a movimentação. Para poder diferenciar dessas localizações padrão, nas seguintes secções criar uma entidade simple para a localização com duas funções: origem e de destino. 

### <a name="create-a-new-intent-for-moving-people-and-assets"></a>Criar um novo objetivo para mover as pessoas e os recursos
Crie um novo objetivo para qualquer utterances que está prestes a mover pessoas ou ativos.

1. Selecione **pendentes** de navegação esquerdo
2. Selecione **criar novo objetivo**
3. Nome a intenção de novo `MoveAssetsOrPeople`
4. Adicione utterances de exemplo:

    ```
    Move Bob Jones from Seattle to Los Colinas
    Move Dave Cooper from Redmond to Seattle
    Move Jim Smith from Toronto to Vancouver
    Move Jill Benson from Boston to London
    Move Travis Hinton from Portland to Orlando
    ```
    ![Captura de ecrã do utterance de exemplo para MoveAssetsOrPeople intenção](./media/luis-tutorial-pattern/intent-moveasserts-example-utt.png)

    O objetivo de utterances de exemplo é atribuir suficiente exemplos. Se, posteriormente, no teste, a entidade de localização não é detetada e, consequentemente, o padrão não é detetado, volte a este passo e adicionar mais exemplos. Em seguida, dar formação e teste novamente. 

5. Marque as entidades na utterances de exemplo com a entidade de empregado ao selecionar o nome próprio, em seguida, o apelido num utterance, em seguida, selecionar a entidade do empregado na lista.

    ![Captura de ecrã do utterances no MoveAssetsOrPeople marcado com a entidade de empregado](./media/luis-tutorial-pattern/intent-moveasserts-employee.png)

6. Selecione o texto `portland` no utterance `move travis hinton from portland to orlando`. Na caixa de diálogo de pop-up, introduza o novo nome de entidade `Location`e selecione **criar nova entidade**. Escolha o **simples** tipo de entidade e selecione **feito**.

    ![Captura de ecrã da criação de nova entidade de localização](./media/luis-tutorial-pattern/create-new-location-entity.png)

    Marque o resto dos nomes de localização nos utterances. 

    ![Captura de ecrã de todas as entidades marcada](./media/luis-tutorial-pattern/moveasset-all-entities-labeled.png)

    O padrão de opção do word e a ordem é óbvios na imagem anterior. Se pretendesse **não** utilizar padrões, e utterances na intenção têm um padrão óbvios, que é um bom indicador de que deve utilizar padrões. 

    Se espera que uma grande variedade de utterances, em vez de um padrão, estes seriam utterances o exemplo errado. Nesse caso, seria aconselhável utterances amplamente variando no prazo ou escolha word, comprimento utterance e posicionamento de entidade. 

<!--TBD: what guidance to move from hier entities to patterns with roles -->
<!--    The [Hierarchical entity quickstart](luis-quickstart-intent-and-hier-entity.md) uses the  same idea of location but uses child entities to find origin and destination locations. 
-->
### <a name="add-role-to-location-entity"></a>Adicionar a função a entidade de localização 
Funções só podem ser utilizadas para padrões. Adicione as funções de origem e destino para a entidade de localização. 

1. Selecione **entidades** no painel de navegação esquerdo, em seguida, **localização** da lista de entidades.

2. Adicionar `Origin` e `Destination` funções para a entidade.

    ![Captura de ecrã da nova entidade com funções](./media/luis-tutorial-pattern/location-entity.png)

    As funções não estão marcadas na página intenção MoveAssetsOrPeople porque as funções não existir no utterances intenção. Só existem no utterances de modelo de padrão. 

### <a name="add-template-utterances-that-uses-location-and-destination-roles"></a>Adicionar utterances de modelo que utiliza as funções de localização e destino
Adicione utterances de modelo que utilizam a nova entidade.

1. Selecione **padrões** do painel de navegação esquerdo.

2. Selecione o **MoveAssetsOrPeople** intenção.

3. Introduza um novo utterance de modelo utilizando a nova entidade `Move {Employee} from {Location:Origin} to {Location:Destination}`. A sintaxe para uma entidade e a função dentro de um utterance de modelo é `{entity:role}`.

    ![Captura de ecrã da nova entidade com funções](./media/luis-tutorial-pattern/pattern-moveassets.png)

4. Preparar a aplicação para o novo objetivo, a entidade e o padrão.

### <a name="test-the-new-pattern-for-role-data-extraction"></a>Testar o novo padrão para extração de dados de função
Valide o novo padrão com um teste.

1. Selecione **teste** no painel superior. 
2. Introduza o utterance `Move Tammi Carlson from Bellingham to Winthrop`.
3. Selecione **inspecionar** sob o resultado para ver os resultados do teste de entidade e intenção.

    ![Captura de ecrã da nova entidade com funções](./media/luis-tutorial-pattern/test-with-roles.png)

    As entidades encontram-se em primeiro lugar, em seguida, o padrão for encontrado, o que indica a intenção. Se tiver um resultado de teste em que as entidades não são detetadas e, por conseguinte, não foi encontrado o padrão, terá de adicionar mais utterances de exemplo na intenção (não o padrão). 

4. Feche o painel de teste, selecionando o **testar** botão no painel de navegação superior.

## <a name="use-a-patternany-entity-to-find-free-form-entities-in-a-pattern"></a>Utilize uma entidade Pattern.any para localizar as entidades de forma livre num padrão
Esta aplicação RecursosHumanos também ajuda os empregados encontrar formulários de empresa. Muitas dos formulários tem títulos são vários de comprimento. O comprimento variando inclui expressões que podem confunda LUIS sobre onde termina o nome de formato. Utilizar um **Pattern.any** entidade num padrão permite-lhe especificar o início e fim do nome do formulário para LUIS extrai corretamente o nome de formato. 

### <a name="create-a-new-intent-for-the-form"></a>Criar um novo objetivo do formulário
Crie um novo objetivo para utterances estiver à procura de formulários.

1. Selecione **pendentes** de navegação esquerdo.

2. Selecione **criar novo objetivo**.

3. Nome a intenção novo `FindForm`.

4. Adicione um utterance de exemplo.

    ```
    `Where is the form What to do when a fire breaks out in the Lab and who needs to sign it after I read it?`
    ```

    ![Captura de ecrã da nova entidade com funções](./media/luis-tutorial-pattern/intent-findform.png)

    O título do formulário é `What to do when a fire breaks out in the Lab`. O utterance é solicitando a localização do formulário e também está a pedir quem deverá assiná-lo a validar o empregado lê-lo. Sem uma entidade Pattern.any, seria possível complicado perceber em que o título do formulário termina e extrair o título do formulário como uma entidade do utterance.

### <a name="create-a-patternany-entity-for-the-form-title"></a>Criar uma entidade Pattern.any para o título do formulário
A entidade de Pattern.any permite entidades do comprimento diferentes. Só funciona num padrão porque o padrão de marca o início e de fim da entidade. Se achar que seu padrão de, quando inclui uma Pattern.any, extrai entidades incorretamente, utilizam um [lista explícita](luis-concept-patterns.md#explicit-lists) para corrigir este problema. 

1. Selecione **entidades** no painel de navegação esquerdo.

2. Selecione **criar nova entidade**. 

3. Nome da entidade `FormName` com tipo **Pattern.any**. Para este tutorial específico, não terá de adicionar quaisquer funções para a entidade.

    ![Imagem da caixa de diálogo para o nome de entidade e tipo de entidade](./media/luis-tutorial-pattern/create-entity-pattern-any.png)

### <a name="add-a-pattern-that-uses-the-patternany"></a>Adicionar um padrão que utiliza o Pattern.any

1. Selecione **padrões** do painel de navegação esquerdo.

2. Selecione o **FindForm** intenção.

3. Introduza um utterance modelo utilizando a nova entidade `Where is the form {FormName} and who needs to sign it after I read it?`

    ![Captura de ecrã do utterance modelo utilizando pattern.any entidade](./media/luis-tutorial-pattern/pattern.any-template-utterance.png)

4. Preparar a aplicação para o novo objetivo, a entidade e o padrão.

### <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Testar o novo padrão para extração de dados do formulário livre
1. Selecione **testar** da barra superior para abrir o painel de teste. 

2. Introduza o utterance `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`.

3. Selecione **inspecionar** sob o resultado para ver os resultados do teste de entidade e intenção.

    ![Captura de ecrã do utterance modelo utilizando pattern.any entidade](./media/luis-tutorial-pattern/test-pattern.any-results.png)

    A entidade encontra-se em primeiro lugar, em seguida, o padrão for encontrado, o que indica a intenção. Se tiver um resultado de teste em que as entidades não são detetadas e, por conseguinte, não foi encontrado o padrão, terá de adicionar mais utterances de exemplo na intenção (não o padrão).

4. Feche o painel de teste, selecionando o **testar** botão no painel de navegação superior.

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não é necessário elimine a aplicação de LUIS. Para tal, selecione o menu de três ponto (. …) para a direita do nome da aplicação na lista de aplicações, selecione **eliminar**. Na caixa de diálogo de pop-up **eliminar aplicação?**, selecione **Ok**.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Utilize a lista de expressão para melhorar a previsão](luis-tutorial-interchangeable-phrase-list.md)

[LUIS]: luis-reference-regions.md
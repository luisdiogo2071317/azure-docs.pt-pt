---
title: Combinar colunas por transformação de exemplo com o Azure Machine Learning Workbench
description: O documento de referência para a transformação de "Combinar colunas por exemplo"
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 621601ad3576aad13f2f71062ee2351cf1a394c8
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35648779"
---
# <a name="combine-columns-by-example-transformation"></a>Combinar colunas por transformação de exemplo
Essa transformação permite ao utilizador adicionar uma nova coluna ao combinar valores de várias colunas. Utilizador pode especificar um separador ou fornecer exemplos de valores combinados para realizar esta transformação. Quando o usuário fornece exemplos de combinação, a transformação é processada pelo mesmo **por exemplo** motor que é utilizada na **derivar coluna por exemplo** transformar.

## <a name="how-to-perform-this-transformation"></a>Como realizar esta transformação

Para executar essa transformação, siga estes passos:
1. Selecione duas ou mais colunas que pretende combinar numa coluna. 
2. Selecione **combinar colunas por exemplo** partir do **transforma** menu. Em alternativa, clique com o botão direito do rato no cabeçalho de qualquer uma das colunas selecionadas e selecione **combinar colunas por meio de exemplos** no menu de contexto. Abre o Editor de transformar e uma nova coluna é adicionada ao lado da coluna da direita mais selecionada. A nova coluna contém os valores combinados, separados por um separador de predefinição. Colunas selecionadas podem ser identificadas pelas caixas de verificação em cabeçalhos de coluna. Adição e remoção de colunas da seleção podem ser feitos usando as caixas de verificação.
3. Pode atualizar o valor combinado na coluna criada recentemente. O valor atualizado é utilizado como um exemplo para saber a transformação.
4. Clique em **OK** para aceitar a transformação.

### <a name="transform-editor-advanced-mode"></a>Transformar editor: modo avançado

Modo avançado fornece uma experiência mais rica para combinar as colunas. 

Selecionando **separador** sob **combinar colunas por** permite aos usuários especificar cadeias de caracteres no **separador** caixa de texto. Separador fora do **separador** caixa de texto para pré-visualizar os resultados em gird os dados. Prima **OK** para consolidar a transformação.

Selecionando **exemplos** sob **combinar colunas por** permite aos usuários fornecer exemplos de valores combinados. Para promover uma linha, por exemplo, faça duplo clique nas linhas da grade. Escreva o resultado esperado na caixa de texto em relação a linha promovida. Separador fora do **separador** caixa de texto para pré-visualizar os resultados em gird os dados. Prima **OK** para consolidar a transformação. 

Usuário pode alternar entre o **modo básico** e o **modo avançado** clicando nas hiperligações no Editor de transformação.

### <a name="transform-editor-send-feedback"></a>Transformar editor: enviar comentários

Clicar no **enviar comentários** abre-se de vincular a **comentários** forneceu a caixa de diálogo com a caixa de comentários pré-preenchido com o utilizador de exemplos. Utilizador deve rever o conteúdo da caixa de comentários e fornecer mais detalhes para ajudar-na compreender o problema. Se o utilizador não quer partilhar dados com a Microsoft, o utilizador deve eliminar os dados de exemplo pré-preenchida antes de clicar o **enviar Feedback** botão. 

### <a name="editing-existing-transformation"></a>Editar transformação existente

Um utilizador pode editar uma existente **combinar coluna por exemplo** transformar selecionando **editar** opção do passo de transformação. Clicar em **edite** é aberto o Editor de transformação de mensagens em fila no **modo básico**. Utilizador pode introduzir a **modo avançado** clicando no link no cabeçalho. Todos os exemplos que foram fornecidos durante a criação da transformação são mostrados aqui.

## <a name="example-using-separators"></a>Exemplo de uso separadores

Uma vírgula seguida por um espaço é usada como um separador neste exemplo, para combinar os *Rua*, *Cidade*, *estado*, e *ZIP* colunas.

|Rua|Localidade|Estado|ZIP|Coluna|
|:----|:----|:----|:----|:----|
|16011 N.E. forma de 36th|REDMOND|WA|98052|16011 N.E. 36th way, REDMOND, WA, 98052|
|16021 N.E. forma de 36th|REDMOND|WA|98052|16021 N.E. 36th way, REDMOND, WA, 98052|
|16031 N.E. forma de 36th|REDMOND|WA|98052|16031 N.E. 36th way, REDMOND, WA, 98052|
|16041 N.E. forma de 36th|REDMOND|WA|98052|16041 N.E. 36th way, REDMOND, WA, 98052|
|16051 N.E. forma de 36th|REDMOND|WA|98052|16051 N.E. 36th way, REDMOND, WA, 98052|
|16061 N.E. forma de 36th|REDMOND|WA|98052|16061 N.E. 36th way, REDMOND, WA, 98052|
|3460 157th Avenida NE|REDMOND|WA|98052|NE de Avenida da 157th 3460, REDMOND, WA, 98052|
|3350 157th Ave N.E.|REDMOND|WA|98052|N.E. de Ave da 157th 3350, REDMOND, WA, 98052|
|3240 157th Avenida N.E.|REDMOND|WA|98052|N.E. de Avenida da 157th 3240, REDMOND, WA, 98052|

## <a name="example-using-by-example"></a>Exemplo de uso por meio de exemplos

O valor na **negrito** foi fornecido como um exemplo.

|Date|Mês|Ano|Hora|Minuto|Segundo|Colunas combinada|
|:----|:----|:----|:----|:----|:----|:----|
|13|Out|2016|15|01|23|**13-Outubro de 2016 15:01:23 PDT**|
|16|Out|2016|16|22|33|16-Outubro de 2016 15:01:33 PDT|
|17|Out|2016|12|43|12|17-Outubro de 2016 15:01:12 PDT|
|12|Nov|2016|14|22|44|12-Novembro de 2016 15:01:44 PDT|
|23|Nov|2016|01|52|45|23-Novembro de 2016 15:01:45 PDT|
|16|Jan|2017|22|34|56|16-Janeiro de 2016 15:01:56 PDT|
|23|Mar.|2017|01|55|25|23-Março de 2016 15:01:25 PDT|
|16|Abr.|2017|11|34|36|16-Abril de 2016 15:01:36 PDT|


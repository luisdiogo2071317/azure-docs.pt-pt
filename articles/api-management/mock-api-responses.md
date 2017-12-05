---
title: Mock respostas de API com o portal do Azure | Microsoft Docs
description: "Este tutorial mostra como utilizar a gestão de API (APIM) para definir uma política numa API, de modo que devolva uma resposta mocked. Este método endables aos programadores para continuar com a implementação e o teste da instância de API Management, no caso de back-end não está disponível para enviar as respostas reais."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: e485071b026c52eb23532639546ad475fc92cde3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="mock-api-responses"></a>Mock respostas de API

APIs de back-end pode ser importada para uma API APIM ou criada e gerida manualmente. Os passos neste tutorial mostram-lhe como utilizar APIM para criar uma API em branco e geri-lo manualmente. O tutorial mostra como definir uma política numa API, de modo que devolva uma resposta mocked. Este método permite aos programadores continuar com a implementação e teste da instância APIM, mesmo que o back-end não está disponível para enviar as respostas reais. Capacidade de mock segurança respostas pode ser útil em vários cenários:

+ Quando a fachada de API concebida pela primeira vez e a implementação de back-end inclui mais tarde. Em alternativa, o back-end está a ser desenvolvido em paralelo.
+ Quando o back-end estiver temporariamente não operacional ou não consegue dimensionar.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um teste de API 
> * Adicionar uma operação para o teste de API
> * Ativar mocking de resposta
> * Testar a API mocked

![Resposta da operação mocked](./media/mock-api-responses/mock-api-responses01.png)

## <a name="prerequisites"></a>Pré-requisitos

Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-a-test-api"></a>Criar um teste de API 

Os passos nesta secção mostram como criar uma API em branco com nenhuma back-end. Também mostra como adicionar uma operação para a API. Chamar a operação depois de concluir os passos nesta secção produz um erro. Irá obter sem erros, depois de concluir os passos na secção "Ativar resposta mocking".

1. Selecione **APIs** em **gestão de API**.
2. No menu à esquerda, selecione **+ adicionar API**.
3. Selecione **API em branco** da lista.
4. Introduza "*API de teste*" para **nome a apresentar**.
5. Introduza "*ilimitados*" para **produtos**.
6. Selecione **Criar**.

## <a name="add-an-operation-to-the-test-api"></a>Adicionar uma operação para o teste de API

1. Selecione a API que criou no passo anterior.
2. Clique em **+ adicionar operação**.

    ![Resposta da operação mocked](./media/mock-api-responses/mock-api-responses02.png)

    |Definição|Valor|Descrição|
    |---|---|---|
    |**URL** (verbo HTTP)|INTRODUÇÃO|Pode escolher um dos verbos HTTP predefinidos.|
    |**URL** |*/Test*|Um caminho de URL para a API. |
    |**Nome a apresentar**|*Chamada de teste*|O nome que é apresentado no **portal do programador**.|
    |**Descrição**||Forneça uma descrição da operação que é utilizada para fornecer a documentação para os programadores a utilizar esta API no **portal do programador**.|
    |**Consulta** separador||Pode adicionar parâmetros de consulta. Para além de fornecer um nome e uma descrição, pode fornecer valores que podem ser atribuídos a este parâmetro. Um dos valores pode ser assinalado como predefinição (opcional).|
    |**Pedir** separador||Pode definir tipos de conteúdo do pedido, exemplos e esquemas. |
    |**Resposta** separador|Ver os passos que se seguem nesta tabela.|Defina códigos de estado de resposta, tipos de conteúdo, exemplos e esquemas.|

3. Selecione o **resposta** separador, localizada sob o URL, nome a apresentar e campos de descrição.
4. Clique em **+ adicionar resposta**.
5. Selecione **200 OK** da lista.
6. Sob o **representações** cabeçalho no lado direito, selecione **+ adicionar representação**.
7. Introduza "*application/json*" para a caixa de pesquisa e selecione o **application/json** tipo de conteúdo.
8. No **exemplo** texto, introduza "*{'sampleField': 'teste'}*".
9. Selecione **Guardar**.

## <a name="enable-response-mocking"></a>Ativar mocking de resposta

1. Selecione a API que criou no passo "Criar uma API de teste".
2. Selecione a operação de teste que adicionou.
2. Na janela do lado direito, clique o **Design** separador.
3. No **processamento entrado** janela, clique no ícone de lápis.
4. No **Mocking** separador, selecione **as respostas estáticas** para **Mocking comportamento**.
5. No **API Management devolve a seguinte resposta:** caixa de texto, escreva **200 OK, application/json**. Esta seleção indica que a sua API deverá devolver o exemplo de resposta definido na secção anterior.
6. Selecione **Guardar**.

## <a name="test-the-mocked-api"></a>Testar a API mocked

1. Selecione a API que criou no passo "Criar uma API de teste".
2. Abra o **teste** separador.
3. Certifique-se a **testar chamada** API está selecionada.

    > [!TIP]
    > Uma barra amarela com o texto **Mocking está ativada** indica que as respostas devolvidas de API Management, envia uma política mocking e não uma resposta de back-end real.

3. Selecione **enviar** para efetuar um teste de chamada.
4. O **resposta de HTTP** apresenta o JSON fornecido como um exemplo na primeira secção do tutorial.

## <a name="video"></a>Vídeo

> [!VIDEO https://www.youtube.com/embed/i9PjUAvw7DQ]
> 
> 

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar um teste de API
> * Adicionar uma operação para o teste de API
> * Ativar mocking de resposta
> * Testar a API mocked

Avançar para o próximo tutorial:

> [!div class="nextstepaction"]
> [Transformar e proteger uma API publicada](transform-api.md)

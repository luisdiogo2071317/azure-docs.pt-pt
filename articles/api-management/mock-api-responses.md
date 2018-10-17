---
title: Simular respostas de API com o portal do Azure | Microsoft Docs
description: Este tutorial mostra como utilizar a Gestão de API (APIM) para definir uma política numa API de modo a devolver uma resposta simulada. Este método permite aos programadores continuar a implementação e o teste da instância de Gestão de API no caso de o back-end não estar disponível para enviar respostas reais.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: 916d0cf37ab3588091d4ca2d45f43a5669afe4f1
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094900"
---
# <a name="mock-api-responses"></a>Simular respostas de API

As APIs de back-end podem ser importadas para uma API de APIM ou criadas e geridas manualmente. Os passos neste tutorial mostram como utilizar o APIM para criar uma API em branco e geri-la manualmente. O tutorial mostra como definir uma política numa API de modo a devolver uma resposta simulada. Este método permite aos programadores continuar a implementação e o teste da instância de APIM mesmo que o back-end não esteja disponível para enviar respostas reais. A capacidade de simular respostas pode ser útil em vários cenários:

+ Quando a fachada de API é concebida primeiro e a implementação de back-end é efetuada mais tarde. Quando o back-end está a ser desenvolvido em paralelo.
+ Quando o back-end está temporariamente não operacional ou não consegue dimensionar.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma API de teste 
> * Adicionar uma operação à API de teste
> * Ativar a simulação de respostas
> * Testar a API simulada

![Resposta de operação simulada](./media/mock-api-responses/mock-api-responses01.png)

## <a name="prerequisites"></a>Pré-requisitos

Conclua o guia de início rápido seguinte: [Criar uma instância da Gestão de API do Azure](get-started-create-service-instance.md).

## <a name="create-a-test-api"></a>Criar uma API de teste 

Os passos nesta secção mostram como criar uma API em branco sem back-end. Mostra também como adicionar uma operação à API. Chamar a operação depois de concluir os passos nesta secção produz um erro. Não obterá erros depois de concluir os passos na secção "Ativar a simulação de respostas".

1. Selecione **APIs** no serviço **Gestão de API**.
2. No menu à esquerda, selecione **+ Adicionar API**.
3. Selecione **API em branco** na lista.
4. Introduza "*API de teste*" para **Nome a apresentar**.
5. Introduza "*Ilimitados*" para **Produtos**.
6. Selecione **Criar**.

## <a name="add-an-operation-to-the-test-api"></a>Adicionar uma operação à API de teste

1. Selecione a API que criou no passo anterior.
2. Clique em **+ Adicionar Operação**.
    ![Resposta da operação simulada](./media/mock-api-responses/mock-api-responses-add-operation.png)

    |Definição|Valor|Descrição|
    |---|---|---|
    |**Nome a apresentar**|*Chamada de teste*|O nome que é apresentado no **Portal do programador**.|
    |**URL** (verbo HTTP)|GET|Pode escolher um dos verbos HTTP predefinidos.|
    |**URL** |*/test*|Um caminho de URL para a API. |
    |**Descrição**||Forneça uma descrição da operação utilizada para fornecer a documentação aos programadores que utilizam esta API no **Portal do programador**.|
    |Separador **Consulta**||Pode adicionar parâmetros de consulta. Além de fornecer um nome e uma descrição, pode fornecer valores que podem ser atribuídos a este parâmetro. Um dos valores pode ser marcado como predefinição (opcional).|
    |Separador **Pedido**||Pode definir os tipos de conteúdo, exemplos e esquemas do pedido. |
    |Separador **Resposta**|Veja os passos que se seguem nesta tabela.|Defina códigos de estado de resposta, tipos de conteúdo, exemplos e esquemas.|

3. Selecione o separador **Resposta**, localizado abaixo dos campos URL, Nome a apresentar e Descrição.
4. Clique em **+ Adicionar resposta**.
5. Selecione **200 OK** na lista.
6. No cabeçalho **Representações** à direita, selecione **+ Adicionar representação**.
7. Introduza "*application/json*" na caixa de pesquisa e selecione o tipo de conteúdo **application/json**.
8. Na caixa de texto **Exemplo**, introduza `{ 'sampleField' : 'test' }`.
9. Selecione **Criar**.

## <a name="enable-response-mocking"></a>Ativar a simulação de respostas

1. Selecione a API que criou no passo "Criar uma API de teste".
2. Selecione a operação de teste que adicionou.
3. Na janela à direita, clique no separador **Conceber**.
4. Na janela **Processamento de entrada**, clique no ícone de lápis.
5. No separador **Simulação**, selecione **Respostas estáticas** para **Comportamento de simulação**.
6. Na caixa de texto **A Gestão de API devolve a seguinte resposta:**, escreva **200 OK, application/json**. Esta seleção indica que a API deve devolver o exemplo de resposta que definiu na secção anterior.
    ![Ativar a simulação de respostas](./media/mock-api-responses/mock-api-responses-set-mocking.png)
7. Clique em **Guardar**.

## <a name="test-the-mocked-api"></a>Testar a API simulada

1. Selecione a API que criou no passo "Criar uma API de teste".
2. Abra o separador **Testar**.
3. Certifique-se de que a API **Chamada de teste** está selecionada.

    > [!TIP]
    > Uma barra amarela com o texto **A simulação está ativada** indica que as respostas devolvidas da Gestão de API enviam uma política de simulação e não uma resposta de back-end real.

4. Selecione **Enviar** para efetuar uma chamada de teste.
5. A **resposta de HTTP** apresenta o JSON fornecido como um exemplo na primeira secção do tutorial.
    ![Ativar a simulação de respostas](./media/mock-api-responses/mock-api-responses-test-response.png)

## <a name="video"></a>Vídeo

> [!VIDEO https://www.youtube.com/embed/i9PjUAvw7DQ]
> 
> 

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma API de teste
> * Adicionar uma operação à API de teste
> * Ativar a simulação de respostas
> * Testar a API simulada

Avance para o tutorial seguinte:

> [!div class="nextstepaction"]
> [Transformar e proteger a sua API](transform-api.md)

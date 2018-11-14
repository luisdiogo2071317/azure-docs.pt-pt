---
title: Ver resultados de teste de sistema e de implementação - Translator personalizado
titleSuffix: Azure Cognitive Services
description: Quando seu treinamento é efetuada com êxito, reveja os testes de sistema para analisar os resultados de treinamento. Se estiver satisfeito com os resultados de treinamento, coloca um pedido de implementação para o modelo preparado.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 61d3869559d88e14c0b9a3c3e23cd8a1f9c2b6c4
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627647"
---
# <a name="view-system-test-results"></a>Ver resultados do teste de sistema

Quando seu treinamento é efetuada com êxito, reveja os testes de sistema para analisar os resultados de treinamento. Se estiver satisfeito com os resultados de treinamento, coloca um pedido de implementação para o modelo preparado. 

## <a name="system-test-results-page"></a>Página de resultados do teste de sistema

Selecione um projeto, em seguida, selecione o separador de modelos desse projeto, localize o modelo que pretende utilizar e, finalmente, selecione o separador de teste.

O separador de teste mostra-lhe:

1.  Resultados do teste de sistema: O resultado do processo de teste nos treinamentos. O processo de teste produz a pontuação de BLEU.

    **Contagem de sentença:** frases paralelas quantas foram utilizadas no conjunto de teste.

     **Pontuação de BLEU:** pontuação BLEU gerada para um modelo após a conclusão do treinamento.

    **Estado:** indica se o processo de teste for concluído ou em curso.

    ![Resultados do teste de sistema](media/how-to/how-to-system-test-results.png)

2.  Clique nos resultados de teste de sistema e que será apresentada a página de detalhes do resultado de teste. Esta página mostra a conversão de máquina de frases que faziam parte do conjunto de dados de teste.

3.  A tabela na página de detalhes do resultado de teste tem duas colunas – uma para cada idioma no par. A coluna para a linguagem de código mostra a frase ser traduzido. A coluna para o idioma de destino contém duas frases em cada linha.

    **Ref:** esta frase é a tradução de referência da frase de origem fornecido no conjunto de dados de teste.

    **MT:** esta frase é a tradução automática da sentença origem feita com o modelo criado após a formação foi realizada.

    ![Comparação de resultados do teste de sistema](media/how-to/how-to-system-test-results-2.png)

## <a name="download-test"></a>Transferir teste

Clique na ligação de traduções de transferir para transferir um ficheiro zip. O zip contém as traduções de máquina de frases de origem no conjunto de dados de teste.

![Transferir teste](media/how-to/how-to-system-test-download.png)

Este arquivo zip transferido contém três arquivos.

1.  Custom.MT.txt: este ficheiro contém traduções de máquina de frases de idioma de origem no idioma de destino efetuada pelo modelo preparado com dados do utilizador.

2.  Ref.txt: este ficheiro contém traduções fornecido pelo utilizador de frases de idioma de origem no idioma de destino.

3.  Source.txt: este ficheiro contém as frases no idioma de origem.

    ![Resultados do teste de sistema transferido](media/how-to/how-to-download-system-test.png)

## <a name="deploy-a-model"></a>Implementar um modelo

Para pedir uma implementação:

1.  Selecione um projeto, aceda ao separador de modelos.

2. Um modelo preparado com êxito, ele mostra o botão "Deploy", se não for implementada.

    ![Implementar modelo](media/how-to/how-to-deploy-model.png)

3.  Clique em implementar.
4.  Selecione as regiões onde pretende que o seu modelo de implementação e clique em Guardar. Pode selecionar várias regiões.

    ![Implementar modelo](media/how-to/how-to-deploy-model-regions.png)

5.  Pode ver o estado do seu modelo na coluna "Estado".

>[!Note]
>Se já é implementado um modelo, verá um botão "Undeploy" para esse modelo. Para anular a implementação um modelo, clique em "Undeploy".

## <a name="next-steps"></a>Passos Seguintes

- Começar a utilizar o seu modelo de tradução personalizadas implementado via [API de texto do Microsoft Translator V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
- Saiba mais [como gerenciar as configurações](how-to-manage-settings.md) para partilhar a sua área de trabalho, gerir a chave de subscrição.
- Saiba mais [como migrar a sua área de trabalho e o projeto](how-to-migrate.md) partir [Hub do Microsoft Translator](https://hub.microsofttranslator.com)
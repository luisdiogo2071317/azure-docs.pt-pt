---
title: Migrar a área de trabalho do Microsoft Translator Hub e projetos? -Tradutor personalizado
titleSuffix: Azure Cognitive Services
description: Migre a sua área de trabalho do Hub e projetos ao Translator personalizado.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 378baad0735238dc0921e5e78e2a27b3ae907e19
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627797"
---
# <a name="migrate-hub-workspace-and-projects-to-custom-translator"></a>Migrar os projetos de área de trabalho de Hub e ao Translator personalizado

Pode migrar seus [Hub do Microsoft Translator](https://hub.microsofttranslator.com/) projetos ao Translator personalizado e de área de trabalho. Migração começa a partir do Hub.


Estes itens são migrados durante o processo:

1.  As definições de projetos.

2.  A definição de treinamento será utilizada para criar uma nova definição de modelo no Translator personalizado.

3.  Os arquivos de paralelos e monolíngües usados dentro de treinamentos serão todos migrados como novos documentos no Translator personalizado.

4.  O teste de sistema gerado automaticamente e a otimização de dados serão exportados e criados como novos documentos no Translator personalizado.

Para todos os treinamentos implementados, Translator personalizado irá preparar o modelo sem qualquer custo. Tem a opção de implementar manualmente.

>[!Note]
>Para um treinamento tenha êxito, o Translator personalizado requer mínimo 10.000 extraídas as frases. Para um número inferior de frases extraídos do que o [mínima sugerida](sentence-alignment.md#suggested-minimum-number-of-extracted-and-aligned-sentences), Translator personalizado não é possível realizar um treinamento.

Para todos os treinamentos com êxito, o que não são implementados, estas serão migradas como rascunho no Translator personalizado.

## <a name="find-custom-translator-workspace-id"></a>Localizar o Id de área de trabalho do Microsoft Translator personalizado

Para migrar [Hub do Microsoft Translator](https://hub.microsofttranslator.com/) terá de destino. o Id de área de trabalho no Translator personalizado de área de trabalho. A área de trabalho de destino no Translator personalizado é onde todas as suas áreas de trabalho de Hub e os projetos devem ser migrados para.

Encontrará sua Id de área de trabalho de destino na página de definições do Microsoft Translator personalizadas: 

1.  Aceda à página de "Definição" no portal do Translator personalizado.

2.  Encontrará o Id de área de trabalho na seção informações básicas.

    ![Como localizar o ID de área de trabalho de destino](media/how-to/how-to-find-destination-ws-id.png)

3. Mantenha o seu Id de área de trabalho para fazer referência durante o processo de migração de destino.

## <a name="migrate-workspace"></a>Migrar a área de trabalho

Ao migrar a sua área de trabalho completa do Hub ao translator personalizado, os seus projetos, documentos e treinamentos são migrados para o translator personalizado. Antes da migração, tem de escolher se pretende migrar apenas treinamentos implementados ou que pretende migrar todos os treinamentos com êxito.

Para migrar um espaço de trabalho:

1.  Inicie sessão no Hub de Microsoft Translator.

2.  Aceda à página "Definições".

3.  Na página "Definições" clique em "Área de trabalho para migrar dados ao Translator personalizada".

    ![Como migrar a partir do Hub](media/how-to/how-to-migrate-workspace-from-hub.png)

4.  Na página seguinte, selecione uma destas duas opções:

    a.  Implementado Treinamentos apenas: a seleção desta opção irá migrar apenas os sistemas implantados e os documentos relacionados.

    b.  Todos os Treinamentos com êxito: A seleção desta opção irá migrar todos os treinamentos com êxito e documentos relacionados.

    c.  Introduza o seu ID de área de trabalho de destino no Translator personalizado.

    ![Como migrar a partir do Hub](media/how-to/how-to-migrate-from-hub-screen.png)

5.  Clique em Submeter pedido.

## <a name="migrate-project"></a>Migrar o projeto

Se pretender migrar seus projetos de forma seletiva, o Microsoft Translator Hub dá-lhe essa capacidade.

Para migrar um projeto:

1.  Inicie sessão no Hub de Microsoft Translator.

2.  Aceda à página de "Projects".

3.  Clique em "Migrar" link para o projeto apropriado.

    ![Como migrar a partir do Hub](media/how-to/how-to-migrate-from-hub.png)

4.  Na página seguinte, selecione uma destas duas opções:

    a.  Implementado Treinamentos apenas: a seleção desta opção irá migrar apenas os sistemas implantados e os documentos relacionados. 

    b.  Todos os Treinamentos com êxito: A seleção desta opção irá migrar todos os treinamentos com êxito e documentos relacionados.

    c.  Introduza o seu ID de área de trabalho de destino no Translator personalizado.

    ![Como migrar a partir do Hub](media/how-to/how-to-migrate-from-hub-screen.png)

5.  Clique em "Submeter o pedido".

## <a name="migration-history"></a>Histórico de migração

Quando pediu a área de trabalho / migração a partir do Hub do projeto, encontrará o histórico de migração na página de definições do Microsoft Translator personalizadas. 

Para ver o histórico de migração, siga estes passos:

1.  Aceda à página de "Definição" no portal do Translator personalizado.

2.  Na secção de histórico de migração da página de definições, clique em histórico de migração.

    ![Histórico de migração](media/how-to/how-to-migration-history.png)

Página de histórico de migração apresenta seguindo as informações de resumo para cada pedido de migração.

1.  Migrados pela: Nome e e-mail do utilizador submeter este pedido de migração

2.  Migrados: Data e hora carimbo da migração

3.  Projetos: O número de projetos solicitado para o número de v/s de migração de projetos migrado com êxito.

4.  Treinamentos: Número de treinamentos solicitado para o número de v/s de migração de treinamentos migrado com êxito.

5.  Documentos: O número de documentos solicitado para o número de v/s de migração de documentos migrado com êxito.

    ![Detalhes do histórico de migração](media/how-to/how-to-migration-history-details.png)

Se quiser que mais detalhadas que o relatório de migração sobre os seus projetos, treinamentos e documentos, terá opção Exportar detalhes como CSV.

>[!Note]
>A migração apenas é suportada para os pares de idiomas em que idiomas NMT existem. Verifique a lista de atualmente [NMT idiomas suportados](https://www.microsoft.com/translator/business/languages/). Para os pares de idiomas em que idiomas NMT não existir, dados serão movidos de Hub ao Translator personalizado, mas não podem ser conduzidos de treinamentos sobre esses pares de idiomas.

## <a name="next-steps"></a>Passos Seguintes

- [Preparar um modelo](how-to-train-model.md).
- Começar a utilizar o seu modelo de tradução personalizadas implementado via [API de texto do Microsoft Translator V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
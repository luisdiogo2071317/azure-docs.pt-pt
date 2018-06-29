---
title: Compreender os conceitos de conversão de dados do LUIS - Azure | Microsoft Docs
description: Saiba como utterances podem ser alteradas antes predições na compreensão de idiomas (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/27/2018
ms.author: v-geberr;
ms.openlocfilehash: 16b0df4b81220885e2c3747470272cee9536e10c
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063566"
---
# <a name="data-conversion-concepts-in-luis"></a>Conceitos de conversão de dados no LUIS
LUIS fornece uma forma para converter utterances de utterances ditas utterances texto antes de predição. 

## <a name="speech-to-intent-conversion-concepts"></a>Reconhecimento de voz para conceitos de intenção de conversão
A conversão de reconhecimento de voz para texto LUIS permite-lhe enviar utterances ditas para um ponto final e receber uma resposta de predição LUIS. O processo é uma integração do [voz](https://docs.microsoft.com/azure/cognitive-services/Speech) serviço com LUIS. 

### <a name="key-requirements"></a>Requisitos de chaves
Não é necessário criar um **API de reconhecimento de voz do Bing** chaves para esta integração. A **compreensão de idiomas** chave criada no portal do Azure funciona para esta integração. Não utilize a chave de arranque LUIS, não irá funcionar para esta integração.

### <a name="new-endpoint"></a>Novo ponto final 
Esta integração cria um novo ponto final e [preços](luis-boundaries.md#key-limits) modelo. O ponto final, através de [SDK de reconhecimento de voz](https://github.com/Azure-Samples/cognitive-services-speech-sdk), pode receber ambos autenticação ditas e utterances de texto, permitindo-lhe para utilizá-lo como um único ponto final. 

### <a name="quota-usage"></a>Utilização da quota
Consulte [chave limites](luis-boundaries.md#key-limits) para obter informações. 

### <a name="data-retention"></a>Retenção de dados
Os dados enviados para o ponto final, através do SDK de reconhecimento de voz, independentemente se se tratar de reconhecimento de voz ou de texto, só são utilizados para melhorar o seu modelo de reconhecimento de voz. Não é utilizado para além do seu modelo para melhorar o reconhecimento de voz ou LUIS uma capacidade geral. Quando a aplicação de LUIS é eliminada, os dados retidos também são eliminados.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Utilize o reconhecimento de voz para texto](luis-tutorial-speech-to-intent.md)


---
title: Terminologia de API Management do Azure | Microsoft Docs
description: "Este artigo fornece definições para os termos de licenciamento que são específicos para a API Management."
services: api-management
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/11/2017
ms.author: apimpm
ms.openlocfilehash: 9391b65a5aade4c050ca964354bfea2d3a2338d8
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="terminology"></a>Terminologia

Este artigo fornece definições para os termos de licenciamento que são específicos para gestão de API (APIM).

## <a name="term-definitions"></a>Definições de termo

* **API de back-end** -serviço de um HTTP que implementa a API e as suas operações. 
* **API de front-end**/**APIM API** -uma API de APIM não alojar APIs, cria fachadas para as suas APIs para personalizar a fachada consoante as suas necessidades sem afetar o back end de API. Para obter mais informações, consulte [importação e publicar uma API](import-and-publish.md).
* **Produto APIM** -um produto contém uma ou mais APIs, bem como uma quota de utilização e os termos de utilização. Pode incluir um número de APIs e oferecem-las para os programadores através do portal do programador. Para obter mais informações, consulte [criar e publicar um produto](api-management-howto-add-products.md).
* **Operação de APIM API** -cada APIM API representa um conjunto de operações disponíveis para os programadores. Cada API APIM contém uma referência para o serviço de back-end que implementa a API e as respetivo operações efetuam o mapeamento para as operações implementadas pelo serviço de back-end. Para obter mais informações, consulte [respostas de Mock API](mock-api-responses.md).
* **Versão** - por vezes, que pretende publicar novos ou diferentes API funcionalidades a alguns utilizadores, enquanto outros pretendem stick com a API atualmente funciona para os mesmos. Para obter mais informações, consulte [publicar várias versões da sua API](api-management-get-started-publish-versions.md).
* **Revisão** - quando a API está pronto para ir e começa a ser utilizado pelos programadores, que, normalmente, tem de asseguramos na realização de alterações para essa API e ao mesmo tempo para não prejudicam os chamadores da sua API. Também é útil permitir que os programadores de saber sobre as alterações que efetuou. Para obter mais informações, consulte [utilizar revisões](api-management-get-started-revise-api.md).
* **Portal do programador** -os seus clientes (programadores) devem utilizar o portal do programador para aceder o APIs. O portal do programador pode ser personalizado. Para obter mais informações, consulte [personalizar o portal do programador](api-management-customize-styles.md).

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Criar uma instância](get-started-create-service-instance.md)


---
title: As restrições e os problemas conhecidos na importação de API de gestão do Azure API | Microsoft Docs
description: Detalhes de problemas conhecidos e restrições a importar para utilizar os formatos de API aberta, WSDL ou WADL de API Management do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: b33c95af94c436b1069658963692242d0f905554
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="api-import-restrictions-and-known-issues"></a>Restrições de importação de API e problemas conhecidos
## <a name="about-this-list"></a>Sobre esta lista
Quando importar uma API, poderá ser apresentados algumas restrições ou identificar problemas que necessitam de ser retificado antes de importar com êxito. Documentos neste artigo estes, organizados pelo formato de importação da API.

## <a name="open-api"> </a>Abra API/Swagger
Se está a receber erros de importar o documento de API aberta, certifique-se de ter validado-la - o utilizando o estruturador no portal do Azure (estrutura - Front-End - abra API especificação Editor) ou com terceiros uma ferramenta como <a href="http://www.swagger.io">Swagger Editor</a>.

* Apenas o formato JSON para OpenAPI é suportado.
* Esquemas referenciadas utilizando **$ref** propriedades não podem conter outros **$ref** propriedades.
* **$ref** os apontadores não é possível fazer referência a ficheiros externos.
* **x-ms-caminhos** e **x servidores** são as extensões suportadas apenas.
* Extensões personalizadas são ignoradas na importação e não são guardadas ou preservadas para a exportação.

> [!IMPORTANT]
> Veja este [documento](https://blogs.msdn.microsoft.com/apimanagement/2018/03/28/important-changes-to-openapi-import-and-export/) para obter informações importantes e sugestões relacionadas com a importação de OpenAPI.

## <a name="wsdl"> </a>WSDL
Ficheiros WSDL são utilizados para gerar SOAP Pass-through APIs ou servem como o back-end de uma API SOAP-REST.
* **Enlaces de SOAP** -SOAP apenas enlaces de estilo "documento" e "literal" codificação são suportados. Não são suportadas para o estilo de "rpc" ou a codificação SOAP.
* **WSDL: import** -este atributo não é suportado. Os clientes devem intercala as importações num único documento.
* **As mensagens com várias partes** -estes tipos de mensagens em fila não são suportados.
* **WCF wsHttpBinding** -serviços SOAP criados com o Windows Communication Foundation devem utilizar o basicHttpBinding, wsHttpBinding não é suportada.
* **MTOM** - serviços utilizando MTOM <em>poderá</em> funcione. Oficial de suporte não é oferecido neste momento.
* **Recursão** -tipos que estão definidos em modo recursivo (por exemplo, consulte a uma matriz dos próprios) não são suportadas pelo APIM.

## <a name="wadl"> </a>WADL
Atualmente, não existem nenhum WADL importar os problemas conhecidos.

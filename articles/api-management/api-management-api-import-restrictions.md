---
title: "As restrições e os problemas conhecidos na importação de API de gestão do Azure API | Microsoft Docs"
description: "Detalhes de problemas conhecidos e restrições a importar para utilizar os formatos de API aberta, WSDL ou WADL de API Management do Azure."
services: api-management
documentationcenter: 
author: vladvino
manager: vlvinogr
editor: 
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: 758babce3ed387ed4864f1934650cf701bda788f
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="api-import-restrictions-and-known-issues"></a>Restrições de importação de API e problemas conhecidos
## <a name="about-this-list"></a>Sobre esta lista
Quando importar uma API, poderá ser apresentados algumas restrições ou identificar problemas que necessitam de ser retificado antes de importar com êxito. Documentos neste artigo estes, organizados pelo formato de importação da API.

## <a name="open-api"></a>Abrir API/Swagger
Se está a receber erros de importar o documento de API aberta, certifique-se de ter validado-la - o utilizando o estruturador no portal do Azure (estrutura - Front-End - abra API especificação Editor) ou com terceiros uma ferramenta como <a href="http://www.swagger.io">Swagger Editor</a>.

* **Nome de anfitrião** APIM requer um atributo de nome de anfitrião.
* **Caminho de base** APIM requer um atributo de caminho de base.
* **Os esquemas** APIM requer uma matriz de esquema. 

## <a name="wsdl"></a>WSDL
Ficheiros WSDL são utilizados para gerar SOAP Pass-through APIs ou servem como o back-end de uma API SOAP-REST.

* **WSDL: import** -atualmente, APIM não suporta APIs utilizando este atributo. Os clientes devem intercalar os elementos importados para um documento.
* **As mensagens com várias partes** , APIM não suporta atualmente estes tipos de mensagens em fila.
* **WCF wsHttpBinding** serviços SOAP criados com o Windows Communication Foundation devem utilizar o basicHttpBinding, wsHttpBinding não é suportada.
* **MTOM** serviços que utilizam MTOM <em>poderá</em> funcione. Oficial de suporte não é oferecido neste momento.
* **Recursão** tipos que estão definidos em modo recursivo (por exemplo, consulte a uma matriz dos próprios) não são suportadas pelo APIM.

## <a name="wadl"></a>WADL
Atualmente, não existem nenhum WADL importar os problemas conhecidos.

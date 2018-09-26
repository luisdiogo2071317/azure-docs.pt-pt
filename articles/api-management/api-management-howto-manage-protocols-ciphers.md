---
title: Gerir protocolos e cifras na gestão de API do Azure | Documentos da Microsoft
description: Saiba como gerir protocolos (TLS, SSL) e cifras (DES) na API Management do Azure.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: 454ba5c42694581bfa8fb1ec69ce97ac906b53d4
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094580"
---
# <a name="manage-protocols-and-ciphers-in-azure-api-management"></a>Gerir protocolos e cifras na gestão de API do Azure

Gestão de API do Azure suporta várias versões do protocolo TLS para os lados de cliente e o back-end, bem como a cifra 3DES.

Este guia mostra-lhe como gerir protocolos e cifras configuração para uma instância de gestão de API do Azure.

![Gerir protocolos e cifras em APIM](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers.png)

## <a name="prerequisites"></a>Pré-requisitos

Para seguir os passos neste artigo, tem de ter:

* Uma instância de gestão de API

## <a name="how-to-manage-tls-protocols-and-3des-cipher"></a>Como gerir cifras TLS de protocolos e 3DES

1. Navegue até à sua **instância de gestão de API** no portal do Azure.
2. Selecione **SSL** no menu.  
    ![Gerir protocolos e cifras em APIM - menu](./media/api-management-howto-manage-protocols-ciphers/api-management-menu.png)
3. Ativar ou desativar os protocolos pretendidos ou cifras.
4. Clique em **Guardar**. As alterações serão aplicadas dentro de uma hora.  
    ![Gerir protocolos e cifras em APIM - guardar](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers-save.png)

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [TLS (Transport Layer Security)](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls).
* Confira mais [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) sobre a gestão de API.
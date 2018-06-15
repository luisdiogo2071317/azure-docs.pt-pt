---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9fa18b14b82376a25bb434acd770d340b1ef9262
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197092"
---
Se estiver a ter problemas em ligar, verifique os seguintes itens:

- Se exportou um certificado de cliente, certifique-se de que o exportou como um ficheiro .pfx com o valor predefinido “Incluir todos os certificados no caminho de certificação, se possível”. Quando o exporta com este valor, as informações do certificado de raiz também são exportadas. Quando o certificado é instalado no computador cliente, o certificado de raiz que se encontra no ficheiro .pfx também é instalado no computador cliente. O computador cliente tem de ter as informações do certificado de raiz instaladas. Para verificar, aceda a **Gerir certificados de utilizador** e navegue para **Autoridades de Certificação de Raiz Fidedigna/Certificados**. Certifique-se de que o certificado de raiz está listado. O certificado de raiz tem de estar presente para que a autenticação funcione.

- Se estiver a utilizar um certificado que foi emitido através de uma solução de AC Empresarial e se deparou com problemas de autenticação, verifique a ordem de autenticação do certificado de cliente. Pode verificar a ordem da lista de autenticação ao clicar duas vezes no certificado de cliente e aceder a **Detalhes > Utilização Avançada de Chaves**. Certifique-se de que a lista apresenta "Autenticação de Cliente" como o primeiro item. Caso contrário, terá de emitir um certificado de cliente com base no modelo de Utilizador que tenha a Autenticação de Cliente como o primeiro item na lista.

- Para obter informações adicionais sobre a resolução de problemas com P2S, veja [Resolução de problemas com ligações P2S](../articles/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
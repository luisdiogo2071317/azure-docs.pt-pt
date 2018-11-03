---
title: Compreender as práticas recomendadas de segurança de duplos Digital do Azure | Documentos da Microsoft
description: Os gémeos Digital segurança melhores práticas do Azure
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: adgera
ms.openlocfilehash: 6ca01523744dbce15f8fdb3bbe2d5a9b44510f3f
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50959482"
---
# <a name="security-best-practices"></a>Melhores práticas de segurança

Segurança de duplos Digital do Azure permite que preciso acesso a recursos específicos e ações no seu gráfico de IoT. Ele faz isso por meio da função granular e controlo de acesso baseado em funções de chamada de gestão de permissões.

Duplos Digital do Azure também utiliza outros recursos de segurança que estão presentes no Azure IoT, que inclui o Azure Active Directory (Azure AD). Por esse motivo, configurar a sua aplicação de duplos Digital do Azure envolve a utilização de muitas das mesmas [práticas de segurança do Azure IoT](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/) recomendado atualmente.

Este artigo resume as principais práticas recomendadas a seguir.

> [!IMPORTANT]
> Para garantir a segurança máximo para o seu espaço de IoT, reveja os recursos de segurança adicional. Certifique-se incluir os seus fornecedores de dispositivos.

## <a name="iot-security-best-practices"></a>Melhores práticas da segurança de IoT

Algumas das principais práticas para proteger os seus dispositivos IoT com segurança incluem:

> [!div class="checklist"]
> * Proteja a cada dispositivo que está ligado ao seu espaço de IoT de uma forma de prova de adulteração.
> * Limite a função de cada pessoa dentro do seu espaço de IoT, sensores e dispositivos. Se comprometido, o efeito é minimizado.
> * Considere a utilização de potencial de IP do dispositivo de filtragem de endereços e restrição de porta.
> * Limitar largura de banda de e/s e dispositivo para melhorar o desempenho. Limitação de velocidade pode aumentar a segurança, impedindo ataques denial-of-service.
> * Mantenha o firmware do dispositivo atualizadas.

Algumas chaves práticas para proteger a um espaço de IoT com segurança incluem:

> [!div class="checklist"]
> * Encripte dados persistentes, guardados ou armazenados.
> * Exigir palavras-passe ou chaves para ser alterada ou atualizada periodicamente.
> * Restringir cuidadosamente o acesso e permissões por função. Consulte a secção "acesso baseado em função de controlo" práticas recomendadas.
> * Utilize a encriptação poderosas. Necessitam de senhas longas e utilize protocolos seguros e a autenticação de dois fatores.

Monitorize os recursos de IoT para deteção de valores atípicos, ameaças ou parâmetros do recurso que fique fora do intervalo da operação normal. Utilize a análise do Azure para gerir a monitorização.

> [!NOTE]
> Para obter mais informações sobre o processamento de eventos e monitorização, consulte [encaminhar eventos e mensagens duplos Digital do Azure](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Práticas recomendadas do Active Directory do Azure

Os gémeos Digital do Azure utiliza o Azure AD para autenticar os utilizadores e proteger as aplicações. O Azure AD suporta a autenticação para uma variedade de modernas arquiteturas. Todos elas são baseadas em protocolos de norma da indústria, como o OAuth 2.0 ou OpenID Connect. Alguns principais práticas para proteger o seu espaço de IoT para o Azure AD incluem:

> [!div class="checklist"]
> * Store chaves e segredos de aplicações do Azure AD numa localização segura, tal como [do Azure Key Vault](https://azure.microsoft.com/services/key-vault/).
> * Utilizar um certificado emitido por uma confiança [autoridade de certificação](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started) em vez de segredos de aplicação para autenticar.
> * Limitar o âmbito de OAuth 2.0 de acesso de um token.
> * Verifique se o período de tempo, que um token é válido e se um token permanece válido.
> * Definir apropriados períodos de tempo que tokens são válidos para.
> * Atualize tokens expirados.

## <a name="role-based-access-control-best-practices"></a>Práticas recomendadas de controlo de acesso baseado em funções

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre as melhores práticas de IoT do Azure, leia [práticas recomendadas de segurança de IoT](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/).

Para saber mais sobre o controlo de acesso baseado em funções, leia [controlo de acesso baseado em funções](./security-role-based-access-control.md).

Para saber mais sobre autenticação, leia [autenticar com as APIs](./security-authenticating-apis.md).

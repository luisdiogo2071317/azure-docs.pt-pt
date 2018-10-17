---
title: Noções básicas sobre práticas recomendadas de segurança de duplos Digital do Azure | Documentos da Microsoft
description: Os gémeos Digital segurança melhores práticas do Azure
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: adgera
ms.openlocfilehash: 28eb8b5dc0f75b5e031070803d35c8a1ceb1f000
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364347"
---
# <a name="security-best-practices"></a>Melhores práticas de segurança

Segurança de duplos Digital do Azure permite que preciso acesso a recursos específicos e ações no seu gráfico de IoT. Ele faz isso por meio do gerenciamento de função e a permissão granular, chamado de controlo de acesso baseado em funções.

Duplos Digital do Azure também tira partido de outras funcionalidades de segurança presentes no IoT do Azure, incluindo o Azure Active Directory. Por esse motivo, configurar a sua aplicação de duplos Digital do Azure envolve a utilização de muitas das mesmas [práticas de segurança do Azure IoT](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/) recomendado atualmente.

Este artigo resume as principais práticas recomendadas a seguir.

> [!IMPORTANT]
> Reveja os recursos de segurança adicionais (incluindo os fornecedores de dispositivos) para garantir a segurança máximo para o seu espaço de IoT.

## <a name="iot-security-best-practices"></a>Melhores práticas de segurança de IoT

Algumas das principais práticas para proteger os seus dispositivos IoT com segurança incluem:

> [!div class="checklist"]
> * Proteja a cada dispositivo que está ligado ao seu espaço de IoT de uma forma de prova de adulteração.
> * Limite a função de cada pessoa dentro do seu espaço de IoT, sensores e dispositivos. Se comprometido, o impacto seja minimizado.
> * Potencial utilização de filtragem de endereço IP do dispositivo.
> * Limitar largura de banda de e/s e dispositivo para melhorar o desempenho. Limitação de velocidade pode aumentar a segurança, impedindo ataques denial-of-service.

Algumas chaves práticas para proteger a um espaço de IoT com segurança incluem:

> [!div class="checklist"]
> * Encripte dados persistentes, guardados ou armazenados.
> * Exigir palavras-passe ou chaves para ser alterada ou atualizada periodicamente.
> * Restringir cuidadosamente o acesso e permissões por função (consulte as práticas recomendadas do controlo de acesso baseado em funções abaixo).
> * Utilize a encriptação poderosas. Isso significa que a necessidade de senhas longas, usando protocolos seguros e autenticação de dois fatores.

Monitorizar os recursos de IoT para deteção de valores atípicos, ameaças ou parâmetros do recurso que fique fora do intervalo da operação normal é gerido através da análise do Azure.

> [!NOTE]
> Para obter mais informações sobre eventos de processamento e a monitorização, consulte nosso artigo sobre [evento > encaminhamento](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Práticas recomendadas do Active Directory do Azure

Duplos Digital do Azure utiliza o Azure Active Directory para autenticar os utilizadores e proteger as aplicações. O Azure Active Directory suporta a autenticação para uma variedade de modernas arquiteturas, todos eles com base em protocolos de norma da indústria, como o OAuth 2.0 ou OpenID Connect. Alguns principais práticas para proteger o seu espaço de IoT do Azure Active Directory incluem:

> [!div class="checklist"]
> * Store, tais como chaves e segredos de aplicações do Azure Active Directory num local seguro [Key Vault](https://azure.microsoft.com/services/key-vault/).
> * Utilizar um certificado emitido por uma confiança [autoridades de certificação](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started) em vez de segredos de aplicação para autenticar.
> * Limitar o âmbito de OAuth 2.0 de acesso de um token.
> * Verifique se o período de tempo, que um token é válido e se um token permanece válido.
> * Definir apropriados períodos de tempo que tokens são válidos para.
> * Atualize tokens expirados.

## <a name="role-based-access-control-best-practices"></a>Práticas recomendadas de controlo de acesso baseado em funções

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre as melhores práticas de IoT do Azure, leia [práticas recomendadas de segurança de IoT](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/).

Para saber mais sobre o controlo de acesso baseado em funções, leia [controlo de acesso baseado em funções](./security-role-based-access-control.md).

Para a autenticação, lido [autenticar com as APIs](./security-authenticating-apis.md).

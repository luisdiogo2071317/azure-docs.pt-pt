---
title: 2 notas de versão de atualização do serviço de aplicações na pilha do Azure | Microsoft Docs
description: Saiba mais sobre as novidades na atualização dois para o serviço de aplicações na pilha do Azure, os problemas conhecidos e onde pode transferir a atualização.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: 8e1790b7d0b3a210a9142fc8580ff8ed4d64311c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34360395"
---
# <a name="app-service-on-azure-stack-update-2-release-notes"></a>Serviço de aplicações no notas de versão de atualização 2 de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Estas notas de versão descrevem as melhorias e correções no App Service do Azure no Azure pilha Update 2 e a quaisquer problemas conhecidos. Problemas conhecidos são divididos em problemas diretamente relacionada com a implementação, o processo de atualização e a problemas com a compilação (pós-instalação).

> [!IMPORTANT]
> Aplicar a atualização 1804 ao seu sistema de pilha do Azure integrado ou implementar o kit de desenvolvimento de pilha do Azure mais recente antes de implementar 1.2 de serviço de aplicações do Azure.
>
>

## <a name="build-reference"></a>Referência de compilação

O serviço de aplicação no Azure pilha Update 2 número de compilação é **72.0.13698.10**

### <a name="prerequisites"></a>Pré-requisitos

> [!IMPORTANT]
> Novas implementações do serviço de aplicações do Azure na pilha do Azure agora requerem um [certificado de caráter universal de três requerente](azure-stack-app-service-before-you-get-started.md#get-certificates) devido a melhorias na forma no qual o SSO para Kudu agora é processada no App Service do Azure. O requerente nova é  **\*. sso.appservice.\< região\>.\< DomainName\>.\< extensão\>**
>
>

Consulte o [documentação antes de começar a utilizar](azure-stack-app-service-before-you-get-started.md) antes de iniciar a implementação.

### <a name="new-features-and-fixes"></a>Novas funcionalidades e correções

App Service do Azure no Azure pilha Update 2 inclui as seguintes melhorias e correções:

- Atualizações **inquilino de serviço de aplicações, administrador, portais de funções e as ferramentas do Kudu**. Consistente com a versão do SDK de Portal de pilha do Azure.

- Atualizações ao serviço de núcleo para melhorar a fiabilidade e ativar o diagnóstico mais fácil de problemas comuns de mensagens de erro.

- **Atualizações para as seguinte estruturas de aplicações e ferramentas**:
  - Foram adicionadas .net Framework 4.7.1
  - Adicionar **Node.JS** versões:
    - NodeJS 6.12.3
    - NodeJS 8.9.4
    - NodeJS 8.10.0
    - NodeJS 8.11.1
  - Adicionar **NPM** versões:
    - 5.6.0
  - Atualização .net Core componentes para estar consistente com o serviço de aplicações do Azure na nuvem pública.
  - Kudu atualizado

- Ranhuras de troca automática da implementação funcionalidade ativada - [configuração automática de comutação](https://docs.microsoft.com/azure/app-service/web-sites-staged-publishing#configure-auto-swap)

- Testar na funcionalidade de produção ativada - [introdução ao testar na produção](https://azure.microsoft.com/resources/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/)

- Proxies de funções do Azure ativada - [trabalhar com os Proxies de funções do Azure](https://docs.microsoft.com/en-us/azure/azure-functions/functions-proxies)

- Adicionar extensão de administração do serviço de aplicações suportam o UX para:
  - Rotação secreta
  - Rotação de certificado
  - Rotação de credencial de sistema
  - Rotação de cadeia de ligação

### <a name="known-issues-post-installation"></a>Problemas conhecidos (pós-instalação)

- Trabalhadores não conseguem alcançar o servidor de ficheiros quando o serviço de aplicações está implementado na rede virtual existente e o servidor de ficheiros só está disponível na rede privada.

Se optar por implementar uma rede virtual existente e um endereço IP para ligar ao seu servidor de ficheiros, tem de adicionar uma regra de segurança de saída, permitindo o tráfego entre a sub-rede de trabalho e o servidor de ficheiros SMB. Para fazê-lo, aceda a WorkersNsg no Portal de administração e adicionar uma regra de segurança de saída com as seguintes propriedades:
 * Origem: nenhuma
 * Intervalo de portas de origem: *
 * Destino: Endereços IP
 * Intervalo de endereços IP de destino: intervalo de IPs para o servidor de ficheiros
 * Intervalo de portas de destino: 445
 * Protocolo: TCP
 * Ação: permitir
 * Prioridade: 700
 * Nome: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Problemas conhecidos para administradores de nuvem funcionamento do serviço de aplicações do Azure na pilha do Azure

Consulte a documentação do [notas de versão 1804 de pilha do Azure](azure-stack-update-1804.md)

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral do App Service do Azure, consulte [do serviço de aplicações do Azure na descrição geral do Azure pilha](azure-stack-app-service-overview.md).
- Para obter mais informações sobre como preparar a implementação do serviço de aplicações na pilha do Azure, consulte [antes de começar com o serviço de aplicações na pilha de Azure](azure-stack-app-service-before-you-get-started.md).

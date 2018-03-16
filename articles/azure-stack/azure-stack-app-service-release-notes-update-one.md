---
title: "Serviço de aplicações no Azure da pilha Update um | Microsoft Docs"
description: "Saiba mais sobre as novidades na atualização de um para o serviço de aplicações na pilha do Azure, os problemas conhecidos e onde pode transferir a atualização."
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: 0c33c8fdefbb27ba8414e58bed1b42ee7aaba88a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="app-service-on-azure-stack-update-one-release-notes"></a>Um notas de versão de atualização do serviço de aplicações na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Estas notas de versão descrevem as melhorias e correções no App Service do Azure no Azure pilha Update 1 e a quaisquer problemas conhecidos. Problemas conhecidos são divididos em problemas diretamente relacionada com a implementação, o processo de atualização e a problemas com a compilação (pós-instalação).

> [!IMPORTANT]
> Aplicar a atualização 1802 ao seu sistema de pilha do Azure integrado ou implementar o kit de desenvolvimento de pilha do Azure mais recente antes de implementar o serviço de aplicações do Azure.
>
>

## <a name="build-reference"></a>Referência de compilação

O serviço de aplicação no número de compilação de 1 de atualização de pilha do Azure é **69.0.13698.9**

### <a name="prerequisites"></a>Pré-requisitos

> [!IMPORTANT]
> App Service do Azure na pilha do Azure agora requer um [certificado de caráter universal de três requerente](azure-stack-app-service-before-you-get-started.md#get-certificates) devido a melhorias na forma no qual o SSO para Kudu agora é processada no App Service do Azure.  O requerente nova é * * *. sso.appservice.<region>. <domainname>.<extension>**
>
>

Consulte o [documentação antes de começar a utilizar](azure-stack-app-service-before-you-get-started.md) antes de iniciar a implementação.

### <a name="new-features-and-fixes"></a>Novas funcionalidades e correções

App Service do Azure no Azure pilha Update 1 inclui as seguintes melhorias e correções:

- **Elevada disponibilidade do App Service do Azure** -cargas de trabalho do 1802 de pilha do Azure a atualização ativada para ser implementado em domínios de falha.  Por conseguinte, a infraestrutura de serviço de aplicações é capaz de ser tolerante a falhas, será implementado em vários domínios de falhas.  Por predefinição todas as novas implementações do App Service do Azure irão têm esta capacidade, no entanto, para implementações concluídas antes do Azure pilha 1802 atualização que está a ser aplicada, consulte ao [documentação de domínio de falhas do serviço de aplicações](azure-stack-app-service-fault-domain-update.md)

- **Implementar na rede virtual existente** -os clientes podem agora implementar o serviço de aplicações na pilha do Azure dentro de uma rede virtual existente.  Implementação de uma rede virtual existente permite aos clientes ligar ao SQL Server e do servidor de ficheiros, necessário para o App Service do Azure, através das portas privadas.  Durante a implementação de clientes podem optar por implementar numa rede virtual existente, no entanto [tem de criar sub-redes para utilização pelo App Service](azure-stack-app-service-before-you-get-started.md#virtual-network) antes da implementação.

- Atualizações **inquilino de serviço de aplicações, administrador, portais de funções e as ferramentas do Kudu**.  Consistente com a versão do SDK de Portal de pilha do Azure.

- **Atualizações para as seguinte estruturas de aplicações e ferramentas**:
    - Adicionar **.Net Core 2.0** suportar
    - Adicionar **Node.JS** versões:
        - 6.11.2
        - 6.11.5
        - 7.10.1
        - 8.0.0
        - 8.1.4
        - 8.4.0
        - 8.5.0
        - 8.7.0
        - 8.8.1
        - 8.9.0
    - Adicionar **NPM** versões:
        - 3.10.10
        - 4.2.0
        - 5.0.0
        - 5.0.3
        - 5.3.0
        - 5.4.2
        - 5.5.1
    - Adicionar **PHP** atualizações:
        - 5.6.32
        - 7.0.26 (x86 e x64)
        - 7.1.12 (x86 e x64)
    - Atualizado **Git para Windows** v 2.14.1
    - Atualizado **Mercurial** para v4.5.0

  - Foi adicionado suporte para **apenas HTTPS** funcionalidade na funcionalidade do domínio personalizado no Portal de inquilino do serviço de aplicações. 

  - Foram adicionada validação da ligação de armazenamento no selecionador de armazenamento personalizada para as funções do Azure 

#### <a name="fixes"></a>Correções

- Ao criar um pacote de implementação offline, os clientes deixará de receber um acesso negado a mensagem de erro ao abrir a pasta do instalador do serviço de aplicações

- Resolver problemas ao trabalhar na funcionalidade de domínios personalizados no Portal de inquilino do serviço de aplicações.

- Impedir que os clientes utilizando nomes de administrador reservado durante a configuração

- Ativar a implementação de serviço de aplicações com **associado a um domínio** servidor de ficheiros

- Obtenção melhorada de raiz de pilha do Azure no script de certificado e agora validar o certificado de raiz no instalador do serviço de aplicações.

- Estado incorreto fixo que está a ser devolvido para o Azure Resource Manager quando uma subscrição é eliminado que recursos contida no espaço de nomes ' Microsoft. Web.

### <a name="known-issues-with-the-deployment-process"></a>Problemas conhecidos relacionados com o processo de implementação

- Não existem não existem problemas conhecidos para a implementação do serviço de aplicações do Azure no Azure pilha Update 1.

### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos relacionados com o processo de atualização

- Não existem não existem problemas conhecidos para a atualização do serviço de aplicações do Azure no Azure pilha Update 1.

### <a name="known-issues-post-installation"></a>Problemas conhecidos (pós-instalação)

- Não existem não existem problemas conhecidos para a instalação do serviço de aplicações do Azure no Azure pilha Update 1.

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Problemas conhecidos para administradores de nuvem funcionamento do serviço de aplicações do Azure na pilha do Azure

Consulte a documentação do [notas de versão 1802 de pilha do Azure](azure-stack-update-1802.md)

## <a name="see-also"></a>Consulte também

- Para obter uma descrição geral do App Service do Azure, consulte [do serviço de aplicações do Azure na descrição geral do Azure pilha](azure-stack-app-service-overview.md).
- Para obter mais informações sobre como preparar a implementação do serviço de aplicações na pilha do Azure, consulte [antes de começar com o serviço de aplicações na pilha de Azure](azure-stack-app-service-before-you-get-started.md).

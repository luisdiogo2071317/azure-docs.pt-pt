---
title: Integração da empresa para B2B - Azure Logic Apps | Documentos da Microsoft
description: Criar fluxos de trabalho de B2B e oferecer suporte a cenários de integração empresarial para aplicações lógicas com o Enterprise Integration Pack
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: jeconnoc
editor: cgronlun
ms.assetid: dd517c4d-1701-4247-b83c-183c4d8d8aae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: c4f83d2bf082b5f1358b4eee6468d470b5cb8088
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42056981"
---
# <a name="overview-b2b-scenarios-and-communication-with-the-enterprise-integration-pack"></a>Descrição geral: Cenários B2B e comunicação com o Enterprise Integration Pack

Para fluxos de trabalho de empresa-empresa (B2B) e comunicação direta com o Azure Logic Apps, pode ativar cenários de integração empresarial com a solução da Microsoft baseado na nuvem, o Enterprise Integration Pack. As organizações podem trocar mensagens eletronicamente, mesmo que utilizam diferentes protocolos e formatos. O pacote de transforma formatos diferentes num formato que sistemas da organização conseguem interpretar e processar. As organizações podem trocar mensagens através de protocolos de norma da indústria, incluindo [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md), e [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md). Também pode proteger as mensagens com a encriptação e assinaturas digitais.

Se estiver familiarizado com o BizTalk Server ou os serviços BizTalk do Microsoft Azure, são fáceis de usar os recursos de integração empresarial como a maioria dos conceitos são semelhantes. Uma grande diferença é que a integração empresarial utiliza contas de integração para simplificar o armazenamento e gestão de artefactos utilizado nas comunicações B2B. 

Em termos de arquitetura, o Enterprise Integration Pack é baseado em "contas de integração". Estas contas são contentores com base na cloud que armazenam todos os seus artefactos, como esquemas, parceiros, certificados, mapas e contratos. Pode utilizar estes artefactos para conceber, implementar e manter as suas aplicações de B2B e também criar fluxos de trabalho de B2B no logic apps. Mas antes de poder utilizar estes artefactos, primeiro tem de associar sua conta de integração a sua aplicação lógica. Depois disso, a aplicação lógica pode aceder aos artefactos da sua conta de integração.

## <a name="why-should-you-use-enterprise-integration"></a>Por que deve usar de integração empresarial?

* Com integração empresarial, pode armazenar todos os seus artefactos num único local – a sua conta de integração.
* Pode criar fluxos de trabalho de B2B e integrar com aplicações do terceiro software-como-serviço (SaaS), aplicações no local e aplicações personalizadas com o motor do Azure Logic Apps e todos os seus conectores.
* Pode criar o código personalizado para as aplicações lógicas com funções do Azure.

## <a name="how-to-get-started-with-enterprise-integration"></a>Como começar com a integração empresarial?

Pode criar e gerir aplicações B2B com Enterprise Integration Pack, através do Estruturador da aplicação lógica no **portal do Azure**. Também pode gerir as aplicações lógicas com [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.logicapp "PowerShell do Logic apps").

Aqui estão as etapas de alto nível que deve tomar antes de poder criar aplicações no portal do Azure:

![imagem de descrição geral](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Quais são alguns cenários comuns?

Integração empresarial suporta estes padrões da indústria:

* EDI - intercâmbio eletrônico de dados
* EAI - integração de aplicações empresariais

## <a name="heres-what-you-need-to-get-started"></a>Aqui está o que precisa para começar a utilizar

* Uma subscrição do Azure com uma conta de integração
* Visual Studio 2015, para criar mapas e esquemas
* [Ferramentas de integração do Microsoft Azure Logic Apps Enterprise para o Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a>Experimentar agora

[Implementar o envio de AS2 de exemplo totalmente operacional e receber a aplicação lógica](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) que utiliza as funcionalidades de B2B para o Azure Logic Apps.

## <a name="learn-more"></a>Saiba mais
* [Contratos](../logic-apps/logic-apps-enterprise-integration-agreements.md "Saiba mais sobre os contratos de integração do enterprise")
* [Cenários do empresa-empresa (B2B)](../logic-apps/logic-apps-enterprise-integration-b2b.md "Saiba como criar aplicações lógicas com funcionalidades de B2B ")  
* [Certificados](logic-apps-enterprise-integration-certificates.md "Saiba mais sobre certificados de integração do enterprise")
* [Simples de arquivo codificando/decodificando](logic-apps-enterprise-integration-flatfile.md "Aprenda a codificar e decodificar o conteúdo do ficheiro simples")  
* [Contas de integração](../logic-apps/logic-apps-enterprise-integration-accounts.md "Saiba mais sobre contas de integração")
* [Mapeia](../logic-apps/logic-apps-enterprise-integration-maps.md "Saiba mais sobre o maps de integração do enterprise")
* [Parceiros](logic-apps-enterprise-integration-partners.md "Saiba mais sobre os parceiros de integração empresarial")
* [Esquemas](logic-apps-enterprise-integration-schemas.md "Saiba mais sobre esquemas de integração do enterprise")
* [Validação XML da mensagem](logic-apps-enterprise-integration-xml.md "aprender como validá mensagens XML com o Logic apps")
* [Transformação XML](logic-apps-enterprise-integration-transform.md "Saiba mais sobre o maps de integração do enterprise")
* [Conectores de integração empresarial](../connectors/apis-list.md "Saiba mais sobre os conectores do enterprise integration pack")
* [Metadados de conta de integração](../logic-apps/logic-apps-enterprise-integration-metadata.md "Saiba mais sobre metadados de conta de integração")
* [Monitorizar mensagens B2B](logic-apps-monitor-b2b-message.md "Saiba mais sobre como monitorizar mensagens B2B")
* [Controlar mensagens B2B no portal do OMS](logic-apps-track-b2b-messages-omsportal.md "Saiba mais sobre o controlo de mensagens B2B no portal do OMS")


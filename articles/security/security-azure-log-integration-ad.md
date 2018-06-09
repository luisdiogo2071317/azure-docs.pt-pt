---
title: Integração de registo do Azure com os registos de auditoria do Azure Active Directory | Microsoft Docs
description: Saiba como instalar o serviço de integração de registo do Azure e integrar os registos de registos de auditoria do Azure
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 06/07/2018
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 07299b960747528dde8dcefc70055442c2150486
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235996"
---
# <a name="integrate-azure-active-directory-audit-logs"></a>Integrar os registos de auditoria do Azure Active Directory

Eventos de auditoria do Azure Active Directory (Azure AD) ajudam a identificar ações privilegiadas ocorridas no Azure Active Directory. Pode ver os tipos de eventos que pode controlar revendo [eventos de relatório de auditoria do Azure Active Directory](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).


>[!IMPORTANT]
> 01/06/2019 será preterida a funcionalidade de integração de registo do Azure. Transferências de AzLog serão desativadas pelo 27 de Jun de 2018. Para obter orientações sobre o que fazer mover revisão reencaminhar a mensagem [monitor de utilização Azure integrar ferramentas SIEM](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a>Passos para integrar o Azure Active Directory de registos de auditoria

> [!NOTE]
> Antes de tentar os passos neste artigo, tem de consultar o [começar](security-azure-log-integration-get-started.md) artigo e conclua os passos relevantes não existe.

1. Abra a linha de comandos e execute este comando:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Execute este comando: 
 
   ``azlog createazureid``

   Este comando pede-lhe o início de sessão do Azure. O comando, em seguida, cria um Azure Active Directory principal de serviço em inquilinos do Azure AD que alojam as subscrições do Azure na qual o utilizador com sessão iniciada é um administrador, coadministrador ou um proprietário. O comando irá falhar se o utilizador com sessão iniciada é apenas um utilizador convidado no inquilino do Azure AD. Autenticação do Azure é feita através do Azure AD. Criar um principal de serviço para a integração de registo do Azure cria a identidade do Azure AD, que é dado acesso ao ler a partir de subscrições do Azure.

3. Execute o seguinte comando para fornecer o ID do inquilino. Tem de ser membro da função de administrador inquilino para executar o comando.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Exemplo:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Verifique as seguintes pastas para confirmar que os ficheiros de JSON do registo de auditoria do Azure Active Directory são criados nos mesmos:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

O vídeo seguinte demonstra os passos abordados neste artigo:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> Para obter instruções específicas sobre colocar as informações dos ficheiros JSON em sistemas security information and system management (SIEM) de eventos, contacte o fabricante do SIEM.

A assistência de Comunidade está disponível através de [fórum MSDN do Azure registo integração](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Neste fórum permite que as pessoas da Comunidade a integração de registo do Azure para suportar entre si com perguntas, respostas, sugestões e truques. Além disso, a equipa de integração de registo do Azure monitoriza neste fórum e ajuda a sempre que possível.

Também pode abrir um [pedido de suporte](../azure-supportability/how-to-create-azure-support-request.md). Selecione **integração de registo** como o serviço para o qual está a pedir suporte.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre a integração de registo do Azure, consulte:

* [Integração de registo do Microsoft Azure para os registos do Azure](https://www.microsoft.com/download/details.aspx?id=53324): página do Centro de transferências este fornece detalhes, requisitos de sistema e as instruções de instalação para a integração de registo do Azure.
* [Introdução ao Azure registo integração](security-azure-log-integration-overview.md): Este artigo apresenta-lhe a integração de registo do Azure, as suas capacidades principais e como funciona.
* [FAQ sobre integração do registo do Azure](security-azure-log-integration-faq.md): Este artigo responde a questões sobre a integração de registo do Azure.
* [Registos de auditoria de novas funcionalidades de diagnóstico do Azure e Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): esta mensagem de blogue apresenta-lhe os registos de auditoria do Azure e outras funcionalidades que o ajudam a obterem informações sobre as operações dos seus recursos Azure.

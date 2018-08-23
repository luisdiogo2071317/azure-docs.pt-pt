---
title: Resolução de problemas de atividade do Active Directory do Azure regista erros de pacote de conteúdos | Documentos da Microsoft
description: Fornece uma lista de mensagens de erro do pacote de conteúdos de atividade do Active Directory do Azure e nos passos para corrigi-los.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: eafbe25a5a0fa9182030304e9142a6013c9fb29b
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060933"
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Resolução de problemas de atividade do Active Directory do Azure regista erros de pacote de conteúdos 


Ao trabalhar com o pacote de conteúdos do Power BI para a pré-visualização do Azure Active Directory, é possível que se deparar com os seguintes erros: 

- [Falha na atualização](troubleshoot-content-pack.md#refresh-failed) 
- [Falha ao atualizar as credenciais de origem de dados](troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Importação de dados está a demorar demasiado tempo](troubleshoot-content-pack.md#importing-of-data-is-taking-too-long) 
 
Este artigo fornece informações sobre as possíveis causas e como corrigir estes erros.
 
## <a name="refresh-failed"></a>Falha na atualização 
 
**Como este erro aparece**: E-Mail a partir do Power BI ou no histórico de atualização do status de falha. 


| Causa | Como corrigir |
| ---   | ---        |
| Falha de erros podem ser provocados quando as credenciais dos utilizadores a ligar ao pacote de conteúdos tem sido repor mas não atualizadas nas definições de ligação do pacote de conteúdos de atualização. | No Power BI, localize o conjunto de dados correspondente para o dashboard de registos de atividade do Active Directory do Azure (registos de atividade do Active Directory do Azure), escolha a atualização de agenda e, em seguida, introduza as credenciais do Azure AD. |
| Uma atualização pode falhar devido a problemas de dados no pacote de conteúdos subjacente. | Enviar um pedido de suporte. Para obter mais informações, consulte [como obter suporte do Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Falha ao atualizar as credenciais de origem de dados 
 
**Como este erro aparece**: no Power BI, quando se liga para o pacote de conteúdos de registos (pré-visualização) de atividade do Active Directory do Azure. 

| Causa | Como corrigir |
| ---   | ---        |
| O utilizador de ligação não é um administrador global ou um leitor de segurança ou administrador de segurança. | Utilize uma conta que seja um administrador global ou um leitor de segurança ou um administrador de segurança para aceder os pacotes de conteúdos. |
| O inquilino não é um inquilino de Premium ou não tem pelo menos um utilizador com o ficheiro de licença de Premium. | Enviar um pedido de suporte. Para obter mais informações, consulte [como obter suporte do Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 

 

## <a name="importing-of-data-is-taking-too-long"></a>Importação de dados está a demorar demasiado tempo 
 
**Como este erro aparece**: no Power BI, depois de ter ligado o pacote de conteúdos, o processo de importação de dados começa a preparar o seu dashboard para o registo de atividade do Active Directory do Azure. Verá a mensagem: "*importação de dados...* "  

| Causa | Como corrigir |
| ---   | ---        |
| Dependendo do tamanho do seu inquilino, este passo pode demorar entre alguns minutos até a 30 minutos. | Seja paciente. Se a mensagem não é alterado para mostrar o seu dashboard dentro de uma hora, submeta um pedido de suporte. Para obter mais informações, consulte [como obter suporte do Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Passos Seguintes

Para instalar o pacote de conteúdos do Power BI para a pré-visualização do Azure Active Directory, clique em [aqui](https://powerbi.microsoft.com/blog/azure-active-directory-meets-power-bi/).



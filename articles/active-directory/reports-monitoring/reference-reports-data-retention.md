---
title: Políticas de retenção de relatórios do Azure Active Directory | Documentos da Microsoft
description: Políticas de retenção de dados de relatórios no Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: report-monitor
ms.date: 05/10/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 68028fd1ba116251860e5c370e9e9ce61fd314bb
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42056914"
---
# <a name="azure-active-directory-report-retention-policies"></a>Políticas de retenção de relatórios do Azure Active Directory


Este artigo fornece respostas às perguntas mais comuns em conjunto com a retenção de dados para os relatórios de atividade diferente no Azure Active Directory. 

### <a name="q-how-can-you-get-the-collection-of-activity-data-started"></a>P: como pode obter a recolha de dados de atividade iniciados?

**R:**

| Edição do Azure AD | Início da coleção |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Quando se inscreve para uma subscrição |
| Azure AD Gratuito | A primeira vez que abre o [painel Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) ou utilizar o [APIs de relatórios](https://aka.ms/aadreports)  |

---
### <a name="q-when-is-your-activity-data-available-in-the-azure-portal"></a>P: quando é que os dados de atividade disponível no portal do Azure?

**R:**

- **Imediatamente** – se já estiver trabalhando com relatórios no portal do Azure.
- **Dentro de 2 horas** - se de que ainda não ativou a criação de relatórios no portal do Azure.

---

### <a name="q-how-can-you-get-the-collection-of-security-signals-started"></a>P: como pode obter a coleção de sinais de segurança a utilizar?  

**R:** para sinais de segurança, o processo de coleta é iniciado quando participar para utilizar o Centro de proteção de identidade. 


---

### <a name="q-for-how-long-is-the-collected-data-stored"></a>P: durante quanto tempo são os dados recolhidos armazenados?

**R:**

**Relatórios de atividade**    

| Relatório                 | Azure AD Gratuito | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Auditoria de Diretórios        | 7 dias        | 30 dias             | 30 dias             |
| Atividade de Início de Sessão       | N/A           | 30 dias             | 30 dias             |
| Utilização MFA do Azure        | 30 dias       | 30 dias             | 30 dias             |

**Sinais de segurança**

| Relatório         | Azure AD Gratuito | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Utilizadores em risco  | 7 dias        | 30 dias             | 90 dias             |
| Inícios de sessão de risco | 7 dias        | 30 dias             | 90 dias             |

---

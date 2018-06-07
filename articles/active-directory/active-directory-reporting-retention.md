---
title: Políticas de retenção de relatórios do Azure Active Directory | Microsoft Docs
description: Políticas de retenção de dados do relatório no Azure Active Directory
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/10/2018
ms.author: rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: fac160d2d5916097afcbb9825bb82a52789e4a89
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589200"
---
# <a name="azure-active-directory-report-retention-policies"></a>Políticas de retenção de relatórios do Azure Active Directory


Este tópico fornece respostas às perguntas mais comuns em conjunto com a retenção de dados para os relatórios de atividade diferente no Azure Active Directory. 

### <a name="q-how-can-you-get-the-collection-of-activity-data-started"></a>P: como é pode obter a recolha de dados de atividade iniciadas?

**R:**

| Edição do Azure AD | Início da coleção |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Quando a inscrição para uma subscrição |
| Azure AD Gratuito | Na primeira vez que abrir o [painel do Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) ou utilize o [APIs de relatórios](https://aka.ms/aadreports)  |

---
### <a name="q-when-is-your-activity-data-available-in-the-azure-portal"></a>P: quando os dados de atividade está disponível no portal do Azure?

**R:**

- **Imediatamente** - se de que tem estado a trabalhar com relatórios no portal do Azure
- **Dentro de 2 horas** - se de que ainda não ativou o Reporting Services no portal do Azure

---

### <a name="q-how-can-you-get-the-collection-of-security-signals-started"></a>P: como é pode obter a coleção de sinais de segurança iniciada?  

**R:** para sinais de segurança, o processo de recolha é iniciado quando que optar ativamente por participar sessão para utilizar o Centro de proteção de identidade. 


---

### <a name="q-for-how-long-is-the-collected-data-stored"></a>P: por quanto estão os dados recolhidos armazenados?

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

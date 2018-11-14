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
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 601169cc62a99438f661adc06ab166b545606edb
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624595"
---
# <a name="azure-active-directory-report-retention-policies"></a>Políticas de retenção de relatórios do Azure Active Directory

Neste artigo, ficará a conhecer as políticas de retenção de dados para os relatórios de atividade diferente no Azure Active Directory. 

### <a name="when-does-azure-ad-start-collecting-data"></a>Quando é que o Azure AD iniciar a recolha de dados?

| Edição do Azure AD | Início da coleção |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Quando se inscreve para uma subscrição |
| Azure AD Gratuito | A primeira vez que abre o [painel Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) ou utilizar o [APIs de relatórios](https://aka.ms/aadreports)  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>Quando os dados de atividade está disponível no portal do Azure?

- **Imediatamente** – se já estiver trabalhando com relatórios no portal do Azure.
- **Dentro de 2 horas** - se de que ainda não ativou a criação de relatórios no portal do Azure.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>Quando é que o Azure AD comece a recolher dados de sinal de segurança?  

Para sinais de segurança, o processo de coleta é iniciado quando participar para utilizar o **Centro de proteção de identidade**. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>O período de tempo é que o Azure AD armazena os dados?

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

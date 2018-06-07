---
title: Auditoria de Active Directory do Azure referência da API | Microsoft Docs
description: Como começar com a API de auditoria do Azure Active Directory
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 44e46be8-09e5-4981-be2b-d474aaa92792
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/08/2018
ms.author: dhanyahk;rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: 44bad584bec6f9dc39ea2f7ed982396819aea90c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34588452"
---
# <a name="azure-active-directory-audit-api-reference"></a>Auditoria de Active Directory referência da API do Azure

> [!TIP] 
> Conheça a nova Microsoft Graph API para [reporting](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit), que irá substituir eventualmente esta API. 


Este artigo é parte de uma coleção de artigos sobre o Azure Active Directory (Azure AD), API do relatório. Relatórios do Azure AD fornecem-lhe uma API que permite-lhe aceder a dados de auditoria utilizando código ou ferramentas relacionadas.
O âmbito deste artigo é para lhe fornecer informações de referência sobre o **API de auditoria**.

Veja:

* [Registos de auditoria](active-directory-reporting-azure-portal.md#activity-reports) para obter mais informações concetuais

* [Começar a utilizar a API do Azure Active Directory Reporting](active-directory-reporting-api-getting-started.md) para obter mais informações sobre a API de relatórios.


Para:

- Perguntas mais frequentes, leia o [FAQ](active-directory-reporting-faq.md) 

- Problemas, [um pedido de suporte de ficheiros](active-directory-troubleshooting-support-howto.md) 


## <a name="who-can-access-the-data"></a>Quem pode aceder aos dados?
* Utilizadores na função de Administrador de Segurança ou de Leitor de Segurança
* Administradores Globais
* Qualquer aplicação que tenha autorização para aceder à API do (autorização da aplicação pode ser configurada apenas com base nas permissões de Administrador Global)

## <a name="prerequisites"></a>Pré-requisitos
Para aceder a este relatório através da API de relatórios, tem de ter:

* Um [gratuito do Azure Active Directory ou a edição melhor](active-directory-whatis.md)
* Concluída a [pré-requisitos para o Azure AD API do relatório de acesso](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-the-api"></a>Aceder à API
Se pode aceder a esta API através de [gráfico Explorer](https://graphexplorer2.cloudapp.net) ou através de programação utilizando, por exemplo, do PowerShell. Utilize o backtick (aka: grave acentos) caráter para "" $ como caráter de escape para se certificar de que o PowerShell pode interpretar a sintaxe de filtro de OData utilizada nas chamadas REST de gráfico do AAD. O caráter de backtick serve como [caráter de escape do PowerShell](https://technet.microsoft.com/library/hh847755.aspx), permitindo que o PowerShell para fazer uma interpretação literal do caráter $ e evitar confusa-lo como um nome de variável do PowerShell (por exemplo, $filter).

O foco deste artigo é o Explorador do gráfico. Para obter um exemplo do PowerShell, consulte este [script do PowerShell](active-directory-reporting-api-audit-samples.md#powershell-script).

## <a name="api-endpoint"></a>Ponto final de API

Pode aceder a esta API utilizando o URI seguinte:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Não há nenhum limite para o número de registos que são devolvidos pela API de auditoria do Azure AD (utilizando a paginação de OData). Para os limites de retenção de dados de relatórios, consulte [relatórios de políticas de retenção](active-directory-reporting-retention.md).

A chamada devolve os dados em lotes. Cada lote tem um máximo de 1000 registos. Para obter o lote seguinte de registos, utilize o **seguinte** ligação. Obter as informações de token skip do primeiro conjunto de registos devolvidos. O token skip será no final do resultado definido.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Filtros suportados

Pode reduzir o número de registos que são devolvidos por uma chamada de API com um filtro.  
Para início de sessão relacionadas com a API dados, são suportados os seguintes filtros:

* **$top =\<número de registos devem ser devolvidos\>**  - para limitar o número de registos devolvidos. Esta é uma operação dispendiosa. Não utilize este filtro, se pretender regressar a milhares de objetos.     
* **$filter =\<sua declaração de filtro\>**  - especificar, on the basis of campos de filtro suportado, o tipo de registos que mais lhe interessam

## <a name="supported-filter-fields-and-operators"></a>Operadores e campos de filtro suportado
Para especificar o tipo de registos que mais lhe interessam, pode criar uma instrução de filtro com um ou uma combinação dos seguintes campos de filtro:

* [activityDate](#activitydate) -define uma data ou um intervalo de datas
* [categoria](#category) -define a categoria que pretende filtrar.
* [activityStatus](#activitystatus) -define o estado de uma atividade
* [activityType](#activitytype) -define o tipo de uma atividade
* [atividade](#activity) -define a atividade como cadeia  
* [nome doactor/](#actorname) -define o ator no formato de nome do actor
* [ator/objectid](#actorobjectid) -define o ator no formato de ID de actor   
* [ator/upn](#actorupn) -define o ator no formato de nome de princípio de ator utilizador (UPN) 
* [nome do destino](#targetname) -define o destino no formato de nome do actor
* [destino/objectid](#targetobjectid) -define o destino no formato de ID de destino  
* [destino/upn](#targetupn) -define o ator no formato de nome de princípio de ator utilizador (UPN)   

- - -
### <a name="activitydate"></a>activityDate
**Suportado operadores**: eq, ge, le, gt, lt

**Exemplo**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

**Notas**:

DateTime deve estar no formato UTC

- - -
### <a name="category"></a>categoria

**Valores suportados**:

| Categoria                         | Valor     |
| :--                              | ---       |
| Diretório do Núcleo                   | Diretório |
| Gestão de Palavra-passe Personalizada | SSPR      |
| Gestão de Grupos Personalizada    | SSGM      |
| Aprovisionamento de Contas             | Sync      |
| Rollover de Palavra-passe Automatizada      | Rollover de Palavra-passe Automatizada |
| Identity Protection              | IdentityProtection |
| Utilizadores Convidados                    | Utilizadores Convidados |
| Serviço MIM                      | Serviço MIM |



**Suportado operadores**: eq

**Exemplo**:

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a>ActivityStatus

**Valores suportados**:

| Estado da Atividade | Valor |
| :--             | ---   |
| Êxito         | 0     |
| Falha         | - 1   |

**Suportado operadores**: eq

**Exemplo**:

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a>activityType
**Suportado operadores**: eq

**Exemplo**:

    $filter=activityType eq 'User'    

**Notas**:

maiúsculas e minúsculas

- - -
### <a name="activity"></a>atividade
**Suportado operadores**: eq, contém, startsWith

**Exemplo**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

**Notas**:

maiúsculas e minúsculas

- - -
### <a name="actorname"></a>nome do actor /
**Suportado operadores**: eq, contém, startsWith

**Exemplo**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

**Notas**:

Sensível

- - -
### <a name="actorobjectid"></a>ator/objectId
**Suportado operadores**: eq

**Exemplo**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a>nome do destino
**Suportado operadores**: eq, contém, startsWith

**Exemplo**:

    $filter=targets/any(t: t/name eq 'some name')    

**Notas**:

Sensível

- - -
### <a name="targetupn"></a>destino/upn
**Suportado operadores**: eq, startsWith

**Exemplo**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

**Notas**:

* Sensível
* Adicione o espaço de nomes completo ao consultar Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

- - -
### <a name="targetobjectid"></a>target/objectId
**Suportado operadores**: eq

**Exemplo**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a>ator/upn
**Suportado operadores**: eq, startsWith

**Exemplo**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

**Notas**:

* Sensível 
* Adicione o espaço de nomes completo ao consultar Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

- - -
## <a name="next-steps"></a>Próximos Passos

- Pretende ver exemplos para atividades de sistema filtrado? Veja o [amostras de API de auditoria do Azure Active Directory](active-directory-reporting-api-audit-samples.md).

- Pretende saber mais sobre o Azure AD API do relatório? Consulte [começar a utilizar a API do Azure Active Directory Reporting](active-directory-reporting-api-getting-started.md).


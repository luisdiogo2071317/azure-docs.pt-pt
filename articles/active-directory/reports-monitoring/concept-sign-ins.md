---
title: Relatórios de atividade de início de sessão no portal do Azure Active Directory | Microsoft Docs
description: Introdução aos relatórios de atividade de início de sessão no portal do Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: ae962cba5e3d08661eb1c93edfc2feb221a9367e
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51623797"
---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a>Relatórios de atividade de início de sessão no portal do Azure Active Directory

A arquitetura de relatórios no Azure Active Directory (Azure AD) é composta pelos seguintes componentes:

- **Atividade** 
    - **Inícios de sessão** – informações sobre a utilização de aplicações geridas e atividades de início de sessão de utilizador.
    - **Registos de auditoria** - [registos de auditoria](concept-audit-logs.md) fornecem informações de atividade do sistema sobre utilizadores e gestão de grupos, aplicações geridas e atividades de diretório.
- **Segurança** 
    - **Inícios de sessão arriscados** - uma [o início de sessão arriscado](concept-risky-sign-ins.md) é um indicador de uma tentativa de início de sessão poderão ter sido executada por alguém que não é o proprietário legítimo de uma conta de utilizador.
    - **Utilizadores sinalizados para risco** - uma [utilizador de risco](concept-user-at-risk.md) é um indicador de uma conta de utilizador que pode ter sido comprometida.

Este tópico fornece-lhe uma descrição geral do relatório de inícios de sessão.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="who-can-access-the-data"></a>Quem pode aceder aos dados?
* Utilizadores nas funções de administrador de segurança, o leitor de segurança e o leitor de relatório
* Administradores globais
* Além disso, qualquer utilizador (não administrador) pode aceder aos seus próprios inícios de sessão 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>Que licença do Azure AD precisa para aceder à atividade de entrada?
* O seu inquilino deve ter uma licença do Azure AD Premium associada ao mesmo para ver o relatório de atividade completo de inícios de sessão

## <a name="sign-ins-report"></a>Relatório de inícios de sessão

O relatório de inícios de sessão de utilizador fornece respostas às seguintes perguntas:

* O que é o padrão de início de sessão de um utilizador?
* Quantos utilizadores iniciaram sessão ao longo de uma semana?
* Qual é o estado destes inícios de sessão?

Pode acessar o relatório de inícios de sessão, selecionando **inícios de sessão** no **atividade** secção do **Azure Active Directory** painel no [portal do Azure](https://portal.azure.com).

![Atividade de início de sessão](./media/concept-sign-ins/61.png "Atividade de início de sessão")

Um registo de inícios de sessão tem uma vista de listas predefinidas que mostra:

- A data de início de sessão
- O utilizador relacionado
- A aplicação em que o utilizador iniciou sessão
- O estado de início de sessão
- O estado da deteção de risco
- O estado do requisito de autenticação multifator (MFA)

![Atividade de início de sessão](./media/concept-sign-ins/01.png "Atividade de início de sessão")

Pode personalizar a vista de lista ao clicar em **Colunas** na barra de ferramentas.

![Atividade de início de sessão](./media/concept-sign-ins/19.png "Atividade de início de sessão")

Isto permite-lhe apresentar campos adicionais ou remover campos que já são apresentados.

![Atividade de início de sessão](./media/concept-sign-ins/02.png "Atividade de início de sessão")

Selecione um item na vista de lista para obter informações mais detalhadas.

![Atividade de início de sessão](./media/concept-sign-ins/03.png "Atividade de início de sessão")

> [!NOTE]
> Os clientes agora podem resolver problemas de políticas de acesso condicional através de todos os relatórios de início de sessão. Ao clicar no **acesso condicional** separador para um registo de início de sessão, os clientes podem rever o estado de acesso condicional e informações detalhadas sobre os detalhes das políticas que são aplicadas para o início de sessão e o resultado para cada política.
> Para obter mais informações, consulte a [perguntas mais frequentes sobre as informações de AC em todos os inícios](reports-faq.md#conditional-access).

![Atividade de início de sessão](./media/concept-sign-ins/ConditionalAccess.png "Atividade de início de sessão")


## <a name="filter-sign-in-activities"></a>Filtrar atividades de início de sessão

Para limitar os dados comunicados para um nível que funcione para si, pode filtrar os dados dos inícios de sessão através dos seguintes campos predefinidos:

- Utilizador
- Aplicação
- Estado de início de sessão
- Acesso Condicional
- Date

![Atividade de início de sessão](./media/concept-sign-ins/04.png "Atividade de início de sessão")

O filtro **Utilizador** permite-lhe especificar o nome ou o nome principal de utilizador (UPN) do utilizador que mais lhe interessa.

O filtro **Aplicação** permite-lhe especificar o nome da aplicação que mais lhe interessa.

O filtro **Estado do início de sessão** permite-lhe selecionar:

- Todos
- Êxito
- Falha

O **acesso condicional** filtro permite-lhe selecionar o estado de política de AC para o início de sessão:

- Todos
- Não Aplicado
- Êxito
- Falha

O filtro **Data** permite-lhe definir um período de tempo para os dados devolvidos.  
Os valores possíveis são:

- 1 mês
- 7 dias
- 24 horas
- Intervalo de tempo personalizado

Quando selecionar um período de tempo personalizado, pode configurar uma hora de início e uma hora de fim.

Se adicionar mais campos à vista de inícios de sessão, estes campos são adicionados automaticamente à lista de filtros. Por exemplo, ao adicionar o campo **Aplicação Cliente** à sua lista, também obtém outra opção de filtro que lhe permite definir os seguintes filtros:

- Browser      
- Exchange ActiveSync (suportado)               
- Exchange ActiveSync (não suportado)
- Outros clientes               
    - IMAP
    - MAPI
    - Clientes mais antigos do Office
    - POP
    - SMTP


![Atividade de início de sessão](./media/concept-sign-ins/12.png "Atividade de início de sessão")


## <a name="download-sign-in-activities"></a>Transferir atividades de início de sessão

Pode [transferir os dados de inícios de sessão](quickstart-download-sign-in-report.md) se quiser trabalhar com eles fora do portal do Azure. Ao clicar em **Transferir** cria um ficheiro CSV dos 5000 registos mais recentes.  Além de um botão de transferência, o portal do Azure também fornece uma opção para [gerar um script para transferir os seus dados](tutorial-signin-logs-download-script.md).  

![Transferir](./media/concept-sign-ins/71.png "Transferir")

Se necessita de mais flexibilidade, pode utilizar a solução de script. Clicar **Script** cria um script do PowerShell que inclui todos os filtros que definiu. Transfira e execute este script numa **modo de administrador** para gerar o ficheiro CSV. 

> [!IMPORTANT]
> O número de registos, pode transferir é restrita pela [políticas de retenção de relatórios do Azure Active Directory](reference-reports-data-retention.md).  

### <a name="running-the-script-on-a-windows-10-machine"></a>Executar o script num computador Windows 10

Se quiser executar o script num **Windows 10** máquina, precisa executar algumas etapas adicionais primeiro. 

1. Instalar o [módulo do AzureRM](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.4.0l).
2. Importe o módulo ao abrir uma linha de comandos do PowerShell e executar o comando **Import-Module AzureRM**.
3. Execute **Set-ExecutionPolicy irrestrito** e escolha **Sim para todos**. 
4. Agora pode executar o script do PowerShell transferido no modo de administrador para gerar o ficheiro CSV.

## <a name="sign-ins-data-shortcuts"></a>Atalhos de dados de inícios de sessão

Para além do Azure AD, o portal do Azure fornece pontos de entrada adicionais para dados de inícios de sessão:

- A descrição de geral de proteção de segurança de identidade
- Utilizadores
- Grupos
- Aplicações Empresariais

### <a name="users-sign-ins-data-in-identity-security-protection"></a>Dados de inícios de sessão de utilizadores na proteção de segurança de identidade

O gráfico de início de sessão do utilizador no **proteção de segurança de identidade** página Visão Geral mostra as agregações semanais de início de sessão ins para todos os utilizadores num determinado período de tempo. A predefinição do período de tempo é 30 dias.

![Atividade de início de sessão](./media/concept-sign-ins/06.png "Atividade de início de sessão")

Quando clica num dia no gráfico de início de sessão, obtém uma descrição geral das atividades de início de sessão para este dia.

Cada linha na lista de atividades de início de sessão mostra:

* Quem iniciou sessão?
* Que aplicação foi o destino do início de sessão?
* Qual o estado do início de sessão?
* Qual o estado MFA do início de sessão?

Ao clicar num item, obtém mais detalhes sobre a operação de início de sessão:

- ID de Utilizador
- Utilizador
- Nome de utilizador
- ID da aplicação
- Aplicação
- Cliente
- Localização
- Endereço IP
- Date
- MFA Necessário
- Estado de início de sessão
 
Na página **Utilizadores**, pode obter uma descrição geral completa de todos os inícios de sessão dos utilizadores ao clicar em **Inícios de sessão** na secção **Atividade**.

![Atividade de início de sessão](./media/concept-sign-ins/08.png "Atividade de início de sessão")

## <a name="usage-of-managed-applications"></a>Utilização de aplicações geridas

Com uma vista centrada em aplicações dos seus dados de início de sessão, poderá responder a perguntas como:

* Quem está a utilizar as minhas aplicações?
* Quais são as três aplicações mais utilizadas na sua organização?
* Lancei recentemente uma aplicação. Como está a correr?

O ponto de entrada para estes dados são as três aplicações mais utilizadas na sua organização no relatório dos últimos 30 dias na secção **Descrição geral** em **Aplicações empresariais**.

![Atividade de início de sessão](./media/concept-sign-ins/10.png "Atividade de início de sessão")

As agregações semanais de inícios de sessão do gráfico de utilização da aplicação das três aplicações mais utilizadas num determinado período de tempo. A predefinição do período de tempo é 30 dias.

![Atividade de início de sessão](./media/concept-sign-ins/47.png "Atividade de início de sessão")

Se quiser, pode colocar o foco numa aplicação específica.

![Relatórios](./media/concept-sign-ins/single_spp_usage_graph.png "Relatórios")

Quando clica num dia no gráfico de utilização da aplicação, obtém uma lista detalhada das atividades de início de sessão.

A opção **Inícios de sessão** dá uma visão geral completa de todos os eventos de início de sessão nas suas aplicações.

![Atividade de início de sessão](./media/concept-sign-ins/11.png "Atividade de início de sessão")

## <a name="next-steps"></a>Passos Seguintes

* [Códigos de erro de relatório de atividades de início de sessão](reference-sign-ins-error-codes.md)
* [Políticas de retenção de dados do Azure AD](reference-reports-data-retention.md)
* [Latências de relatórios do Azure AD](reference-reports-latencies.md)


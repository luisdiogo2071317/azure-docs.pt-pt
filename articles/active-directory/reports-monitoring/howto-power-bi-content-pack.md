---
title: Como utilizar o Pacote de Conteúdos do Power BI para o Azure Active Directory | Microsoft Docs
description: Saiba como utilizar o Pacote de Conteúdos do Power BI Para o Azure Active Directory
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d810b8be496bbfd1c5bd88d8221e77dd5b76c889
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622975"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Como utilizar o pacote de conteúdos do Azure Active Directory Power BI

|  |
|--|
|Atualmente, o pacote de conteúdos do Power BI para o Azure AD utiliza as APIs do Azure AD Graph para obter dados do seu inquilino do Azure AD. Como resultado, poderá observar alguma disparidade entre os dados disponíveis no pacote de conteúdos e os dados obtidos com as [APIs do Microsoft Graph para relatórios](concept-reporting-api.md). |
|  |

O Power BI conteúdo de pacote para o Azure Active Directory (Azure AD) contém relatórios criados previamente para o ajudar a compreender como os utilizadores adotam e utilizam as funcionalidades do Azure AD. Isto permite-lhe obter informações sobre todas as atividades em seu diretório, com a experiência Rica visualização no Power BI. Também pode criar seu próprio dashboard e partilhá-lo com qualquer pessoa na sua organização. 

## <a name="prerequisites"></a>Pré-requisitos

Precisa de uma licença de premium (P1/P2) do Azure AD para utilizar o pacote de conteúdos. 

## <a name="install-the-content-pack"></a>Instalar o pacote de conteúdos

Veja a [guia de introdução](quickstart-install-power-bi-content-pack.md) para instalar o pacote de conteúdos do Azure AD Power BI.

### <a name="reports-included-in-this-version-of-azure-ad-logs-content-pack"></a>Pacote de conteúdos de registos de relatórios incluídos nesta versão do Azure AD

Os seguintes relatórios estão incluídos no pacote de conteúdos do Azure AD Power BI. Os relatórios contêm dados a partir da **últimos 30 dias**.

**Relatório de utilização da aplicação e as tendências**: este relatório fornece informações sobre as aplicações utilizadas na sua organização. Pode obter uma lista dos aplicativos mais populares ou compreender como uma aplicação que tenha implementado recentemente na sua organização está a ser utilizada. Isto permite-lhe controlar e melhorar a utilização ao longo do tempo.

**Inícios de sessão por localização e utilizadores**: este relatório fornece dados sobre todos os inícios de sessão executados usando a identidade do Azure. Com este relatório, pode desagregar para inícios de sessão individuais e responder a perguntas como:

- Onde este utilizador inícios de sessão de?
- Que utilizador iniciou sessão mais vezes e de onde é que o fez? 
- O início de sessão foi bem-sucedido?  
 
Também pode filtrar os resultados ao selecionar uma data ou localização específica.

**Utilizadores exclusivos por aplicação**: este relatório fornece uma vista de todos os utilizadores exclusivos com uma determinada aplicação. Incluir apenas os utilizadores que têm "*com êxito*" conectado a uma aplicação.

**Inícios de sessão de dispositivo**: este relatório ajuda-o a compreender os vários perfis de dispositivo utilizados na sua organização e determinar políticas de dispositivos com base na utilização. Ele fornece dados sobre o tipo de SO e os browsers utilizados para início de sessão para aplicações, juntamente com informações detalhadas sobre os utilizadores, incluindo:

- Nome de Utilizador
- Endereço IP
- Localização 
- Estado de início de sessão 

**Funil de SSPR**: este relatório ajuda-o a compreender como a ferramenta SSPR é utilizada na sua organização. Pode ver a palavra-passe quantas reposições foram tentadas com a ferramenta SSPR e quantas foram bem-sucedidas. Também pode aprofundar mais as falhas de reposições de palavra-passe e compreender por que motivo ocorreram determinadas falhas. 

## <a name="customize-azure-ad-activity-content-pack"></a>Personalizar o pacote de conteúdos de atividades do Azure AD

**Alterar a Visualização**:pode clicar em **Editar Relatório** para alterar a visualização de um relatório e selecionar a visualização que pretende.
 
![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/09.png) 
 
![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/10.png) 

**Incluir campos adicionais**: pode adicionar um campo ao relatório ou removê-lo ao selecionar o elemento visual ao qual quer adicionar/remover o campo. Por exemplo, pode adicionar o campo "sign-in status" para a exibição de tabela, conforme mostrado abaixo. 
 
![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/11.png) 

**Afixar visualizações ao dashboard**: pode personalizar o dashboard, incluindo as suas próprias visualizações ao relatório e afixá-lo ao dashboard. 

![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/13.png) 
 
**Partilhar o dashboard**: também pode partilhar o dashboard com os utilizadores na sua organização. Depois de partilhar o relatório, os utilizadores podem ver os campos que selecionou no relatório.
 
![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/14.png) 

## <a name="schedule-a-daily-refresh-of-your-power-bi-report"></a>Agendar uma atualização diária do seu relatório do Power BI

Para agendar uma atualização diária do seu relatório do Power BI, aceda a **conjuntos de dados** > **definições** > **agendar atualização** e defini-lo, conforme mostrado abaixo.
 
![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/15.png) 

## <a name="update-to-newer-version-of-content-pack"></a>Atualizar para a versão mais recente do pacote de conteúdos

Se pretender atualizar o pacote de conteúdos para uma versão mais recente:

- Baixe o novo pacote de conteúdos e configurá-lo com as instruções neste artigo.

- Assim que tiver configurado para ele, aceda a **origem de dados** > **definições** > **credenciais da origem de dados** e volte a introduzir as suas credenciais.

    ![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/16.png) 

Depois de verificar se a nova versão do pacote de conteúdos funciona conforme esperado, pode remover a antiga versão, se necessário, eliminando os relatórios e conjuntos de dados associados esse pacote de conteúdos subjacentes.

## <a name="still-having-issues"></a>Ainda está com problemas? 

Veja o nosso [guia de resolução de problemas](troubleshoot-content-pack.md). Para obter ajuda geral com o Power BI, veja estes [artigos de ajuda](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
 
 
## <a name="next-steps"></a>Passos Seguintes

* [Instalar o pacote de conteúdos do Power BI](quickstart-install-power-bi-content-pack.md).
* [Resolver erros do pacote de conteúdos](troubleshoot-content-pack.md).
* [Quais são os relatórios do Azure AD? ](overview-reports.md).

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
ms.date: 12/06/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: c0326a6b611d5f3d5633db2d2b64b8cdc15e10a7
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816689"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Como utilizar o Pacote de Conteúdos do Power BI para o Azure Active Directory

|  |
|--|
|Atualmente, o pacote de conteúdos do Azure AD Power BI utiliza as APIs do Azure AD Graph para recuperar dados de inquilino do Azure AD. Como resultado, poderá ver algumas disparidade entre os dados disponíveis no pacote de conteúdos e os dados obtidos com o [APIs de relatórios do Microsoft Graph](concept-reporting-api.md). |
|  |

Como um administrador de TI, precisa entender como os utilizadores adotam e utilizam as funcionalidades do Azure Active Directory. Isto permite-lhe planear a sua infraestrutura de TI e comunicação para aumentar a utilização e tirar o máximo partido de funcionalidades do Azure AD. Pacote de conteúdos Power BI para o Azure Active Directory dá-lhe a capacidade de analisar mais aprofundadamente os dados para recolher informações mais ricas sobre o que está acontecendo com o seu diretório. Com a integração de APIs do Azure Active Directory para o Power BI, pode transferir o pacote de conteúdos criado previamente e obtenha informações detalhadas sobre todas as atividades dentro do seu Azure Active Directory com a experiência de visualização avançada que o Power BI oferece facilmente. Pode criar o seu próprio dashboard e partilhá-lo facilmente com qualquer pessoa da sua organização. 

## <a name="prerequisites"></a>Pré-requisitos

Precisa de uma licença de premium (P1/P2) do Azure AD para utilizar o pacote de conteúdos. 

## <a name="install-the-content-pack"></a>Instalar o pacote de conteúdos

Veja a [guia de introdução](quickstart-install-power-bi-content-pack.md) para instalar o pacote de conteúdos do Azure AD Power BI.

## <a name="what-can-i-do-with-this-content-pack"></a>O que posso fazer com este pacote de conteúdos?

Antes de avançarmos para aquilo que pode fazer com este pacote de conteúdos, segue-se uma pré-visualização rápida dos diversos relatórios que o mesmo contém. Os dados dos relatórios remontam aos **últimos 30 dias**.

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a>Relatórios incluídos nesta versão do Pacote de Conteúdos de registos do Azure Active Directory

**Relatório de Utilização de Aplicações e Tendências**: obtenha informações sobre as aplicações utilizadas na sua organização e quais as que o são mais vezes e quando. Pode utilizar este relatório para recolher dados sobre como é que uma aplicação que tenha implementado recentemente na sua organização está a ser utilizada ou para descobrir quais são as aplicações mais populares. Com estas informações, pode melhorar a utilização, se vir que uma determinada aplicação não está a ser utilizada.

**Inícios de sessão por localização e utilizadores**: recolha informações sobre todos os inícios de sessão feitos com o Azure Identity, ao mesmo tempo que revela informações sobre a identidade dos utilizadores. Com estas informações, pode analisar mais aprofundadamente inícios de sessão individuais e responder a perguntas como:

- De onde é que este utilizador iniciou sessão?
- Que utilizador iniciou sessão mais vezes e de onde é que o fez? 
- O início de sessão foi bem-sucedido?  
 
Pode clicar numa data ou localização específica para ver informações mais detalhadas.

**Utilizadores exclusivos por aplicação**: veja todos os utilizadores exclusivos que estão a utilizar uma determinada aplicação. Inclui apenas os utilizadores que iniciaram sessão “*com êxito*” numa aplicação.

**Inícios de sessão de dispositivo**: veja que tipo de SO e de browsers os utilizadores da sua organização estão a utilizar, com informações detalhadas sobre eles, incluindo:

- Nome de Utilizador
- Endereço IP
- Localização 
- Estado de início de sessão 

Com este relatório, pode ficar a saber quais são os diversos perfis de dispositivos utilizados na sua organização e determinar políticas de dispositivos com base no que é utilizado.

**Funil de SSPR**: saiba como é que as reposições de palavras-passe são feitas na sua organização. Veja quantas tentativas de repor palavras-passe foram feitas com a ferramenta SSPR e quantas foram bem-sucedidas. Utilize o funil de SSPR (reposição personalizada de palavras-passe) para analisar os detalhes das falhas de Reposições de palavras-passe e ver por que motivo ocorreram determinadas falhas. Este relatório proporciona informações mais completas sobre a forma como a ferramenta SSPR é utilizada na sua organização, para que possa tomar as decisões certas.

## <a name="customizing-azure-ad-activity-content-pack"></a>Personalizar o Pacote de Conteúdos de Atividades do Azure AD

**Alterar a Visualização**:pode clicar em **Editar Relatório** para alterar a visualização de um relatório e selecionar a visualização que pretende.
 
![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/09.png) 
 
![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/10.png) 

**Incluir campos adicionais**: pode adicionar um campo ao relatório ou removê-lo ao selecionar o elemento visual ao qual quer adicionar/remover o campo. No exemplo a seguir, vamos adicionar o campo “sign-in status” à vista de tabela. 
 
![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/11.png) 

**Afixar visualizações ao dashboard**: pode personalizar o seu dashboard e incluir as suas próprias visualizações no relatório e afixá-las ao dashboard. No exemplo a seguir, adicionámos um filtro novo, com o nome “Sign-in Status”, e incluímo-lo no relatório. Também alterámos a visualização de gráfico de barras para gráfico de linhas e posso afixar este elemento visual novo ao dashboard.

![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/12.png) 

![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/13.png) 
 

 


**Partilhar o seu dashboard**: depois de criar o conteúdo que queria, pode partilhar o dashboard com os utilizadores da sua organização. Não se esqueça de que, depois de partilhar, eles podem ver os campos que selecionou no relatório.
 
![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a>Agendar uma atualização diária do seu relatório do Power BI

Para agendar uma atualização diária do seu relatório do Power BI, aceda a **Conjuntos de dados > Definições > Agendar Atualização** e defina-a conforme mostrado abaixo.
 
![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/15.png) 

## <a name="updating-to-newer-version-of-content-pack"></a>Atualizar para uma versão mais recente do pacote de conteúdos

Se quiser atualizar um pacote de conteúdos para obter uma versão mais recente:

- Transfira o pacote novo e configure-e de acordo com as instruções apresentadas neste artigo.

- Depois de concluída a configuração, aceda a **Origem de Dados > Definições > Credenciais da origem de dados**

    ![Pacote de Conteúdos do Power BI para o Azure Active Directory](./media/howto-power-bi-content-pack/16.png) 

Assim que a versão nova do pacote de conteúdos estiver em funcionamento, pode remover a antiga, se necessário, eliminando os relatórios subjacentes e os conjuntos de dados associados a esse pacote de conteúdos.

## <a name="still-having-issues"></a>Ainda está com problemas? 

Veja o nosso [guia de resolução de problemas](troubleshoot-content-pack.md). Para obter ajuda geral com o Power BI, veja estes [artigos de ajuda](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
 

## <a name="next-steps"></a>Passos Seguintes

* [Instalar o pacote de conteúdos do Power BI](quickstart-install-power-bi-content-pack.md).
* [Resolver erros de pacote de conteúdos](troubleshoot-content-pack.md).
* [Quais são os relatórios do Azure AD? ](overview-reports.md).

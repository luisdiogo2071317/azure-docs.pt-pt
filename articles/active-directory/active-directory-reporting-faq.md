---
title: O Azure Active Directory FAQ de relatórios | Documentos da Microsoft
description: O Azure Active Directory FAQ de relatórios.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance-reports
ms.date: 05/10/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d069d0e74c1bc10baa4d14cdb91c137203495ae2
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247413"
---
# <a name="azure-active-directory-reporting-faq"></a>O Azure Active Directory FAQ de relatórios

Este artigo contém respostas para perguntas mais frequentes sobre o Azure Active Directory (Azure AD) de relatórios. Para obter mais informações, veja [Relatórios do Azure Active Directory](active-directory-reporting-azure-portal.md). 

## <a name="getting-started"></a>Introdução 

**P: Estou usando o https://graph.windows.net/&lt; nome do inquilino&gt;/reports/ ponto final de APIs para auditoria de solicitação do Azure AD e a utilização de aplicação integrada relatórios em nossos sistemas de relatórios por meio de programação. O que posso alternar para?**

**R:** pesquisar o [documentação de referência de API](https://developer.microsoft.com/graph/) para ver como pode usar as novas APIs para aceder ao [relatórios de atividade](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal). Este ponto final tem dois relatórios (auditoria e inícios de sessão) que fornecem a todos os dados de que obteve o antigo ponto de final de API. Este novo ponto de extremidade também tem um relatório de inícios de sessão com a licença do Azure AD Premium que pode utilizar para obter a utilização da aplicação, a utilização de dispositivos e informações de início de sessão de utilizador.

--- 

**P: Estou usando o https://graph.windows.net/&lt; nome do inquilino&gt;/reports/ ponto final de APIs para extrair os relatórios de segurança do Azure AD (tipos específicos de detecções, tais como a fuga de credenciais ou inícios de sessão de endereços IP anónimos) para nossos sistemas de relatórios por meio de programação. O que posso alternar para?**

**R:** pode utilizar o [eventos de risco do Identity Protection API](active-directory-identityprotection-graph-getting-started.md) para deteções de segurança de acesso através do Microsoft Graph. Esse novo formato proporciona maior flexibilidade na forma como pode consultar os dados, com filtragem avançada, seleção de campo e muito mais e padroniza eventos de risco num tipo de integração mais fácil com SIEMs e outras ferramentas de recolha de dados. Uma vez que os dados estão num formato diferente, não é possível substituir uma nova consulta para as suas consultas antigas. No entanto, [a nova API utiliza o Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent), que é o padrão da Microsoft para essas APIs, como o Office 365 ou do Azure AD. Para que o trabalho necessário a estender seus investimentos atuais do MS Graph ou ajuda a começar sua transição para essa nova plataforma padrão.

--- 

**P: como posso obter uma licença premium?**

**R:** veja [introdução ao Azure Active Directory Premium](fundamentals/active-directory-get-started-premium.md) uma resposta para essa pergunta.

---

**P: como logo verá dados de atividades depois de obter uma licença premium?**

**R:** se já tiver dados de atividades como uma licença gratuita, então pode ver os mesmos dados. Se não tiver quaisquer dados, em seguida, irá demorar um ou dois dias.

---

**P: Posso ver dados do mês anterior depois de obter uma licença do Azure AD premium?**

**R:** se recentemente tiver mudado para uma versão Premium (incluindo uma versão de avaliação), pode ver dados de cópia de segurança a 7 dias inicialmente. Quando os dados acumulam-se, verá até 30 dias.

---

**P: é necessário ser um administrador global para ver os atividade inícios de sessão no portal do Azure ou para obter dados através da API?**

**R:** Não. Tem de ser um **leitor de segurança**, uma **administrador de segurança**, ou uma **Administrador Global** para obter dados no portal do Azure ou através da API de relatórios.

---


## <a name="activity-logs"></a>Registos de atividade


**P: qual é o período de retenção de dados de registos de Atividades (auditorias e inícios de sessão) no portal do Azure?** 

**R:** veja [para o período de tempo é os dados recolhidos armazenados?](active-directory-reporting-retention.md#q-for-how-long-is-the-collected-data-stored) uma resposta para essa pergunta.

--- 

**P: quanto tempo demora até que eu possa ver os dados de atividade depois de eu ter concluído a minha tarefa?**

**R:** registos de atividade de auditoria tem uma latência desde de 15 minutos até uma hora. Registos de atividades de início de sessão podem demorar entre 15 minutos até 2 horas para alguns registos.

---


**P: posso obter informações de registo de atividade do Office 365 através do portal do Azure?**

**R:** , apesar de atividades do Office 365 e a partilha de registos de atividade do Azure AD muitos dos recursos de diretório, se desejar uma visão completa dos registos de atividades do Office 365, deve ir para o Centro de administração do Office 365 para obter informações de registo de atividades do Office 365.

---


**P: quais APIs utilizo para obter informações sobre os registos de atividades do Office 365?**

**R:** utilizar as APIs de gestão do Office 365 para aceder a [registos de atividades do Office 365 através de uma API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).

---

**P: o número de registos, pode transferir do portal do Azure?**

**R:** pode baixar até 5000 registos do portal do Azure. Os registos são ordenados pelo *mais recente* e por padrão, obtém os registos de 5000 mais recentes.

---

**P: o número de registos pode consultar através das API de atividades**

**R:** pode consultar os registos de até 1 milhão de (se não usar o operador top, que classifica o registo por mais recente). Se utilizar o operador "top", pode consultar a registros de 500 mil. Pode encontrar exemplos de consultas sobre como utilizar a API [aqui](active-directory-reporting-api-getting-started.md).

---

## <a name="risky-sign-ins"></a>Inícios de sessão de risco

**P: existe um evento de risco no Identity Protection, mas não estou vendo correspondente início de sessão em todos os inícios de sessão. É isto esperado?**

**R:** Sim, o Identity Protection avalia o risco para todos os fluxos de autenticação se interativos ou não-interativa. No entanto, todos os relatórios de apenas inícios de sessão mostra apenas os interativos inícios de sessão.

---

**P: como posso transferir o relatório "Os utilizadores sinalizados para risco" no portal do Azure?**

**R:** a opção para transferir *utilizadores sinalizados para risco* relatório vai ser adicionado em breve.

---

**P: como posso saber por que um início de sessão ou um utilizador foi sinalizado risco no portal do Azure?**

**R:** aos clientes da edição Premium podem saber mais sobre os eventos de risco subjacentes ao clicar no utilizador no "Utilizadores sinalizados para risco" ou clicando nos "risco inícios de sessão". Obtém os clientes da edição gratuita e básica ver os utilizadores de risco e os inícios de sessão sem as informações de eventos de risco subjacentes.

---

**P: como são calculados os endereços IP nos inícios de sessão e relatório de inícios de sessão de risco?**

**R:** endereços IP são emitidos de forma que não existe nenhuma ligação definitiva entre um endereço IP e em que o computador com esse endereço está fisicamente localizado. Isso é complicado por fatores como fornecedores de móveis e VPNs de emissão de endereços IP de pools centrais, muitas vezes, muito longe de ser em que o dispositivo de cliente, na verdade, é utilizado. Tendo em conta o acima, a conversão de endereço IP para uma localização física é um melhor esforço com base em rastreamentos, os dados de registo, pesquisas inversa e outras informações. 

---

**P: o que faz o evento de risco "Início de sessão com risco adicional detetado" significar?**

**R:** para dar uma visão geral de todo os risco inícios de sessão no seu ambiente, "início de sessão com risco adicional detetado" funciona como marcador de posição para inícios de sessão de deteções que são exclusivas para subscritores do Azure AD Identity Protection.

---

## <a name="conditional-access"></a>Acesso condicional

**P: o que há de novo com esta funcionalidade?**

**R:** os clientes agora podem resolver problemas de políticas de acesso condicional através de todos os relatórios de inícios de sessão. Os clientes podem rever o estado de acesso condicional e informações detalhadas sobre os detalhes das políticas que são aplicadas para o início de sessão e o resultado para cada política.

**P: como posso começar a utilizar?**

**R:** para começar a utilizar:
    * Navegue para o relatório de inícios de sessão na [portal do Azure](https://portal.azure.com). 
    * Clique no início de sessão que pretende resolver problemas.
    * Navegue para o **acesso condicional** separador. Aqui, pode ver todas as políticas que afetou o início de sessão e o resultado para cada política. 
    
**P: quais são todos os valores possíveis para o estado de acesso condicional?**

**R:** estado do acesso condicional pode ter os seguintes valores:
    * **Não foram aplicados**: isso significa que não havia nenhuma política de acesso condicional com o utilizador e a aplicação no âmbito. 
    * **Êxito**: isso significa que não havia uma política de acesso condicional com o utilizador e a aplicação no âmbito e políticas de acesso condicional serem satisfeitas com êxito. 
    * **Falha de**: isso significa que não havia uma política de acesso condicional com o utilizador e a aplicação no âmbito e políticas de acesso condicional não foram satisfeitas. 
    
**P: quais são todos os valores possíveis para o resultado de política de acesso condicional?**

**R:** uma política de acesso condicional pode ter os seguintes resultados:
    * **Êxito**: A política foi cumprida com êxito.
    * **Falha de**: A política não foi cumprida.
    * **Não foram aplicados**: tal poderá acontecer porque as condições de política não corresponde.
    * **Não ativado**: isso se deve a política no estado desativado. 
    
**P: o nome da política do relatório de todos os início de sessão não corresponde ao nome de política na AC. Porquê?**

**R:** o nome da política do relatório de todos os início de sessão se baseia no nome da política de AC no momento da entrada. Isso pode ser inconsistente com o nome da política na AC se atualizou o nome da política mais tarde, ou seja, após o início de sessão.
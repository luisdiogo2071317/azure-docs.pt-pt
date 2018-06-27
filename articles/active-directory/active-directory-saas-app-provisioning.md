---
title: O aprovisionamento de utilizador de aplicação do SaaS no Azure AD automatizado | Microsoft Docs
description: Uma introdução à forma como pode utilizar o Azure AD para aprovisionar automaticamente, anular o aprovisionamento e atualizar continuamente as contas de utilizador entre várias aplicações de SaaS de terceiros.
services: active-directory
documentationcenter: ''
author: asmalser-msft
manager: mtillman
editor: ''
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/26/2018
ms.author: asmalser
ms.openlocfilehash: c7a18132a797bd7411487c233fc41647cc20dfb4
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37026026"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatizar utilizador aprovisionamento e desaprovisionamento para aplicações de SaaS no Azure Active Directory
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>O que é o utilizador o aprovisionamento automatizado para aplicações SaaS?
Azure Active Directory (Azure AD) permite-lhe automatizar a criação, a manutenção e a remoção de identidades de utilizador na nuvem ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) aplicações como o Dropbox, Salesforce, ServiceNow e muito mais.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

**Seguem-se alguns exemplos de que esta funcionalidade permite-lhe fazer:**

* Crie automaticamente novas contas nos sistemas direitos para pessoas novo quando que se associam a sua equipa ou organização.
* Desative automaticamente contas nos sistemas direita quando as pessoas deixam a equipa ou organização.
* Certifique-se de que as identidades nas suas aplicações e sistemas são mantidas atualizadas com base nas alterações no diretório ou o sistema de recursos humanos.
* Aprovisionar objetos de não utilizadores, tais como grupos, para aplicações que suportam-los.

**Aprovisionamento de utilizadores automatizada inclui também a funcionalidade seguinte:**

* A capacidade para fazer corresponder identidades existentes entre sistemas de origem e de destino.
* Mapeamentos de atributos personalizáveis que definem que dados de utilizador devem fluir de sistema de origem para o sistema de destino.
* Alertas de e-mail opcional para o aprovisionamento de erros
* Registos de atividade e relatórios para ajudar na monitorização e resolução de problemas.

## <a name="why-use-automated-provisioning"></a>Porquê utilizar o aprovisionamento automatizado?
Alguns motivações comuns para utilizar esta funcionalidade incluem:

* Evitando os custos, inefficiencies e erro humano associado com processos de aprovisionamento manuais.
* Evitando os custos associados de alojamento e manter soluções de aprovisionamento personalizadas desenvolvidas e scripts
* Para proteger a sua organização removendo instantaneamente as identidades dos utilizadores de aplicações de SaaS chaves quando deixam a organização.
* Para importar facilmente um grande número de utilizadores para uma determinada aplicação SaaS ou sistema.
* Para desfrutar ter um único conjunto de políticas para determinar que está aprovisionado e que pode iniciar sessão numa aplicação.

## <a name="how-does-automatic-provisioning-work"></a>Como funciona o aprovisionamento automático?
    
O **serviço do Azure de aprovisionamento de AD** Aprovisiona utilizadores para aplicações SaaS e outros sistemas, através da ligação a pontos finais de API de gestão de utilizador fornecidos pelo fornecedor de cada aplicação. Estes pontos finais API de gestão de utilizador permite que o Azure AD para programaticamente criar, atualizar e remover utilizadores. Para aplicações selecionadas, que o serviço de aprovisionamento também pode criar, atualizar e remover objetos de relacionadas com identidade adicionais, tais como grupos e funções. 

![Aprovisionamento](./media/active-directory-saas-app-provisioning/provisioning0.PNG)
*figura 1: O Azure AD aprovisionamento do serviço*

![Aprovisionamento de saída](./media/active-directory-saas-app-provisioning/provisioning1.PNG)
*figura 2: fluxo de trabalho do Azure AD para aplicações de SaaS populares de aprovisionamento de utilizadores de "Saída"*

![Aprovisionamento de entrada](./media/active-directory-saas-app-provisioning/provisioning2.PNG)
*figura 3: fluxo de trabalho de aplicações de gestão de Capital humana (HCM) popular para o Azure Active Directory e o Windows Server Active Directory de aprovisionamento de utilizador de "Entrada"*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>As aplicações e sistemas posso utilizar com o aprovisionamento de utilizador automáticas do Azure AD?

Funcionalidades do Azure AD pré-integrado suporte para uma variedade de aplicações de SaaS populares e sistemas de recursos humanos, bem como suporte genérico para aplicações que implementam partes específicas de padrão SCIM 2.0.

### <a name="pre-integrated-applications"></a>Aplicações previamente integradas
Para obter uma lista de todas as aplicações para o qual o AD do Azure suporta um conector de aprovisionamento previamente integrado, consulte o [lista de tutoriais de aplicação para o aprovisionamento de utilizador](saas-apps/tutorial-list.md).

Contactar o Azure AD engenharia equipa para pedir suporte aprovisionamento para aplicações adicionais, enviar uma mensagem através de [fórum de comentários do Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).

> [!NOTE]
> Ordem de uma aplicação suportar o aprovisionamento automatizado do utilizador,-lo primeiro tem de fornecer as APIs que permitem que programas externos automatizar a criação, a manutenção e a remoção de utilizadores de gestão de utilizador necessário. Por conseguinte, nem todas as aplicações de SaaS são compatíveis com esta funcionalidade. Para aplicações que suportam as APIs de gestão de utilizador, a equipa de engenharia do Azure AD, em seguida, será capaz de criar um conector aprovisionamento às aplicações e este trabalho é definido pelas necessidades atuais e potenciais clientes. 

### <a name="connecting-applications-that-support-scim-20"></a>Ligar aplicações que suportem o SCIM 2.0
Para obter informações sobre como genericamente ligar aplicações que implementa o SCIM 2.0 - gestão de utilizadores com base em APIs, consulte [a utilizar o SCIM para aprovisionar automaticamente os utilizadores e grupos do Azure Active Directory para aplicações](manage-apps/use-scim-to-provision-users-and-groups.md).

    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Como configurar o aprovisionamento automático para uma aplicação?

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

Configuração do Azure AD para uma aplicação selecionada é iniciado no serviço de fornecimento de  **[portal do Azure](https://portal.azure.com)**. No **do Azure Active Directory > aplicações da empresa** secção, selecione **adicionar**, em seguida, **todos os**e, em seguida, adicione o seguinte dependendo do seu cenário:

* Todas as aplicações a **em destaque aplicações** aprovisionamento automático do secção suporte. Consulte o [lista de tutoriais de aplicação para o aprovisionamento de utilizador](saas-apps/tutorial-list.md) para adicionais.

* Utilize a opção "não Galeria a aplicação" para integrações de SCIM desenvolvidos personalizada

![Galeria](./media/active-directory-saas-app-provisioning/gallery.png)

No ecrã de gestão de aplicações, aprovisionamento está configurado no **aprovisionamento** separador.

![Definições](./media/active-directory-saas-app-provisioning/provisioning_settings0.PNG)


* **Credenciais de administrador** tem de ser fornecido para o Azure AD aprovisionamento do serviço que irão permiti-lhe estabelecer ligação com a API fornecida pela aplicação de gestão do utilizador. Esta secção também permite-lhe ativar notificações por correio eletrónico se as credenciais falharem ou tarefa de aprovisionamento de entra em [quarentena](#quarantine).

* **Mapeamentos de atributos** podem ser configurados que especificam qual campos no sistema de origem (exemplo: do Azure AD) irão ter os respetivos conteúdos sincronizados com os campos no sistema de destino (exemplo: ServiceNow). Se a aplicação de destino suportar, esta secção permitirá opcionalmente, configure o aprovisionamento dos grupos para além das contas de utilizador. "Propriedades correspondentes" permitem-lhe para selecionar os campos são utilizados para corresponder a contas entre os sistemas. "[Expressões](active-directory-saas-writing-expressions-for-attribute-mappings.md)" permitem-lhe modificar e transformar os valores obtidos a partir do sistema de origem antes de estas são escritas para sistema de destino. Para obter mais informações, consulte [personalizar mapeamentos de atributos](active-directory-saas-customizing-attribute-mappings.md).

![Definições](./media/active-directory-saas-app-provisioning/provisioning_settings1.PNG)

* **Filtros de âmbito** informar o serviço de aprovisionamento que utilizadores e grupo no sistema de origem deve ser aprovisionado e/ou desaprovisionado ao sistema de destino. Existem dois aspetos a filtros são avaliados em conjunto de âmbito que determinem quem se encontra no âmbito de aprovisionamento:

    * **Filtro de valores de atributo** -usando o menu "Âmbito de objeto de origem" nos mapeamentos de atributos permite a filtragem de valores de atributo específico. Por exemplo, pode especificar que apenas os utilizadores com um atributo de "Departamento" de "Vendas" devem estar no âmbito de aprovisionamento. Para obter mais informações, consulte [utilizando filtros de âmbito](active-directory-saas-scoping-filters.md).

    * **Filtro em atribuições de** -o menu de "Âmbito" o aprovisionamento > da secção definições do portal permite-lhe especificar se devem ser apenas "atribuídos" os utilizadores e grupos no âmbito de aprovisionamento ou se devem ser todos os utilizadores no diretório do Azure AD aprovisionado. Para obter informações sobre "atribuir" utilizadores e grupos, consulte [atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md).
    
* **Definições** controlam o funcionamento do serviço aprovisionamento para uma aplicação, incluindo se está em execução ou não.

* **Registos de auditoria** fornecer registos de cada operação efetuada pelo Azure AD que o serviço de fornecimento. Para obter mais detalhes, consulte o [guia de relatórios aprovisionamento](active-directory-saas-provisioning-reporting.md).

![Definições](./media/active-directory-saas-app-provisioning/audit_logs.PNG)

> [!NOTE]
> O serviço de fornecimento de utilizador do Azure AD também podem ser configurado e geridas utilizando o [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview).


## <a name="what-happens-during-provisioning"></a>O que acontece durante o aprovisionamento?

Quando o Azure AD é o sistema de origem, o serviço de aprovisionamento utiliza o [funcionalidade de consulta diferencial do Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query) para monitorizar os utilizadores e grupos. O serviço de aprovisionamento é executada uma sincronização inicial com o sistema de origem e o sistema de destino, seguido de sincronizações incrementais periódicas. 

### <a name="initial-sync"></a>Sincronização inicial
Quando é iniciado o serviço de aprovisionamento, será a primeira sincronização efetuada alguma vez:

1. Consultar todos os utilizadores e grupos de sistema de origem, obter todos os atributos definidos no [mapeamentos de atributos](active-directory-saas-customizing-attribute-mappings.md).
2. Filtrar os utilizadores e grupos devolvidos, utilizando qualquer configurado [atribuições](manage-apps/assign-user-or-group-access-portal.md) ou [filtros de âmbito baseadas em atributos](active-directory-saas-scoping-filters.md).
3. Quando é encontrado um utilizador que seja atribuído ou no âmbito de aprovisionamento, o serviço de consulta o sistema de destino para um utilizador correspondente utilizando designado [correspondente atributos](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-properties). Exemplo: Se o nome de userPrincipal no sistema de origem é o atributo correspondente e mapeia para o nome de utilizador no sistema de destino, em seguida, o serviço de aprovisionamento consulta o sistema de destino para nomes de utilizador que correspondem aos valores de nome userPrincipal no sistema de origem.
4. Se um utilizador correspondente não foi encontrado no sistema de destino, é criada utilizando os atributos devolvidos do sistema de origem.
5. Se um utilizador correspondente for encontrado, é atualizado com os atributos fornecidos pelo sistema de origem.
6. Se os mapeamentos de atributos contém atributos "reference", o serviço efetua atualizações adicionais no sistema de destino para criar e ligar os objetos referenciados. Por exemplo, um utilizador pode ter um atributo "Gestor" no sistema de destino, que está ligado a outro utilizador criado no sistema de destino.
7. Manter uma marca d'água no final da sincronização inicial, o que fornece o ponto de partida para as sincronizações incrementais subsequentes.

Algumas aplicações, tais como o ServiceNow, Google Apps, caixa de suporte e não apenas os utilizadores de aprovisionamento, mas também aprovisionamento grupos e os respetivos membros. Nesses casos, se o aprovisionamento de grupo está ativado no [mapeamentos](active-directory-saas-customizing-attribute-mappings.md), o aprovisionamento do serviço sincroniza os utilizadores e os grupos e, em seguida, sincronize subsequentemente as associações. 

### <a name="incremental-syncs"></a>Sincronizações incrementais
Após a sincronização inicial, todas as sincronizações subsequentes irão:

1. Consulta o sistema de origem para os utilizadores e grupos que tenham sido atualizados desde que foi armazenada a marca d'água último.
2. Filtrar os utilizadores e grupos devolvidos, utilizando qualquer configurado [atribuições](manage-apps/assign-user-or-group-access-portal.md) ou [filtros de âmbito baseadas em atributos](active-directory-saas-scoping-filters.md).
3. Quando é encontrado um utilizador que seja atribuído ou no âmbito de aprovisionamento, o serviço de consulta o sistema de destino para um utilizador correspondente utilizando designado [correspondente atributos](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-properties).
4. Se um utilizador correspondente não foi encontrado no sistema de destino, é criada utilizando os atributos devolvidos do sistema de origem.
5. Se um utilizador correspondente for encontrado, é atualizado com os atributos fornecidos pelo sistema de origem.
6. Se os mapeamentos de atributos contém atributos "reference", o serviço efetua atualizações adicionais no sistema de destino para criar e ligar os objetos referenciados. Por exemplo, um utilizador pode ter um atributo "Gestor" no sistema de destino, que está ligado a outro utilizador criado no sistema de destino.
7. Se um utilizador que se encontravam anteriormente no âmbito de aprovisionamento é removido do âmbito (incluindo a ser não atribuídos), o serviço desativa o utilizador no sistema de destino através de uma atualização.
8. Se um utilizador que se encontravam anteriormente no âmbito de aprovisionamento está desativado ou eliminados de forma recuperável no sistema de origem, o serviço desativa o utilizador no sistema de destino através de uma atualização.
9. Se um utilizador que se encontravam anteriormente no âmbito de aprovisionamento é eliminado no sistema de origem, o serviço elimina o utilizador no sistema de destino. No Azure AD, os utilizadores são eliminado 30 dias após a sua eliminados de forma recuperável.
10. Manter uma marca d'água novo no final da sincronização incremental, o que fornece o ponto de partida para as sincronizações incrementais subsequentes.

>[!NOTE]
> Opcionalmente, pode desativar a criação, atualização ou operações delete utilizando o **ações de objeto de destino** caixas de verificação no [mapeamentos de atributos](active-directory-saas-customizing-attribute-mappings.md) secção. A lógica para desativar um utilizador durante uma atualização também é controlada através de um mapeamento de atributos de um campo, tais como "accountEnabled".

O serviço de aprovisionamento irá continuar a executar sincronizações incrementais back indefinidamente, em intervalos definidos no [tutorial específica para cada aplicação](saas-apps/tutorial-list.md), até que um dos seguintes eventos ocorra:

* O serviço foi interrompido manualmente no portal do Azure, ou utilizando o comando Graph API adequado 
* Uma sincronização inicial novo é acionada através de **limpar o estado e reiniciar** opção no portal do Azure, ou utilizando o comando Graph API adequado. Isto limpa qualquer armazenada marca d'água e faz com que todos os objetos de origem deve ser avaliada novamente.
* Uma sincronização inicial novo é acionada devido a uma alteração de mapeamentos de atributos ou filtros de âmbito. Isto também limpa qualquer armazenada marca d'água e faz com que todos os objetos de origem deve ser avaliada novamente.
* O processo de aprovisionamento entra em quarentena (ver abaixo) devido a uma elevada taxa de erros e permanece em quarentena durante mais de quatro semanas. Neste caso, o serviço será automaticamente desativado.

### <a name="errors-and-retries"></a>Erros e de tentativas 
Se um utilizador individual não pode ser adicionado, atualizado ou eliminado no sistema de destino devido a um erro no sistema de destino, a operação será repetida no próximo ciclo de sincronização. Se o utilizador continuar a falhar, as repetições irão começar a adotar uma frequência reduzida, aumentar gradualmente para apenas uma tentativa por dia. Para resolver a falha, os administradores terão de verificar o [registos de auditoria](active-directory-saas-provisioning-reporting.md) para "caução de processo" eventos para determinar a raiz provocar e tome as medidas adequadas. Falhas comuns na podem incluir:

* Utilizadores não ter um atributo preenchido no sistema de origem que é necessárias no sistema de destino
* Os utilizadores com um valor de atributo no sistema de origem é uma restrição exclusiva no sistema de destino e o mesmo valor não está presentes no outro registo de utilizador

Estas falhas podem ser resolvidas por ajustar os valores de atributo para o utilizador afectado no sistema de origem ou ajustar os mapeamentos de atributos não causar conflitos.   

### <a name="quarantine"></a>Quarentena
Se a maior parte ou todas as chamadas efetuadas no sistema de destino consistentemente falhar devido a um erro (tal como no caso de credenciais de administrador inválido), em seguida, a tarefa de aprovisionamento entra no estado "quarentena". Esta situação é indicada no [relatório de resumo de aprovisionamento](active-directory-saas-provisioning-reporting.md)e através de correio eletrónico se as notificações de e-mail que foram configuradas no portal do Azure. 

Quando em quarentena, a frequência de sincronizações incrementais gradualmente é reduzida para uma vez por dia. 

A tarefa de aprovisionamento será removida da quarentena depois de todos os erros inválidos que está a ser fixos e o próximo ciclo de sincronização é iniciada. Se a tarefa de aprovisionamento permanece em quarentena durante mais de quatro semanas, a tarefa de aprovisionamento está desativada.


## <a name="how-long-will-it-take-to-provision-users"></a>Quanto tempo necessário para aprovisionar utilizadores?

Desempenho depende se a tarefa de aprovisionamento está a efetuar uma sincronização inicial ou uma sincronização incremental, conforme descrito na secção anterior.

Para **inicial sincronizações**, o tempo de tarefa depende de vários fatores, incluindo o número de utilizadores e grupos no âmbito de aprovisionamento e o número total de utilizadores e grupo no sistema de origem. Uma lista completa dos fatores que afetam o desempenho de sincronização inicial estão resumidos posteriormente nesta secção.

Para **sincronizações incrementais**, o tempo de tarefa depende o número de foram detetadas alterações nesse ciclo de sincronização. Se existirem menos de 5000 utilizadores ou alterações de associação de grupo, pode concluir a tarefa dentro de um ciclo de sincronização incremental único. 

A tabela seguinte resume as horas de sincronização para cenários comuns de aprovisionamento. Nestes cenários, o sistema de origem é o Azure AD e o sistema de destino é uma aplicação SaaS. Os tempos de sincronização são derivados de uma análise estatística de tarefas de sincronização para as aplicações de SaaS ServiceNow, à área de trabalho, Salesforce e Google Apps.


| Configuração de âmbito | Os utilizadores, grupos e membros de âmbito | Hora de sincronização inicial | Hora de sincronização incremental |
| -------- | -------- | -------- | -------- |
| Sincronização e atribuídos a utilizadores apenas a grupos |  < 1000 |  < 30 minutos | < 30 minutos |
| Sincronização e atribuídos a utilizadores apenas a grupos |  1.000 - 10.000 | 142 - 708 minutos | < 30 minutos |
| Sincronização e atribuídos a utilizadores apenas a grupos |   10.000 - 100 000 | 1,170 - 2,340 minutos | < 30 minutos |
| Sincronizar todos os utilizadores e grupos no Azure AD |  < 1000 | < 30 minutos  | < 30 minutos |
| Sincronizar todos os utilizadores e grupos no Azure AD |  1.000 - 10.000 | < 30 120 minutos | < 30 minutos |
| Sincronizar todos os utilizadores e grupos no Azure AD |  10.000 - 100 000  | 713 - 1,425 minutos | < 30 minutos |
| Sincronizar todos os utilizadores no Azure AD|  < 1000  | < 30 minutos | < 30 minutos |
| Sincronizar todos os utilizadores no Azure AD | 1.000 - 10.000  | 43 - 86 minutos | < 30 minutos |


Para a configuração **sincronização atribuídos apenas a grupos de utilizadores e**, pode utilizar as fórmulas seguintes para determinar a aproximado mínimo e máximo esperado **inicial sincronização** vezes:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
Resumo dos fatores que influenciam o tempo que demora a concluir um **inicial sincronização**:

* O número total de utilizadores e grupos no âmbito de aprovisionamento

* O número total de utilizadores, grupos e membros do grupo presentes no sistema de origem (Azure AD)

* Se pretende ou não os utilizadores no âmbito de aprovisionamento são correspondidos para os utilizadores existentes da aplicação de destino ou tem de ser criado pela primeira vez. Tarefas de sincronização para o qual todos os utilizadores são criados pela primeira vez demorar, aproximadamente, *duas vezes, desde* como sincronizar tarefas para o qual todos os utilizadores são correspondidos para os utilizadores existentes.

* Número de erros no [registos de auditoria](active-directory-saas-provisioning-reporting.md). Desempenho é mais lento se houver muitos erros e o serviço de aprovisionamento tornou-se num Estado de quarentena   

* Limites de velocidade e limitação implementada pelo sistema de destino do pedido. Alguns sistemas de destino implementam limites de velocidade do pedido e limitação que pode afetar o desempenho durante as operações de grande sincronização. Nas seguintes condições, uma aplicação que recebe demasiados pedidos demasiado rápidos poderá lenta a respetiva taxa de resposta ou fechar a ligação. Para melhorar o desempenho, o conector tem de ajustar por não enviar os pedidos de aplicação mais rapidamente do que a aplicação pode processá-los. Aprovisionamento conectores criados pela Microsoft tornar este ajuste. 

* O número e tamanhos de grupos atribuídos. A sincronizar atribuídos grupos demora mais de sincronizar os utilizadores. O número e os tamanhos dos grupos atribuídos impacto no desempenho. Se tiver uma aplicação [mapeamentos ativados para sincronização do objeto de grupo](active-directory-saas-customizing-attribute-mappings.md#editing-group-attribute-mappings), as propriedades do grupo, tais como nomes de grupo e as associações são sincronizadas, além de utilizadores. Estes sincronizações adicionais irão demorar mais do que apenas a sincronizar objetos de utilizador.


##<a name="how-can-i-tell-if-users-are-being-provisioned-properly"></a>Como posso saber se os utilizadores estão a ser aprovisionados corretamente?

Todas as operações executadas pelo utilizador, serviço de fornecimento são registadas no Azure AD registos de auditoria. Isto inclui todos os ler e escrever operações efetuadas para os sistemas de origem e de destino, bem como os dados de utilizador foi lidas ou escritos durante cada operação.

Para obter informações sobre como a leitura de os registos de auditoria no portal do Azure, consulte o [guia de relatórios aprovisionamento](active-directory-saas-provisioning-reporting.md).


##<a name="how-do-i-troubleshoot-issues-with-user-provisioning"></a>Como posso resolver problemas com o aprovisionamento de utilizadores?

Para orientações sobre como resolver problemas de aprovisionamento de utilizadores automática baseada em cenários, consulte [problemas de configuração e o aprovisionamento de utilizadores a uma aplicação](active-directory-application-provisioning-content-map.md).


##<a name="what-are-the-best-practices-for-rolling-out-automatic-user-provisioning"></a>Quais são as melhores práticas para disponibilizando o aprovisionamento de utilizadores automática?

> [!VIDEO https://www.youtube.com/embed/MAy8s5WSe3A]

Para um plano de implementação passo a passo de exemplo para o aprovisionamento de utilizador de saída para uma aplicação, consulte o [guia de implementação de identidade para o aprovisionamento de utilizador](https://aka.ms/userprovisioningdeploymentplan)/


## <a name="related-articles"></a>Artigos relacionados
* [Lista de tutoriais sobre como integrar aplicações SaaS](saas-apps/tutorial-list.md)
* [Personalizar os mapeamentos de atributos para o aprovisionamento de utilizador](active-directory-saas-customizing-attribute-mappings.md)
* [Escrever expressões para mapeamentos de atributos](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtros de âmbito para o aprovisionamento de utilizador](active-directory-saas-scoping-filters.md)
* [Utilizar o SCIM para ativar o aprovisionamento automático de utilizadores e grupos do Azure Active Directory a aplicações](manage-apps/use-scim-to-provision-users-and-groups.md)
* [Descrição geral de API de sincronização do Azure AD](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)

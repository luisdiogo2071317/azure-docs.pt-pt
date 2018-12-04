---
title: 'Tutorial: Configurar o dia de trabalho para aprovisionamento automático de utilizadores no Azure Active Directory | Documentos da Microsoft'
description: Saiba como configurar o Azure Active Directory para aprovisionar e desaprovisionar contas de utilizador com o Workday.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: mtillman
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/18/2018
ms.author: chmutali
ms.openlocfilehash: 754c3278cb01e010718fa4d3cb257acf6ffe99c9
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52849858"
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-preview"></a>Tutorial: Configurar o Workday para automático de utilizadores aprovisionamento (pré-visualização)

O objetivo deste tutorial é mostrar a os passos que necessários para efetuar para importar perfis de trabalho a partir do Workday para Active Directory e Azure Active Directory, com repetição de escrita opcional de endereço de e-mail no Workday.

## <a name="overview"></a>Descrição geral

O [utilizador do Azure Active Directory, serviço de aprovisionamento](../manage-apps/user-provisioning.md) integra-se com o [Workday Human Resources API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) para aprovisionar contas de utilizador. Azure AD utiliza esta ligação para permitir que o utilizador seguinte fluxos de trabalho de aprovisionamento:

* **Aprovisionar utilizadores para o Active Directory** -sincronizar selecionados conjuntos de utilizadores do Workday para um ou mais domínios do Active Directory.

* **Aprovisionar utilizadores apenas na cloud para o Azure Active Directory** - em cenários onde não for utilizado o Active Directory, os utilizadores no local podem ser aprovisionados diretamente a partir do Workday para o Azure Active Directory com o serviço de aprovisionamento de utilizador do Azure AD. 

* **Endereços de repetição de escrita de e-mail para Workday** -o serviço de aprovisionamento de utilizador do Azure AD pode gravar os endereços de e-mail dos utilizadores do Azure AD no Workday. 

### <a name="what-human-resources-scenarios-does-it-cover"></a>Quais cenários de recursos humanos cobre?

Os Workday utilizador Aprovisiona fluxos de trabalho suportados pelo serviço de aprovisionamento de utilizador do Azure AD ativar a automatização dos seguintes recursos humanos e cenários de gestão do ciclo de vida de identidade:

* **Novos funcionários a contratar** - quando um novo funcionário é adicionado ao Workday, uma conta de utilizador é criada automaticamente no Active Directory, Azure Active Directory e, opcionalmente, do Office 365 e [outras aplicações SaaS suportadas pelo Azure AD](../manage-apps/user-provisioning.md), com repetição de escrita do endereço de e-mail no Workday.

* **Atualizações de atributo e perfil de funcionário** - quando um registro de funcionário é atualizado no Workday (por exemplo, o respetivo nome, título ou gestor), a sua conta de utilizador será atualizada automaticamente no Active Directory, Azure Active Directory e, opcionalmente, do Office 365 e [outras aplicações SaaS suportadas pelo Azure AD](../manage-apps/user-provisioning.md).

* **Términos funcionário** - quando um funcionário é demitido no Workday, a conta de utilizador é automaticamente desativada no Active Directory, Azure Active Directory e, opcionalmente, do Office 365 e [outras aplicações SaaS suportadas pelo Azure AD](../manage-apps/user-provisioning.md).

* **Funcionário novamente contratações** - quando um funcionário é rehired no Workday, sua conta antiga pode ser reativada ou voltar aprovisionada (dependendo da sua preferência) para Active Directory, Azure Active Directory e, opcionalmente, para o Office 365 e automaticamente[outras aplicações SaaS suportadas pelo Azure AD](../manage-apps/user-provisioning.md).

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Quem é que esta solução de aprovisionamento de utilizador melhor adequado para?

Esta solução de aprovisionamento de utilizadores de Workday estão atualmente em pré-visualização pública e é ideal para:

* Organizações que desejam acesso uma solução previamente criada com base na cloud para aprovisionamento de utilizadores do Workday

* Organizações que precisam de aprovisionamento de utilizadores direta do Workday para o Active Directory ou do Azure Active Directory

* As organizações que exigem que os usuários a ser aprovisionado com dados obtidos a partir do módulo de Workday HCM (consulte [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* As organizações que necessitem da associação, mover, e deixar que os utilizadores serão sincronizados para um ou mais florestas de diretório do Active Directory, domínios e UOs com base apenas no alterar informações detetadas no módulo do Workday HCM (consulte [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Organizações que usam o Office 365 para e-mail

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="solution-architecture"></a>Arquitetura da Solução

Esta secção descreve a arquitetura da solução para ambientes de híbridos comuns de aprovisionamento de utilizadores de ponto-a-ponto. Existem dois fluxos relacionados:

* **Fluxo de dados HR autoritativo – partir do Workday para o Active Directory no local:** neste fluxo de trabalho (por exemplo, novas contratações, transferências, términos) primeiro ocorrem eventos na cloud de inquilino do Workday RH e passa, então, os dados de eventos ativos no local Diretório através do Azure AD e o agente de aprovisionamento. Consoante o evento, ele pode levar a operações criar/atualizar/Ativar/desativar no AD.
* **Fluxo de repetição de escrita – de e-mail do Active Directory no local para Workday:** assim que a criação de conta estiver concluída no Active Directory, ele está sincronizado com o Azure AD através do Azure AD Connect e o atributo de correio eletrónico obtido a partir do Active Directory pode ser escrito novamente no Workday.

![Descrição geral](./media/workday-inbound-tutorial/wd_overview.png)

### <a name="end-to-end-user-data-flow"></a>Fluxo de dados de utilizador de ponto a ponto

1. A equipe de RH realiza transações de trabalho (Joiners/manda/Leavers ou novas contratações/transferências/términos) no Workday HCM
2. O serviço de aprovisionamento do AD Azure executa sincronizações agendadas de identidades de RH do Workday e identifica as alterações que precisam de ser processados para sincronização com o Active Directory no local.
3. O serviço de aprovisionamento do AD Azure invoca o agente no local AAD Connect aprovisionamento com um payload de pedido que contém a operações de criar/atualizar/Ativar/desativar de conta do AD.
4. O agente do Azure AD Connect aprovisionamento utiliza uma conta de serviço para adicionar ou atualizar dados de conta do AD.
5. O Azure AD Connect / o motor de sincronização do AD executa a sincronização delta para receber atualizações no AD.
6. As atualizações do Active Directory são sincronizadas com o Azure Active Directory.
7. Se o conector de repetição de escrita do Workday está configurado, ele escrita-faz uma cópia de atributo de correio eletrónico no Workday, com base no atributo correspondente utilizado.


## <a name="planning-your-deployment"></a>Planear a implementação

Antes de iniciar a integração de Workday, verifique os pré-requisitos abaixo e leia as seguintes orientações sobre como de acordo com sua arquitetura do Active Directory e requisitos de aprovisionamento com a melhor ou soluções fornecida pelo Azure Active Directory do utilizador atual.

### <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial parte do princípio de que já tem os seguintes itens:

* Uma subscrição do Azure AD Premium P1 válida com acesso de administrador global
* Um inquilino de implementação do Workday para fins de teste e integração
* Permissões de administrador no Workday para criar um utilizador de integração do sistema e fazer alterações para testar os dados de funcionários para fins de teste
* Para o aprovisionamento do utilizador do Active Directory, um servidor com o Windows Server 2012 ou superior com o .NET 4.7 + tempo de execução é necessário para alojar o [agente de aprovisionamento do local](https://go.microsoft.com/fwlink/?linkid=847801)
* [O Azure AD Connect](../hybrid/whatis-hybrid-identity.md) para a sincronização entre o Active Directory e o Azure AD

### <a name="planning-considerations"></a>Considerações de planeamento

O Azure AD fornece um conjunto avançado de aprovisionamento de conectores para ajudar a resolver o aprovisionamento e ciclo de vida gestão de identidades do Workday para o Active Directory, Azure AD, aplicações SaaS e muito mais. Que funcionalidades de mensagens em fila irão utilizar e como configurar a solução irá variar dependendo do ambiente e os requisitos da sua organização. Como primeiro passo, e fazer de estão quantos das seguintes ações presentes e implementado na sua organização:

* Quantos florestas de diretório do Active Directory estão em utilização?
* Quantos domínios do Active Directory estão em utilização?
* Quantos Active Directory UOs (unidades organizacionais) estão em utilização?
* O número de inquilinos do Azure Active Directory está em utilização?
* Existem utilizadores que precisam de ser aprovisionada para o Active Directory e Azure Active Directory (por exemplo "híbrido" os utilizadores)?
* Existem utilizadores que precisam de ser aprovisionada para o Azure Active Directory, mas não do Active Directory (por exemplo "nuvem" os utilizadores só)?
* Endereços de e-mail do utilizador é necessário para a repetição de escrita Workday?

Assim que tiver respostas para essas perguntas, pode planejar seu dia de trabalho de aprovisionamento implementação ao seguir as orientações abaixo.

#### <a name="planning-deployment-of-aad-connect-provisioning-agent"></a>Planejamento de implantação do agente de aprovisionamento Connect da AAD

Dia de trabalho para o aprovisionamento de utilizadores do AD solução requer a implementação de um ou mais agentes de aprovisionamento em servidores que executam o Windows 2012 R2 ou superior com o mínimo de 4 GB de RAM e o .NET 4.7 + tempo de execução. As seguintes considerações devem ser levadas em conta antes de instalar o agente de aprovisionamento:

* Certifique-se de que o servidor de anfitrião que executa o agente de aprovisionamento tem acesso à rede para o domínio de destino AD
* O Assistente de configuração do agente de aprovisionamento regista o agente no seu inquilino do Azure AD e o processo de registo requer acesso de *. msappproxy.net na porta 8082. Certifique-se de que as regras de firewall de saída estão em vigor que permitem esta comunicação.
* O agente de aprovisionamento utiliza uma conta de serviço para comunicar com no local domínios do AD. Antes da instalação do agente, recomenda-se que crie uma conta de serviço com permissões de leitura/escrita de propriedades de utilizador e uma palavra-passe não expira.  
* Durante a configuração do agente de aprovisionamento, pode selecionar os controladores de domínio que devem processar solicitações de provisionamento. Se tiver vários controladores de domínio distribuídos geograficamente, instale o agente de aprovisionamento no mesmo site que o controlador de domínio preferencial (es), para melhorar a fiabilidade e desempenho da solução ponto a ponto
* Para elevada disponibilidade, pode implementar mais do que um agente de aprovisionamento e registá-lo para lidar com o mesmo conjunto de locais domínios do AD.

> [!IMPORTANT]
> Em ambientes de produção, a Microsoft recomenda que tem um mínimo de 3 agentes aprovisionamento configurado com o inquilino do Azure AD para elevada disponibilidade.

#### <a name="selecting-provisioning-connector-apps-to-deploy"></a>Selecionar o aprovisionamento de conector de aplicações a implementar

Ao integrar o Workday e o Active Directory, existem vários sistemas de origem e de destino para ser considerado:

| Sistema de origem | Sistema de destino | Notas |
| ---------- | ---------- | ---------- |
| Dia de trabalho | Domínio do Active Directory | Cada domínio é tratado como um sistema de destino distintos |
| Dia de trabalho | Inquilino do Azure AD | Conforme necessário para os utilizadores apenas na cloud |
| Floresta do Active Directory | Inquilino do Azure AD | Este fluxo é processado pelo AAD Connect hoje mesmo |
| Inquilino do Azure AD | Dia de trabalho | Para a repetição de escrita de endereços de e-mail |

Para facilitar o provisionamento de fluxos de trabalho entre o dia de trabalho e o Active Directory, o Azure AD fornece várias aplicações de conector aprovisionamento que pode adicionar a partir da Galeria de aplicações do Azure AD:

![Galeria de aplicações do AAD](./media/workday-inbound-tutorial/wd_gallery.png)

* **Workday para o aprovisionamento do Active Directory** -conta aprovisionamento de utilizadores do Workday para um único domínio do Active Directory facilita a esta aplicação. Se tiver vários domínios, pode adicionar uma instância desta aplicação a partir da Galeria de aplicações do Azure AD para cada domínio do Active Directory que precisa de aprovisionar a.

* **Workday para o Azure AD aprovisionamento** – apesar do AAD Connect é a ferramenta que deve ser usada para sincronizar o Active Directory, os utilizadores ao Azure Active Directory, esta aplicação podem ser usados para facilitar o aprovisionamento de utilizadores apenas na cloud a partir do Workday para um único Azure Inquilino do Active Directory.

* **Repetição de escrita do workday** -repetição de escrita de endereços de e-mail do utilizador do Azure Active Directory para Workday facilita a esta aplicação.

> [!TIP]
> A aplicação regular de "Workday" é utilizada para configurar o início de sessão único entre o dia de trabalho e o Azure Active Directory. 

#### <a name="determine-workday-to-ad-user-attribute-mapping-and-transformations"></a>Determinar o Workday para o mapeamento do atributo de utilizador do AD e transformações

Antes de configurar o aprovisionamento de utilizadores a um domínio do Active Directory, considere as seguintes perguntas. As respostas a essas perguntas determinará como os filtros de âmbito e mapeamentos de atributo tem de ser definido.

* **O que os utilizadores no Workday tem de ser aprovisionada para esta floresta do Active Directory?**

   * *Exemplo: Os utilizadores em que o atributo do dia de trabalho "da empresa" contém o valor de "Contoso", e o atributo "Worker_Type" contém "Normais"*

* **Como são encaminhados os usuários em unidades diferentes organizacionais (UO)?**

   * *Exemplo: Os utilizadores são encaminhados para UOs que correspondem a uma localização do office, conforme definido no Workday "Município" e "Country_Region_Reference" atributos*

* **Como devem ser preenchidos os seguintes atributos no Active Directory?**

   * Nome comum (cn)
      * *Exemplo: Utilizar o valor Workday User_ID, conforme definido pelos recursos humanos*
      
   * Identificação do funcionário (employeeId)
      * *Exemplo: Utilize o valor do Workday Worker_ID*
      
   * Nome da conta SAM (sAMAccountName)
      * *Exemplo: Utilizar o valor Workday User_ID, filtrado por meio de um Azure AD aprovisionamento expressão para remover os carateres ilegais*
      
   * Nome do Principal de utilizador (userPrincipalName)
      * *Exemplo: Utilizar o valor Workday User_ID, com um Azure AD aprovisionamento expressão para acrescentar um nome de domínio*

* **Como devem ser correspondidos utilizadores entre o dia de trabalho e o Active Directory?**

  * *Exemplo: Os utilizadores com um dia de trabalho específico "Worker_ID", valor são correspondidas com utilizadores do Active Directory em que "employeeID" tem o mesmo valor. Se o valor de Worker_ID não for encontrado no Active Directory, em seguida, crie um novo utilizador.*
  
* **Floresta do Active Directory já contém o usuário IDs necessários para a lógica correspondente para trabalhar?**

  * *Exemplo: Se esta for uma nova implementação do Workday, recomenda-se vivamente que do Active Directory ser previamente preenchido com os valores corretos do Workday Worker_ID (ou valor de ID exclusivo da preferência) para manter a lógica correspondente mais simples possível.*



Como definir e configurar estas aplicações de conector de aprovisionamento especiais é o assunto das seções restantes deste tutorial. Que pode optar por configurar as aplicações dependem são de inquilinos que sistemas terá de aprovisionar e número de domínios do Active Directory e do Azure AD no seu ambiente.



## <a name="configure-integration-system-user-in-workday"></a>Configurar o utilizador de sistema de integração no Workday

Um requisito comum de todos os Workday aprovisionamento conectores é necessitam de credenciais para uma conta de integração de sistema do Workday ligar à API de recursos humanos do Workday. Esta secção descreve como criar um utilizador de sistema de integração no Workday.

> [!NOTE]
> É possível ignorar este procedimento em vez disso, uma conta de administrador global do Workday que a conta de integração do sistema. Isso pode funcionar bem para demonstrações, mas não é recomendado para implementações de produção.

### <a name="create-an-integration-system-user"></a>Criar um utilizador de sistema de integração

**Para criar um utilizador de sistema de integração:**

1. Inicie sessão no seu inquilino do Workday através de uma conta de administrador. Na **Workday aplicativo**, introduza criar utilizador na caixa de pesquisa e, em seguida, clique em **criar utilizador do sistema de integração**.

    ![Criar utilizador](./media/workday-inbound-tutorial/wd_isu_01.png "criar utilizador")
2. Concluir o **criar utilizador do sistema de integração** tarefa ao fornecer um nome de utilizador e palavra-passe para um novo utilizador do sistema de integração.  
 * Deixe o **requerem nova palavra-passe no próximo sessão** opção desmarcada, uma vez que este utilizador será logon por meio de programação.
 * Deixe o **minutos de tempo limite da sessão** com o respetivo valor predefinido de 0, que irá impedir sessões do usuário exceder o tempo limite prematuramente.
 * Selecione a opção **fazer permitir sessões de interface do Usuário** pois ele fornece uma camada adicional de segurança que impede que um utilizador com a palavra-passe do sistema de integração de iniciar sessão no Workday. 

    ![Criar utilizador do sistema de integração](./media/workday-inbound-tutorial/wd_isu_02.png "criar utilizador do sistema de integração")

### <a name="create-a-security-group"></a>Criar um grupo de segurança
Neste passo, irá criar um grupo de segurança do sistema de integração irrestrita no Workday e atribua o utilizador de sistema de integração criado no passo anterior a este grupo.

**Para criar um grupo de segurança:**

1. Introduza criar grupo de segurança na caixa de pesquisa e, em seguida, clique em **criar grupo de segurança**.

    ![Grupo de CreateSecurity](./media/workday-inbound-tutorial/wd_isu_03.png "CreateSecurity grupo")
2. Concluir o **criar grupo de segurança** tarefas.  
   * Selecione **grupo de segurança de sistema de integração (Unconstrained)** partir a **tipo de inquilinos grupo de segurança** lista pendente.

    ![Grupo de CreateSecurity](./media/workday-inbound-tutorial/wd_isu_04.png "CreateSecurity grupo")

3. Após a criação do grupo de segurança com êxito, verá uma página onde a pode atribuir membros ao grupo de segurança. Adicione o novo utilizador do sistema de integração para este grupo de segurança e selecione o âmbito da organização adequada.
![Editar grupo de segurança](./media/workday-inbound-tutorial/wd_isu_05.png "Editar grupo de segurança")
 
### <a name="configure-domain-security-policy-permissions"></a>Configurar permissões de política de segurança de domínio
Neste passo, terá de conceder permissões de política para os dados de trabalho para o grupo de segurança "segurança de domínio".

**Para configurar permissões de política de segurança de domínio:**

1. Introduza **configuração de segurança do domínio** na caixa de pesquisa e, em seguida, clique no link **relatório de configuração de segurança do domínio**.  

    ![As políticas de segurança do domínio](./media/workday-inbound-tutorial/wd_isu_06.png "políticas de segurança de domínio")  
2. Na **domínio** caixa de texto, procure os seguintes domínios e adicioná-los para o filtro individualmente.  
   * *Aprovisionamento de conta externa*
   * *Dados de trabalho: Relatórios de trabalho público*
   * *Dados de pessoa: Informações de contacto de trabalho*
   * *Dados de trabalho: Posições de todos os*
   * *Dados de trabalho: Atual a equipe de informações*
   * *Dados de trabalho: Título de negócios no perfil de trabalho*
 
    ![As políticas de segurança do domínio](./media/workday-inbound-tutorial/wd_isu_07.png "políticas de segurança de domínio")  

    ![As políticas de segurança do domínio](./media/workday-inbound-tutorial/wd_isu_08.png "políticas de segurança de domínio") 

    Clique em **OK**.

3. No relatório que aparece, selecione as reticências (...), que é apresentado junto a **aprovisionamento da conta externa** e clique na opção de menu **domínio -> Editar permissões de política de segurança**

    ![As políticas de segurança do domínio](./media/workday-inbound-tutorial/wd_isu_09.png "políticas de segurança de domínio")  

4. Sobre o **editar permissões de política de segurança de domínio** página, desloque para baixo para a seção **permissões de integração**. Clique no sinal "+" para adicionar o grupo de sistema de integração para a lista de grupos de segurança com **Obtenha** e **colocar** permissões de integração.

    ![Editar permissão](./media/workday-inbound-tutorial/wd_isu_10.png "Editar permissão")  

5. Clique no sinal "+" para adicionar o grupo de sistema de integração para a lista de grupos de segurança com **Obtenha** e **colocar** permissões de integração.

    ![Editar permissão](./media/workday-inbound-tutorial/wd_isu_11.png "Editar permissão")  

6. Repita os passos 3 a 5 acima para cada uma destas políticas de segurança restantes:

   | Operação | Política de segurança de domínio |
   | ---------- | ---------- | 
   | Get e Put | Dados de trabalho: Relatórios de trabalho público |
   | Get e Put | Dados de pessoa: Informações de contacto de trabalho |
   | Get | Dados de trabalho: Posições de todos os |
   | Get | Dados de trabalho: Atual a equipe de informações |
   | Get | Dados de trabalho: Título de negócios no perfil de trabalho |

### <a name="configure-business-process-security-policy-permissions"></a>Configurar permissões de política de segurança de processo de negócio
Neste passo, terá de conceder permissões de política para os dados de trabalho para o grupo de segurança "segurança do processo de negócio". Isto é necessário para configurar o conector de aplicações de repetição de escrita do Workday. 

**Para configurar permissões de política de segurança de processos de negócio:**

1. Introduza **política de processo de negócio** na caixa de pesquisa e, em seguida, clique no link **Editar política de segurança de processo de negócio** tarefas.  

    ![As políticas de segurança do processo de negócio](./media/workday-inbound-tutorial/wd_isu_12.png "políticas de segurança do processo de negócio")  

2. Na **tipo de processo de negócio** caixa de texto, procure *contacto* e selecione **contacto alteração** processo empresarial e clique em **OK**.

    ![As políticas de segurança do processo de negócio](./media/workday-inbound-tutorial/wd_isu_13.png "políticas de segurança do processo de negócio")  

3. Sobre o **Editar política de segurança de processo de negócio** página, desloque-se para o **manter informações de contacto (serviço Web)** secção.

    ![As políticas de segurança do processo de negócio](./media/workday-inbound-tutorial/wd_isu_14.png "políticas de segurança do processo de negócio")  

4. Selecione e adicione o novo grupo de segurança do sistema de integração para a lista de grupos de segurança que podem iniciar a solicitação de serviços da web. Clique em **feito**. 

    ![As políticas de segurança do processo de negócio](./media/workday-inbound-tutorial/wd_isu_15.png "políticas de segurança do processo de negócio")  

 
### <a name="activate-security-policy-changes"></a>Ativar as alterações de política de segurança

**Para ativar as alterações de política de segurança:**

1. Introduza ativar na caixa de pesquisa e, em seguida, clique no link **ativar alterações de política de segurança pendentes**.

    ![Ativar](./media/workday-inbound-tutorial/wd_isu_16.png "ativar") 
2. Comece a tarefa ativar alterações de política de segurança pendentes por introduzir um comentário para fins de auditoria e, em seguida, clique em **OK**. 

    ![Ativar pendentes de segurança](./media/workday-inbound-tutorial/wd_isu_17.png "ativar pendentes de segurança")  
1. Concluir a tarefa no ecrã seguinte, marcando a caixa de verificação **confirmar**e, em seguida, clique em **OK**.

    ![Ativar pendentes de segurança](./media/workday-inbound-tutorial/wd_isu_18.png "ativar pendentes de segurança")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Configurar o aprovisionamento de utilizadores do Workday para o Active Directory

Siga estas instruções para configurar o aprovisionamento a partir do Workday para cada domínio do Active Directory dentro do escopo de sua integração de conta de utilizador.

### <a name="part-1-install-and-configure-on-premises-provisioning-agents"></a>Parte 1: Instalar e configurar os agentes de aprovisionamento no local

Para aprovisionar ao Active Directory no local, um agente tem de estar instalado num servidor que tem o .NET 4.7 + Framework e acesso à rede para os domínios do Active Directory pretendidos.

> [!TIP]
> Pode verificar a versão do .NET framework no seu servidor com as instruções fornecidas [aqui](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed).
> Se o servidor não tem o .NET 4.7 ou superior instalado, pode transferi-lo no [aqui](https://support.microsoft.com/help/3186497/the-net-framework-4-7-offline-installer-for-windows).  

Depois de ter implementado o .NET 4.7 +, pode baixar o **[agente aprovisionamento aqui locais](https://go.microsoft.com/fwlink/?linkid=847801)** e siga os passos abaixo para concluir a configuração do agente.

1. Início de sessão para o Windows Server onde pretende instalar o agente de novo.
2. Inicie o instalador do agente de aprovisionamento, aceitar os termos e clique nas **instalar** botão.
![Instalar o ecrã](./media/workday-inbound-tutorial/pa_install_screen_1.png "instalar ecrã")

3. Após a instalação estiver concluída, será iniciado o assistente e verá a **Connect do Azure AD** ecrã. Clique nas **Authenticate** botão para ligar à sua instância do Azure AD.
![Ligar o Azure AD](./media/workday-inbound-tutorial/pa_install_screen_2.png "ligam o Azure AD")

4. Autenticar-se à sua instância do Azure AD com credenciais de Administrador Global. 
![Autenticação de administrador](./media/workday-inbound-tutorial/pa_install_screen_3.png "autenticação de Admin")

5. Após a autenticação com êxito com o Azure AD, verá os **ligar o Active Directory** ecrã. Neste passo, introduza o seu nome de domínio do AD e clique nas **Adicionar diretório** botão.
![Adicionar diretório](./media/workday-inbound-tutorial/pa_install_screen_4.png "Adicionar diretório")

6. Agora será solicitado para introduzir as credenciais necessárias para ligar ao domínio do AD. No mesmo ecrã, pode utilizar o **selecione a prioridade de controlador de domínio** para especificar controladores de domínio que o agente irá usar para o envio de solicitações de provisionamento.
![Credenciais do domínio](./media/workday-inbound-tutorial/pa_install_screen_5.png "credenciais do domínio")

7. Depois de configurar o domínio, o instalador apresenta uma lista de domínios configurados. Neste ecrã, pode repetir o passo #5 e 6 # para adicionar mais domínios ou clique em **seguinte** para avançar para o registo do agente. 
![Configurar domínios](./media/workday-inbound-tutorial/pa_install_screen_6.png "configurado domínios")

   > [!NOTE]
   > Se tiver vários domínios de AD (por exemplo, na.contoso.com, emea.contoso.com), adicione cada domínio individualmente à lista. Não é suficiente apenas a adição de domínio principal (por exemplo, contoso.com) e recomenda-se que registrar cada domínio subordinado com o agente. 

8. Reveja os detalhes de configuração e clique em **confirmar** para registar o agente. 
![Confirmar a tela](./media/workday-inbound-tutorial/pa_install_screen_7.png "confirmar ecrã")

9. O Assistente de configuração apresenta o progresso do registo do agente.
![Registo do agente](./media/workday-inbound-tutorial/pa_install_screen_8.png "registo do agente")

10. Assim que o registo do agente é efetuada com êxito, pode clicar em **sair** para sair do assistente. 
![Sair do ecrã](./media/workday-inbound-tutorial/pa_install_screen_9.png "sair do ecrã")

11. Verificar a instalação do agente e certifique-se de que está em execução ao abrir o Snap-In "Serviços" e procura o serviço com o nome "Microsoft Azure AD Connect aprovisionamento Agent" ![serviços](./media/workday-inbound-tutorial/services.png)  


**Resolução de problemas de agente**

O [registo de eventos do Windows](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) no Windows Server que aloja o agente de máquina contém eventos para todas as operações executadas pelo agente. Para ver esses eventos:
    
1. Open **eventvwr. msc**.
2. Selecione **registos do Windows > aplicação**.
3. Ver todos os eventos registados na origem **AAD. Connect.ProvisioningAgent**. 
4. Verificar a existência de erros e avisos.

    
### <a name="part-2-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Parte 2: Adicionar a aplicação de conector de aprovisionamento e criar a ligação ao Workday

**Para configurar o Workday para o aprovisionamento do Active Directory:**

1.  Ir para <https://portal.azure.com>

2.  Na barra de navegação esquerdo, selecione **do Azure Active Directory**

3.  Selecione **aplicações empresariais**, em seguida, **todos os aplicativos**.

4.  Selecione **adicionar uma aplicação**e selecione o **todos os** categoria.

5.  Procure **Workday Provisioning para Active Directory**e adicionar essa aplicação a partir da galeria.

6.  Depois da aplicação é adicionada e o ecrã de detalhes da aplicação é apresentado, selecione **aprovisionamento**

7.  Alteração da **aprovisionamento** **modo** para **automática**

8.  Concluir o **credenciais de administrador** secção da seguinte forma:

   * **Nome de utilizador administrador** – introduza o nome de utilizador da conta de sistema de integração do Workday, com o nome de domínio de inquilino anexado. **Deve ser algo semelhante: username@contoso4**

   * **Palavra-passe de administrador –** introduza a palavra-passe da conta de sistema de integração do Workday

   * **URL – de inquilinos** introduza o URL para o ponto de extremidade do serviços de web do Workday para o seu inquilino. Isso deve ser semelhante: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources, onde contoso4 é substituído pelo nome do seu inquilino correto e wd3 impl é substituído com a cadeia de caracteres do ambiente correto.

   * **Floresta de diretório do Active Directory -** o "nome" do seu domínio do Active Directory, como registrado com o agente. Isso geralmente consiste numa cadeia de caracteres, como: *contoso.com*

   * **Contentor do Active Directory -** introduzir o DN do contêiner em que o agente deve criar contas de utilizador por predefinição. 
        Exemplo: *UO = usuários padrão, UO = Users, DC = contoso, DC = test*
> [!NOTE]
> Esta definição só entra em jogo para criação de contas de utilizador se o *parentDistinguishedName* atributo não é configurado nos mapeamentos de atributos. Esta definição não é utilizada para pesquisa de usuário ou atualizar operações. A árvore de sub-rotina de todo o domínio está no âmbito da operação de pesquisa.
   * **E-Mail de notificação –** introduza o seu endereço de e-mail e marque a caixa de verificação "enviar e-mail se ocorrer uma falha".
> [!NOTE]
> O serviço de aprovisionamento do AD do Azure envia a notificação por e-mail se o trabalho de aprovisionamento vai para um [quarentena](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning#quarantine) estado.

   * Clique nas **Testar ligação** botão. Se o teste de ligação for bem-sucedida, clique nas **guardar** botão na parte superior. Se falhar, verifique novamente que as credenciais do Workday e as credenciais de AD configuradas na configuração de agente são válidas.

![Portal do Azure](./media/workday-inbound-tutorial/wd_1.png)

### <a name="part-2-configure-attribute-mappings"></a>Parte 2: Configurar mapeamentos de atributos 

Nesta secção, irá configurar como os dados de utilizador fluem a partir do Workday para o Active Directory.

1.  No separador aprovisionamento em **mapeamentos**, clique em **sincronizar trabalhadores do Workday para OnPremises**.

2.  Na **âmbito de objeto de origem** campo, pode selecionar quais conjuntos de utilizadores no Workday devem estar no âmbito de aprovisionamento para o AD, definindo um conjunto de filtros baseados em atributo. O escopo de padrão é "todos os utilizadores no Workday". Filtros de exemplo:

   * Exemplo: Âmbito para os utilizadores com os IDs de trabalho entre 1000000 e 2000000

      * Atributo: WorkerID

      * Operador: Correspondência REGEX

      * Valor: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Exemplo: Apenas os funcionários e não os operadores de contingência 

      * Atributo: EmployeeID

      * Operador: Não é nulo

3.  Na **ações do objeto de destino** campo, pode filtrar globalmente que ações podem ser executados no Active Directory. **Crie** e **atualização** são mais comuns.

4.  Na **mapeamentos de atributo** secção, pode definir o dia de trabalho individual como atributos do mapa para atributos do Active Directory.

5. Clique num mapeamento de atributo existente para atualizá-lo ou clique em **adicionar novo mapeamento** na parte inferior do ecrã, para adicionar novos mapeamentos. Um mapeamento do atributo individual suporta estas propriedades:

      * **Tipo de mapeamento**

         * **Direto** – escreve o valor do atributo do Workday para o atributo de AD, sem alterações

         * **Constante** -escrever um valor de cadeia de caracteres constante, estático para o atributo de AD

         * **Expressão** – permite-lhe escrever um valor personalizado para o atributo de AD, com base num ou mais atributos do Workday. [Para obter mais informações, veja este artigo sobre expressões](../manage-apps/functions-for-customizing-application-data.md).

      * **Atributo de origem** -o atributo de utilizador a partir do Workday. Se o atributo que procura não estiver presente, veja [personalizar a lista de atributos de utilizador do Workday](#customizing-the-list-of-workday-user-attributes).

      * **Valor predefinido** – opcional. Se o atributo de origem tem um valor vazio, o mapeamento de escrever este valor em vez disso.
            Configuração mais comuns é deixar em branco.

      * **Atributo de destino** – o atributo de utilizador no Active Directory.

      * **Correspondência de objetos utilizando este atributo** – independentemente desse mapeamento deve ser utilizado para identificar exclusivamente os utilizadores entre o dia de trabalho e o Active Directory. Isso normalmente é definido no campo de ID de função de trabalho para Workday, que normalmente é mapeado para um dos atributos de identificação do funcionário no Active Directory.

      * **Precedência de correspondência** – o várias correspondência de atributos pode ser definida. Quando houver múltiplos, eles são avaliados na ordem definida por este campo. Assim que uma correspondência for encontrada, não são necessárias mais correspondência de atributos são avaliados.

      * **Aplicar este mapeamento**
       
         * **Sempre** – aplicar este mapeamento em ambos os criação de utilizador e ações de atualização

         * **Apenas durante a criação** -aplicar este mapeamento apenas nas ações de criação do utilizador

6. Para guardar os seus mapeamentos, clique em **guardar** na parte superior da seção de mapeamento do atributo.

![Portal do Azure](./media/workday-inbound-tutorial/WD_2.PNG)

**Seguem-se alguns exemplos mapeamentos de atributos entre o dia de trabalho e o Active Directory, com algumas expressões comuns**

-   A expressão que mapeia para o atributo parentDistinguishedName é utilizada para aprovisionar utilizadores para diferentes UOs com base num ou mais atributos de origem do Workday. Este exemplo coloca os usuários em diferentes UOs com base em que Localidade se encontram.

-   O atributo userPrincipalName no Active Directory é gerado pela concatenação o ID de utilizador do dia de trabalho com um sufixo de domínio

-   [Há documentação sobre como escrever expressões aqui](../manage-apps/functions-for-customizing-application-data.md). Isto inclui exemplos sobre como remover os carateres especiais.

  
| ATRIBUTO DE WORKDAY | ATRIBUTO DO ACTIVE DIRECTORY |  CORRESPONDÊNCIA DE ID? | CRIAR / ATUALIZAR |
| ---------- | ---------- | ---------- | ---------- |
| **WorkerID**  |  employeeID | **Sim** | Escrito na criação de apenas |
| **UserID**    |  CN    |   |   Escrito na criação de apenas |
| **Junte-se a ("@", [ID de utilizador], "contoso.com")**   | userPrincipalName     |     | Escrito na criação de apenas 
| **Replace(Mid(Replace(\[UserID\], , "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\\\&lt;\\\\&gt;\])", , "", , ), 1, 20), , "([\\\\.)\*\$](file:///\\.)*$)", , "", , )**      |    sAMAccountName            |     |         Escrito na criação de apenas |
| **Comutador (\[Active Directory\],, "0", "True", "1", "False")** |  accountDisabled      |     | Criar + atualizar |
| **FirstName**   | givenName       |     |    Criar + atualizar |
| **LastName**   |   sn   |     |  Criar + atualizar |
| **PreferredNameData**  |  displayName |     |   Criar + atualizar |
| **Empresa**         | Empresa   |     |  Criar + atualizar |
| **SupervisoryOrganization**  | Departamento  |     |  Criar + atualizar |
| **ManagerReference**   | gestor  |     |  Criar + atualizar |
| **BusinessTitle**   |  título     |     |  Criar + atualizar | 
| **AddressLineData**    |  streetAddress  |     |   Criar + atualizar |
| **Município**   |   l   |     | Criar + atualizar |
| **CountryReferenceTwoLetter**      |   Co |     |   Criar + atualizar |
| **CountryReferenceTwoLetter**    |  c  |     |         Criar + atualizar |
| **CountryRegionReference** |  St     |     | Criar + atualizar |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Criar + atualizar |
| **PostalCode**  |   postalCode  |     | Criar + atualizar |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Criar + atualizar |
| **Fax**      | facsimileTelephoneNumber     |     |    Criar + atualizar |
| **móvel**  |    móvel       |     |       Criar + atualizar |
| **LocalReference** |  preferredLanguage  |     |  Criar + atualizar |                                               
| **Comutador (\[município\], "UO = usuários padrão, UO = Users, UO = Default, UO = localizações, DC = contoso, DC = com", "Dallas", "UO = para os usuários padrão, a UO = Users, UO = Dallas, UO = localizações, DC = contoso, DC = com", "Austin", "UO = para os usuários padrão, a UO = Users, UO = Austin, UO = localizações, DC = contoso, DC = com ","Seattle"," UO = para os usuários padrão, a UO = Users, UO = Seattle, UO = localizações, DC = contoso, DC = com ","Londres"," UO = usuários padrão, UO = Users, UO = Londres, UO = localizações, DC = contoso, DC = com ")**  | parentDistinguishedName     |     |  Criar + atualizar |
  


### <a name="part-4-start-the-service"></a>Parte 4: Iniciar o serviço
Uma vez, partes 1 a 3 tem sido concluídas, pode iniciar o serviço de aprovisionamento no portal do Azure.

1.  Na **aprovisionamento** separador, defina o **estado de aprovisionamento** para **no**.

2. Clique em **Guardar**.

3. Isso iniciará a sincronização inicial, o que pode demorar um número variável de horas, consoante o número de utilizadores é no Workday.

4. Em qualquer altura, verifique os **registos de auditoria** separador no portal do Azure para ver as ações que efetuou o serviço de aprovisionamento. Os registos de auditoria apresenta uma lista de todos os eventos de sincronização individuais realizados pelo serviço de aprovisionamento, por exemplo, o que os utilizadores estão a ser lido de dia de trabalho e, em seguida, em seguida, adicionados ou atualizados para o Active Directory. **[Consulte o guia de criação de relatórios de aprovisionamento para obter instruções detalhadas sobre como ler os registos de auditoria](../manage-apps/check-status-user-account-provisioning.md)**

1.  Verifique os [registo de eventos do Windows](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) na máquina do Windows Server que aloja o agente para novos erros ou avisos. Esses eventos são exibidos ao iniciar **eventvwr. msc** no servidor e selecionando **registos do Windows > aplicação**. Todas as mensagens relacionadas com o aprovisionamento são registadas na origem **AADSyncAgent**.

6. Um concluído, ele irá escrever um relatório de resumo de auditoria no **aprovisionamento** separador, conforme mostrado abaixo.

![Portal do Azure](./media/workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-to-azure-active-directory"></a>Configurar o aprovisionamento para o Azure Active Directory
Como configurar o aprovisionamento do Azure Active Directory irá depender dos requisitos de aprovisionamento, conforme detalhado na tabela abaixo.

| Cenário | Solução |
| -------- | -------- |
| **Os utilizadores têm de ser aprovisionada para o Active Directory e o Azure AD** | Uso  **[o AAD Connect](../hybrid/whatis-hybrid-identity.md)** |
| **Os utilizadores têm de ser aprovisionado apenas ao Active Directory** | Uso  **[o AAD Connect](../hybrid/whatis-hybrid-identity.md)** |
| **Os utilizadores têm de ser aprovisionado com o Azure AD apenas (apenas na nuvem)** | Utilize o **Workday para o aprovisionamento do Azure Active Directory** aplicação na Galeria de aplicações |

Para obter instruções sobre como configurar o Azure AD Connect, consulte a [documentação do Azure AD Connect](../hybrid/whatis-hybrid-identity.md).

As secções seguintes descrevem como configurar uma ligação entre o dia de trabalho e o Azure AD para aprovisionar os utilizadores apenas na cloud.

> [!IMPORTANT]
> Apenas siga o procedimento a seguir se tiver utilizadores de apenas na cloud que necessitam de ser aprovisionada para o Azure AD e não no local do Active Directory.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Parte 1: Adicionar a aplicação do conector de aprovisionamento do Azure AD e criar a ligação ao Workday

**Para configurar o Workday para o aprovisionamento do Azure Active Directory para utilizadores apenas na cloud:**

1.  Aceda a <https://portal.azure.com>.

2.  Na barra de navegação esquerdo, selecione **do Azure Active Directory**

3.  Selecione **aplicações empresariais**, em seguida, **todos os aplicativos**.

4.  Selecione **adicionar uma aplicação**e, em seguida, selecione a **todos os** categoria.

5.  Procure **Workday para o aprovisionamento do Azure AD**e adicionar essa aplicação a partir da galeria.

6.  Depois da aplicação é adicionada e o ecrã de detalhes da aplicação é apresentado, selecione **aprovisionamento**

7.  Alteração da **aprovisionamento** **modo** para **automática**

8.  Concluir o **credenciais de administrador** secção da seguinte forma:

   * **Nome de utilizador administrador** – introduza o nome de utilizador da conta de sistema de integração do Workday, com o nome de domínio de inquilino anexado. Deve ser algo semelhante: username@contoso4

   * **Palavra-passe de administrador –** introduza a palavra-passe da conta de sistema de integração do Workday

   * **URL – de inquilinos** introduza o URL para o ponto de extremidade do serviços de web do Workday para o seu inquilino. Isso deve ser semelhante: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources, onde contoso4 é substituído pelo nome do seu inquilino correto e wd3 impl é substituído com a cadeia de caracteres do ambiente correto. Se este URL não é conhecido, trabalhe em conjunto com o seu representante de suporte ou de parceiro de integração de Workday para determinar o URL correto a utilizar.

   * **E-Mail de notificação –** introduza o seu endereço de e-mail e marque a caixa de verificação "enviar e-mail se ocorrer uma falha".

   * Clique nas **Testar ligação** botão.

   * Se o teste de ligação for bem-sucedida, clique nas **guardar** botão na parte superior. Se falhar, verifique novamente que o URL de dia de trabalho e as credenciais são válidas no Workday.

### <a name="part-2-configure-attribute-mappings"></a>Parte 2: Configurar mapeamentos de atributos 

Nesta secção, irá configurar como os dados de utilizador fluem do Workday para o Azure Active Directory para utilizadores apenas na cloud.

1. No separador aprovisionamento em **mapeamentos**, clique em **sincronizar funções de trabalho para o Azure AD**.

2. Na **âmbito de objeto de origem** campo, pode selecionar quais conjuntos de utilizadores no Workday devem estar no âmbito de aprovisionamento para o Azure AD, definindo um conjunto de filtros baseados em atributo. O escopo de padrão é "todos os utilizadores no Workday". Filtros de exemplo:

   * Exemplo: Âmbito para os utilizadores com os IDs de trabalho entre 1000000 e 2000000

      * Atributo: WorkerID

      * Operador: Correspondência REGEX

      * Valor: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Exemplo: Os operadores de contingência apenas e funcionários não regulares

      * Atributo: ContingentID

      * Operador: Não é nulo

3. Na **ações do objeto de destino** campo, pode filtrar globalmente que ações podem ser executadas no Azure AD. **Crie** e **atualização** são mais comuns.

4. Na **mapeamentos de atributo** secção, pode definir o dia de trabalho individual como atributos do mapa para atributos do Active Directory.

5. Clique num mapeamento de atributo existente para atualizá-lo ou clique em **adicionar novo mapeamento** na parte inferior do ecrã, para adicionar novos mapeamentos. Um mapeamento do atributo individual suporta estas propriedades:

   * **Tipo de mapeamento**

      * **Direto** – escreve o valor do atributo do Workday para o atributo de AD, sem alterações

      * **Constante** -escrever um valor de cadeia de caracteres constante, estático para o atributo de AD

      * **Expressão** – permite-lhe escrever um valor personalizado para o atributo de AD, com base num ou mais atributos do Workday. [Para obter mais informações, veja este artigo sobre expressões](../manage-apps/functions-for-customizing-application-data.md).

   * **Atributo de origem** -o atributo de utilizador a partir do Workday. Se o atributo que procura não estiver presente, veja [personalizar a lista de atributos de utilizador do Workday](#customizing-the-list-of-workday-user-attributes).

   * **Valor predefinido** – opcional. Se o atributo de origem tem um valor vazio, o mapeamento de escrever este valor em vez disso.
            Configuração mais comuns é deixar em branco.

   * **Atributo de destino** – o atributo de utilizador no Azure AD.

   * **Correspondência de objetos utilizando este atributo** – independentemente desse mapeamento deve ser utilizado para identificar exclusivamente os utilizadores entre o dia de trabalho e o Azure AD. Isso normalmente é definido no campo de ID de função de trabalho para Workday, que normalmente é mapeado para o atributo de identificação do funcionário (novo) ou um atributo de extensão no Azure AD.

   * **Precedência de correspondência** – o várias correspondência de atributos pode ser definida. Quando houver múltiplos, eles são avaliados na ordem definida por este campo. Assim que uma correspondência for encontrada, não são necessárias mais correspondência de atributos são avaliados.

   * **Aplicar este mapeamento**

     * **Sempre** – aplicar este mapeamento em ambos os criação de utilizador e ações de atualização

     * **Apenas durante a criação** -aplicar este mapeamento apenas nas ações de criação do utilizador

6. Para guardar os seus mapeamentos, clique em **guardar** na parte superior da seção de mapeamento do atributo.

### <a name="part-3-start-the-service"></a>Parte 3: Iniciar o serviço
Uma vez, partes 1 a 2 estiverem concluídos, pode iniciar o serviço de aprovisionamento.

1. Na **aprovisionamento** separador, defina o **estado de aprovisionamento** para **no**.

2. Clique em **Guardar**.

3. Isso iniciará a sincronização inicial, o que pode demorar um número variável de horas, consoante o número de utilizadores é no Workday.

4. Eventos de sincronização individuais podem ser visualizados no **registos de auditoria** separador. **[Consulte o guia de criação de relatórios de aprovisionamento para obter instruções detalhadas sobre como ler os registos de auditoria](../manage-apps/check-status-user-account-provisioning.md)**

5. Um concluído, ele irá escrever um relatório de resumo de auditoria no **aprovisionamento** separador, conforme mostrado abaixo.

## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Configurar a repetição de escrita de endereços de e-mail para Workday
Siga estas instruções para configurar a repetição de escrita de endereços de e-mail do utilizador do Azure Active Directory para Workday.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Parte 1: Adicionar a aplicação de conector de aprovisionamento e criar a ligação ao Workday

**Para configurar o conector de repetição de escrita do dia de trabalho:**

1. Ir para <https://portal.azure.com>

2. Na barra de navegação esquerdo, selecione **do Azure Active Directory**

3. Selecione **aplicações empresariais**, em seguida, **todos os aplicativos**.

4. Selecione **adicionar uma aplicação**, em seguida, selecione a **todos os** categoria.

5. Procure **repetição de escrita do Workday**e adicionar essa aplicação a partir da galeria.

6. Depois da aplicação é adicionada e o ecrã de detalhes da aplicação é apresentado, selecione **aprovisionamento**

7. Alteração da **aprovisionamento** **modo** para **automática**

8. Concluir o **credenciais de administrador** secção da seguinte forma:

   * **Nome de utilizador administrador** – introduza o nome de utilizador da conta de sistema de integração do Workday, com o nome de domínio de inquilino anexado. Deve ser algo semelhante: username@contoso4

   * **Palavra-passe de administrador –** introduza a palavra-passe da conta de sistema de integração do Workday

   * **URL – de inquilinos** introduza o URL para o ponto de extremidade do serviços de web do Workday para o seu inquilino. Isso deve ser semelhante: https://wd3-impl-services1.workday.com/ccx/service/contoso4/Human_Resources, onde contoso4 é substituído pelo nome do seu inquilino correto e wd3 impl é substituído com a cadeia de caracteres do ambiente correto (se necessário).

   * **E-Mail de notificação –** introduza o seu endereço de e-mail e marque a caixa de verificação "enviar e-mail se ocorrer uma falha".

   * Clique nas **Testar ligação** botão. Se o teste de ligação for bem-sucedida, clique nas **guardar** botão na parte superior. Se falhar, verifique novamente que o URL de dia de trabalho e as credenciais são válidas no Workday.

### <a name="part-2-configure-attribute-mappings"></a>Parte 2: Configurar mapeamentos de atributos 

Nesta secção, irá configurar como os dados de utilizador fluem a partir do Workday para o Active Directory.

1. No separador aprovisionamento em **mapeamentos**, clique em **sincronizar do Azure AD utilizadores no Workday**.

2. Na **âmbito de objeto de origem** campo, pode, opcionalmente, filtrar quais conjuntos de utilizadores no Azure Active Directory devem ter seus endereços de e-mail repetição de escrita Workday. O escopo de padrão é "todos os utilizadores no Azure AD". 

3. Na **mapeamentos de atributo** secção, atualize o ID correspondente para indicar o atributo no Azure Active Directory onde está armazenada o ID de trabalho do dia de trabalho ou a identificação do funcionário. É um método correspondente popular sincronizar o ID de trabalho de dia de trabalho ou a identificação do funcionário para extensionAttribute1 15 no Azure AD e, em seguida, utilizar esse atributo no Azure AD para corresponder a utilizadores no Workday. 

4. Para guardar os seus mapeamentos, clique em **guardar** na parte superior da seção de mapeamento do atributo.

### <a name="part-3-start-the-service"></a>Parte 3: Iniciar o serviço
Uma vez, partes 1 a 2 estiverem concluídos, pode iniciar o serviço de aprovisionamento.

1. Na **aprovisionamento** separador, defina o **estado de aprovisionamento** para **no**.

2. Clique em **Guardar**.

3. Isso iniciará a sincronização inicial, o que pode demorar um número variável de horas, consoante o número de utilizadores é no Workday.

4. Eventos de sincronização individuais podem ser visualizados no **registos de auditoria** separador. **[Consulte o guia de criação de relatórios de aprovisionamento para obter instruções detalhadas sobre como ler os registos de auditoria](../manage-apps/check-status-user-account-provisioning.md)**

5. Um concluído, ele irá escrever um relatório de resumo de auditoria no **aprovisionamento** separador, conforme mostrado abaixo.

## <a name="customizing-the-list-of-workday-user-attributes"></a>Personalizar a lista de atributos de utilizador do Workday
O Workday aprovisionamento de aplicações para o Active Directory e o Azure AD incluem uma lista de padrão de atributos de utilizador do Workday pode selecionar. No entanto, essas listas não são abrangentes. Workday oferece suporte a várias centenas de atributos de utilizador possível, o que podem ser exclusivo para o seu inquilino do Workday ou padrão. 

O Azure AD que o serviço de aprovisionamento suporta a capacidade de personalizar a sua lista ou um atributo do Workday para incluir todos os atributos expostos no [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) operação da API de recursos humanos.

Para tal, tem de utilizar [Workday Studio](https://community.workday.com/studio-download) para extrair as expressões XPath que representam os atributos que deseja usar e, em seguida, adicioná-los à sua configuração de aprovisionamento com o editor de atributos avançadas no portal do Azure.

**Para obter uma expressão de XPath para um atributo de utilizador do dia de trabalho:**

1. Transfira e instale [Workday Studio](https://community.workday.com/studio-download). Precisará de uma conta de Comunidade do Workday para acessar o instalador.

2. Transfira o ficheiro de Workday Human_Resources WDSL a partir deste URL: https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Human_Resources.wsdl

3. Inicie Workday Studio.

4. Na barra de comandos, selecione o **Workday > serviço Web de teste no Tester** opção.

5. Selecione **externo**e selecione o ficheiro Human_Resources WSDL que transferiu no passo 2.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio1.png)

6. Definir o **localização** campo `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources`, mas substituindo "IMPL CC" real com tipo de instância e "INQUILINO" com o nome do seu inquilino real.

7. Definir **operação** para **Get_Workers**

8.  Clique em pequenos **configurar** link abaixo os painéis de solicitação/resposta para definir as suas credenciais do Workday. Verifique **autenticação**e, em seguida, introduza o nome de utilizador e palavra-passe para a sua conta de sistema de integração do Workday. Certifique-se de que formatar o nome de utilizador como name@tenante deixe o **WS-Security UsernameToken** opção selecionada.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio2.png)

9. Selecione **OK**.

10. O **pedir** painel, colar no XML abaixo e definido **Employee_ID** para o ID de funcionário de um utilizador real no seu inquilino do Workday. Selecione um utilizador que tem o atributo preenchido que pretende extrair.

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="https://www.w3.org/2001/XMLSchema">
      <env:Body>
        <wd:Get_Workers_Request xmlns:wd="urn:com.workday/bsvc" wd:version="v21.1">
          <wd:Request_References wd:Skip_Non_Existing_Instances="true">
            <wd:Worker_Reference>
              <wd:ID wd:type="Employee_ID">21008</wd:ID>
            </wd:Worker_Reference>
          </wd:Request_References>
          <wd:Response_Group>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Personal_Information>true</wd:Include_Personal_Information>
            <wd:Include_Employment_Information>true</wd:Include_Employment_Information>
            <wd:Include_Management_Chain_Data>true</wd:Include_Management_Chain_Data>
            <wd:Include_Organizations>true</wd:Include_Organizations>
            <wd:Include_Reference>true</wd:Include_Reference>
            <wd:Include_Transaction_Log_Data>true</wd:Include_Transaction_Log_Data>
            <wd:Include_Photo>true</wd:Include_Photo>
            <wd:Include_User_Account>true</wd:Include_User_Account>
            <wd:Include_Roles>true</wd:Include_Roles>
          </wd:Response_Group>
        </wd:Get_Workers_Request>
      </env:Body>
    </env:Envelope>
    ```
 
11. Clique nas **enviar pedido** (seta verde) a executar o comando. Se tiver êxito, a resposta deverá aparecer na **resposta** painel. Verifique a resposta para garantir que tem do ID de utilizador que introduziu os dados e não um erro.

12. Se tiver êxito, copie o XML dos **resposta** painel e guarde-o como um arquivo XML.

13. No comando barra do Workday Studio, selecione **ficheiro > abrir o ficheiro...**  e abra o ficheiro XML que guardou. Isso abre no editor de XML do Workday Studio.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio3.png)

14. Na árvore de arquivos, percorra **/env: Envelope > env: corpo > wd:Get_Workers_Response > wd:Response_Data > wd: trabalho** para localizar os dados dos seus utilizadores. 

15. Sob **wd: trabalho**, encontrar o atributo que pretende adicionar e selecione-o.

16. Copie a expressão de XPath para seu atributo selecionado do **caminho do documento** campo.

1. Remover os **/env:Envelope / env:Body / wd:Get_Workers_Response / wd:Response_Data /** prefixo da expressão copiado.

18. Se o último item da expressão de copiado é um nó (exemplo: "/ wd: Birth_Date"), em seguida, anexe **/text()** no final da expressão. Isso não é necessário se o último item é um atributo (exemplo: "/@wd: tipo").

19. O resultado deve ser algo como `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`. Este é o que serão copiados no portal do Azure.


**Para adicionar o atributo de utilizador do Workday personalizado à sua configuração de aprovisionamento:**

1. Iniciar o [portal do Azure](https://portal.azure.com)e navegue para a secção de aprovisionamento do seu aplicativo, o aprovisionamento de dia de trabalho, conforme descrito anteriormente neste tutorial.

2. Definir **estado de aprovisionamento** ao **desativar**e selecione **guardar**. Isto ajudará a garantir que as alterações entrarão em vigor apenas quando estiver pronto.

3. Sob **mapeamentos**, selecione **os operadores de sincronizar a OnPremises** (ou **sincronizar funções de trabalho para o Azure AD**).

4. Desloque-se para a parte inferior do ecrã seguinte e selecione **Mostrar opções avançadas**.

5. Selecione **lista de atributos de edição para Workday**.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio_aad1.png)

6. Desloque-se para a parte inferior da lista de atributos, onde estão os campos de entrada.

7. Para **nome**, introduza um nome de exibição para seu atributo.

8. Para **tipo**, selecione o tipo de forma adequada ao seu atributo (**cadeia de caracteres** é mais comum).

9. Para **expressão de API**, introduza a expressão XPath que copiou a partir do Workday Studio. Exemplo: `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`

10. Selecione **adicionar atributo**.

    ![Workday Studio](./media/workday-inbound-tutorial/wdstudio_aad2.png)

11. Selecione **salvar** acima e, em seguida **Sim** na caixa de diálogo. Feche o ecrã de mapeamento do atributo, caso esteja ainda aberto.

12. Novamente no principal **aprovisionamento** separador, selecione **os operadores de sincronizar a OnPremises** (ou **sincronizar funções de trabalho para o Azure AD**) novamente.

13. Selecione **adicionar novo mapeamento**.

14. O novo atributo deverá agora aparecer na **atributo de origem** lista.

15. Adicione um mapeamento para o novo atributo conforme pretendido.

16. Quando terminar, não se esqueça de definir **estado de aprovisionamento** voltar ao **no** e guardar.

## <a name="known-issues"></a>Problemas conhecidos

* Escrita de dados para o atributo thumbnailPhoto de utilizador no Active Directory no local não é atualmente suportada.

* O conector "Workday para o Azure AD" não é atualmente suportado em inquilinos do Azure AD em que o AAD Connect está ativado.  

## <a name="managing-personal-data"></a>Gerir dados pessoais

O Workday, solução de provisionamento do Active Directory requer um agente de sincronização ser instalado num servidor associado a um domínio, e este agente cria registos no log de eventos do Windows que pode conter informações de identificação pessoal.

## <a name="next-steps"></a>Passos Seguintes

* [Saiba como rever os registos e obter relatórios de atividade de aprovisionamento](../manage-apps/check-status-user-account-provisioning.md)
* [Saiba como configurar o início de sessão único entre o dia de trabalho e o Azure Active Directory](workday-tutorial.md)
* [Saiba como integrar as outras aplicações SaaS com o Azure Active Directory](tutorial-list.md)


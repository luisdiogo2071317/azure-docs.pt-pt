---
title: 'Tutorial: Configurar Workday para o aprovisionamento de utilizador automáticas com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador ao Workday automaticamente.
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/18/2018
ms.author: asmalser
ms.openlocfilehash: 8e1fb97d23c294d02341ac89120bbafefa4b0f77
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35946012"
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning"></a>Tutorial: Configurar Workday para o aprovisionamento de utilizador automáticas

O objetivo deste tutorial é mostrar-lhe os passos que necessários para efetuar para importar as pessoas do Workday para o Active Directory e o Azure Active Directory, com repetição de escrita opcional de alguns atributos ao Workday.

## <a name="overview"></a>Descrição geral

O [utilizador do Azure Active Directory, serviço de fornecimento](../active-directory-saas-app-provisioning.md) integra-se com o [Workday recursos humanos API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) para aprovisionar contas de utilizador. Azure AD utiliza esta ligação para ativar o aprovisionamento de fluxos de trabalho de utilizador seguinte:

* **Aprovisionamento de utilizadores do Active Directory** -sincronizar selecionados conjuntos de utilizadores do Workday para um ou mais florestas do Active Directory.

* **Aprovisionamento apenas na nuvem que os utilizadores do Azure Active Directory** -híbrida utilizadores existem no Active Directory e do Azure Active Directory podem ser aprovisionados na última utilizando [AAD Connect](../connect/active-directory-aadconnect.md). No entanto, os utilizadores que são apenas na nuvem podem ser aprovisionados diretamente a partir do Workday para o Azure Active Directory utilizando o serviço de fornecimento de utilizador do Azure AD.

* **Repetição de escrita de correio eletrónico endereços Workday** -o serviço de fornecimento de utilizador do Azure AD é escrita selecionar atributos de utilizador do Azure AD para Workday, tais como o endereço de correio eletrónico.

### <a name="what-human-resources-scenarios-does-it-cover"></a>Os cenários de recursos humanos-abrange?

Os Workday utilizador Aprovisiona fluxos de trabalho suportados pelo serviço de aprovisionamento de utilizador do Azure AD ativar a automatização dos seguintes recursos humanos e cenários de gestão do ciclo de vida de identidade:

* **Contratação novos funcionários** - quando um empregado a nova é adicionado ao Workday, uma conta de utilizador é criada automaticamente no Active Directory, Azure Active Directory e, opcionalmente, Office 365 e [outras aplicações de SaaS suportadas pelo Azure AD](../active-directory-saas-app-provisioning.md), com repetição de escrita do endereço de correio eletrónico ao Workday.

* **Atualizações de atributo e o perfil de empregado** - quando um registo de empregado é atualizado no Workday (tais como o respetivo nome, o título ou o gestor), a conta de utilizador será automaticamente atualizada no Active Directory, Azure Active Directory e, opcionalmente, Office 365 e [outras aplicações de SaaS suportadas pelo Azure AD](../active-directory-saas-app-provisioning.md).

* **Terminations empregado** - quando um empregado é terminado no Workday, a conta de utilizador é desativada automaticamente no Active Directory, Azure Active Directory e, opcionalmente, Office 365 e [outras aplicações de SaaS suportadas pelo Azure AD](../active-directory-saas-app-provisioning.md).

* **Empregado novamente hires** - quando um empregado é rehired no Workday, a conta antiga pode ser reativada ou novamente aprovisionada (dependendo da sua preferência) do Active Directory, Azure Active Directory e, opcionalmente, para o Office 365 e automaticamente[outras aplicações de SaaS suportadas pelo Azure AD](../active-directory-saas-app-provisioning.md).

### <a name="who-is-this-user-provisioning-solution-best-suited-for"></a>Quem é que esta solução de aprovisionamento de utilizador melhor adequados?

Este aprovisionamento solução de utilizadores do Workday estão atualmente em pré-visualização pública e o ideal é adequada para:

* Organizações que uma solução baseada na nuvem, previamente concebida para aprovisionamento de utilizadores do Workday pretendidos ao nível

* Organizações que requerem o aprovisionamento de utilizadores direta do Workday do Active Directory ou do Azure Active Directory

* As organizações que exigem que os utilizadores a ser aprovisionado utilizando os dados obtidos a partir do módulo do Workday HCM (consulte [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* As organizações que necessitam de se associar, mover, e deixando os utilizadores sincronizados com uma ou mais florestas do Active Directory, domínios e UOs com base apenas no alterar informações detetadas no módulo Workday HCM (consulte [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html))

* Organizações com o Office 365 para correio eletrónico

## <a name="planning-your-solution"></a>Planeamento da solução

Antes de iniciar a integração do Workday, verifique os pré-requisitos abaixo e ler as seguintes orientações sobre como correspondem à sua arquitetura do Active Directory e requisitos de aprovisionamento com solution(s) fornecidos pelo Azure Active Directory do utilizador atual.

### <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem os seguintes itens:

* Uma subscrição do Azure AD Premium P1 válida com acesso de administrador global
* Um inquilino de implementação do Workday para fins de teste e a integração
* Permissões de administrador no Workday para criar um utilizador de integração do sistema e fazer alterações para testar os dados de empregado para fins de teste
* Para o aprovisionamento de utilizador no Active Directory, é necessário para alojar um servidor associado a um domínio com o serviço do Windows 2012 ou superior a [agente de sincronização no local](https://go.microsoft.com/fwlink/?linkid=847801)
* [O Azure AD Connect](../connect/active-directory-aadconnect.md) para sincronizar entre o Active Directory e o Azure AD

### <a name="solution-architecture"></a>Arquitetura de solução

O Azure AD fornece um conjunto avançado de conectores para o ajudar a resolver o aprovisionamento e a identidade do ciclo de vida de gestão do Workday para o Active Directory, Azure AD, aplicações de SaaS e para além de aprovisionamento. Quais as funcionalidades que irão utilizar e como configurar a solução irá variar dependendo do ambiente e requisitos da sua organização. Como primeiro passo, execute o gráfico de cotações de quantos dos seguintes procedimentos estão presentes e implementado na sua organização:

* Quantas florestas do Active Directory estão em utilização?
* Quantos domínios do Active Directory estão em utilização?
* Como muitas unidades de organizacional de diretório (UOs) de Active Directory estão em utilização?
* Quantos inquilinos do Azure Active Directory estão em utilização?
* Existem utilizadores que necessitam de ser aprovisionada para Active Directory e o Azure Active Directory (por exemplo utilizadores "híbrido")?
* Existem utilizadores que necessitam de ser aprovisionada para o Azure Active Directory, mas não do Active Directory (por exemplo "apenas na cloud" utilizadores)?
* Endereços de e-mail do utilizador precisa de repetição de escrita ao Workday?

Assim que tiver as respostas a estas perguntas, pode planear o Workday aprovisionamento implementação ao seguir as instruções abaixo.

#### <a name="using-provisioning-connector-apps"></a>Utilizar o aprovisionamento de conector de aplicações

Azure Active Directory suporta conectores aprovisionamento previamente integrados para Workday e um grande número de outras aplicações SaaS.

Um único conector aprovisionamento das interfaces com a API de um sistema de origem única e ajuda a dados de aprovisionamento para um sistema de destino única. A maioria dos conectores de aprovisionamento que suporte do Azure AD são para um único sistema de origem e destino (por exemplo, Azure AD para ServiceNow) e podem ser configurados ao adicionar a aplicação em questão da Galeria de aplicações do Azure AD (por exemplo, ServiceNow).

Há uma relação unívoca entre instâncias de conector aprovisionamento e instâncias da aplicação no Azure AD:

| Sistema de Origem | Sistema de destino |
| ---------- | ---------- |
| Inquilino do Azure AD | Aplicação SaaS |

No entanto, ao trabalhar com Workday e o Active Directory, existem vários sistemas de origem e de destino para ser considerado:

| Sistema de Origem | Sistema de destino | Notas |
| ---------- | ---------- | ---------- |
| Workday | Floresta do Active Directory | Cada floresta é tratada como um sistema de destino distintos |
| Workday | Inquilino do Azure AD | Conforme necessário para os utilizadores apenas na nuvem |
| Floresta do Active Directory | Inquilino do Azure AD | Este fluxo é processado pelo AAD Connect hoje |
| Inquilino do Azure AD | Workday | Para a repetição de escrita de endereços de e-mail |

Para facilitar a estes fluxos de trabalho vários para vários sistemas de origem e de destino, o Azure AD fornece várias aplicações de aprovisionamento conector que pode adicionar a partir da Galeria de aplicações do Azure AD:

![Galeria de aplicações do AAD](./media/workday-inbound-tutorial/WD_Gallery.PNG)

* **Workday para aprovisionamento do Active Directory** -conta aprovisionamento de utilizadores do Workday para uma única floresta do Active Directory facilita a esta aplicação. Se tiver várias florestas, pode adicionar uma instância desta aplicação na Galeria de aplicações do Azure AD para cada floresta do Active Directory, que terá de aprovisionar a.

* **Workday para aprovisionamento do Azure AD** - enquanto o AAD Connect é a ferramenta que deve ser utilizada para sincronizar o Active Directory, os utilizadores ao Azure Active Directory, esta aplicação podem ser utilizados para facilitar o aprovisionamento de utilizadores apenas na nuvem do Workday para um único Azure Inquilino do Active Directory.

* **Repetição de escrita do workday** -repetição de escrita de endereços de e-mail do utilizador do Azure Active Directory para Workday facilita a esta aplicação.

> [!TIP]
> A aplicação "Workday" regular é utilizada para configurar o início de sessão entre Workday e o Azure Active Directory. 

Como configurar estas aplicações de conector de aprovisionamento especiais é o assunto do restantes secções deste tutorial. Que aplicações optar por configurar irão depender são de inquilinos que sistemas terá de aprovisionar e quantas florestas do Active Directory e o Azure AD no seu ambiente.

![Descrição geral](./media/workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a>Configurar um utilizador de integração do sistema no Workday
Um requisito de todos os Workday aprovisionamento conectores comum é necessitam de credenciais para uma conta de integração do Workday sistema ligar à API de recursos humanos Workday. Esta secção descreve como criar uma conta de integrador de sistema no Workday.

> [!NOTE]
> É possível ignorar este procedimento em vez disso, uma conta de administrador global do Workday como conta de integração do sistema. Isto poderá funcionar ajustar para demonstrações, mas não é recomendado para implementações de produção.

### <a name="create-an-integration-system-user"></a>Criar um utilizador do sistema de integração

**Para criar um utilizador do sistema de integração:**

1. Inicie sessão no seu inquilino do Workday através de uma conta de administrador. No **Workday Workbench**, introduza criar utilizador na caixa de pesquisa e, em seguida, clique em **criar utilizador do sistema de integração**.

    ![Criar o utilizador](./media/workday-inbound-tutorial/IC750979.png "criar utilizador")
2. Concluir o **criar utilizador do sistema de integração** tarefa fornecendo um nome de utilizador e palavra-passe para um novo utilizador do sistema de integração.  
 * Deixe o **necessitam de nova palavra-passe no próximo início de sessão** opção desmarcada, porque este utilizador será possível iniciar sessão através de programação.
 * Deixe o **minutos de tempo limite da sessão** com o respetivo valor predefinido de 0, que impede as sessões do utilizador exceder o tempo limite prematuramente.

    ![Criar utilizador do sistema de integração](./media/workday-inbound-tutorial/IC750980.png "criar utilizador do sistema de integração")

### <a name="create-a-security-group"></a>Criar um grupo de segurança
Terá de criar um grupo de segurança do sistema de integração e atribua o utilizador a ele.

**Para criar um grupo de segurança:**

1. Introduza criar grupo de segurança na caixa de pesquisa e, em seguida, clique em **criar grupo de segurança**.

    ![Grupo de CreateSecurity](./media/workday-inbound-tutorial/IC750981.png "CreateSecurity grupo")
2. Concluir o **criar grupo de segurança** tarefas.  
3. Selecione **integração do grupo de segurança do sistema (Unconstrained)** do **tipo de Tenanted grupo de segurança** pendente.
4. Crie um grupo de segurança ao qual se serão adicionados explicitamente membros.

    ![Grupo de CreateSecurity](./media/workday-inbound-tutorial/IC750982.png "CreateSecurity grupo")

### <a name="assign-the-integration-system-user-to-the-security-group"></a>Atribua o utilizador de sistema de integração para o grupo de segurança

**Para atribuir o utilizador do sistema de integração:**

1. Introduza o grupo de segurança de editar na caixa de pesquisa e, em seguida, clique em **Editar grupo de segurança**.

    ![Grupo de segurança de editar](./media/workday-inbound-tutorial/IC750983.png "Editar grupo de segurança")
1. Procure e selecione o novo grupo de segurança de integração por nome.

    ![Grupo de segurança de editar](./media/workday-inbound-tutorial/IC750984.png "Editar grupo de segurança")
2. Adicione o novo utilizador do sistema de integração para o novo grupo de segurança. 

    ![Grupo de segurança do sistema](./media/workday-inbound-tutorial/IC750985.png "grupo de segurança do sistema")  

### <a name="configure-security-group-options"></a>Configurar opções do grupo de segurança
Neste passo, terá de conceder permissões de política para os dados de trabalho para o grupo de segurança de segurança de domínio.

**Para configurar opções do grupo de segurança:**

1. Introduza **políticas de segurança do domínio** na caixa de pesquisa e, em seguida, clique na ligação **políticas de segurança de domínio para a área funcional**.  

    ![As políticas de segurança do domínio](./media/workday-inbound-tutorial/IC750986.png "políticas de segurança do domínio")  
2. Pesquisa de sistema e selecione o **sistema** área funcional.  Clique em **OK**.  

    ![As políticas de segurança do domínio](./media/workday-inbound-tutorial/IC750987.png "políticas de segurança do domínio")  
3. Na lista de políticas de segurança para a área funcional do sistema, expanda **administração de segurança** e selecione a política de segurança do domínio **aprovisionamento da conta externa**.  

    ![As políticas de segurança do domínio](./media/workday-inbound-tutorial/IC750988.png "políticas de segurança do domínio")  
1. Clique em **editar permissões de**e, em seguida, no **editar permissões de** página da caixa de diálogo, adicione o grupo de segurança novas à lista de grupos de segurança com **obter** e **colocar**  permissões de integração.

    ![Editar permissão](./media/workday-inbound-tutorial/IC750989.png "editar permissões")  

1. Repita os passos 1 a 4 acima para cada uma destas políticas de segurança restantes:

| Operação | Política de segurança do domínio |
| ---------- | ---------- | 
| Obter e colocar | Dados do Worker: Relatórios de trabalho público |
| Obter e colocar | Dados do Worker: Informações de contacto de trabalho |
| Get | Dados do Worker: As posições de todos os |
| Get | Dados do Worker: Atual Staffing informações |
| Get | Dados do Worker: Título de negócio no perfil de trabalho |


### <a name="activate-security-policy-changes"></a>Ativar as alterações de política de segurança

**Para ativar as alterações de política de segurança:**

1. Introduza ativar na caixa de pesquisa e, em seguida, clique na ligação **ativar alterações de política de segurança pendentes**.

    ![Ativar](./media/workday-inbound-tutorial/IC750992.png "ativar") 
2. Iniciar a tarefa ativar alterações de política de segurança pendentes ao introduzir um comentário para fins de auditoria e, em seguida, clique em **OK**. 

    ![Ativar pendente segurança](./media/workday-inbound-tutorial/IC750993.png "ativar pendente segurança")  
1. Concluir a tarefa no ecrã seguinte marcando a caixa de verificação **confirmar**e, em seguida, clique em **OK**.

    ![Ativar pendente segurança](./media/workday-inbound-tutorial/IC750994.png "ativar pendente segurança")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Configurar o aprovisionamento de utilizadores do Workday para o Active Directory
Siga estas instruções para configurar o aprovisionamento do Workday para cada floresta do Active Directory que necessitam de aprovisionamento da conta de utilizador.

### <a name="planning"></a>Planeamento

Antes de configurar o aprovisionamento de utilizadores a uma floresta do Active Directory, considere as seguintes perguntas. As respostas a estas questões irão determinar como os filtros de âmbito e mapeamentos de atributos têm de ser definida. 

* **O que os utilizadores do Workday tem de ser aprovisionada para esta floresta do Active Directory?**

   * *Exemplo: Os utilizadores em que o atributo do Workday "Empresa" contém o valor "Contoso" e o atributo "Worker_Type" contém "Regular"*

* **Como são encaminhados os utilizadores em unidades diferentes organizacionais (UO)?**

   * *Exemplo: Os utilizadores são encaminhados para UOs que correspondem a uma localização do escritório, tal como definido no Workday "Municipality" e atributos "Country_Region_Reference"*

* **Como devem ser preenchidos os seguintes atributos do Active Directory?**

   * Nome comum (cn)
      * *Exemplo: Utilizar o valor do Workday User_ID, conforme definido pela recursos humanos*
      
   * ID de empregado (campo IDdeEmpregado)
      * *Exemplo: Utilizar o valor do Workday Worker_ID*
      
   * Nome da conta do SAM (sAMAccountName)
      * *Exemplo: Utilizar o valor do Workday User_ID, filtrado através de um Azure AD aprovisionamento expressão para remover os carateres ilegais*
      
   * Nome Principal de utilizador (userPrincipalName)
      * *Exemplo: Utilizar o valor do Workday User_ID, com um Azure AD aprovisionamento expressão acrescentar um nome de domínio*

* **Como devem ser correspondidos utilizadores entre Workday e do Active Directory?**

  * *Exemplo: Os utilizadores com um Workday específico "Worker_ID" valor são correspondido com utilizadores do Active Directory, onde "campo IDdeEmpregado" tem o mesmo valor. Se o valor de Worker_ID não foi encontrado no Active Directory, em seguida, crie um novo utilizador.*
  
* **A floresta do Active Directory já contém o utilizador IDs necessários para a lógica de correspondência de trabalho?**

  * *Exemplo: Se se tratar de uma nova implementação do Workday, é vivamente recomendado que o Active Directory ser pré-preenchidos com valores do Workday Worker_ID corretos (ou valor de ID exclusivo da preferência) para manter a lógica de correspondência tão simples quanto possível.*
    
    
### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Parte 1: Adicionar a aplicação de conector de aprovisionamento e criar a ligação ao Workday

**Para configurar Workday aprovisionamento do Active Directory:**

1.  Ir para <https://portal.azure.com>

2.  Na barra de navegação esquerdo, selecione **do Azure Active Directory**

3.  Selecione **aplicações empresariais**, em seguida, **todas as aplicações**.

4.  Selecione **adicionar uma aplicação**e selecione o **todos os** categoria.

5.  Procurar **Workday aprovisionamento para o Active Directory**e adicione essa aplicação a partir da galeria.

6.  Depois da aplicação é adicionada e do ecrã de detalhes da aplicação é apresentado, selecione **aprovisionamento**

7.  Alterar o **aprovisionamento** **modo** para **automática**

8.  Concluir o **credenciais de administrador** secção da seguinte forma:

   * **Nome de utilizador de Admin** – introduza o nome de utilizador da conta de sistema de integração do Workday, com o nome de domínio de inquilino anexado. **Deverá ter um aspeto semelhante ao seguinte: username@contoso4**

   * **Palavra-passe de administrador –** introduza a palavra-passe da conta de sistema de integração do Workday

   * **Inquilino URL –** introduza o URL ao ponto final dos serviços web do Workday para o seu inquilino. Isto deve ter o seguinte aspeto: https://wd3-impl-services1.workday.com/ccx/service/contoso4, onde contoso4 é substituído pelo nome do seu inquilino correto e wd3 impl é substituído com a cadeia de ambiente correto.

   * **Floresta do Active Directory -** o "nome" do Active Directory de floresta, tal como devolvido pelo commandlet do powershell Get-ADForest. Isto é, geralmente, uma cadeia como: *contoso.com*

   * **Contentor do Active Directory -** introduza a cadeia de contentor que contém todos os utilizadores na sua floresta do AD. Exemplo: *UO = utilizadores padrão, UO = utilizadores, DC = contoso, DC = test*

   * **E-Mail de notificação –** introduza o seu endereço de e-mail e marque a caixa de verificação "enviar correio eletrónico se ocorrer uma falha".

   * Clique em de **Testar ligação** botão. Se o teste de ligação for bem sucedida, clique em de **guardar** botão na parte superior. Se falhar, verifique se as credenciais do Workday estão válidas no Workday. 

![Portal do Azure](./media/workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a>Parte 2: Configurar os mapeamentos de atributos 

Nesta secção, irá configurar a forma como fluem de dados de utilizador do Workday para o Active Directory.

1.  No separador de aprovisionamento em **mapeamentos**, clique em **sincronizar trabalhadores do Workday para OnPremises**.

2.  No **âmbito de objeto de origem** campo, pode selecionar os conjuntos de utilizadores no Workday devem estar no âmbito de aprovisionamento para o AD, ao definir um conjunto de filtros baseadas em atributos. O âmbito de predefinido é "todos os utilizadores no Workday". Filtros de exemplo:

   * Exemplo: Âmbito para os utilizadores com IDs de trabalho entre 1000000 e 2000000

      * Atributo: WorkerID

      * Operador: Correspondência REGEX

      * Valor: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Exemplo: Apenas os funcionários e não os trabalhadores contingent 

      * Atributo: campo IDdeEmpregado

      * Um operador: Não é nula

3.  No **ações de objeto de destino** campo, pode filtrar global que ações podem ser executados no Active Directory. **Criar** e **atualização** são mais comuns.

4.  No **mapeamentos de atributos** secção, pode definir Workday individual como atributos mapeiam para atributos do Active Directory.

5. Clique num mapeamento de atributos existente para atualizá-lo ou clique em **adicionar novo mapeamento** na parte inferior do ecrã, para adicionar novos mapeamentos. Um mapeamento de atributos individuais suporta estas propriedades:

      * **Tipo de mapeamento**

         * **Direto** – escreve o valor do atributo Workday para o atributo de AD, sem alterações

         * **Constante** -escrever um valor de cadeia estática, constante para o atributo de AD

         * **Expressão** – permite-lhe escrever um valor personalizado para o atributo de AD, com base num ou mais atributos do Workday. [Para obter mais informações, consulte este artigo em expressões](../active-directory-saas-writing-expressions-for-attribute-mappings.md).

      * **Atributo de origem** -o atributo de utilizador do Workday. Se o atributo que procura não estiver presente, consulte [personalizar a lista de atributos de utilizador do Workday](#customizing-the-list-of-workday-user-attributes).

      * **Valor predefinido** – opcional. Se o atributo de origem tem um valor vazio, o mapeamento irá escrever este valor.
            A configuração mais comuns é a deixar isto em branco.

      * **Atributo de destino** – o atributo de utilizador no Active Directory.

      * **Corresponder objetos utilizando este atributo** – se ou não desta mapeamento deve ser utilizado para identificar exclusivamente os utilizadores entre Workday e do Active Directory. Normalmente, isto é definido no campo ID de trabalho para Workday, que normalmente é mapeado para um dos atributos ID de empregado no Active Directory.

      * **Correspondência de precedência** – vários atributos de correspondência pode ser definido. Quando existem vários, estes são avaliados por ordem definida por este campo. Assim que for encontrada uma correspondência, não são necessárias mais correspondência de atributos são avaliados.

      * **Aplicar este mapeamento**
       
         * **Sempre** – aplicar desta mapeamento em ambos os criação de utilizador e as ações de atualização

         * **Apenas durante a criação** -aplicar desta mapeamento apenas ações de criação de utilizador

6. Para guardar os mapeamentos, clique em **guardar** na parte superior da secção de mapeamento de atributos.

![Portal do Azure](./media/workday-inbound-tutorial/WD_2.PNG)

**Seguem-se alguns exemplos mapeamentos de atributos entre Workday e Active Directory, com algumas expressões comuns**

-   A expressão que mapeia para o atributo parentDistinguishedName é utilizada para aprovisionar um UOs diferentes com base num ou mais atributos de origem do Workday que os utilizadores. Neste exemplo aqui coloca os utilizadores no UOs diferentes com base em que cidade estão a ser.

-   O atributo userPrincipalName no Active Directory é gerado pelo concatenar o ID de utilizador do Workday com um sufixo de domínio

-   [Não há documentação sobre como escrever expressões aqui](../active-directory-saas-writing-expressions-for-attribute-mappings.md). Isto inclui exemplos sobre como remover os carateres especiais.

  
| ATRIBUTO DE WORKDAY | ATRIBUTOS DO ACTIVE DIRECTORY |  ID DE CORRESPONDENTE? | CRIAR / ATUALIZAR |
| ---------- | ---------- | ---------- | ---------- |
| **WorkerID**  |  Campo IDdeEmpregado | **Sim** | Escrito no apenas a criar |
| **UserID**    |  CN    |   |   Escrito no apenas a criar |
| **Associar ("@", [UserID], "contoso.com")**   | userPrincipalName     |     | Escrito no apenas a criar 
| **Replace(Mid(Replace(\[UserID\], , "(\[\\\\/\\\\\\\\\\\\\[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\\\&lt;\\\\&gt;\])", , "", , ), 1, 20), , "([\\\\.)\*\$](file:///\\.)*$)", , "", , )**      |    sAMAccountName            |     |         Escrito no apenas a criar |
| **Comutador (\[Active Directory\],, "0", "True", "1")** |  AccountDisabled      |     | Criar + atualizar |
| **FirstName**   | givenName       |     |    Criar + atualizar |
| **LastName**   |   sn   |     |  Criar + atualizar |
| **PreferredNameData**  |  displayName |     |   Criar + atualizar |
| **Empresa**         | Empresa   |     |  Criar + atualizar |
| **SupervisoryOrganization**  | departamento  |     |  Criar + atualizar |
| **ManagerReference**   | gestor  |     |  Criar + atualizar |
| **BusinessTitle**   |  título     |     |  Criar + atualizar | 
| **AddressLineData**    |  StreetAddress  |     |   Criar + atualizar |
| **Municipality**   |   l   |     | Criar + atualizar |
| **CountryReferenceTwoLetter**      |   Co |     |   Criar + atualizar |
| **CountryReferenceTwoLetter**    |  c  |     |         Criar + atualizar |
| **CountryRegionReference** |  St     |     | Criar + atualizar |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Criar + atualizar |
| **PostalCode**  |   postalCode  |     | Criar + atualizar |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Criar + atualizar |
| **Fax**      | facsimileTelephoneNumber     |     |    Criar + atualizar |
| **Mobile**  |    Mobile       |     |       Criar + atualizar |
| **LocalReference** |  preferredLanguage  |     |  Criar + atualizar |                                               
| **Comutador (\[Municipality\], "UO = utilizadores padrão, UO = utilizadores, UO = predefinido, UO = localizações, DC = contoso, DC = com", "Dallas", "UO = utilizadores padrão, UO = utilizadores, UO = Dallas, UO = localizações, DC = contoso, DC = com", "Austin", "UO = padrão UO dos utilizadores, = Utilizadores, UO = Austin, UO = localizações, DC = contoso, DC = com ","Coimbra"," UO = utilizadores padrão, UO = utilizadores, UO = Seattle, UO = localizações, DC = contoso, DC = com ","Londres"," UO = utilizadores padrão, UO = utilizadores, UO = Londres, UO = localizações, DC = contoso, DC = com ")**  | parentDistinguishedName     |     |  Criar + atualizar |
  
### <a name="part-3-configure-the-on-premises-synchronization-agent"></a>Parte 3: Configurar o agente de sincronização no local

Para aprovisionar ao Active Directory no local, um agente tem de ser instalado num servidor associado a um domínio na intenção de floresta do Active Directory. Administrador de domínio (ou administrador empresarial) são necessárias credenciais para concluir o procedimento.

**[Pode transferir o agente de sincronização no local](https://go.microsoft.com/fwlink/?linkid=847801)**

Depois de instalar o agente, execute os comandos do Powershell abaixo para configurar o agente para o seu ambiente.

**Comando #1**

> CD "C:\Program Files\Microsoft Azure AD Connect aprovisionamento Agent\Modules\AADSyncAgent" agente\\módulos\\AADSyncAgent

> Import-Module "C:\Program Files\Microsoft Azure AD Connect aprovisionamento Agent\Modules\AADSyncAgent\AADSyncAgent.psd1"

**Comando #2**

> Add-ADSyncAgentActiveDirectoryConfiguration

* Entrada: Para "Nome do diretório", introduza o nome de floresta do AD, tal como foi introduzido em parte \#2
* Entrada: Nome de utilizador de administrador e a palavra-passe para a floresta do Active Directory

**Comando #3**

> Add-ADSyncAgentAzureActiveDirectoryConfiguration

* Entrada: Nome de utilizador de Administrador Global e a palavra-passe para o seu inquilino do Azure AD

>[!IMPORTANT]
>Não há atualmente um problema conhecido com credenciais de administrador global não está a funcionar se utilizarem um domínio personalizado (exemplo: admin@contoso.com). Como solução, criar e utilizar uma conta de administrador global com um domínio onmicrosoft.com (exemplo: admin@contoso.onmicrosoft.com)

>[!IMPORTANT]
>Atualmente, não há um problema conhecido com credenciais de administrador global não está a funcionar se tiverem o multi-factor authentication ativada. Como solução, desative a autenticação multifator para o administrador global.


**Comando #4**

> Get-AdSyncAgentProvisioningTasks

* Ação: Confirme que são devolvidos dados. Este comando Deteta automaticamente Workday aprovisionamento de aplicações no seu inquilino do Azure AD. Exemplo de saída:

> Nome: Minha floresta do AD
>
> Ativado: True
>
> DirectoryName: mydomain.contoso.com
>
> Credentialed: False
>
> Identifier    : WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203

**Comando #5**

> Início AdSyncAgentSynchronization-automática

**Comando #6**

> net stop aadsyncagent

**Comando #7**

> net start aadsyncagent

>[!TIP]
>Para além de comandos "net" no Powershell, o serviço de agente de sincronização também pode ser iniciado e parado utilizando **Services.msc**. Se obtiver erros ao executar os comandos do Powershell, certifique-se de que o **Microsoft Azure AD aprovisionamento agente Connect** está em execução no **Services.msc**.

![Serviços](./media/workday-inbound-tutorial/Services.png)  

**Configuração adicional para os clientes na União Europeia**

Se o seu inquilino do Azure Active Directory estiver localizado dos centros de dados EU, em seguida, siga os passos adicionais abaixo.

1. Abra **Services.msc**e pare o **Microsoft Azure AD aprovisionamento agente Connect** serviço.
2. Aceda à pasta de instalação do agente (exemplo: C:\Program Files\Microsoft agente do Azure AD Connect aprovisionamento).
3. Abra **SyncAgnt.exe.config** num editor de texto.
4. Substitua https://manage.hub.syncfabric.windowsazure.com/Management com **https://eu.manage.hub.syncfabric.windowsazure.com/Management**
5. Substitua https://provision.hub.syncfabric.windowsazure.com/Provisioning com **https://eu.provision.hub.syncfabric.windowsazure.com/Provisioning**
6. Guardar o **SyncAgnt.exe.config** ficheiro.
7. Abra **Services.msc**e iniciar o **Microsoft Azure AD aprovisionamento agente Connect** serviço.

**Agente de resolução de problemas**

O [registo de eventos do Windows](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) no Windows Server que aloja o agente contém eventos para todas as operações executadas pelo agente. Para ver estes eventos:
    
1. Abra **Eventvwr.msc**.
2. Selecione **registos do Windows > aplicação**.
3. Ver todos os eventos registados na origem **AADSyncAgent**. 
4. Verifique a existência de erros e avisos.

Se existir um problema de permissões com credenciais do Active Directory ou do Azure Active Directory fornecidas nos comandos Powershell, verá um erro, tal como esta: 
    
![Registos de eventos](./media/workday-inbound-tutorial/Windows_Event_Logs.png) 


### <a name="part-4-start-the-service"></a>Parte 4: Inicie o serviço
Depois de partes de 1 a 3 foram concluídas, pode iniciar o serviço de aprovisionamento volta no portal do Azure.

1.  No **aprovisionamento** separador, defina o **estado de aprovisionamento** para **no**.

2. Clique em **Guardar**.

3. Isto iniciará a sincronização inicial, que pode demorar várias horas, consoante o número de utilizadores que estiverem no Workday variável.

4. Em qualquer altura, verifique o **registos de auditoria** separador no portal do Azure para ver que ações que efetuou o serviço de aprovisionamento. Os registos de auditoria apresenta uma lista de todos os eventos de sincronização individuais efetuados pelo serviço de aprovisionamento, por exemplo, o que os utilizadores estão a ser de leitura fora do Workday e, em seguida, subsequentemente, adicionados ou atualizados ao Active Directory. **[Consulte o guia de relatórios de aprovisionamento para obter instruções detalhadas sobre como ler os registos de auditoria](../active-directory-saas-provisioning-reporting.md)**

1.  Verifique o [registo de eventos do Windows](https://technet.microsoft.com/library/cc722404(v=ws.11).aspx) num computador Windows Server que aloja o agente para novos erros ou avisos. Estes eventos são visíveis, iniciando **Eventvwr.msc** no servidor e selecionando **registos do Windows > aplicação**. Todas as mensagens relacionadas com o aprovisionamento são registadas na origem **AADSyncAgent**.

6. Uma conclusão, irá escrever um relatório de resumo de auditoria **aprovisionamento** separador, conforme mostrado abaixo.

![Portal do Azure](./media/workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-to-azure-active-directory"></a>Configurar o aprovisionamento de utilizadores ao Azure Active Directory
Como configurar o aprovisionamento ao Azure Active Directory irá depender os requisitos de aprovisionamento, conforme especificado na tabela abaixo.

| Cenário | Solução |
| -------- | -------- |
| **Os utilizadores têm de ser aprovisionada para Active Directory e o Azure AD** | Utilize  **[AAD Connect](../connect/active-directory-aadconnect.md)** |
| **Os utilizadores têm de ser aprovisionados apenas ao Active Directory** | Utilize  **[AAD Connect](../connect/active-directory-aadconnect.md)** |
| **Os utilizadores têm de ser aprovisionados com o Azure AD apenas (apenas na nuvem)** | Utilize o **Workday aprovisionamento do Azure Active Directory** aplicação na Galeria de aplicações |

Para obter instruções sobre como configurar o Azure AD Connect, consulte o [documentação do Azure AD Connect](../connect/active-directory-aadconnect.md).

As secções seguintes descrevem como configurar uma ligação entre Workday e o Azure AD para aprovisionar utilizadores apenas na nuvem.

> [!IMPORTANT]
> Apenas siga o procedimento abaixo se tiver utilizadores apenas na nuvem que precisam de ser aprovisionada para o Azure AD e não no local do Active Directory.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Parte 1: Adicionar a aplicação do conector de aprovisionamento do Azure AD e criar a ligação ao Workday

**Para configurar Workday aprovisionamento para os utilizadores apenas na nuvem do Azure Active Directory:**

1.  Aceda a <https://portal.azure.com>.

2.  Na barra de navegação esquerdo, selecione **do Azure Active Directory**

3.  Selecione **aplicações empresariais**, em seguida, **todas as aplicações**.

4.  Selecione **adicionar uma aplicação**e, em seguida, selecione o **todos os** categoria.

5.  Procurar **Workday para aprovisionamento do Azure AD**e adicione essa aplicação a partir da galeria.

6.  Depois da aplicação é adicionada e do ecrã de detalhes da aplicação é apresentado, selecione **aprovisionamento**

7.  Alterar o **aprovisionamento** **modo** para **automática**

8.  Concluir o **credenciais de administrador** secção da seguinte forma:

   * **Nome de utilizador de Admin** – introduza o nome de utilizador da conta de sistema de integração do Workday, com o nome de domínio de inquilino anexado. Deverá ter um aspeto semelhante ao seguinte: username@contoso4

   * **Palavra-passe de administrador –** introduza a palavra-passe da conta de sistema de integração do Workday

   * **Inquilino URL –** introduza o URL ao ponto final dos serviços web do Workday para o seu inquilino. Isto deve ter o seguinte aspeto: https://wd3-impl-services1.workday.com/ccx/service/contoso4, onde contoso4 é substituído pelo nome do seu inquilino correto e wd3 impl é substituído com a cadeia de ambiente correto. Se este URL não é conhecido,. funciona com o seu representante de parceiro ou suporte de integração de Workday para determinar o URL correto a utilizar.

   * **E-Mail de notificação –** introduza o seu endereço de e-mail e marque a caixa de verificação "enviar correio eletrónico se ocorrer uma falha".

   * Clique em de **Testar ligação** botão.

   * Se o teste de ligação for bem sucedida, clique em de **guardar** botão na parte superior. Se falhar, verifique que o URL do Workday e as credenciais são válidas no Workday.


### <a name="part-2-configure-attribute-mappings"></a>Parte 2: Configurar os mapeamentos de atributos 

Nesta secção, irá configurar como os dados de utilizador fluem do Workday para o Azure Active Directory para os utilizadores apenas na nuvem.

1.  No separador de aprovisionamento em **mapeamentos**, clique em **trabalhadores sincronizar com o Azure AD**.

2.   No **âmbito de objeto de origem** campo, pode selecionar os conjuntos de utilizadores no Workday devem estar no âmbito de aprovisionamento para o Azure AD, ao definir um conjunto de filtros baseadas em atributos. O âmbito de predefinido é "todos os utilizadores no Workday". Filtros de exemplo:

   * Exemplo: Âmbito para os utilizadores com IDs de trabalho entre 1000000 e 2000000

      * Atributo: WorkerID

      * Operador: Correspondência REGEX

      * Valor: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Exemplo: Trabalhadores contingent apenas e não regulares empregados

      * Atributo: ContingentID

      * Um operador: Não é nula

3.  No **ações de objeto de destino** campo, pode filtrar global que ações podem ser executados no Azure AD. **Criar** e **atualização** são mais comuns.

4.  No **mapeamentos de atributos** secção, pode definir Workday individual como atributos mapeiam para atributos do Active Directory.

5. Clique num mapeamento de atributos existente para atualizá-lo ou clique em **adicionar novo mapeamento** na parte inferior do ecrã, para adicionar novos mapeamentos. Um mapeamento de atributos individuais suporta estas propriedades:

   * **Tipo de mapeamento**

      * **Direto** – escreve o valor do atributo Workday para o atributo de AD, sem alterações

      * **Constante** -escrever um valor de cadeia estática, constante para o atributo de AD

      * **Expressão** – permite-lhe escrever um valor personalizado para o atributo de AD, com base num ou mais atributos do Workday. [Para obter mais informações, consulte este artigo em expressões](../active-directory-saas-writing-expressions-for-attribute-mappings.md).

   * **Atributo de origem** -o atributo de utilizador do Workday. Se o atributo que procura não estiver presente, consulte [personalizar a lista de atributos de utilizador do Workday](#customizing-the-list-of-workday-user-attributes).

   * **Valor predefinido** – opcional. Se o atributo de origem tem um valor vazio, o mapeamento irá escrever este valor.
            A configuração mais comuns é a deixar isto em branco.

   * **Atributo de destino** – o atributo de utilizador no Azure AD.

   * **Corresponder objetos utilizando este atributo** – se ou não desta mapeamento deve ser utilizado para identificar exclusivamente os utilizadores entre Workday e o Azure AD. Normalmente, isto é definido no campo ID de trabalho para Workday, que normalmente é mapeado para o atributo de ID de empregado (novo) ou um atributo de extensão no Azure AD.

   * **Correspondência de precedência** – vários atributos de correspondência pode ser definido. Quando existem vários, estes são avaliados por ordem definida por este campo. Assim que for encontrada uma correspondência, não são necessárias mais correspondência de atributos são avaliados.

   * **Aplicar este mapeamento**

     * **Sempre** – aplicar desta mapeamento em ambos os criação de utilizador e as ações de atualização

     * **Apenas durante a criação** -aplicar desta mapeamento apenas ações de criação de utilizador

6. Para guardar os mapeamentos, clique em **guardar** na parte superior da secção de mapeamento de atributos.

### <a name="part-3-start-the-service"></a>Parte 3: Iniciar o serviço
Depois de partes 1-2 tem sido concluídas, pode iniciar o serviço de aprovisionamento.

1.  No **aprovisionamento** separador, defina o **estado de aprovisionamento** para **no**.

2. Clique em **Guardar**.

3. Isto iniciará a sincronização inicial, que pode demorar várias horas, consoante o número de utilizadores que estiverem no Workday variável.

4. Eventos de sincronização individuais podem ser visualizados no **registos de auditoria** separador. **[Consulte o guia de relatórios de aprovisionamento para obter instruções detalhadas sobre como ler os registos de auditoria](../active-directory-saas-provisioning-reporting.md)**

5. Uma conclusão, irá escrever um relatório de resumo de auditoria **aprovisionamento** separador, conforme mostrado abaixo.


## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Configurar a repetição de escrita de endereços de e-mail para Workday
Siga estas instruções para configurar a repetição de escrita de endereços de e-mail do utilizador do Azure Active Directory para Workday.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Parte 1: Adicionar a aplicação de conector de aprovisionamento e criar a ligação ao Workday

**Para configurar Workday aprovisionamento do Active Directory:**

1.  Ir para <https://portal.azure.com>

2.  Na barra de navegação esquerdo, selecione **do Azure Active Directory**

3.  Selecione **aplicações empresariais**, em seguida, **todas as aplicações**.

4.  Selecione **adicionar uma aplicação**, em seguida, selecione o **todos os** categoria.

5.  Procurar **repetição de escrita do Workday**e adicione essa aplicação a partir da galeria.

6.  Depois da aplicação é adicionada e do ecrã de detalhes da aplicação é apresentado, selecione **aprovisionamento**

7.  Alterar o **aprovisionamento** **modo** para **automática**

8.  Concluir o **credenciais de administrador** secção da seguinte forma:

   * **Nome de utilizador de Admin** – introduza o nome de utilizador da conta de sistema de integração do Workday, com o nome de domínio de inquilino anexado. Deverá ter um aspeto semelhante ao seguinte: username@contoso4

   * **Palavra-passe de administrador –** introduza a palavra-passe da conta de sistema de integração do Workday

   * **Inquilino URL –** introduza o URL ao ponto final dos serviços web do Workday para o seu inquilino. Isto deve ter o seguinte aspeto: https://wd3-impl-services1.workday.com/ccx/service/contoso4, onde contoso4 é substituído pelo nome do seu inquilino correto e wd3 impl é substituído com a cadeia de ambiente correto (se necessário).

   * **E-Mail de notificação –** introduza o seu endereço de e-mail e marque a caixa de verificação "enviar correio eletrónico se ocorrer uma falha".

   * Clique em de **Testar ligação** botão. Se o teste de ligação for bem sucedida, clique em de **guardar** botão na parte superior. Se falhar, verifique que o URL do Workday e as credenciais são válidas no Workday.


### <a name="part-2-configure-attribute-mappings"></a>Parte 2: Configurar os mapeamentos de atributos 


Nesta secção, irá configurar a forma como fluem de dados de utilizador do Workday para o Active Directory.

1.  No separador de aprovisionamento em **mapeamentos**, clique em **sincronizar do Azure AD que os utilizadores Workday**.

2.  No **âmbito de objeto de origem** campo, pode opcionalmente filtrar os conjuntos de utilizadores no Azure Active Directory devem ter os respetivos endereços de correio eletrónico gravados Workday. O âmbito de predefinido é "todos os utilizadores no Azure AD". 

3.  No **mapeamentos de atributos** secção, pode definir Workday individual como atributos mapeiam para atributos do Active Directory. Não há um mapeamento para o endereço de e-mail por predefinição. No entanto, o ID de correspondente tem de ser atualizado para corresponder aos utilizadores no Azure AD com as respetivas entradas correspondentes no Workday. É um método de correspondência popular sincronizar o ID de trabalho do Workday ou o ID de empregado para extensionAttribute1-15 no Azure AD e, em seguida, utilize este atributo no Azure AD para corresponder aos utilizadores no Workday.

4.  Para guardar os mapeamentos, clique em **guardar** na parte superior da secção de mapeamento de atributos.

### <a name="part-3-start-the-service"></a>Parte 3: Iniciar o serviço
Depois de partes 1-2 tem sido concluídas, pode iniciar o serviço de aprovisionamento.

1.  No **aprovisionamento** separador, defina o **estado de aprovisionamento** para **no**.

2. Clique em **Guardar**.

3. Isto iniciará a sincronização inicial, que pode demorar várias horas, consoante o número de utilizadores que estiverem no Workday variável.

4. Eventos de sincronização individuais podem ser visualizados no **registos de auditoria** separador. **[Consulte o guia de relatórios de aprovisionamento para obter instruções detalhadas sobre como ler os registos de auditoria](../active-directory-saas-provisioning-reporting.md)**

5. Uma conclusão, irá escrever um relatório de resumo de auditoria **aprovisionamento** separador, conforme mostrado abaixo.


## <a name="customizing-the-list-of-workday-user-attributes"></a>Personalizar a lista de atributos de utilizador do Workday
Aprovisionamento de aplicações para o Active Directory e o Azure AD incluem uma lista predefinida de atributos de utilizador do Workday evidenciam pode selecionar. No entanto, estas listas não são abrangentes. Workday suporta várias centenas de atributos de utilizador possível, o que podem ser exclusivo no seu inquilino do Workday ou padrão. 

O Azure AD que o serviço de fornecimento suporta a capacidade para personalizar a lista ou um atributo do Workday para incluir quaisquer atributos expostos no [Get_Workers](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) operação da API de recursos humanos.

Para tal, tem de utilizar [Workday Studio](https://community.workday.com/studio-download) para extrair as expressões XPath que representam os atributos que pretende utilizar e, em seguida, adicioná-los à sua configuração de aprovisionamento utilizando o editor de atributos avançadas no portal do Azure.

**Para obter uma expressão de XPath para um atributo de utilizador do Workday:**

1. Transfira e instale [Workday Studio](https://community.workday.com/studio-download). Precisa de uma conta de Comunidade do Workday para o instalador de acesso.

2. Transfira o ficheiro de Workday Human_Resources WDSL a partir deste URL: https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Human_Resources.wsdl

3. Inicie o Studio do Workday.

4. Na barra de comandos, selecione o **Workday > serviço Web do teste no Tester** opção.

5. Selecione **externo**e selecione o ficheiro de Human_Resources WSDL que transferiu no passo 2.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio1.PNG)

6. Definir o **localização** campo para `https://IMPL-CC.workday.com/ccx/service/TENANT/Human_Resources`, mas, substituindo "IMPL-CC" real com tipo de instância e "INQUILINO" com o nome do seu inquilino real.

7. Definir **operação** para **Get_Workers**

8.  Clique nos pequenos **configurar** ligação abaixo os painéis de pedido/resposta para definir as credenciais do Workday. Verifique **autenticação**e, em seguida, introduza o nome de utilizador e palavra-passe para a sua conta de sistema de integração do Workday. Certifique-se formatar o nome de utilizador como name@tenante deixe o **UsernameToken de WS-segurança** opção selecionada.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio2.PNG)

9. Selecione **OK**.

10. O **pedido** painel, colar no XML abaixo e definido **Employee_ID** no ID de empregado de um utilizador real no seu inquilino do Workday. Selecione um utilizador que tem o atributo preenchido que pretende extrair.

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
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
 
11. Clique em de **enviar pedido** (seta verde) ao executar o comando. Se tiver êxito, a resposta deve aparecer no **resposta** painel. Verifique a resposta para se certificar de que tem do ID de utilizador que introduziu os dados e não um erro.

12. Se tiver êxito, copie o XML do **resposta** painel e guardá-lo como um ficheiro XML.

13. No comando barra do Workday Studio, selecione **ficheiro > abrir ficheiros...**  e abra o ficheiro XML que guardou. Esta ação abre-lo no editor de XML do Workday Studio.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio3.PNG)

14. Na árvore de ficheiros, navegue através de **/env: Envelope > env: Body > wd:Get_Workers_Response > wd:Response_Data > wd: trabalho** para localizar dados do utilizador. 

15. Em **wd: trabalho**, localizar o atributo que pretende adicionar e selecione-o.

16. Copie a expressão de XPath para o atributo selecionado fora do **caminho do documento** campo.

1. Remova o **/env:Envelope / env: body / wd:Get_Workers_Response / wd:Response_Data /** prefixo da expressão copiado.

18. Se o último item na expressão copiado é um nó (exemplo: "/ wd: Birth_Date"), em seguida, anexe **/text()** no fim da expressão. Isto não é necessário se o último item é um atributo (exemplo: "/@wd: tipo").

19. O resultado deve ser algo semelhante ao seguinte `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`. Este é o que serão copiados para o portal do Azure.


**Para adicionar o atributo de utilizador do Workday personalizado à sua configuração de aprovisionamento:**

1. Inicie o [portal do Azure](https://portal.azure.com)e navegue para a secção de aprovisionamento do seu Workday aprovisionamento de aplicações, como descrito anteriormente neste tutorial.

2. Definir **estado de aprovisionamento** para **desativar**e selecione **guardar**. Isto irá ajudar a garantir que as alterações terão efeito apenas quando estiver preparado.

3. Em **mapeamentos**, selecione **sincronizar funcionários OnPremises** (ou **trabalhadores sincronizar com o Azure AD**).

4. Desloque-se na parte inferior do ecrã seguinte e selecione **Mostrar opções avançadas**.

5. Selecione **Editar lista de atributos para Workday**.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio_AAD1.PNG)

6. Desloque-se a parte inferior da lista de atributos onde estão os campos de entrada.

7. Para **nome**, introduza um nome a apresentar para o atributo.

8. Para **tipo**, selecione o tipo que adequadamente corresponda ao seu atributo (**cadeia** é mais comum).

9. Para **API expressão**, introduza a expressão XPath que copiou do Workday Studio. Exemplo: `wd:Worker/wd:Worker_Data/wd:Personal_Data/wd:Birth_Date/text()`

10. Selecione **adicionar atributo**.

    ![Workday Studio](./media/workday-inbound-tutorial/WDstudio_AAD2.PNG)

11. Selecione **guardar** acima e, em seguida, **Sim** à caixa de diálogo. Se estiver aberto, feche o ecrã de mapeamento de atributos.

12. Novamente no principal **aprovisionamento** separador, selecione **sincronizar funcionários OnPremises** (ou **trabalhadores sincronizar com o Azure AD**) novamente.

13. Selecione **adicionar novo mapeamento**.

14. O novo atributo deverá agora aparecer no **atributo de origem** lista.

15. Adicione um mapeamento para o novo atributo conforme pretendido.

16. Quando terminar, lembre-se definir **estado de aprovisionamento** para **no** e guardar.

## <a name="known-issues"></a>Problemas conhecidos

* Ao executar o **adicionar ADSyncAgentAzureActiveDirectoryConfiguration** comando do Powershell, não há atualmente um problema conhecido com credenciais de administrador global não está a funcionar se utilizarem um domínio personalizado (exemplo: admin@contoso.com) . Como solução, criar e utilizar uma conta de administrador global no Azure AD com um domínio onmicrosoft.com (exemplo: admin@contoso.onmicrosoft.com).

* Um problema com os registos de auditoria não são apresentados no inquilinos do Azure AD, localizados na União Europeia anterior foi resolvido. No entanto, a configuração do agente adicionais é necessária para inquilinos do Azure AD no EU. Para obter mais informações, consulte [parte 3: configurar o agente de sincronização no local](#Part 3: Configure the on-premises synchronization agent)


## <a name="managing-personal-data"></a>Gestão de dados pessoais

Workday solução de aprovisionamento do Active Directory requer um agente de sincronização ser instalada num servidor associado a um domínio, e este agente cria registos no registo de eventos do Windows que pode conter informações de identificação pessoal.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]  onde a... / referências correspondente a estrutura de hierarquia do seu artigo

> [!NOTE]
> Se estiver interessado em ver ou eliminar os dados pessoais, reveja a documentação de orientação da Microsoft no [dados do Windows do requerente pedidos para o GDPR](https://review.docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows) site. Se estiver a procurar informações gerais sobre GDPR, consulte o [secção GDPR do Portal do serviço de confiança](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="next-steps"></a>Passos Seguintes

* [Saiba como rever os registos e obter relatórios sobre o aprovisionamento de atividade](../active-directory-saas-provisioning-reporting.md)
* [Saiba como configurar o início de sessão entre Workday e o Azure Active Directory](workday-tutorial.md)
* [Saiba como integrar a outras aplicações de SaaS no Azure Active Directory](tutorial-list.md)

---
title: 'Tutorial: Integração do Azure Active Directory com nuvem de objeto do SAP Business | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e na Cloud de objeto do SAP Business.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: ffd4480a13549caba17becff27a43f51fcaa1988
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041743"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Tutorial: Integração do Azure Active Directory com SAP Business objeto Cloud

Neste tutorial, saiba como integrar a Cloud do SAP Business objeto com o Azure Active Directory (Azure AD).

Obtém as seguintes vantagens quando integrar a Cloud de objeto de negócios SAP no Azure AD:

- No Azure AD, pode controlar quem tem acesso à nuvem de objeto do SAP Business.
- Pode assinar automaticamente a que os seus utilizadores para a Cloud do SAP Business objeto com o início de sessão único e a conta do Azure AD de um utilizador.
- Pode gerir as suas contas num, local central, o portal do Azure.

Para saber mais sobre o software como uma integração de aplicação de serviço (SaaS) com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o SAP Cloud de objeto de negócios, precisa do seguinte:

- Uma subscrição do Azure AD
- Uma Cloud de objeto de negócios SAP, com início de sessão único ativada

> [!NOTE]
> Se testar os passos neste tutorial, recomendamos que não testá-los num ambiente de produção.

Recomendações para testar os passos neste tutorial:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. 

O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicione SAP Cloud de objeto comercial a partir da galeria.
2. Configure e teste do Azure AD início de sessão único.

## <a name="add-sap-business-object-cloud-from-the-gallery"></a>Adicionar SAP Cloud de objeto comercial a partir da Galeria
Para configurar a integração da Cloud de objeto de negócios SAP no Azure AD, na galeria, adicione na Cloud de objeto do SAP Business à sua lista de aplicações de SaaS geridas.

Para adicionar SAP Cloud de objeto comercial a partir da galeria:

1. Na [portal do Azure](https://portal.azure.com), no menu da esquerda, selecione **Azure Active Directory**. 

    ![O botão do Azure Active Directory][1]

2. Selecione **aplicações empresariais**e, em seguida, selecione **todos os aplicativos**.

    ![A página de aplicações do Enterprise][2]
    
3. Para adicionar uma nova aplicação, selecione **nova aplicação**.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, introduza **SAP Business objeto Cloud**.

    ![Caixa de pesquisa](./media/sapboc-tutorial/tutorial_sapboc_search.png)

5. No painel de resultados, selecione **na Cloud de objeto do SAP Business**e, em seguida, selecione **Add**.

    ![SAP Business objeto na Cloud, na lista de resultados](./media/sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a>Configurar e teste do Azure AD início de sessão único

Nesta secção, vai configurar e teste do Azure AD início de sessão único com o SAP Cloud de objeto de negócios com base num utilizador de teste com o nome *Eduarda Almeida*.

Para início de sessão único funcione, o Azure AD precisa saber o utilizador de equivalente do Azure AD na Cloud de objeto do SAP Business. É necessário estabelecer uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Cloud de objeto do SAP Business.

Para estabelecer a relação de hiperligação, na Cloud de objeto de negócios SAP, para **nome de utilizador**, atribua o valor da **nome de utilizador** no Azure AD.

Para configurar e testar o Azure AD início de sessão único com o SAP Cloud de objeto de negócios, conclua as seguintes tarefas:

1. [Configurar o Azure AD início de sessão único](#set-up-azure-ad-single-sign-on). Configura um utilizador para utilizar esta funcionalidade.
2. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user). Testes do Azure AD início de sessão único com o utilizador Eduarda Almeida.
3. [Criar um utilizador de teste na Cloud de objeto do SAP Business](#create-an-sap-business-object-cloud-test-user). Cria um equivalente da Eduarda Almeida na Cloud de objeto de negócios SAP que está ligado a representação do Azure AD do utilizador.
4. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user). Configura a Eduarda Almeida para utilizar o Azure AD início de sessão único.
5. [Testar início de sessão único](#test-single-sign-on). Verifica-se de que a configuração funciona.

### <a name="set-up-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta seção, ativar do Azure AD único início de sessão no portal do Azure. Em seguida, configurar o início de sessão único na sua aplicação na Cloud de objeto do SAP Business.

Para configurar o Azure AD início de sessão único com o SAP Business objeto Cloud:

1. No portal do Azure, sobre o **na Cloud de objeto do SAP Business** página de integração de aplicações, selecione **início de sessão único**.

    ![Selecione início de sessão único][4]

2. Sobre o **início de sessão único** página, para **modo**, selecione **baseado em SAML logon**.
 
    ![Selecione baseado em SAML início de sessão](./media/sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. Sob **SAP Business objeto Cloud domínio e URLs**, conclua os seguintes passos:

    1. Na **URL de início de sessão** , introduza um URL com o seguinte padrão: 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. Na **identificador** , introduza um URL com o seguinte padrão:
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![URLs de domínio na Cloud do SAP Business objeto e URLs de página](./media/sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > Os valores estes URLs são para apenas demonstração. Atualize os valores com o URL de início de sessão real e o URL de identificador. Para obter o URL de início de sessão, entre em contato com o [equipa de suporte de cliente de Cloud do SAP Business objeto](https://help.sap.com/viewer/product/SAP_BusinessObjects_Cloud/release/en-US). Pode obter o URL de identificador ao transferir os metadados de nuvem de objeto de negócios SAP a partir da consola de administrador. Isso é explicado mais tarde no tutorial. 

4. Sob **certificado de assinatura SAML**, selecione **XML de metadados**. Em seguida, guarde o ficheiro de metadados no seu computador.

    ![Selecione o XML de metadados](./media/sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. Selecione **Guardar**.

    ![Selecionar guardar](./media/sapboc-tutorial/tutorial_general_400.png)

6. Numa janela do browser web diferente, inicie sessão no site da sua empresa na Cloud do SAP Business objeto como um administrador.

7. Selecione **Menu** > **sistema** > **administração**.
    
    ![Selecione o Menu, em seguida, sistema e, em seguida, administração](./media/sapboc-tutorial/config1.png)

8. Sobre o **Security** separador, selecione a **editar** ícone (caneta).
    
    ![Na guia segurança, selecione o ícone de edição](./media/sapboc-tutorial/config2.png)  

9. Para **método de autenticação**, selecione **SAML único início de sessão (SSO)**.

    ![Selecione SAML início de sessão único para o método de autenticação](./media/sapboc-tutorial/config3.png)  

10. Para transferir os metadados do fornecedor de serviço (passo 1), selecione **transferir**. No ficheiro de metadados, localizar e copiar o **entityID** valor. No portal do Azure, em **SAP Business objeto Cloud domínio e URLs**, cole o valor no **identificador** caixa.

    ![Copie e cole o valor de entityID](./media/sapboc-tutorial/config4.png)  

11. Para carregar os metadados do fornecedor de serviço (etapa 2) no ficheiro que transferiu a partir do portal do Azure, em **metadados de carregar o fornecedor de identidade**, selecione **carregar**.  

    ![Em metadados de carregar o fornecedor de identidade, selecionar o carregamento](./media/sapboc-tutorial/config5.png)

12. Na **atributo de utilizador** , selecione o atributo de utilizador (etapa 3) que pretende utilizar para a sua implementação. Este atributo de utilizador é mapeado para o fornecedor de identidade. Para introduzir um atributo personalizado na página do utilizador, utilize o **mapeamento de SAML personalizada** opção. Em alternativa, pode selecionar **E-Mail** ou **ID de utilizador** como o atributo de utilizador. No nosso exemplo, selecionamos **E-Mail** porque estamos mapeado a afirmação de identificador de utilizador com o **userprincipalname** atributo o **atributos de utilizador** secção no Azure Portal. Esta opção fornece um e-mail de utilizador exclusivo, que é enviado para a aplicação na Cloud de objeto do SAP Business em cada resposta SAML com êxito.

    ![Selecione o atributo de utilizador](./media/sapboc-tutorial/config6.png)

13. Para verificar a conta com o fornecedor de identidade (etapa 4), na **credenciais de início de sessão (E-Mail)** , introduza o endereço de e-mail do utilizador. Em seguida, selecione **Verifique se conta**. O sistema adiciona as credenciais de início de sessão para a conta de utilizador.

    ![Introduza o e-mail e selecionar verificar conta](./media/sapboc-tutorial/config7.png)

14. Selecione o **guardar** ícone.

    ![Ícone de guardar](./media/sapboc-tutorial/save.png)

> [!TIP]
> Pode ler uma versão concisa destas instruções no [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação! Depois de adicionar a aplicação, selecionando **do Active Directory** > **aplicações empresariais**, selecione o **Single Sign-On** separador. Pode acessar a documentação do embedded na **configuração** secção, na parte inferior da página. Para obter mais informações, consulte [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
Nesta secção, vai criar um utilizador de teste com o nome Eduarda Almeida no portal do Azure.

Para criar um utilizador de teste no Azure AD:

1. No portal do Azure, no menu à esquerda, selecione **do Azure Active Directory**.

    ![Criar um utilizador de teste do Azure AD](./media/sapboc-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, selecione **utilizadores e grupos**e, em seguida, selecione **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/sapboc-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, selecione **Add**.
 
    ![Criar um utilizador de teste do Azure AD](./media/sapboc-tutorial/create_aaduser_03.png) 

4. Na **utilizador** diálogo caixa, conclua os seguintes passos:
 
    1. Na **Name** , introduza **BrittaSimon**.

    2. Na **nome de utilizador** , introduza o endereço de e-mail do utilizador Eduarda Almeida.

    3. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    4. Selecione **Criar**.

        ![A caixa de diálogo de utilizador](./media/sapboc-tutorial/create_aaduser_04.png) 

    ![Criar utilizador do Azure AD][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a>Criar um utilizador de teste na Cloud de objeto do SAP Business

Utilizadores do Azure AD tem de ser aprovisionados na Cloud do SAP Business objeto antes de poderem iniciar sessão na cloud de objeto do SAP Business. Na Cloud de objeto de negócios SAP, o aprovisionamento é uma tarefa manual.

Para Aprovisionar uma conta de utilizador:

1. Inicie sessão no site da sua empresa na Cloud do SAP Business objeto como um administrador.

2. Selecione **Menu** > **segurança** > **utilizadores**.

    ![Adicionar o funcionário](./media/sapboc-tutorial/user1.png)

3. Sobre o **usuários** página, para adicionar novos detalhes de utilizador, selecione **+**. 

    ![Adicionar página usuários](./media/sapboc-tutorial/user4.png)

    Em seguida, conclua os seguintes passos:

    1. Na **ID de utilizador** , introduza o ID de utilizador do utilizador, como **Eduarda**.

    2. Na **nome próprio** , introduza o nome do utilizador, como **Eduarda**.

    3. Na **SOBRENOME** , introduza o apelido do utilizador, como **Simon**.

    4. Na **nome a apresentar** , introduza o nome completo do utilizador, como **Eduarda Almeida**.

    5. Na **email** , introduza o endereço de e-mail do utilizador, como **brittasimon@contoso.com**.

    6. Sobre o **selecionar funções** página, selecione a função adequada para o utilizador e, em seguida, selecione **OK**.

      ![Selecionar função](./media/sapboc-tutorial/user3.png)

    7. Selecione o **guardar** ícone.    


### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta seção, permite que o usuário Eduarda Almeida para utilizar o Azure AD início de sessão único ao conceder o acesso de conta de utilizador para a Cloud de objeto do SAP Business.

Para atribuir a Eduarda Almeida a Cloud de objeto do SAP Business:

1. No portal do Azure, abra a vista de aplicativos e, em seguida, vá para o modo de exibição do diretório. Selecione **aplicações empresariais**e, em seguida, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **SAP Business objeto Cloud**.

    ![Configurar o início de sessão único](./media/sapboc-tutorial/tutorial_sapboc_app.png) 

3. No menu da esquerda, selecione **utilizadores e grupos**.

    ![Selecionar utilizadores e grupos][202] 

4. Selecione **Adicionar**. Em seguida, no **adicionar atribuição** página, selecione **utilizadores e grupos**.

    ![A página Adicionar atribuição][203]

5. Sobre o **utilizadores e grupos** página, na lista de utilizadores, selecione **Eduarda Almeida**.

6. Sobre o **utilizadores e grupos** página, selecione **selecione**.

7. Sobre o **adicionar atribuição** página, selecione **atribuir**.

![Atribuir a função de utilizador][200] 
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Ao selecionar o mosaico de Cloud do SAP Business objeto no painel de acesso, deve ser automaticamente a sessão iniciada sua aplicação na Cloud de objeto do SAP Business.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/sapboc-tutorial/tutorial_general_01.png
[2]: ./media/sapboc-tutorial/tutorial_general_02.png
[3]: ./media/sapboc-tutorial/tutorial_general_03.png
[4]: ./media/sapboc-tutorial/tutorial_general_04.png

[100]: ./media/sapboc-tutorial/tutorial_general_100.png

[200]: ./media/sapboc-tutorial/tutorial_general_200.png
[201]: ./media/sapboc-tutorial/tutorial_general_201.png
[202]: ./media/sapboc-tutorial/tutorial_general_202.png
[203]: ./media/sapboc-tutorial/tutorial_general_203.png


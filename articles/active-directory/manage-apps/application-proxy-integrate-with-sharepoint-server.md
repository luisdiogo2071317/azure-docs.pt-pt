---
title: Ativar o acesso remoto ao SharePoint com o Proxy de aplicações do Azure AD | Documentos da Microsoft
description: Abrange as noções básicas sobre como integrar um servidor do SharePoint no local com o Proxy de aplicações do Azure AD.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: 0f1b46176ba440a11d1584846019859c63d2f263
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53135785"
---
# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>Ativar o acesso remoto ao SharePoint com o Proxy de aplicações do Azure AD

Este artigo aborda como integrar um servidor do SharePoint no local com o Proxy de aplicações do Azure Active Directory (Azure AD).

Para ativar o acesso remoto ao SharePoint com o Proxy de aplicações do Azure AD, siga as secções neste artigo passo a passo.

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que já tenha SharePoint 2013 ou mais recente no seu ambiente. Além disso, considere os seguintes pré-requisitos:

* O SharePoint inclui suporte nativo do Kerberos. Por conseguinte, os utilizadores que estão a aceder a sites internos remotamente através do Proxy de aplicações do Azure AD podem assumir que tenham uma experiência de início de sessão único (SSO).

* Este cenário inclui alterações de configuração ao seu servidor do SharePoint. Recomendamos que utilize um ambiente de teste. Dessa forma, pode fazer atualizações ao seu servidor de teste pela primeira vez e, em seguida, facilitam um ciclo de testes antes de entrar em produção.

* É necessário SSL no URL publicado. Tem de ter o SSL ativado no seu site interno para garantir que as ligações são enviados/mapeada corretamente. Se ainda não tiver configurado o SSL, consulte [configurar o SSL para SharePoint 2013](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) para obter instruções. Além disso, certifique-se de que a máquina de conector confia no certificado que emite. (O certificado não é preciso publicamente emissão.)

## <a name="step-1-set-up-single-sign-on-to-sharepoint"></a>Passo 1: Configurar o início de sessão único para o SharePoint

Para aplicações no local que utilizam a autenticação do Windows, pode obter início de sessão único (SSO) com o protocolo de autenticação Kerberos e um recurso chamado delegação restringida de Kerberos (KCD). KCD, quando configurado, permite que o conector de Proxy de aplicações obter um token do Windows para um utilizador, mesmo que o utilizador não tiver sessão iniciada Windows diretamente. Para saber mais sobre o KCD, veja [descrição geral de delegação restrita de Kerberos](https://technet.microsoft.com/library/jj553400.aspx).

Para configurar o KCD para um servidor do SharePoint, utilize os procedimentos nas seguintes secções seqüenciais:

### <a name="ensure-that-sharepoint-is-running-under-a-service-account"></a>Certifique-se de que o SharePoint está em execução sob uma conta de serviço

Em primeiro lugar, certifique-se de que o SharePoint está em execução sob uma conta de serviço definido – um sistema não local, serviço local ou serviço de rede. Opte por fazê-lo para que pode anexar os nomes de principal de serviço (SPNs) para uma conta válida. SPNs são a forma como o protocolo Kerberos identifica os diferentes serviços. E terá da conta mais tarde para configurar o KCD.

> [!NOTE]
Tem de ter um criado anteriormente conta do Azure AD para o serviço. Sugerimos que permite que uma alteração de palavra-passe automática. Para obter mais informações sobre o conjunto completo de passos e resolução de problemas, consulte [configurar a alteração de palavra-passe automático no SharePoint 2013](https://technet.microsoft.com/library/ff724280.aspx).

Para garantir que seus sites estão em execução sob uma conta de serviço definido, execute os seguintes passos:

1. Abra o **Administração Central do SharePoint 2013** site.
2. Aceda a **Security** e selecione **configurar contas de serviço**.
3. Selecione **Web Pool de aplicativos - SharePoint – 80**. As opções podem ser ligeiramente diferentes com base no nome do seu conjunto de web, ou se o conjunto de web utiliza SSL por padrão.

  ![Opções para configurar uma conta de serviço](./media/application-proxy-integrate-with-sharepoint-server/service-web-application.png)

4. Se **Selecione uma conta para este componente** campo é definido como **Serviço Local** ou **serviço de rede**, tem de criar uma conta. Se não tiver terminado e pode avançar para a secção seguinte.
5. Selecione **Registre-se conta gerida novo**. Depois de criar a sua conta, tem de definir **Pool de aplicativos Web** antes de poder utilizar a conta.

### <a name="configure-sharepoint-for-kerberos"></a>Configurar o SharePoint para Kerberos

Utilize o KCD para efetuar o início de sessão único para o servidor do SharePoint.

Para configurar o seu site do SharePoint para a autenticação Kerberos:

1. Abra o **Administração Central do SharePoint 2013** site.
2. Aceda a **gestão de aplicações**, selecione **gerenciar aplicativos web**e selecione o seu site do SharePoint. Neste exemplo, é **SharePoint – 80**.

  ![Selecionar o site do SharePoint](./media/application-proxy-integrate-with-sharepoint-server/manage-web-applications.png)

3. Clique em **provedores de autenticação** na barra de ferramentas.
4. Na **provedores de autenticação** , clique em **zona padrão** para ver as definições.
5. Na **editar autenticação** caixa de diálogo caixa, desloque para baixo até ver **tipos de autenticação de afirmações**. Certifique-se de que ambos **ativar a autenticação do Windows** e **autenticação integrada do Windows** estão selecionadas.
6. Na caixa de lista pendente para o campo de autenticação integrada do Windows, certifique-se de que **negociar (Kerberos)** está selecionada.

  ![Editar caixa de diálogo de autenticação](./media/application-proxy-integrate-with-sharepoint-server/service-edit-authentication.png)

7. Na parte inferior a **editar autenticação** caixa de diálogo, clique em **guardar**.

### <a name="set-a-service-principal-name-for-the-sharepoint-service-account"></a>Defina um nome de principal de serviço para a conta de serviço do SharePoint

Antes de configurar o KCD, tem de identificar o serviço do SharePoint a ser executado como conta de serviço que configurou. Identificar o serviço ao definir um SPN. Para obter mais informações, consulte [nomes principais de serviço](https://technet.microsoft.com/library/cc961723.aspx).

O formato do SPN é:

```
<service class>/<host>:<port>
```

O formato do SPN:

* _classe de serviço_ é um nome exclusivo para o serviço. Para o SharePoint, usar **HTTP**.

* _anfitrião_ é o domínio completamente qualificado ou o nome NetBIOS do anfitrião que o serviço está em execução. Para um site do SharePoint, este texto poderá ter de ser o URL do site, dependendo da versão do IIS que está a utilizar.

* _porta_ é opcional.

Se o FQDN do servidor do SharePoint:

```
sharepoint.demo.o365identity.us
```

Em seguida, o SPN é:

```
HTTP/sharepoint.demo.o365identity.us demo
```

Também poderá ter de definir o SPN para sites específicos no seu servidor. Para obter mais informações, consulte [a autenticação Kerberos configurar](https://technet.microsoft.com/library/cc263449(v=office.12).aspx). Preste muita atenção para a secção "Criar nomes de Principal de serviço para as suas aplicações Web através da autenticação de Kerberos".

A maneira mais fácil para definir o SPN é seguir os formatos SPN que podem já estar presentes para seus sites. Copie esses SPNs para registrar em relação à conta de serviço. Para efetuar este procedimento:

1. Navegue para o site com o SPN a partir de outra máquina.
 Quando o fizer, o conjunto relevante de tíquetes Kerberos é colocado em cache na máquina. Destas permissões de contenham o SPN da qual navegou para site de destino.

2. Pode extrair o SPN para esse site utilizando uma ferramenta chamada [Klist](https://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html). Na janela de comando que está em execução no mesmo contexto como o utilizador que acessavam o site no navegador, execute o seguinte comando:
```
Klist
```
Klist, em seguida, devolve o conjunto de SPN de destino. Neste exemplo, o valor realçado é o SPN que é necessário:

  ![Resultados de Klist de exemplo](./media/application-proxy-integrate-with-sharepoint-server/remote-sharepoint-target-service.png)

4. Agora que tem o SPN, certifique-se de que está configurado corretamente na conta de serviço que configurou para a aplicação web anteriormente. Execute o seguinte comando na linha de comandos como administrador do domínio:

 ```
 setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
 ```

 Este comando define o SPN para a conta de serviço do SharePoint a ser executado como _demo\sp_svc_.

 Substitua _http/sharepoint.demo.o365identity.us_ com o SPN para o seu servidor e _demo\sp_svc_ com a conta de serviço no seu ambiente. O comando Setspn procura o SPN antes de adicioná-lo. Neste caso, poderá ver uma **valor de SPN duplicado** erro. Se vir este erro, certifique-se de que o valor é associado com a conta de serviço.

Pode verificar se o SPN foi adicionado ao executar o comando Setspn com a opção – l. Para saber mais sobre este comando, consulte [Setspn](https://technet.microsoft.com/library/cc731241.aspx).

### <a name="ensure-that-the-connector-is-set-as-a-trusted-delegate-to-sharepoint"></a>Certifique-se de que o conector está definido como um delegado fidedigno para o SharePoint

Configure o KCD para que o serviço de Proxy de aplicações do Azure AD pode delegar as identidades de utilizador para o serviço do SharePoint. Configure o KCD ao ativar o conector do Proxy de aplicações para obter permissões de Kerberos para seus usuários que tiverem sido autenticados no Azure AD. Em seguida, esse servidor passa o contexto para a aplicação de destino ou o SharePoint neste caso.

Para configurar o KCD, repita os passos seguintes para cada computador do conector:

1. Iniciar sessão como um administrador de domínio para um controlador de domínio e, em seguida, abra **Active Directory Users and Computers**.
2. Encontre o que o conector está em execução no computador. Neste exemplo, é o mesmo servidor do SharePoint.
3. Clique duas vezes o computador e, em seguida, clique nas **delegação** separador.
4. Certifique-se de que as definições de delegação estiverem definidas como **confiar no computador para delegação apenas as serviços especificados**. Em seguida, selecione **utilizar qualquer protocolo de autenticação**.

  ![Definições de delegação](./media/application-proxy-integrate-with-sharepoint-server/delegation-box.png)

5. Clique nas **Add** botão, clique em **usuários ou computadores**e localize a conta de serviço.

  ![Adicionar o SPN para a conta de serviço](./media/application-proxy-integrate-with-sharepoint-server/users-computers.png)

6. Na lista de SPNs, selecione aquela que criou anteriormente para a conta de serviço.
7. Clique em **OK**. Clique em **OK** novamente para guardar as alterações.

## <a name="step-2-enable-remote-access-to-sharepoint"></a>Passo 2: Ativar o acesso remoto para o SharePoint

Agora que ativou o SharePoint para Kerberos e KCD configurado, está pronto para publicar o farm do SharePoint para acesso remoto através do Proxy de aplicações do Azure AD.

1. Publica o seu site do SharePoint com as seguintes definições. Para obter instruções passo a passo, consulte [publicar aplicações com o Proxy de aplicações do Azure AD](application-proxy-add-on-premises-application.md). 
   - **URL interno**: o URL do site do SharePoint internamente, tal como **https://SharePoint/**. Neste exemplo, certifique-se de usar **https**
   - **Método de pré-autenticação**: o Azure Active Directory
   - **Traduzir URL nos cabeçalhos**: não

   >[!TIP]
   >O SharePoint usa a _cabeçalho de anfitrião_ valor para procurar o site. Também gera links com base nesse valor. O efeito líquido é que qualquer ligação que gera o SharePoint é um URL publicado que está corretamente definido para utilizar o URL externo. Definindo o valor como **Sim** também permite que o conector encaminhar a solicitação para a aplicação de back-end. No entanto, definindo o valor como **não** significa que o conector não enviará o nome de anfitrião interno. Em vez disso, o conector envia o cabeçalho de anfitrião como o URL publicado para a aplicação de back-end.

   ![Publicar o SharePoint como aplicação](./media/application-proxy-integrate-with-sharepoint-server/publish-app.png)

2. Assim que a sua aplicação for publicada, configure as definições de início de sessão únicas com os seguintes passos:

   1. Na página da aplicação no portal, selecione **início de sessão único**.
   2. Para o início de sessão em modo único, selecione **a autenticação integrada do Windows**.
   3. Definir o SPN da aplicação interna para o valor que definiu anteriormente. Neste exemplo, isso seria **http/sharepoint.demo.o365identity.us**.

   ![Configurar a autenticação integrada do Windows para SSO](./media/application-proxy-integrate-with-sharepoint-server/configure-iwa.png)

3. Para concluir a configuração a sua aplicação, vá para o **utilizadores e grupos** secção e atribuir utilizadores a aceder a esta aplicação. 

## <a name="step-3-ensure-that-sharepoint-knows-about-the-external-url"></a>Passo 3: Certifique-se de que o SharePoint sabe sobre o URL externo

A última etapa é garantir que o SharePoint pode localizar o site com base na URL externo, para que ele processa links com base no que o URL externo. Pode fazê-lo através da configuração de mapeamentos de acesso alternativo para o site do SharePoint.

1. Abra o **Administração Central do SharePoint 2013** site.
2. Sob **definições do sistema**, selecione **configurar mapeamentos de acesso alternativo**. Abre a caixa de mapeamentos de acesso alternativo.

  ![Caixa de mapeamentos de acesso alternativa](./media/application-proxy-integrate-with-sharepoint-server/alternate-access1.png)

3. Na lista pendente junto **a coleção de mapeamento de acesso alternativo**, selecione **alteração alternativo acesso mapeamento coleção**.
4. Selecione o seu site – por exemplo, **SharePoint – 80**.
5. Pode optar por adicionar o URL publicado como um URL interno ou um URL público. Este exemplo utiliza um URL público, como da extranet. Se estiver a utilizar uma marca de porta personalizado se esqueça de incluir a porta personalizada no URL.
6. Clique em **Editar URLs públicas** no **Extranet** caminho e, em seguida, introduza o URL externo que foi criada quando publicou a aplicação. Por exemplo, introduza **https://sharepoint-iddemo.msappproxy.net**.

  ![Entrar no caminho](./media/application-proxy-integrate-with-sharepoint-server/alternate-access3.png)

7. Clique em **Guardar**.

Agora pode acessar o site do SharePoint externamente através do Proxy de aplicações do Azure AD.

## <a name="next-steps"></a>Passos Seguintes

- [Trabalhar com domínios personalizados no Proxy de aplicações do Azure AD](application-proxy-configure-custom-domain.md)
- [Compreender os conectores de Proxy de aplicações do Azure AD](application-proxy-connectors.md)


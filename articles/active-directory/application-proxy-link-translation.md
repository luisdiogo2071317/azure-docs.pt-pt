---
title: Traduzir ligações e URLs do Proxy de aplicação do Azure AD | Microsoft Docs
description: Abrange as noções básicas sobre conectores de Proxy de aplicações do Azure AD.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: a96296e8918c658f1dc3ebeadf01bd064e80716a
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Redirecionar codificado ligações para aplicações publicadas com o Proxy de aplicações do Azure AD

Proxy de aplicações do Azure AD disponibiliza as suas aplicações no local para os utilizadores que são remotos, ou os seus próprios dispositivos. No entanto, algumas aplicações foram desenvolvidas com ligações locais incorporadas no HTML. Estas ligações não funcionam corretamente quando a aplicação é utilizada remotamente. Quando tiver várias aplicações no local a apontar para si, os utilizadores esperam que as ligações para manter a funcionar quando não estiverem no escritório. 

É a melhor forma para se certificar de que as ligações funcionam da mesma dentro e fora da rede empresarial para configurar os URLs externos das suas aplicações para ser o mesmo que os URLs internos. Utilize [domínios personalizados](manage-apps/application-proxy-configure-custom-domain.md) para configurar os URLs externos para ter o seu nome de domínio empresarial em vez do domínio de proxy de aplicações predefinido.


Se não conseguir utilizar domínios personalizados no seu inquilino, existem várias outras opções para fornecer esta funcionalidade. Todos estes também são compatíveis com domínios personalizados e outros, pelo que pode configurar domínios personalizados e outras soluções se for necessário. 

**Opção 1: Utilizar o Browser gerido** – esta solução só é aplicável se planeia recomendável ou exigir que os utilizadores aceder à aplicação através do Browser gerido do Intune. Este irá processar todos os URLs publicados. 

**Opção 2: Utilizar a extensão de MyApps** – esta solução requer que os utilizadores instalem uma extensão de browser do lado do cliente, mas irá processar publicados todos os URLs e funciona com os browsers mais populares. 

**Opção 3: Utilizar a definição de tradução de ligação** – esta é uma definição de lado de admin é invisível para os utilizadores. No entanto, só irá processar os URLs em HTML e CSS. URLs internos hard-coded gerados através de Javascript (por exemplo) não funcionará.  

Estas três funcionalidades manter as ligações, independentemente de onde os utilizadores estão a funcionar. Quando tiver aplicações que vá diretamente a pontos finais internos ou portas, pode mapear estes URLs internos para os URLs de Proxy de aplicações externas publicada. 

 
> [!NOTE]
> A última opção é apenas para inquilinos que, para que pelo motivo, não é possível utilizar domínios personalizados com os mesmos URLs internos e externos para as suas aplicações. Antes de ativar esta funcionalidade, consulte o artigo se [domínios personalizados no Proxy de aplicações do Azure AD](manage-apps/application-proxy-configure-custom-domain.md) pode resolver o problema. 

>Ou, se a aplicação tem de configurar com ligação tradução é SharePoint, consulte [configurar mapeamentos de acesso alternativo para SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx) para outra abordagem para ligações de mapeamento. 

 
### <a name="option-1-intune-managed-browser-integration"></a>Opção 1: Integração de Browser gerido do Intune 

Pode utilizar o Intune Managed Browser para proteger ainda mais a sua aplicação e o conteúdo. Para utilizar esta solução, terá de exigir/Recomendamos os utilizadores acedam a aplicação através do Browser gerido do Intune. Todos os URLs internos publicados com o Proxy de aplicações serão reconhecidos pelo Browser gerido e redirecionados para o URL externo correspondente. Isto garante que todos os hard-coded internos URLs trabalham e, se um utilizador passa para o browser e tipos diretamente o URL interno, funciona mesmo se o utilizador for remoto.  

Para obter mais informações, incluindo como configurar esta opção, consulte o [Managed Browser](https://docs.microsoft.com/intune/app-configuration-managed-browser) documentação.  

### <a name="option-2-myapps-browser-extension"></a>Opção 2: Extensão de MyApps Browser 

Com a extensão de Browser MyApps, todos os URLs internos publicados com o Proxy da aplicação são reconhecidos pela extensão e redirecionados para o URL externo correspondente. Isto garante que todos os hard-coded internos URLs trabalham e, se um utilizador vai para a barra de endereço do browser e tipos diretamente o URL interno, funciona mesmo se o utilizador for remoto.  

Para utilizar esta funcionalidade, o utilizador tem de transferir a extensão e de ter sessão iniciada. Não há nenhuma outra configuração necessária para administradores ou pelos utilizadores. 

 

### <a name="option-3-link-translation-setting"></a>Opção 3: Definição de tradução de ligação 

Quando a tradução de ligação está ativada, o serviço de Proxy de aplicações procura através de HTML e CSS publicadas ligações internas e converte-los para que os utilizadores obtêm uma experiência ininterrupta. 



## <a name="how-link-translation-works"></a>Como funciona a tradução de ligação

Após a autenticação, quando o servidor proxy transmite os dados da aplicação para o utilizador, o Proxy de aplicações analisa a aplicação para ligações de codificado e substitui-los com os seus respetivos publicados URLs externos.

Proxy de aplicações parte do princípio de que as aplicações são com codificação UTF-8. Se não for esse o caso, especifique o tipo de codificação no cabeçalho de resposta de http, tal como `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>As ligações são afetadas?

A funcionalidade de tradução de ligação procura apenas as ligações que estão em tags de código no corpo de uma aplicação. Proxy de aplicações tem uma funcionalidade à parte para traduzir cookies ou URLs nos cabeçalhos. 

Existem dois tipos comuns de ligações internos em aplicações no local:

- **Ligações internas relativas** que ponto para um recurso partilhado numa estrutura de ficheiros local como `/claims/claims.html`. Estas ligações funcionam automaticamente nas aplicações que são publicadas através do Proxy de aplicações e continuam a trabalhar com ou sem tradução de ligação. 
- **Ligações internas codificado** para outras aplicações no local como `http://expenses` ou publicado ficheiros como `http://expenses/logo.jpg`. A funcionalidade de tradução de ligação funciona em ligações interno codificado e altera-los para que apontem para os URLs externos que os utilizadores remotos têm de passar por.

### <a name="how-do-apps-link-to-each-other"></a>Como ligação a aplicações entre si?

Tradução de ligação está ativada para cada aplicação, para que tenham o controlo sobre a experiência de utilizador ao nível por aplicação. Ativar a tradução de ligação para uma aplicação quando pretender que as ligações *de* essa aplicação para ser convertido, não ligar *para* essa aplicação. 

Por exemplo, suponha que tem três aplicações publicadas através do Proxy de aplicações que todos os ligação entre si: benefícios, as despesas e levar. Há uma quarta aplicação, comentários, que não estão publicado através do Proxy de aplicações.

Quando ativar a tradução de ligação para a aplicação de benefícios, as ligações para as despesas e levar são redirecionadas para os URL externos para essas aplicações, mas a ligação para comentários não é redirecionada porque não há nenhum URL externo. As ligações de despesas e levar para vantagens não funcionam porque a tradução de ligação não foi ativada para as duas aplicações.

![Ligações de benefícios para outras aplicações quando a tradução de ligação está ativada](./media/application-proxy-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>As ligações não são convertidas?

Para melhorar o desempenho e segurança, não são convertidas algumas hiperligações:

- Ligações não dentro de etiquetas de código. 
- Ligações não no HTML ou CSS. 
- Ligações internas abertas a partir de outros programas. Não possível converter ligações enviados através de e-mail ou mensagem instantânea, ou parte de outros documentos. Os utilizadores precisam de saber para ir para o URL externo.

Se precisar de suportar um dos dois destes cenários, utilize os mesmos URLs internos e externos em vez de tradução de ligação.  

## <a name="enable-link-translation"></a>Ativar a tradução de ligação

Introdução à tradução de ligação é tão fácil como clicar num botão:

1. Inicie sessão no [portal do Azure](https://portal.azure.com) como administrador.
2. Aceda a **do Azure Active Directory** > **aplicações empresariais** > **todas as aplicações** > selecione a aplicação que pretende gerir > **proxy de aplicações**.
3. Ativar **traduzir URLs no corpo de aplicação** para **Sim**.

   ![Selecione Sim para tradução de URLs no corpo de aplicação](./media/application-proxy-link-translation/select_yes.png).
4. Selecione **guardar** para aplicar as suas alterações.

Agora, quando os utilizadores acederem a esta aplicação, o proxy irá analisar automaticamente para URL interno que tiverem sido publicada através do Proxy de aplicações no seu inquilino.

## <a name="send-feedback"></a>Enviar comentários

Queremos a sua ajudá-lo para tornar esta funcionalidade funciona para todas as suas aplicações. Iremos procurar mais de 30 tags HTML e CSS. Se tiver um exemplo de ligações geradas que não estão a ser traduzidos, envie um fragmento de código para [comentários de Proxy de aplicação](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Passos Seguintes
[Utilizar domínios personalizados com o Proxy de aplicações do Azure AD](manage-apps/application-proxy-configure-custom-domain.md) para ter o mesmo URL interno e externo

[Configurar os mapeamentos de acesso alternativo para SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx)

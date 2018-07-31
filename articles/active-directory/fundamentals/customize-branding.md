---
title: Personalizar a página de início de sessão do seu inquilino do Azure AD | Microsoft Docs
description: Saiba como adicionar uma imagem corporativa à página de início de sessão no Azure
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 07/20/2018
ms.author: lizross
ms.reviewer: kexia
custom: it-pro
ms.openlocfilehash: 7804d6b0d4a100997fb545e678458424dac6ceed
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39227291"
---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Início Rápido: adicionar uma imagem corporativa à sua página de início de sessão no Azure AD
Para evitar confusões, muitas empresas pretendem aplicar um aspeto e funcionalidade consistentes em todos os Web sites e serviços que gerem. O Azure Active Directory (Azure AD) fornece esta capacidade ao permitir-lhe personalizar o aspeto da página de início de sessão com o logótipo da sua empresa e esquemas de cores personalizados. A página de início de sessão é apresentada ao iniciar sessão em aplicações baseadas na Web, como o Office 365, que utilizam o Azure AD como o fornecedor de identidade. Interage com esta página para introduzir as suas credenciais.

> [!NOTE]
> * A imagem corporativa só estará disponível se tiver comprado a licença Premium ou Básica do Azure AD, ou se tiver uma licença do Office 365. Para saber se uma funcionalidade é suportada pelo seu tipo de licença, consulte a [página de informações de preços do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * As edições Premium e Básica do Azure AD estão disponíveis para clientes na China que utilizem a instância mundial do Azure Active Directory. As edições Premium e Básica do Azure AD não são atualmente suportadas pelo serviço do Azure operado pela 21Vianet na China. Para obter mais informações, contacte-nos através do [Fórum do Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customizing-the-sign-in-page"></a>Personalizar a página de início de sessão

<!--You can customize the following elements on the sign-in page: <attach image>-->

As personalizações de imagem corporativa são apresentadas na página de início de sessão no Azure AD quando os utilizadores acedem a um URL de inquilino específico, como [*https://outlook.com/contoso.com*](https://outlook.com/contoso.com), ou passando a variável de domínio no URL, como [*https://passwordreset.microsoftonline.com/?whr=contoso.com*](https://passwordreset.microsoftonline.com/?whr=contoso.com)

Por exemplo, quando os utilizadores visitam www.office.com, a página de início de sessão não mostra quaisquer personalizações de imagem corporativa, uma vez que o utilizador não inseriu ainda as credenciais. Depois de um utilizador introduzir o respetivo ID de utilizador ou selecionar um mosaico de utilizador, a imagem corporativa é apresentada.

> [!NOTE]
> * O seu nome de domínio tem de aparecer como "Ativo" na parte **Domínios** do portal do Azure em que tiver configurado a imagem corporativa. Para obter mais informações, veja [Adicionar um nome de domínio personalizado](add-custom-domain.md).
> * A imagem corporativa na página de início de sessão não passa para a página de início de sessão de contas Microsoft. Se os seus funcionários ou convidados empresariais iniciarem sessão com uma conta Microsoft, a página de início de sessão deles não reflete a imagem corporativa da sua organização.


### <a name="banner-logo"></a>Logótipo de faixa 

Descrição | Restrições | Recomendações
------- | ------- | ----------
O logótipo de faixa é apresentado nas páginas de início de sessão e do painel de Acesso.<br>Na página de início de sessão, o logótipo é apresentado após o nome de utilizador ser introduzido. | JPG ou PNG transparente<br>Altura máx.: 36 px<br>Largura máx.: 245 px | Utilize o logótipo da sua organização aqui.<br>Utilize uma imagem transparente. Não parta do princípio que o fundo será branco.<br>Não adicione preenchimento em torno do seu logótipo na imagem, pois o logótipo parecerá desproporcionalmente pequeno.

### <a name="username-hint"></a>Sugestão de nome do utilizador   
Descrição | Restrições | Recomendações
------- | ------- | ----------
Esta opção personaliza o texto de sugestão no campo de nome de utilizador. | Texto Unicode, até 64 carateres<br>Apenas texto simples | Se prevê que utilizadores convidados fora da sua organização irão iniciar sessão na sua aplicação, recomendamos que não configure esta opção.
            
### <a name="sign-in-page-text"></a>Texto da página de início de sessão   
Descrição | Restrições | Recomendações
------- | ------- | ----------
Esta opção é apresentada na parte inferior do formulário de início de sessão e pode ser utilizada para comunicar informações adicionais, como o número de telefone do suporte técnico ou uma instrução legal. | Texto Unicode, até 256 carateres<br>Apenas texto simples (sem ligações ou tags de HTML)    

### <a name="sign-in-page-image"></a>Imagem da página de início de sessão  
Descrição | Restrições | Recomendações
------- | ------- | ----------
Esta opção é apresentada no fundo da página de início de sessão, está ancorada ao centro do espaço visível e dimensiona e recorta-se para preencher a janela do browser.    <br>Em ecrãs estreitos, como os dos telemóveis, esta imagem não é apresentada.<br>Uma máscara preta com 0,55 de opacidade é aplicada nesta imagem quando a página é carregada. | JPG ou PNG<br>Dimensões da imagem: 1920 x 1080 px<br>Tamanho do ficheiro: &lt; 300 KB | <br>Utilize imagens quando não existir um foco forte no assunto. O formulário de início de sessão opaco é apresentado por cima do centro desta imagem e pode cobrir qualquer parte da imagem, dependendo do tamanho da janela do browser.<br>Mantenha o tamanho de ficheiro pequeno para garantir tempos de carregamento rápidos. 

### <a name="sign-in-page-background-color"></a>Cor de fundo da página de início de sessão
Descrição | Restrições | Recomendações
------- | ------- | ----------
Esta cor é utilizada em vez da imagem de fundo em ligações de largura de banda baixa. | Cor RGB em hexadecimal (exemplo: #FFFFFF) | Sugerimos que utilize a cor primária do logótipo de faixa ou a cor da organização.

### <a name="square-logo-image"></a>Imagem de logótipo quadrado
Descrição | Restrições | Recomendações
------- | ------- | ----------
Esta imagem é apresentada durante a configuração de novos PCs Enterprise Windows 10. Fornece contexto aos funcionários quando estiverem a configurar os novos PCs do emprego. A imagem é apresentada aos inquilinos que utilizam o [Windows AutoPilot](https://blogs.windows.com/business/2017/06/29/delivering-modern-promise-windows-10/?utm_source=dlvr.it&utm_medium=twitter#gDTp1u6q35bvDWIS.97) para implementar os dispositivos de trabalho e em páginas de introdução de palavra-passe noutras experiências do Windows 10. | PNG (preferido) ou JPG transparente<br>Dimensões da imagem: 240 x 240 px<br>Tamanho do ficheiro: &lt; 10 KB | Utilize o logótipo da sua organização aqui.<br> Utilize uma imagem transparente.<br>Não parta do princípio que o fundo será branco.<br>Não adicione preenchimento ao logótipo na imagem, pois o logótipo parecerá desproporcionalmente pequeno.

### <a name="show-option-to-remain-signed-in"></a>Mostrar a opção para manter a sessão iniciada
Descrição | Restrições | Recomendações
------- | ------- | ----------
O início de sessão no Azure AD dá ao utilizador a opção de manter a sessão iniciada depois de fechar e abrir novamente o browser. Esta definição oculta essa opção.<br>Defina como **Não** para ocultar esta opção dos utilizadores. | &nbsp; | Ocultar a opção não afeta a duração da sessão.<br>Algumas funcionalidades do SharePoint Online e do Office 2010 dependem da capacidade de os utilizadores poderem escolher manter a sessão iniciada. Se tiver definido esta opção como **Não**, os utilizadores poderão ver avisos adicionais e inesperados para iniciar sessão.

> [!NOTE]
> Todos os elementos são opcionais. Por exemplo, se especificar um logótipo de faixa sem imagem de fundo, a página de início de sessão irá mostrar o seu logótipo e a imagem de fundo do site de destino (por exemplo, o Office 365).

## <a name="add-company-branding-to-your-directory"></a>Adicionar a imagem corporativa ao seu diretório

1. Inicie sessão no [Centro de administradores do Azure AD](https://aad.portal.azure.com) com uma conta que seja administrador global do inquilino.
2. Selecione **Azure Active Directory** > **Imagem Corporativa** > **Editar**.
  
  ![Abrir uma imagem corporativa personalizada](./media/customize-branding/navigation-to-branding.png)
3. Modifique os elementos que pretende personalizar. Todos os elementos são opcionais.
  
  ![Editar uma imagem corporativa personalizada](./media/customize-branding/edit-branding.png)
4. Quando tiver terminado, selecione **Guardar**.

Pode demorar até uma hora para que sejam apresentadas quaisquer alterações que tiver efetuado na imagem corporativa da página de início de sessão.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Adicionar uma imagem corporativa específica de idiomas ao seu diretório

1. Inicie sessão no [Centro de administradores do Azure AD](https://aad.portal.azure.com) com uma conta que seja administrador global do diretório.
2. Selecione **Azure Active Directory** > **Imagem Corporativa** > **Novo idioma**.
  
  ![Adicionar elementos de imagem corporativa específica de idiomas](./media/customize-branding/add-language.png)
3. Modifique os elementos que pretende personalizar. Todos os elementos são opcionais.
4. Quando tiver terminado, selecione **Guardar**.

Pode demorar até uma hora para que sejam apresentadas quaisquer alterações que tiver efetuado na imagem corporativa da página de início de sessão.

## <a name="next-steps"></a>Passos seguintes
Neste início rápido, aprendeu a adicionar a imagem corporativa ao seu diretório do Azure AD. 

Pode utilizar a ligação seguinte para configurar a imagem corporativa no Azure AD a partir do portal do Azure.

> [!div class="nextstepaction"]
> [Configurar o branding da empresa](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 
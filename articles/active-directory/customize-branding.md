---
title: "Personalizar a página de início de sessão para o seu inquilino do Azure Active Directory | Microsoft Docs"
description: "Saiba como adicionar uma imagem corporativa para a página de início de sessão do Azure"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/19/2018
ms.author: curtand
ms.reviewer: kexia
custom: it-pro
ms.openlocfilehash: 03a6b82f769ed9a36c5d3ff9934de75d1536e1ae
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Guia de introdução: Adicionar imagem corporativa à sua página de início de sessão no Azure AD
Para evitar confusões, muitas empresas pretendem aplicar um aspeto e funcionalidade consistentes em todos os Web sites e serviços que gerem. Azure Active Directory (Azure AD) fornece esta capacidade ao permitir-lhe personalizar o aspeto da página de início de sessão com o logótipo da empresa e esquemas de cores personalizados. É apresentada a página de início de sessão quando iniciar sessão para aplicações baseadas na web, como o Office 365 que utilizam o Azure AD como o fornecedor de identidade. Interagir com esta página para introduzir as suas credenciais.

> [!NOTE]
> * Imagem corporativa da empresa está disponível apenas se comprou a licença básica ou Premium do Azure AD ou tem uma licença do Office 365. Para saber se uma funcionalidade é suportada pelo tipo de licença, verifique o [página de informações de preços do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Edições do Azure AD Premium e Basic estão disponíveis para os clientes na China utilizando a instância mundial do Azure Active Directory. Edições do Azure AD Premium e Basic não são atualmente suportadas no serviço do Azure operado pela 21Vianet na China. Para obter mais informações, comunique com-na [fórum do Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customizing-the-sign-in-page"></a>Personalizar a página de início de sessão

<!--You can customize the following elements on the sign-in page: <attach image>-->

Empresa personalizações de imagem corporativa apresentado na página de início de sessão do Azure AD quando os utilizadores acedem um URL de inquilino específico como [ *https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

Por exemplo, quando os utilizadores visitam www.office.com, a página de início de sessão não mostra as personalizações de imagem corporativa porque o utilizador ainda não introduziu as credenciais da empresa. Depois de um utilizador introduz o seu ID de utilizador ou seleciona um mosaico de utilizador, apresenta a imagem corporativa da empresa.

> [!NOTE]
> * O nome de domínio tem de aparecer como "Ativo" no **domínios** parte do portal do Azure em que tiver configurado uma imagem corporativa. Para obter mais informações, consulte [adicionar um nome de domínio personalizado](add-custom-domain.md).
> * A imagem corporativa na página de início de sessão não passa para a página de início de sessão de contas Microsoft. Se os seus funcionários e convidados de negócio inicia sessão com uma conta Microsoft pessoal, a respetiva página de início de sessão não reflete a imagem corporativa da sua organização.


### <a name="banner-logo"></a>Logótipo de faixa 

Descrição | Restrições | Recomendações
------- | ------- | ----------
O logótipo de faixa é apresentado o início de sessão e as páginas do painel de acesso.<br>Na página de início de sessão, o logótipo mostra após o nome de utilizador é introduzido. | Transparente JPG ou PNG<br>A altura máxima: 36 px<br>A largura máxima: 245 px | Utilize aqui o logótipo da sua organização.<br>Utilize uma imagem transparente. Não parta do princípio que a imagem de fundo será branca.<br>Não adicione preenchimento à volta o logótipo na imagem ou o logótipo procurará disproportionately pequeno.

### <a name="username-hint"></a>Sugestão de nome de utilizador   
Descrição | Restrições | Recomendações
------- | ------- | ----------
Esta opção customizes o texto de sugestão no campo do nome de utilizador. | Texto Unicode até 64 carateres<br>Apenas texto simples | Se espera que os utilizadores convidados fora da sua organização para iniciar sessão na sua aplicação, recomendamos que não configurou esta opção.
            
### <a name="sign-in-page-text"></a>Texto da página de início de sessão   
Descrição | Restrições | Recomendações
------- | ------- | ----------
Esta opção é apresentada na parte inferior do formulário de início de sessão e pode ser utilizada para comunicar informações adicionais, tais como o número de telefone para o suporte técnico ou uma instrução legal. | Texto Unicode até 256 carateres<br>Apenas texto simples (sem ligações ou tags de HTML)    

### <a name="sign-in-page-image"></a>Imagem da página de início de sessão  
Descrição | Restrições | Recomendações
------- | ------- | ----------
Esta opção é apresentada no fundo da página de início de sessão, está ancorada o Centro de espaço podem ser visualizado, escalas e crops para preencher a janela do browser.    <br>Em ecrãs estreitos, como telemóveis, esta imagem não é apresentada.<br>Uma máscara preta com opacidade 0.55 é aplicada através desta imagem quando a página é carregada. | JPG ou PNG<br>As dimensões de imagem: px de 1920 x 1080<br>Tamanho de ficheiro: &lt; 300 KB | <br>Utilizar imagens onde não é um foco requerente forte. O formulário de início de sessão opaco é apresentado ao longo do centro desta imagem e pode abranger qualquer parte da imagem, dependendo do tamanho da janela do browser.<br>Mantenha o tamanho do ficheiro pequeno para garantir que os tempos de carregamento rápida. 

### <a name="sign-in-page-background-color"></a>Cor de fundo da página de início de sessão
Descrição | Restrições | Recomendações
------- | ------- | ----------
Esta cor é utilizado em vez da imagem de fundo em ligações de largura de banda baixa. | Cor RGB como hexadecimal (exemplo: #FFFFFF | Sugerimos que utilizar a cor primária do logótipo de faixa ou a cor da organização.

### <a name="square-logo-image"></a>Imagem de logótipo quadrado
Descrição | Restrições | Recomendações
------- | ------- | ----------
Esta imagem é apresentado durante a configuração para o novo Enterprise 10 PCs com o Windows. Fornece um contexto aos funcionários quando configuraram seus PC de trabalho nova. A imagem é apresentada para os inquilinos que utilizam [Windows AutoPilot](https://blogs.windows.com/business/2017/06/29/delivering-modern-promise-windows-10/?utm_source=dlvr.it&utm_medium=twitter#gDTp1u6q35bvDWIS.97) para implementar os seus dispositivos de trabalho, e na entrada de palavra-passe ocorre com páginas no outros Windows 10. | Transparente PNG (preferidos) ou JPG<br>As dimensões de imagem: 240 x 240 px<br>Tamanho de ficheiro: &lt; 10 KB | Utilize aqui o logótipo da sua organização.<br> Utilize uma imagem transparente.<br>Não parta do princípio que a imagem de fundo será branca.<br>Não adicione preenchimento para o logótipo na imagem ou o logótipo procurará disproportionately pequeno.

### <a name="show-option-to-remain-signed-in"></a>Mostrar a opção para manter a sessão iniciada
Descrição | Restrições | Recomendações
------- | ------- | ----------
Azure AD início de sessão fornece ao utilizador a opção de permanecer com sessão iniciada quando fechar e reabrir o respetivo browser. Esta definição oculta essa opção.<br>Definido como **não** para ocultar esta opção dos seus utilizadores. | &nbsp; | Ocultar a opção não afeta a duração de sessão.<br>Algumas funcionalidades do SharePoint Online e Office 2010 dependem dos utilizadores a capacidade de escolher permanecer com sessão iniciada. Se definir esta opção para **não**, os utilizadores, podem ver avisos adicionais e inesperados para o início de sessão.

> [!NOTE]
> Todos os elementos são opcionais. Por exemplo, se especificar um logótipo de faixa com nenhuma imagem de fundo, a página de início de sessão irá apresentar o logótipo e a imagem de fundo para o site de destino (por exemplo, o Office 365).

## <a name="add-company-branding-to-your-directory"></a>Adicionar a imagem corporativa para o seu diretório

1. Inicie sessão no [o Centro de administração do Azure AD](https://aad.portal.azure.com) com uma conta que seja um administrador global do inquilino.
2. Selecione **utilizadores e grupos** > **da empresa de imagem corporativa** > **editar**.
  
  ![Abrir a imagem corporativa personalizado](./media/customize-branding/navigation-to-branding.png)
3. Modifique os elementos que pretende personalizar. Todos os elementos são opcionais.
  
  ![Editar a imagem corporativa personalizado](./media/customize-branding/edit-branding.png)
5. Quando estiver pronto, selecione **guardar**.

Pode demorar até uma hora para todas as alterações efetuadas para a página de início de sessão de imagem corporativa sejam apresentados.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Adicionar a imagem corporativa específicas do idioma ao seu diretório

1. Inicie sessão no [Centro de administradores do Azure AD](https://aad.portal.azure.com) com uma conta que seja administrador global do diretório.
2. Selecione **utilizadores e grupos** > **da empresa de imagem corporativa** > **novo idioma**.
  
  ![Adicionar elementos de imagem corporativa específicas do idioma](./media/customize-branding/add-language.png)
5. Modifique os elementos que pretende personalizar. Todos os elementos são opcionais.
6. Quando estiver pronto, selecione **guardar**.

Pode demorar até uma hora para todas as alterações efetuadas para a página de início de sessão de imagem corporativa sejam apresentados.

## <a name="next-steps"></a>Passos Seguintes
Este guia de introdução, aprendeu como adicionar imagem corporativa para o diretório do Azure AD. 

Pode utilizar a hiperligação seguinte para configurar a sua imagem corporativa no Azure AD a partir do portal do Azure.

> [!div class="nextstepaction"]
> [Configurar o branding da empresa](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 
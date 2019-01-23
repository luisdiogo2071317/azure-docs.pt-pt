---
title: Resolução de problemas a extensão do painel de acesso do Azure para o IE | Documentos da Microsoft
description: Como utilizar a política de grupo para implementar o suplemento do Internet Explorer para o portal as minhas aplicações.
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/11/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7ccaf272a79268ecd781821593a6041d5653a39b
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54471893"
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>A extensão do painel de acesso de resolução de problemas do Internet Explorer
Este artigo ajuda-o a resolver os problemas seguintes:

* Não for possível aceder às suas aplicações através do portal as minhas aplicações ao utilizar o Internet Explorer.
* Verá a mensagem "A instalar o Software", mesmo que já tiver instalado o software.

Se for um administrador, consulte também: [Como implementar a extensão do painel de acesso para o Internet Explorer usando a diretiva de grupo](deploy-access-panel-browser-extension.md)

## <a name="run-the-diagnostic-tool"></a>Execute a ferramenta de diagnóstico
Pode diagnosticar problemas de instalação com a extensão do painel de acesso ao transferir e executar a ferramenta de diagnóstico do painel de acesso:

1. [Clique aqui para transferir a ferramenta de diagnóstico.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Abra o ficheiro e prima **extrair todos os** botão.
   
    ![Prima extrair tudo](./media/manage-access-panel-browser-extension/extract1.png)
3. Em seguida, prima a **extrair** botão para continuar.
   
    ![Prima Extract](./media/manage-access-panel-browser-extension/extract2.png)
4. Para executar a ferramenta, com o botão direito no arquivo chamado **AccessPanelExtensionDiagnosticTool**, em seguida, selecione **aberta com > Microsoft Windows com base em Script Host**.
   
    ![Abrir com > Microsoft Windows com base em Host de Script](./media/manage-access-panel-browser-extension/open_tool.png)
5. Em seguida, verá a janela de diagnóstico seguinte, que descreve o que poderá estar errado com a sua instalação.
   
    ![Um exemplo da janela de diagnóstico](./media/manage-access-panel-browser-extension/tool_preview.png)
6. Clique em "**Sim**" para permitir que o programa de corrigir os problemas que foram encontrados.
7. Para guardar estas alterações, feche cada janela do Internet Explorer e, em seguida, abra novamente o Internet Explorer.<br />Se ainda não é possível aceder às suas aplicações, tente os passos abaixo.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Verifique que a extensão do painel de acesso está ativada
Para verificar que a extensão do painel de acesso está ativada no Internet Explorer:

1. No Internet Explorer, clique nas **ícone de engrenagem** no canto superior direito da janela. Em seguida, selecione **opções da Internet**.<br />(Em versões mais antigas do Internet Explorer pode encontrá-lo na **ferramentas > Opções da Internet**.
   
    ![Vá para ferramentas > Opções da Internet](./media/manage-access-panel-browser-extension/internetoptions.png)
2. Clique nas **programas** separador, em seguida, clique nas **gerir suplementos** botão.
   
    ![Clique em Gerir suplementos](./media/manage-access-panel-browser-extension/internetoptions_programs.png)
3. Nesta caixa de diálogo, selecione **extensão do painel de acesso** e, em seguida, clique nas **ativar** botão.
   
    ![Clique em ativar](./media/manage-access-panel-browser-extension/enableaddon.png)
4. Para guardar estas alterações, feche cada janela do Internet Explorer e, em seguida, abra novamente o Internet Explorer.

## <a name="enable-extensions-for-inprivate-browsing"></a>Ativar extensões para navegação InPrivate
Se estiver a utilizar o modo de Navegação InPrivate:

1. No Internet Explorer, clique nas **ícone de engrenagem** no canto superior direito da janela. Em seguida, selecione **opções da Internet**.<br />(Em versões mais antigas do Internet Explorer pode encontrá-lo na **ferramentas > Opções da Internet**.
   
    ![Um exemplo da janela de diagnóstico](./media/manage-access-panel-browser-extension/inprivateoptions.png)
2. Vá para o **privacidade** separador, em seguida, **desmarque** a caixa de seleção rotulada como **desativar as barras de ferramentas e extensões quando começar a Navegação InPrivate**</p>
   
    ![Desmarque a opção Disable barras de ferramentas e extensões quando começar a Navegação InPrivate](./media/manage-access-panel-browser-extension/enabletoolbars.png)
3. Para guardar estas alterações, feche cada janela do Internet Explorer e, em seguida, abra novamente o Internet Explorer.

## <a name="uninstall-the-access-panel-extension"></a>Desinstalar a extensão do painel de acesso
Para desinstalar a extensão do painel de acesso do seu computador:

1. No seu teclado, prima a **chave de Windows** para abrir o menu Iniciar. Quando o menu é aberto, pode digitar nada para fazer uma pesquisa. Escreva "Painel de controlo" e, em seguida, abra a **painel de controlo** quando for apresentada nos resultados da pesquisa.
   
    ![Procure o painel de controlo](./media/manage-access-panel-browser-extension/search_sm.png)
2. No canto superior direito do painel de controle, altere a **visualizar** a opção de **ícones grandes**. Em seguida, localize e clique nas **programas e funcionalidades** botão.
   
    ![Chang o modo de exibição para mostrar ícones grandes](./media/manage-access-panel-browser-extension/control_panel.png)
3. Na lista, selecione **extensão do painel de acesso**e um simples clique a **desinstalação** botão.
   
    ![Clique em desinstalar](./media/manage-access-panel-browser-extension/uninstall.png)
4. Pode, em seguida, tentar instalar a extensão novamente para ver se o problema foi resolvido.

Se tiver problemas a desinstalar a extensão, pode também removê-lo com o [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) ferramenta.

## <a name="related-articles"></a>Artigos relacionados
* [Acesso a aplicações e início de sessão único com o Azure Active Directory](what-is-single-sign-on.md)
* [Como implementar a extensão do painel de acesso para o Internet Explorer usando a diretiva de grupo](deploy-access-panel-browser-extension.md)


---
title: Galerias de módulos e Runbooks de automatização do Azure
description: Runbooks e módulos da Microsoft e da Comunidade estão disponíveis para instalar e utilizar no seu ambiente de automatização do Azure.  Este artigo descreve como pode aceder a estes recursos e para contribuir com os runbooks na galeria.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 7330d826cb196a664f06198a0e83f73bd7763ef9
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54428113"
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Galerias de módulos e Runbooks de automatização do Azure
Em vez de criar seus próprios runbooks e módulos na automatização do Azure, pode aceder a uma variedade de cenários que já foram criadas pela Microsoft e pela Comunidade.  Pode utilizar estes cenários sem modificações ou pode usá-los como um ponto de partida e editá-los às suas necessidades específicas.

> [!NOTE]
> A nova [módulo do Azure PowerShell Az](/powershell/azure/new-azureps-module-az?view=azurermps-6.13.0) não são suportados na automatização do Azure. Quaisquer scripts transferidos a partir da galeria do PowerShell com os cmdlets que não funcionará na automatização do Azure.

Pode obter runbooks a partir da [Galeria de Runbooks](#runbooks-in-runbook-gallery) e módulos da [galeria do PowerShell](#modules-in-powerShell-gallery).  Também pode contribuir para a Comunidade por meio do compartilhamento de cenários que desenvolver, consulte [adicionar um runbook na Galeria](automation-runbook-gallery.md#adding-a-runbook-to-the-runbook-gallery)

## <a name="runbooks-in-runbook-gallery"></a>Runbooks na Galeria de Runbooks
O [Galeria de Runbooks](https://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=RootCategory&f\[0\].Value=WindowsAzure&f\[1\].Type=SubCategory&f\[1\].Value=WindowsAzure_automation&f\[1\].Text=Automation) fornece uma variedade de runbooks da Microsoft e da Comunidade que pode ser importado para a automatização do Azure. Pode baixar um runbook a partir da galeria, que está alojada na [TechNet Script Center](https://gallery.technet.microsoft.com/scriptcenter/site/upload), ou pode importar runbooks diretamente a partir da galeria no portal do Azure.

Só é possível importar diretamente a partir da Galeria de Runbooks no portal do Azure. Não é possível executar esta função com o Windows PowerShell.

> [!NOTE]
> Deve validar os conteúdos de todos os runbooks que obter a partir da Galeria de Runbooks e tenha muito cuidado na instalação e executá-los num ambiente de produção.
> 
> 

### <a name="to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Para importar um runbook a partir da Galeria de Runbooks no portal do Azure
1. No portal do Azure, abra a sua conta da Automatização.
2. Sob **automatização de processos**, clique em **Galeria de Runbooks**
3. Localize o item de galeria que pretende e selecione-o para ver os respectivos detalhes. No lado esquerdo pode introduzir parâmetros adicionais de pesquisa para o publicador e o tipo.
   
    ![Procurar Galeria](media/automation-runbook-gallery/browse-gallery.png)
5. Clique em **projeto de código do modo de exibição** para ver o item no [TechNet Script Center](https://gallery.technet.microsoft.com/).
6. Para importar um item, clique na mesma para ver os detalhes e, em seguida, clique nas **importar** botão.
   
    ![Botão Importar](media/automation-runbook-gallery/gallery-item-detail.png)
7. Opcionalmente, altere o nome do runbook e, em seguida, clique em **OK** para importar o runbook.
8. O runbook é apresentado no **Runbooks** separador para a conta de automatização.

### <a name="adding-a-runbook-to-the-runbook-gallery"></a>Adicionar um runbook para a Galeria de runbooks
A Microsoft incentiva a adicionar runbooks para a Galeria de Runbooks que acredita que seria útil para outros clientes.  Pode adicionar um runbook pelo [carregá-lo para o Script Center](https://gallery.technet.microsoft.com/site/upload) tendo em conta os seguintes detalhes.

* Tem de especificar *Windows Azure* para o **categoria** e *automatização* para o **subcategoria** para o runbook seja exibida na Assistente.  
* O carregamento tem de ser um único arquivo. ps1 ou. graphrunbook.  Se o runbook necessitar de todos os módulos, runbooks subordinados ou ativos, em seguida, deverá apresentar a aqueles na descrição de envio e na secção de comentários do runbook.  Se tiver um cenário que requerem vários runbooks, em seguida, carrega cada um separadamente e listar os nomes dos runbooks relacionados em cada uma das suas descrições. Certifique-se de que utiliza as mesmas etiquetas para que elas aparecem da mesma categoria. Um usuário terá que ler a descrição de saber que outros runbooks são necessários o cenário funcione.
* Adicione a etiqueta "GraphicalPS" se estiver publicando um **runbook gráfico** (não um fluxo de trabalho gráfico). 
* Inserir trecho de código de um PowerShell ou o fluxo de trabalho do PowerShell para a descrição usando **inserir a secção de código** ícone.
* O resumo para o carregamento é apresentado nos resultados da Galeria de Runbooks, para que deve fornecer informações detalhadas que ajudam um usuário a identificar a funcionalidade do runbook.
* Deve atribuir um a três das etiquetas seguintes para o carregamento.  O runbook está listado no assistente nas categorias que correspondem a suas marcas.  Todas as marcas não nesta lista são ignoradas pelo assistente. Se não especificar qualquer etiquetas correspondentes, o runbook está listado na outra categoria.
  
  * Cópia de segurança
  * Gestão de Capacidade
  * Controle de alterações
  * Conformidade
  * Desenvolvimento / teste ambientes
  * Recuperação Após Desastre
  * Monitorização
  * Aplicação de patches
  * Aprovisionamento
  * Remediação
  * Gerenciamento de ciclo de vida VM
* Automatização atualiza a Galeria de uma vez por hora, para que não verá as suas contribuições imediatamente.

## <a name="modules-in-powershell-gallery"></a>Módulos na galeria do PowerShell
Módulos do PowerShell contêm cmdlets que podem ser utilizados nos runbooks e módulos existentes que podem instalar na automatização do Azure estão disponíveis no [galeria do PowerShell](https://www.powershellgallery.com).  Pode iniciar esta galeria a partir do portal do Azure e instalá-los diretamente na automatização do Azure ou pode transferi-los e instalá-los manualmente.  

### <a name="to-import-a-module-from-the-automation-module-gallery-with-the-azure-portal"></a>Para importar um módulo a partir da galeria do módulo de automatização no portal do Azure
1. No portal do Azure, abra a sua conta da Automatização.
2. Selecione **módulos** sob **recursos partilhados** para abrir a lista de módulos.
4. Clique em **Galeria de procura** da parte superior da página.
   
    ![Galeria de módulos](media/automation-runbook-gallery/modules-blade.png) <br>
5. Sobre o **Galeria de procura** página, pode pesquisar pelos seguintes campos:
   
   * Nome do Módulo
   * Etiquetas
   * Autor
   * Nome do cmdlet/DSC de recursos
6. Localize um módulo que está interessado e selecioná-lo para ver os detalhes.  
   Quando é fazer uma busca num módulo específico, pode ver mais informações sobre o módulo, incluindo uma ligação para a galeria do PowerShell, as necessárias, dependências e todos os cmdlets e/ou os recursos de DSC que contém o módulo.
   
    ![Detalhes de módulos do PowerShell](media/automation-runbook-gallery/gallery-item-details-blade.png) <br>
7. Para instalar o módulo diretamente na automatização do Azure, clique nas **importação** botão.
8. Se clicar no botão de importação, o **importar** painel, verá o nome do módulo que está prestes a importar. Se estiverem instaladas todas as dependências, o **OK** botão é ativado. Se perder as dependências, terá de importar os antes de pode importar este módulo.
9. Sobre o **importe** página, clique em **OK** para importar o módulo. Embora a automatização do Azure importa um módulo para a sua conta, extrai metadados sobre o módulo e os cmdlets. Esta operação poderá demorar alguns minutos, uma vez que cada atividade tem de ser extraídos.
10. Receberá uma notificação inicial que o módulo está a ser implementado e outra notificação se foi concluída.
11. Após a importação do módulo, pode ver as atividades disponíveis e pode usar seus recursos nos seus runbooks e de Desired State Configuration.

> [!NOTE]
> Módulos que só suportam o PowerShell core não são suportados na automatização do Azure e não conseguem ser importados no portal do Azure ou implementado diretamente a partir da galeria do PowerShell.

## <a name="python-runbooks"></a>Python Runbooks

Runbooks de Python estão disponíveis no [Galeria de centro de scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B1%5D.Type=ProgrammingLanguage&f%5B1%5D.Value=Python&f%5B1%5D.Text=Python&sortBy=Date&username=). Pode contribuir com runbooks de Python para a galeria do Script Center. Quando, certifique-se de que adicionar a marca **Python** ao carregar a sua contribuição.

## <a name="requesting-a-runbook-or-module"></a>Pedir um runbook ou o módulo
Pode enviar pedidos para [User Voice](https://feedback.azure.com/forums/246290-azure-automation/).  Se precisar de ajuda a escrever um runbook ou tem uma pergunta sobre o PowerShell, postar uma pergunta para nosso [fórum](https://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Próximos Passos
* Para começar com runbooks, consulte [criar ou importar um runbook na automatização do Azure](automation-creating-importing-runbook.md)
* Para compreender as diferenças entre o PowerShell e o fluxo de trabalho do PowerShell com os runbooks, consulte [fluxo de trabalho do PowerShell de aprendizagem](automation-powershell-workflow.md)



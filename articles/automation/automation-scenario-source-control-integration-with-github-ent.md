---
title: Integração de controlo de origem de automatização do Azure com o GitHub Enterprise
description: Descreve os detalhes de como configurar a integração com o GitHub Enterprise para o controlo de origem de runbooks de automatização.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 09/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d987e7d1e47e2e88c2f615e1d1bb5446c575ef47
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785203"
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-github-enterprise"></a>Cenário da automatização do Azure - integração de controlo de origem de automatização com o GitHub Enterprise

Atualmente, a automatização suporta integração de controlo de origem, que permite-lhe associar runbooks na conta de automatização para um repositório de controle de origem do GitHub. No entanto, os clientes que tenham implementado [GitHub Enterprise](https://enterprise.github.com/home) para suportar as suas práticas de DevOps, também desejam usá-lo para gerir o ciclo de vida de runbooks que são desenvolvidos para automatizar os processos de negócios e operações de gestão de serviço.

Neste cenário, tem um computador Windows no seu centro de dados configurado como uma função de trabalho de Runbook híbrida com os módulos do Azure Resource Manager e as ferramentas de Git instaladas. A máquina de trabalho híbrida tem um clone do repositório de Git local. Quando o runbook é executado na função de trabalho híbrida, o diretório de Git está sincronizado e o conteúdo do ficheiro de runbook é importado para a conta de automatização.

Este artigo descreve como configurar esta configuração no seu ambiente de automatização do Azure. Comece por configurar a automatização com as credenciais de segurança, runbooks, necessário para suportar este cenário e a implantação de uma função de trabalho de Runbook híbrida no seu centro de dados para executar os runbooks e aceder ao seu repositório do GitHub Enterprise para sincronizar runbooks com a sua conta de automatização.

## <a name="getting-the-scenario"></a>Obter o cenário

Neste cenário consiste em dois runbooks do PowerShell que pode ser importado diretamente a partir da [Galeria de Runbooks](automation-runbook-gallery.md) no portal do Azure ou transferência a partir do [galeria do PowerShell](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | Descrição|
--------|------------|
Export-RunAsCertificateToHybridWorker | Runbook exporta um certificado de RunAs a partir de uma conta de automatização para uma função de trabalho híbrida, de modo a que os runbooks na função de trabalho possa ser autenticado com o Azure para importar runbooks para a conta de automatização.|
Sync-LocalGitFolderToAutomationAccount | Runbook sincroniza a pasta de Git local no computador híbrida e, em seguida, importe os ficheiros do runbook (*. ps1) para a conta de automatização.|

### <a name="credentials"></a>Credenciais

Credencial | Descrição|
-----------|------------|
GitHRWCredential | Recurso de credencial cria para conter o nome de utilizador e palavra-passe para um utilizador com permissões para a função de trabalho híbrida.|

## <a name="installing-and-configuring-this-scenario"></a>Instalar e configurar este cenário

### <a name="prerequisites"></a>Pré-requisitos

1. O runbook de sincronização LocalGitFolderToAutomationAccount autentica utilizando o [conta Run As do Azure](automation-sec-configure-azure-runas-account.md).

2. Também é necessária uma área de trabalho do Log Analytics com a solução de automatização do Azure ativada e configurada. Se não tiver um que está associado com a conta de automatização utilizada para instalar e configurar este cenário, é criado e configurado para quando executa o **New-OnPremiseHybridWorker.ps1** script do runbook híbridas função de trabalho.

    > [!NOTE]
    > Atualmente as regiões seguintes apenas suportam a integração da automatização com o Log Analytics - **Sudeste da Austrália**, **E.U.A. Leste 2**, **Sudeste asiático**, e  **Europa Ocidental**.

3. Um computador que pode servir como um dedicado Runbook Worker híbrido que também aloja o software do GitHub e manter os ficheiros do runbook (*runbook*. ps1) num diretório de origem no sistema de ficheiros para sincronizar entre o GitHub e a automatização conta.

### <a name="import-and-publish-the-runbooks"></a>Importar e publicar os runbooks

Para importar o *Export-RunAsCertificateToHybridWorker* e *sincronização LocalGitFolderToAutomationAccount* runbooks a partir da Galeria de Runbooks da sua conta de automatização no portal do Azure, siga o procedimentos [importar Runbook da galeria do Runbook](automation-runbook-gallery.md#to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal). Publica os runbooks depois de terem sido importados com êxito para a sua conta de automatização.

### <a name="deploy-and-configure-hybrid-runbook-worker"></a>Implementar e configurar o Runbook Worker híbrido

Se não tiver uma função de trabalho de Runbook híbrida já implementado no seu centro de dados, deve rever os requisitos e siga os passos de instalação automatizada usando o procedimento no Azure Automation os Runbook Workers híbridos - automatizar a instalação e configuração para [Windows](automation-windows-hrw-install.md#automated-deployment) ou [Linux](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker). Depois de instalar com êxito a função de trabalho híbrida num computador, execute os seguintes passos para concluir a respetiva configuração para suportar este cenário.

1. Inicie sessão computador que aloja a função de trabalho de Runbook híbrida com uma conta que tenha direitos administrativos locais e criar um diretório para armazenar os ficheiros de runbook do Git. Clone o repositório de Git interno para o diretório.
1. Se ainda não tiver uma conta RunAs criada ou se pretende criar um novo um dedicado para esta finalidade, do portal do Azure navegue para as contas de automatização, selecione a sua conta de automatização e criar um [recurso de credencial](automation-credentials.md) que contém o nome de utilizador e palavra-passe para um utilizador com permissões para a função de trabalho híbrida.
1. Da sua conta de automatização [editar o runbook](automation-edit-textual-runbook.md)**Export-RunAsCertificateToHybridWorker** e modifique o valor para a variável *$Password* com uma palavra-passe segura.    Depois de modificar o valor, clique em **publicar** ter a versão de rascunho do runbook publicado.
1. Iniciar o runbook **Export-RunAsCertificateToHybridWorker**e, no **iniciar Runbook** painel, sob a opção **definições de execução** selecionar a opção  **Função de trabalho híbrida** e na lista pendente, selecione o grupo de trabalho híbrida que criou anteriormente para este cenário.

    Isto exporta um certificado para a função de trabalho híbrida, para que os runbooks em que a função de trabalho pode autenticar com o Azure com a ligação Run As para gerir recursos do Azure (em especial para este cenário - importar runbooks para a conta de automatização).

1. Na sua conta de automatização, selecione o grupo de trabalho híbrida criado anteriormente e [especifique uma conta RunAs](automation-hrw-run-runbooks.md#runas-account) para o grupo de trabalho híbridas e escolha o recurso de credencial apenas ou já tenha criado. Isso garante que o runbook de sincronização pode executar comandos do Git. 
1. Iniciar o runbook **sincronização LocalGitFolderToAutomationAccount**, forneça os seguintes valores de parâmetro de entrada de necessários e, no **iniciar Runbook** painel, sob a opção **definições de execução**  selecione a opção **função de trabalho híbrida** e na lista pendente, selecione o grupo de trabalho híbrida que criou anteriormente para este cenário:

   * *ResourceGroup* -o nome do seu grupo de recursos associados a sua conta de automatização
   * *AutomationAccountName* -o nome da sua conta de automatização
   * *GitPath* -a pasta local ou o ficheiro na função de trabalho de Runbook híbrida, em que o Git está configurado para extrair alterações mais recentes para

    Esta sincroniza a pasta de Git local no computador de trabalho híbrida e, em seguida, importa os ficheiros. ps1 do diretório de origem para a conta de automatização.

1. Ver detalhes de resumo de tarefa do runbook, selecionando-o a partir da **Runbooks** painel na sua conta de automatização e, em seguida, selecione a **tarefas** mosaico. Confirmar que foi concluída com êxito ao selecionar o **todos os registos** mosaico e rever o fluxo de registo detalhado.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre os tipos de runbook, as vantagens e limitações, consulte o artigo [Tipos de runbook da Automatização do Azure](automation-runbook-types.md)
* Para mais informações sobre a funcionalidade de suporte de scripts do PowerShell, consulte o artigo [Suporte de scripts do PowerShell Nativo na Automatização do Azure](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)

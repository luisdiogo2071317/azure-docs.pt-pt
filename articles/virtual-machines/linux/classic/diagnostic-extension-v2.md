---
title: Monitorização de uma VM do Linux com uma extensão VM | Documentos da Microsoft
description: Saiba como utilizar a extensão de diagnóstico do Linux para monitorizar o desempenho e dados de diagnóstico de uma VM do Linux no Azure.
services: virtual-machines-linux
author: NingKuang
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: f54a11c5-5a0e-40ff-af6c-e60bd464058b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: Ning
ms.openlocfilehash: f1415e2cfbe48b287db5851bb8ebef1ff9251280
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "32193057"
---
# <a name="use-the-linux-diagnostic-extension-to-monitor-the-performance-and-diagnostic-data-of-a-linux-vm"></a>Utilizar a Extensão de Diagnóstico do Linux para monitorizar os dados de desempenho e diagnóstico de uma VM do Linux

Este documento descreve a versão 2.3 da extensão de diagnóstico do Linux.

> [!IMPORTANT]
> Esta versão é preterida e pode ser anulada qualquer altura após 30 de Junho de 2018. Foi substituída pela versão 3.0. Para obter mais informações, consulte a [documentação para a versão 3.0 da extensão de diagnóstico do Linux](../diagnostic-extension.md).

## <a name="introduction"></a>Introdução

(**Nota**: A extensão de diagnóstico do Linux é aberto na [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) onde as informações mais atuais sobre a extensão são publicadas pela primeira vez. Talvez queira verificar o [página do GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) primeiro.)

A extensão de diagnóstico do Linux ajuda um monitor de utilizador as VMs do Linux em execução no Microsoft Azure. Ele tem as seguintes capacidades:

* Recolhe e carrega as informações de desempenho do sistema de VM do Linux a tabela de armazenamento do usuário, incluindo informações de diagnóstico e de syslog.
* Permite aos utilizadores personalizar as métricas de dados que serão recolhidas e carregadas.
* Permite aos utilizadores carregar ficheiros de registo especificado para uma tabela de armazenamento designada.

Na versão atual 2.3, os dados incluem:

* Todos os registos de Rsyslog do Linux, incluindo o sistema, segurança e registos de aplicações.
* Todos os dados de sistema que estão especificados na [o site de soluções de plataforma cruzada do System Center](https://scx.codeplex.com/wikipage?title=xplatproviders).
* Ficheiros de registo especificado pelo utilizador.

Esta extensão funciona com modelos de implementação do Resource Manager e clássica.

### <a name="current-version-of-the-extension-and-deprecation-of-old-versions"></a>Versão atual da extensão e descontinuação de versões antigas

É a versão mais recente da extensão **2.3**, e **quaisquer versões antigas (2.0, 2.1 e 2.2) serão preteridas e não publicadas pelo final do ano (2017)**. Se instalou a extensão de diagnóstico do Linux com a atualização de versão secundária automática desativada, é altamente recomendável que desinstale a extensão e reinstalá-lo com a atualização de versão secundária automática ativada. Em VMs do clássicas (ASM), pode conseguir isto ao especificar "2.*" como a versão, se estiver a instalar a extensão pela CLI XPLAT do Azure ou o Powershell. Em VMs do ARM, pode conseguir isto ao incluir ' "autoUpgradeMinorVersion": true' no modelo de implementação de VM. Além disso, qualquer nova instalação da extensão deve ter a versão secundária do automática atualizar opção ativada.

## <a name="enable-the-extension"></a>Ativar a extensão

Pode ativar esta extensão com o [portal do Azure](https://portal.azure.com/#), o Azure PowerShell ou scripts da CLI do Azure.

Para ver e configurar os dados de sistema e o desempenho diretamente a partir do portal do Azure, siga [estes passos no blogue do Azure](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).

Este artigo se concentra em como ativar e configurar a extensão ao utilizar comandos da CLI do Azure. Isto permite-lhe ler e ver os dados diretamente a partir da tabela de armazenamento.

Tenha em atenção que os métodos de configuração que são descritos aqui não funcionará no portal do Azure. Para ver e configurar os dados de sistema e o desempenho diretamente a partir do portal do Azure, a extensão tem de estar ativada através do portal.

## <a name="prerequisites"></a>Pré-requisitos

* **O agente Linux do Azure versão 2.0.6 ou posterior**.

  Tenha em atenção que a maioria das imagens da galeria do Azure VM Linux incluem a versão 2.0.6 ou posterior. Pode executar **WAAgent-versão** para confirmar a versão está instalada na VM. Se a VM estiver a executar uma versão anterior ao 2.0.6, pode seguir [estas instruções no GitHub](https://github.com/Azure/WALinuxAgent "instruções") atualizá-la.
* **CLI do Azure**. Siga [nesta orientação para instalar a CLI](../../../cli-install-nodejs.md) para configurar o ambiente da CLI do Azure no seu computador. Após a instalação da CLI do Azure, pode utilizar o **azure** comando a partir da sua interface de linha de comandos (Bash, Terminal ou linha de comandos) para acessar os comandos da CLI do Azure. Por exemplo:

  * Execute **conjunto de extensão de vm do azure – ajuda** para informações de ajuda detalhadas.
  * Execute **início de sessão do azure** para iniciar sessão no Azure.
  * Execute **lista de VMS do azure** para listar todas as máquinas virtuais que têm no Azure.
* Uma conta de armazenamento para armazenar os dados. Será necessário um nome de conta de armazenamento que foi criado anteriormente e uma chave de acesso para carregar os dados para o seu armazenamento.

## <a name="use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension"></a>Utilize o comando da CLI do Azure para ativar a extensão de diagnóstico do Linux

### <a name="scenario-1-enable-the-extension-with-the-default-data-set"></a>Cenário 1. Ativar a extensão com o conjunto de dados padrão

Na versão 2.3 ou posterior, os dados de predefinição que serão recolhidos incluem:

* Todas as informações de Rsyslog (incluindo o sistema, segurança e registos de aplicações).  
* Um conjunto de principais de dados do sistema de base. Tenha em atenção que o conjunto de dados completo é descrito no [site de soluções de plataforma cruzada do System Center](https://scx.codeplex.com/wikipage?title=xplatproviders).
  Se pretender ativar a dados Extras, continue com os passos em cenários de 2 e 3.

Passo 1. Crie um ficheiro denominado PrivateConfig.json com o seguinte conteúdo:

    {
        "storageAccountName" : "the storage account to receive data",
        "storageAccountKey" : "the key of the account"
    }

Passo 2. Executar **vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2 de definir a extensão de vm do azure.* – o caminho de configuração privada PrivateConfig.json**.

### <a name="scenario-2-customize-the-performance-monitor-metrics"></a>Cenário 2. Personalizar as métricas de monitor de desempenho

Esta secção descreve como personalizar o desempenho e a tabela de dados de diagnóstico.

Passo 1. Crie um ficheiro denominado PrivateConfig.json com o conteúdo que foi descrito no cenário 1. Também crie um arquivo chamado PublicConfig.json. Especifique os dados que pretende recolher.

Para fornecedores todos com suporte e as variáveis, consulte a [site de soluções de plataforma cruzada do System Center](https://scx.codeplex.com/wikipage?title=xplatproviders). Pode ter várias consultas e armazená-las em várias tabelas ao acrescentar mais consultas para o script.

Por predefinição, os dados de Rsyslog sempre são recolhidos.

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


Passo 2. Execute **extensão de vm do azure definir vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*"– o caminho de configuração privada PrivateConfig.json – o caminho de configuração público PublicConfig.json**.

### <a name="scenario-3-upload-your-own-log-files"></a>Cenário 3. Carregar seus próprios ficheiros de registo

Esta secção descreve como recolher e carregar ficheiros de registo específicos para a conta de armazenamento. Tem de especificar os dois o caminho para o seu ficheiro de registo e o nome da tabela onde pretende armazenar o início de sessão. Pode criar vários arquivos de log, adicionando várias entradas de tabela/ficheiro para o script.

Passo 1. Crie um ficheiro denominado PrivateConfig.json com o conteúdo que foi descrito no cenário 1. Em seguida, crie outro arquivo, chamado PublicConfig.json com o seguinte conteúdo:

```json
{
    "fileCfg" :
    [
        {
            "file" : "/var/log/mysql.err",
            "table" : "mysqlerr"
            }
    ]
}
```

Passo 2. Execute `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json`.

Tenha em atenção que, com esta definição nas versões de extensão antes de 2.3, todos os registos escritos `/var/log/mysql.err` podem ser duplicadas para `/var/log/syslog` (ou `/var/log/messages` consoante a distribuição de Linux) também. Se gostaria de evitar este registo duplicado, pode excluir o registo de `local6` registos de instalação na configuração rsyslog. Depende da distribuição de Linux, mas num sistema de Ubuntu 14.04, é o ficheiro para modificar `/etc/rsyslog.d/50-default.conf` e pode substituir a linha `*.*;auth,authpriv.none -/var/log/syslog` para `*.*;auth,authpriv,local6.none -/var/log/syslog`. Este problema ser corrigido na versão de correções mais recentes do 2.3 (2.3.9007), portanto, se tiver a versão 2.3 da extensão, não deve ocorrer este problema. Se ainda existir até mesmo depois de reiniciar a VM, contacte-nos e ajude-na resolver o motivo pelo qual a versão mais recente de correção não é instalada automaticamente.

### <a name="scenario-4-stop-the-extension-from-collecting-any-logs"></a>Cenário de 4. Parar a extensão de recolher quaisquer registos

Esta secção descreve como interromper a extensão de recolha de registos. Tenha em atenção que o processo de agente de monitorização irá estar ainda em execução, mesmo com essa reconfiguração. Se quiser parar totalmente o processo de agente de monitorização, pode fazê-lo ao desativar a extensão. O comando para desativar a extensão é `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.

Passo 1. Crie um ficheiro denominado PrivateConfig.json com o conteúdo que foi descrito no cenário 1. Crie outro arquivo, chamado PublicConfig.json com o seguinte conteúdo:

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


Passo 2. Execute **extensão de vm do azure definir vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*"– o caminho de configuração privada PrivateConfig.json – o caminho de configuração público PublicConfig.json**.

## <a name="review-your-data"></a>Reveja os seus dados

O desempenho e dados de diagnóstico são armazenados numa tabela do armazenamento do Azure. Revisão [como utilizar o Azure Table Storage do Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) para aprender a aceder aos dados na tabela de armazenamento ao utilizar scripts de CLI do Azure.

Além disso, pode utilizar seguindo as ferramentas de interface do Usuário para acessar os dados:

1. Explorador de servidores do Visual Studio. Aceda à sua conta de armazenamento. Depois da VM seja executada durante cerca de cinco minutos, verá as tabelas de quatro padrão: "LinuxCpu", "LinuxDisk", "LinuxMemory" e "Linuxsyslog". Faça duplo clique os nomes de tabela para ver os dados.
1. [Explorador de armazenamento do Azure](https://azurestorageexplorer.codeplex.com/ "Explorador de armazenamento do Azure").

![image](./media/diagnostic-extension/no1.png)

Se tiver habilitado fileCfg ou perfCfg (conforme descrito em cenários de 2 e 3), pode utilizar o Explorador de servidores do Visual Studio e o Explorador de armazenamento do Azure para ver dados não padrão.

## <a name="known-issues"></a>Problemas conhecidos

* As informações de Rsyslog e o ficheiro de registo especificada do cliente apenas podem ser acedidos através de scripts.

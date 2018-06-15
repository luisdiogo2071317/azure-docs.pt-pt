---
title: Ficheiro integridade monitorização no Centro de segurança do Azure (pré-visualização) | Microsoft Docs
description: " Saiba como ativar a monitorização da integridade dos ficheiros no Centro de segurança do Azure. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2018
ms.author: terrylan
ms.openlocfilehash: 722a4fd11f35f04ed22d73638f07d15c49ea3c26
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34162177"
---
# <a name="file-integrity-monitoring-in-azure-security-center-preview"></a>Ficheiro integridade monitorização no Centro de segurança do Azure (pré-visualização)
Saiba como configurar a monitorização de integridade de ficheiro (FIM) no Centro de segurança do Azure a utilizar estas instruções.

## <a name="what-is-fim-in-security-center"></a>O que é FIM no Centro de segurança?
Ficheiro integridade monitorização (FIM), também conhecido como alteração de monitorização, examina ficheiros e os registos do sistema operativo, o software de aplicação e outras alterações que possam indicar um ataque. Um método de comparação é utilizado para determinar se o estado atual do ficheiro é diferente da última análise do ficheiro. Pode tirar partido desta comparação para determinar se tiverem sido efetuadas modificações de válido ou suspeitas aos seus ficheiros.

Monitorização de integridade de ficheiro do Centro de segurança valida a integridade dos ficheiros do Windows, o registo do Windows e Linux ficheiros. Selecione os ficheiros que pretende que sejam monitorizados, permitindo FIM. Centro de segurança monitoriza ficheiros com o FIM ativada para a atividade, tal como:

- Criação e registo de ficheiros e de remoção
- Modificações do ficheiro (alterações ao tamanho do ficheiro, listas de controlo de acesso e o hash do conteúdo)
- Modificações de registo (alterações no tamanho, acesso conrol listas, tipo e o conteúdo)

Centro de segurança recomenda entidades para monitorizar, que pode ativar facilmente FIM no. Também pode definir os seus próprios políticas FIM ou entidades para monitorizar. Estas instruções mostram como.

> [!NOTE]
> A funcionalidade de monitorização de integridade de ficheiro (FIM) funciona para computadores Windows e Linux e VMs e está disponível na camada padrão do Centro de segurança. Veja [Preços](security-center-pricing.md) para saber mais sobre os escalões de preços do Centro de Segurança.
FIM carrega dados para a área de trabalho de análise de registos. Dados são aplicáveis encargos, com base na quantidade de dados que carrega. Consulte [preços de análise de registos](https://azure.microsoft.com/pricing/details/log-analytics/) para obter mais informações.
>
>

> [!NOTE]
> FIM utiliza a solução de controlo de alterações do Azure para controlar e identificar as alterações no seu ambiente. Quando a monitorização da integridade dos ficheiros estiver ativada, tem um **alterações** recurso do tipo de solução. Se remover o **alterações** recurso, desativar a integridade do ficheiro de funcionalidade no Centro de segurança de monitorização.
>
>

## <a name="which-files-should-i-monitor"></a>Os ficheiros que deverá a monitorizar
Deve considerar sobre os ficheiros que são críticos para o sistema e as aplicações quando escolher os ficheiros para monitorizar. Considere a escolha de ficheiros que não pretende alterar sem planeamento. Escolher entre ficheiros que são alterados frequentemente por aplicações ou sistema operativo (tal como ficheiros de registo e ficheiros de texto) criam muito ruído que torna difícil identificar um ataque.

Recomenda de centro de segurança que os ficheiros deve monitorizar por predefinição, de acordo com padrões de ataques conhecidos que incluem ficheiros e registo de alterações.

## <a name="using-file-integrity-monitoring"></a>Utilizando o ficheiro de monitorização de integridade
1. Abra o dashboard **Centro de Segurança**.
2. No painel esquerdo em **avançadas defesa de nuvem**, selecione **monitorização da integridade dos ficheiros**.
![Dashboard do Centro de segurança][1]

**Monitorização de integridade de ficheiro** abre.
  ![Dashboard do Centro de segurança][2]

As seguintes informações são fornecidas para cada área de trabalho:

- Número total de alterações que ocorreram na última semana (poderá ver um traço "-" se FIM não está ativado na área de trabalho)
- Número total de computadores e VMs do Reporting Services para a área de trabalho
- Localização geográfica da área de trabalho
- Subscrição do Azure que se encontra sob a área de trabalho

Também poderão ser apresentados os seguintes botões para uma área de trabalho:

- ![Ativar o ícone][3] Indica que o FIM não está ativado para a área de trabalho. Selecionar a área de trabalho permite-lhe ativar FIM em todas as máquinas na área de trabalho.
- ![Ícone de plano de atualização][4] indica que a área de trabalho ou a subscrição não está em execução no escalão Standard do Centro de segurança. Para utilizar a funcionalidade FIM, a subscrição tem de estar a executar padrão.  Selecionar a área de trabalho permite-lhe atualizar para o padrão. Para saber mais sobre o escalão Standard e como atualizar, consulte [atualizar para o escalão Standard do Centro de segurança para a segurança avançada](security-center-pricing.md).
- Um em branco (não há nenhum botão) significa que o FIM já está ativado na área de trabalho.

Em **monitorização da integridade dos ficheiros**, pode selecionar uma área de trabalho para ativar o FIM para essa área de trabalho, ver o dashboard de monitorização da integridade dos ficheiros de mensagens em fila para essa área de trabalho, ou [atualizar](security-center-pricing.md) a área de trabalho para Standard.

## <a name="enable-fim"></a>Ativar FIM
Para ativar o FIM na área de trabalho:

1. Em **monitorização da integridade dos ficheiros**, selecione uma área de trabalho com o **ativar** botão.
2. **Ativar a monitorização da integridade dos ficheiros** abre-se de que apresenta o número de máquinas Windows e Linux, na área de trabalho.

   ![Ativar a monitorização da integridade dos ficheiros][5]

   As definições recomendadas para o Windows e Linux são também apresentadas.  Expanda **ficheiros do Windows**, **registo**, e **Linux ficheiros** para ver a lista completa de itens recomendados.

3. Desmarque qualquer entidades recomendadas que não pretende aplicar FIM para.
4. Selecione **aplicam-se de que a monitorização da integridade dos ficheiros** para ativar o FIM.

> [!NOTE]
> Pode alterar as definições em qualquer altura. Consulte [editar monitorizado entidades](security-center-file-integrity-monitoring.md#edit-monitored-items) abaixo para obter mais informações.
>
>

## <a name="view-the-fim-dashboard"></a>Ver o dashboard FIM
O **monitorização da integridade dos ficheiros** dashboard apresenta para áreas de trabalho onde o FIM está ativado. O dashboard FIM é aberto depois de ativar a FIM de uma área de trabalho ou quando seleciona uma área de trabalho a **monitorização da integridade dos ficheiros** janela que já tenha FIM ativada.

![Dashboard de monitorização de integridade de ficheiro][6]

O dashboard FIM para uma área de trabalho apresenta o seguinte:

- Número total de máquinas ligado à área de trabalho
- Número total de alterações que ocorreram durante o período de tempo selecionado
- Uma análise detalhada do tipo de alteração (ficheiros, registo)
- Uma repartição da categoria da alteração (modificado adicionadas, removidas)

Selecionar o filtro na parte superior do dashboard permite-lhe aplicar o período de tempo que pretende ver as alterações para.

![Filtro de período de tempo][7]

O **computadores** separador (mostrado acima) apresenta uma lista de todos os computadores que relatam a esta área de trabalho. Para cada máquina, o dashboard apresenta:

- Totais de alterações que ocorreram durante o período de tempo selecionado
- Uma análise detalhada das alterações totais como as alterações ao ficheiro ou alterações de registo

**Registo de pesquisa** é aberta quando introduzir um nome de computador na pesquisa campo ou selecione uma máquina listada no separador computadores. Pesquisa de registo apresenta todas as alterações efetuadas durante o período de tempo selecionado para a máquina. Pode expandir uma alteração para obter mais informações.

![Pesquisa de Registos][8]

O **alterações** separador (mostrado abaixo) apresenta uma lista de todas as alterações para a área de trabalho durante o período de tempo selecionado. Para cada entidade que foi alterada, as listas de dashboard de:

- Computador que a alteração ocorreu
- Tipo de alteração (ficheiro ou de registo)
- Categoria da alteração (modificado adicionadas, removidas)
- Data e hora da alteração

![Alterações para a área de trabalho][9]

**Alterar detalhes** abre-se ao introduzir uma alteração na pesquisa campo ou selecione uma entidade listada no **alterações** separador.

![Detalhes de alteração][10]

## <a name="edit-monitored-entities"></a>Editar monitorizado entidades

1. Volte à **dashboard de monitorização da integridade dos ficheiros** e selecione **definições**.

  ![Definições][11]

  **Configuração de área de trabalho** abre-se de que apresenta os três separadores: **registo do Windows**, **ficheiros do Windows**, e **Linux ficheiros**. Cada separador lista as entidades que possa editar dessa categoria. Para cada entidade listada, Centro de segurança identifica se o FIM está ativado (true) ou não ativada (false).  Editar a entidade permite-lhe ativar ou desativar FIM.

  ![Configuração de área de trabalho][12]

2. Selecione um identityprotection. Neste exemplo, selecionamos um item de registo do Windows. **Editar para controlo de alterações** abre.

  ![Editar ou registo de alterações][13]

Em **Editar para controlo de alterações** , pode:

- Ativar (True) ou desativar a monitorização de integridade de ficheiro (FALSO)
- Fornecer ou altere o nome da entidade
- Fornecer ou altere o valor ou o caminho
- Eliminar a entidade, eliminar a alteração ou guardar a alteração

## <a name="add-a-new-entity-to-monitor"></a>Adicionar uma nova entidade para monitorizar
1. Volte à **integirty ficheiro dashboard de monitorização** e selecione **definições** na parte superior. **Configuração de área de trabalho** abre.
2. Em **configuração de área de trabalho**, selecione o separador para o tipo de entidade que pretende adicionar: registo do Windows, os ficheiros do Windows ou Linux ficheiros. Neste exemplo, selecionamos **Linux ficheiros**.

  ![Adicionar um novo item para monitorizar][14]

3. Selecione **Adicionar**. **Adicionar controlo de alterações** abre.

  ![Introduza as informações pedidas][15]

4. No **adicionar** página, introduza as informações pedidas e selecione **guardar**.

## <a name="disable-monitored-entities"></a>Desativar monitorizado entidades
1. Volte à **monitorização da integridade dos ficheiros** dashboard.
2. Selecione uma área de trabalho onde FIM está atualmente ativado. Uma área de trabalho está ativada para FIM se não tiver o botão de ativar ou o botão de planear a atualização.

  ![Selecione uma área de trabalho onde o FIM está ativado][16]

3. Sob monitorização de integridade de ficheiro, selecione **definições**.

  ![Selecione as definições][17]

4. Em **configuração de área de trabalho**, selecione um grupo de onde **ativado** está definido como true.

  ![Configuração da Área de Trabalho][18]

5. Em **Editar para controlo de alterações** janela conjunto **ativado** como False.

  ![Ativado definido como falso][19]

6. Selecione **Guardar**.

## <a name="disable-fim"></a>Desativar FIM
Pode desativar FIM. FIM utiliza a solução de controlo de alterações do Azure para controlar e identificar as alterações no seu ambiente. Através da desativação de FIM, remova a solução de controlo de alterações da área de trabalho selecionada.

1. Para desativar o FIM, voltar para o **monitorização da integridade dos ficheiros** dashboard.
2. Selecione uma área de trabalho.
3. Em **monitorização da integridade dos ficheiros**, selecione **desativar**.

  ![Desativar FIM][20]

4. Selecione **remover** para desativar.

## <a name="next-steps"></a>Passos Seguintes
Este artigo aprendeu a utilizar a monitorização de integridade de ficheiro (FIM) no Centro de segurança. Para saber mais acerca do Centro de Segurança, consulte o seguinte:

* [Definir políticas de segurança](security-center-policies.md) – Saiba como configurar políticas de segurança para as subscrições do Azure e os grupos de recursos.
* [Gerir recomendações de segurança](security-center-recommendations.md) – Saiba como recomendações ajudam a proteger os seus recursos do Azure.
* [Monitorização de estado de funcionamento de segurança](security-center-monitoring.md)– Saiba como monitorizar o estado de funcionamento dos seus recursos Azure.
* [Gerir e responder a alertas de segurança](security-center-managing-and-responding-alerts.md)– Saiba como gerir e responder a alertas de segurança.
* [Monitorizar soluções de parceiros](security-center-partner-solutions.md) – Saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
* [FAQ do Centro de segurança](security-center-faq.md)– encontre as perguntas mais frequentes sobre como utilizar o serviço.
* [Blogue de Segurança do Azure](http://blogs.msdn.com/b/azuresecurity/) – Obtenha as notícias e informações mais recentes de segurança do Azure.

<!--Image references-->
[1]: ./media/security-center-file-integrity-monitoring/security-center-dashboard.png
[2]: ./media/security-center-file-integrity-monitoring/file-integrity-monitoring.png
[3]: ./media/security-center-file-integrity-monitoring/enable.png
[4]: ./media/security-center-file-integrity-monitoring/upgrade-plan.png
[5]: ./media/security-center-file-integrity-monitoring/enable-fim.png
[6]: ./media/security-center-file-integrity-monitoring/fim-dashboard.png
[7]: ./media/security-center-file-integrity-monitoring/filter.png
[8]: ./media/security-center-file-integrity-monitoring/log-search.png
[9]: ./media/security-center-file-integrity-monitoring/changes-tab.png
[10]: ./media/security-center-file-integrity-monitoring/change-details.png
[11]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings.png
[12]: ./media/security-center-file-integrity-monitoring/workspace-config.png
[13]: ./media/security-center-file-integrity-monitoring/edit.png
[14]: ./media/security-center-file-integrity-monitoring/add.png
[15]: ./media/security-center-file-integrity-monitoring/add-item.png
[16]: ./media/security-center-file-integrity-monitoring/fim-dashboard-disable.png
[17]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings-disabled.png
[18]: ./media/security-center-file-integrity-monitoring/workspace-config-disable.png
[19]: ./media/security-center-file-integrity-monitoring/edit-disable.png
[20]: ./media/security-center-file-integrity-monitoring/disable-fim.png

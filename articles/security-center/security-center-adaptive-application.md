---
title: Controlos de aplicações adaptativos no Centro de Segurança do Azure | Microsoft Docs
description: Este documento ajuda-o a utilizar os controlos de aplicações adaptativos no Centro de Segurança do Azure para adicionar as aplicações em execução em VMs do Azure à lista de permissões.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2018
ms.author: terrylan
ms.openlocfilehash: fa2f3c10687a02c5d0be8d7bb0ae88b2b0c38e19
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989970"
---
# <a name="adaptive-application-controls-in-azure-security-center-preview"></a>Controlos de aplicações adaptativos no Centro de Segurança do Azure (Pré-visualização)
Saiba como configurar o controlo de aplicações no Centro de Segurança do Azure com estas instruções.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>O que são os controlos de aplicações adaptativos do Centro de Segurança?
Os controlos de aplicações adaptativos ajudam a controlar que aplicações podem ser executadas nas suas VMs localizadas no Azure, o que, entre outros benefícios, permite proteger as VMs contra malware. O Centro de Segurança utiliza machine learning para analisar as aplicações que estão a ser executadas na VM e ajuda-o a aplicar regras de inclusão nas listas de permissões com base nessas informações. Esta capacidade simplifica grandemente o processo de configuração e manutenção de listas de permissões de aplicações, permitindo-lhe:

- Bloquear ou alertar relativamente a tentativas de execução de aplicações maliciosas, incluindo aplicações que possam não ser detetadas pelas soluções de antimalware.
- Estar em conformidade com a política de segurança da sua organização que estabelece que só pode ser utilizado software licenciado.
- Evitar a utilização de software não desejado no seu ambiente.
- Evitar a execução de aplicações antigas e não suportadas.
- Impedir ferramentas de software específicas que não são permitidas na sua organização.
- Permitir que a equipa de TI controle o acesso a dados confidenciais através da utilização de aplicações.

## <a name="how-to-enable-adaptive-application-controls"></a>Como ativar os controlos de aplicações adaptáveis?
Os controlos de aplicações adaptáveis ajudam-no a definir um conjunto de aplicações cuja execução é permitida em grupos de recursos configurados. Esta funcionalidade só está disponível para máquinas Windows (todas as versões, clássica ou Azure Resource Manager). Os passos seguintes podem ser utilizados para configurar a inclusão de aplicações nas listas de permissões do Centro de Segurança:

1. Abra o dashboard **Centro de Segurança**.
2. No painel esquerdo, selecione **Controlos de aplicação adaptáveis** localizados em **Defesa da cloud avançada**.

    ![Defesa](./media/security-center-adaptive-application/security-center-adaptive-application-fig1-new.png)

É apresentada a página **Controlos de aplicação adaptável**.

![controlos](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

A secção **Grupos de VMs** contém três separadores:

* **Configurados**: lista de grupos que contêm as VMs que foram configuradas com o controlo de aplicações.
* **Recomendados**: lista de grupos para os quais o controlo de aplicações é recomendado. O Centro de Segurança utiliza machine learning para identificar as VMs que são boas candidatas para o controlo de aplicações com base no facto de executarem consistentemente as mesmas aplicações.
* **Nenhuma recomendação**: lista de grupos que contêm as VMs para as quais não existem recomendações de controlo de aplicações. Por exemplo, VMs em que as aplicações estão sempre a ser alteradas e que não atingiram um estado estável.

> [!NOTE]
> O Centro de Segurança utiliza um algoritmo proprietário de clustering para criar grupos de VMs, garantindo que as VMs semelhantes têm uma política de controlo de aplicações recomendada ideal.
>
>

### <a name="configure-a-new-application-control-policy"></a>Configurar uma política de controlo de aplicações nova
1. Clique no separador **Recomendados** para ver uma lista dos grupos com recomendações de controlo de aplicações:

  ![Recomendado](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

  A lista inclui:

  - **NOME**: o nome do grupo e da subscrição
  - **VMs**: o número de máquinas virtuais no grupo
  - **ESTADO**: o estado das recomendações, que, na maioria dos casos, será aberto
  - **GRAVIDADE**: o nível de gravidade das recomendações

2. Selecione um grupo para abrir a opção **Criar regras de controlo de aplicações**.

  ![Regras de controlo de aplicações](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

3. Em **Selecionar VMs**, reveja a lista de VMs recomendadas e desmarque aquelas às quais não pretende aplicar o controlo de aplicações. Em seguida, verá duas listas:

  - **Aplicações recomendadas**: uma lista de aplicações que são frequentes nas VMs neste grupo e, por conseguinte, recomendadas para as regras de controlo de aplicações pelo Centro de Segurança.
  - **Mais aplicações**: uma lista de aplicações que são menos frequentes nas VMs neste grupo ou que são conhecidas como Exploráveis (mais informações abaixo) e recomendadas para revisão antes de se aplicarem as regras.

4. Reveja as aplicações em cada lista e desmarque as que não pretende aplicar. Cada lista inclui:

  - **NOME**: as informações de certificado de uma aplicação ou o respetivo caminho de aplicação completo
  - **TIPOS DE FICHEIRO**: o tipo de ficheiro da aplicação. Pode ser EXE, Script ou MSI.
  - **EXPLORÁVEIS**: um ícone de aviso indica se as aplicações podem ser utilizadas por um atacante para ignorar as listas de permissões de aplicações. Recomenda-se que reveja estas aplicações antes da respetiva aprovação.
  - **UTILIZADORES**: os utilizadores que são recomendados para permissão de execução de uma aplicação

5. Depois de concluir as suas seleções, selecione **Criar**.

Por predefinição, o Centro de Segurança ativa sempre o controlo de aplicações no modo *Auditoria*. Depois de confirmar que a lista de permissões não tem nenhum efeito adverso na sua carga de trabalho, pode alterar para o modo *Imposição*.

O Centro de Segurança precisa de um mínimo de duas semanas de dados para criar uma linha de base e preencher as recomendações exclusivas por grupo de VMs. Os novos clientes do escalão standard do Centro de Segurança devem esperar um comportamento em que, primeiro, os grupos de VMs são apresentados no separador *nenhuma recomendação*.

> [!NOTE]
> Como melhor prática de segurança, o Centro de Segurança tentará sempre criar uma regra de publicador para as aplicações que devem ser adicionadas à lista de permissões e, a menos que as aplicações não tenham informações do publicador (ou seja, não sejam assinadas), é criada uma regra de caminho para o caminho completo do EXE específico.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Editar e monitorizar grupos configurados com o controlo de aplicações

1. Para editar e monitorizar um grupo configurado com controlo de aplicação, volte para a página **Controlos de aplicação adaptáveis** e selecione **CONFIGURADO** em **Grupos de VMs**:

  ![Grupos](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

  A lista inclui:

  - **NOME**: o nome do grupo e da subscrição
  - **VMs**: o número de máquinas virtuais no grupo
  - **MODO**: o modo de auditoria vai registar as tentativas de executar aplicações que não estão na lista de permissões; "Impor" não vai permitir a execução de aplicações não presentes na lista de permissões
  - **PROBLEMAS**: quaisquer violações atuais

2. Selecione um grupo ao qual fazer alterações na página **Editar política de controlo de aplicações**.

  ![Proteção](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

3. No **Modo de proteção**, tem a possibilidade de selecionar entre o seguinte:

  - **Auditoria**: neste modo, a solução de controlo de aplicações não impõe as regras e realiza apenas a auditoria à atividade nas VMs protegidas. Esta opção é recomendada para cenários nos quais pretende primeiro observar o comportamento geral antes de bloquear uma aplicação para ser executada na VM de destino.
  - **Impor**: neste modo, a solução de controlo de aplicações impõe as regras e certifica-se de que as aplicações que não têm autorização para ser executadas são bloqueadas.

  Conforme mencionado anteriormente, por predefinição, é sempre configurada uma política de controlo de aplicações nova no modo *Auditoria*. Em **Extensão da política**, pode adicionar os seus próprios caminhos de aplicações que pretende incluir na lista de permissões. Depois de adicionar estes caminhos, o Centro de Segurança cria as regras adequadas para estas aplicações, para além das regras que já estão em vigor.

  Na secção **Problemas Recentes**, são listadas todas as violações atuais.

  ![Problemas](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

  A lista inclui:
  - **PROBLEMAS**: todas as violações que foram registadas, que podem incluir o seguinte:

      - **ViolationsBlocked**: quando a solução está ativada no modo Imposição e tenta executar uma aplicação que não se encontra na lista de permissões.
      - **ViolationsAudited**: quando a solução está ativada no modo Auditoria e é executada uma aplicação que não está na lista de permissões.

 - **N.º DE VMS**: o número de máquinas virtuais com este tipo de problema.

  Se clicar em cada linha, é redirecionado para a página [Registo de Atividades do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), onde pode ver informações sobre todas as VMs com este tipo de violação. Se clicar nos três pontos no final de cada linha, pode eliminar essa entrada específica. A secção **Máquinas virtuais configuradas** mostra as VMs às quais estas regras se aplicam.

  ![Máquinas virtuais configuradas](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

  Em **Regras de permissão do publicador**, a lista contém:

  - **REGRA**: aplicações para as quais uma regra de publicador foi criada com base nas informações de certificado que foram encontradas para cada aplicação
  - **TIPO DE FICHEIRO**: os tipos de ficheiro que são abrangidos por uma regra de editor específica. Pode ser qualquer um dos seguintes: EXE, Script ou MSI.
  - **UTILIZADORES**: número de utilizadores com permissão para executar cada aplicação

  Veja [Understanding Publisher Rules in Applocker](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker) (Compreender as Regras de Publicador no Applocker) para obter mais informações.

  ![Regras de inclusão em listas de permissões](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

  Se clicar nos três pontos no final de cada linha, pode eliminar essa regra específica ou editar os utilizadores permitidos.

  A secção **Regras de inclusão em listas de permissões de caminho** mostram o caminho completo de aplicação (incluindo o tipo de ficheiro específico) das aplicações que não estão assinadas com um certificado digital, mas ainda são atuais nas regras de inclusão.

  > [!NOTE]
  > Por predefinição, como melhor prática de segurança, o Centro de Segurança tentará sempre criar uma regra de publicador para os EXEs que devem ser adicionados à lista de permissões e, a menos que os EXEs não tenham informações do publicador (ou seja, não sejam assinados), é criada uma regra de caminho para o caminho completo do EXE específico.

  ![Regras de inclusão em listas de permissões de caminho](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

  A lista contém:
  - **NOME**: o caminho completo do executável
  - **TIPO DE FICHEIRO**: os tipos de ficheiro que são abrangidos por uma regra de caminho específica. Pode ser qualquer um dos seguintes: EXE, Script ou MSI.
  - **UTILIZADORES**: número de utilizadores com permissão para executar cada aplicação

  Se clicar nos três pontos no final de cada linha, pode eliminar essa regra específica ou editar os utilizadores permitidos.

4. Depois de realizar alterações na página **Controlos de aplicação adaptáveis**, clique no botão **Guardar**. Se optar por não aplicar as alterações, clique em **Eliminar**.

### <a name="not-recommended-list"></a>Lista de não recomendadas

O Centro de Segurança só recomenda a inclusão de aplicações em listas de permissões para máquinas virtuais que executem um conjunto de aplicações estável. Não são criadas recomendações se as aplicações nas VMs associadas forem constantemente alteradas.

![Recomendação](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

A lista contém:
- **NOME**: o nome do grupo e da subscrição
- **VMs**: o número de máquinas virtuais no grupo

## <a name="next-steps"></a>Passos seguintes
Neste documento, aprendeu a utilizar os controlos de aplicações adaptativos no Centro de Segurança do Azure para adicionar as aplicações em execução em VMs do Azure à lista de permissões. Para saber mais acerca do Centro de Segurança do Azure, consulte o seguinte:

* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Saiba como gerir alertas e responder a incidentes de segurança no Centro de Segurança.
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md). Saiba como monitorizar o estado de funcionamento dos recursos do Azure.
* [Compreender os alertas de segurança no Centro de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Saiba mais sobre os diferentes tipos de alertas de segurança.
* [Guia de Resolução de Problemas do Centro de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Saiba como resolver problemas comuns no Centro de Segurança.
* [Centro de Segurança do Azure FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md). Encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Blogue de Segurança do Azure](http://blogs.msdn.com/b/azuresecurity/). Encontre mensagens do blogue acerca da segurança e conformidade do Azure.

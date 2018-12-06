---
title: Recomendações do Centro de segurança do Azure de utilização para melhorar a segurança | Documentos da Microsoft
description: " Saiba como utilizar políticas de segurança e recomendações no Centro de segurança do Azure para ajudar a mitigar um ataque de segurança. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: rkarlin
ms.openlocfilehash: 36e3e8c6c34099149cb13aa60efb6668fcc7d53c
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52958419"
---
# <a name="use-azure-security-center-recommendations-to-enhance-security"></a>Recomendações do Centro de segurança do Azure de utilização para melhorar a segurança
Pode reduzir as chances de um evento de segurança significativos ao configurar uma política de segurança e, em seguida, implementar as recomendações fornecidas pelo centro de segurança do Azure. Este artigo mostra-lhe como utilizar políticas de segurança e recomendações no Centro de segurança para ajudar a mitigar um ataque de segurança.

> [!NOTE]
> Este artigo baseia-se as funções e os conceitos apresentados no Centro de segurança [guia de operações e planeamento](security-center-planning-and-operations-guide.md). É uma boa idéia para rever o guia de planeamento antes de continuar.
>
>

## <a name="managing-security-recommendations"></a>Gerir recomendações de segurança
Uma política de segurança define o conjunto de controlos que são recomendados para recursos dentro da subscrição especificada ou o grupo de recursos. No Centro de segurança, define as políticas de acordo com os requisitos de segurança da sua empresa. Para obter mais informações, consulte [definir políticas de segurança no Centro de segurança](security-center-azure-policy.md).

Políticas de segurança para grupos de recursos são herdadas do nível da subscrição.

![Herança de diretiva de segurança][1]

Se precisar de políticas personalizadas em grupos de recursos específico, pode desativar a herança no grupo de recursos. Para desativar, herança para recursos exclusivos no painel de política de segurança e personalizar os controlos que o Centro de segurança mostra as recomendações para.

Por exemplo, se tiver cargas de trabalho que não exigem a política de SQL da base de dados transparente encriptação de dados (TDE), desative a política ao nível da subscrição e ativá-la apenas nos grupos de recursos onde a SQL TDE é necessária.

> [!NOTE]
> Caso exista um conflito entre a política de nível da subscrição e a política de nível do grupo de recursos, a política de nível do grupo de recursos tem prioridade.
>
>

O Centro de Segurança analisa o estado de segurança dos seus recursos do Azure. Quando o Centro de segurança identifica potenciais vulnerabilidades de segurança, cria recomendações com base nos controlos definidos na política de segurança. As recomendações orientam-no processo de configurar os controlos de segurança necessário.

Recomendações de política atual no Centro de segurança se concentrem nas atualizações do sistema, configuração do SO, grupos de segurança em sub-redes e máquinas virtuais (VMs), auditoria de base de dados SQL, TDE de base de dados SQL, de rede e firewalls de aplicações web. A cobertura mais atualizadas de recomendações do Centro de segurança, consulte [recomendações de segurança no Centro de segurança](security-center-recommendations.md).

## <a name="scenario"></a>Cenário
Este cenário mostra como utilizar o Centro de segurança para ajudar a reduzir as chances de um incidente de segurança significativo ao monitorizar as recomendações do Centro de segurança e tomar a ação. O cenário utiliza a empresa fictícia, Contoso e funções apresentadas no Centro de segurança [guia de operações e planeamento](security-center-planning-and-operations-guide.md#security-roles-and-access-controls). As funções representam utilizadores individuais e equipes que podem utilizar o Centro de segurança para efetuar diferentes tarefas relacionadas à segurança. As funções são:

![Funções de cenário][2]

Contoso migrou recentemente alguns dos seus recursos no local para o Azure. Contoso quer implementar e manter a proteção que reduzir sua vulnerabilidade a um ataque de segurança de seus recursos na cloud.

## <a name="recommended-solution"></a>Solução recomendada
Uma solução consiste em utilizar o Centro de segurança para evitar e detectar vulnerabilidades de segurança. Contoso tem acesso ao centro de segurança através da sua subscrição do Azure. O [escalão gratuito](security-center-pricing.md) do Centro de segurança é ativado automaticamente todas as subscrições do Azure e a recolha de dados é ativada em todas as VMs na sua subscrição.

David, na segurança de TI da Contoso, configura um **política de segurança** o Centro de segurança. Centro de segurança analisa o estado de segurança dos recursos do Azure da Contoso. Quando o Centro de segurança identifica potenciais vulnerabilidades de segurança, ele cria **recomendações** com base nos controlos definidos na política de segurança.

Jeff, um proprietário de carga de trabalho na cloud, é responsável pela implementação e manutenção de proteções de acordo com as políticas de segurança da Contoso. Jeff pode monitorizar as recomendações criadas pelo centro de segurança para aplicar proteção. As recomendações orientam Jeff pelo processo de configurar os controlos de segurança necessário.

Para Jeff implementar e manter proteções e eliminar as vulnerabilidades de segurança, ele precisa:

- Recomendações de segurança de monitor fornecidas pelo centro de segurança
- Recomendações de segurança de avaliar e decidir se ele deve aplicar ou dispensar
- Aplicar recomendações de segurança

Vamos seguir os passos de Jeff para ver como ele utiliza as recomendações do Centro de segurança para ajudá-lo durante o processo de configuração de controlos para eliminar a vulnerabilidades de segurança.

## <a name="how-to-implement-this-solution"></a>Como implementar esta solução
Jeff inicia sessão [portal do Azure](https://azure.microsoft.com/features/azure-portal/) e abre a consola do Centro de segurança. Como parte de suas atividades de monitorização diárias, ele verifica se existem recomendações de segurança, efetuando os seguintes passos:

1. Jeff seleciona os **recomendações** mosaico para abrir **recomendações**.
   ![Selecione o mosaico de recomendações][3]
2. Jeff revê a lista de recomendações. Ele vê que o Centro de segurança forneceu a lista de recomendações apresentadas na ordem de prioridade, de prioridade máxima de prioridade mais baixa. Ele decide resolver uma recomendação de prioridade alta na lista. Ele seleciona **instalar o Endpoint Protection** sob **recomendações**.
3. **Instalar o Endpoint Protection** abre-se apresentar uma lista de VMs sem antimalware ativada. Jeff revê a lista de VMs, seleciona todas as VMs e, em seguida, seleciona **instalar em 3 VMs**.
   ![Install endpoint protection][4] (Instalar a proteção de ponto final)
4. **Selecionar a proteção de ponto final** abre-se de fornecer Jeff com duas soluções de antimalware. Jeff seleciona os **Microsoft Antimalware** solução.
5. São apresentadas informações adicionais sobre a solução de antimalware. Jeff seleciona **criar**.
   ![Antimalware da Microsoft][5]
6. Jeff introduz as definições de configuração necessárias em **instale** e seleciona **OK**.

[O Microsoft Antimalware](../security/azure-security-antimalware.md) já se encontra ativa nas VMs selecionadas.

Jeff continua mover de alta prioridade e as recomendações de prioridade média, a tomada de decisões em implementação. Jeff faz referência a [recomendações de segurança](security-center-recommendations.md) artigo para compreender as recomendações e o que cada um deles faz se ele aplica-se.

Jeff fica a saber que [Microsoft Security Response Center (MSRC)](../security/azure-security-response-center.md) efetua a segurança selecione monitorização da rede do Azure e infraestrutura e recebe a reclamações de abuso e inteligência de ameaças de terceiros. Se Jeff fornece detalhes de contacto de segurança para a subscrição do Azure da Contoso, contactos de Microsoft Contoso se o MSRC Deteta os dados do cliente da Contoso que foram acedidos por uma entidade ilícita ou não autorizada. Vamos seguir Jeff como ele se aplica a **fornecer detalhes de contactos de segurança** recomendação (uma recomendação com gravidade média na lista de recomendações acima).

1. Jeff seleciona **fornecer detalhes de contactos de segurança** sob **recomendações**, que abre **fornecer detalhes de contactos de segurança**.
2. Jeff seleciona a subscrição do Azure para fornecer informações de contacto no. Um segundo **fornecer detalhes de contactos de segurança** é aberto o painel.
   ![Detalhes de contacto de segurança][6]
3. Sob **fornecer detalhes de contactos de segurança**, Jeff introduz:

  - os endereços de correio eletrónico de contacto de segurança separados por vírgulas (não existe um limite ao número de endereços de e-mail que ele pode introduzir)
  - número de telefone de contato de segurança

4. Jeff também ativa a opção **enviar-me e-mails sobre alertas** para receber e-mails sobre alertas de gravidade elevada.
5. Jeff seleciona **OK** para aplicar as informações de contacto de segurança para a subscrição da Contoso.

Por fim, Jeff analisa a recomendação de baixa prioridade **vulnerabilidades do SO remediar** e determina a que esta recomendação não é aplicável. Pretende ignorar a recomendação. Jeff seleciona as reticências que aparecem à direita e, em seguida, seleciona **dispensar**.
   ![Ignorar a recomendação][7]

## <a name="conclusion"></a>Conclusão
Recomendações no Centro de segurança de monitorização podem ajudar a eliminar vulnerabilidades de segurança antes de ocorre um ataque. Pode impedir que um incidente de segurança pela implementação e manutenção de proteções com as políticas de segurança no Centro de segurança.

## <a name="next-steps"></a>Passos Seguintes
Neste cenário de mostrar como utilizar políticas de segurança e recomendações no Centro de segurança para ajudar a mitigar um ataque de segurança. Consulte a [cenário de resposta a incidentes](security-center-incident-response.md) para aprender a ter uma plano em vigor antes que ocorra um ataque de resposta a incidente.

Para saber mais sobre o Centro de segurança, veja:

* [Monitorização de estado de funcionamento de segurança](security-center-monitoring.md) – Saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
* [Gerir e responder a alertas de segurança](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
* [Monitorização e a processar eventos de segurança](security-center-events-dashboard.md) - Saiba como monitorizar e processar eventos de segurança recolhidos ao longo do tempo.
* [Monitorizar soluções de parceiros](security-center-partner-solutions.md) – Saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Blogue de segurança do Azure](https://blogs.msdn.com/b/azuresecurity/) — obtenha os mais recentes notícias de segurança do Azure e as informações.

<!--Image references-->
[1]: ./media/security-center-using-recommendations/security-center-policy-inheritance.png
[2]: ./media/security-center-using-recommendations/scenario-roles.png
[3]: ./media/security-center-using-recommendations/select-recommendations-tile.png
[4]: ./media/security-center-using-recommendations/install-endpoint-protection.png
[5]:./media/security-center-using-recommendations/microsoft-antimalware.png
[6]: ./media/security-center-using-recommendations/provide-security-contact-details.png
[7]: ./media/security-center-using-recommendations/dismiss-recommendation.png

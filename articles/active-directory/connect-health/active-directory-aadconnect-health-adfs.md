---
title: Utilizar o Azure AD Connect Health com o AD FS | Microsoft Docs
description: Esta é a página do Azure AD Connect Health acerca de como monitorizar a infraestrutura do AD FS no local.
services: active-directory
documentationcenter: ''
author: karavar
manager: mtillman
editor: curtand
ms.assetid: dc0e53d8-403e-462a-9543-164eaa7dd8b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ad8ed320a8dd91ea83dbaf71e2e9514b4df4cdb5
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="monitor-ad-fs-using-azure-ad-connect-health"></a>Monitorizar o AD FS utilizando o Azure AD Connect Health
A seguinte documentação é específica para monitorizar a infraestrutura do AD FS com o Azure AD Connect Health. Para obter informações sobre a monitorização do Azure AD Connect (Sincronização) com o Azure AD Connect Health, consulte [Utilizar Azure AD Connect Health para Sincronização](active-directory-aadconnect-health-sync.md). Além disso, para obter informações sobre a monitorização dos Serviços de Domínio do Active Directory com o Azure AD Connect Health, consulte [Utilizar Azure AD Connect Health com AD DS](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Alertas do AD FS
A secção Alertas do Azure AD Connect Health apresenta a lista de alertas ativos. Cada alerta inclui informações relevantes, os passos de resolução e ligações para a documentação relacionada.

Pode fazer duplo clique num alerta ativo ou resolvido para abrir um painel novo com informações adicionais, passos que pode tomar para resolver o alerta e ligações para documentação relevante. Pode também visualizar dados históricos de alertas que foram resolvidos no passado.

![Portal do Azure AD Connect Health](./media/active-directory-aadconnect-health/alert2.png)

## <a name="usage-analytics-for-ad-fs"></a>Análise de Utilização do AD FS
A Análise de Utilização do Azure AD Connect Health analisa o tráfego de autenticação dos servidores de federação. Pode fazer duplo clique na caixa de análise de utilização para abrir o painel de análise de utilização, que lhe mostra várias métricas e agrupamentos.

> [!NOTE]
> Para utilizar a Análise de Utilização com o AD FS, tem de se certificar de que a auditoria do AD FS está ativada. Para obter mais informações, consulte [Ativar Auditoria do AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).
>
>

![Portal do Azure AD Connect Health](./media/active-directory-aadconnect-health/report1.png)

Para selecionar métricas adicionais, especifique um intervalo de tempo ou, para alterar o agrupamento, clique com o botão direito do rato no gráfico de análise de utilização e selecione Editar Gráfico. Em seguida, pode especificar o intervalo de tempo, selecionar outra métrica e alterar o agrupamento. Pode ver a distribuição do tráfego de autenticação com base em diferentes “métricas” e agrupar cada uma com os parâmetros “agrupar por” relevantes descritos na secção seguinte:

**Métrica: Total de Pedidos** - o número total de pedidos processados pelos servidores do AD FS.

|Agrupar por | O que significa agrupamento e por que motivo é útil? |
| --- | --- |
| Todos | Apresenta a contagem do número total de pedidos processados por todos os servidores do AD FS.|
| Aplicação | Agrupa o total de pedidos com base na entidade confiadora visada. Este agrupamento é útil para entender que aplicação está a receber que percentagem do tráfego total. |
|  Servidor |Agrupa o total de pedidos com base no servidor que os processou. Este agrupamento é útil para entender a distribuição de carga do tráfego total.
| Associação à área de trabalho |Agrupa o total de pedidos consoante sejam provenientes de dispositivos que estão associados à área de trabalho (conhecidos). Este agrupamento é útil para entender se os recursos são acedidos através de dispositivos que são desconhecidos da infraestrutura de identidade. |
|  Método de Autenticação | Agrupa o total de pedidos com base no método de autenticação utilizado para a autenticação. Este agrupamento é útil para entender o método de autenticação comum que é utilizado para autenticação. Seguem-se os métodos de autenticação possíveis <ol> <li>Autenticação Integrada do Windows (Windows)</li> <li>Autenticação Baseada em Formulários (Formulários)</li> <li>SSO (Início de Sessão Único)</li> <li>Autenticação de Certificados X509 (Certificado)</li> <br>Se os servidores de federação receberem o pedido com um Cookie de SSO, esse pedido é contabilizado como SSO (Início de Sessão Único). Nestes casos, se o cookie for válido, o utilizador não é solicitado a fornecer credenciais e obtém acesso totalmente integrado à aplicação. Este comportamento é comum se tiver várias entidades confiadoras protegidas pelos servidores de federação. |
| Localização na Rede | Agrupa o total de pedidos com base na localização na rede do utilizador. Pode ser intranet ou extranet. Este agrupamento é útil para saber que percentagem do tráfego provêm da intranet versus a extranet. |


**Métrica: Pedidos Falhados Totais** - o número total de pedidos falhados processados pelo serviço de federação. (Esta métrica só está disponível no AD FS para o Windows Server 2012 R2)

|Agrupar por | O que significa agrupamento e por que motivo é útil? |
| --- | --- |
| Tipo de Erro | Mostra o número de erros com base em tipos de erros predefinidos. Este agrupamento é útil para entender os tipos comuns de erros. <ul><li>Nome de Utilizador ou Palavra-passe Incorretos: erros devido a nome de utilizador ou palavra-passe incorretos.</li> <li>“Bloqueio de Extranet”: falhas devido a pedidos recebidos de um utilizador que foi bloqueado da extranet </li><li> “Palavra-passe Expirada”: falhas devido a utilizadores que iniciam sessão com uma palavra-passe expirada.</li><li>“Conta Desativada”: falhas devido a utilizadores que iniciam sessão com uma conta desativada.</li><li>“Autenticação do Dispositivo”: falhas devido a utilizadores que não se autenticam utilizando a Autenticação do Dispositivo.</li><li>“Autenticação do Certificado de Utilizador”: falhas devido a utilizadores que não se autenticam devido a um certificado inválido.</li><li>“MFA”: falhas devido a utilizadores que não se autenticam com o Multi-Factor Authentication.</li><li>“Outras Credenciais”: “Autorização de Emissão”: falhas devido a falhas de autorização.</li><li>“Delegação de Emissão”: falhas devido a erros de delegação de emissão.</li><li>“Aceitação de Token”: falhas devido ao AD FS rejeitar o token de um Fornecedor de Identidade de terceiros.</li><li>“Protocolo”: falha devido a erros de protocolo.</li><li>“Desconhecido”: cobre tudo. Todas as outras falhas que não se encaixem nas categorias definidas.</li> |
| Servidor | Agrupa os erros com base no servidor. Este agrupamento é útil para entender a distribuição de erros pelos servidores. Uma distribuição desigual pode indicar um servidor num estado defeituoso. |
| Localização na Rede | Agrupa os erros com base na localização na rede dos pedidos (intranet vs. extranet). Este agrupamento é útil para entender o tipo de pedidos que estão a falhar. |
|  Aplicação | Agrupa as falhas com base na aplicação de destino (entidade confiadora). Este agrupamento é útil para compreender que aplicação de destino está a ter o maior número de erros. |

**Métrica: Contagem do Utilizador** - número médio de utilizadores exclusivos em autenticação ativa com o AD FS

|Agrupar por | O que significa agrupamento e por que motivo é útil? |
| --- | --- |
|Todos |Esta métrica disponibiliza uma contagem do número médio de utilizadores que utilizam o serviço de federação no intervalo de tempo selecionado. Os utilizadores não são agrupados. <br>A média depende do intervalo de tempo selecionado. |
| Aplicação |Agrupa o número médio de utilizadores com base na aplicação de destino (entidade confiadora). Este agrupamento é útil para entender quantos utilizadores estão a utilizar que aplicação. |

## <a name="performance-monitoring-for-ad-fs"></a>Monitorização de Desempenho do AD FS
A Monitorização de Desempenho do Azure AD Connect Health fornece informações de monitorização sobre métricas. Ao selecionar a caixa Monitorização, é apresentado um painel novo com informações detalhadas sobre as métricas.

![Portal do Azure AD Connect Health](./media/active-directory-aadconnect-health/perf1.png)

Ao selecionar a opção Filtrar no topo do painel, pode filtrar por servidor para ver as métricas de um servidor específico. Para alterar as métricas, clique com o botão direito do rato no gráfico de monitorização no painel de monitorização e selecione Editar Gráfico (ou selecione o botão Editar Gráfico). No novo painel apresentado, pode selecionar métricas adicionais na lista pendente e especificar um intervalo de tempo para ver os dados de desempenho.

## <a name="top-50-users-with-failed-usernamepassword-logins"></a>Principais 50 Utilizadores com inícios de sessão falhados por Nome de Utilizador/Palavra-passe
Uma das razões comuns para um pedido de autenticação falhado num servidor do AD FS é um pedido com credenciais inválidas, ou seja, um nome de utilizador ou palavra-passe incorreto. Normalmente, acontece aos utilizadores devido a palavras-passe complexas, palavras-passe esquecidas ou erros ortográficos.

Contudo, existem outros motivos que podem resultar num número inesperado de pedidos que os seus servidores AD FS têm de processar, como, por exemplo, uma aplicação que coloque em cache as credenciais do utilizador e estas expiram ou um utilizador malicioso que tenta iniciar sessão numa conta com uma série de palavras-passe bem conhecidas. Estes dois exemplos são motivos válidos que podem levar a um aumento nos pedidos.

O Azure AD Connect Health para ADFS fornece um relatório sobre os principais 50 utilizadores com tentativas de início de sessão falhadas devido a palavra-passe ou nome de utilizador inválido. Este relatório é obtido através do processamento dos eventos de auditoria gerados por todos os servidores AD FS nos farms.

![Portal do Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report1a.png)

Neste relatório, terá acesso fácil às seguintes informações:

* N.º total de pedidos falhados com nome de utilizador/palavra-passe incorretos nos últimos 30 dias.
* N.º médio de utilizadores que falharam com um início de sessão com nome de utilizador/palavra-passe incorretos, por dia.

Clicar nesta parte leva-o para o painel do relatório principal, que disponibiliza detalhes adicionais. Este painel inclui um gráfico com informações de tendências para ajudar a estabelecer uma linha de base sobre pedidos com nome de utilizador ou palavra-passe errados. Além disso, fornece a lista dos 50 principais utilizadores com mais tentativas falhadas durante a última semana. Tenha em conta que os 50 principais utilizadores da última semana podem ajudar a identificar picos de palavras-passe erradas.  

O gráfico fornece as seguintes informações:

* O n.º total de inícios de sessão falhados devido a um nome de utilizador/palavra-passe incorreto numa base diária.
* O n.º total de utilizadores exclusivos que falharam inícios de sessão numa base diária.
* Endereço IP do cliente do último pedido

![Portal do Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report3a.png)

O relatório disponibiliza as seguintes informações:

| Item de Relatório | Descrição |
| --- | --- |
| ID de Utilizador |Mostra o ID de utilizador que foi utilizado. Este valor é o que o utilizador escreveu, que, em alguns casos, é o ID errado que está a ser utilizado. |
| Tentativas Falhadas |Mostra o n.º total de tentativas falhadas para esse ID de utilizador específico. A tabela está ordenada por ordem descendente do maior número de tentativas falhadas. |
| Última Falha |Mostra o carimbo de data/hora em que ocorreu a última falha. |
| IP da Última Falha |Mostra o endereço IP do Cliente no pedido incorreto mais recente. |

> [!NOTE]
> Este relatório é atualizado automaticamente a cada 12 horas com novas informações recolhidas nesse período de tempo. Sendo assim, as tentativas de início de sessão nas últimas 12 horas poderão não estar incluídas no relatório.
>
>

## <a name="risky-ip-report"></a>Relatório de IP Duvidoso 
Os clientes do AD FS podem expor pontos finais de autenticação de palavras-passe à Internet para disponibilizar serviços de autenticação aos utilizadores finais, para que aqueles acedam a aplicações SaaS, como o Office 365. Neste caso, é possível que um ator indevido tente inícios de sessão no seu sistema do AD FS para adivinhar a palavra-passe de um utilizador final e obter acesso aos recursos das aplicações. O AD FS proporciona a funcionalidade de bloqueio de conta de extranet para evitar estes tipos de ataques a partir do AD FS no Windows Server 2012 R2. Se tiver uma versão inferior, recomendamos vivamente que atualize o sistema do AD FS para o Windows Server 2016. <br />
Além disso, é possível que um único endereço IP tente vários inícios de sessão para vários utilizadores. Nesses casos, o número de tentativas por utilizador poderá estar abaixo do limiar da proteção de bloqueio de conta no AD FS. O Azure AD Connect Health oferece agora o “relatório de IP em Risco”, que deteta esta condição e notifica os administradores quando a mesma ocorre. Seguem-se as principais vantagens deste relatório: 
- Deteção de endereços IP que excedem um limiar de inícios de sessão baseados em palavra-passe falhados
- Suporte para inícios de sessão falhados devido a palavra-passe errada ou a estado de bloqueio de extranet
- Notificações por e-mail para alertar os administradores mal essas situações se verifiquem com definições de e-mail personalizáveis
- Definições de limiar personalizáveis que estão em linha com a política de segurança das organizações
- Relatórios transferíveis para análise offline e integração com outros sistemas através de automatização

> [!NOTE]
> Para utilizar este relatório, tem de se certificar de que a auditoria do AD FS está ativada. Para obter mais informações, consulte [Ativar Auditoria do AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs). <br />
> Para aceder à pré-visualização, precisa da permissão do Administrador Global ou do [Leitor de Segurança](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles#security-reader).  
> 

### <a name="what-is-in-the-report"></a>Novidades do relatório
Cada item no relatório de IP em Risco mostra informações agregadas sobre as atividades de início de sessão do AD FS falhadas que excedam o limiar designado. Disponibiliza as informações seguintes: ![Portal do Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report4a.png)

| Item de Relatório | Descrição |
| ------- | ----------- |
| Time Stamp (Carimbo de Data/Hora) | Mostra o carimbo de data/hora com base na hora local do portal do Azure quando a janela de tempo da deteção é iniciada.<br /> Todos os eventos diários são gerados à meia-noite, no fuso horário UTC. <br />O carimbo de data/hora dos eventos de hora é arredondado para o início da hora. Pode ver a hora de início da primeira atividade em “firstAuditTimestamp” no ficheiro exportado. |
| Trigger Type (Tipo de Acionador) | Mostra o tipo da janela de tempo da deteção. Os tipos de acionadores de agregação são horários ou diários. É útil para detetar um ataque de força bruta de frequência elevada por oposição a um ataque lento, em que o número de tentativas é distribuído ao longo do dia. |
| Endereço IP | O endereço IP em risco individual que recebeu atividades de início de sessão com palavra-passe errada ou bloqueio de extranet. Este pode ser um endereço IPv4 ou IPv6. |
| Bad Password Error Count (Contagem de Erros de Palavra-passe Errada) | A contagem de erros de Palavra-Passe Errada que ocorreram no endereço IP durante a janela de tempo da deteção. Estes erros podem acontecer várias vezes a determinados utilizadores Tenha em atenção que esta contagem não inclui tentativas falhadas devido a palavras-passe expiradas. |
| Extranet Lock Out Error Count (Contagem de Erros de Bloqueio de Extranet) | A contagem de erros de Bloqueio de Extranet que ocorreram no endereço IP durante a janela de tempo da deteção. Os erros de Bloqueio de Extranet podem acontecer várias vezes a determinados utilizadores. Só serão vistos de o Bloqueio de Extranet estiver configurado no AD FS (versões 2012R2 ou superiores). <b>Tenha em atenção</b> que recomendamos vivamente ativar esta funcionalidade caso permita inícios de sessão de extranet através da utilização de palavras-passe. |
| Unique Users Attempted (Utilizadores Individuais Tentados) | A contagem de contas de utilizador individuais tentadas no endereço IP durante a janela de tempo da deteção. Esta contagem proporciona um mecanismo para diferenciar um padrão de ataque a um único utilizador por oposição a um padrão de ataque a vários utilizadores.  |

Por exemplo, o item de relatório abaixo indica que, na janela entre as 18:00 e as 19:00 do dia 28/02/2018, o endereço IP <i>104.2XX.2XX.9</i> não teve erros de palavra-passe errada e teve 284 erros de bloqueio de extranet. Foram afetados 14 utilizadores individuais dentro dos critérios. O evento de atividade excedeu o limiar designado de hora do relatório. 

![Portal do Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report4b.png)

> [!NOTE]
> - Só aparecerão na lista do relatório as atividades que excedam o limiar designado. 
> - Este relatório pode remontar, no máximo, aos últimos 30 dias.
> - Este relatório de alerta não mostra endereços IP do Exchange nem endereços IP privados. Permanecem incluídos na lista de exportação. 
>


![Portal do Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report4c.png)

### <a name="download-risky-ip-report"></a>Transferir o relatório de IP em Risco
Ao utilizar a funcionalidade **Exportar**, é possível exportar toda a lista de endereços IP em risco dos últimos 30 dias a partir do portal do Connect Health. O resultado da exportação incluíra todas as atividades de início de sessão do AD FS falhadas em cada janela de tempo de deteção, pelo que pode personalizar a filtragem após a exportação. Para além das agregações realçadas no portal, o resultado da exportação também mostrará mais detalhes sobre as atividades de início de sessão falhadas por endereço IP:

|  Item de Relatório  |  Descrição  | 
| ------- | ----------- | 
| firstAuditTimestamp | Mostra o primeiro carimbo de data/hora de quando as atividades falhadas se iniciaram durante a janela de tempo de deteção.  | 
| lastAuditTimestamp | Mostra o último carimbo de data/hora de quando as atividades falhadas terminaram durante a janela de tempo de deteção.  | 
| attemptCountThresholdIsExceeded | O sinalizador se as atividades atuais estiverem a exceder o limiar de alerta.  | 
| isWhitelistedIpAddress | O sinalizador se o endereço IP é filtrado em alertas e relatórios. Os endereços IP privados (<i>10.x.x.x, 172.x.x.x e 192.168.x.x</i>) e os endereços IP do Exchange são filtrados e marcados como Verdadeiros. Se vir intervalos de endereços IP privados, é altamente provável que o seu balanceador de carga externo não esteja a enviar o endereço IP cliente quando transmite o pedido para o servidor Proxy de Aplicação Web.  | 

### <a name="configure-notification-settings"></a>Configurar as Definições de Notificação
Os contactos dos administradores do relatório podem ser atualizados através das **Definições de Notificação**. Por predefinição, a notificação de alerta de e-mail de IP em risco está no estado desativado. Para ativar a notificação, utilize o botão em “Obter notificações por e-mail para endereços IP que excedam o relatório de limiar de atividades falhadas”. Tal como as definições de notificação de alertas genéricas no Connect Health, permite-lhe personalizar a lista de destinatários designados da notificação quanto ao relatório de IP em risco a partir daí. Também pode notificar todos os administradores globais quando fizer a alteração. 

### <a name="configure-threshold-settings"></a>Configurar as Definições do Limiar
O limiar de alerta pode ser atualizado a partir das Definições do Limiar. Para começar, o sistema tem um limiar predefinido. Existem quatro categorias nas definições de limiar do relatório de IP em risco:

![Portal do Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report4d.png)

| Item do Limiar | Descrição |
| --- | --- |
| (Bad U/P + Extranet Lockout) / Day [(U/P incorreta + Bloqueio de Extranet) / Dia]  | Definição do limiar para reportar a atividade e acionar a notificação de alerta quando a contagem de Palavras-passe Erradas mais a contagem de Bloqueios de Extranet o excede por **dia**. |
| (Bad U/P + Extranet Lockout) / Hour [U/P incorreta + Bloqueio de Extranet / Hora] | Definição do limiar para reportar a atividade e acionar a notificação de alerta quando a contagem de Palavras-passe Erradas mais a contagem de Bloqueios de Extranet o excede por **hora**. |
| Extranet Lockout / Day [Bloqueio de Extranet / Dia] | Definição do limiar para reportar a atividade e acionar a notificação de alerta quando a contagem de Bloqueios de Extranet o excede por **dia**. |
| Extranet Lockout / Hour [Bloqueio de Extranet / Hora]| Definição do limiar para reportar a atividade e acionar a notificação de alerta quando a contagem de Bloqueios de Extranet o excede por **hora**. |

> [!NOTE]
> - A alteração do limiar de relatório será aplicada passado uma hora da alteração da definição. 
> - Os itens reportados existentes não serão afetados pela alteração ao limiar. 
> - Recomendamos que analise o número de eventos vistos no seu ambiente e ajuste o limiar em conformidade. 
>
>

### <a name="faq"></a>FAQ
1. Por que razão estou a ver intervalos de endereços IP privados no relatório?  <br />
Os endereços IP privados (<i>10.x.x.x, 172.x.x.x e 192.168.x.x</i>) e os endereços IP do Exchange são filtrados e marcados como Verdadeiros na lista de permissões. Se vir intervalos de endereços IP privados, é altamente provável que o seu balanceador de carga externo não esteja a enviar o endereço IP cliente quando transmite o pedido para o servidor Proxy de Aplicação Web.

2. Como posso bloquear o endereço IP?  <br />
Deve adicionar um endereço IP malicioso identificado à firewall ou bloqueá-lo no Exchange.   <br />
No AD FS 2016 + 1803.C+ QFE, pode bloquear o endereço IP diretamente no AD FS. 

3. Por que razão não vejo nenhum item neste relatório? <br />
   - As atividades de início de sessão falhadas não estão a exceder as definições de limiar. 
   - Confirme que não tem nenhum alerta “O serviço de estado de funcionamento não está atualizado” ativo na sua lista de servidores do AD FS.  Leia mais sobre [como resolver problemas com este alerta](active-directory-aadconnect-health-data-freshness.md).
   - As auditorias não estão ativadas nos farms do AD FS.


## <a name="related-links"></a>Ligações relacionadas
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalação do Agente do Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health Operations (Operações do Azure AD Connect Health)](active-directory-aadconnect-health-operations.md)
* [Utilizar o Azure AD Connect Health para sincronização](active-directory-aadconnect-health-sync.md)
* [Utilizar o Azure AD Connect Health com o AD DS](active-directory-aadconnect-health-adds.md)
* [FAQ do Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Histórico das Versões do Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)

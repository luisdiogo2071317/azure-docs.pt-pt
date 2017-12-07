---
title: "Vídeo indexada, aplicações de SQL de SaaS do Azure | Microsoft Docs"
description: "Este artigo indexa vários pontos de tempo no nosso minutos 81 vídeos sobre a estrutura de aplicação de inquilinos de BD de SaaS, de conferência o Ignite contida 11 de Outubro de 2017. Pode avançar diretamente para a parte que lhe interessa. Pelo menos 3 padrões são descritas. Funcionalidades do Azure que simplificam o desenvolvimento e gestão descritas."
ms.custom: 
ms.date: 12/06/2017
ms.prod: sql-server
ms.reviewer: billgib
ms.suite: 
ms.technology: database-engine
ms.tgt_pltfrm: 
ms.topic: article
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: db47d1dd0a6114ebca0715da179cc0629b26781a
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2017
---
# <a name="video-indexed-and-annotated-for-mulit-tenant-saas-app-using-azure-sql-database"></a>Vídeo indexada e anotadas para a aplicação de SaaS de mulit inquilino com a SQL Database do Azure

Este artigo é um índice anotado para as localizações de tempo de um minuto 81 vídeo sobre modelos de inquilinos de SaaS ou padrões. Este artigo permite-lhe ignorar trás ou reencaminhar as vídeo para os quais lhe interessa. O vídeo explica as opções de estrutura principal para uma aplicação de base de dados do multi-inquilino na base de dados do Azure SQL. O vídeo inclui demonstrações, instruções de código de gestão e, por vezes mais detalhadamente informado por experiência que pode ser na nossa documentação escrita.

O vídeo amplifies informações na documentação escrita, encontrada em: 
- *Conceptual:* [padrões de inquilinos da base de dados de SaaS multi-inquilino][saas-concept-design-patterns-563e]
- *Tutoriais:* [aplicação Wingtip bilhetes SaaS][saas-how-welcome-wingtip-app-679t]

Os artigos e o vídeo descrevem as várias fases de criação de uma aplicação multi-inquilino na base de dados do Azure SQL Server na nuvem. Funcionalidades especiais da SQL Database do Azure tornam mais fácil desenvolver e implementar aplicações de multi-inquilinos que são mais fáceis de gerir e fiável performant.

Atualizamos regularmente a nossa documentação escrita. O vídeo não editado ou não está atualizado, por isso, eventualmente, mais do detalhe podem tornar-se desatualizados.



## <a name="sequence-of-38-time-indexed-screenshots"></a>Sequência de 38 capturas de ecrã indexada de tempo

Esta secção indexa a localização do tempo de 38 debates durante o minuto 81 vídeo. Índice de cada vez está anotado com uma captura de ecrã da vídeo e, por vezes, com informações adicionais.

O índice de cada hora está no formato de *h:mm*. Por exemplo, o segundo indexada localização de tempo, com a etiqueta **objetivos de sessão**, começa a localização de tempo aproximado de **0:03:11**.


### <a name="compact-links-to-video-indexed-time-locations"></a>Ligações Compact para localizações de tempo indexada vídeo

Os títulos de seguintes estão ligações para as respetivas correspondentes anotadas secções mais à frente neste artigo:

- [1. **(Início)**  Diapositivo boas-vindas, 0:00:03](#anchor-image-wtip-min00001)
- [2. Objetivos de sessão, 0:03:11](#anchor-image-wtip-min00311)
- [3. Agenda, 0:04:17](#anchor-image-wtip-min00417)
- [4. Aplicação web do multi-inquilino, 0:05:05](#anchor-image-wtip-min00505)
- [5. Formulário web de aplicação em ação, 0:05:55](#anchor-image-wtip-min00555)
- [6. Custo (escala, isolamento, recuperação), 0 por inquilino: 09:31](#anchor-image-wtip-min00931)
- [7. Modelos de base de dados para o multi-inquilino: os profissionais de TI e contras, 0:11:59](#anchor-image-wtip-min01159)
- [8. Modelo híbrida mistura vantagens da MT/ST, 0:13:01](#anchor-image-wtip-min01301)
- [9. Multi-inquilino de inquilino único vs: os profissionais de TI e contras, 0:16:44](#anchor-image-wtip-min01644)
- [10. Os conjuntos são rentáveis para cargas de trabalho imprevisíveis, 0:19:36](#anchor-image-wtip-min01936)
- [11. Demonstração da base de dados por inquilino e híbridos ST/MT, 0:20:08](#anchor-image-wtip-min02008)
- [12. Em direto formulário de aplicação que mostra Dojo, 0:20:29](#anchor-image-wtip-min02029)
- [13. MYOB e não DBA vencedor, 0:28:54 em](#anchor-image-wtip-min02854)
- [14. Exemplo de utilização do conjunto elástico MYOB, 0:29:40](#anchor-image-wtip-min02940)
- [15. Aprendizagem de MYOB e outros ISVs, 0:31:36](#anchor-image-wtip-min03136)
- [16. Padrões de compõem no cenário de E2E SaaS, 0:43:15](#anchor-image-wtip-min04315)
- [17. Aplicações de SaaS multi-inquilino de canónico híbrida, 0:47:33](#anchor-image-wtip-min04733)
- [18. Aplicação de exemplo de SaaS Wingtip, 0:48:10](#anchor-image-wtip-min04810)
- [19. Cenários e padrões explorou nos tutoriais, 0:49:10](#anchor-image-wtip-min04910)
- [20. Demonstração de tutoriais e repositório do Github, 0:50:18](#anchor-image-wtip-min05018)
- [21. Repositório do Github Microsoft/WingtipSaaS, 0:50:38](#anchor-image-wtip-min05038)
- [22. Explorar os padrões, 0:56:20](#anchor-image-wtip-min05620)
- [23. Aprovisionamento de inquilinos e integração, 0:57:44](#anchor-image-wtip-min05744)
- [24. Aprovisionamento de inquilinos e a ligação à aplicação, 0:58:58](#anchor-image-wtip-min05858)
- [25. Demonstração da gestão de scripts de aprovisionamento de um único inquilino, 0:59:43](#anchor-image-wtip-min05943)
- [26. PowerShell para aprovisionar e o catálogo, 1:00:02](#anchor-image-wtip-min10002)
- [27. SELECIONE de T-SQL * de TenantsExtended, 03:1:30](#anchor-image-wtip-min10330)
- [28. Gestão de cargas de trabalho de inquilino imprevisíveis, 1:04:36](#anchor-image-wtip-min10436)
- [29. Agrupamento elástico de monitorização, 1:06:39](#anchor-image-wtip-min10639)
- [30. Geração de carga e monitorização do desempenho de, 1:09:42](#anchor-image-wtip-min10942)
- [31. Gestão de esquema em escala, 1:10:33](#anchor-image-wtip-min11033)
- [32. Consulta distribuída entre bases de dados do inquilino, 1:12:21](#anchor-image-wtip-min11221)
- [33. Demonstração da geração de permissão, 1:12:32](#anchor-image-wtip-min11232)
- [34. Análise de adhoc SSMS, 1:12:46](#anchor-image-wtip-min11246)
- [35. Extrair dados do inquilino para o armazém de dados do SQL Server, 1:16:32](#anchor-image-wtip-min11632)
- [36. Gráfico de distribuição de venda diariamente, 1:16:48](#anchor-image-wtip-min11648)
- [37. Moldar cópias de segurança e chamar a ação, 1:19:52](#anchor-image-wtip-min11952)
- [38. Recursos para obter mais informações, 1:20:42](#anchor-image-wtip-min12042)


&nbsp;

### <a name="annotated-index-time-locations-in-the-video"></a>Localizações de tempo de índice anotado as vídeo

Ao clicar em qualquer imagem de captura de ecrã leva-o para a localização de momento exato as vídeo.


&nbsp; <a name="anchor-image-wtip-min00001"/>
#### <a name="1-start-welcome-slide-00001"></a>1. *(Início)*  Diapositivo boas-vindas, 0:00:01

*Aprendizagem de MYOB: padrões para aplicações SaaS na SQL Database do Azure - BRK3120 de conceção*

[![Bem-vindo ao diapositivo][image-wtip-min00003-brk3120-whole-welcome]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1)

- Título: De aprendizagem do MYOB: conceber padrões para aplicações SaaS na SQL Database do Azure
- Bill.Gibson@microsoft.com
- Gestor de programa principal, base de dados SQL do Azure
- Microsoft Ignite sessão BRK3120, Orlando, FL EUA, Outubro/11 2017


&nbsp; <a name="anchor-image-wtip-min00311"/>
#### <a name="2-session-objectives-00153"></a>2. Objetivos de sessão, 0:01:53
[![Objetivos de sessão][image-wtip-min00311-session]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=113)

- Modelos alternativos para aplicações de multi-inquilino, com os profissionais de TI e contras.
- Padrões de SaaS para reduzir os custos de desenvolvimento, gestão e recursos.
- Uma aplicação de exemplo + scripts.
- Funcionalidades de PaaS + padrões de SaaS tornam base de dados SQL uma plataforma de dados altamente dimensionável e económica para multi-inquilino SaaS.


&nbsp; <a name="anchor-image-wtip-min00417"/>
#### <a name="3-agenda-00409"></a>3. Agenda, 0:04:09
[![Agenda][image-wtip-min00417-agenda]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=249)


&nbsp; <a name="anchor-image-wtip-min00505"/>
#### <a name="4-multi-tenant-web-app-00500"></a>4. Aplicação web do multi-inquilino, 0:05:00
[![Aplicações de SaaS Wingtip: aplicação web do multi-inquilino][image-wtip-min00505-web-app]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=300)


&nbsp; <a name="anchor-image-wtip-min00555"/>
#### <a name="5-app-web-form-in-action-00539"></a>5. Formulário web de aplicação em ação, 0:05:39
[![Formulário web de aplicação em ação][image-wtip-min00555-app-web-form]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=339)


&nbsp; <a name="anchor-image-wtip-min00931"/>
#### <a name="6-per-tenant-cost-scale-isolation-recovery-00658"></a>6. Custo (escala, isolamento, recuperação), 0 por inquilino: 06:58
[![Por-inquilinos custo, escala, isolamento, recuperação][image-wtip-min00931-per-tenant-cost]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=418)


&nbsp; <a name="anchor-image-wtip-min01159"/>
#### <a name="7-database-models-for-multi-tenant-pros-and-cons-00952"></a>7. Modelos de base de dados para o multi-inquilino: os profissionais de TI e contras, 0:09:52
[![Modelos de base de dados para o multi-inquilino: os profissionais de TI e contras][image-wtip-min01159-db-models-pros-cons]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=592)


&nbsp; <a name="anchor-image-wtip-min01301"/>
#### <a name="8-hybrid-model-blends-benefits-of-mtst-01229"></a>8. Modelo híbrida mistura vantagens da MT/ST, 0:12:29
[![Modelo híbrida mistura vantagens da MT/ST][image-wtip-min01301-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=749)


&nbsp; <a name="anchor-image-wtip-min01644"/>
#### <a name="9-single-tenant-vs-multi-tenant-pros-and-cons-01311"></a>9. Multi-inquilino de inquilino único vs: os profissionais de TI e contras, 0:13:11
[![Multi-inquilino de inquilino único vs: os profissionais de TI e contras][image-wtip-min01644-st-vs-mt]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=791)


&nbsp; <a name="anchor-image-wtip-min01936"/>
#### <a name="10-pools-are-cost-effective-for-unpredictable-workloads-01749"></a>10. Os conjuntos são rentáveis para cargas de trabalho imprevisíveis, 0:17:49
[![Os conjuntos são rentáveis para cargas de trabalho imprevisíveis][image-wtip-min01936-pools-cost]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1069)


&nbsp; <a name="anchor-image-wtip-min02008"/>
#### <a name="11-demo-of-database-per-tenant-and-hybrid-stmt-01959"></a>11. Demonstração da base de dados por inquilino e híbridos ST/MT, 0:19:59
[![Demonstração da base de dados por inquilino e híbridos ST/MT][image-wtip-min02008-demo-st-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1199)


&nbsp; <a name="anchor-image-wtip-min02029"/>
#### <a name="12-live-app-form-showing-dojo-02010"></a>12. Em direto formulário de aplicação que mostra Dojo, 0:20:10
[![Formulário de aplicação em direto que mostra Dojo][image-wtip-min02029-live-app-form-dojo]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1210)

&nbsp; <a name="anchor-image-wtip-min02854"/>
#### <a name="13-myob-and-not-a-dba-in-sight-02506"></a>13. MYOB e não DBA vencedor, 0:25:06
[![MYOB e não DBA vencedor][image-wtip-min02854-myob-no-dba]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1506)


&nbsp; <a name="anchor-image-wtip-min02940"/>
#### <a name="14-myob-elastic-pool-usage-example-02930"></a>14. Exemplo de utilização do conjunto elástico MYOB, 0:29:30
[![Exemplo de utilização do conjunto elástico MYOB][image-wtip-min02940-myob-elastic]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1770)


&nbsp; <a name="anchor-image-wtip-min03136"/>
#### <a name="15-learning-from-myob-and-other-isvs-03125"></a>15. Aprendizagem de MYOB e outros ISVs, 0:31:25
[![Aprendizagem de MYOB e outros ISVs][image-wtip-min03136-learning-isvs]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1885)


&nbsp; <a name="anchor-image-wtip-min04315"/>
#### <a name="16-patterns-compose-into-e2e-saas-scenario-03142"></a>16. Padrões de compõem no cenário de E2E SaaS, 0:31:42
[![Padrões de compõem no cenário de SaaS de E2E][image-wtip-min04315-patterns-compose]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1902)


&nbsp; <a name="anchor-image-wtip-min04733"/>
#### <a name="17-canonical-hybrid-multi-tenant-saas-app-04604"></a>17. Aplicações de SaaS multi-inquilino de canónico híbrida, 0:46:04
[![Aplicações de SaaS multi-inquilino de híbrida canónico][image-wtip-min04733-canonical-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2764)


&nbsp; <a name="anchor-image-wtip-min04810"/>
#### <a name="18-wingtip-saas-sample-app-04801"></a>18. Aplicação de exemplo de SaaS Wingtip, 0:48:01
[![Aplicação de exemplo do Wingtip SaaS][image-wtip-min04810-wingtip-saas-app]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2881)


&nbsp; <a name="anchor-image-wtip-min04910"/>
#### <a name="19-scenarios-and-patterns-explored-in-the-tutorials-04900"></a>19. Cenários e padrões explorou nos tutoriais, 0:49:00
[![Cenários e padrões explorou nos tutoriais][image-wtip-min04910-scenarios-tutorials]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2940)


&nbsp; <a name="anchor-image-wtip-min05018"/>
#### <a name="20-demo-of-tutorials-and-github-repository-05012"></a>20. Demonstração de tutoriais e repositório do Github, 0:50:12
[![Tutoriais de demonstração e repositório do Github][image-wtip-min05018-demo-tutorials-github]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3012)


&nbsp; <a name="anchor-image-wtip-min05038"/>
#### <a name="21-github-repo-microsoftwingtipsaas-05032"></a>21. Repositório do Github Microsoft/WingtipSaaS, 0:50:32
[![Repositório do Github Microsoft/WingtipSaaS][image-wtip-min05038-github-wingtipsaas]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3032)


&nbsp; <a name="anchor-image-wtip-min05620"/>
#### <a name="22-exploring-the-patterns-05615"></a>22. Explorar os padrões, 0:56:15
[![Explorar os padrões][image-wtip-min05620-exploring-patterns]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3375)


&nbsp; <a name="anchor-image-wtip-min05744"/>
#### <a name="23-provisioning-tenants-and-onboarding-05619"></a>23. Aprovisionamento de inquilinos e integração, 0:56:19
[![Os inquilinos e integração de aprovisionamento][image-wtip-min05744-provisioning-tenants-onboarding-1]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3379)


&nbsp; <a name="anchor-image-wtip-min05858"/>
#### <a name="24-provisioning-tenants-and-application-connection-05752"></a>24. Aprovisionamento de inquilinos e a ligação à aplicação, 0:57:52
[![Aprovisionamento de inquilinos e ligação de aplicação][image-wtip-min05858-provisioning-tenants-app-connection-2]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3472)


&nbsp; <a name="anchor-image-wtip-min05943"/>
#### <a name="25-demo-of-management-scripts-provisioning-a-single-tenant-05936"></a>25. Demonstração da gestão de scripts de aprovisionamento de um único inquilino, 0:59:36
[![Demonstração de aprovisionamento de um único inquilino de scripts de gestão][image-wtip-min05943-demo-management-scripts-st]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3576)


&nbsp; <a name="anchor-image-wtip-min10002"/>
#### <a name="26-powershell-to-provision-and-catalog-05956"></a>26. PowerShell para aprovisionar e o catálogo, 0:59:56
[![PowerShell para aprovisionar e de catálogo][image-wtip-min10002-powershell-provision-catalog]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3596)


&nbsp; <a name="anchor-image-wtip-min10330"/>
#### <a name="27-t-sql-select--from-tenantsextended-10325"></a>27. SELECIONE de T-SQL * de TenantsExtended, 1:03:25
[![SELECIONE de T-SQL * de TenantsExtended][image-wtip-min10330-sql-select-tenantsextended]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3805)


&nbsp; <a name="anchor-image-wtip-min10436"/>
#### <a name="28-managing-unpredictable-tenant-workloads-10334"></a>28. Gestão de cargas de trabalho de inquilino imprevisíveis, 1:03:34
[![Gestão de cargas de trabalho inquilinas imprevisíveis][image-wtip-min10436-managing-unpredictable-workloads]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3814)


&nbsp; <a name="anchor-image-wtip-min10639"/>
#### <a name="29-elastic-pool-monitoring-10632"></a>29. Agrupamento elástico de monitorização, 1:06:32
[![Monitorização do conjunto elástico][image-wtip-min10639-elastic-pool-monitoring]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3992)


&nbsp; <a name="anchor-image-wtip-min10942"/>
#### <a name="30-load-generation-and-performance-monitoring-10937"></a>30. Geração de carga e monitorização do desempenho de, 1:09:37
[![Monitorização de desempenho e de geração de carga][image-wtip-min10942-load-generation]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4117)


&nbsp; <a name="anchor-image-wtip-min11033"/>
#### <a name="31-schema-management-at-scale-10940"></a>31. Gestão de esquema em escala, 1:09:40
[![Gestão de esquema à escala][image-wtip-min11033-schema-management-scale]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=34120)


&nbsp; <a name="anchor-image-wtip-min11221"/>
#### <a name="32-distributed-query-across-tenant-databases-11118"></a>32. Consulta distribuída entre bases de dados do inquilino, 1:11:18
[![Consulta distribuída entre bases de dados do inquilino][image-wtip-min11221-distributed-query]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4278)


&nbsp; <a name="anchor-image-wtip-min11232"/>
#### <a name="33-demo-of-ticket-generation-11228"></a>33. Demonstração da geração de permissão, 1:12:28
[![Demonstração da geração de permissão][image-wtip-min11232-demo-ticket-generation]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4348)


&nbsp; <a name="anchor-image-wtip-min11246"/>
#### <a name="34-ssms-adhoc-analytics-11235"></a>34. Análise de adhoc SSMS, 1:12:35
[![Análise de adhoc SSMS][image-wtip-min11246-ssms-adhoc-analytics]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4355)


&nbsp; <a name="anchor-image-wtip-min11632"/>
#### <a name="35-extract-tenant-data-into-sql-dw-11546"></a>35. Extrair dados do inquilino para o armazém de dados do SQL Server, 1:15:46
[![Extrair dados do inquilino para o armazém de dados do SQL Server][image-wtip-min11632-extract-tenant-data-sql-dw]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4546)


&nbsp; <a name="anchor-image-wtip-min11648"/>
#### <a name="36-graph-of-daily-sale-distribution-11638"></a>36. Gráfico de distribuição de venda diariamente, 1:16:38
[![Gráfico de distribuição de venda diária][image-wtip-min11648-graph-daily-sale-distribution]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4598)


&nbsp; <a name="anchor-image-wtip-min11952"/>
#### <a name="37-wrap-up-and-call-to-action-11743"></a>37. Moldar cópias de segurança e chamar a ação, 1:17:43
[![Moldar cópias de segurança e chamar a ação][image-wtip-min11952-wrap-up-call-action]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4663)


&nbsp; <a name="anchor-image-wtip-min12042"/>
#### <a name="38-resources-for-more-information-12035"></a>38. Recursos para obter mais informações, 1:20:35
[![Recursos para obter mais informações][image-wtip-min12042-resources-more-info]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4835)

- [Mensagem de blogue, 22 de Maio de 2017][resource-blog-saas-patterns-app-dev-sql-db-768h]

- *Conceptual:* [padrões de inquilinos da base de dados de SaaS multi-inquilino][saas-concept-design-patterns-563e]

- *Tutoriais:* [aplicação Wingtip bilhetes SaaS][saas-how-welcome-wingtip-app-679t]

- Repositórios do Github para tipos de aplicação de inquilinos Wingtip bilhetes SaaS:
    - [Repositório do Github para o modelo de aplicação autónoma -][github-wingtip-standaloneapp].
    - [Repositório do Github - DB por inquilino modelo de][github-wingtip-dbpertenant].
    - [Repositório do Github para - modelo de base de dados do multi-inquilino][github-wingtip-multitenantdb].





## <a name="next-steps"></a>Passos seguintes

- [Artigo tutorial primeiro][saas-how-welcome-wingtip-app-679t]




<!-- Image link reference IDs. -->

[image-wtip-min00003-brk3120-whole-welcome]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00003-brk3120-welcome-myob-design-saas-app-sql-db.png "Bem-vindo ao diapositivo"

[image-wtip-min00311-session]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00311-session-objectives-takeaway.png "Objetivos de sessão."

[image-wtip-min00417-agenda]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00417-agenda-app-management-models-patterns.png "Agenda."

[image-wtip-min00505-web-app]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00505-wingtip-saas-app-mt-web.png "Aplicações de SaaS Wingtip: aplicação web do multi-inquilino"

[image-wtip-min00555-app-web-form]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00555-app-form-contoso-concert-hall-night-opera.png "Formulário web de aplicação em ação"

[image-wtip-min00931-per-tenant-cost]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00931-saas-data-management-concerns.png "Por-inquilinos custo, escala, isolamento, recuperação"

[image-wtip-min01159-db-models-pros-cons]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01159-db-models-multi-tenant-saas-apps.png "Modelos de base de dados para o multi-inquilino: os profissionais de TI e contras"

[image-wtip-min01301-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01301-hybrib-model-blends-benefits-mt-st.png "Modelo híbrida mistura vantagens da MT/ST"

[image-wtip-min01644-st-vs-mt]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01644-st-mt-pros-cons.png "Multi-inquilino de inquilino único vs: os profissionais de TI e contras"

[image-wtip-min01936-pools-cost]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01936-pools-cost-effective-unpredictable-workloads.png "Os conjuntos são rentáveis para cargas de trabalho imprevisíveis"

[image-wtip-min02008-demo-st-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02008-demo-st-hybrid.png "Demonstração da base de dados por inquilino e híbridos ST/MT"

[image-wtip-min02029-live-app-form-dojo]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02029-app-form-dogwwod-dojo.png "Formulário de aplicação em direto que mostra Dojo"

[image-wtip-min02854-myob-no-dba]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02854-myob-no-dba.png "MYOB e não DBA vencedor"

[image-wtip-min02940-myob-elastic]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02940-myob-elastic-pool-usage-example.png "Exemplo de utilização do conjunto elástico MYOB"

[image-wtip-min03136-learning-isvs]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min03136-myob-isv-saas-patterns-design-scale.png "Aprendizagem de MYOB e outros ISVs"

[image-wtip-min04315-patterns-compose]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04315-patterns-compose-into-e2e-saas-scenario-st-mt.png "Padrões de compõem no cenário de SaaS de E2E"

[image-wtip-min04733-canonical-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04733-canonical-hybrid-mt-saas-app.png "Aplicações de SaaS multi-inquilino de híbrida canónico"

[image-wtip-min04810-wingtip-saas-app]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04810-saas-sample-app-descr-of-modules-links.png "Aplicação de exemplo do Wingtip SaaS"

[image-wtip-min04910-scenarios-tutorials]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04910-scenarios-patterns-explored-tutorials.png "Cenários e padrões explorou nos tutoriais"

[image-wtip-min05018-demo-tutorials-github]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05018-demo-saas-tutorials-github-repo.png "Demonstração de tutoriais e repositório do Github"

[image-wtip-min05038-github-wingtipsaas]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05038-github-repo-wingtipsaas.png "Repositório do Github Microsoft/WingtipSaaS"

[image-wtip-min05620-exploring-patterns]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05620-exploring-patterns-tutorials.png "Explorar os padrões"

[image-wtip-min05744-provisioning-tenants-onboarding-1]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05744-provisioning-tenants-connecting-run-time-1.png "Os inquilinos e integração de aprovisionamento"

[image-wtip-min05858-provisioning-tenants-app-connection-2]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05858-provisioning-tenants-connecting-run-time-2.png "Aprovisionamento de inquilinos e ligação de aplicação"

[image-wtip-min05943-demo-management-scripts-st]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05943-demo-management-scripts-provisioning-st.png "Demonstração de aprovisionamento de um único inquilino de scripts de gestão"

[image-wtip-min10002-powershell-provision-catalog]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10002-powershell-code.png "PowerShell para aprovisionar e de catálogo"

[image-wtip-min10330-sql-select-tenantsextended]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10330-ssms-tenantcatalog.png "SELECIONE de T-SQL * de TenantsExtended"

[image-wtip-min10436-managing-unpredictable-workloads]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10436-managing-unpredictable-tenant-workloads.png "Gestão de cargas de trabalho inquilinas imprevisíveis"

[image-wtip-min10639-elastic-pool-monitoring]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10639-elastic-pool-monitoring.png "Monitorização do conjunto elástico"

[image-wtip-min10942-load-generation]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10942-schema-management-scale.png "Monitorização de desempenho e de geração de carga"

[image-wtip-min11033-schema-management-scale]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11033-schema-manage-1000s-dbs-one.png "Gestão de esquema à escala"

[image-wtip-min11221-distributed-query]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11221-distributed-query-all-tenants-asif-single-db.png "Consulta distribuída entre bases de dados do inquilino"

[image-wtip-min11232-demo-ticket-generation]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11232-demo-ticket-generation-distributed-query.png "Demonstração da geração de permissão"

[image-wtip-min11246-ssms-adhoc-analytics]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11246-tsql-adhoc-analystics-db-elastic-query.png "Análise de adhoc SSMS"

[image-wtip-min11632-extract-tenant-data-sql-dw]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11632-extract-tenant-data-analytics-db-dw.png "Extrair dados do inquilino para o armazém de dados do SQL Server"

[image-wtip-min11648-graph-daily-sale-distribution]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11648-graph-daily-sale-contoso-concert-hall.png "Gráfico de distribuição de venda diária"

[image-wtip-min11952-wrap-up-call-action]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11952-wrap-call-action-saasfeedback.png "Moldar cópias de segurança e chamar a ação"

[image-wtip-min12042-resources-more-info]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min12042-resources-blog-github-tutorials-get-started.png "Recursos para obter mais informações"




<!-- Article link reference IDs. -->

[saas-concept-design-patterns-563e]: saas-tenancy-app-design-patterns.md

[saas-how-welcome-wingtip-app-679t]: saas-tenancy-welcome-wingtip-tickets-app.md


[video-on-youtube-com-478y]: https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1

[video-on-channel9-479c]: https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3120


[resource-blog-saas-patterns-app-dev-sql-db-768h]: https://azure.microsoft.com/blog/saas-patterns-accelerate-saas-application-development-on-sql-database/


[github-wingtip-standaloneapp]: https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp/

[github-wingtip-dbpertenant]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/

[github-wingtip-multitenantdb]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/


| Recurso | Gratuito | Partilhado (Pré-visualização) | Básica | Standard | Premium </th> |
| --- | --- | --- | --- | --- | --- |
| [Aplicações Web, móveis ou API](https://azure.microsoft.com/services/app-service/) por [plano do App Service](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup> |10 |100 |Ilimitado<sup>2</sup> |Ilimitado<sup>2</sup> |Ilimitado<sup>2</sup> |
| [Aplicações lógicas](https://azure.microsoft.com/services/app-service/logic/) por [plano do App Service](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup> |10 |10 |10 |20 por núcleo |20 por núcleo |
| [Plano do Serviço de Aplicações](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |1 por região |10 por grupo de recursos |100 por grupo de recursos |100 por grupo de recursos |100 por grupo de recursos |
| Tipo de instância de computação |Partilhado |Partilhado |Dedicado<sup>3</sup> |Dedicado<sup>3</sup> |Dedicado<sup>3</sup></p> |
| [Escalamento horizontal](../articles/app-service/web-sites-scale.md) (máximo de instâncias) |1 partilhado |1 partilhado |3 dedicado<sup>3</sup> |10 dedicado<sup>3</sup> |20 dedicado (50 num ASE)<sup>3,4</sup> |
| Armazenamento<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |500 GB<sup>4,5</sup></p> |
| Tempo de CPU (5 min)<sup>6</sup> |3 minutos |3 minutos |Ilimitado, pague no padrão [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sem limite, paga tarifas padrão |Sem limite, paga tarifas padrão |
| Tempo de CPU (dia)<sup>6</sup> |60 minutos |240 minutos |Ilimitado, pague no padrão [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> |Sem limite, paga tarifas padrão |Sem limite, paga tarifas padrão |
| Memória (1 hora) |1024 MB por plano do serviço de aplicações |1024 MB por aplicação |N/A |N/D |N/A |
| Largura de Banda |165 MB |Ilimitado, [taxas de transferência de dados](https://azure.microsoft.com/pricing/details/data-transfers/) aplicam-se |Sem limite, podem ser aplicadas taxas de transferência de dados |Sem limite, podem ser aplicadas taxas de transferência de dados |Sem limite, podem ser aplicadas taxas de transferência de dados |
| Arquitetura da aplicação |32 bits |32 bits |32-bit/64-bit |32-bit/64-bit |32-bit/64-bit |
| Web Sockets por instância<sup>7</sup> |5 |35 |350 |Ilimitado |Ilimitado |
| Em simultâneo [ligações do depurador](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md) por aplicação |1 |1 |1 |5 |5 |
| [subdomínio azurewebsites.NET com FTP/S e SSL](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |X |X |X |X |X |
| [Domínio personalizado](../articles/app-service/app-service-web-tutorial-custom-domain.md) de suporte | |X |X |X |X |
| Domínio personalizado [suporte SSL](../articles/app-service/app-service-web-tutorial-custom-ssl.md) | | |Ligações de SNI SSL ilimitadas |SSL de SNI ilimitados e 1 ligações de IP SSL incluídos |SSL de SNI ilimitados e 1 ligações de IP SSL incluídos |
| Load Balancer Integrado | |X |X |X |X |
| [Always On](../articles/app-service/web-sites-configure.md) | | |X |X |X |
| [Cópias de segurança agendadas](../articles/app-service/web-sites-backup.md) | | | | Cópias de segurança agendadas cada 2 horas, um máximo de 12 cópias de segurança por dia (manual + agendado) | Cópias de segurança agendadas cada hora, um máximo de 50 cópias de segurança por dia (manual + agendado) |
| [Dimensionamento automático](../articles/app-service/web-sites-scale.md) | | | |X |X |
| [WebJobs](../articles/app-service/web-sites-create-web-jobs.md)<sup>8</sup> |X |X |X |X |X |
| [O Azure Scheduler](https://azure.microsoft.com/services/scheduler/) de suporte | |X |X |X |X |
| [Monitorização de pontos finais](../articles/app-service/web-sites-monitor.md) | | |X |X |X |
| [Ranhuras de teste](../articles/app-service/web-sites-staged-publishing.md) | | | |5 |20 |
| Domínios personalizados por aplicação</a> | |500 |500 |500 |500 |
| SLA | |<p> |99,9% |99.95%<sup>10</sup> |99.95%<sup>9</sup> |

<sup>1</sup>aplicações e quotas de armazenamento são por plano do serviço de aplicações, salvo indicação em contrário.  
<sup>2</sup>o número real de aplicações que pode alojar nessas máquinas depende da atividade das aplicações, o tamanho das instâncias da máquina e a utilização de recursos correspondente.  
<sup>3</sup>instâncias dedicadas podem ser de tamanhos diferentes. Ver [preços do App Service](https://azure.microsoft.com/pricing/details/app-service/) para obter mais detalhes.  
<sup>4</sup>escalão premium permite até 50 computa instâncias (sujeito a disponibilidade) e 500 GB de espaço em disco quando utiliza ambientes do serviço de aplicações, e 20 instâncias de computação e 250 GB de armazenamento caso contrário.  
<sup>5</sup>o limite de armazenamento é o tamanho total do conteúdo em todas as aplicações no mesmo plano de serviço de aplicações. Mais opções de armazenamento estão disponíveis no [ambiente de serviço de aplicações](../articles/app-service/environment/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup>estes recursos estão restritos pelo recursos físicos nas instâncias de dedicado (o tamanho da instância e o número de instâncias).  
<sup>7</sup>se dimensionar uma aplicação no escalão básico para duas instâncias, tem 350 conexões simultâneas para cada uma das duas instâncias.  
<sup>8</sup>execute executáveis personalizados e/ou scripts a pedido, com base numa agenda, ou continuamente como tarefa em segundo plano no seu serviço de aplicações instância. Sempre Ativo é necessário para a execução contínua do WebJobs. É necessário o Azure Scheduler, Gratuito ou Padrão, para WebJobs agendados. Não tem qualquer limite predefinido no número de WebJobs que podem ser executadas numa instância de serviço de aplicações, mas há limites práticos que dependem de que o código do aplicativo é a tentando fazer.   
<sup>9</sup>SLA de 99,95% fornecido para implementações que utilizem várias instâncias com o Azure Traffic Manager configurado para ativação pós-falha.  


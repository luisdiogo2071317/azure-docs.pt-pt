---
title: Monitorizar os serviços e aplicações do Azure com o Grafana
description: Dados de rota do Azure Monitor e o Application Insights para que pode vê-los numa Grafana.
services: azure-monitor
keywords: ''
author: rboucher
ms.author: robb
ms.date: 11/06/2017
ms.topic: conceptual
ms.service: azure-monitor
ms.component: ''
ms.openlocfilehash: b4fbd1248f91e0766cca66d1c51033a8b338c324
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957383"
---
# <a name="monitor-your-azure-services-in-grafana"></a>Monitorizar os serviços do Azure no Grafana
Agora também pode monitorizar os serviços do Azure e aplicações a partir [Grafana](https://grafana.com/) utilizando o [Plug-in de origem de dados do Azure Monitor](https://grafana.com/plugins/grafana-azure-monitor-datasource). O plug-in reúne dados de desempenho do aplicativo recolhidos pelo Application Insights SDK, bem como os dados de infraestrutura fornecidos pelo Azure Monitor. Em seguida, pode exibir esses dados no seu dashboard da Grafana.

O plug-in está atualmente em pré-visualização.

Utilize os seguintes passos para configurar um servidor o Grafana do Azure Marketplace e criar dashboards para métricas do Azure Monitor e o Application Insights.

## <a name="set-up-a-grafana-instance"></a>Configure uma instância do Grafana
1. Ir para o Azure Marketplace e escolha o Grafana por laboratórios da Grafana.

2. Preencha os nomes e os detalhes. Crie um novo grupo de recursos. Mantenha um registo de valores que escolher para o nome de utilizador VM, a palavra-passe VM e a palavra-passe de administrador de servidor o Grafana.  

3. Escolha o tamanho da VM e uma conta de armazenamento.

4. Configure as definições de configuração de rede.

5. Ver o resumo e selecione **criar** depois de aceitar os termos de utilização.

## <a name="log-in-to-grafana"></a>Inicie sessão para o Grafana
1. Depois de concluída a implementação, selecione **vá para o grupo de recursos**. É apresentada uma lista de recursos criados recentemente.

    ![Objetos de grupo de recursos o Grafana](.\media\monitor-how-to-grafana\grafana1.png)

    Se selecionar o grupo de segurança de rede (*grafana-nsg* neste caso), pode ver que a porta 3000 é utilizada para aceder ao servidor da Grafana.

2. Volte à lista de recursos e selecione **endereço IP público**. Utilizando os valores encontrados neste ecrã, escreva *http://<IP address>: 3000* ou a  *<DNSName>: 3000* no seu browser. Deverá ver uma página de início de sessão do servidor o Grafana que acabou de criar.

    ![Ecrã de início de sessão o Grafana](.\media\monitor-how-to-grafana\grafana2.png)

3. Inicie sessão com o nome de utilizador como *administrador* e a senha de administrador de servidor Grafana que criou anteriormente.

## <a name="configure-data-source-plugin"></a>Configurar o plug-in de origem de dados

Depois de iniciado sessão com êxito, deverá ver que o plug-in da origem de dados do Azure Monitor já está incluído.

![O Grafana mostra o plug-in do Azure Monitor](.\media\monitor-how-to-grafana\grafana3.png)

1. Selecione **Adicionar origem de dados** para configurar o Azure Monitor e o Application Insights.

2. Escolha um nome para a origem de dados e selecione **do Azure Monitor** como a origem de dados na lista pendente.


## <a name="create-a-service-principal"></a>Criar um principal de serviço

O Grafana utiliza um principal de serviço do Azure Active Directory para ligar a APIs do Azure Monitor e recolher dados de métricas. Deve criar um principal de serviço para gerir o acesso aos recursos do Azure.

1. Ver [estas instruções](../active-directory/develop/howto-create-service-principal-portal.md) para criar um principal de serviço. Copie e guarde o seu inquilino ID, ID de cliente e um segredo do cliente.

2. Ver [atribua a aplicação à função](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#assign-application-to-role) para atribuir a função de leitor para a aplicação do Azure Active Directory.     

3. Se utilizar o Application Insights, também pode incluir o API do Application Insights e o ID de aplicação para recolher métricas do Application Insights com base. Para obter mais informações, consulte [obter a sua chave de API e o ID da aplicação](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).

4. Após introduzir todas estas informações, selecione **guardar** e o Grafana testa a API. Deverá ver uma mensagem semelhante ao seguinte.  

    ![O Grafana mostra o plug-in do Azure Monitor](.\media\monitor-how-to-grafana\grafana4-1.png)

> [!NOTE]
> Ao configurar o plug-in, pode indicar que Cloud do Azure (público, do Azure US Government, Azure Alemanha ou Azure China) gostaria de ter o plug-in para ser configurado em relação.
>
>

## <a name="build-a-grafana-dashboard"></a>Criar um dashboard da Grafana

1. Aceda à página principal e selecione **novo Dashboard**.

2. No novo dashboard, selecione o **Graph**. Pode experimentar outras opções de criação de gráficos, mas este artigo usa *Graph* como exemplo.

    ![Novo Dashboard da Grafana](.\media\monitor-how-to-grafana\grafana5.png)

3. Um gráfico em branco aparece no seu dashboard.

4. Clique com o título do painel e selecione **editar** para introduzir os detalhes dos dados que quer representar neste gráfico de gráfico.

5. Assim que tiver selecionado a todas as VMs corretas, pode começar a ver as métricas no dashboard.

Segue-se um dashboard simple com dois gráficos. Um no lado esquerdo mostra a percentagem de CPU de duas VMs. O gráfico à direita mostra as transações numa conta de armazenamento do Azure, dividida por tipo de transação de API.

![O Grafana dois gráficos exemplo](.\media\monitor-how-to-grafana\grafana6.png)


## <a name="optional-create-dashboard-playlists"></a>Opcional: Criar listas de reprodução do dashboard

Uma das várias funcionalidades da Grafana é a lista de reprodução do dashboard. Pode criar vários dashboards e adicioná-los para configurar um intervalo para cada dashboard mostrar uma lista de reprodução. Selecione **reproduzir** para ver os dashboards percorrer. Poderá exibi-los num monitor de parede grande para fornecer um quadro"Estado" para o seu grupo.

![Exemplo de lista de reprodução o Grafana](.\media\monitor-how-to-grafana\grafana7.png)


## <a name="optional-monitor-your-custom-metrics-in-the-same-grafana-server"></a>Opcional: Monitorizar as métricas personalizadas no mesmo servidor o Grafana

Também pode instalar Telegraf e InfluxDB para recolher e métricas personalizadas e baseado em agente na mesma instância de Grafana de desenho. Existem muitos plugins de origem de dados que pode utilizar para reunir estas métricas num dashboard.

Também pode reutilizar esta configuração para incluir as métricas do seu servidor Prometheus. Utilize o plug-in da origem de dados do Prometheus na Galeria de plug-in da Grafana.

Seguem-se artigos de boa referência sobre como utilizar Telegraf, InfluxDB, Prometheus e Docker
 - [Como monitorizar as métricas do sistema com a pilha de escala no Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-monitor-system-metrics-with-the-tick-stack-on-ubuntu-16-04)

 - [Monitorizar as métricas de recurso do Docker com o Grafana, InfluxDB e Telegraf](https://blog.vpetkov.net/2016/08/04/monitor-docker-resource-metrics-with-grafana-influxdb-and-telegraf/)

 - [Uma solução de monitorização para anfitriões do Docker, contentores e serviços em contentores](https://stefanprodan.com/2016/a-monitoring-solution-for-docker-hosts-containers-and-containerized-services/)

Aqui está uma imagem de um dashboard da Grafana completa que tem as métricas do Azure Monitor e o Application Insights.
![Métricas de exemplo o Grafana](.\media\monitor-how-to-grafana\grafana8.png)


## <a name="clean-up-resources"></a>Limpar recursos

É cobrado quando as VMs estão em execução quer esteja a utilizá-los ou não. Para evitar incorrer em custos adicionais, limpe o grupo de recursos criado neste artigo.

1. No menu esquerdo no portal do Azure, clique em **grupos de recursos** e, em seguida, clique em **Grafana**.
2. Na página do grupo de recursos, clique em **elimine**, tipo **Grafana** na caixa de texto e, em seguida, clique **eliminar**.

## <a name="next-steps"></a>Passos Seguintes
* [Descrição geral das métricas de Monitor do Azure](monitoring-overview-metrics.md)

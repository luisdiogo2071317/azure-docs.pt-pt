---
title: Recolher métricas personalizadas para uma VM do Linux com o agente de Telegraf InfluxData
description: Recolher métricas personalizadas para uma VM do Linux com o agente de Telegraf InfluxData
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 651706b269cf24eca85e0601384ef63cb6ed06a2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990710"
---
# <a name="collect-custom-metrics-for-a-linux-vm-with-the-influxdata-telegraf-agent"></a>Recolher métricas personalizadas para uma VM do Linux com o agente de Telegraf InfluxData

O Azure Monitor permite-lhe recolher métricas personalizadas através de telemetria da sua aplicação, um agente em execução em seus recursos do Azure, ou até mesmo fora para dentro de sistemas de monitorização e enviá-los diretamente para o Azure Monitor. Este artigo se concentra em fornecer instruções sobre como implementar o [InfluxData](https://www.influxdata.com/) Telegraf agente numa VM do Linux no Azure e configurar o agente para publicar as métricas para o Azure Monitor. 

## <a name="influxdata-telegraf-agent"></a>Agente de Telegraf InfluxData 

[Telegraf](https://docs.influxdata.com/telegraf/v1.7/) é um agente controlado por plug-in que permite a recolha de métricas mais de 150 origens diferentes. Dependendo de quais as cargas de trabalho estão sendo executados na sua VM (ex. MySQL, NGINX, Apache, etc.) Pode configurar o agente para tirar partido especializadas entrada plug-ins para recolher métricas. Plug-ins de saída, em seguida, ativar o agente para gravar em destinos de sua escolha. O agente de Telegraf integrou-se diretamente com as métricas personalizadas do Azure Monitor REST API e suporta um "do Azure Monitor saída Plug-in". Isto permite que o agente recolher métricas específicas da carga de trabalho na sua VM do Linux e enviá-los como métricas personalizadas para o Azure Monitor. 

 ![Descrição geral do agente telégrafo](./media/metrics-store-custom-linux-telegraf/telegraf-agent-overview.png)

## <a name="send-custom-metrics"></a>Enviar métricas personalizadas 

Para efeitos deste tutorial, vamos implementar uma VM do Linux com o sistema de operativo Ubuntu 16.04 LTS. O agente de Telegraf é suportado para a maioria dos sistemas de operativos Linux. Pacotes Debian e RPM estão disponíveis, juntamente com descompactadas binários de Linux no portal de download do InfluxData. Consulte este guia de instalação para obter instruções adicionais de instalação Telegraf e as opções. 

Início de sessão para o [Portal do Azure](https://portal.azure.com)

Para criar uma nova VM do Linux: 

1. Clique nas **criar um recurso** opção no painel de navegação esquerdo. 
1. Procure *Máquina Virtual*  
1. Selecione *Ubuntu 16.04 LTS* e clique em **Create** 
1. Forneça um nome VM, como *MyTelegrafVM*.  
1. Deixe o tipo de disco como **SSD**, em seguida, forneça um **nome de utilizador**, como *azureuser*. 
1. Para *tipo de autenticação*, selecione **palavra-passe**, em seguida, escreva uma palavra-passe irá utilizar mais tarde para encaminhar o SSH para esta VM. 
1. Optar por **criar novo grupo de recursos**, em seguida, forneça um nome, tal como *myResourceGroup*.  Escolha a localização pretendida, em seguida, selecione **OK**. 

     ![Criar VM do Ubuntu](./media/metrics-store-custom-linux-telegraf/create-vm.png)

1. Selecione um tamanho para a VM. Pode filtrar por tipo de computação ou o tipo de disco, por exemplo. 

     ![Tamanho da máquina virtual Telegraph descrição geral do agente](./media/metrics-store-custom-linux-telegraf/vm-size.png)

1. Sobre o **página de definições**, na **rede** > **grupo de segurança de rede**   > **Selecione as portas de entrada públicas**, selecione *HTTP* e *SSH (22)*. Deixe o resto dos padrões e selecione **OK**. 

1. Na página Resumo, selecione criar para iniciar a implementação de VM. 

1. A VM é afixada ao dashboard do portal do Azure. Depois de concluída a implementação, o resumo da VM abre-se automaticamente. 

1. No painel da VM, navegue para o **identidade** separador e certifique-se de que a VM tem uma identidade de sistema atribuído *no*. 
 
![FILLIN](./media/metrics-store-custom-linux-telegraf/connect-to-VM.png)
 
## <a name="connect-to-the-vm"></a>Ligar à VM 

Crie uma ligação SSH à VM. Selecione o botão Ligar na página de descrição geral para a sua VM. 

![FILLIN](./media/metrics-store-custom-linux-telegraf/connect-VM-button2.png)

Na ligar à página de máquina virtual, mantenha as opções predefinidas para se conectar por nome DNS através da porta 22. No início de sessão com a VM é mostrada um comando de ligação de conta local. Clique no botão para copiar o comando. O exemplo seguinte mostra o aspeto do comando de ligação SSH: 

```cmd
ssh azureuser@XXXX.XX.XXX 
```

Cole a ligação SSH num shell, tais como o Azure Cloud Shell de comandos Bash no Ubuntu no Windows, ou utilizar um cliente SSH à sua escolha para criar a ligação. 

## <a name="install-and-configure-telegraf"></a>Instalar e configurar Telegraf 

Para instalar o pacote Debian Telegraf na VM, execute os seguintes comandos da sua sessão SSH: 

```cmd
# download the package to the VM 
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.8.0~rc1-1_amd64.deb 
# install the package 
sudo dpkg -i telegraf_1.8.0~rc1-1_amd64.deb
```
Ficheiro de configuração do Telegraf define as operações do Telegraf. Por predefinição, um arquivo de configuração de exemplo está instalado no caminho "/ etc/telegraf/telegraf.conf". O ficheiro de configuração de exemplo apresenta uma lista de todas as entradas de possíveis e plug-ins de saída. No entanto, vamos criar um ficheiro de configuração personalizada e têm o agente de usá-lo ao executar os comandos seguintes 

```cmd
# generate the new Telegraf config file in the current directory 
telegraf --input-filter cpu:mem --output-filter azure_monitor config > azm-telegraf.conf 

# replace the example config with the new generated config 
sudo cp azm-telegraf.conf /etc/telegraf/telegraf.conf 
```

> [!NOTE]
> O procedimento acima apenas permite que dois plug-ins de entrada "cpu" e "memória", fique à vontade adicionar mais entrada plug-ins (Docker, MySQL, NGINX, etc.) dependendo da carga de trabalho em execução no seu computador. Uma lista completa de plug-ins entrada pode ser encontrada aqui. 

Por fim, para que o início de agente utilizando a nova configuração, forçar o agente para parar e iniciar, executando os comandos seguintes 

```cmd
# stop the telegraf agent on the VM 
sudo systemctl stop telegraf 
# start the telegraf agent on the VM to ensure it picks up the latest configuration 
sudo systemctl start telegraf 
```
Agora, o agente irá recolher métricas de todos os plug-ins de entrada especificados e emiti-los para o Azure Monitor. 

## <a name="plot-your-telegraf-metrics-in-the-azure-portal"></a>Desenhar as suas métricas Telegraf no portal do Azure 

1. Abra o [portal do Azure](https://portal.azure.com) 

1. Navegue para o novo separador do Monitor e, em seguida, selecione **métricas**.  
     ![FILLIN](./media/metrics-store-custom-linux-telegraf/metrics.png)

1. Selecione a sua VM no Seletor de recursos

     ![FILLIN](./media/metrics-store-custom-linux-telegraf/metric-chart.png)

1. Selecione o *Telegraf/CPU* espaço de nomes e selecione o *usage_system* métrica. Pode escolher o filtro por dimensões nesta métrica ou dividir nos mesmos.  

     ![FILLIN](./media/metrics-store-custom-linux-telegraf/VM-resource-selector.png)

## <a name="additional-configuration"></a>Configuração adicional 

O passo a passo acima fornece informações sobre como configurar o agente de Telegraf para recolher métricas de alguns básicas entradas plug-ins. O agente de Telegraf tem suporte para mais de 150 entradas plug-ins, com algumas opções de configuração adicionais de suporte. InfluxData publicou um [lista de plug-ins suportados](https://docs.influxdata.com/telegraf/v1.7/plugins/inputs/) e instruções sobre [como configurá-las](https://docs.influxdata.com/telegraf/v1.7/administration/configuration/).  

Além disso, este passo a passo permitiu que usar o agente de Telegraf para emitir métricas sobre a VM, o agente é implementado no. O agente de Telegraf também pode ser utilizado como um recoletor e o reencaminhador de métricas para outros recursos. Para saber como configurar o agente para emitir métricas para outros recursos do Azure, veja [saída de métrica do Azure Monitor personalizado para Telegraf](https://github.com/influxdata/telegraf/blob/fb704500386214655e2adb53b6eb6b15f7a6c694/plugins/outputs/azure_monitor/README.md).  

## <a name="clean-up-resources"></a>Limpar recursos 

Quando já não forem necessários, pode eliminar o grupo de recursos, a máquina virtual e todos os recursos relacionados. Para fazer isso, selecione o grupo de recursos para a máquina virtual, selecione Delete, em seguida, confirme o nome do grupo de recursos para eliminar. 

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [métricas personalizadas](metrics-custom-overview.md).



---
title: Implementação da máquina virtual do Azure com o Chef | Documentos da Microsoft
description: Saiba como utilizar o Chef para implementações de máquina de virtual automatizado e configuração no Microsoft Azure
services: virtual-machines-windows
documentationcenter: ''
author: diegoviso
manager: jeconnoc
tags: azure-service-management,azure-resource-manager
editor: ''
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: diviso
ms.openlocfilehash: 31a0177ecfd87fa7ea78989b36141070c2ac193b
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865731"
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatizar a implementação de máquinas virtuais do Azure com o Chef
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Chef é uma ótima ferramenta para o fornecimento de automatização e assim o desejar configurações de estado.

Api da cloud com a versão mais recente versão, Chef fornece uma integração perfeita com o Azure, dando-lhe a capacidade de aprovisionar e implementar os Estados de configuração através de um único comando.

Neste artigo, vai configurar o ambiente do Chef para aprovisionar máquinas virtuais do Azure e percorrer a criar uma política ou "Manual" e, em seguida, implementar este manual de uma máquina virtual do Azure.

Vamos começar!

## <a name="chef-basics"></a>Noções básicas do chef
Antes de começar, [rever os conceitos básicos do Chef](http://www.chef.io/chef). 

O diagrama seguinte ilustra a arquitetura geral do Chef.

![][2]

O chef tem três componentes principais de arquiteturais: servidor do Chef, o cliente do Chef (node) e o Chef estação de trabalho.

O servidor de Chef é o ponto de gestão e existem duas opções para o servidor do Chef: uma solução alojada ou uma solução no local. Usaremos uma solução alojada.

O cliente do Chef (node) é o agente que encontra-se nos servidores que está gerenciando.

A estação de trabalho do Chef é a estação de trabalho do administrador para onde podemos criar políticas e executar comandos de gestão. Podemos executar o **knife** comando da estação de trabalho Chef para gerir a infraestrutura.

Também é o conceito de "Guias detalhados" e "Receitas". Estes são, efetivamente, as políticas que definem e aplicam-se aos servidores.

## <a name="preparing-the-workstation"></a>A preparar a estação de trabalho
Em primeiro lugar, permite que a estação de trabalho de preparação. Estou usando uma estação de trabalho padrão do Windows. É necessário criar um diretório para armazenar os ficheiros de configuração e os guias detalhados.

Primeiro, crie um diretório chamado C:\chef.

Em seguida, crie um diretório de segunda chamado c:\chef\cookbooks.

Agora, precisamos transferir o ficheiro de definições do Azure, portanto, a Chef pode comunicar com a subscrição do Azure.

Transferir a publicar as definições com o PowerShell Azure [Get-AzurePublishSettingsFile](https://docs.microsoft.com/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) comando. 

Guarde o ficheiro de definições de publicação no C:\chef.

## <a name="creating-a-managed-chef-account"></a>Criar uma conta gerida do Chef
Inscreva-se uma conta do Chef alojada [aqui](https://manage.chef.io/signup).

Durante o processo de inscrição, será solicitado para criar uma nova organização.

![][3]

Depois de criar a sua organização, Baixe o starter kit.

![][4]

> [!NOTE]
> Se receber uma mensagem avisando que as suas chaves serão repostas, há problema continuar porque não temos nenhuma infraestrutura existente configurada ainda.
> 
> 

Este ficheiro de zip do starter kit contém os ficheiros de configuração da organização e as chaves.

## <a name="configuring-the-chef-workstation"></a>Configurar a estação de trabalho do Chef
Extraia o conteúdo do chef-starter.zip para C:\chef.

Copiar todos os ficheiros no repositório do starter\chef chef\.chef para o seu diretório c:\chef.

O diretório deve agora ter um aspeto semelhante ao seguinte exemplo.

![][5]

Agora, deve ter quatro arquivos, incluindo o arquivo de publicação do Azure na raiz da c:\chef.

Os ficheiros PEM contêm sua organização e as chaves privadas de administrador para a comunicação, enquanto o ficheiro de knife.rb contém a configuração de knife. Precisamos de editar o ficheiro de knife.rb.

Abra o ficheiro no seu editor preferencial e modificar o "cookbook_path" ao remover o /.... / do caminho para que seja apresentado, conforme mostrado a seguir.

    cookbook_path  ["#{current_dir}/cookbooks"]

Adicione também o seguinte ficheiro de definições de publicação de linha que reflete o nome do seu Azure.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

O ficheiro de knife.rb deve agora ter um aspeto semelhante ao seguinte exemplo.

![][6]

Estas linhas garantirá que referencia o diretório de guias detalhados em c:\chef\cookbooks Knife e também utiliza o nosso arquivo de definições de publicação do Azure durante as operações do Azure.

## <a name="installing-the-chef-development-kit"></a>Instalar o Kit de desenvolvimento do Chef
Próxima [transferir e instalar](http://downloads.getchef.com/chef-dk/windows) o ChefDK (Chef Development Kit) para configurar a sua estação de trabalho do Chef.

![][7]

Instale a localização predefinida da c:\opscode. Esta instalação irá demorar cerca de 10 minutos.

Confirme se que a variável de caminho contém entradas para C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Se não estiverem lá, certifique-se de que adicionar estes caminhos!

*TENHA EM ATENÇÃO DE QUE A ORDEM DO CAMINHO É IMPORTANTE!* Se seus caminhos de opscode não estão na ordem correta, que terá problemas.

Reinicie a sua estação de trabalho antes de continuar.

Em seguida, vamos instalar a extensão do Azure de Knife. Esta opção fornece Knife com o "Azure Plug-in".

Execute o seguinte comando.

    chef gem install knife-azure ––pre

> [!NOTE]
> O argumento – pré garante que está recebendo a última versão RC do plug-in de Azure Knife que fornece acesso ao conjunto de APIs mais recentes.
> 
> 

É provável que um número de dependências também será instalado ao mesmo tempo.

![][8]

Para garantir que tudo está configurada corretamente, execute o seguinte comando.

    knife azure image list

Se tudo o que está configurado corretamente, verá uma lista de rolagem de imagens do Azure disponíveis por meio de.

Parabéns! A estação de trabalho é configurada!

## <a name="creating-a-cookbook"></a>Criação de um guia detalhado
Um guia detalhado é usado por Chef para definir um conjunto de comandos que deseja executar no seu cliente gerenciado. A criação de um guia detalhado é simples e usamos o **chef cookbook de gerar** comando para gerar o modelo do manual. Eu a chamada meu servidor de web Cookbook como eu gostaria de ter uma política que implementa automaticamente o IIS.

Em seu diretório C:\Chef execute o seguinte comando.

    chef generate cookbook webserver

Isso irá gerar um conjunto de ficheiros no diretório C:\Chef\cookbooks\webserver. Agora, precisamos definir o conjunto de comandos, gostaríamos do cliente do Chef para executar na máquina virtual gerida.

Os comandos são armazenados em default.rb o ficheiro. Nesse arquivo, eu vai definir um conjunto de comandos que instala o IIS, inicia o IIS e copia um ficheiro de modelo para a pasta wwwroot.

Modificar o ficheiro de C:\chef\cookbooks\webserver\recipes\default.rb e adicione as seguintes linhas.

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

Quando tiver terminado, guarde o ficheiro.

## <a name="creating-a-template"></a>Criar um modelo
Como mencionamos anteriormente, é necessário gerar um arquivo de modelo que será utilizado como a página default. HTML.

Execute o seguinte comando para gerar o modelo.

    chef generate template webserver Default.htm

Agora, navegue para o ficheiro de C:\chef\cookbooks\webserver\templates\default\Default.htm.erb. Edite o ficheiro ao adicionar um código de "Hello World" HTML simples e, em seguida, guarde o ficheiro.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Carregue o manual de para o servidor do Chef
Neste passo, estamos fazendo uma cópia do guia que criámos no computador local e carregá-lo para o servidor de alojado Chef. Depois de carregados, o Cookbook será apresentado sob o **política** separador.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Implementar uma máquina virtual com o Knife Azure
Iremos agora implementar uma máquina virtual do Azure e aplicar o manual de "Servidor Web", que irá instalar o web service e o padrão página web do IIS.

Para fazer isso, utilize o **criar servidor do azure de knife** comando.

Estou exemplo do comando apresentada a seguir.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Os parâmetros são facilmente compreensíveis. Substitua as variáveis específicas e executar.

> [!NOTE]
> Através da linha de comando, eu estou também automatizar minhas regras de filtro de rede do ponto final ao utilizar o parâmetro de pontos de extremidade – tcp. Posso abrir as portas 80 e a 3389 para fornecer acesso a minha página da web e sessão RDP.
> 
> 

Depois de executar o comando, vá para o portal do Azure e verá que o seu computador começar a aprovisionar.

![][13]

O prompt de comando apresentada a seguir.

![][10]

Assim que a implementação estiver concluída, poderemos ligar ao serviço web através da porta 80, como a porta tivéssemos aberto quando estamos aprovisionado a máquina virtual com o comando Knife Azure. Como esta máquina virtual é a máquina virtual única no meu serviço em nuvem, irá ligá-la com o url do serviço cloud.

![][11]

Como pode ver, tenho criativo com meu código HTML.

Não se esqueça de que também pode ligar através de uma sessão RDP a partir do portal do Azure por meio da porta 3389.

Espero que isso tem sido útil! Aceda e comece a infraestrutura como jornada de código com o Azure hoje mesmo!

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png


<!--Link references-->

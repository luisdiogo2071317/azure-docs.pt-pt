---
title: Alojar um Ruby on Rails Web site numa VM do Linux | Documentos da Microsoft
description: Configurar e alojar um Ruby on Rails com base no Web site no Azure através de uma máquina virtual Linux.
services: virtual-machines-linux
documentationcenter: ruby
author: rmcmurray
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: aad32685-3550-4bff-9c73-beb8d70b3291
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: ruby
ms.topic: article
ms.date: 06/27/2017
ms.author: robmcm
ms.openlocfilehash: 6ea1d249b7f9aec3a45923b162a97ce7f83d0d31
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901158"
---
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Aplicação Web Ruby on Rails numa VM do Azure
Este tutorial mostra como alojar um Ruby on Rails Web site no Azure com uma máquina virtual Linux.  

Este tutorial foi validado com o Ubuntu Server 14.04 LTS. Se utilizar uma distribuição Linux diferente, poderá ter de modificar os passos para instalar o Rails.

> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com os recursos: [Resource Manager e clássico](../../../azure-resource-manager/resource-manager-deployment-model.md).  Este artigo cobre a utilização do modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]
>

## <a name="create-an-azure-vm"></a>Criar uma VM do Azure
Comece por criar uma VM do Azure com uma imagem do Linux.

Para criar a VM, pode utilizar o portal do Azure ou a Interface de linha de comandos (CLI do Azure).

### <a name="azure-portal"></a>Portal do Azure
1. Inicie sessão no [portal do Azure](https://portal.azure.com)
2. Clique em **criar um recurso**, em seguida, escreva "Ubuntu Server 14.04" na caixa de pesquisa. Clique na entrada devolvida pela pesquisa. Para o modelo de implementação, selecione **clássico**, em seguida, clique em "Criar".
3. No painel Noções básicas, fornecer valores para os campos obrigatórios: nome (para a VM), nome de utilizador, tipo de autenticação e as credenciais correspondentes, subscrição do Azure, o grupo de recursos e localização.

   ![Criar uma nova imagem de Ubuntu](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. Depois da VM está aprovisionada, clique no nome da VM e clique em **pontos de extremidade** no **definições** categoria. Encontrar o ponto final SSH, listado na **autónomo**.

   ![Ponto final predefinido](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a>CLI do Azure
Siga os passos em [criar numa máquina Virtual com Linux em execução][vm-instructions].

Depois da VM está aprovisionada, pode obter o ponto final SSH ao executar o seguinte comando:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Instale o Ruby on Rails
1. Utilize o SSH para ligar à VM.
2. A partir da sessão SSH, utilize os seguintes comandos para instalar o Ruby na VM:

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > The brightbox repository contains the current Ruby distribution.

    A instalação pode demorar alguns minutos. Quando estiver concluída, utilize o seguinte comando para verificar se o Ruby está instalado:

        ruby -v

3. Utilize o seguinte comando para instalar o Rails:

        sudo gem install rails --no-rdoc --no-ri -V

    Utilize os-- não-rdoc e --não-RIS sinalizadores para ignorar a instalação a documentação, que é mais rápida.
    Este comando provavelmente levará muito tempo a executar, portanto, adicionar -V irá apresentar informações sobre o progresso da instalação.

## <a name="create-and-run-an-app"></a>Criar e executar uma aplicação
Ainda com sessão iniciada através de SSH, execute os seguintes comandos:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

O [novo](http://guides.rubyonrails.org/command_line.html#rails-new) comando cria uma nova aplicação Rails. O [servidor](http://guides.rubyonrails.org/command_line.html#rails-server) comando inicia o servidor de web de WEBrick que vem com Rails. (Para utilização em produção, provavelmente desejaria utilizar um servidor diferente, como unicórnio ou passageiro.)

Deverá ver um resultado semelhante ao seguinte.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Adicionar um ponto final
1. Aceda ao [portal do Azure] [https://portal.azure.com] e selecione a sua VM.

2. Selecione **pontos de extremidade** no **definições** no lado esquerdo da página de borda.

3. Clique em **adicionar** na parte superior da página.

4. Na **adicionar ponto final** caixa de diálogo página, introduza as seguintes informações:

   * **Nome**: HTTP
   * **Protocolo**: TCP
   * **Porta pública**: 80
   * **Porta privada**: 3000
   * **Endereço de instalador de plataforma de vírgula flutuante**: desativado
   * **Lista de controle de acesso - ordem**: 1001 ou outro valor que define a prioridade desta regra de acesso.
   * **Lista de controle de acesso - nome**: allowHTTP
   * **Lista de controle de acesso - ação**: permitir
   * **Lista de controle de acesso - sub-rede remota**: 1.0.0.0/16

     Este ponto final tem uma porta pública 80 que irá encaminhar o tráfego para a porta privada de 3000, onde o servidor do Rails está a escutar. A regra de lista de controlo de acesso permite tráfego público na porta 80.

     ![new-endpoint](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. Clique em OK para guardar o ponto final.

6. Deve aparecer uma mensagem que indica **a guardar ponto final de máquina virtual**. Assim que esta mensagem desaparece, o ponto final está ativo. Agora pode testar seu aplicativo ao navegar para o nome DNS da sua máquina virtual. O Web site deve ter um aspeto semelhante ao seguinte:

    ![página predefinida do rails][default-rails-cloud]

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, fez a maioria dos passos manualmente. Num ambiente de produção, teria de escrever a sua aplicação na máquina de desenvolvimento e implantá-lo à VM do Azure. Além disso, a maioria dos ambientes de produção alojar a aplicação Rails em conjunto com outro processo de servidor, como o Apache ou o NginX, que gere pedidos de encaminhamento para várias instâncias da aplicação Rails e que serve recursos estáticos. Para obter mais informações, consulte http://guides.rubyonrails.org/routing.html.

Para saber mais sobre o Ruby on Rails, visite o [Ruby no Rails guias][rails-guides].

Para utilizar serviços Azure da sua aplicação Ruby, consulte:

* [Store dados não estruturados usando blobs][blobs]
* [Store pares de chave/valor usando tabelas][tables]
* [Servir conteúdo de largura de banda alta com a rede de entrega de conteúdo][cdn-howto]

<!-- WA.com links -->
[blobs]:../../../storage/blobs/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]:https://azure.microsoft.com/develop/ruby/app-services/
[tables]:../../../cosmos-db/table-storage-how-to-use-ruby.md
[vm-instructions]:createportal-classic.md

<!-- External Links -->
[rails-guides]:http://guides.rubyonrails.org/
[sqlite3]:http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]:./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]:./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]:./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png

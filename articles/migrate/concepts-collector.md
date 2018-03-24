---
title: Aplicação de recoletor no Azure migrar | Microsoft Docs
description: Fornece uma descrição geral do dispositivo de Recoletor e como configurá-lo.
author: ruturaj
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 01/23/2017
ms.author: ruturajd
services: azure-migrate
ms.openlocfilehash: ea2367a6e1facfbe6a36cb145e258491a1c99517
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="collector-appliance"></a>Aplicação de recoletor

[Migrar do Azure](migrate-overview.md) avalia cargas de trabalho no local para a migração para o Azure. Este artigo fornece informações sobre como utilizar a aplicação do Recoletor.



## <a name="overview"></a>Descrição geral

Um Recoletor migrar do Azure é uma aplicação simples que pode ser utilizada para detetar o seu ambiente do vCenter no local. Este dispositivo Deteta máquinas de VMware no local e envia os metadados sobre-los para o serviço Azure migrar.

A aplicação do Recoletor é um OVF que pode transferir do projeto migrar do Azure. Para instanciar uma máquina virtual do VMware com 4 núcleos, 8 GB de RAM e um disco de 80 GB. O sistema operativo do dispositivo de é Windows Server 2012 R2 (64 bits).

Pode criar o Recoletor seguindo os passos aqui - [como criar a VM de Recoletor](tutorial-assessment-vmware.md#create-the-collector-vm).

## <a name="collector-communication-diagram"></a>Diagrama de comunicação de recoletor

![Diagrama de comunicação de recoletor](./media/tutorial-assessment-vmware/portdiagram.PNG)


| Componente      | Para comunicar com   | Porta necessária                            | Razão                                   |
| -------------- | --------------------- | ---------------------------------------- | ---------------------------------------- |
| Recoletor      | Serviço do Azure Migrate | TCP 443                                  | Recoletor deverá conseguir comunicar com o serviço através da porta SSL 443 |
| Recoletor      | vCenter Server        | Predefinição 443                             | Recoletor deverá conseguir comunicar com o servidor vCenter. Ligar ao vCenter no 443 por predefinição. Se o vCenter escuta numa porta diferente, essa porta deve estar disponível como porta de saída no recoletor |
| Recoletor      | RDP|   | TCP 3389 | Para que possa ser capaz de RDP no computador do Recoletor |





## <a name="collector-pre-requisites"></a>Pré-requisitos do recoletor

O Recoletor tem de passar algumas verificações de pré-requisitos para garantir que pode ligar ao serviço Azure migrar e carregar os dados detetados. Este artigo observa cada um dos pré-requisitos e compreender por que motivo é necessário.

### <a name="internet-connectivity"></a>Conectividade Internet

A aplicação de recoletor tem de estar ligado à internet para enviar as informações de máquinas detetados. Pode ligar a máquina à internet de uma das seguintes duas formas.

1. Pode configurar o Recoletor tem conectividade internet direta.
2. Pode configurar o Recoletor para ligar através de um servidor proxy.
    * Se o servidor proxy requer autenticação, pode especificar o nome de utilizador e palavra-passe nas definições de ligação.
    * O FQDN/endereço IP do servidor Proxy deve ter o formato http://IPaddress ou http://FQDN. Proxy de http só é suportada.

> [!NOTE]
> Servidores proxy baseado em HTTPS não são suportadas pelo recoletor.

#### <a name="whitelisting-urls-for-internet-connection"></a>Adicionar à lista branca URLs para ligação à internet

A verificação de pré-requisitos é efetuada com êxito se o Recoletor pode estabelecer ligação à internet através de definições fornecidas. A verificação de conectividade é validada, ligando-se a uma lista de URLs como indicado na tabela seguinte. Se estiver a utilizar qualquer proxy firewall baseada no URL para controlar a conectividade de saída, certifique-se a lista branca que estes necessários URLs:

**URL** | **Objetivo**  
--- | ---
*.portal.azure.com | Necessário para verificar a conectividade com o serviço do Azure e validar a sincronização de hora problemas.

Além disso, a verificação também tenta validar a conetividade para os seguintes URLs, mas não falhou a verificação se não está acessível. Configurar a lista branca para os seguintes URLs é opcional, mas precisa de tomar passos manuais para mitigar a verificação de pré-requisitos.

**URL** | **Objetivo**  | **E se não lista branca**
--- | --- | ---
*.oneget.org:443 | Necessário para transferir o powershell com base vCenter PowerCLI módulo. | A instalação PowerCLI falha. Instale o módulo manualmente.
*.windows.net:443 | Necessário para transferir o powershell com base vCenter PowerCLI módulo. | A instalação PowerCLI falha. Instale o módulo manualmente.
*.windowsazure.com:443 | Necessário para transferir o powershell com base vCenter PowerCLI módulo. | A instalação PowerCLI falha. Instale o módulo manualmente.
*.powershellgallery.com:443 | Necessário para transferir o powershell com base vCenter PowerCLI módulo. | A instalação PowerCLI falha. Instale o módulo manualmente.
*.msecnd.net:443 | Necessário para transferir o powershell com base vCenter PowerCLI módulo. | A instalação PowerCLI falha. Instale o módulo manualmente.
*.visualstudio.com:443 | Necessário para transferir o powershell com base vCenter PowerCLI módulo. | A instalação PowerCLI falha. Instale o módulo manualmente.

### <a name="time-is-in-sync-with-the-internet-server"></a>Tempo está em sincronização com o servidor de internet

O Recoletor deve ser sincronizado com o servidor de tempo de internet para garantir que os pedidos para o serviço são autenticados. O portal.azure.com url deve ser acessível a partir do Recoletor para que possa ser validada a hora. Se a máquina não está sincronizada, terá de alterar a hora do relógio de Recoletor de VM para corresponder à hora atual, da seguinte forma:

1. Abra uma linha de comandos de administrador na VM.
1. Para verificar o fuso horário, execute w32tm /tz.
1. Sincronizar o tempo, execute w32tm /resync.

### <a name="collector-service-should-be-running"></a>Serviço do recoletor deve estar em execução

O serviço do Recoletor de migrar Azure deve estar em execução na máquina. Este serviço é iniciado automaticamente quando o computador arranca. Se o serviço de mensagens em fila não está em execução, pode iniciar o *Azure migrar Recoletor* serviço através do painel de controlo. O serviço do Recoletor é da responsabilidade ligar ao servidor vCenter, recolher os dados de metadados e o desempenho da máquina e enviá-lo para o serviço.

### <a name="vmware-powercli-65"></a>VMware PowerCLI 6.5 

O módulo do powershell VMware PowerCLI tem de ser instalado para que o Recoletor possa comunicar com o servidor vCenter e a consulta para os detalhes da máquina e os respetivos dados de desempenho. O módulo do powershell automaticamente é transferido e instalado como parte da verificação de pré-requisitos. Transferência automática requer alguns URLs na lista de permissões, que precisam de fornecer está a falhar aceder, adicionar à lista branca-los, ou instalar manualmente o módulo.

Instale o módulo manualmente utilizando os seguintes passos:

1. Para instalar o PowerCli no Recoletor sem ligação à internet, siga os passos fornecidos na [esta ligação](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html) .
2. Depois de ter instalado o módulo do PowerShell num computador diferente, que tem acesso à internet, copie os ficheiros VMware.* do que a máquina para a máquina do Recoletor.
3. Reinicie as verificações de pré-requisitos e confirme que PowerCLI está instalado.

## <a name="connecting-to-vcenter-server"></a>A ligar ao vCenter Server

O Recoletor deve ligar ao vCenter Server e ser capaz de consultar as máquinas virtuais, os respetivos metadados e os respetivos contadores de desempenho. Estes dados são utilizados pelo projeto para calcular a avaliação.

1. Para ligar ao vCenter Server, uma conta de só de leitura com permissões como indicado na tabela seguinte pode ser utilizada para executar a deteção. 

    |Tarefa  |Necessária/conta da função  |Permissões  |
    |---------|---------|---------|
    |Deteção de recoletores de aplicação baseada em    | Precisa de, pelo menos, um utilizador só de leitura        |Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura         |

2. Podem ser acedidos apenas esses centros de dados que estão acessíveis para a conta do vCenter especificada para a deteção.
3. Tem de especificar o FQDN/endereço IP para ligar ao servidor vCenter do vCenter. Por predefinição, irá ligar através da porta 443. Se tiver configurado o vCenter escutar no número de porta diferente, pode especificá-lo como parte do endereço de servidor no formato IPAddress:Port_Number ou FQDN:Port_Number.
4. As definições de estatísticas do vCenter Server devem ser definidas ao nível 3 antes de começar a implementação. Se o nível for inferior a 3, a deteção será concluído, mas não serão possível recolher os dados de desempenho de armazenamento e rede. As recomendações de tamanho de avaliação neste caso, serão com base em dados de desempenho para a CPU e memória e apenas os dados de configuração de adaptadores de rede e de disco. [Leia mais](./concepts-collector.md) em que dados são recolhidos e como afeta a avaliação.
5. O Recoletor deve ter uma visão de of de linha de rede para o servidor vCenter.

> [!NOTE]
> Apenas vCenter Server versões 5.5, 6.0 e 6.5 oficialmente são suportadas.

> [!IMPORTANT]
> Recomendamos que defina o nível mais elevado do comuns (3) para o nível de estatísticas para que todos os contadores de são recolhidos corretamente. Se tiver vCenter definido num nível inferior, apenas alguns contadores podem ser recolhidos completamente, com o resto definido como 0. A avaliação, em seguida, poderá mostrar dados incompletos. 

### <a name="selecting-the-scope-for-discovery"></a>Selecionar o âmbito de deteção

Assim que estiver ligado ao vCenter, pode selecionar um âmbito de deteção. Selecionar um âmbito Deteta todas as máquinas virtuais a partir do caminho de inventário vCenter especificado.

1. O âmbito pode ser um centro de dados, uma pasta ou um anfitrião ESXi. 
2. Pode selecionar apenas um âmbito de cada vez. Para selecionar mais máquinas virtuais, pode concluir uma deteção e reinicie o processo de deteção com um novo âmbito.
3. Pode selecionar apenas um âmbito que tenha *menos de 1000 máquinas de virtuais*. Se selecionar um âmbito que tem mais de 1000 máquinas virtuais, precisa de dividir o âmbito em unidades mais pequenas através da criação de pastas. Em seguida, terá de executar deteções de independentes das pastas mais pequenas.

## <a name="specify-migration-project"></a>Especifique o projeto de migração

Depois do vCenter no local está ligado e um âmbito for especificado, agora pode especificar os detalhes do projeto de migração que têm de ser utilizados para deteção e de avaliação. Especifique o ID de projeto e a chave e ligar.

## <a name="start-discovery-and-view-collection-progress"></a>Iniciar a deteção e a vista de progresso de coleção

Assim que for iniciada a deteção, as máquinas virtuais do vCenter são detetadas e os respetivos dados de metadados e o desempenho são enviados para o servidor. O estado do progresso também informa-o de IDs os seguintes:

1. ID do recoletor: Um ID exclusivo para a máquina do Recoletor. Este ID de não alterar para uma determinada máquina por diferentes deteções. Pode utilizar este ID em caso de falhas ao reportar o problema ao Microsoft Support.
2. ID de sessão: Um ID exclusivo para a tarefa de coleção em execução. Pode consultar o mesmo ID de sessão no portal quando a tarefa de deteção é concluída. Este ID é alterado para cada trabalho de recolha. Em caso de falhas, pode reportar este ID ao Microsoft Support.

### <a name="what-data-is-collected"></a>Que dados são recolhidos?

A tarefa de recolha Deteta os metadados estático seguintes sobre as máquinas virtuais selecionadas. 

1. Nome a apresentar da VM (no vCenter)
2. Caminho de inventário da VM (anfitrião/pasta no vCenter)
3. Endereço IP
4. Endereço MAC
5. Número de núcleos, discos, NICs
6. RAM, tamanhos de disco
7. E os contadores de desempenho da VM, disco e rede, tal como indicado na tabela abaixo.

A tabela seguinte lista os contadores de desempenho que são recolhidos e também apresenta os resultados de avaliação são afetados se um determinado contador não é recolhido.

|Contador                                  |Nível    |Nível de por dispositivo  |Impacto de avaliação                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|cpu.usage.average                        | 1       |ND                |Tamanho da VM recomendada e o custo                    |
|mem.usage.average                        | 1       |ND                |Tamanho da VM recomendada e o custo                    |
|virtualDisk.read.average                 | 2       |2                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|virtualDisk.write.average                | 2       |2                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|net.received.average                     | 2       |3                 |Custo de tamanho e a rede VM                        |
|net.transmitted.average                  | 2       |3                 |Custo de tamanho e a rede VM                        |

> [!WARNING]
> Se tiver apenas um nível mais elevado de estatísticas,-irá demorar um dia para gerar os contadores de desempenho. Por isso, recomendamos que execute a deteção depois de um dia.

### <a name="time-required-to-complete-the-collection"></a>Tempo necessário para concluir a coleção

O Recoletor só Deteta os dados da máquina e envia-ao projeto. O projeto poderá demorar mais tempo antes dos dados detetados são apresentados no portal e pode iniciar a criação de uma avaliação.

Com base no número de máquinas virtuais no âmbito selecionado, que demora até 15 minutos para enviar os metadados estático para o projeto. Depois dos metadados estático estão disponível no portal, pode ver a lista de máquinas no portal e começar a criar grupos. Não é possível criar uma avaliação até que a tarefa de coleção é concluída e o projeto tem de processar os dados. Uma vez a tarefa de coleção foi concluída no Recoletor, pode demorar até uma hora para os dados de desempenho estar disponível no portal, com base no número de máquinas virtuais no âmbito selecionado.

## <a name="locking-down-the-collector-appliance"></a>Bloquear o dispositivo de recoletor
Recomendamos a execução contínuas atualizações do Windows no dispositivo de recoletor. Se um recoletor é atualizado não 45 dias, o recoletor irá iniciar auto-encerrar a máquina. Se estiver a executar uma deteção, a máquina será não ser desativada, mesmo se for passado o período de 45 dias. Após a tarefa de deteção estiver concluída, a máquina será desativada. Se estiver a utilizar o recoletor mais de 45 dias, é recomendável manter a máquina atualizada em todas as vezes por atualização do Windows em execução.

Também Recomendamos os seguintes passos para proteger a sua aplicação
1. Não partilhe ou misplace palavras-passe de administrador com partes não autorizadas.
2. Encerre a aplicação quando não está em utilização.
3. Colocar o dispositivo de uma rede protegida.
4. Assim que o trabalho de migração estiver concluído, elimine a instância de aplicação. Lembre-se de que também eliminar o disco fazer uma cópia de ficheiros (VMDKs), como os discos podem ter credenciais vCenter em cache nos mesmos.

## <a name="how-to-upgrade-collector"></a>Como atualizar o Recoletor

Pode atualizar o Recoletor para a versão mais recente sem a transferir o OVA novamente.

1. Transferir a versão mais recente [pacote de atualização](https://aka.ms/migrate/col/latestupgrade).
2. Certifique-se de que a correção transferida é segura, abra a janela de comandos de administrador e execute o seguinte comando para gerar o hash de ficheiro ZIP. O hash gerado deve corresponder com hash mencionado em relação a versão específica:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    
    (exemplo de utilização c:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.5.zip SHA256)
3. Copie o ficheiro zip para a Azure migração recoletor da máquina virtual (aplicação recoletor).
4. Clique com o botão direito no ficheiro zip e selecione extrair todas.
5. Clique com o botão direito no Setup.ps1 e selecionar executar com o PowerShell e siga as instruções no ecrã para instalar a atualização.

### <a name="list-of-updates"></a>Lista de atualizações

#### <a name="upgrade-to-version-1095"></a>Atualize para versão 1.0.9.5

Para atualizar para transferências de versão 1.0.9.5 [pacote](https://aka.ms/migrate/col/upgrade_9_5)

**Algoritmo** | **Valor de hash**
--- | ---
MD5 | d969ebf3bdacc3952df0310d8891ffdf
SHA1 | f96cc428eaa49d597eb77e51721dec600af19d53
SHA256 | 07c03abaac686faca1e82aef8b80e8ad8eca39067f1f80b4038967be1dc86fa1

## <a name="next-steps"></a>Passos Seguintes

[Configurar uma avaliação para VMs de VMware no local](tutorial-assessment-vmware.md)

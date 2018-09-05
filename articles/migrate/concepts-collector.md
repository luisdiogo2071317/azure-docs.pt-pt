---
title: Aplicação de recoletor no Azure Migrate | Documentos da Microsoft
description: Fornece uma descrição geral da aplicação Recoletora e como configurá-lo.
author: ruturaj
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 08/25/2018
ms.author: ruturajd
services: azure-migrate
ms.openlocfilehash: 74caf0ab052e1f6558dc20d15d84c01177b3f9cb
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665585"
---
# <a name="collector-appliance"></a>Aplicação recoletora

[O Azure Migrate](migrate-overview.md) avalia as cargas de trabalho no local para migração para o Azure. Este artigo fornece informações sobre como utilizar a aplicação Recoletora.



## <a name="overview"></a>Descrição geral

Um Recoletor do Azure Migrate é uma aplicação simples que pode ser utilizada para detetar o seu ambiente do vCenter no local. Esta aplicação Deteta máquinas do VMware no local e envia os metadados sobre as mesmas para o serviço Azure Migrate.

A aplicação Recoletora é um OVF que pode baixar do projeto do Azure Migrate. Ele cria uma instância de uma máquina virtual do VMware com 4 núcleos, 8 GB de RAM e um disco de 80 GB. O sistema operativo da aplicação é o Windows Server 2012 R2 (64 bits).

É possível criar o Recoletor ao seguir os passos aqui – [como criar a VM do Recoletor](tutorial-assessment-vmware.md#create-the-collector-vm).

## <a name="collector-communication-diagram"></a>Diagrama de comunicação do recoletor

![Diagrama de comunicação do recoletor](./media/tutorial-assessment-vmware/portdiagram.PNG)


| Componente      | Para comunicar com   | Porta necessária                            | Razão                                   |
| -------------- | --------------------- | ---------------------------------------- | ---------------------------------------- |
| Recoletor      | Serviço do Azure Migrate | TCP 443                                  | Recoletor deverá conseguir comunicar com o serviço através da porta SSL 443 |
| Recoletor      | vCenter Server        | Predefinição 443                             | Recoletor deve ser capaz de comunicar com o servidor vCenter. Ele se conecta ao vCenter em 443 por predefinição. Se o vCenter escuta numa porta diferente, essa porta deve estar disponível como porta de saída no recoletor |
| Recoletor      | RDP|   | TCP 3389 | Para que possa fazer o RDP à máquina do Recoletor |





## <a name="collector-pre-requisites"></a>Pré-requisitos do recoletor

O Recoletor tem de passar algumas verificações de pré-requisitos para garantir que se pode ligar o serviço Azure Migrate e carregar os dados detetados. Este artigo examina cada um dos pré-requisitos e compreender por que motivo é necessário.

### <a name="internet-connectivity"></a>Conectividade Internet

A aplicação recoletora precisa de estar ligado à internet para enviar as informações de máquinas detetadas. Pode ligar a máquina para a internet em uma das seguintes duas formas.

1. Pode configurar o Recoletor tem conectividade internet direta.
2. Pode configurar o Recoletor para ligar através de um servidor proxy.
    * Se o servidor proxy requer autenticação, pode especificar o nome de utilizador e palavra-passe nas definições de ligação.
    * Endereço de IP/FQDN do servidor Proxy deve ter o formato http://IPaddress ou http://FQDN. Proxy de http apenas é suportado.

> [!NOTE]
> Servidores proxy baseado em HTTPS não são suportadas pelo recoletor.

#### <a name="internet-connectivity-with-intercepting-proxy"></a>Ligação à Internet com a interceptação de proxy

Se o servidor de proxy a que utilizar para ligar à internet é um proxy de interceção, tem de importar o certificado de proxy para a sua VM do recoletor. Seguem-se passos sobre como pode importar o certificado para a VM do recoletor.

1. Na VM do recoletor, aceda a **Menu Iniciar** e localize e abra **gerir os certificados de computador**.
2. Na ferramenta de certificados, no painel esquerdo, sob **certificados - Computador Local**, localize **fabricantes fidedignos**. Sob **fabricantes fidedignos**, clique em **certificados** para ver a lista de certificados no painel à direita.

    ![Ferramenta de certificados](./media/concepts-intercepting-proxy/certificates-tool.png)

3. Copie o certificado de proxy para a VM do recoletor. Poderá ter de entrar em contacto com a equipe de administrador de rede na sua organização para obter este certificado.
4. Faça duplo clique no certificado para abri-lo. Clique em **instalar certificado**. Isto leva-o para o Assistente de importação de certificado.
5. No Assistente para importar certificados, para a localização de Store, escolha **computador Local**. **Clique em seguinte**.

    ![Localização do arquivo de certificados](./media/concepts-intercepting-proxy/certificate-store-location.png)

6. Escolha a opção para **colocar todos os certificados no seguinte arquivo**. Clique em **navegue** e selecione **fabricantes fidedignos** na lista de certificados que surgem. Clique em **Seguinte**.

    ![Arquivo de certificados](./media/concepts-intercepting-proxy/certificate-store.png)
    
7. Clique em **Concluir**. Isto irá importar o certificado. 
8. Opcionalmente, pode verificar que o certificado é importado ao abrir a ferramenta de certificados como no passo 1 e 2 acima.
9. Na aplicação de recoletor do Azure Migrate, verifique se que a verificação de pré-requisitos de conectividade de internet é efetuada com êxito.


#### <a name="whitelisting-urls-for-internet-connection"></a>URLs da lista de permissões para ligação à internet

A verificação de pré-requisitos é bem-sucedida se o coletor pode ligar à internet através das definições fornecidas. A verificação de conectividade é validada ao ligar a uma lista de URLs, como indicado na tabela seguinte. Se estiver a utilizar qualquer proxy de firewall baseado em URL para controlar a conectividade de saída, certifique-se a lista branca que estes URLs necessários:

**URL** | **Objetivo**  
--- | ---
*.portal.azure.com | Necessário para verificar a conectividade com o serviço do Azure e validar a sincronização de hora problemas.

Além disso, a verificação também tenta validar a conectividade com os seguintes URLs, mas não falha a verificação se não está acessível. Configurar a lista de permissões para os seguintes URLs é opcional, mas o que precisa de efetuar passos manuais para mitigar a verificação de pré-requisitos.

**URL** | **Objetivo**  | **E se não tiver a lista de permissões**
--- | --- | ---
*.oneget.org:443 | Necessário transferir o powershell com base vCenter PowerCLI módulo. | Falha de instalação do PowerCLI. Instale manualmente o módulo.
*.windows.net:443 | Necessário transferir o powershell com base vCenter PowerCLI módulo. | Falha de instalação do PowerCLI. Instale manualmente o módulo.
*.windowsazure.com:443 | Necessário transferir o powershell com base vCenter PowerCLI módulo. | Falha de instalação do PowerCLI. Instale manualmente o módulo.
*.powershellgallery.com:443 | Necessário transferir o powershell com base vCenter PowerCLI módulo. | Falha de instalação do PowerCLI. Instale manualmente o módulo.
*.msecnd.net:443 | Necessário transferir o powershell com base vCenter PowerCLI módulo. | Falha de instalação do PowerCLI. Instale manualmente o módulo.
*.visualstudio.com:443 | Necessário transferir o powershell com base vCenter PowerCLI módulo. | Falha de instalação do PowerCLI. Instale manualmente o módulo.

### <a name="time-is-in-sync-with-the-internet-server"></a>A hora está sincronizada com o servidor de internet

O Recoletor deve ser sincronizado com o servidor de horas da internet para garantir que os pedidos para o serviço são autenticados. A portal.azure.com url deve ser acessível a partir do Recoletor para que o tempo pode ser validado. Se a máquina não está sincronizada, terá de alterar a hora de relógio na VM do Recoletor de acordo com a hora atual, da seguinte forma:

1. Abra uma linha de comandos de administrador na VM.
1. Para verificar o fuso horário, execute w32tm /tz.
1. Para sincronizar a hora, execute w32tm /resync.

### <a name="collector-service-should-be-running"></a>Serviço do recoletor deve estar em execução

O serviço do Recoletor do Azure Migrate deve ser executado na máquina. Este serviço é iniciado automaticamente quando a máquina é inicializada. Se o serviço de mensagens em fila não está em execução, pode começar a *Recoletor do Azure Migrate* serviço através do painel de controlo. O serviço do Recoletor é responsável para ligar ao vCenter server, recolher dados de metadados e o desempenho da máquina e enviá-lo para o serviço.

### <a name="vmware-powercli-65"></a>VMware PowerCLI 6.5

O módulo do powershell do VMware PowerCLI tem de ser instalado para que o coletor pode comunicar com o servidor vCenter e a consulta para os detalhes da máquina e os seus dados de desempenho. O módulo do powershell é automaticamente transferido e instalado como parte da verificação de pré-requisitos. Transferência automática requer alguns URLs na lista de permissões, que precisa fornecer a falhar aceder por listas de permissões-los ou instalar manualmente o módulo.

Instale o módulo manualmente utilizando os seguintes passos:

1. Para instalar o PowerCli no Recoletor sem ligação à internet, siga os passos fornecidos na [esta ligação](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html) .
2. Depois de instalar o módulo do PowerShell num computador diferente, que tem acesso à internet, copie os ficheiros VMware.* desse computador para a máquina do Recoletor.
3. Reinicie as verificações de pré-requisitos e confirme se o PowerCLI está instalado.

## <a name="connecting-to-vcenter-server"></a>Ligar ao vCenter Server

O Recoletor deve ligar ao vCenter Server e ser capaz de consultar as máquinas virtuais, seus metadados e seus contadores de desempenho. Estes dados são utilizados pelo projeto para calcular uma avaliação.

1. Para ligar ao vCenter Server, pode servir-se uma conta só de leitura com permissões como indicado na tabela seguinte para executar a deteção.

    |Tarefa  |Conta/função necessária  |Permissões  |
    |---------|---------|---------|
    |Deteção de aplicação com base do recoletor    | Tem de, pelo menos, um utilizador só de leitura        |Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura         |

2. Podem ser acessados apenas os data Centers que sejam acessíveis à conta do vCenter especificada para a deteção.
3. Tem de especificar o endereço FQDN/IP para ligar ao servidor vCenter do vCenter. Por predefinição, irá ligar através da porta 443. Se tiver configurado o vCenter para escutar um número de porta diferente, pode especificá-lo como parte do endereço de servidor sob a forma IPAddress:Port_Number ou FQDN:Port_Number.
4. As definições de estatísticas do vCenter Server devem ser definidas como nível 3 antes de iniciar a implementação. Se o nível for inferior a 3, a deteção será concluída, mas não serão recolhidos dados de desempenho para armazenamento e rede. As recomendações de tamanho de avaliação em vez disso se baseará em dados de desempenho de CPU e memória e apenas os dados de configuração de adaptadores de rede e disco. [Leia mais](./concepts-collector.md) sobre os dados que são recolhidos e como elas impactam a avaliação.
5. O Recoletor deve ter uma rede linha Visual para o servidor vCenter.

> [!NOTE]
> Apenas vCenter Server versões 5.5, 6.0 e 6.5 são suportadas oficialmente.

> [!IMPORTANT]
> Recomendamos que defina o nível mais elevado do comuns (3) para o nível de estatísticas para que todos os contadores são recolhidos corretamente. Se tiver definido num nível inferior do vCenter, apenas alguns contadores podem ser recolhidas completamente, com o restante definido como 0. A avaliação, em seguida, poderá mostrar dados incompletos.

### <a name="selecting-the-scope-for-discovery"></a>Selecionar o âmbito de deteção

Assim que estiver ligado ao vCenter, pode selecionar um âmbito para descobrir. Selecionar um âmbito Deteta todas as máquinas de virtuais a partir do caminho de inventário do vCenter especificado.

1. O âmbito pode ser um Data Center, uma pasta ou um anfitrião ESXi.
2. Só pode selecionar um âmbito de cada vez. Para selecionar mais máquinas virtuais, pode concluir a deteção de um e reiniciar o processo de deteção com um novo âmbito.
3. Só pode selecionar um âmbito que tenha *menos de 1500 máquinas de virtuais*.

## <a name="specify-migration-project"></a>Especificar o projeto de migração

Assim que o vCenter no local está ligado e um âmbito for especificado, pode especificar os detalhes do projeto de migração que têm de ser utilizados para deteção e avaliação. Especifique o ID do projeto e a chave e ligar.

## <a name="start-discovery-and-view-collection-progress"></a>Iniciar a deteção e Ver progresso da coleção

Assim que for iniciado a deteção, as máquinas de virtuais de vCenter são detetadas e seus dados de metadados e o desempenho são enviados para o servidor. O estado do progresso também informa sobre os seguintes IDs de:

1. ID do recoletor: Um ID exclusivo que é atribuído à sua máquina de Recoletor. Este ID não é alterado para um determinado computador diferentes deteções. Pode utilizar este ID em caso de falhas ao reportar o problema ao Support da Microsoft.
2. ID de sessão: Um ID exclusivo para a tarefa de coleção em execução. Pode consultar o mesmo ID de sessão no portal do quando a tarefa de deteção estiver concluída. Este ID é alterado para todas as tarefas de recolha. Em caso de falhas, pode denunciar este ID Support da Microsoft.

### <a name="what-data-is-collected"></a>Que dados são recolhidos?

O trabalho de recolha Deteta os seguintes metadados estático sobre as máquinas virtuais selecionadas.

1. Nome a apresentar da VM (no vCenter)
2. Caminho de inventário da VM (anfitrião/pasta no vCenter)
3. Endereço IP
4. Endereço MAC
5. Sistema operativo
5. Número de núcleos, discos, NICs
6. Tamanho da memória, tamanhos de disco
7. E contadores de desempenho da VM, disco e rede, tal como indicado na tabela abaixo.

A tabela seguinte lista os contadores de desempenho que são recolhidas e também apresenta uma lista de resultados da avaliação que são afetados se um contador específico não é coletado.

|Contador                                  |Nível    |Nível de por dispositivo  |Impacto da avaliação                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|cpu.usage.average                        | 1       |ND                |Tamanho VM recomendados e os custos                    |
|Mem.Usage.Average                        | 1       |ND                |Tamanho VM recomendados e os custos                    |
|virtualDisk.read.average                 | 2       |2                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|virtualDisk.write.average                | 2       |2                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|NET.Received.Average                     | 2       |3                 |Custo de tamanho e a rede VM                        |
|net.transmitted.average                  | 2       |3                 |Custo de tamanho e a rede VM                        |

> [!WARNING]
> Se apenas tiver definido um nível mais elevado de estatísticas, ele irá demorar um dia para gerar os contadores de desempenho. Por isso, recomendamos que execute a deteção após um dia.

### <a name="time-required-to-complete-the-collection"></a>Tempo necessário para concluir a recolha

O Recoletor só Deteta os dados da máquina e envia-os ao projeto. O projeto pode demorar mais tempo antes dos dados detetados são apresentados no portal e pode começar a criar uma avaliação.

Com base no número de máquinas virtuais no âmbito selecionado, demora até 15 minutos para enviar os metadados de estático para o projeto. Depois dos metadados estático estão disponível no portal, pode ver a lista de máquinas no portal e começar a criar grupos. Não é possível criar uma avaliação até que a tarefa de coleção é concluída e o projeto processou os dados. Uma vez a tarefa de coleção foi concluída no Recoletor, pode demorar até uma hora para os dados de desempenho estejam disponíveis no portal, com base no número de máquinas virtuais no âmbito selecionado.

## <a name="locking-down-the-collector-appliance"></a>Bloquear a aplicação recoletora
Recomendamos a execução contínuas atualizações do Windows em que a aplicação recoletora. Se um recoletor não forem atualizado durante 60 dias, o recoletor será iniciado automaticamente-encerrar a máquina. Se estiver a executar uma deteção, a máquina será não ser desativada, mesmo que seja seu período de 60 dias. A tarefa de deteção de publicação estiver concluída, a máquina será desativada. Se estiver a utilizar o recoletor por mais de 45 dias, é recomendável manter a máquina de atualização em todo o tempo pela atualização do Windows em execução.

Também Recomendamos os seguintes passos para proteger a sua aplicação
1. Não partilhe ou perdas de palavras-passe de administrador com partes não autorizadas.
2. Encerre a aplicação quando não está em utilização.
3. Coloca a aplicação numa rede segura.
4. Quando o trabalho de migração estiver concluído, elimine a instância de aplicação. Certifique-se de que também eliminar o disco de ficheiros (VMDKs), de segurança, como os discos podem ter as credenciais do vCenter em cache nos mesmos.

## <a name="how-to-upgrade-collector"></a>Como atualizar o Recoletor

Pode atualizar o Recoletor para a versão mais recente sem baixar o OVA mais uma vez.

1. Baixe a versão mais recente [pacote de atualização](https://aka.ms/migrate/col/upgrade_9_14) (versão 1.0.9.14).
2. Para garantir que a correção transferida é segura, abra a janela de comando de administrador e execute o seguinte comando para gerar o hash para o ficheiro ZIP. O hash gerado deve corresponder com o hash mencionado em relação a versão específica:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (exemplo de utilização c:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)
3. Copie o ficheiro zip para a máquina de virtual do recoletor de Azure Migrate (aplicação recoletora).
4. Com o botão direito no ficheiro zip e selecione extrair tudo.
5. Com o botão direito no Setup.ps1 e selecionar executar com o PowerShell e siga as instruções no ecrã para instalar a atualização.

### <a name="list-of-updates"></a>Lista de atualizações

#### <a name="upgrade-to-version-10914"></a>Atualizar para versão 1.0.9.14

Valores para a atualização de hash [1.0.9.14 do pacote](https://aka.ms/migrate/col/upgrade_9_14)

**Algoritmo** | **Valor de hash**
--- | ---
MD5 | c5bf029e9fac682c6b85078a61c5c79c
SHA1 | af66656951105e42680dfcc3ec3abd3f4da8fdec
SHA256 | 58b685b2707f273aa76f2e1d45f97b0543a8c4d017cd27f0bdb220e6984cc90e

#### <a name="upgrade-to-version-10913"></a>Atualizar para versão 1.0.9.13

Valores para a atualização de hash [1.0.9.13 do pacote](https://aka.ms/migrate/col/upgrade_9_13)

**Algoritmo** | **Valor de hash**
--- | ---
MD5 | 739f588fe7fb95ce2a9b6b4d0bf9917e
SHA1 | 9b3365acad038eb1c62ca2b2de1467cb8eed37f6
SHA256 | 7a49fb8286595f39a29085534f29a623ec2edb12a3d76f90c9654b2f69eef87e

#### <a name="upgrade-to-version-10911"></a>Atualizar para versão 1.0.9.11

Valores para a atualização de hash [1.0.9.11 do pacote](https://aka.ms/migrate/col/upgrade_9_11)

**Algoritmo** | **Valor de hash**
--- | ---
MD5 | 0e36129ac5383b204720df7a56b95a60
SHA1 | aa422ef6aa6b6f8bc88f27727e80272241de1bdf
SHA256 | 5f76dbbe40c5ccab3502cc1c5f074e4b4bcbf356d3721fd52fb7ff583ff2b68f

#### <a name="upgrade-to-version-1097"></a>Atualizar para a versão ova 1.0.9.7

Valores para a atualização de hash [1.0.9.7 do pacote](https://aka.ms/migrate/col/upgrade_9_7)

**Algoritmo** | **Valor de hash**
--- | ---
MD5 | 01ccd6bc0281f63f2a672952a2a25363
SHA1 | 3e6c57523a30d5610acdaa14b833c070bffddbff
SHA256 | e3ee031fb2d47b7881cc5b13750fc7df541028e0a1cc038c796789139aa8e1e6

## <a name="next-steps"></a>Passos Seguintes

[Configurar uma avaliação para VMs de VMware no local](tutorial-assessment-vmware.md)

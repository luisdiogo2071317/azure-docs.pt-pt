---
title: Sobre a aplicação Recoletora no Azure Migrate | Documentos da Microsoft
description: Fornece informações sobre a aplicação Recoletora no Azure Migrate.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: snehaa
services: azure-migrate
ms.openlocfilehash: 81e6731068db84f02073f02c49bea9a8fb7c7c70
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241196"
---
# <a name="about-the-collector-appliance"></a>Sobre a aplicação Recoletora

 Este artigo fornece informações sobre o Recoletor do Azure Migrate.

Recoletor do Azure Migrate é uma aplicação simples que pode ser utilizado para detetar um ambiente de vCenter no local para fins de avaliação com o [do Azure Migrate](migrate-overview.md) serviço específico, antes da migração para o Azure.  

## <a name="discovery-methods"></a>Métodos de deteção

Existem duas opções para a aplicação de Recoletor, deteção única ou deteção contínua.

### <a name="one-time-discovery"></a>Deteção única

A aplicação Recoletora comunica de forma única com o vCenter Server para recolher os metadados sobre as VMs. Através deste método:

- A aplicação da continuamente não está ligada ao projeto do Azure Migrate.
- Alterações no ambiente no local não são refletidas no Azure Migrate, após a conclusão da deteção. Para refletir as alterações, precisa descobrir novamente o mesmo ambiente no mesmo projeto.
- Aquando da recolha de dados de desempenho para uma VM, a aplicação da conta com os dados de desempenho do histórico armazenados no vCenter Server. Recolhe o histórico de desempenho para o último mês.
- Para a recolha de dados de desempenho históricos, terá de configurar as definições de estatísticas no vCenter Server para o nível de três. Depois de definir o nível a três, terá de aguardar pelo menos um dia para o vCenter recolher contadores de desempenho. Como tal, recomendamos que execute a deteção depois de, pelo menos, um dia. Se pretender avaliar o ambiente com base nos dados de desempenho de uma semana ou de 1 mês, terá de esperar em conformidade.
- Neste método de deteção, o Azure Migrate recolhe contadores médios para cada métrica (em vez de contadores de pico) que podem resultar em dimensionamento insuficientemente. Recomendamos que utilize a opção de deteção contínua para obter mais precisos, os resultados de dimensionamento.

### <a name="continuous-discovery"></a>Deteção contínua

A aplicação Recoletora fica continuamente conectada ao projeto do Azure Migrate e continuamente recolhe dados de desempenho de VMs.

- O Recoletor perfis continuamente o ambiente no local para recolher dados de utilização em tempo real a cada 20 segundos.
- A aplicação agrega os exemplos de 20 segundos e cria um único ponto de dados a cada 15 minutos.
- Para criar os dados ponto a aplicação da seleciona o valor de pico de exemplos de 20 segundos e envia-os para o Azure.
- Este modelo não depende das definições de estatísticas do vCenter Server para recolher dados de desempenho.
- Pode parar contínua de criação de perfis em qualquer altura do Recoletor.

Tenha em atenção que a aplicação recolhe apenas dados de desempenho continuamente, não deteta qualquer alteração de configuração no ambiente no local (ou seja, adição de VM, eliminação, adição de disco, etc.). Se houver uma alteração de configuração no ambiente no local, pode fazer o seguinte para refletir as alterações no portal:

- Adição de itens (VMs, discos, núcleos, etc.): para refletir estas alterações no portal do Azure, pode parar a deteção a partir da aplicação e, em seguida, iniciá-la novamente. Isto irá garantir que as alterações são atualizadas no projeto do Azure Migrate.

- Eliminação das VMs: devido à forma como a aplicação foi concebida, a eliminação de VMs não será refletida, mesmo se parar e iniciar a deteção. Isto acontece porque os dados das deteções subsequentes são anexados às deteções mais antigas e não são substituídos. Neste caso, pode simplesmente ignorar a VM no portal, ao removê-la do seu grupo e recalcular a avaliação.

> [!NOTE]
> A funcionalidade de deteção contínua está em pré-visualização. Recomendamos que utilize este método, porque este recolhe dados de desempenho granular e resulta num dimensionamento preciso.

## <a name="deploying-the-collector"></a>Implementar o Recoletor

Implementar a aplicação Recoletora através de um modelo OVF:

- Transfira o modelo OVF a partir de um projeto do Azure Migrate no portal do Azure. Importar o ficheiro transferido para o vCenter Server, para configurar a aplicação Recoletora VM.
- Do OVF, VMware define uma VM com 4 núcleos, 8 GB de RAM e um disco de 80 GB. O sistema operacional é Windows Server 2012 R2 (64 bits).
- Ao executar o Recoletor, execute um número de verificações de pré-requisitos para se certificar de que o coletor pode ligar ao Azure Migrate.

- [Saiba mais](tutorial-assessment-vmware.md#create-the-collector-vm) sobre como criar o Recoletor.


## <a name="collector-prerequisites"></a>Pré-requisitos do recoletor

O Recoletor tem de passar algumas verificações de pré-requisitos para garantir que pode estabelecer ligação ao serviço Azure Migrate através da internet e detetados de carregamento de dados.

- **Verifique a ligação de internet**: O Recoletor pode ligar à internet diretamente ou através de um proxy.
    - A verificação de pré-requisitos verifica a conetividade à [URLs obrigatórios e opcionais](#connect-to-urls).
    - Se tiver uma ligação direta à internet, nenhuma ação específica é necessária, que não seja de certificar-se de que o coletor de contactar os URLs necessários.
    - Se estiver a ligar através de um proxy, tenha em atenção a [os requisitos abaixo](#connect-via-a-proxy).
- **Certifique-se a sincronização de hora**: O Recoletor devem sincronizados com o servidor de horas da internet para garantir que os pedidos para o serviço são autenticados.
    - A portal.azure.com url deve ser acessível a partir do Recoletor para que o tempo pode ser validado.
    - Se a máquina não está sincronizada, terá de alterar a hora de relógio na VM do Recoletor de acordo com a hora atual. Para fazer isso abra uma linha de administrador na VM, execute **w32tm /tz** para verificar o fuso horário. Execute **w32tm /resync** para sincronizar a hora.
- **Verificar a execução do serviço de recoletor**: serviço do Recoletor do Azure de Migrate deve estar em execução na VM do Recoletor.
    - Este serviço é iniciado automaticamente quando a máquina é inicializada.
    - Se o serviço não está em execução, inicie-o do painel de controle.
    - O serviço do Recoletor liga-se ao vCenter Server, recolhe os dados de metadados e o desempenho da VM e envia-os para o serviço Azure Migrate.
- **Verificar o VMware PowerCLI 6.5 instalado**: módulo o VMware PowerCLI 6.5 do PowerShell tem de estar instalado na VM do Recoletor, para que ele possa comunicar com o vCenter Server.
    - Se o coletor pode aceder aos URLs necessários para instalar o módulo, é instalar automaticamente durante a implementação do Recoletor.
    - Se o Recoletor não é possível instalar o módulo durante a implementação, deve [instalá-lo manualmente](#install-vwware-powercli-module-manually).
- **Verifique a ligação ao vCenter Server**: O Recoletor tem de ser capaz de servidor vCenter e consulta por VMs, metadados e contadores de desempenho. [Verificar pré-requisitos](#connect-to-vcenter-server) para ligar.


### <a name="connect-to-the-internet-via-a-proxy"></a>Ligar à internet através de um proxy

- Se o servidor proxy requer autenticação, pode especificar o nome de utilizador e palavra-passe quando configurou o Recoletor.
- Endereço de IP/FQDN do servidor Proxy deve especificado como *http://IPaddress* ou *http://FQDN*.
- Apenas é suportado o proxy HTTP. Servidores proxy baseado em HTTPS não são suportados pelo Recoletor.
- Se o servidor proxy é um proxy de interceção, tem de importar o certificado de proxy para a VM do Recoletor.
    1. Na VM do recoletor, aceda a **Menu Iniciar** > **gerir certificados de computador**.
    2. Na ferramenta de certificados, sob **certificados - Computador Local**, localize **fabricantes fidedignos** > **certificados**.

        ![Ferramenta de certificados](./media/concepts-intercepting-proxy/certificates-tool.png)

    3. Copie o certificado de proxy para a VM do recoletor. Poderá ter de obtê-lo a partir do seu administrador de rede.
    4. Clique duas vezes para abrir o certificado e clique em **instalar certificado**.
    5. No Assistente para importar certificados > localização da Store, escolha **computador Local**.

    ![Localização do arquivo de certificados](./media/concepts-intercepting-proxy/certificate-store-location.png)

    6. Selecione **colocar todos os certificados no seguinte arquivo** > **procurar** > **fabricantes fidedignos**. Clique em **concluir** para importar o certificado.

    ![Arquivo de certificados](./media/concepts-intercepting-proxy/certificate-store.png)

    7. Verifique que o certificado é importado conforme esperado e verifique que a verificação de pré-requisitos a conectividade internet funcionar conforme esperado.




### <a name="connect-to-urls"></a>Ligar a URLs

A verificação de conectividade é validada ao ligar a uma lista de URLs.

**URL** | **Detalhes**  | **Verificação de pré-requisitos**
--- | --- | ---
*.portal.azure.com | Verificações de conectividade com o serviço do Azure e a sincronização de hora. | Acesso a URL é necessário.<br/><br/> A verificação de pré-requisitos falha se nenhuma conectividade.
*.oneget.org:443<br/><br/> *.windows.net:443<br/><br/> *.windowsazure.com:443<br/><br/> *.powershellgallery.com:443<br/><br/> *.msecnd.net:443<br/><br/> *.visualstudio.com:443| Utilizado para transferir o módulo do PowerShell vCenter PowerCLI. | Acesso a URLs opcionais.<br/><br/> Verificação de pré-requisitos não falhará.<br/><br/> Instalação do módulo automática na VM do Recoletor irá falhar. Terá de instalar manualmente o módulo.


### <a name="install-vmware-powercli-module-manually"></a>Instalar manualmente o módulo de VMware PowerCLI

1. Instala o módulo usando [essas etapas](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html). Estes passos descrevem a instalação tanto online quanto offline.
2. Se a VM do Recoletor está offline e instalar o módulo num computador diferente com acesso à internet, terá de copiar os ficheiros de VMware.* desse computador para a VM do Recoletor.
3. Após a instalação, pode reiniciar as verificações de pré-requisitos para confirmar se o PowerCLI está instalado.

### <a name="connect-to-vcenter-server"></a>Ligar ao vCenter Server

O Recoletor liga-se ao vCenter Server e consultas de metadados da VM e contadores de desempenho. Eis o que precisa para a ligação.

- Apenas vCenter Server 5.5, 6.0 e 6.5 são suportadas as versões.
- Precisa de uma conta só de leitura com as permissões resumidas abaixo para a deteção. Pode ser acessados apenas acessíveis com a conta de datacenters para a deteção.
- Por predefinição, vai ligar ao vCenter Server com um endereço IP ou FQDN. Se o vCenter Server escuta numa porta diferente, se conectar a ela utilizando a forma *IPAddress:Port_Number* ou *FQDN:Port_Number*.
- Para recolher dados de desempenho de rede e armazenamento, as definições de estatísticas do vCenter Server pode ser definido como nível três.
- Se o nível é menor do que funciona a deteção de três, mas não serão recolhidos os dados de desempenho. Alguns contadores podem ser recolhidas, mas outros será definido como zero.
- Se não são recolhidos dados de desempenho de rede e armazenamento, recomendações de tamanho de avaliação são dados de desempenho com base para a CPU e memória e nos dados de configuração para os adaptadores de rede e disco.
- O Recoletor deve ter uma rede linha Visual para o servidor vCenter.

#### <a name="account-permissions"></a>Permissões de conta

**Conta** | **Permissões**
--- | ---
Pelo menos uma conta de utilizador só de leitura | Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura   


## <a name="collector-communications"></a>Comunicações de recoletor

O recoletor comunica conforme resumido no diagrama e a tabela seguinte.

![Diagrama de comunicação do recoletor](./media/tutorial-assessment-vmware/portdiagram.PNG)


**Recoletor comunica com** | **Porta** | **Detalhes**
--- | --- | ---
Serviço do Azure Migrate | TCP 443 | Recoletor se comunica com o serviço Azure Migrate através de SSL 443.
vCenter Server | TCP 443 | O Recoletor tem de ser capaz de comunicar com o vCenter Server.<br/><br/> Por padrão, ele se conecta ao vCenter em 443.<br/><br/> Se o vCenter Server escuta numa porta diferente, essa porta deve estar disponível como porta de saída no Recoletor.
RDP | TCP 3389 |


## <a name="securing-the-collector-appliance"></a>Proteger a aplicação Recoletora

Recomendamos os seguintes passos para proteger a aplicação Recoletora:

- Não partilhe ou perdas de palavras-passe de administrador com partes não autorizadas.
- Encerre a aplicação quando não está em utilização.
- Coloca a aplicação numa rede segura.
- Após a migração estiver concluída, elimine a instância da aplicação.
- Além disso, após a migração, também elimine os ficheiros de cópia de segurança de discos (VMDKs), como os discos podem ter as credenciais do vCenter em cache nos mesmos.

## <a name="os-license-in-the-collector-vm"></a>Licença de SO na VM do recoletor

O recoletor vem com uma licença de avaliação do Windows Server 2012 R2 que é válida por 180 dias. Se o período de avaliação está prestes a expirar para a sua VM do recoletor, recomenda-se para transferir uma nova OVA e criar um novo dispositivo.

## <a name="updating-the-os-of-the-collector-vm"></a>A atualizar o SO da VM do Recoletor

Apesar da aplicação recoletora ter uma licença de avaliação de 180 dias, precisa atualizar continuamente o sistema operacional na aplicação para evitar desligar automaticamente para baixo da aplicação.

- Se o Recoletor não é atualizado nos últimos 60 dias, este começa a encerrar a máquina automaticamente.
- Se estiver a executar uma deteção, a máquina não ser desativada, mesmo se passaram 60 dias. A máquina será desativada depois de concluída a deteção.
- Se já usou o Recoletor durante mais de 60 dias, recomendamos que mantenha a máquina de atualização em todo o tempo pela atualização do Windows em execução.

## <a name="upgrading-the-collector-appliance-version"></a>Atualizar a versão da aplicação de Recoletor

Pode atualizar o Recoletor para a versão mais recente sem baixar o OVA novamente.

1. Transferir o [mais recente listado o pacote de atualização](concepts-collector-upgrade.md)
2. Para garantir que a correção transferida é segura, abra a janela de comando de administrador e execute o seguinte comando para gerar o hash para o ficheiro ZIP. O hash gerado deve corresponder com o hash mencionado em relação a versão específica:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (exemplo de utilização c:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)
3. Copie o ficheiro zip para a máquina de virtual do recoletor de Azure Migrate (aplicação recoletora).
4. Com o botão direito no ficheiro zip e selecione extrair tudo.
5. Com o botão direito no Setup.ps1 e selecionar executar com o PowerShell e siga as instruções no ecrã para instalar a atualização.

## <a name="discovery-process"></a>Processo de deteção

Depois de configurar a aplicação, pode executar a deteção. Eis como funciona:

- Executar uma deteção por âmbito. Todas as VMs no caminho de inventário do vCenter especificado serão detetadas.
    - Definir um âmbito de cada vez.
    - O âmbito pode incluir 1500 VMs ou menos.
    - O âmbito pode ser um centro de dados, uma pasta ou um anfitrião ESXi.
- Depois de ligar ao vCenter Server, se conectar ao especificar um projeto de migração para a coleção.
- Deteção das VMs e seus dados de desempenho e de metadados são enviados para o Azure. Estas ações fazem parte de uma tarefa de coleção.
    - A aplicação Recoletora é atribuída um ID específico do Recoletor que é persistente para um determinado computador entre deteções.
    - Uma tarefa em execução é fornecida um ID de sessão específicos. O ID é alterado para cada tarefa de coleção e pode ser utilizado para resolução de problemas.

### <a name="collected-metadata"></a>Metadados recolhidos

A aplicação recoletora Deteta os seguintes metadados estático para VMs:

- Nome de exibição VM (no vCenter Server)
- Caminho de inventário da VM (o anfitrião/pasta no vCenter Server)
- Endereço IP
- Endereço MAC
- Sistema operativo
- Número de núcleos, discos, NICs
- Tamanho da memória, tamanhos de disco
- Contadores de desempenho da VM, disco e rede.

#### <a name="performance-counters"></a>Contadores de desempenho

- **Deteção única**: ao contadores são recolhidos para uma deteção única, tenha em atenção o seguinte:

    - Pode demorar até 15 minutos para recolher e enviar metadados de configuração para o projeto.
    - Depois de recolhidos, os dados de configuração pode demorar até uma hora para dados de desempenho estejam disponíveis no portal.
    - Depois dos metadados estão disponível no portal, é apresentada a lista de VMs e pode começar a criar grupos para avaliação.
- **Deteção contínua**: para a deteção contínua, tenha em atenção o seguinte:
    - Dados de configuração para a VM estão disponíveis uma hora depois de Iniciar deteção
    - Dados de desempenho começaram a se tornarem disponíveis após 2 horas.
    - Depois de iniciar a deteção, aguarde pelo menos um dia para a aplicação para criar um perfil do ambiente, antes de criar avaliações.

**Contador** | **Nível** | **Nível de por dispositivo** | **Impacto na avaliação**
--- | --- | --- | ---
cpu.usage.average | 1 | ND | Tamanho VM recomendados e os custos  
Mem.Usage.Average | 1 | ND | Tamanho VM recomendados e os custos  
virtualDisk.read.average | 2 | 2 | Calcula o tamanho do disco, o custo de armazenamento, o tamanho da VM
virtualDisk.write.average | 2 | 2  | Calcula o tamanho do disco, o custo de armazenamento, o tamanho da VM
virtualDisk.numberReadAveraged.average | 1 | 3 |  Calcula o tamanho do disco, o custo de armazenamento, o tamanho da VM
virtualDisk.numberWriteAveraged.average | 1 | 3 |   Calcula o tamanho do disco, o custo de armazenamento, o tamanho da VM
NET.Received.Average | 2 | 3 |  Calcula o tamanho da VM                          |
net.transmitted.average | 2 | 3 | Calcula o tamanho da VM     

## <a name="next-steps"></a>Passos Seguintes

[Configurar uma avaliação para VMs de VMware no local](tutorial-assessment-vmware.md)

---
title: Criar o integration runtime autoalojado no Azure Data Factory | Documentos da Microsoft
description: Saiba como criar integration runtime autoalojado no Azure Data Factory, que permite que as fábricas de dados acessar arquivos de dados de uma rede privada.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 863cda349ca951bee1c43f09eefd364645de9dbd
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746806"
---
# <a name="how-to-create-and-configure-self-hosted-integration-runtime"></a>Como criar e configurar o Runtime de integração autoalojado
O Runtime de integração (IR) é a infraestrutura de computação utilizada pelo Azure Data Factory para fornecer capacidades de integração de dados em diferentes ambientes de rede. Para obter detalhes sobre o runtime de integração, consulte [descrição geral do Runtime de integração](concepts-integration-runtime.md).

Um runtime de integração autoalojado é capaz de executar atividades de cópia entre os dados na cloud recursos num local de computação de arquivos e um arquivo de dados numa rede privada e a distribuição de atividades de transformação ou de rede Virtual do Azure. Instale as necessidades de runtime de integração autoalojado numa máquina no local ou uma máquina virtual dentro de uma rede privada.  

Este documento apresenta como pode criar e configurar a ir de autoalojado.

## <a name="high-level-steps-to-install-self-hosted-ir"></a>Passos de alto nível para instalar o runtime de integração autoalojado
1. Crie um runtime de integração autoalojado. Pode usar a interface do Usuário do ADF para criar o ir autoalojado. Eis um exemplo do PowerShell:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
    ```
2. Transferir e instalar o runtime de integração autoalojado (na máquina local).
3. Obter a chave de autenticação e registar o runtime de integração autoalojado com a chave. Eis um exemplo do PowerShell:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime.  
    ```

## <a name="setting-up-self-hosted-ir-on-azure-vm-using-azure-resource-manager-template-automatation"></a>Configuração do Runtime de integração autoalojado na VM do Azure com o modelo do Resource Manager do Azure (automatation)
Pode automatizar a configuração do Runtime de integração autoalojado sobre como utilizar uma VM do Azure [este modelo do Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vms-with-selfhost-integration-runtime). Isso fornece uma forma fácil de ter um ir Autoalojado totalmente funcional na VNet do Azure com a funcionalidade de escalabilidade e elevada Avalaibility (desde que definir a contagem de nós para ser 2 ou superior).

## <a name="command-flow-and-data-flow"></a>Fluxo de comando e fluxo de dados
Ao mover os dados entre aplicações no local e na cloud, a atividade utiliza um runtime de integração autoalojado para transferir os dados da origem de dados no local para a cloud e vice-versa.

Este é um fluxo de dados de alto nível para o resumo das etapas de cópia com o runtime de integração autoalojado:

![Descrição geral de alto nível](media\create-self-hosted-integration-runtime\high-level-overview.png)

1. Programadores de dados cria um runtime de integração autoalojado dentro de uma fábrica de dados do Azure com um cmdlet do PowerShell. Atualmente, o portal do Azure não suporta esta funcionalidade.
2. Programadores de dados cria um serviço ligado para um arquivo de dados no local, especificando a instância do runtime de integração autoalojado que ele deve utilizar para ligar aos arquivos de dados. Como parte da configuração de serviço ligado, os programadores de dados utiliza a aplicação do Gestor de credenciais (atualmente, não suportado) para a definição de tipos de autenticação e credenciais. A caixa de diálogo de aplicativo do Credential manager comunica com o arquivo de dados para testar a ligação e o runtime de integração autoalojado para guardar as credenciais.
   - Nó do runtime de integração autoalojado encripta a credencial a utilizar o Windows dados DPAPI Protection Application Programming Interface () e o salva localmente. Se vários nós forem definidos para elevada disponibilidade, as credenciais são ainda mais sincronizadas em todos os outros nós. Cada nó criptografa com a interface DPAPI e armazena-o localmente. Sincronização de credenciais é transparente para o desenvolvedor de dados e é manipulada por ir autoalojado.    
   - Serviço do Data Factory se comunica com o runtime de integração autoalojado para agendamento e gestão de tarefas via **canal de controlo** que utiliza uma fila de barramento de serviço do Azure partilhado. Quando uma tarefa de atividade tem de ser executado, Data Factory coloca em fila o pedido, juntamente com informações de credenciais (caso as credenciais já não estão armazenadas no runtime de integração autoalojado). Runtime de integração autoalojado inicia a tarefa depois da fila de consulta.
   - Runtime de integração autoalojado copia dados de um arquivo no local para um armazenamento na cloud, ou vice versa dependendo da configuração a atividade de cópia no pipeline de dados. Para este passo, o runtime de integração autoalojado se comunica diretamente com os serviços de armazenamento baseado na nuvem, como o armazenamento de Blobs do Azure através de um canal de seguro (HTTPS).

## <a name="considerations-for-using-self-hosted-ir"></a>Considerações sobre como utilizar o runtime de integração autoalojado

- Um runtime de integração autoalojado único pode ser utilizado para várias origens de dados no local. No entanto, uma **único integration runtime autoalojado está ligado à fábrica de dados do Azure apenas um** e não pode ser partilhado com outro data factory.
- Pode ter **apenas uma instância do integration runtime autoalojado** instalado numa única máquina. Suponha, tem duas fábricas de dados que precisam de aceder a origens de dados no local, terá de instalar o integration runtime autoalojado em dois computadores no local. Em outras palavras, um runtime de integração autoalojado está ligado a uma fábrica de dados específico
- O **integration runtime autoalojado não precisa de estar no mesmo computador, como a origem de dados**. No entanto, o mais perto da origem de dados ter integration runtime autoalojado reduz o tempo para o runtime de integração autoalojado ligar à origem de dados. Recomendamos que instale o runtime de integração autoalojado num computador que é diferente dessa origem de dados de locais de anfitriões. Quando a origem de dados e do runtime de integração autoalojado em computadores diferentes, o runtime de integração autoalojado não compitam por recursos com a origem de dados.
- Pode ter **vários runtimes de integração autoalojado em computadores diferentes, ligar à mesma origem de dados no local**. Por exemplo, talvez tenha dois runtime de integração autoalojado que serve a duas fábricas de dados, mas a mesma origem de dados no local é registrada em ambas as fábricas de dados.
- Se já tiver um gateway instalado no seu serviço de computador uma **Power BI** cenário, instale um **separado integration runtime autoalojado do Azure Data Factory** em outra máquina.
- Runtime de integração autoalojado tem de ser utilizado para dar suporte a integração de dados dentro da rede Virtual do Azure.
- Tratar a sua origem de dados como uma origem de dados no local (que é protegido por uma firewall), mesmo quando usar **ExpressRoute**. Utilize o runtime de integração autoalojado para estabelecer a conectividade entre o serviço e a origem de dados.
- Tem de utilizar o runtime de integração autoalojado, mesmo que o arquivo de dados está na cloud numa **máquina virtual IaaS do Azure**.
- Tarefas poderão falhar no Integration Runtime autoalojado instalado num servidor do Windows no qual compatíveis com FIPS encriptação está ativada. Para contornar este problema, desative a encriptação compatível com FIPS no servidor. Para desativar a encriptação compatível com FIPS, altere o valor de registo seguinte de 1 (ativado) para 0 (desativado): `HKLM\System\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy\Enabled`.

## <a name="prerequisites"></a>Pré-requisitos

- O suporte **sistema operativo** versões são o Windows 7 Service Pack 1, Windows 8.1, Windows 10, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016. Instalação do runtime de integração autoalojado numa **controlador de domínio não é suportado**.
- **.NET framework 4.6.1 ou acima** é necessária. Se estiver a instalar o integration runtime autoalojado num computador Windows 7, instalar o .NET Framework 4.6.1 ou posterior. Ver [requisitos de sistema do .NET Framework](/dotnet/framework/get-started/system-requirements) para obter detalhes.
- O recomendado **configuração** para o runtime de integração autoalojado máquina é, pelo menos, 2 GHz, 4 núcleos, 8 GB de RAM e disco de 80 GB.
- Se a máquina de anfitrião hiberna, o runtime de integração autoalojado não responde a pedidos de dados. Por conseguinte, configure uma esquema de energia adequado no computador antes de instalar o runtime de integração autoalojado. Se a máquina está configurada em hibernação, a instalação do runtime de integração autoalojado pede-lhe uma mensagem.
- Tem de ser um administrador no computador para instalar e configurar o runtime de integração autoalojado com êxito.
- À medida que ocorrem execuções de atividade de cópia numa frequência específica, a utilização de recursos (CPU, memória) na máquina também segue o mesmo padrão com horas de pico e tempos de inatividade. Utilização de recursos também depende muito a quantidade de dados a ser movidos. Quando várias tarefas de cópia em curso, verá subir durante as horas de pico de utilização de recursos.

## <a name="installation-best-practices"></a>Melhores práticas de instalação
Runtime de integração autoalojado que pode ser instalado baixando um pacote de instalação do MSI do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Ver [mover dados entre locais e na cloud artigo](tutorial-hybrid-copy-powershell.md) para obter instruções passo a passo.

- Configure o esquema de energia no computador anfitrião para o runtime de integração autoalojado para que a máquina sem a hibernação. Se a máquina de anfitrião hiberna, o runtime de integração autoalojado torna-se offline.
- Criar cópias de segurança as credenciais associadas com o runtime de integração autoalojado regularmente.

## <a name="install-and-register-self-hosted-ir-from-download-center"></a>Instalar e registar o ir Autoalojado do Centro de download

1. Navegue para [página de download do Microsoft Integration Runtime](https://www.microsoft.com/download/details.aspx?id=39717).
2. Clique em **baixe**, selecione a versão adequada (**32-bit** vs. **64-bit**) e clique em **próxima**.
3. Executar o **MSI** diretamente ou guardá-lo para o seu disco rígido e a execução.
4. Sobre o **bem-vindo** página, selecione um **linguagem** clique em **seguinte**.
5. **Aceite** o contrato de licença de utilizador final e clique em **próxima**.
6. Selecione **pasta** para instalar o runtime de integração autoalojado e clique em **próxima**.
7. Sobre o **pronto para instalar o** página, clique em **instalar**.
8. Clique em **concluir** para concluir a instalação.
9. Obtenha a chave de autenticação com o Azure PowerShell. Exemplo do PowerShell para obter a chave de autenticação:

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resouceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime
    ```
11. Sobre o **registar o Integration Runtime (autoalojado)** página do Microsoft Integration Runtime Configuration Manager em execução no seu computador, siga os passos abaixo:
    1. Colar o **chave de autenticação** na área de texto.
    2. Opcionalmente, clique em **chave de autenticação Show** para ver o texto da chave.
    3. Clique em **registar**.


## <a name="high-availability-and-scalability"></a>Elevada disponibilidade e escalabilidade
Um Runtime de integração autoalojado pode ser associado a várias máquinas no local. Estas máquinas são chamadas de nós. Pode ter até quatro nós associados a um Runtime de integração autoalojado. Os benefícios de ter vários nós (máquinas no local com o gateway instalado) para um gateway lógico são:
1. Disponibilidade mais elevada do Runtime de integração autoalojado, de modo que ele deixou de ser o ponto único de falha na sua integração de dados de grandes volumes de dados solução ou na cloud, Azure Data Factory, garantindo a continuidade com até 4 nós.
2. Melhorou o desempenho e a produtividade durante o movimento de dados entre aplicações no local e na cloud arquivos de dados. Obtenha mais informações sobre [comparações de desempenho](copy-activity-performance.md).

Pode associar vários nós, simplesmente instalando o software de Runtime de integração autoalojado a partir da [Centro de transferências](https://www.microsoft.com/download/details.aspx?id=39717) e registrando-o por qualquer uma das chaves de autenticação obtido a partir do Cmdlet de novo AzureRmDataFactoryV2IntegrationRuntimeKey conforme descrito no [Tutorial](tutorial-hybrid-copy-powershell.md)

> [!NOTE]
> Não é necessário criar um novo Runtime de integração autoalojado para associar a cada nó. Pode instalar o runtime de integração autoalojado em outra máquina e registá-lo com a mesma chave de autenticação. 

> [!NOTE]
> Antes de adicionar outro nó para **elevada disponibilidade e escalabilidade**, certifique-se **'Acesso remoto à intranet'** é a opção **ativada** no nó 1 (Microsoft Gestor de configuração do Runtime de integração -> Definições -> remoto acesso à intranet). 

### <a name="scale-considerations"></a>Considerações de dimensionamento

#### <a name="scale-out"></a>Aumentar horizontalmente

Quando o **memória disponível no runtime de integração autoalojado é baixa** e o **utilização da CPU é alta**, adicionar um novo nó ajuda a aumentar horizontalmente a carga entre máquinas. Se atividades estão a falhar devido a tempo limite ou autoalojado nó do Runtime de integração a ser offline, isso será útil se adicionar um nó para o gateway.

#### <a name="scale-up"></a>Aumentar verticalmente

Quando a memória disponível e a CPU não são utilizados bem, mas a execução de tarefas simultâneas está a atingir o limite, deve aumentar verticalmente ao aumentar o número de tarefas simultâneas que podem ser executadas num nó. Pode também querer aumentar verticalmente quando as atividades são exceder o tempo limite porque o runtime de integração autoalojado está sobrecarregado. Conforme mostrado na imagem seguinte, pode aumentar a capacidade máxima para um nó.  

![](media\create-self-hosted-integration-runtime\scale-up-self-hosted-IR.png)

### <a name="tlsssl-certificate-requirements"></a>Requisitos de certificado TLS/SSL

Eis os requisitos para o certificado TLS/SSL que é utilizado para proteger as comunicações entre o integration runtime nós:

- O certificado tem de ser um publicamente fidedigno X509 v3 certificado. Recomendamos que utilize certificados emitidos por uma autoridade de certificação de (de terceiros) pública (AC).
- Cada nó do integration runtime têm de confiar este certificado.
- Os certificados de caráter universal são suportados. Se o nome FQDN for **node1.domain.contoso.com**, pode utilizar ***. domain.contoso.com** como nome do requerente do certificado.
- Certificados SAN não são recomendados uma vez que será utilizado apenas o último item dos nomes alternativos do requerente e todos os outros serão ignorados devido a limitação atual. Por exemplo, tem um certificado SAN é cujo SAN **node1.domain.contoso.com** e **node2.domain.contoso.com**, só pode utilizar este certificado no computador cujo FQDN for **node2.domain.contoso.com**.
- Suporta qualquer tamanho de chave suportado pelo Windows Server 2012 R2 para certificados SSL.
- Usando a CNG de certificado não são suportadas chaves.  

## <a name="sharing-the-self-hosted-integration-runtime-ir-with-multiple-data-factories"></a>Partilhar o Runtime de integração autoalojado (IR) com várias fábricas de dados

É possível reutilizar uma infraestrutura existente do runtime de integração autoalojado que pode já ter o programa de configuração numa fábrica de dados. Isto permite-lhe criar uma **ligado integration runtime autoalojado** numa diferentes dados fábrica fazendo referência um já existente autoalojado (compartilhado).

#### <a name="terminologies"></a>**Terminologias**

- **Partilhado IR** – original autoalojado que está em execução numa infraestrutura física.  
- **Runtime de integração de ligado** – o runtime de integração que faz referência a outro ir partilhado. Este é um runtime de integração lógico e utiliza a infraestrutura de outro Runtime de integração autoalojado (compartilhado).

#### <a name="high-level-steps-for-creating-a-linked-self-hosted-ir"></a>Passos de nível altos para a criação de um runtime de integração autoalojado ligado

No runtime de integração autoalojado para ser compartilhado,

1. Conceder permissão à fábrica de dados em que gostaria de criar o ir ligado. 

   ![](media\create-self-hosted-integration-runtime\grant-permissions-IR-sharing.png)

2. Tenha em atenção a **ID de recurso** do Runtime de integração autoalojado para ser partilhado.

   ![](media\create-self-hosted-integration-runtime\4_ResourceID_self-hostedIR.png)

Na fábrica de dados aos quais foram concedidas as permissões,

3. Criar um novo ir Autoalojado (ligado) e introduza o acima **ID de recurso**

   ![](media\create-self-hosted-integration-runtime\6_create-linkedIR_2.png)

   ![](media\create-self-hosted-integration-runtime\6_create-linkedIR_3.png)

#### <a name="known-limitations-of-self-hosted-ir-sharing"></a>Limitações conhecidas de compartilhamento de Runtime de integração autoalojado

1. Número predefinido de Runtime de integração ligado, que pode ser criada em único Runtime de integração autoalojado é **20**. Se precisar de mais, em seguida, contacte o suporte. 

2. A fábrica de dados em que o runtime de integração ligado é a ser criada tem de ter um MSI ([identidade do serviço gerido](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)). Por predefinição, as fábricas de dados criado no Ibiza portal ou cmdlets do PowerShell terão MSI criado implicitamente. No entanto, em alguns casos, quando é criado a fábrica de dados de utilização de um modelo de Gestor de recursos do Azure ou SDK, o "**identidade**" **tem de definir a propriedade** explicitamente para garantir que o Gestor de recursos do Azure cria uma fábrica de dados que contém um MSI. 

3. A versão de Runtime de integração autoalojada tem de ser igual ou superior a 3.8.xxxx.xx. Tente [transferir a versão mais recente](https://www.microsoft.com/download/details.aspx?id=39717) do Runtime de integração autoalojado

4. A fábrica de dados em que o runtime de integração ligado é a ser criada tem de ter um MSI ([identidade do serviço gerido](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)). Por predefinição, as fábricas de dados criadas no Ibiza portal ou cmdlets do PowerShell terão MSI ([identidade do serviço gerido](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)).
criada implicitamente, no entanto, as fábricas de dados criadas com o modelo Azure Resource Manager (ARM) ou o SDK requer "Identity" propriedade ser definida para garantir que é criado um MSI.

5. O SDK que suportam esta funcionalidade do .net do ADF é a versão > = 1.1.0

6. O Azure PowerShell que suportam esta funcionalidade é a versão > = 6.6.0 (AzureRM.DataFactoryV2 > = 0.5.7)

7. Para conceder permissão, o usuário exigirá a função de "Proprietário" ou função de "Proprietário" herdado na fábrica de dados, onde existe o IR partilhado. 

  > [!NOTE]
  > Esta funcionalidade só está disponível no Azure Data Factory versão 2 

## <a name="system-tray-icons-notifications"></a>Ícones de tabuleiro de sistema / notificações

Se mover o cursor sobre a mensagem de ícone/notificação do tabuleiro de sistema, pode encontrar detalhes sobre o estado do runtime de integração autoalojado.

![Notificações de tabuleiro de sistema](media\create-self-hosted-integration-runtime\system-tray-notifications.png)

## <a name="ports-and-firewall"></a>Portas e firewall
Existem duas firewalls, precisa considerar: **firewall Corporativo** em execução no router central da organização, e **firewall do Windows** configurado como um daemon no computador local em que o runtime de integração autoalojado é instalado.

![Firewall](media\create-self-hosted-integration-runtime\firewall.png)

Em **firewall Corporativo** nível, precisa configurar os seguintes domínios e as portas de saída:

Nomes de domínio | Portas | Descrição
------------ | ----- | ------------
*.servicebus.windows.net | 443, 80 | Utilizado para comunicação com o back-end do serviço de movimento de dados
*.core.windows.net | 443 | Utilizado para a cópia de teste usando BLOBs do Azure (se configurada)
*.frontend.clouddatahub.net | 443 | Utilizado para comunicação com o back-end do serviço de movimento de dados
download.microsoft.com | 443 | Utilizado para transferir as atualizações

Em **firewall do Windows** nível (nível de máquina), estas portas de saída normalmente estão ativadas. Se não, pode configurar as portas em conformidade no autoalojado e domínios de computador do runtime de integração.

> [!NOTE]
> Com base na sua origem / sinks, poderá ter de domínios adicionais da lista de permissões e portas de saída na firewall empresarial/Windows.
>
> Para algumas bases de dados na Cloud (por exemplo: SQL Database do Azure, Azure Data Lake, etc.), poderá ter de endereço IP da lista branca do computador do runtime de integração autoalojado em suas configurações de firewall.

### <a name="copy-data-from-a-source-to-a-sink"></a>Copiar dados de uma origem para um sink
Certifique-se de que as regras de firewall estão ativadas corretamente do firewall Corporativo, o firewall do Windows no computador de runtime de integração autoalojado, e o arquivo de dados em si. Ativar estas regras, permite que o runtime de integração autoalojado ligar a ambas as origem e sink com êxito. Ative as regras para cada arquivo de dados que está envolvido na operação de cópia.

Por exemplo, a cópia a partir de um **do arquivo de dados no local para um sink de base de dados do Azure SQL ou um coletor do Azure SQL Data Warehouse**, siga os passos abaixo:

- Permitir saída **TCP** comunicações na porta **1433** para o firewall do Windows e o firewall Corporativo.
- Configure definições de firewall do servidor SQL do Azure para adicionar o endereço IP do computador runtime de integração autoalojado para a lista de endereços IP permitidos.

> [!NOTE]
> Se a firewall não permite a saída através da porta 1433, o runtime de integração autoalojado não é possível acessar SQL do Azure diretamente. Neste caso, pode usar [cópia faseada](copy-activity-performance.md) à base de dados do SQL Azure / armazém de dados do SQL Azure. Neste cenário, apenas exigiria HTTPS (porta 443) para o movimento de dados.


## <a name="proxy-server-considerations"></a>Considerações do servidor proxy
Se o seu ambiente de rede Corporativo utiliza um servidor proxy para aceder à internet, configure o runtime de integração autoalojado para utilizar definições de proxy apropriados. Pode definir o proxy durante a fase de registro inicial.

![Especificar o proxy](media\create-self-hosted-integration-runtime\specify-proxy.png)

Runtime de integração autoalojado utiliza o servidor proxy para ligar ao serviço cloud. Clique a ligação de alteração durante a configuração inicial. Verá a caixa de diálogo de definição de proxy.

![Proxy de conjunto](media\create-self-hosted-integration-runtime\set-http-proxy.png)

Existem três opções de configuração:

- **Não utilizar o proxy**: integration runtime autoalojado não utiliza explicitamente qualquer proxy para ligar a serviços cloud.
- **Utilizar o proxy do sistema**: autoalojado usos de runtime de integração a definição que de proxy está configurado no diahost.exe.config e diawp.exe.config. Se nenhum proxy estiver configurado no diahost.exe.config e diawp.exe.config, o integration runtime autoalojado liga ao serviço em nuvem diretamente sem passar pelo proxy.
- **Utilizar proxy personalizado**: configurar a definição a utilizar para o runtime de integração autoalojado, em vez de usar configurações no diahost.exe.config e diawp.exe.config de proxy HTTP. O endereço e porta são necessários. Nome de utilizador e palavra-passe são opcionais, consoante a definição de autenticação de seu proxy. Todas as definições são encriptadas com a DPAPI do Windows, o tempo de execução de integração autoalojado e armazenadas localmente na máquina.

O serviço de anfitrião do integration runtime é reiniciado automaticamente depois de guardar as definições de proxy atualizadas.

Depois do integration runtime autoalojado for registado com êxito, se pretender ver ou atualizar as definições de proxy, utilize o Gestor de configuração do Runtime de integração.

1. Inicie **Gestor de configuração do Microsoft Integration Runtime**.
   - Mudar para o separador **Definições**.
   - Clique em **alteração** ligação na **HTTP Proxy** secção para iniciar o **definir o Proxy de HTTP** caixa de diálogo.
   - Depois de clicar no **seguinte** botão, verá uma caixa de diálogo de aviso solicitando a permissão Guardar a definição de proxy e reinicie o serviço de anfitrião do Integration Runtime.

Pode ver e atualizar o proxy HTTP utilizando a ferramenta do Configuration Manager.

![Proxy de vista](media\create-self-hosted-integration-runtime\view-proxy.png)

> [!NOTE]
> Se configurar um servidor proxy com a autenticação NTLM, o runtime de integração é executado o serviço de anfitrião sob a conta de domínio. Se alterar a palavra-passe para a conta de domínio mais tarde, lembre-se atualizar as definições de configuração para o serviço e reinicie-o em conformidade. Devido a este requisito, sugerimos que usar uma conta de domínio dedicado para aceder ao servidor de proxy que não necessita de atualizar a palavra-passe com frequência.

### <a name="configure-proxy-server-settings"></a>Configurar definições do servidor proxy

Se selecionou **utilizar o proxy do sistema** definir para o proxy HTTP, o runtime de integração autoalojado que utiliza a definição no diahost.exe.config e diawp.exe.config de proxy. Se não for especificado nenhum proxy no diahost.exe.config e diawp.exe.config, o integration runtime autoalojado liga ao serviço em nuvem diretamente sem passar pelo proxy. O procedimento seguinte fornece instruções para atualizar o ficheiro de diahost.exe.config.

1. No Explorador de ficheiros, faça uma cópia de segurança do C:\Program Files\Microsoft integração Runtime\3.0\Shared\diahost.exe.config para fazer backup do arquivo original.
2. Inicie Notepad.exe em execução como administrador e abrir o arquivo de texto "C:\Program Files\Microsoft integração Runtime\3.0\Shared\diahost.exe.config. Encontrará a etiqueta predefinida para system.net, conforme mostrado no seguinte código:

    ```xml
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```
    Em seguida, pode adicionar detalhes do servidor proxy, conforme mostrado no exemplo a seguir:

    ```xml
    <system.net>
        <defaultProxy enabled="true">
              <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Propriedades adicionais são permitidas dentro da marca de proxy para especificar as definições necessárias como scriptLocation. Ver [proxy elemento (definições de rede)](https://msdn.microsoft.com/library/sa91de1e.aspx) sintaxe.

    ```xml
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Guarde o ficheiro de configuração na localização original, em seguida, reinicie o serviço de Host de Runtime de integração autoalojado, que seleciona as alterações. Para reiniciar o serviço: usar serviços miniaplicativo do painel de controlo ou a partir do **Gestor de configuração do Runtime de integração** > clique no **parar serviço** botão, em seguida, clique no **iniciar Serviço**. Se o serviço não iniciar, é provável que foi adicionada uma sintaxe de marca XML inválida para o ficheiro de configuração de aplicação que foi editado.

> [!IMPORTANT]
> Não se esqueça de atualizar diahost.exe.config e diawp.exe.config.

Além desses pontos, terá também de certificar-se de que o Microsoft Azure está na lista de permissões da sua empresa. A lista de endereços válidos de IP do Microsoft Azure pode ser transferida a partir da [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Sintomas possíveis problemas relacionados a servidores de proxy e firewall
Se encontrar erros semelhantes aos seguintes, é provável devido a uma configuração incorreta do servidor proxy ou firewall, que bloqueia o runtime de integração autoalojado se a fábrica de dados para se autenticar. Consulte a secção anterior para garantir que a firewall e servidor proxy estão configuradas corretamente.

1. Ao tentar registar o runtime de integração autoalojado, receberá o seguinte erro: "Falha ao registar este nó do Integration Runtime! Confirme que a chave de autenticação é válida e o serviço de anfitrião do serviço de integração está em execução neste computador. "
   - Quando abre o Gestor de configuração do Runtime de integração, verá o estado como "**desligado**"ou"**Connecting**". Ao visualizar registos de eventos do Windows, sob "Visualizador de eventos" > "Application e registos de serviços" > "Microsoft Integration Runtime", verá mensagens de erro, como o seguinte erro:

    ```
    Unable to connect to the remote server
    A component of Integration Runtime has become unresponsive and restarts automatically. Component name: Integration Runtime (Self-hosted).
    ```

### <a name="enable-remote-access-from-intranet"></a>Ativar o acesso remoto da Intranet  
Caso se usar **PowerShell** ou **aplicação do Gestor de credenciais** para encriptar as credenciais de outro computador (na rede) que não seja em que o runtime de integração autoalojado é instalado, em seguida, exigiria a **"Acesso remoto a partir da Intranet"** opção esteja ativada. Se executar o **PowerShell** ou **aplicação do Gestor de credenciais** para encriptar a credencial na mesma máquina em que o runtime de integração autoalojado é instalado, em seguida, **"acesso remoto da Intranet "** poderá não estar ativada.

Deve ser o acesso remoto a partir da Intranet **habilitado** antes de adicionar outro nó para **elevada disponibilidade e escalabilidade**.  

Durante a configuração do runtime de integração autoalojado (e posteriores 3.3.xxxx.x v), por predefinição, a instalação do runtime de integração autoalojado desativa o **"Acesso remoto a partir da Intranet"** na máquina de runtime de integração autoalojado.

Se estiver a utilizar uma firewall de terceiros, é possível abrir manualmente a porta 8060 (ou a porta de utilizador configurado). Caso se depare com problema do firewall durante a configuração do runtime de integração autoalojado, pode experimentar com o seguinte comando para instalar o runtime de integração autoalojado sem configurar a firewall.

```
msiexec /q /i IntegrationRuntime.msi NOFIREWALL=1
```
> [!NOTE]
> **Aplicação do Gestor de credenciais** ainda não está disponível para encriptar as credenciais no ADFv2. Vamos adicionar esse suporte mais tarde.  

Se optar por não abrir a porta 8060 no computador runtime de integração autoalojado, utilizar mecanismos, além de usar o * * definição credenciais * * aplicação para configurar as credenciais do arquivo de dados. Por exemplo, pode utilizar o cmdlet do PowerShell New-AzureRmDataFactoryV2LinkedServiceEncryptCredential. Consulte a secção de credenciais de definição e a segurança na forma como as credenciais do arquivo de dados pode ser definida.


## <a name="next-steps"></a>Passos Seguintes
Veja o tutorial seguinte para obter instruções passo a passo: [Tutorial: copiar dados no local para a cloud](tutorial-hybrid-copy-powershell.md).

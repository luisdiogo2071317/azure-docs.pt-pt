---
title: Criar uma imagem de máquina virtual para o Azure Marketplace | Documentos da Microsoft
description: Obter instruções detalhadas sobre como criar uma imagem de máquina virtual para o Azure Marketplace para outras pessoas comprar.
services: Azure Marketplace
documentationcenter: ''
author: msmbaldwin
manager: mbaldwin
editor: ''
ms.assetid: 5c937b8e-e28d-4007-9fef-624046bca2ae
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 01/05/2017
ms.author: mbaldwin
ms.openlocfilehash: f997df7b6a36752ffa9f53979101b09421d68802
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39425141"
---
# <a name="guide-to-create-a-virtual-machine-image-for-the-azure-marketplace"></a>Guia para criar uma imagem de máquina virtual para o Azure Marketplace
Este artigo **passo 2**, explica como preparar os discos rígidos virtuais (VHDs) irá implementar no Azure Marketplace. Os VHDs constituem os alicerces do SKU. O processo é diferente dependendo se está a fornecer um SKU baseado em Linux ou Windows. Este artigo aborda os dois cenários. Este processo pode ser executado em paralelo com [criação e registo de contas][link-acct-creation].

## <a name="1-define-offers-and-skus"></a>1. Definir ofertas e SKUs
Nesta secção, vai aprender a definir as ofertas e os SKUs associados.

Uma oferta funciona como o "elemento principal" de todos os SKUs correspondentes. Pode ter várias ofertas. A forma como as ofertas são estruturadas é uma questão que lhe cabe a si decidir. Quando uma oferta é emitida para teste, a emissão é propagada por todos os SKUs correspondentes. Considere cuidadosamente os seus identificadores SKU, porque eles serão visíveis no URL:

* Azure.com: http://azure.microsoft.com/marketplace/partners/{PartnerNamespace}/{OfferIdentifier}-{SKUidentifier}
* Portal de pré-visualização do Azure: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{SKUIDdentifier}  

Um SKU é o nome comercial de uma imagem de VM. Uma imagem VM contém o disco de um sistema operativo e zero ou mais discos de dados. Trata-se, no fundo, do perfil de armazenamento completo de uma máquina virtual. É necessário um VHD por disco. Os discos de dados, mesmo em branco precisam de um VHD a ser criada.

Independentemente do sistema operativo utilizado, adicione apenas o número mínimo de discos de dados necessários ao SKU. Os clientes não é possível remover os discos que fazem parte de uma imagem no momento da implementação, mas podem sempre adicionar discos durante ou após a implantação se forem necessários.

> [!IMPORTANT]
> **Não altere o número de discos numa nova versão de imagem.** Se tem de reconfigurar discos de dados na imagem, defina um novo SKU. Publicar uma nova versão de imagem com contagens de outro disco será têm o potencial da nova implementação de última hora com base na nova versão de imagem em casos de Implantações de dimensionamento automático e automática de soluções através de modelos ARM e outros cenários.
>
>

### <a name="11-add-an-offer"></a>1.1 adicionar uma oferta
1. Inicie sessão para o [Portal de publicação] [ link-pubportal] usando sua conta de vendedor.
1. Selecione o **máquinas virtuais** separador do Portal de publicação. No campo de entrada que, introduza o nome da sua oferta. O nome da oferta, normalmente, é o nome do produto ou serviço que pretende vender no Azure Marketplace.
1. Selecione **Criar**.

### <a name="12-define-a-sku"></a>1.2 definir um SKU
Depois de ter adicionado uma oferta, terá de definir e identificar suas SKUs. Pode ter várias ofertas e cada oferta pode ter várias SKUs sob a mesma. Quando uma oferta é emitida para teste, a emissão é propagada por todos os SKUs correspondentes.

1. **Adicione um SKU.** O SKU requer um identificador, o que é utilizado no URL. O identificador tem de ser exclusivo no seu perfil de publicação, mas não há risco de colidir identificador com outros editores.

   > [!NOTE]
   > Os identificadores de SKU e a oferta são apresentados no URL oferta no Marketplace.
   >
   >
1. **Adicione uma descrição de resumo para o SKU.** Descrições de resumidas são visíveis para os clientes, para que deve fazê-las facilmente legível. Estas informações não precisam de ser bloqueado até a fase de "fase emitir para teste". Até lá, pode editá-las conforme necessário.
1. Se estiver a utilizar SKUs baseadas no Windows, siga as ligações sugeridas para obter as versões aprovadas do Windows Server.

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a>2. Criar um VHD compatível com o Azure (baseado em Linux)
Esta secção concentra-se nas melhores práticas para criar uma imagem de VM baseado em Linux para o Azure Marketplace. Para obter instruções passo a passo, consulte a documentação do seguinte: [criar uma imagem de VM do Linux personalizada](../virtual-machines/linux/create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a>3. Criar um VHD compatível com o Azure (baseados em Windows)
Esta secção aborda os passos para criar um SKU com base no Windows Server para o Azure Marketplace.

### <a name="31-ensure-that-you-are-using-the-correct-base-vhds"></a>3.1 Certifique-se de que está a utilizar os VHDs de bases corretos
O sistema operativo VHD da imagem de VM deve basear-se numa imagem base aprovada do Azure que contém o Windows Server ou SQL Server.

Para começar, crie uma VM a partir de um dos seguintes imagens, localizado no [portal do Microsoft Azure][link-azure-portal]:

* Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1] [link-datactr-2008-r2])
* SQL Server 2014 ([Enterprise][link-sql-2014-ent], [padrão][link-sql-2014-std], [Web] [ link-sql-2014-web])
* O SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [padrão][link-sql-2012-std], [Web] [ link-sql-2012-web])

Estas ligações encontram-se igualmente disponíveis no Portal de Publicação, na página SKU.

> [!TIP]
> Se estiver a utilizar o portal do Azure atual ou o PowerShell, as imagens do Windows Server publicadas a 8 de Setembro de 2014 e versões posteriores são aprovadas.
>
>

### <a name="32-create-your-windows-based-vm"></a>3.2 criar a sua VM com base no Windows
No portal do Microsoft Azure, pode criar sua VM com base numa imagem base aprovada seguindo alguns passos simples. Segue-se uma descrição geral do processo:

1. Na página da imagem base, selecione **criar Máquina Virtual** para ser direcionado para a nova [portal do Microsoft Azure][link-azure-portal].

    ![desenho][img-acom-1]
1. Inicie sessão no portal com a conta Microsoft e a palavra-passe para a subscrição do Azure que pretende utilizar.
1. Siga as instruções para criar uma VM com a imagem base selecionada. Tem de fornecer um host name (nome do computador), (registado como administrador) de nome de utilizador e palavra-passe para a VM.

    ![desenho][img-portal-vm-create]
1. Selecione o tamanho da VM para implementar:

    a.    Se tenciona desenvolver o VHD no local, o tamanho não é relevante. Pondere utilizar uma das VMs mais pequenas.

    b.    Se tenciona desenvolver a imagem no Azure, pondere utilizar um dos tamanhos de VM recomendados para a imagem selecionada.

    c.    Para obter informações sobre preços, consulte a **escalões de preço recomendados** Seletor apresentado no portal. Ser-lhe-ão indicados os três tamanhos recomendados fornecidos pelo editor. (Neste caso, o editor é a Microsoft.)

    ![desenho][img-portal-vm-size]
1. Definir as propriedades:

    a.    Para uma implementação rápida, pode deixar os valores predefinidos das propriedades em **configuração opcional** e **grupo de recursos**.

    b.    Sob **conta de armazenamento**, opcionalmente, pode selecionar a conta de armazenamento na qual o sistema de operativo VHD será armazenado.

    c.    Sob **grupo de recursos**, opcionalmente, pode selecionar o grupo lógico onde colocar a VM.
1. Selecione o **localização** para a implementação:

    a.    Se tenciona desenvolver o VHD no local, a localização não é relevante porque irá carregar a imagem para o Azure mais tarde.

    b.    Se tenciona desenvolver a imagem no Azure, pondere utilizar uma das regiões do Microsoft Azure baseadas nos E.U.A. desde o início. Isso acelera o processo de cópia do VHD que a Microsoft efetua em seu nome quando submete a imagem para certificação.

    ![desenho][img-portal-vm-location]
1. Clique em **Criar**. Inicie a VM a implementação. No espaço de alguns minutos, terá uma implementação bem-sucedida à sua disposição, podendo começar a criar a imagem para o SKU.

### <a name="33-develop-your-vhd-in-the-cloud"></a>3.3 desenvolva o VHD na cloud
Recomendamos vivamente que desenvolva o VHD na cloud utilizando o protocolo RDP (Remote Desktop). Ligar para RDP com o nome de utilizador e palavra-passe especificados durante o aprovisionamento.

> [!IMPORTANT]
> **Não utilize discos geridos.** A máquina virtual utilizada para desenvolver o VHD para a cloud não devem ser baseada em discos geridos, tal como atualmente não suporta a criação de uma imagem dos mesmos.
> Criar a máquina virtual em que a alteração de funcionalidade opcional a predefinição para discos geridos.

> Se desenvolver o VHD no local (que não é recomendado), consulte [criando uma imagem de máquina virtual no local](marketplace-publishing-vm-image-creation-on-premise.md). Transferir o VHD não é necessário se estiver a desenvolver na cloud.
>
>

**Ligar via RDP usando o [portal do Microsoft Azure][link-azure-portal]**

1. Selecione **todos os serviços** > **VMs**.
1. É aberto o painel de máquinas virtuais. Certifique-se de que a VM que pretende ligar-se com está em execução e, em seguida, selecione-o na lista de VMs implantadas.
1. É aberto um painel que descreve a VM selecionada. Na parte superior, clique em **Connect**.
1. São-lhe pedido para introduzir o nome de utilizador e palavra-passe que especificou durante o aprovisionamento.

**Ligar via RDP com o PowerShell**

Para transferir um ficheiro de ambiente de trabalho remoto para um computador local, utilize o [cmdlet Get-AzureRemoteDesktopFile][link-technet-2]. Para utilizar este cmdlet, precisa saber o nome do serviço e o nome da VM. Se criou a VM a partir da [portal do Microsoft Azure][link-azure-portal], pode encontrar estas informações em Propriedades de VM:

1. No portal do Microsoft Azure, selecione **todos os serviços** > **VMs**.
1. É aberto o painel de máquinas virtuais. Selecione a VM que implementou.
1. É aberto um painel que descreve a VM selecionada.
1. Clique em **Propriedades**.
1. A primeira parte do nome de domínio corresponde ao nome do serviço. O nome do anfitrião é o nome da VM.

    ![desenho][img-portal-vm-rdp]
1. Segue-se o cmdlet para transferir o ficheiro RDP da VM criada a área de trabalho do administrador local.

        Get‐AzureRemoteDesktopFile ‐ServiceName “baseimagevm‐6820cq00” ‐Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

Podem encontrar mais informações sobre RDP no MSDN no artigo [ligar a uma VM do Azure com RDP ou SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).

**Configurar uma VM e criar o SKU**

Depois do sistema de operativo que VHD tiver sido transferido, utilize o Hyper-v e configurar uma VM para começar a criar o SKU. Os passos detalhados podem ser encontrados em ligação do TechNet seguinte: [instalar o Hyper-v e configurar uma VM](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="34-choose-the-correct-vhd-size"></a>3.4 escolher o tamanho do VHD correto
O sistema de operativo VHD do Windows na imagem de VM deverá ser criado como um VHD de formato fixo de 128 GB.  

Se o tamanho físico for inferior a 128 GB, o VHD deverá ser disperso. As imagens do Windows e o SQL Server bases, fornecidas já cumprem estes requisitos, portanto, não altere o formato ou o tamanho do VHD obtido.  

Discos de dados podem ter até 1 TB. Ao decidir o tamanho do disco, lembre-se de que os clientes não podem redimensionar os VHDs de uma imagem no momento da implantação. Os VHDs de discos de dados deverá ser criados como um VHD de formato fixo. Também deverá ser dispersos. Os discos de dados podem estar vazios ou conter dados.

### <a name="35-install-the-latest-windows-patches"></a>3.5 instalar os patches mais recentes do Windows
As imagens base contêm os patches mais recentes até à data da publicação das mesmas. Antes de publicar o sistema de operativo VHD que criou, certifique-se de que a atualização do Windows foi executada e que todas as atualizações mais recentes críticas e importantes segurança foram instaladas.

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a>3.6 efetuar tarefas de configuração e programação adicionais conforme necessário
Se é necessária qualquer configuração adicional, considere a utilização de uma tarefa agendada que seja executada no arranque para realizar eventuais alterações finais na VM depois de este ter sido implementado:

* É considerada uma melhor prática que a tarefa esteja programada para se eliminar automaticamente após a execução bem-sucedida da mesma.
* Nenhuma configuração deverá depender de outras unidades que não unidades C ou D, porque estes são as únicas duas unidades são existência garantidas. Unidade C é o disco do sistema operativo e a unidade D é o disco local temporário.

### <a name="37-generalize-the-image"></a>3.7 generalizar a imagem
Todas as imagens no Azure Marketplace tem de ser reutilizáveis de um modo geral. Em outras palavras, o sistema de operativo VHD tem de ser generalizado:

* Para Windows, a imagem deve ser "Sysprep" e devem ser feitas sem configurações que não suportam o **sysprep** comando.
* Pode executar o seguinte comando a partir do diretório % windir%\System32\Sysprep.

        sysprep.exe /generalize /oobe /shutdown

  Documentação de orientação sobre como ao sysprep do sistema operacional é fornecido no passo do seguinte artigo do MSDN: [criar e carregar um VHD do Windows Server para o Azure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="4-deploy-a-vm-from-your-vhds"></a>4. Implementar uma VM a partir dos VHDs
Depois de carregar os VHDs (o sistema de operativo VHD generalizado e dados de zero ou mais VHDs de discos) a uma conta de armazenamento do Azure, pode registá-los como uma imagem de VM do utilizador. Em seguida, pode testar essa imagem. Tenha em atenção que uma vez que o sistema de operativo VHD é generalizado, não é possível diretamente implementar a VM ao fornecer o URL de VHD.

Para mais informações sobre imagens VM, reveja as seguintes mensagens de blogue:

* [Imagem de VM](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [Como PowerShell de imagem VM para](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [Sobre imagens VM no Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-the-necessary-tools-powershell-and-azure-cli"></a>Configurar as ferramentas necessárias, PowerShell e CLI do Azure
* [Como configurar o PowerShell](/powershell/azure/overview)
* [Como configurar a CLI do Azure](../cli-install-nodejs.md)

### <a name="41-create-a-user-vm-image"></a>4.1 criar uma imagem de VM do utilizador
#### <a name="capture-vm"></a>Captura de VM
Leia as ligações indicadas abaixo para obter orientações sobre como capturar a VM com a CLI do PowerShell/API/Azure.

* [API](https://msdn.microsoft.com/library/mt163560.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [CLI do Azure](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a>Generalizar a imagem
Leia as ligações indicadas abaixo para obter orientações sobre como capturar a VM com a CLI do PowerShell/API/Azure.

* [API](https://msdn.microsoft.com/library/mt269439.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [CLI do Azure](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a>4.2 implementar uma VM a partir de uma imagem VM do utilizador
Para implementar uma VM a partir de uma imagem VM do utilizador, pode utilizar o atual [portal do Azure](https://manage.windowsazure.com) ou o PowerShell.

**Implementar uma VM a partir do portal do Azure**

1. Aceda a **New** > **computação** > **Máquina Virtual** > **da galeria**.

1. Aceda a **as minhas imagens**e, em seguida, selecione a imagem de VM a partir do qual pode implementar uma VM:

   1. Preste muita atenção à imagem que selecionar, porque o **as minhas imagens** vista lista imagens do sistema operativo e imagens de VM.
   1. Repare no número de discos pode ajudar a determinar o tipo de imagem está a implementar, porque a maioria das imagens de VM tem mais de um disco. No entanto, é possível ter uma imagem de VM com apenas um único disco do sistema operativo que, em seguida, teria **número de discos** definido como 1.

      ![desenho][img-manage-vm-select]
1. Siga o Assistente de criação de VM e especificar o VM name, VM tamanho, localização, nome de utilizador e palavra-passe.

**Implementar uma VM a partir do PowerShell**

Para implementar uma VM grande a partir da imagem de VM generalizada acabou de criar, pode utilizar os seguintes cmdlets.

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> Para obter mais ajuda, consulte [Resolução de problemas o problemas comuns encontrados durante a criação do VHD].
>
>

## <a name="5-obtain-certification-for-your-vm-image"></a>5. Obter certificação para sua imagem VM
O passo seguinte na preparação da imagem VM para o Azure Marketplace consiste em certificá-la.

Este processo inclui a execução de uma ferramenta de certificação especial, carregar os resultados de verificação para o contentor do Azure onde residem os VHDs, adição de uma oferta, definindo o SKU e submeter a sua imagem VM para obter uma certificação.

### <a name="51-download-and-run-the-certification-test-tool-for-azure-certified"></a>5.1 Transferir e executar a ferramenta de teste de certificação para o Azure Certified
A ferramenta de certificação é executado numa VM em execução, aprovisionada a partir da sua imagem VM do utilizador, para garantir que a imagem de VM é compatível com o Microsoft Azure. A ferramenta irá verificar se as orientações e os requisitos inerentes à preparação do VHD foram cumpridos. A saída da ferramenta é um relatório de compatibilidade, que deve ser carregado no Portal de publicação ao solicitante de certificação.

A ferramenta de certificação pode ser utilizada com o Windows e VMs do Linux. Ele se conecta a VMs baseadas no Windows através do PowerShell e liga-se para VMs do Linux via SSH.Net:

1. Em primeiro lugar, baixar a ferramenta de certificação no [site de download da Microsoft][link-msft-download].
1. Abra a ferramenta de certificação e, em seguida, clique a **Start New Test** botão.
1. Partir do **Test Information** ecrã, introduza um nome para a execução do teste.
1. Indique se a VM reporta ao Linux ou ao Windows. Dependendo da escolha efetuada, selecione as opções subsequentes.

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>**Ligar a ferramenta de certificação a uma imagem de VM do Linux**
1. Selecione o modo de autenticação de SSH: palavra-passe ou ficheiro de chave.
1. Se utilizar autenticação baseada em palavra-passe, introduza o nome, nome de utilizador e palavra-passe do sistema de nomes de domínio (DNS).
1. Se utilizar a autenticação do ficheiro de chave, introduza o nome DNS, o nome de utilizador e a localização da chave privada.

   ![Autenticação de palavra-passe da imagem de VM do Linux][img-cert-vm-pswd-lnx]

   ![Autenticação de chave de ficheiro de imagem de VM do Linux][img-cert-vm-key-lnx]

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Ligar a ferramenta de certificação a uma imagem VM baseada no Windows**
1. Introduza o nome de VM DNS completamente qualificado (por exemplo, MyVMName.Cloudapp.net).
1. Introduza o nome de utilizador e palavra-passe.

   ![Autenticação de palavra-passe da imagem de VM do Windows][img-cert-vm-pswd-win]

Depois de selecionar as opções corretas para a sua imagem VM baseado em Windows ou do Linux, selecione **Testar ligação** para garantir que o SSH.Net ou o PowerShell tem uma ligação válida para fins de teste. Após uma conexão é estabelecida, selecione **seguinte** para iniciar o teste.

Quando o teste estiver concluído, receberá os resultados (Pass/Fail/Warning) para cada elemento de teste.

![Casos de teste de imagem de VM do Linux][img-cert-vm-test-lnx]

![Casos de teste de imagem de VM do Windows][img-cert-vm-test-win]

Se algum dos testes falharem, a imagem não irá ser certificada. Se isto ocorrer, reveja os requisitos e faça as alterações necessárias.

Após o teste automatizado, é-lhe perguntado para fornecer comentários adicionais sobre a imagem de VM por meio de uma tela de questionário.  Conclua as perguntas e, em seguida, selecione **seguinte**.

![Questionário de ferramenta de certificação][img-cert-vm-questionnaire]

![Questionário de ferramenta de certificação][img-cert-vm-questionnaire-2]

Depois de concluir o questionário, pode fornecer informações adicionais, tais como informações de acesso SSH para a VM do Linux, imagem e uma explicação para obter avaliações de qualquer falha. Pode transferir os resultados de teste e ficheiros de registo para os casos de teste foi executados, além das suas respostas ao questionário. Guarde os resultados no mesmo contentor que dos VHDs.

![Guardar os resultados do teste de certificação][img-cert-vm-results]

### <a name="52-get-the-shared-access-signature-uri-for-your-vm-images"></a>5.2 obter o URI de assinatura de acesso partilhado para as imagens de VM
Durante o processo de publicação, especificar os identificadores de recurso uniforme (URIs) que conduzem a cada um dos VHDs criados para o SKU. A Microsoft precisa de aceder a estes VHDs durante o processo de certificação. Por conseguinte, terá de criar um URI de assinatura de acesso partilhado para cada VHD. Este é o URI que deve ser introduzido no **imagens** separador no Portal de publicação.

A assinatura de acesso partilhado criado deve obedecer aos seguintes requisitos:

Nota: as instruções seguintes são aplicáveis apenas para discos não geridos que são as únicas suportada.

* Ao gerar a assinatura de acesso partilhado URIs para os VHDs, as permissões lista e leitura são suficientes. Não forneça acesso de Escrita ou de Eliminação.
* A duração do acesso deve ser um mínimo de três (3) semanas desde quando é criado o URI de assinatura de acesso partilhado.
* Para proteger-se para a hora UTC, selecione o dia antes da data atual. Por exemplo, se a data atual é de 6 de Outubro de 2014, selecione 10/5/2014.

URL de SAS pode ser gerada de várias formas de partilhar o seu VHD para o Azure Marketplace.
Seguem-se a 3 ferramentas recomendadas:

1.  Explorador do Storage do Azure
1.  Explorador de armazenamento da Microsoft
1.  CLI do Azure

**Explorador de armazenamento do Azure (recomendado para utilizadores do Windows)**

Seguem-se os passos para gerar o URL de SAS através do Explorador de armazenamento do Azure

1. Baixe [armazenamento do Azure Explorer 6 pré-visualização 3](https://azurestorageexplorer.codeplex.com/) do CodePlex. Aceda a [pré-visualização do Azure Storage Explorer 6](https://azurestorageexplorer.codeplex.com/) e clique em **"Downloads"**.

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_01.png)

1. Baixe [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) e instalar após descomprimi-lo.

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_02.png)

1. Depois de ser instalado, abra a aplicação.
1. Clique em **adicionar conta**.

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_03.png)

1. Especifique o nome da conta de armazenamento, a chave de conta de armazenamento e o domínio de pontos finais de armazenamento. Esta é a conta de armazenamento na sua subscrição do Azure onde tiver mantido o VHD no portal do Azure.

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_04.png)

1. Assim que o Explorador de armazenamento do Azure está ligado à sua conta de armazenamento específica, irá começar mostrando todos os contém na conta de armazenamento. Selecione o contentor onde copiou o ficheiro VHD de disco do sistema operativo (também discos de dados se estas forem aplicáveis para o seu cenário).

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_05.png)

1. Depois de selecionar o contentor de BLOBs, o Explorador de armazenamento do Azure é iniciado mostrando os arquivos dentro do contentor. Selecione o ficheiro de imagem (. vhd) que tem de ser submetido.

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_06.png)

1.  Depois de selecionar o ficheiro. vhd no contentor, clique nas **segurança** separador.

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_07.png)

1.  Na **segurança do contentor de BLOBs** caixa de diálogo caixa, deixe as predefinições no **nível de acesso** separador e, em seguida, clique em **assinaturas de acesso partilhado** separador.

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_08.png)

1.  Siga os passos abaixo para gerar um URI de assinatura de acesso partilhado para a imagem de VHD:

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_09.png)

    a. **Acesso permitido a partir de:** para salvaguardar para a hora UTC, selecione o dia antes da data atual. Por exemplo, se a data atual é de 6 de Outubro de 2014, selecione 10/5/2014.

    b. **Permissão de acesso para:** Selecione uma data que seja, pelo menos, três semanas após a **acesso permitido de** data.

    c. **Ações permitidas:** selecione o **lista** e **leitura** permissões.

    d. Se selecionou corretamente o ficheiro. vhd, então, seu arquivo aparece na **nome do Blob para aceder a** com. VHD de extensão.

    e. Clique em **gerar assinatura**.

    f. Na **gerado partilhado acesso assinatura URI desse contêiner**, verifique o seguinte conforme realçado acima:

       - Certifique-se de que sua imagem de nome de ficheiro e **". vhd"** são no URI.
       - No final da assinatura, certifique-se de que **"= rl"** aparece. Isto demonstra que o acesso de leitura e lista foi fornecido com êxito.
       - No meio da assinatura, certifique-se de que **"sr = c"** aparece. Isto demonstra que tem acesso ao nível do contentor

1.  Para garantir que o gerado compartilhados funciona URI de assinatura de acesso, clique em **Test in Browser**. Ele deve começar o processo de transferência.

1.  Copie o URI de assinatura de acesso partilhado. Este é o URI que deverá colar no Portal de Publicação.

1.  Repita os passos 6 a 10 para cada VHD no SKU.

**Explorador de armazenamento do Microsoft Azure (Windows/MAC/Linux)**

Seguem-se os passos para gerar o URL de SAS com o Explorador de armazenamento do Microsoft Azure

1.  Baixe o formulário do Microsoft Azure Storage Explorer [ http://storageexplorer.com/ ](http://storageexplorer.com/) Web site. Aceda a [Explorador de armazenamento do Microsoft Azure](http://storageexplorer.com/releasenotes.html) e clique em **"Transferir para Windows"**.

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_10.png)

1.  Depois de ser instalado, abra a aplicação.

1.  Clique em **adicionar conta**.

1.  Configurar o Explorador de armazenamento do Microsoft Azure à sua subscrição, inicie sessão na sua conta

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_11.png)

1.  Aceder à conta de armazenamento e selecione o contentor

1.  Selecione **"Obter assinatura de acesso de partilha..."** com o clique direito do **contentor**

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_12.png)

1.  Hora de início de atualização, o tempo de expiração e as permissões de acordo com o seguinte

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_13.png)

    a.  **Hora de início:** para salvaguardar para a hora UTC, selecione o dia antes da data atual. Por exemplo, se a data atual é de 6 de Outubro de 2014, selecione 10/5/2014.

    b.  **Hora de expiração:** Selecione uma data que seja, pelo menos, três semanas após a **Start Time** data.

    c.  **Permissões:** selecione o **lista** e **leitura** permissões

1.  Copie o URI de assinatura de acesso partilhado do contentor

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_14.png)

    URL de SAS gerado é para o nível de contêiner e agora, precisamos adicionar o nome do VHD no mesmo.

    Formato do URL de SAS de nível de contêiner: `https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Inserir o nome do VHD após o nome de contentor no URL de SAS, conforme mostrado a seguir `https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Exemplo:

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_15.png)

    TestRGVM201631920152.vhd é o nome do VHD, em seguida, vai ser o URL de SAS de VHD `https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    - Certifique-se de que sua imagem de nome de ficheiro e **". vhd"** são no URI.
    - No meio da assinatura, certifique-se de que **"sp = rl"** aparece. Isto demonstra que o acesso de leitura e lista foi fornecido com êxito.
    - No meio da assinatura, certifique-se de que **"sr = c"** aparece. Isto demonstra que tem acesso ao nível do contentor

1.  Para garantir que o gerado compartilhados funciona URI de assinatura de acesso, testá-la no browser. Este deve iniciar o processo de transferência

1.  Copie o URI de assinatura de acesso partilhado. Este é o URI que deverá colar no Portal de Publicação.

1.  Repita estes passos para cada VHD no SKU.

**CLI do Azure (recomendada para integração contínua e de não Windows)**

Seguem-se os passos para gerar o URL de SAS com a CLI do Azure

1.  Transferir o Microsoft Azure CLI partir [aqui](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/). Também pode encontrar ligações diferentes para **[Windows](http://aka.ms/webpi-azure-cli)** e  **[MAC OS](http://aka.ms/mac-azure-cli)**.

1.  Depois de transferida, instale o

1.  Criar um PowerShell (ou outro executável de script) de ficheiros com o código seguinte e guarde-o localmente

          $conn="DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<Storage Account Key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl <Permission End Date> -c $conn --start <Permission Start Date>  

    Atualize os parâmetros seguintes no acima

    a. **`<StorageAccountName>`**: A dar o nome da sua conta de armazenamento

    b. **`<Storage Account Key>`**: Dar a sua chave de conta de armazenamento

    c. **`<Permission Start Date>`**: Para salvaguardar para a hora UTC, selecione o dia antes da data atual. Por exemplo, se a data atual é de 25 de Outubro de 2016, em seguida, valor deve ser 10/25/2016. Se utilizar a CLI 2.0 do Azure (comando az), forneça a data e a hora no início e datas de término, por exemplo: 10-25-2016T00:00:00Z.

    d. **`<Permission End Date>`**: Selecione uma data que seja, pelo menos, três semanas após a **data de início**. O valor deve estar **11/02/2016**. Se utilizar a CLI 2.0 do Azure (comando az), forneça a data e a hora no início e datas de término, por exemplo: 11-02-2016T00:00:00Z.

    Segue-se o código de exemplo depois de atualizar os parâmetros corretos

          $conn="DefaultEndpointsProtocol=https;AccountName=st20151;AccountKey=<account-key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl 11/02/2016 -c $conn --start 10/25/2016  

1.  Abra o editor do Powershell com o modo de "Executar como administrador" e abra o ficheiro no passo 3 de #. Pode utilizar qualquer editor de script que está disponível no seu SO.

1.  Execute o script e irá fornecer-lhe o URL de SAS para acesso ao nível do contentor

    Seguinte será a saída da assinatura SAS e copiar a parte realçada num bloco de notas

    ![desenho](media/marketplace-publishing-vm-image-creation/img5.2_16.png)

1.  Agora, irá obter URL de SAS de nível de contêiner e tem de adicionar o nome do VHD no mesmo.

    URL de SAS de nível contentor #

    `https://st20151.blob.core.windows.net/vhds?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

1.  Insira o nome do VHD após o nome do contentor no URL de SAS conforme mostrado abaixo `https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    Exemplo:

    TestRGVM201631920152.vhd é o nome do VHD, em seguida, vai ser o URL de SAS de VHD

    `https://st20151.blob.core.windows.net/vhds/ TestRGVM201631920152.vhd?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    - Certifique-se de que seu nome de ficheiro de imagem e ". vhd" no URI.
    -   No meio da assinatura, certifique-se de que "sp = rl" é apresentada. Isto demonstra que o acesso de leitura e lista foi fornecido com êxito.
    -   No meio da assinatura, certifique-se de que "sr = c" é apresentada. Isto demonstra que tem acesso ao nível do contentor

1.  Para garantir que o gerado compartilhados funciona URI de assinatura de acesso, testá-la no browser. Este deve iniciar o processo de transferência

1.  Copie o URI de assinatura de acesso partilhado. Este é o URI que deverá colar no Portal de Publicação.

1.  Repita estes passos para cada VHD no SKU.


### <a name="53-provide-information-about-the-vm-image-and-request-certification-in-the-publishing-portal"></a>5.3 fornecem informações sobre a imagem de VM e solicitar a certificação no Portal de publicação
Depois de criar a oferta e o SKU, que deve introduzir os detalhes de imagem associados esse SKU:

1. Vá para o [Portal de publicação][link-pubportal]e, em seguida, inicie sessão com a sua conta de vendedor.
1. Selecione o **imagens de VM** separador.
1. O identificador listado na parte superior da página é, na verdade, o identificador da oferta e não ao identificador do SKU.
1. Preencha as propriedades sob o **SKUs** secção.
1. Sob **família do sistema operativo**, clique no tipo de sistema operativo associado ao sistema de operativo VHD.
1. Na **sistema operativo** caixa, descreva o sistema operativo. Considere um formato, como a família do sistema operativo, tipo, versão e atualizações. Um exemplo é "Windows Server Datacenter 2014 R2".
1. Selecione até seis tamanhos de máquinas virtuais recomendados. Estas são recomendações apresentadas para o cliente no painel de escalão de preço no Portal do Azure quando este decide comprar e implementar a sua imagem. **Estas são apenas recomendações. O cliente é capaz de selecionar qualquer tamanho VM que englobe os discos especificados na sua imagem.**
1. Introduza a versão. O campo versão encapsula uma versão semântica para identificar o produto e suas atualizações:
   * Versões devem ter o formato x.y. z, em que X, Y e Z são números inteiros.
   * Imagens em SKUs diferentes podem ter diferentes versões principais e secundárias.
   * Versões dentro de um SKU só devem ser as alterações incrementais, aumentam a versão de patch (Z do x.y. z).
1. Na **URL de VHD do SO** , introduza a URI criado para o sistema de operativo VHD de assinatura de acesso partilhado.
1. Se existirem discos de dados associados este SKU, selecione o número de unidade lógica (LUN) para o qual pretende que este disco de dados seja montado aquando da implementação.
1. Na **URL de VHD LUN X** , introduza a URI criado para o primeiro VHD de dados de assinatura de acesso partilhado.

    ![desenho](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)


## <a name="common-sas-url-issues--fixes"></a>URL de SAS problemas comuns & de correções

|Problema|Mensagem de falha|Corrigir|Hiperligação para a documentação|
|---|---|---|---|
|Falha na cópia dos imagens - "?" não foi encontrado no url SAS|Falha: Copiar as imagens. Não é possível transferir o blob com fornecida a Uri de SAS.|Atualizar o Url de SAS com recomendado de ferramentas|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Imagens de falha em Copiar - parâmetros "st" e "se" não está no url de SAS|Falha: Copiar as imagens. Não é possível transferir o blob com fornecida a Uri de SAS.|Atualize o Url de SAS com datas de início e fim no mesmo|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Falha na cópia dos imagens - "sp = rl" não está no url de SAS|Falha: Copiar as imagens. Não é possível transferir o blob com fornecida a Uri de SAS|Atualizar o Url de SAS com as permissões definidas como "Leitura" & "List|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Falha na cópia dos imagens - url de SAS tem espaços em branco no nome do vhd|Falha: Copiar as imagens. Não é possível transferir o blob com fornecida a Uri de SAS.|Atualizar o Url de SAS, sem espaços em branco|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Falha na cópia dos imagens – erro de autorização de Url de SAS|Falha: Copiar as imagens. Não é possível transferir blob devido a erro de autorização|Voltar a gerar o Url de SAS|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Falha na cópia dos imagens – o Url de SAS "st" e os parâmetros de "se" não tem a especificação de data / hora completa|Falha: Copiar as imagens. Não é possível transferir blob devido a Url de SAS incorretos |Parâmetros de Url de SAS começar e a data de fim ("st", "se") são necessários para ter a especificação completa de data / hora, por exemplo, 11-02-2017T00:00:00Z e não apenas a data ou versões abreviadas para o tempo. É possível encontrar este cenário através do Azure CLI 2.0 (comando az). Certifique-se de que fornecem a especificação de data / hora completa e voltar a gerar o Url de SAS.|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|

## <a name="next-step"></a>Passo seguinte
Depois de terminar com os detalhes SKU, pode avançar para o [guia de conteúdo de marketing do Azure Marketplace][link-pushstaging]. Essa etapa do processo de publicação, fornecer o conteúdo de marketing, os preços e outras informações necessárias anteriores ao **passo 3: testar a sua VM oferecem para efeitos de teste**, onde testar vários cenários de casos de utilização antes de implementar o oferta no Azure Marketplace para visibilidade pública e de compra.  

## <a name="see-also"></a>Consulte também
* [Introdução: como publicar uma oferta no Azure Marketplace](marketplace-publishing-getting-started.md)

[img-acom-1]:media/marketplace-publishing-vm-image-creation/vm-image-acom-datacenter.png
[img-portal-vm-size]:media/marketplace-publishing-vm-image-creation/vm-image-portal-size.png
[img-portal-vm-create]:media/marketplace-publishing-vm-image-creation/vm-image-portal-create-vm.png
[img-portal-vm-location]:media/marketplace-publishing-vm-image-creation/vm-image-portal-location.png
[img-portal-vm-rdp]:media/marketplace-publishing-vm-image-creation/vm-image-portal-rdp.png
[img-azstg-add]:media/marketplace-publishing-vm-image-creation/vm-image-storage-add.png
[img-manage-vm-new]:media/marketplace-publishing-vm-image-creation/vm-image-manage-new.png
[img-manage-vm-select]:media/marketplace-publishing-vm-image-creation/vm-image-manage-select.png
[img-cert-vm-key-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-keyfile-linux.png
[img-cert-vm-pswd-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-linux.png
[img-cert-vm-pswd-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-win.png
[img-cert-vm-test-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-linux.png
[img-cert-vm-test-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-win.png
[img-cert-vm-results]:media/marketplace-publishing-vm-image-creation/vm-image-certification-results.png
[img-cert-vm-questionnaire]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire.png
[img-cert-vm-questionnaire-2]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire-2.png
[img-pubportal-vm-skus]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus.png
[img-pubportal-vm-skus-2]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-2.png

[link-pushstaging]:marketplace-publishing-push-to-staging.md
[link-github-waagent]:https://github.com/Azure/WALinuxAgent
[link-azure-codeplex]:https://azurestorageexplorer.codeplex.com/
[link-azure-2]:../storage/blobs/storage-dotnet-shared-access-signature-part-2.md
[link-azure-1]:../storage/common/storage-dotnet-shared-access-signature-part-1.md
[link-msft-download]:http://www.microsoft.com/download/details.aspx?id=44299
[link-technet-3]:https://technet.microsoft.com/library/hh846766.aspx
[link-technet-2]:https://msdn.microsoft.com/library/dn495261.aspx
[link-azure-portal]:https://portal.azure.com
[link-pubportal]:https://publish.windowsazure.com
[link-sql-2014-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014enterprisewindowsserver2012r2/
[link-sql-2014-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014standardwindowsserver2012r2/
[link-sql-2014-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014webwindowsserver2012r2/
[link-sql-2012-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2enterprisewindowsserver2012/
[link-sql-2012-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2standardwindowsserver2012/
[link-sql-2012-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2webwindowsserver2012/
[link-datactr-2012-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012r2datacenter/
[link-datactr-2012]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012datacenter/
[link-datactr-2008-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2008r2sp1/
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-technet-1]:https://technet.microsoft.com/library/hh848454.aspx
[link-azure-vm-2]:./virtual-machines-linux-agent-user-guide/
[link-openssl]:https://www.openssl.org/
[link-intsvc]:http://www.microsoft.com/download/details.aspx?id=41554
[link-python]:https://www.python.org/

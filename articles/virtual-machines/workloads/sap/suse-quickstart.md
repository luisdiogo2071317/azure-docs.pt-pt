---
title: Testar o SAP NetWeaver em VMs do Linux do Microsoft Azure SUSE | Documentos da Microsoft
description: Testar o SAP NetWeaver em VMs do Linux do Microsoft Azure SUSE
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 645e358b-3ca1-4d3d-bf70-b0f287498d7a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/14/2017
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 032ab2a221f64d01af25056a4eff3ee3384de0c3
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2019
ms.locfileid: "54157229"
---
# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Executar o SAP NetWeaver em VMs SUSE Linux do Microsoft Azure
Este artigo descreve vários aspetos a considerar quando estiver a executar o SAP NetWeaver em máquinas virtuais do Microsoft Azure SUSE Linux (VMs). A partir de 19 de Maio de 2016 é oficialmente suportado SAP NetWeaver em VMs do SUSE Linux no Azure. Todos os detalhes sobre as versões de Linux, versões de kernel do SAP e outros pré-requisitos podem ser encontrados no SAP 1928533 de nota "SAP Applications no Azure: Produtos suportados e tipos de VM do Azure ".
Mais documentação sobre o SAP em VMs do Linux pode ser encontrada aqui: [Utilizar o SAP nas máquinas virtuais do Linux (VMs)](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

As seguintes informações devem ajudar a evitar algumas armadilhas potenciais.

## <a name="suse-images-on-azure-for-running-sap"></a>Imagens SUSE no Azure para executar o SAP
Para executar o SAP NetWeaver no Azure, utilize o SUSE Linux Enterprise Server SLES 12 (SPx) ou SLES para SAP – Consulte também a nota SAP 1928533. Uma imagem SUSE especial é no Azure Marketplace ("SLES 11 SP3 para SAP CAL"), mas a imagem, não se destina a ser utilização geral. Não utilize esta imagem porque este está reservado para o [SAP Cloud Appliance Library](https://cal.sap.com/) solução.  

Tem de utilizar a estrutura de implementação Azure Resource Manager para todas as instalações no Azure. A procurar imagens do SUSE SLES e as versões com o Azure PowerShell ou a interface de linha de comandos (CLI) do Azure, utilize os comandos mostrados abaixo. Em seguida, pode utilizar a saída, por exemplo, para definir a imagem do SO num modelo JSON para implementar uma nova VM do Linux SUSE.
Estes comandos do PowerShell são válidas para o Azure PowerShell versão 1.0.1 e versões posteriores.

Embora seja possível utilizar as imagens standard do SLES para instalações do SAP, é recomendado utilizar o novo SLES para imagens SAP. Estas imagens estão agora disponíveis na Galeria de imagens do Azure. Podem encontrar mais informações sobre estas imagens no correspondente [página do Azure Marketplace]( https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SUSE.SLES-SAP ) ou o [página da web SUSE FAQ sobre SLES para SAP]( https://www.suse.com/products/sles-for-sap/frequently-asked-questions/ ).


* Procure por publicadores existentes, incluindo o SUSE:
  
   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```
* Procure ofertas atuais do SUSE:
  
   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```
* Procure ofertas do SUSE SLES:
  
   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP"
   CLI : azure vm image list-skus westeurope SUSE SLES
   CLI : azure vm image list-skus westeurope SUSE SLES-SAP
   ```
* Procure por uma versão específica de um SKU de SLES:
  
   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12-SP2"
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP" -skus "12-SP2"
   CLI : azure vm image list westeurope SUSE SLES 12-SP2
   CLI : azure vm image list westeurope SUSE SLES-SAP 12-SP2
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Instalar WALinuxAgent numa VM de SUSE
O agente chamado WALinuxAgent faz parte das imagens SLES no Azure Marketplace. Para obter informações sobre a instalá-lo manualmente (por exemplo, ao carregar um disco de rígido virtual SLES SO (VHD) no local), consulte:

* [openSUSE](http://software.opensuse.org/package/WALinuxAgent)
* [Azure](../../linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SUSE](https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP "monitorização avançada"
SAP "avançado de monitorização" é um pré-requisito obrigatório para executar o SAP no Azure. Verifique os detalhes em SAP note 2191498 "SAP no Linux com o Azure: Aprimorado de monitorização".

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Anexar discos de dados do Azure para uma VM do Linux do Azure
Nunca discos de dados do Azure de montagem para uma VM do Linux do Azure com o ID de dispositivo. Em alternativa, utilize o identificador exclusivo universalmente (UUID). Tenha cuidado ao utilizar ferramentas gráficas para discos de dados do Azure de montagem, por exemplo. Verifique novamente as entradas em /etc/fstab.

O problema com o ID de dispositivo é que podem ser alteradas e, em seguida, a VM do Azure poderá bloquear no processo de arranque. Para mitigar o problema, pode adicionar o parâmetro nofail em /etc/fstab. Contudo, tenha cuidado com nofail porque aplicativos poderão utilizar o ponto de montagem como antes e podem gravar no sistema de arquivos de raiz no caso de um disco de dados do Azure externos não foi montado durante o arranque.

A única exceção a montagem por meio do UUID é anexar um disco de SO para a resolução, conforme descrito na secção que se seguem.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Resolução de problemas de uma VM de SUSE que não esteja acessível mais
Podem existir situações em que uma VM de SUSE no Azure são paradas do processo de inicialização (por exemplo, com um erro relacionado com a montagem de discos). Pode verificar este problema ao utilizar a funcionalidade de diagnóstico de arranque para máquinas virtuais do Azure v2 no portal do Azure. Para obter mais informações, consulte [diagnósticos de arranque](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Uma forma de resolver o problema é anexar o disco do SO da VM danificada a outra VM SUSE no Azure. Em seguida, efetue as alterações necessárias, como editar /etc/fstab. ou remover regras de udev de rede, conforme descrito na secção seguinte.

Há uma coisa importante a ter em consideração. Implementar várias VMs do SUSE partir da mesma imagem do Azure Marketplace (por exemplo, o SLES 11 SP4) faz com que o disco do SO de ser montado sempre pelo mesmo UUID. Por conseguinte, usando o UUID para anexar um disco de SO a partir de uma VM diferente, que foi implementada com os mesmos resultados de imagem do Azure Marketplace em dois UUIDs idênticos. Dois causa de UUIDs não idêntico a VM utilizada para a resolução de problemas, inicializando a partir do disco de SO anexado e danificado em vez do disco do SO original.

Existem duas formas de evitar problemas:

* Utilize uma imagem diferente do Azure Marketplace para a VM de resolução de problemas (por exemplo, SLES 11 SPx em vez de SLES 12).
* Não anexar o disco do SO danificado partir de outra VM com o UUID – utilizar algo mais.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Carregar uma VM de SUSE no local para o Azure
Para obter uma descrição dos passos para carregar uma VM de SUSE no local para o Azure, veja [preparar uma máquina virtual SLES ou openSUSE para o Azure](../../linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Se pretender carregar uma VM sem o passo de desaprovisionamento no final (por exemplo, para manter uma instalação existente do SAP, bem como o nome de anfitrião), verifique os seguintes itens:

* Certifique-se de que o disco do SO está montado com o UUID e não o ID de dispositivo. Alterar para o UUID just-in /etc/fstab. não é suficiente para o disco do SO. Além disso, não se esqueça de adaptar o carregador de inicialização por meio do YaST ou editando /boot/grub/menu.lst.
* Se utilizar o formato VHDX para o disco do SO do SUSE e convertê-lo para o VHD para carregamento para o Azure, é provável que o dispositivo de rede é alterado de eth0 para eth1. Para evitar problemas quando estiver inicializando no Azure mais tarde, reverta para eth0, conforme descrito em [corrigir eth0 no clonados SLES 11 VMware](https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Além de que está descrito no artigo, recomendamos que remova este ficheiro:

   /lib/udev/Rules.d/75-persistent-NET-Generator.Rules

Também pode instalar o agente do Linux do Azure (waagent) para o ajudar a evitar potenciais problemas, desde que não existem várias NICs.

## <a name="deploying-a-suse-vm-on-azure"></a>Implementar uma VM de SUSE no Azure
Deve criar novas VMs SUSE usando arquivos de modelo JSON no novo modelo do Azure Resource Manager. Depois de criar o ficheiro de modelo JSON, pode implementar a VM ao utilizar o seguinte comando da CLI como uma alternativa ao PowerShell:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Para obter mais informações sobre arquivos de modelo JSON, veja [modelos Authoring Azure Resource Manager](../../../resource-group-authoring-templates.md) e [modelos de início rápido do Azure](https://azure.microsoft.com/documentation/templates/).

Para obter mais informações sobre a CLI clássica do Azure e Azure Resource Manager, consulte [utilizar a CLI clássica do Azure para Mac, Linux e Windows com o Azure Resource Manager](../../../xplat-cli-azure-resource-manager.md).

## <a name="sap-license-and-hardware-key"></a>Chave de licenciamento e hardware SAP
Para obter uma certificação de SAP para o Azure oficial, um novo mecanismo foi introduzido para calcular a chave de hardware SAP que é utilizada para a licença do SAP. O kernel SAP tinha que ser adaptados para fazer uso do novo algoritmo. Versões anteriores do kernel SAP para o Linux não incluía essa alteração de código. Por conseguinte, em determinadas situações (por exemplo, a VM do Azure de redimensionamento), alterar a chave de hardware SAP e a licença do SAP foi já não é válido. Uma solução é fornecida com kernels de Linux do SAP mais recentes.  Os patches de kernel SAP detalhados estão documentados na nota SAP 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>Pacote de sapconf SUSE / adm ajustado
SUSE fornece um pacote chamado "sapconf", que gere um conjunto de definições específicas do SAP. Para obter mais informações sobre o que faz este pacote e como instalar e utilizá-lo, consulte:  [Utilizar o sapconf para preparar um SUSE Linux Enterprise Server para executar sistemas SAP](https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) e [What ' s sapconf ou como preparar um SUSE Linux Enterprise Server para a execução de sistemas SAP?](http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

Entretanto, há uma nova ferramenta, que substitui 'sapconf - adm ajustado'. É possível encontrar mais informações sobre esta ferramenta duas ligações a seguir:

- Pode encontrar documentação SLES sobre o perfil de 'adm ajustado' sap-hana [aqui](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_saptune.html) 

- Sistemas de ajuste para cargas de trabalho de SAP com 'ajustado-adm' - podem ser encontradas [aqui](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) capítulo 6.2

## <a name="nfs-share-in-distributed-sap-installations"></a>Em instalações de SAP distribuídas de partilha NFS
Se tiver uma instalação distribuída – por exemplo, onde pretende instalar a base de dados e os servidores de aplicações SAP em VMs separadas-- pode compartilhar o diretório de /sapmnt por meio de NFS Network File System (). Se tiver problemas com os passos de instalação depois de criar a partilha NFS para /sapmnt, verifique se "no_root_squash" é definido para a partilha.

## <a name="logical-volumes"></a>Volumes lógicos
No passado, se uma for necessário um grande volume de lógico em vários discos de dados do Azure (por exemplo, para a base de dados do SAP), ele foi recomendado que utilize a ferramenta de gerenciamento de Raid MDADM, uma vez que o Linux Manager de Volume lógicos (LVM) não foi totalmente validada ainda no Azure. Para saber como configurar o RAID de Linux no Azure utilizando mdadm, veja [configurar o RAID de software no Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Entretanto, a partir do início de Maio de 2016, Gestor de volumes lógicos do Linux é totalmente suportado no Azure e pode ser utilizado como uma alternativa ao MDADM. Para obter mais informações sobre LVM no Azure, leia:  
[Configurar LVM numa VM do Linux no Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-suse-repository"></a>Repositório SUSE do Azure
Se tiver um problema com o acesso ao repositório Azure SUSE padrão, pode utilizar um comando para repô-lo. Esse tipo de problema pode acontecer se criar uma imagem privada do sistema operacional numa região do Azure e, em seguida, copie a imagem para uma região diferente do Azure para implementar novas VMs que se baseiam nesta imagem de SO privada. Execute o seguinte comando no interior da VM:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Ambiente de trabalho gnome
Se pretender utilizar a área de trabalho Gnome para instalar um sistema completo para a demonstração SAP dentro de uma única VM, incluindo uma GUI do SAP, o navegador e a consola de gestão da SAP-- utilizam desta sugestão para instalá-lo nas imagens SLES do Azure:

   Para SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Para SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>Suporte do SAP para Oracle em Linux na cloud
Há uma restrição de suporte da Oracle no Linux em ambientes virtualizados. Embora esta restrição de suporte não é um tópico específico do Azure, é importante compreender. SAP não suporta o Oracle no SUSE ou no Red Hat numa nuvem pública, como o Azure. Enquanto isso em execução, Oracle DB no Azure é totalmente suportada pela SAP no Oracle Linux (consulte 1928533 de nota SAP). Se outras combinações são necessárias, contacte diretamente o Oracle.


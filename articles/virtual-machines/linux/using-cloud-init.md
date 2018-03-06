---
title: "Descrição geral do suporte de nuvem init para computadores virtuais Linux no Azure | Microsoft Docs"
description: "Descrição geral das capacidades de nuvem init no Microsoft Azure"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: fbb6fc15663570d9b9470fc7d4de3c8eb30de9d9
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="cloud-init-support-for-virtual-machines-in-azure"></a>Suporte de nuvem init para máquinas virtuais no Azure
Este artigo explica o suporte se existe para [nuvem init](https://cloudinit.readthedocs.io) para configurar uma máquina virtual ou máquina virtual (VM) conjuntos de dimensionamento (VMSS) no aprovisionamento de tempo no Azure. Estes scripts de nuvem init executam no primeiro arranque depois dos recursos foram aprovisionados através do Azure.  

## <a name="cloud-init-overview"></a>Descrição geral da inicialização de cloud
[Cloud-init](https://cloudinit.readthedocs.io) é uma abordagem amplamente utilizada para personalizar uma VM com Linux quando arranca pela primeira vez. Pode utilizar o cloud-init para instalar pacotes e escrever ficheiros ou para configurar utilizadores e segurança. Uma vez init de nuvem é chamado durante o processo de arranque inicial, existem não existem passos adicionais ou agentes necessários para aplicar a configuração.  Para mais informações sobre como corretamente formatar o `#cloud-config` ficheiros, consulte o [site de documentação de nuvem init](http://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config` os ficheiros são ficheiros de texto codificados em base64.

O cloud-init também funciona em distribuições. Por exemplo, não utiliza **apt-get install** nem **yum install** para instalar um pacote. Em vez disso, pode definir uma lista dos pacotes a instalar. O cloud-init utiliza automaticamente a ferramenta de gestão de pacotes nativa para a distribuição que selecionar.

 Estamos ativamente a trabalhar com os nossos parceiros de distro Linux endorsed para ter imagens de nuvem-init ativada disponíveis no Azure marketplace. Estas imagens fará com que as implementações de nuvem init e configurações funcionam sem problemas com as VMs e conjuntos de dimensionamento da VM (VMSS). A tabela seguinte descreve a disponibilidade de imagens de nuvem-init ativada atual na plataforma do Azure:

| Publicador | Oferta | SKU | Versão | nuvem init pronto
|:--- |:--- |:--- |:--- |:--- |:--- |
|Canónico |UbuntuServer |16.04-LTS |mais recente |sim | 
|Canónico |UbuntuServer |14.04.5-LTS |mais recente |sim |
|CoreOS |CoreOS |Estável |mais recente |sim |
|OpenLogic |CentOS |7-CI |mais recente |pré-visualização |
|RedHat |RHEL |7-RAW-CI |mais recente |pré-visualização |

Pilha do Azure não suporta atualmente o aprovisionamento do RHEL 7.4 e 7.4 CentOS utilizando init de nuvem.

## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>O que é a diferença entre init de nuvem e o agente Linux (WALA)?
WALA é um agente específico da plataforma do Azure utilizado para aprovisionar, configurar VMs e processar extensões do Azure. Iremos são melhorando a tarefa de configurar VMs para utilizar init nuvem em vez do agente Linux para permitir que os clientes de nuvem init existentes utilizar os scripts de nuvem init atual.  Se tiver investimentos em scripts de nuvem init para configurar os sistemas Linux, existem **sem definições adicionais necessárias** para ativá-los. 

Se não incluir a CLI do Azure `--custom-data` comutador no aprovisionamento de tempo, WALA assume a parâmetros necessários para aprovisionar a VM e concluir a implementação com as predefinições de aprovisionamento de VM mínima.  Se referenciar a nuvem-init `--custom-data` comutador, que está contido nos seus dados personalizados (definições individuais ou script completo) substitui as predefinições WALA. 

Configurações de WALA de VMs são restrita tempo a funcionar dentro da tempo de aprovisionamento de VM máxima.  Configurações de nuvem init aplicadas às VMs tem restrições de tempo e não irão causar uma falha por exceder o tempo limite da implementação. 

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Implementação de uma nuvem-init ativada a Máquina Virtual
Implementar uma máquina virtual de nuvem-init ativada é tão simple como referenciar uma distribuição de nuvem-init ativada durante a implementação.  Maintainers de distribuição de Linux têm de optar por ativar e integrar nuvem init nas suas imagens de publicados base do Azure. Assim que tiver confirmado a imagem que pretende implementar é nuvem-init ativado, pode utilizar a CLI do Azure para implementar a imagem. 

O primeiro passo para implementar esta imagem é criar um grupo de recursos com o [criar grupo az](/cli/azure/group#az_group_create) comando. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. 

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
O próximo passo é criar um ficheiro na sua shell atual, com o nome *nuvem init.txt* e cole a seguinte configuração. Neste exemplo, crie o ficheiro na Shell na nuvem não no seu computador local. Pode utilizar qualquer editor que desejar. Introduza `sensible-editor cloud-init.txt` para criar o ficheiro e ver uma lista dos editores disponíveis. Escolha #1 para utilizar o **nano** editor. Certifique-se de que o ficheiro de inicialização da cloud é copiado corretamente, especialmente a primeira linha:

```yaml
#cloud-config
package_upgrade: true
packages:
  - httpd
```
Prima `ctrl-X` para sair do ficheiro, escreva `y` para guardar o ficheiro e prima `enter` para confirmar o nome de ficheiro ao sair.

O último passo consiste em criar uma VM com o [az vm criar](/cli/azure/vm#az_vm_create) comando. 

O exemplo seguinte cria uma VM chamada *centos74* e cria as chaves SSH se estes ainda não existir numa localização chave predefinido. Para utilizar um conjunto específico de chaves, utilize a opção `--ssh-key-value`.  Utilize o parâmetro `--custom-data` para passar o ficheiro de configuração de inicialização da cloud. Forneça o caminho completo para a configuração do *cloud-init.txt*, se tiver guardado o ficheiro fora do diretório de trabalho atual. O exemplo seguinte cria uma VM chamada *centos74*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

Quando a VM foi criada, a CLI do Azure mostra informações específicas para a sua implementação. Tome nota do `publicIpAddress`. Este endereço é utilizado para aceder à VM.  Demora algum tempo para a VM ser criada, os pacotes para instalar e iniciar a aplicação. Existem tarefas em segundo plano que continuam a ser executadas após a CLI do Azure devolver o utilizador à linha de comandos. Pode SSH para a VM e utilize os passos descritos na secção de resolução de problemas para ver os registos de nuvem init. 

## <a name="troubleshooting-cloud-init"></a>Resolução de problemas de init de nuvem
Depois da VM tenha sido aprovisionada, init nuvem será executado através de todos os módulos e de script definido no `--custom-data` para configurar a VM.  Se precisar de resolver quaisquer erros ou omissões da configuração, terá de procurar o nome do módulo (`disk_setup` ou `runcmd` por exemplo) no registo de nuvem init - localizados em **/var/log/cloud-init.log**.

> [!NOTE]
> Nem todas as falhas de módulo resulta numa nuvem-init fatal falha de configuração geral. Por exemplo, utilizando o `runcmd` módulo, se o script falhar, nuvem init ainda comunicará aprovisionamento com êxito porque o módulo de runcmd foi executado.

Para obter mais detalhes de registo de nuvem init, consulte o [documentação init de nuvem](http://cloudinit.readthedocs.io/en/latest/topics/logging.html) 

## <a name="next-steps"></a>Passos Seguintes
Para obter exemplos de nuvem init das alterações de configuração, consulte os seguintes documentos:
 
- [Adicionar um utilizador de Linux adicional para uma VM](cloudinit-add-user.md)
- [Executar um Gestor de pacote para atualizar pacotes existentes no primeiro arranque](cloudinit-update-vm.md)
- [Alterar o nome local do anfitrião de VM](cloudinit-update-vm-hostname.md) 
- [Instalar um pacote de aplicações, ficheiros de configuração de atualização e injetar chaves](tutorial-automate-vm-deployment.md)

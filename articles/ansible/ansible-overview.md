---
title: Utilizar Ansible com o Azure
description: "Introdução à utilização Ansible para automatiza a nuvem de aprovisionamento, gestão de configuração e implementações de aplicações."
ms.service: ansible
keywords: "ansible, do azure, devops, descrição geral, aprovisionar de nuvem, gestão de configuração, implementação de aplicação, ansible módulos, ansible playbooks"
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/19/2018
ms.topic: article
ms.openlocfilehash: a7ce3c239a50462a9af137eb958268f72dbf79d1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="ansible-with-azure"></a>Ansible com o Azure

[Ansible](http://www.ansible.com) é um produto de open source que automatiza o aprovisionamento de nuvem, a gestão de configuração e implementações de aplicações. Utilizar Ansible pode aprovisionar máquinas virtuais, contentores e da rede e concluir infraestruturas de nuvem. Além disso, Ansible permite-lhe automatizar a implementação e configuração de recursos no seu ambiente.

Este artigo fornece uma descrição geral básica de algumas das vantagens da utilização Ansible com o Azure.

## <a name="ansible-playbooks"></a>Ansible playbooks

[Ansible playbooks](http://docs.ansible.com/ansible/latest/playbooks.html) são idioma do Ansible de orquestração, configuração e implementação. Pode descrevem uma política que pretende que os sistemas remotos para impor ou um conjunto de passos um processo geral de TI. Quando cria um manual de comunicação social fazê-lo ao utilizar YAML, que define um modelo de uma configuração ou de um processo.

## <a name="ansible-modules"></a>Módulos de Ansible

Ansible inclui um conjunto de [Ansible módulos](http://docs.ansible.com/ansible/latest/modules_by_category.html) que pode ser executado diretamente no anfitriões remotos ou através de [playbooks](http://docs.ansible.com/ansible/latest/playbooks.html). Os utilizadores também podem criar os seus próprios módulos. Módulos podem ser utilizados para controlar os recursos do sistema - como serviços, pacotes ou ficheiros - ou executar comandos de sistema.

Para interagir com os serviços do Azure, Ansible inclui um conjunto de [módulos de nuvem Ansible](http://docs.ansible.com/ansible/list_of_cloud_modules.html#azure) que fornece as ferramentas para facilmente criar e orquestrar a sua infraestrutura no Azure. 

## <a name="migrate-existing-workload-to-azure"></a>Migrar a carga de trabalho existente para o Azure

Assim que tiver utilizado o Ansible para definir a sua infraestrutura, pode aplicar manual de comunicação social da sua aplicação, permitindo que o Azure, dimensionar o seu ambiente conforme necessário. 

## <a name="automate-cloud-native-application-in-azure"></a>Automatizar a aplicação nativa na nuvem no Azure

Ansible permite-lhe automatizar aplicações nativa na nuvem no Azure através do Azure micro-serviços como [das funções do Azure](https://azure.microsoft.com//services/functions/) e [Kubernetes no Azure](https://azure.microsoft.com/services/container-service/kubernetes/)).  

## <a name="manage-deployments-with-dynamic-inventory"></a>Gerir as implementações com o inventário dinâmicas
Através do respetivo [inventário dinâmicas](http://docs.ansible.com/ansible/intro_dynamic_inventory.html) funcionalidade, Ansible fornece a capacidade para inventário de solicitação a partir dos recursos do Azure. Em seguida, pode marcar as implementações do Azure existentes e gerir esses marcado implementações através de Ansible.

## <a name="additional-azure-marketplace-options"></a>Opções adicionais do Azure Marketplace
O [Ansible Torre](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) imagem do Azure Marketplace pelo Red Hat ajuda as organizações a automatização de TI de dimensionar e gerir com implementações complexas em físicos, virtuais e infraestruturas de cloud. Torre de Ansible inclui funcionalidades que fornecem níveis adicionais de visibilidade, controlo, segurança e eficiência necessária para as empresas de hoje. Torre de Ansible encripta credenciais tal como chaves do Azure e o SSH para que pode delegar tarefas aos funcionários menos experiente sem o risco de exposição das suas credenciais.

## <a name="next-steps"></a>Passos Seguintes
- [Configurar Ansible](/azure/virtual-machines/linux/ansible-install-configure?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Criar uma VM com Linux](/azure/virtual-machines/linux/ansible-create-vm?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
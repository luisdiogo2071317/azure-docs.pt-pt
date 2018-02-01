---
title: "Matriz módulo e a versão Ansible para o Azure"
description: "Matriz módulo e a versão Ansible para o Azure"
ms.service: ansible
keywords: "ansible, funções, matriz, versão, azure, devops"
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/19/2018
ms.topic: article
ms.openlocfilehash: da5d1a8277d87a771b080ef9cefb3b40448d1563
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="ansible-module-and-version-matrix"></a>Matriz de módulo e a versão de Ansible

## <a name="ansible-modules-for-azure"></a>Ansible módulos do Azure
Ansible é fornecido com um número de módulos que podem ser executados diretamente nos anfitriões remotos ou através de playbooks.
Este artigo apresenta os módulos Ansible do Azure que pode aprovisionar os recursos de nuvem do Azure como máquina virtual, redes e serviços de contentor. Pode obter estes módulos do lançamento oficial do Ansible ou das seguintes funções do manual de comunicação social publicadas pela Microsoft.

| Módulo de Ansible para o Azure                   |  Ansible 2.4 |  Manual de comunicação social função [azure_module](#introduction-to-azuremodule) |  Manual de comunicação social função [azure_preview_module](#introduction-to-azurepreviewmodule) | 
|---------------------------------------------|--------------|-----------------------------|-------------------------------------| 
| **Computação**                    |           |                          |                                  | 
| azure_rm_availabilityset                    | Sim          | Sim                         | Sim                                 | 
| azure_rm_availabilityset_facts              | Sim          | Sim                         | Sim                                 | 
| azure_rm_deployment                         | Sim          | Sim                         | Sim                                 | 
| azure_rm_virtualmachine_scaleset_facts      | Sim          | Sim                         | Sim                                 | 
| azure_rm_virtualmachineimage_facts          | Sim          | Sim                         | Sim                                 | 
| azure_rm_resourcegroup                      | Sim          | Sim                         | Sim                                 | 
| azure_rm_resourcegroup_facts                | Sim          | Sim                         | Sim                                 | 
| azure_rm_virtualmachine                     | Sim          | Sim                         | Sim                                 | 
| azure_rm_virtualmachine_extension           | Sim          | Sim                         | Sim                                 | 
| azure_rm_virtualmachine_scaleset            | Sim          | Sim                         | Sim                                 | 
| **Redes**                    |           |                          |                                  | 
| azure_rm_virtualnetwork                     | Sim          | Sim                         | Sim                                 | 
| azure_rm_virtualnetwork_facts               | Sim          | Sim                         | Sim                                 | 
| azure_rm_subnet                             | Sim          | Sim                         | Sim                                 | 
| azure_rm_networkinterface                   | Sim          | Sim                         | Sim                                 | 
| azure_rm_networkinterface_facts             | Sim          | Sim                         | Sim                                 | 
| azure_rm_publicipaddress                    | Sim          | Sim                         | Sim                                 | 
| azure_rm_publicipaddress_facts              | Sim          | Sim                         | Sim                                 | 
| azure_rm_dnsrecordset                       | Sim          | Sim                         | Sim                                 | 
| azure_rm_dnsrecordset_facts                 | Sim          | Sim                         | Sim                                 | 
| azure_rm_dnszone                            | Sim          | Sim                         | Sim                                 | 
| azure_rm_dnszone_facts                      | Sim          | Sim                         | Sim                                 | 
| **Armazenamento**                    |           |                          |                                  | 
| azure_rm_loadbalancer                       | Sim          | Sim                         | Sim                                 | 
| azure_rm_loadbalancer_facts                 | Sim          | Sim                         | Sim                                 | 
| azure_rm_applicationgateway                 | -            | Sim                         |                                     | 
| azure_rm_applicationgateway_facts           | -            | -                           | Sim                                 | 
| azure_rm_securitygroup                      | -            | -                           | Sim                                 | 
| azure_rm_securitygroup_facts                | -            | -                           | Sim                                 | 
| azure_rm_storageaccount                     | Sim          | Sim                         | Sim                                 | 
| azure_rm_storageaccount_facts               | Sim          | Sim                         | Sim                                 | 
| azure_rm_storageblob                        | Sim          | Sim                         | Sim                                 | 
| azure_rm_managed_disk                       | Sim          | Sim                         | Sim                                 | 
| azure_rm_managed_disk_facts                 | Sim          | Sim                         | Sim                                 | 
| **Contentores**                    |           |                          |                                  | 
| azure_rm_acs                                | Sim          | Sim                         | Sim                                 | 
| azure_rm_containerinstance                  | -            | Sim                        |                                     | 
| azure_rm_containerinstance_facts            | -            | -                           | Sim                                 | 
| azure_rm_containerregistry                  | -            | Sim                         | Sim                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | Sim                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | Sim                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | Sim                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | Sim                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | Sim                                 | 
| **Funções do Azure**                    |           |                          |                                  | 
| azure_rm_functionapp                        | Sim          | Sim                         | Sim                                 | 
| azure_rm_functionapp_facts                  | Sim          | Sim                         | Sim                                 | 
| **Bases de dados**                    |           |                          |                                  | 
| azure_rm_sqlserver                          | -            | Sim                         | Sim                                 | 
| azure_rm_sqlserver_facts                    | -            | -                           | Sim                                 | 
| azure_rm_sqldatabase                        | -            | -                           | Sim                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | Sim                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | Sim                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | Sim                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | Sim                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | Sim                                 | 
| azure_rm_mysqlserver                        | -            | Sim                         | Sim                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | Sim                                 | 
| azure_rm_mysqldatabase                      | -            | -                           | Sim                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | Sim                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | Sim                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | Sim                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | Sim                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | Sim                                 | 
| azure_rm_postgresqlserver                   | -            | Sim                         | Sim                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | Sim                                 | 
| azure_rm_postgresqldatabase                 | -            | -                           | Sim                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | Sim                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | Sim                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | Sim                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | Sim                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | Sim                                 | 

## <a name="introduction-to-azuremodule"></a>Introdução ao azure_module
O [função do manual de comunicação social azure_module](https://galaxy.ansible.com/Azure/azure_modules/) inclui as alterações e correções de erros de módulos do Azure da mais recente do [devel ramo do repositório Ansible](https://github.com/ansible/ansible/tree/devel). Se não puder aguardar versão seguinte do Ansible, instalar a função de azure_module é uma boa opção.

A função do manual de comunicação social azure_module for lançada a cada três semanas.

## <a name="introduction-to-azurepreviewmodule"></a>Introdução ao azure_preview_module
O [função do manual de comunicação social azure_preview_module](https://galaxy.ansible.com/Azure/azure_preview_modules/) é a função mais completa e inclui todos os módulos do Azure mais recentes. As atualizações e correções de erros são efetuadas atempadamente mais que a versão de Ansible oficial. Se utilizar Ansible para fins de aprovisionamento de recursos do Azure, está encorajados a instalar a função de azure_preview_module.

A função do manual de comunicação social azure_preview_module for lançada a cada três semanas.

## <a name="next-steps"></a>Passos Seguintes
Obter mais informações relacionadas com as funções do manual de comunicação social, consulte [criar Playbooks de Reusable](http://docs.ansible.com/ansible/latest/playbooks_reuse.html). 
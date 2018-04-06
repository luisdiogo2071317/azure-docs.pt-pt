---
title: Matriz módulo e a versão Ansible para o Azure
description: Matriz módulo e a versão Ansible para o Azure
ms.service: ansible
keywords: ansible, funções, matriz, versão, azure, devops
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 03/25/2018
ms.topic: article
ms.openlocfilehash: 011cb173ffdecc7a22c2e470209719ccaf6bda58
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="ansible-module-and-version-matrix"></a>Matriz de módulo e a versão de Ansible

## <a name="ansible-modules-for-azure"></a>Ansible módulos do Azure
Ansible é fornecido com um número de módulos que podem ser executados diretamente nos anfitriões remotos ou através de playbooks.
Este artigo apresenta os módulos Ansible do Azure que pode aprovisionar os recursos de nuvem do Azure como máquina virtual, redes e serviços de contentor. Pode obter estes módulos do lançamento oficial do Ansible ou das seguintes funções do manual de comunicação social publicadas pela Microsoft.

| Módulo de Ansible para o Azure                   |  Ansible 2.4 |  Ansible 2.5 |  Manual de comunicação social função [azure_preview_module](#introduction-to-azurepreviewmodule) | 
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
| azure_rm_image                              |              | Sim                         | Sim                                 | 
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
| azure_rm_loadbalancer                       | Sim          | Sim                         | Sim                                 | 
| azure_rm_loadbalancer_facts                 | Sim          | Sim                         | Sim                                 | 
| azure_rm_appgw                              | -            | -                           | Sim                                 | 
| azure_rm_appgwroute                         | -            | -                           | Sim                                 | 
| azure_rm_appgwroute                         | -            | -                           | Sim                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | Sim                                 |
| azure_rm_appgwroutetable                    | -            | -                           | Sim                                 |
| azure_rm_securitygroup                      | Sim          | Sim                         | Sim                                 | 
| azure_rm_appgwroutetable_facts              | -            | -                           | Sim                                 | 
| **Armazenamento**                    |           |                          |                                  | 
| azure_rm_storageaccount                     | Sim          | Sim                         | Sim                                 | 
| azure_rm_storageaccount_facts               | Sim          | Sim                         | Sim                                 | 
| azure_rm_storageblob                        | Sim          | Sim                         | Sim                                 | 
| azure_rm_managed_disk                       | Sim          | Sim                         | Sim                                 | 
| azure_rm_managed_disk_facts                 | Sim          | Sim                         | Sim                                 | 
| **Contentores**                    |           |                          |                                  | 
| azure_rm_acs                                | Sim          | Sim                         | Sim                                 | 
| azure_rm_containerinstance                  | -            | Sim                         | Sim                                 | 
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
| azure_rm_sqlserver_facts                    | -            | Sim                         | Sim                                 | 
| azure_rm_sqldatabase                        | -            | Sim                         | Sim                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | Sim                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | Sim                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | Sim                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | Sim                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | Sim                                 | 
| azure_rm_mysqlserver                        | -            | Sim                         | Sim                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | Sim                                 | 
| azure_rm_mysqldatabase                      | -            | Sim                         | Sim                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | Sim                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | Sim                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | Sim                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | Sim                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | Sim                                 | 
| azure_rm_postgresqlserver                   | -            | Sim                         | Sim                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | Sim                                 | 
| azure_rm_postgresqldatabase                 | -            | Sim                         | Sim                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | Sim                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | Sim                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | Sim                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | Sim                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | Sim                                 | 
| **Cofre de Chaves**                    |           |                          |                                  | 
| azure_rm_keyvault                           | -            | Sim                         | Sim                                 |
| azure_rm_keyvault_facts                     | -            | -                           | Sim                                 |
| azure_rm_keyvaultkey                        | -            | Sim                         | Sim                                 |
| azure_rm_keyvaultsecret                     | -            | Sim                         | Sim                                 |


## <a name="introduction-to-playbook-role-for-azure"></a>Introdução à função do manual de comunicação social do Azure
O [função do manual de comunicação social azure_preview_module](https://galaxy.ansible.com/Azure/azure_preview_modules/) é a função mais completa e inclui todos os módulos do Azure mais recentes. As atualizações e correções de erros são efetuadas atempadamente mais que a versão de Ansible oficial. Se utilizar Ansible para fins de aprovisionamento de recursos do Azure, está encorajados a instalar a função de azure_preview_module.

A função do manual de comunicação social azure_preview_module for lançada a cada três semanas.

## <a name="next-steps"></a>Passos Seguintes
Obter mais informações relacionadas com as funções do manual de comunicação social, consulte [criar Playbooks de Reusable](http://docs.ansible.com/ansible/latest/playbooks_reuse.html). 

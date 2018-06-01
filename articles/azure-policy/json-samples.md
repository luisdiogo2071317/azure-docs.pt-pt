---
title: Exemplos de modelo de política
description: Exemplos JSON para o Azure Policy
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 01/17/2018
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 4b9096c1fb0d9ee74849e259a6e0af2486c5d29b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34195129"
---
# <a name="templates-for-azure-policy"></a>Modelos do Azure Policy

A seguinte tabela inclui ligações para modelos json do Azure Policy. Estes exemplos encontram-se no [repositório de exemplos do Azure Policy](https://github.com/Azure/azure-policy).

| | |
|---|---|
|**Computação**||
| [Imagens de VM aprovadas](scripts/allowed-custom-images.md) | Requer que sejam implementadas apenas imagens personalizadas aprovadas no seu ambiente. Especifica uma matriz de IDs de imagens aprovadas. |
| [Auditar quando a VM não utiliza o Disco Gerido](scripts/create-vm-managed-disk.md) | Faz uma auditoria quando é criada uma máquina virtual que não utiliza discos geridos.|
| [Auditar se a extensão não existir](scripts/audit-ext-not-exist.md) | Faz uma auditoria se uma extensão não estiver implementada numa máquina virtual. Especifica o publicador da extensão e o tipo para verificar se foi implementada. |
| [Permitir que a imagem de VM personalizada seja de um Grupo de Recursos](scripts/allow-custom-vm-image.md) |  Requer que as imagens personalizadas sejam provenientes de um grupo de recursos aprovado. Especifica o nome do grupo de recursos aprovado. |
| [Negar o benefício de utilização híbrida](scripts/deny-hybrid-use.md) | Proibe a utilização do Benefício de Utilização Híbrida do Azure (AHUB). Utilize quando não quiser permitir a utilização de licenças no local. |
| [Extensões de VM não permitidas](scripts/not-allowed-vm-ext.md) | Proíbe a utilização de extensões especificadas. Especifica uma matriz que contém os tipos de extensão proibidos. |
| [Permitir apenas uma determinada imagem da plataforma de VM](scripts/allow-certain-vm-image.md) | Requer que as máquinas virtuais utilizem uma versão específica do UbuntuServer. |
| [Criar VM com o Disco Gerido](scripts/use-managed-disk-vm.md) | Requer que as máquinas virtuais utilizem discos geridos.|
|**Monitorização**||
| [Definição de diagnóstico de auditoria](scripts/audit-diag-setting.md) | Audita se as definições de diagnóstico não estão ativadas para tipos de recursos especificados. Especifica uma matriz de tipos de recursos para verificar se as definições de diagnóstico estão ativadas. |
|**Convenções de nome e texto**||
| [Permitir múltiplos padrões de nome](scripts/allow-multiple-name-patterns.md) | Permita que um dos muitos padrões de nomes seja utilizado para recursos. |
| [Exigir padrão like](scripts/enforce-like-pattern.md) | Certifique-se de que os nomes de recursos cumprem a condição like de um padrão. |
| [Exigir padrão de correspondência](scripts/enforce-match-pattern.md) | Certifique-se de que os nomes de recursos correspondem a um padrão de nomenclatura. |
| [Exigir padrão de correspondência de etiqueta](scripts/enforce-tag-match-pattern.md) | Certifique-se de que um valor de etiqueta corresponde a um padrão de texto. |
|**Rede**||
| [SKUs de Gateways de Aplicação permitidos](scripts/allowed-app-gate-sku.md) | Requer que os gateways de aplicação utilizem um SKU aprovado. Especifica uma matriz de SKUs aprovados. |
| [Auditar se o Observador de Rede não estiver ativado para a região](scripts/net-watch-not-enabled.md) | Faz uma auditoria se o observador de rede não estiver ativado para uma região especificada. Especifica o nome da região para verificar se o observador de rede está ativado. |
| [NSG X em cada NIC](scripts/nsg-on-nic.md) | Exige que um grupo de segurança de rede específico seja utilizado em cada interface de rede virtual. Especifica o ID do grupo de segurança de rede a utilizar. |
| [NSG X em cada sub-rede](scripts/nsg-on-subnet.md) | Exige que um grupo de segurança de rede específico seja utilizado em cada sub-rede virtual. Especifica o ID do grupo de segurança de rede a utilizar. |
| [Largura de banda de rotas Express permitida](scripts/allowed-er-band.md) | Requer que as rotas express utilizem um conjunto especificado de larguras de banda. Especifica uma matriz de SKUs que pode ser especificada para uma Rota Express. |
| [Localização de Peering Permitida para uma Rota Express](scripts/allowed-peering-er.md) | Requer que as rotas Express utilizem localizações de peering especificadas. Especifica uma matriz de localizações de peering permitidas. |
| [SKUs de Rotas Express permitidos](scripts/allowed-er-skus.md) | Requer que as Rotas Express utilizem um SKU aprovado. Especifica uma matriz de SKUs permitidos. |
| [SKUs de Balanceadores de Carga permitidos](scripts/allowed-lb-skus.md) | Requer que os balanceadores de carga utilizem um SKU aprovado. Especifica uma matriz de SKUs permitidos. |
| [Sem peering de rede à rede do ER](scripts/no-peering-er-net.md) | Proíbe um peering de rede de ser associado a uma rede num grupo de recursos especificado. Utilize para impedir a ligação com a infraestrutura de rede gerida central. Especifique o nome do grupo de recursos para impedir a associação. |
| [Nenhuma Tabela de Rotas Definida pelo Utilizador](scripts/no-user-def-route-table.md)  |Proíbe a implementação de redes virtuais numa tabela de rotas definida pelo utilizador. |
| [SKUs de Gateways de Rede Virtual Permitidos](scripts/allowed-vn-gate-sku.md) | Requer que os gateways de rede virtual utilizem um tipo de SKU e gateway aprovado. Especifica uma matriz de SKUs aprovados e uma matriz de tipos de gateway aprovados. |
| [Utilizar sub-rede aprovada para interfaces de rede de VM](scripts/use-approved-subnet-vm-nics.md) | Exige que as interfaces de rede utilizem uma sub-rede aprovada. Especifica o ID da sub-rede aprovada. |
| [Utilizar vNet aprovada para interfaces de rede de VM](scripts/use-approved-vnet-vm-nics.md) | Exige que as interfaces de rede utilizem uma rede virtual aprovada. Especifica o ID da rede virtual aprovada. |
|**Etiquetas**||
| [Iniciativa de Política de Etiquetas de Faturação](scripts/billing-tags-policy-init.md) | Exige valores de etiqueta especificados para o nome do produto e o centro de custos. Utiliza políticas incorporadas para aplicar e impor etiquetas necessárias. O utilizador especifica os valores necessários para as etiquetas.  |
| [Impor a etiqueta e o respetivo valor em grupos de recursos](scripts/enforce-tag-rg.md) | Exige uma etiqueta e um valor num grupo de recursos. Especifica o nome de etiqueta e o valor necessário.  |
|**SQL**||
| [Auditar a Definição de Auditoria do Nível de DB SQL](scripts/audit-sql-db-audit-setting.md) | Faz uma auditoria das definições de auditoria da base de dados SQL se essas definições não corresponderem a uma definição especificada. Especifica um valor que indica se as definições de auditoria devem estar ativadas ou desativadas.  |
| [Auditar o estado de encriptação de dados transparente](scripts/audit-trans-data-enc-status.md) | Faz uma auditoria se a encriptação de dados transparente da base de dados SQL não tiver ativada.  |
| [Auditar a definição de deteção de ameaças do nível de BD](scripts/audit-db-threat-det-setting.md) | Faz uma auditoria às políticas de alerta de segurança da base de dados SQL se as políticas não estiverem definidas para o estado especificado. Especifica um valor que indica se a deteção de ameaças está ativada ou desativada.  |
| [Auditar a Definição de Auditoria do Nível do SQL Server](scripts/audit-sql-ser-leve-audit-setting.md) | Faz uma auditoria das definições de auditoria do SQL Server se essas definições não corresponderem a uma definição especificada. Especifica um valor que indica se as definições de auditoria devem estar ativadas ou desativadas. |
| [Auditar a definição de deteção de ameaças do nível do Servidor](scripts/audit-sql-ser-threat-det-setting.md) | Faz uma auditoria às políticas de alerta de segurança da base de dados SQL se as políticas não estiverem definidas para o estado especificado. Especifica um valor que indica se a deteção de ameaças está ativada ou desativada.  |
| [Auditar se não existir um administrador do Azure Active Directory](scripts/audit-no-aad-admin.md) | Auditar quando não existe nenhum administrador do Azure Active Directory atribuído ao servidor SQL. |
| [SKUs de DB SQL permitidos](scripts/allowed-sql-db-skus.md) | Requer que as bases de dados SQL utilizem um SKU aprovado. Especifica uma matriz de IDs ou nomes de SKUs permitidos. |
|**Armazenamento**||
| [SKUs Permitidos para Contas de Armazenamento e Máquinas Virtuais](scripts/allowed-skus-storage.md) | Requer que as contas de armazenamento e as máquinas virtuais utilizem SKUs aprovados. Utiliza políticas incorporadas para garantir SKUs aprovados. Especifique uma matriz de SKUs de máquinas virtuais aprovadas e uma matriz de SKUs de contas de armazenamento aprovadas. |
| [Assegurar o tráfego https apenas para contas de armazenamento](scripts/ensure-https-stor-acct.md) | Requer que as contas de armazenamento utilizem o tráfego HTTPS.  |
| [Negar camadas de acesso esporádico para contas de armazenamento](scripts/deny-cool-access-tiering.md) | Proíbe a utilização de camadas de acesso esporádico para contas de armazenamento de blobs.  |
| [Assegurar a encriptação de ficheiros de armazenamento](scripts/ensure-store-file-enc.md) | Requer que a encriptação de ficheiros esteja ativada para contas de armazenamento.  |
|**Política Incorporada**||
| [Localizações permitidas](scripts/allowed-locs.md) | Exige que todos os recursos sejam implementados nas localizações aprovadas. Especifica uma matriz de localizações aprovadas.  |
| [Tipos de recursos permitidos](scripts/allowed-res-types.md) | Garante que apenas os tipos de recursos aprovados são implementados. Especifica uma matriz dos tipos de recursos que são permitidos.  |
| [SKUs de contas de armazenamento permitidos](scripts/allowed-stor-acct-skus.md) | Requer que as contas de armazenamento utilizem um SKU aprovado. Especifica uma matriz de SKUs aprovados. |
| [Aplicar etiqueta e o respetivo valor predefinido](scripts/apply-tag-def-val.md) | Acrescenta um nome de etiqueta e um valor especificado, se essa etiqueta não for fornecida. Especifica o nome de etiqueta e o valor a aplicar.  |
| [Auditar a encriptação da Base de Dados SQL](scripts/sql-database-encryption-audit.md) | Faz uma auditoria se a base de dados SQL não tiver a encriptação de dados transparente ativada. |
| [Auditar as definições de auditoria do SQL Server](scripts/sql-server-audit.md) | Faz uma auditoria ao SQL Server para verificar se as definições de auditoria estão ativadas. |
| [Aplicar encriptação do Data Lake Store](scripts/enforce-datalakestore-encryption.md) | Recusa todas as contas do Data Lake Store que não tenham a encriptação ativada. |
| [Impor etiqueta e o respetivo valor](scripts/enforce-tag-val.md) | Exige um nome de etiqueta e um valor especificado. Especifica o nome de etiqueta e o valor a impor.  |
| [Tipos de recursos não permitidos](scripts/not-allowed-res-type.md) | Proíbe a implementação de tipos de recursos especificados. Especifica uma matriz dos tipos de recursos a bloquear.  |
| [Requer a Versão do SQL Server 12.0](scripts/req-sql-12.md) | Requer que os servidores do SQL utilizem a versão 12.0.  |
| [Requerer a encriptação de contas de armazenamento](scripts/req-store-acct-enc.md) | Requer que a conta de armazenamento utilize a encriptação de blobs.  |

---
title: Implementação de DBMS de máquinas virtuais do Azure para SAP NetWeaver | Documentos da Microsoft
description: Implementação de DBMS de máquinas virtuais do Azure para SAP NetWeaver
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5654dac7-4204-4387-b312-3d8b2898eb3a
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/26/2018
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f6b0ea7479910f7026974e37f8c05099453c0b26
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/21/2018
ms.locfileid: "42059732"
---
# <a name="azure-virtual-machines-dbms-deployment-for-sap-netweaver"></a>Implementação de DBMS de máquinas virtuais do Azure para SAP NetWeaver
[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md 
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f 
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b 
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d 
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c 
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8
[dbms-guide-managed-disks]:dbms-guide.md#f42c6cb5-d563-484d-9667-b07ae51bce29

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md 
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab 
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e 
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e 
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc 
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d 
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f 
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca 
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b 
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d 
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md 
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff 
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f 
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a 
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c 
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef 
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a 
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d 
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 

[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd 
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f 

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
[sap-pam]:https://support.sap.com/pam 
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../windows/premium-storage.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:../../../resource-manager-deployment-model.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md 
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md 
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability-linux]:../../linux/manage-availability.md
[virtual-machines-manage-availability-windows]:../../windows/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes-linux]:../../linux/sizes.md
[virtual-machines-sizes-windows]:../../windows/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/resources/templates/sql-server-2014-alwayson-existing-vnet-and-ad/
[virtual-network-deploy-multinic-arm-cli]:../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/template-samples.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/manage-virtual-network.md#create-a-virtual-network
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-network-deploy-multinic-classic-ps.md
[virtual-networks-nsg]:../../../virtual-network/security-overview.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Este guia faz parte da documentação sobre como implementar e implantar o software SAP no Microsoft Azure. Antes de ler este guia, leia os [guia de implementação e planeamento][planning-guide]. Este documento aborda a implementação de vários sistemas de gestão de base de dados relacional (RDBMS) e produtos relacionados em combinação com o SAP em máquinas de virtuais de Azure do Microsoft (VMs) com a infraestrutura do Azure como um capacidades do serviço (IaaS).

O documento complementa a documentação de instalação de SAP e SAP Notes, que representam os recursos principais para instalações e implementações de SAP software em determinadas plataformas.

## <a name="general-considerations"></a>Considerações gerais
Neste capítulo, são introduzidas considerações da execução de sistemas relacionados com o SAP DBMS em VMs do Azure. Existem algumas referências para sistemas específicos do DBMS neste capítulo. Em vez disso, os sistemas DBMS específicos são tratados dentro deste documento, depois deste capítulo.

### <a name="definitions-upfront"></a>Definições de início
Em todo o documento, podemos utilizar os seguintes termos:

* IaaS: Infraestrutura como serviço.
* PaaS: Plataforma como um serviço.
* SaaS: Software como serviço.
* Componente SAP: um SAP aplicativos individuais como ECC, BW, Gestor de solução ou EP.  Componentes SAP podem basear-se em tecnologias tradicionais de ABAP ou Java ou um aplicativo de não com base em NetWeaver, como objetos comerciais.
* Ambiente de SAP: um ou mais componentes SAP logicamente agrupados para executar uma função de negócio, como o desenvolvimento, QAS, treinamento, DR ou de produção.
* Ambiente SAP: Refere-se para os ativos SAP inteiros num cliente paisagem IT. A paisagem SAP inclui todos os ambientes de não produção e produção.
* Sistema SAP: A combinação de camada do DBMS e camada de aplicativo de, por exemplo, um sistema de desenvolvimento SAP ERP, sistema de teste de SAP BW, sistema de produção do SAP CRM, etc. Nas implementações do Azure, não é suportada para dividir estas duas camadas entre no local e o Azure. Isso significa que um sistema SAP é implementado no local ou está implementado no Azure. No entanto, pode implantar os sistemas diferentes de um ambiente SAP no Azure ou no local. Por exemplo, pode implementar o desenvolvimento de CRM do SAP e testar sistemas no Azure, mas a SAP CRM produção sistema no local.
* Implementação apenas na cloud: uma implementação em que a subscrição do Azure não está ligada através de um site a site ou a ligação do ExpressRoute para a infraestrutura de rede no local. Em comum documentação do Azure esses tipos de implementações também são descritos como implementações de "Apenas na Cloud". Máquinas virtuais implementadas com este método são acedidas através da Internet e os pontos de extremidade do Internet públicos atribuídos às VMs no Azure. Os locais do Active Directory (AD) e DNS não estiver expandido para o Azure nesses tipos de implementações. Por conseguinte, as VMs não fazem parte do Active Directory no local. Nota: As implementações apenas na Cloud neste documento são definidas como concluídas panoramas SAP, que executem exclusivamente no Azure sem a extensão do Active Directory ou a resolução do nome do local para a cloud pública. Configurações de apenas na cloud não são suportadas para sistemas SAP de produção ou configurações onde SAP STMS ou a outros recursos no local tem de ser usada entre sistemas SAP alojados no Azure e recursos que residem no local.
* Em vários locais: Descreve um cenário em que as VMs são implementadas para uma subscrição do Azure que tem conectividade do ExpressRoute entre o datacenter(s) no local e o Azure, múltiplos sites ou site a site. Documentação do Azure em comum, esses tipos de implementações também são descritos como cenários de vários locais. O motivo para a ligação é para expandir os domínios no local, Active Directory no local e o DNS no local para o Azure. O Panorama de no local é expandido para recursos do Azure da subscrição. Com esta extensão, as VMs podem ser parte do domínio no local. Os utilizadores de domínio do domínio no local podem aceder aos servidores e podem executar serviços nessas VMS (como o DBMS serviços). Resolução de nomes e de comunicação entre VMs implementadas no local e as VMs implementadas no Azure, é possível. Esperamos que este seja o cenário mais comum para a implementação de ativos SAP no Azure. Para obter mais informações, consulte [este artigo] [ vpn-gateway-cross-premises-options] e [neste artigo][vpn-gateway-site-to-site-create].

> [!NOTE]
> Implementações em vários locais dos sistemas SAP em máquinas virtuais do Azure com sistemas SAP são membros de um domínio no local são suportadas para sistemas SAP de produção. Configurações em vários locais são suportadas para a implementação de partes ou concluir cenários SAP no Azure. Até mesmo executar o ambiente completo do SAP no Azure requer ter essas VMs a fazer parte do domínio no local e anúncios. Nas versões anteriores da documentação, falamos sobre cenários de TI híbrida, em que o termo *híbrida* está enraizada no fato de que existe uma conectividade em vários locais entre no local e o Azure. Neste caso *híbrida* também significa que as VMs no Azure fazem parte do Active Directory no local.
> 
> 

Alguma documentação de Microsoft descreve os cenários de em vários locais de forma um pouco diferente, especialmente para configurações de HA do DBMS. No caso dos documentos relacionados com o SAP, o cenário de vários locais se para baixo para ter uma conectividade de site a site ou privada (ExpressRoute) e ao fato de que o SAP paisagem é distribuído entre no local e o Azure.

### <a name="resources"></a>Recursos
Os guias seguintes estão disponíveis para implementações de SAP no Azure:

* [Máquinas de virtuais de planeamento e implementação para o SAP NetWeaver do Azure][planning-guide]
* [Implementação de máquinas virtuais do Azure para SAP NetWeaver][deployment-guide]
* [Implementação de DBMS de máquinas virtuais do Azure para SAP NetWeaver (neste documento)][dbms-guide]

As seguintes notas de SAP estão relacionadas com o SAP no Azure:

| Número de nota | Cargo |
| --- | --- |
| [1928533] |Aplicações SAP no Azure: produtos suportados e a VM do Azure tipos |
| [2015553] |SAP no Microsoft Azure: pré-requisitos do suporte |
| [1999351] |Resolução de problemas avançada a monitorização do Azure para SAP |
| [2178632] |Chave de métricas de monitorização para o SAP no Microsoft Azure |
| [1409604] |Virtualização no Windows: avançada de monitorização |
| [2191498] |SAP no Linux com o Azure: avançada de monitorização |
| [2039619] |Aplicações SAP no Microsoft Azure utilizando a base de dados do Oracle: produtos suportados e versões |
| [2233094] |DB6: Aplicações de SAP no Azure com o IBM DB2 para Linux, UNIX e Windows - informações adicionais |
| [2243692] |Linux no Microsoft Azure (IaaS) VM: problemas de licença do SAP |
| [1984787] |SUSE LINUX Enterprise Server 12: Observações de instalação |
| [2002167] |Red Hat Enterprise Linux 7.x: instalação e atualização |
| [2069760] |Instalação de SAP do Oracle Linux 7.x e atualização |
| [1597355] |Recomendação de espaço de comutação para Linux |
| [2171857] |Base de dados Oracle 12C - suporte de sistema de ficheiros no Linux |
| [1114181] |Oracle Database 11g - suporte de sistema de ficheiros no Linux |


Leia também a [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) que contém todas as anotações do SAP para o Linux.

Deve ter um conhecimento prático sobre a arquitetura do Microsoft Azure e como as máquinas virtuais do Microsoft Azure são implementados e em funcionamento. Pode encontrar mais informações em <https://azure.microsoft.com/documentation/>

> [!NOTE]
> Estamos **não** discutindo a plataforma Microsoft Azure como um ofertas de serviço (PaaS) da plataforma Microsoft Azure. Este documento trata de um sistema de gerenciamento de banco de dados (DBMS) no Microsoft Azure máquinas virtuais em execução (IaaS) à medida que executaria o DBMS no seu ambiente no local. Capacidades de base de dados e de funcionalidades entre estas duas ofertas são muito diferentes e não devem misturar entre si. Consulte também: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Uma vez que estamos são discutir IaaS, em geral a instalação do Windows, Linux e DBMS e configuração são, essencialmente, igual a qualquer máquina virtual ou máquina bare-metal, terá de instalar no local. No entanto, existem alguns gestão de arquitetura e o sistema suas decisões de implementação, que são diferentes, ao utilizar o IaaS. O objetivo deste documento é explicar as diferenças de gestão do sistema que precisa estar preparado para quando utilizar IaaS e específicos de arquitetura.

Em geral, as áreas gerais de diferença que este documento aborda são:

* Planear o layout VM/disco adequado dos sistemas SAP para se certificar de que tem os dados adequados layout de ficheiros e pode alcançar suficiente IOPS para a sua carga de trabalho.
* Considerações de rede ao utilizar o IaaS.
* Funcionalidades de base de dados específica à sua disposição para otimizar o esquema de base de dados.
* Considerações sobre backup e restauração em IaaS.
* Utilização de diferentes tipos de imagens para implantação.
* Elevada disponibilidade no Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Estrutura de um RDBMS de implementação
Para seguir este capítulo, é necessário compreender o que foi apresentado no [isso] [ deployment-guide-3] capítulo o [guia de implementação][deployment-guide]. Dados de conhecimento sobre a série de VM diferente e suas diferenças e as diferenças de padrão do Azure e o armazenamento Premium devem ser compreendidos e conhecido antes de ler este capítulo.

Até Março de 2015, discos, que contém um sistema operativo foram limitados a 127 GB de tamanho. Esta limitação tem elevada em Março de 2015 (para obter mais informações, veja <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/>). A partir daí em discos que contém o sistema operacional pode ter o mesmo tamanho que qualquer outro disco. No entanto, preferimos ainda uma estrutura de implementação em que o sistema operativo, o DBMS e a eventual binários SAP estão separados dos arquivos de banco de dados. Portanto, podemos esperar que os sistemas SAP em execução em máquinas de virtuais do Azure têm o VM base (ou disco) instalados com o sistema operativo, executáveis de sistema de gestão de base de dados e executáveis SAP. Os ficheiros de dados e de registo do DBMS são armazenados no armazenamento do Azure (armazenamento Standard ou Premium) em discos separados e anexados como discos lógicos para a imagem de sistema de operativo Azure VM original. 

Dependendo de tirar partido do Azure Standard ou o armazenamento Premium (por exemplo, ao utilizar a série DS ou VMs da série GS) aqui estão outras quotas no Azure, que estão documentadas [aqui (Linux)] [ virtual-machines-sizes-linux] e [ aqui (Windows)][virtual-machines-sizes-windows]. Ao planejar seu layout de disco, tem de encontrar o melhor equilíbrio das quotas para os seguintes itens:

* O número de ficheiros de dados.
* O número de discos que contêm os ficheiros.
* As quotas IOPS de um único disco.
* A taxa de transferência de dados por disco.
* O número de discos de dados adicionais possíveis por tamanho da VM.
* O débito global do armazenamento uma VM pode fornecer.

Azure impõe uma quota IOPS por disco de dados. Estas quotas são diferentes para discos alojados no armazenamento padrão do Azure e o armazenamento Premium. Latências de e/s também são muito diferentes entre os dois tipos de armazenamento com o armazenamento Premium, fornecendo fatores melhor latências de e/s. Cada um dos diferentes tipos VM tem um número limitado de discos de dados que pode anexar. Outra restrição é que apenas determinados tipos VM podem tirar partido do armazenamento Premium do Azure. Isso significa que a decisão para um determinado tipo VM pode não apenas ser orientada pelos requisitos de CPU e memória, mas também por IOPS, latência e disco requisitos de débito que normalmente são dimensionados com o número de discos ou o tipo de discos de armazenamento Premium. Especialmente com o armazenamento Premium o tamanho de um disco pode de ser ditado também pelo número de IOPS e débito de que tem de ser possível através de cada disco.

O fato de que a taxa de IOPS geral, o número de discos montados e o tamanho da VM estão todos ligados em conjunto, pode fazer com que uma configuração do Azure de um sistema SAP para ser diferente da sua implementação no local. Os limites IOPS por LUN são normalmente configuráveis em implementações no local. Ao passo que o armazenamento do Azure esses limites são fixo ou como no armazenamento Premium dependente do tipo de disco. Então, com implementações no local, podemos ver as configurações de cliente de servidores de base de dados que estão a utilizar muitas diferentes volumes para executáveis especiais, como SAP e o DBMS ou volumes especiais para bancos de dados temporários ou espaços de tabela. Quando um sistema de locais desse tipo é movido para o Azure, pode levar a uma perda da largura de banda IOPS potencial por perda de um disco para executáveis ou de bancos de dados, não Efetue qualquer uma ou não um monte de IOPS. Assim, em VMs do Azure é recomendável que os executáveis DBMS e SAP seja instalado no disco do SO, se possível.

A colocação de ficheiros de base de dados e ficheiros de registo e o tipo de armazenamento do Azure utilizado, deve ser definida por requisitos de débito, latência e IOPS. Para ter suficiente IOPS para o log de transação, será forçado a tirar partido de vários discos para o ficheiro de registo de transação ou utilizar um disco de armazenamento Premium maior. Nesse caso, um criaria um software RAID (por exemplo Windows Storage Pool para Windows ou MDADM e LVM (Gestor de volumes lógicos) para Linux) com os discos que contêm o registo de transações.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Unidade D:\ numa VM do Azure é uma unidade não persistente, que é suportada por alguns discos locais no nó de computação do Azure. Como é não persistente, isso significa que todas as alterações feitas para o conteúdo na unidade D:\ é perdido quando a VM é reiniciada. Por "quaisquer alterações", queremos dizer arquivos salvos, diretórios criados, aplicativos instalados, etc.
> 
> ![Linux][Logo_Linux] Linux
> 
> As VMs do Linux do Azure automaticamente montar uma unidade durante /mnt/resource que é uma unidade não persistente, apoiada por discos locais no nó de computação do Azure. Como é não persistente, isso significa que todas as alterações efetuadas ao conteúdo em /mnt/resource são perdidas quando a VM é reiniciada. Com todas as alterações, podemos dizer que arquivos salvos, diretórios criados, aplicativos instalados, etc.
> 
> 

- - -
Depende da série de VM do Azure, os discos locais no nó de computação mostram o desempenho de diferente, que pode ser categorizado como:

* A0-A7: Desempenho muito limitado. Não pode ser utilizada para qualquer coisa além do ficheiro de paginação do windows
* A8-A11: Características de desempenho muito bom, com algumas IOPS de dez mil e > débito de 1GB/seg
* Série de D: Características de desempenho muito bom, com algumas IOPS de dez mil e > débito de 1GB/seg
* Séries de DS: Características de desempenho muito bom, com algumas IOPS de dez mil e > débito de 1GB/seg
* Série G: Características de desempenho muito bom, com algumas IOPS de dez mil e > débito de 1GB/seg
* Série GS: Características de desempenho muito bom, com algumas IOPS de dez mil e > débito de 1GB/seg

Instruções acima são válidos para os tipos VM com certificação SAP. A série de VM com o excelente IOPS e débito se qualificam para tirar partido por alguns recursos do DBMS, como tempdb ou espaço de tabela temporária.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Colocação em cache para VMs e os discos de dados
Quando criamos discos de dados através do portal ou quando estamos carregados discos para VMs de montagem, podemos escolher se o tráfego de e/s entre a VM e esses discos localizados no armazenamento do Azure são armazenados em cache. O armazenamento Premium e Standard do Azure utilizam duas tecnologias diferentes para este tipo de cache. Em ambos os casos, o cache próprio seria feito nas unidades mesmo utilizadas pelo disco temporário (D:\ no Windows) ou /mnt/resource no Linux de VM de disco.

Para armazenamento padrão do Azure, os tipos de cache de possíveis são:

* Sem colocação em cache
* Leia a colocação em cache
* Ler e escrever a colocação em cache

Para obter um desempenho consistente e determinista, deve definir a colocação em cache no armazenamento padrão do Azure para todos os discos que contêm **arquivos de dados relacionados com o DBMS, ficheiros de registo e tabela espaço para 'NONE'**. A colocação em cache da VM pode permanecer com o padrão.

Existem as seguintes opções de colocação em cache para armazenamento Premium do Azure:

* Sem colocação em cache
* Leia a colocação em cache

Recomendação para o armazenamento Premium do Azure consiste em tirar partido **colocação em cache para ficheiros de dados de leitura** da base de dados do SAP e escolha **sem colocação em cache para os discos dos ficheiros de registo**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>RAID de software
Como já mencionado acima, tem de equilibrar o número de IOPS necessário para os ficheiros de base de dados entre o número de discos, pode configurar e o IOPS máximo de uma VM do Azure fornece por disco ou tipo de disco de armazenamento Premium. A maneira mais fácil lidar com a carga IOPS através de discos é criar um RAID de software em discos diferentes. Em seguida, coloca um número de ficheiros de dados do SAP DBMS em LUNS criados fora o RAID de software. Depende dos requisitos, que convém considerar a utilização do armazenamento Premium também desde duas de três diferentes discos de armazenamento Premium fornecem maior quota IOPS de discos com base no armazenamento Standard. Além da latência de e/s de melhor significativa, fornecida pelo armazenamento Premium do Azure. 

Mesmo se aplica para o log de transação de diferentes sistemas DBMS. Com muitos deles adicionar mais arquivos de Tlog não ajudam, uma vez que os sistemas DBMS escreverem em um dos arquivos de cada vez só. Se forem necessários a taxas de IOPS mais elevadas do que pode fornecer um único disco de armazenamento Standard com base, pode do stripe ao longo de vários discos de armazenamento Standard ou pode utilizar um tipo de disco de armazenamento Premium maior que, além das taxas de IOPS mais elevadas, também oferece latência mais baixa para a gravação de fatores / SO no log de transação.

Situações encontradas em implementações do Azure, o que seriam de favorece usando um RAID de software são:

* Registo de registo/Refazer transações exigem mais IOPS que o Azure fornece para um único disco. Conforme mencionado acima isso pode ser resolvido com a criação de um LUN ao longo de vários discos com um software de RAID.
* E/s carga de trabalho distribuição desigual os ficheiros de dados diferentes da base de dados SAP. Em tais casos um pode assistir a um ficheiro de dados em vez disso, muitas vezes, a atingir a quota. Ao passo que outros arquivos de dados não são até mesmo se aproximar da quota IOPS de um único disco. Nesses casos, a solução mais simples é criar um LUN sobre vários discos com um software de RAID. 
* Não sabe o que a carga de trabalho de e/s exata por ficheiro de dados é e apenas aproximadamente saber qual é a carga de trabalho IOPS geral contra o DBMS. Mais fácil de fazer é criar um LUN com a ajuda de um RAID de software. A soma de quotas de vários discos por trás este LUN, em seguida, deve cumprir a taxa IOPS conhecida.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Recomendamos que utilize espaços de armazenamento do Windows, se executar no Windows Server 2012 ou superior. É mais eficiente do que o Windows repartição de versões anteriores do Windows. Poderá ter de criar os espaços de armazenamento e agrupamentos de armazenamento do Windows por comandos do PowerShell ao utilizar o Windows Server 2012 como sistema operativo. Os comandos do PowerShell podem ser encontrados aqui <https://technet.microsoft.com/library/jj851254.aspx>
> 
> ![Linux][Logo_Linux] Linux
> 
> Apenas MDADM LVM (Gestor de volumes lógicos) são suportados e para criar um RAID de software no Linux. Para obter mais informações, leia os artigos seguintes:
> 
> * [Configurar o RAID de Software no Linux] [ virtual-machines-linux-configure-raid] (para MDADM)
> * [Configurar LVM numa VM do Linux no Azure][virtual-machines-linux-configure-lvm]
> 
> 

- - -
Considerações para tirar partido da VM da série, que são capaz de trabalhar com o armazenamento Premium do Azure, normalmente, são:

* Demandas de latências de e/s que estão próximos dos que geram dispositivos SAN/NAS.
* Pedido para a latência de e/s melhor fatores que podem fornecer armazenamento padrão do Azure.
* IOPS por VM maior do que o que pode ser conseguido com vários discos de armazenamento Standard em relação a um determinado tipo VM.

Uma vez que o armazenamento subjacente do Azure replica cada disco para, pelo menos, três nós de armazenamento, simple RAID 0 repartição pode ser utilizada. Não é necessário para implementar RAID5 ou RAID1.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Armazenamento do Microsoft Azure
Armazenamento do Microsoft Azure armazena a VM base (com o sistema operacional) e discos ou os BLOBs para, pelo menos, três nós de armazenamento separada. Quando criar uma conta de armazenamento ou disco gerido, há uma opção de proteção conforme mostrado aqui:

![Georreplicação ativada para a conta de armazenamento do Azure][dbms-guide-figure-100]

Replicação Azure do armazenamento Local (localmente redundante) fornece níveis de proteção contra perda de dados devido uma falha de infraestrutura que alguns clientes podem estar a implementar. Conforme mostrado acima há quatro opções diferentes com uma quinta a ser uma variação de um dos três primeiro. Análise mais cuidadosa da-los, pode distinguir:

* **Premium armazenamento localmente redundante (LRS)**: armazenamento Premium do Azure fornece suporte de discos de elevado desempenho e de baixa latência para máquinas virtuais que executam cargas de trabalho de e/S intensivas. Existem três réplicas dos dados no mesmo datacenter do Azure de uma região do Azure. As cópias estão em diferentes domínios de atualização e de falha (para conceitos, veja [isso] [ planning-guide-3.2] capítulo o [guia de planejamento][planning-guide]). Em caso de uma réplica dos dados que saem do serviço devido a uma falha de nó de armazenamento ou a falha de disco, é gerada automaticamente uma nova réplica.
* **Armazenamento localmente redundante (LRS)**: neste caso, existem três réplicas dos dados no mesmo datacenter do Azure de uma região do Azure. As cópias estão em diferentes domínios de atualização e de falha (para conceitos, veja [isso] [ planning-guide-3.2] capítulo o [guia de planejamento][planning-guide]). Em caso de uma réplica dos dados que saem do serviço devido a uma falha de nó de armazenamento ou a falha de disco, é gerada automaticamente uma nova réplica. 
* **Armazenamento Georredundante (GRS)**: neste caso, há uma replicação assíncrona que alimentam um adicionais três réplicas dos dados noutra região do Azure, que se encontra na maioria dos casos na mesma região geográfica (como Europa do Norte e Europa Ocidental ). Isso resulta em três réplicas adicionais, para que existem seis réplicas na soma. Uma variação disso é uma adição onde os dados na região do Azure replicadas geo podem ser utilizados para fins de leitura (acesso de leitura Georredundante).
* **Zona de armazenamento com redundância (ZRS)**: neste caso, as três réplicas dos dados permanecem na mesma região do Azure. Conforme explicado [isso] [ planning-guide-3.1] capítulo o [guia de planejamento] [ planning-guide] uma região do Azure pode ser um número de datacenters próximos. No caso de LRS as réplicas teriam de ser distribuídas por diferentes centros de dados que o tornam uma região do Azure.

Podem encontrar mais informações [aqui][storage-redundancy].

> [!NOTE]
> Para implementações do DBMS, não é recomendada a utilização de armazenamento Georredundante
> 
> Georreplicação-replicação de armazenamento do Azure é assíncrona. Replicação dos discos individuais montado para uma única VM não estão sincronizadas no passo de bloqueio. Por conseguinte, não é adequado replicar arquivos DBMS que são distribuídos em discos diferentes ou implementados em relação a um software de RAID, com base em vários discos. O software do DBMS requer que o armazenamento de disco persistente precisamente está sincronizado transversalmente LUNs diferentes e eixos/discos subjacentes. Software do DBMS usa vários mecanismos para atividades de sequência de escrita de e/s e um DBMS relatórios que o armazenamento de disco visado pela replicação está danificado se estes variam de até mesmo por alguns milissegundos. Por conseguinte, se quer uma configuração de base de dados com uma base de dados transferido entre vários discos georreplicado, tal uma replicação tem de ser efetuada com meios de base de dados e a funcionalidade. Um não deve confiar no Azure armazenamento de Georreplicação para realizar esta tarefa. 
> 
> O problema é mais simples explicar com um sistema de exemplo. Vamos supor que tem um sistema SAP carregou para o Azure, o que tem oito discos que contém ficheiros de dados do DBMS e ainda um disco que contém o arquivo de log de transação. Cada um destes discos nove ter os dados gravados num método consistente, de acordo com o DBMS, se os dados que está a ser escritos para os ficheiros de registo de dados ou a transação.
> 
> Na ordem de corretamente georreplicá-os dados e manter uma imagem de base de dados consistente, o conteúdo de todos os discos de nove tinha que ser georreplicado na ordem exata as operações de e/s foram executadas contra os nove discos diferentes. No entanto, georreplicação de armazenamento do Azure não permite declarar as dependências entre discos. Isso significa a georreplicação do armazenamento do Microsoft Azure não conhece o fato de que o conteúdo desses nove diferentes discos está relacionado entre si e que as alterações de dados são consistentes apenas quando a replicar as operações de e/s na ordem ocorreu em todos os os discos de nove.
> 
> Além de possibilidades que está a ser elevada de que as imagens georreplicado no cenário não fornecem uma imagem de base de dados consistente, também há uma penalidade de desempenho que aparece com o armazenamento georredundante com que possam afetar gravemente o desempenho. Em resumo, não utilize este tipo de redundância de armazenamento para cargas de trabalho do DBMS tipo.
> 
> 

#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Mapeamento de VHDs em contas de armazenamento do serviço de Máquina Virtual do Azure
Este capítulo só se aplica às contas de armazenamento do Azure. Se planeia utilizar Managed Disks, as limitações mencionadas neste capítulo não se aplicam. Para obter mais informações sobre os Managed Disks, leia o capítulo [Managed Disks] [ dbms-guide-managed-disks] deste guia.

Uma conta de armazenamento do Azure é não apenas uma construção administrativa, mas também um sujeito de limitações. Ao passo que as limitações variar de acordo se podemos falar sobre uma conta de armazenamento Standard do Azure ou uma conta de armazenamento Premium do Azure. As capacidades exatas e limitações são apresentadas [aqui][storage-scalability-targets]

Portanto, para armazenamento padrão do Azure é importante observar que existe um limite sobre o IOPS por conta de armazenamento (linha que contém **taxa Total de pedidos** na [o artigo][storage-scalability-targets]). Além disso, existe um limite inicial de 100 contas de armazenamento por subscrição do Azure (a partir de Julho de 2015). Por conseguinte, é recomendado para equilibrar o IOPS de VMs entre várias contas de armazenamento ao utilizar o armazenamento padrão do Azure. Enquanto que uma única VM utiliza o ideal é que uma conta de armazenamento se possível. Então, se podemos falar sobre implementações do DBMS em que cada VHD que está alojado no armazenamento padrão do Azure foi possível contactar o respetivo limite de quota, deve implementar apenas VHDs de 30 e 40 por conta de armazenamento do Azure que utiliza o armazenamento padrão do Azure. Por outro lado, se tirar partido do armazenamento Premium do Azure e pretende armazenar os volumes de banco de dados grande, pode ser bom em termos de IOPS. Mas, uma conta de armazenamento Premium do Azure é muito mais restritiva no volume de dados do que uma conta de armazenamento Standard do Azure. Como resultado, só pode implementar um número limitado de VHDs dentro de uma conta de armazenamento Premium do Azure antes de atingir o limite de volume de dados. No final, considere uma conta de armazenamento do Azure como uma "SAN Virtual" que tem capacidades em IOPS e/ou capacidade limitadas. Como resultado, a tarefa permanece, como em implementações no local, para definir o layout dos VHDs de diferentes sistemas SAP sobre os diferentes 'imaginário SAN dispositivos' ou contas de armazenamento do Azure.

Armazenamento do Azure Standard, não é recomendado para apresentar o armazenamento de contas de armazenamento diferentes para uma única VM se possível.

Quando utilizar o DS ou série GS das VMs do Azure, é possível montar VHDs fora das contas de armazenamento Standard do Azure e contas de armazenamento Premium. Casos de uso, como escrever cópias de segurança para o armazenamento padrão de segurança VHDs e ter dados DBMS e ficheiros de registo no armazenamento Premium vêm à mente, onde pode ser aproveitado tal armazenamento heterogéneo. 

Com base em implementações dos clientes e teste de cerca de 30 a 40 VHDs que contêm arquivos de dados do banco de dados e arquivos de log podem ser provisionados num único do Azure conta de armazenamento Standard com um desempenho aceitável. Como mencionado anteriormente, a limitação de uma conta de armazenamento Premium do Azure é provável que seja a capacidade de dados que pode conter e não de IOPS.

Como com SAN dispositivos no local, a partilha requer alguma monitorização para, eventualmente, detectar afunilamentos numa conta de armazenamento do Azure. A extensão de monitorização do Azure para SAP e o portal do Azure são ferramentas que podem ser utilizadas para detetar contas de armazenamento de Azure ocupado, que pode ser fornecer desempenho de e/s inferior ao ideal.  Se esta situação é detectada, recomenda-se para mover VMs ocupadas para outra conta de armazenamento do Azure. Consulte a [guia de implementação] [ deployment-guide] para obter detalhes sobre como ativar o SAP alojam capacidades de monitorização.

Outro artigo resumir as práticas recomendadas sobre o armazenamento padrão do Azure e as contas de armazenamento Standard do Azure pode ser encontrado aqui <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>

#### <a name="f42c6cb5-d563-484d-9667-b07ae51bce29"></a>Discos geridos
Discos geridos são um novo tipo de recurso no Azure Resource Manager que pode ser utilizado em vez de VHDs que são armazenadas em contas de armazenamento do Azure. Discos geridos automaticamente se alinham com o conjunto de disponibilidade da máquina virtual que estão ligados a e, portanto, aumentam a disponibilidade da sua máquina virtual e os serviços que estão em execução na máquina virtual. Para saber mais, leia os [artigo de descrição geral](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

Atualmente o SAP suporta apenas os Managed Disks Premium. Leia a nota SAP [1928533] para obter mais detalhes.

#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>Mover implementado DBMS VMs de armazenamento padrão do Azure para o armazenamento Premium do Azure
Encontramos há algum cenários em que enquanto cliente pretende mover uma VM implementada do armazenamento padrão do Azure para armazenamento Premium do Azure. Se os discos são armazenados nas contas de armazenamento do Azure, isto não é possível sem mover fisicamente os dados. Existem várias formas para alcançar o objetivo:

* Pode copiar todos os VHDs, base VHD, bem como VHDs de dados numa nova conta de armazenamento do Azure Premium. Muitas vezes, escolheu o número de VHDs no armazenamento padrão do Azure não pelo fato de que precisava de volume de dados. Mas precisava de VHDs que muitos devido a IOPS. Agora que mover, para o armazenamento Premium do Azure, que poderia ir com a forma menos VHDs para alcançar o mesmo débito IOPS. Tendo em conta o fato de que no armazenamento Standard do Azure paga os dados utilizados e não o tamanho do disco nominal, o número de VHDs não relevante em termos de custos. No entanto, com o armazenamento do Azure Premium, pagaria para o tamanho do disco nominal. Por conseguinte, a maioria dos clientes tenta manter o número de VHDs do Azure no armazenamento Premium o número necessário para alcançar o débito IOPS necessário. Por isso, a maioria dos clientes decida em relação a forma de um simples 1:1 cópia.
* Se ainda não foi montado, montar um único VHD, que pode conter uma cópia de segurança da base de dados da base de dados SAP. Depois da cópia de segurança, desmontar todos os VHDs, incluindo o VHD que contém a cópia de segurança e copie o VHD base e o VHD com a cópia de segurança para uma conta de armazenamento Premium do Azure. Em seguida, seria implementar a VM com base no base VHD e montar o VHD com a cópia de segurança. Agora, criar discos de armazenamento Premium vazio adicionais para a VM que são utilizados para restaurar a base de dados em. Isso assume que o DBMS permite que altere os caminhos para os ficheiros de dados e de registo como parte do processo de restauro.
* Outra possibilidade é uma variação do processo anterior, onde pode copiar a cópia de segurança VHD para o armazenamento Premium do Azure e ligá-los contra uma VM que recentemente implementado e instalado.
* A possibilidade de quarta que deve escolher quando estiver necessários alterar o número de ficheiros de dados da base de dados. Nesse caso, poderia executar uma cópia de homogéneo sistema SAP usando exportação/importação. PUT aqueles exportar ficheiros num VHD que é copiado para uma conta de armazenamento Premium do Azure e ligá-los a uma VM que utiliza para executar os processos da importação. Os clientes utilizam essa possibilidade, principalmente quando pretende reduzir o número de ficheiros de dados.

Se utilizar Managed Disks, pode migrar para o armazenamento Premium por:

1. Desalocar a máquina virtual
1. Se necessário, redimensionar a máquina virtual para um tamanho que suporte o armazenamento Premium (por exemplo, DS ou GS)
1. Alterar o tipo de conta de disco gerido Premium (SSD)
1. Alterar a colocação em cache dos discos de dados conforme recomendado nas capítulo [colocação em cache para discos de dados e VMs][dbms-guide-2.1]
1. Inicie sua máquina virtual

### <a name="deployment-of-vms-for-sap-in-azure"></a>Implementação de VMs para o SAP no Azure
O Microsoft Azure oferece várias formas de implementar VMs e os discos associados. Deste modo, é importante compreender as diferenças, uma vez que as preparações das VMs poderão diferir depende da forma de implementação. Em geral, vamos examinar os cenários descritos nos capítulos a seguir.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Implementar uma VM no Azure Marketplace
Como fazer um Microsoft ou de terceiros fornecida a imagem do Azure Marketplace para implementar a sua VM. Depois de implementada a VM no Azure, siga as mesmas diretrizes e ferramentas para instalar o software SAP dentro da sua VM, como faria num ambiente no local. Para instalar o software SAP dentro da VM do Azure, o SAP e a Microsoft, recomendamos a carregar e armazenam a mídia de instalação de SAP em discos ou para criar uma VM do Azure a funcionar como um "servidor de ficheiros", que contém todo a necessária SAP mídia de instalação.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Implementar uma VM com uma imagem generalizada específicas do cliente
Devido a requisitos de patch específicas em relação à sua versão de SO ou DBMS, as imagens fornecidas no Azure Marketplace não podem satisfazer suas necessidades. Por conseguinte, poderá ter de criar uma VM com a sua própria imagem de VM de SO/DBMS "privada", que pode ser implementada posteriormente várias vezes. Para preparar essa uma imagem de "privada" para duplicação, o sistema operacional tem de ser generalizado na VM no local. Consulte a [guia de implementação] [ deployment-guide] para obter detalhes sobre como generalizar uma VM.

Se já tiver instalado o conteúdo SAP na sua VM no local (especialmente para sistemas de camada 2), pode adaptar as definições do sistema SAP após a implementação da VM do Azure por meio da instância mudar o nome do procedimento suportado pelo SAP Software aprovisionamento Manager (SAP Tenha em atenção [1619720]). Caso contrário, pode instalar o software SAP mais tarde após a implementação da VM do Azure.

À medida da base de dados conteúdo utilizada pela aplicação SAP, pode optar por gerar o conteúdo recentemente por uma instalação de SAP ou pode importar o conteúdo para o Azure com um VHD com uma cópia de segurança de base de dados do DBMS ou ao tirar partido das capacidades do DBMS diretamente fazer cópias de segurança em  Armazenamento do Microsoft Azure. Neste caso, também preparar o VHD com o DBMS dados e registo de ficheiros no local e, em seguida, importar os como discos para o Azure. Mas a transferência de dados do DBMS, o que é ao carregar a partir do local para o Azure funciona através de discos VHD que precisem de ser preparado no local.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Mover uma VM no local para o Azure com um disco não generalizada
Planeia mover um sistema específico do SAP no local para o Azure (migração lift and shift). Isso pode ser feito através do carregamento de disco, que contém o sistema operacional, os binários do SAP e eventual binários do DBMS e ainda os discos com os arquivos de dados e de registo do DBMS no Azure. Em diferentes dos padrões convencionais cenário #2 acima, mantenha o nome de anfitrião, SAP SID e contas de utilizador SAP na VM do Azure como eles foram configurados no ambiente no local. Por conseguinte, a generalizar a imagem não é necessária. Este caso aplica-se principalmente para cenários em vários locais em que parte do ambiente SAP é executada no local e partes no Azure.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Elevada disponibilidade e recuperação após desastre com VMs do Azure
O Azure oferece funcionalidades a seguir elevada disponibilidade (HA) e a recuperação após desastre (DR), que se aplicam a diferentes componentes que utilizaria para implementações do SAP e DBMS

### <a name="vms-deployed-on-azure-nodes"></a>VMs implementadas em nós do Azure
A plataforma do Azure não oferece funcionalidades como migração em direto para as VMs implementadas. Isso significa que se houver manutenção necessários num cluster de servidor no qual uma VM é implementada, a VM tem de obter parado e reiniciado. Manutenção no Azure é executada usando chamado por isso, os domínios de atualização nos clusters de servidores. Está a ser mantido apenas um domínio de atualização de cada vez. Durante um reinício, houver uma interrupção do serviço enquanto a VM é encerrada, a manutenção é efetuada e a VM reiniciou. No entanto, a maioria dos fornecedores do DBMS fornecem funcionalidade de elevada disponibilidade e recuperação após desastre que rapidamente reinicia os serviços do DBMS no outro nó, se o nó principal não estiver disponível. A plataforma do Azure oferece funcionalidade para distribuir as VMs, armazenamento e outros serviços do Azure entre os domínios de atualização para se certificar de que falhas de infraestrutura ou de manutenção planeadas seriam afetam apenas um pequeno subconjunto das VMs ou serviços.  Com um planejamento cuidadoso, é possível alcançar os níveis de disponibilidade comparáveis para infraestruturas no local.

Conjuntos de disponibilidade do Microsoft Azure são um agrupamento lógico de VMs ou serviços que garante que as VMs e outros serviços são distribuídos para os domínios de atualização e de falha diferentes dentro de um cluster, de modo a que apenas haveria um encerramento de nó em qualquer um ponto no tempo (ler [(Linux)] [ virtual-machines-manage-availability-linux] ou [(Windows)] [ virtual-machines-manage-availability-windows] artigo para obter mais detalhes).

Ele precisa ser configurado por objetivo ao implantar VMs, como visto aqui:

![Definição do conjunto de disponibilidade para configurações de HA do DBMS][dbms-guide-figure-200]

Se quisermos criar configurações de elevada disponibilidade de implementações do DBMS (independentemente da individual HA do DBMS funcionalidade utilizado), as VMs do DBMS seria necessário:

* Adicionar as VMs à mesma rede Virtual do Azure (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* As VMs da configuração HA também devem ser na mesma sub-rede. Resolução de nomes entre as sub-redes diferentes não é possível em implementações apenas na Cloud, só funciona de resolução IP. Utilizar o site a site ou de conectividade do ExpressRoute para implementações em vários locais, uma rede com pelo menos uma sub-rede já está estabelecida. Resolução de nomes é feita de acordo com no local infraestrutura de rede e de políticas do AD. 



#### <a name="ip-addresses"></a>Endereços IP
É altamente recomendado para configurar as VMs para as configurações de HA de forma resiliente. Contando com endereços IP para resolver o partner(s) HA na configuração do HA não é confiável no Azure, a menos que os endereços IP estáticos são utilizados. Existem dois conceitos de "Encerrar" no Azure:

* Encerrado para baixo através do portal do Azure ou do Azure PowerShell cmdlet Stop-AzureRmVM: neste caso, a Máquina Virtual obtém o encerramento e remover a atribuição. Sua conta do Azure já não é cobrada para esta VM, para que os custos únicos que incorrem em são para o armazenamento utilizado. No entanto, se o endereço IP privado da interface de rede não for estático, o endereço IP é libertado e não é garantido que a interface de rede obtém o endereço IP antigo atribuído novamente após um reinício da VM. A desligar a efetuar para baixo através do portal do Azure ou com uma chamada Stop-AzureRmVM automaticamente faz com que a Desalocação. Se não pretender ao desalocar a utilização de máquina Stop-AzureRmVM - StayProvisioned 
* Se encerrar a VM a partir de um nível de sistema operacional, a VM obtém encerrar e não desalocar. No entanto, neste caso, sua conta do Azure é igualmente cobrada a VM, apesar do fato de que ele é encerrado. Nesse caso, a atribuição do endereço IP para uma VM parada permanece intacta. A encerrar a VM a partir de dentro de não forçar automaticamente Desalocação.

Mesmo para cenários em vários locais, por predefinição um encerramento e Desalocação significa eliminação a atribuição dos endereços IP da VM, mesmo se as políticas de locais nas definições de DHCP são diferentes. 

* A exceção é se um atribuir um endereço IP estático a uma interface de rede, como descrita [aqui][virtual-networks-reserved-private-ip].
* Nesse caso o endereço IP permanece fixo enquanto a interface de rede não é eliminada.

> [!IMPORTANT]
> Para manter a implementação de toda, simples e gerenciáveis, desmarque recomenda-se configurar as VMs em parcerias num DBMS HA ou configuração de DR no Azure de forma que não existe uma resolução de nomes está a funcionar entre as VMs diferentes envolvidas.
> 
> 

## <a name="deployment-of-host-monitoring"></a>Implementação do anfitrião de monitorização
Para utilização produtiva de aplicações SAP em máquinas de virtuais do Azure, o SAP requer a capacidade de obter anfitrião de monitorização dos dados dos anfitriões físicos em execução de máquinas virtuais do Azure. Um nível de patch do agente de anfitrião do SAP específico é necessário que permite que esta capacidade em SAPOSCOL e o agente de anfitrião do SAP. O nível de patch exata está documentado na nota SAP [1409604].

Para obter mais detalhes sobre a implementação de componentes que fornecem dados de anfitrião para SAPOSCOL e o agente de anfitrião do SAP e o gerenciamento de ciclo de vida desses componentes, consulte o [guia de implementação][deployment-guide]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Informações específicas para o Microsoft SQL Server
### <a name="sql-server-iaas"></a>SQL Server IaaS
A partir do Microsoft Azure, pode migrar facilmente seus aplicativos existentes do SQL Server criados na plataforma do Windows Server para máquinas de virtuais do Azure. SQL Server numa máquina Virtual permite-lhe reduzir o custo total de propriedade de implantação, gerenciamento e manutenção de aplicativos de amplitude empresariais ao migrar facilmente esses aplicativos para o Microsoft Azure. Com o SQL Server na máquina Virtual do Azure, os administradores e desenvolvedores podem continuar a utilizar o mesmo desenvolvimento e as ferramentas de administração que estão disponíveis no local. 

> [!IMPORTANT]
> Não estamos discutindo base de dados do SQL do Azure de Microsoft, que é uma plataforma como uma oferta de serviço da plataforma Microsoft Azure. A discussão neste artigo é sobre a execução do produto SQL Server que é conhecido por implementações no local nas máquinas virtuais do Azure, tirando partido da infraestrutura como um recurso de serviço do Azure. Capacidades de base de dados e de funcionalidades entre estas duas ofertas são diferentes e não devem misturar entre si. Consulte também: <https://azure.microsoft.com/services/sql-database/>
> 
> 

É recomendado que reveja [isso] [ virtual-machines-sql-server-infrastructure-services] documentação antes de continuar.

Nas secções seguintes partes das partes da documentação, sob a ligação acima são agregados e mencionados. Especificações em torno SAP mencionadas também e alguns conceitos são descritos mais detalhadamente. No entanto, é altamente recomendado para trabalhar com a documentação acima primeiro antes de ler a documentação específica do servidor SQL.

Há alguns do SQL Server em IaaS informações específicas, que deve saber antes de continuar:

* **SLA das máquinas virtuais**: existe um SLA para máquinas virtuais em execução no Azure, que pode ser encontrado aqui: <https://azure.microsoft.com/support/legal/sla/>  
* **Suporte da versão de SQL**: para os clientes SAP, suportamos o SQL Server 2008 R2 e superior na máquina Virtual do Microsoft Azure. Edições anteriores não são suportadas. Rever esta gerais [declaração de suporte](https://support.microsoft.com/kb/956893) para obter mais detalhes. Tenha em atenção de que em geral do SQL Server 2008 também é suportado pela Microsoft. No entanto, devido à funcionalidade significativa para SAP, que foi introduzida com o SQL Server 2008 R2, SQL Server 2008 R2 é a versão mínima para o SAP. Tenha em atenção que o SQL Server 2012 e 2014 foi estendido com uma maior integração com o cenário de IaaS (como criar cópias de segurança diretamente no armazenamento do Azure). Portanto, podemos restringir este documento ao SQL Server 2012 e 2014 com seu nível de patch mais recentes para o Azure.
* **Suporte de funcionalidades do SQL**: mais funcionalidades do SQL Server são suportadas em máquinas virtuais do Microsoft Azure com algumas exceções. **Não é suportada a ativação pós-falha de agrupamento do SQL Server a utilizar discos partilhados**.  Distribuído tecnologias, como o espelhamento de banco de dados, grupos de Disponibilidade AlwaysOn, replicação, envio de registo e Mediador de serviço são suportadas numa única região do Azure. SQL Server AlwaysOn também é suportado entre diferentes regiões do Azure conforme documentado aqui: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Reveja os [declaração de suporte](https://support.microsoft.com/kb/956893) para obter mais detalhes. Mostra um exemplo sobre como implementar uma configuração de AlwaysOn [isso] [ virtual-machines-workload-template-sql-alwayson] artigo. Além disso, verifique as práticas recomendadas documentadas [aqui][virtual-machines-sql-server-infrastructure-services] 
* **Desempenho de SQL**: acreditamos que o Microsoft Azure máquinas virtuais alojadas funcionam muito bem em comparação com outras ofertas de Virtualização de nuvem pública, mas os resultados individuais podem variar. Confira [isso] [ virtual-machines-sql-server-performance-best-practices] artigo.
* **Utilização de imagens do Azure Marketplace**: A forma mais rápida de implementar uma nova VM do Azure da Microsoft é usar uma imagem do Azure Marketplace. Existem imagens no Azure Marketplace, que contêm o SQL Server. As imagens em que o SQL Server já está instalado não podem ser utilizadas imediatamente para aplicações SAP NetWeaver. O motivo é que o agrupamento do SQL Server padrão for instalado dentro dessas imagens e não o agrupamento exigido pela SAP NetWeaver sistemas. Para usar essas imagens, verifique os passos documentados no capítulo [utilizando uma imagem do SQL Server fora do Microsoft Azure Marketplace][dbms-guide-5.6]. 
* Confira [detalhes de preços](https://azure.microsoft.com/pricing/) para obter mais informações. O [guia de licenciamento do SQL Server 2012](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) e [guia de licenciamento do SQL Server 2014](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) também são um recurso importante.

### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>Diretrizes de configuração do SQL Server para instalações de servidor de SQL relacionadas com o SAP em VMs do Azure
#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Recomendações sobre a estrutura VM/VHD para implementações de servidor de SQL relacionadas com o SAP
Em conformidade com a descrição geral, os executáveis do SQL Server devem ser localizado ou instalado numa unidade de sistema de disco do SO da VM (unidade c:\).  Normalmente, a maioria das bases de dados de sistema do SQL Server não é utilizada num alto nível por carga de trabalho do SAP NetWeaver. Por conseguinte, as bases de dados do sistema do SQL Server (mestre, msdb e model) podem permanecer na unidade C:\. Uma exceção pode ser tempdb, que, no caso de todas as cargas de trabalho de BW e alguns SAP ERP, poderão necessitar de mais alto volume de dados ou volume de operações de e/s, o que não é possível se adaptam a VM original. Para esses sistemas, devem ser efetuados os seguintes passos:

* Mova os ficheiros de dados tempdb primário para a mesma unidade lógica que o ficheiro ou ficheiros de dados principal da base de dados SAP.
* Adicione quaisquer ficheiros de dados tempdb adicionais para as unidades lógicas que contém um ficheiro de dados da base de dados de utilizador da SAP.
* Adicione o ficheiro de registo de tempdb para a unidade lógica, que contém o ficheiro de registo do utilizador da base de dados.
* **Exclusivamente para tipos de VM que utilizam o SSD local** no registo de dados tempdb do nó de computação e os ficheiros podem ser colocados na unidade D:\. No entanto, poderá ser recomendado para utilizar vários arquivos de dados tempdb. Tenha em atenção volumes de unidade D:\ são diferentes consoante o tipo VM.

Essas configurações permitem tempdb consumir mais espaço do que a unidade do sistema é capaz de fornecer. Para determinar o tamanho de tempdb adequado, um pode verificar os tamanhos de tempdb em sistemas já existentes, que são executadas no local. Além disso, esta configuração permitiria números IOPS contra tempdb, que não pode ser fornecido com a unidade do sistema. Novamente, os sistemas que estejam a executar no local podem ser utilizados para monitorizar a carga de trabalho de e/s em relação a tempdb para que pode derivar os números IOPS que é esperar em sua tempdb.

Uma configuração de VM, que executa o SQL Server com uma base de dados do SAP e onde os dados de tempdb e o ficheiro de registo de tempdb são colocados na unidade D:\ teria o seguinte aspeto:

![Referência de configuração da VM de IaaS do Azure para SAP][dbms-guide-figure-300]

Lembre-se de que a unidade D:\ tem diferentes tamanhos de dependentes do tipo VM. Depende do requisito de tamanho de tempdb será forçado a par tempdb dados e arquivos de log com os arquivos de dados e de registo da base de dados SAP em casos em que a unidade D:\ é demasiado pequena.

#### <a name="formatting-the-disks"></a>Os discos de formatação
Bloquear o tamanho dos discos que contêm dados do SQL Server para o SQL Server NTFS e ficheiros de registo devem ser 64K. Não é necessário para formatar a unidade D:\. Esta unidade vem previamente formatada.

Para certificar-se de que a criação de bases de dados ou restauro está não a inicializar os ficheiros de dados por zeroing o conteúdo dos ficheiros, uma deve certificar-se de que o contexto de utilizador, que o serviço SQL Server está em execução no tem uma determinada permissão. Normalmente, os utilizadores no grupo de administrador do Windows tem estas permissões. Se o serviço SQL Server for executado no contexto de utilizador do utilizador de administrador não - Windows, tem de atribuir o direito de usuário de que o usuário **executar tarefas de manutenção de volume**.  Consulte os detalhes neste artigo de Conhecimento Microsoft: <https://support.microsoft.com/kb/2574695>

#### <a name="impact-of-database-compression"></a>Impacto da compressão de base de dados
Em configurações em que a largura de banda de e/s pode ser um fator restritivo, a cada medida, que reduz o IOPS poderá ajudar a Stretch Database a carga de trabalho é possível executar um cenário de IaaS, como o Azure. Por conseguinte, se ainda não o fez, aplicar a compactação PAGE do SQL Server é recomendado pela SAP e Microsoft antes de carregar uma base de dados existente do SAP para o Azure.

A recomendação para fazem a compressão de base de dados antes de carregar para o Azure é fornecida sem dois motivos:

* A quantidade de dados a ser carregado é mais baixa.
* A duração da execução de compactação é mais curta, partindo do princípio que é possível usar o hardware mais forte com mais CPUs ou largura de banda de e/s superior ou menos e/s latência no local.
* Tamanhos de base de dados menores podem originar menos os custos de alocação do disco

Compressão de base de dados também funciona num máquinas de virtuais do Azure, como no local. Para obter mais detalhes sobre como compactar uma base de dados existente do SQL Server do SAP, veja aqui: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>

### <a name="sql-server-2014---storing-database-files-directly-on-azure-blob-storage"></a>SQL Server 2014 - armazenamento de ficheiros de base de dados diretamente no armazenamento de Blobs do Azure
SQL Server 2014 abre a possibilidade de armazenar os ficheiros de base de dados diretamente no Store de Blobs do Azure sem o "wrapper" de um VHD em torno delas. Especialmente com a utilização do armazenamento do Azure Standard ou tipos de VM mais pequenos, isto permite cenários em que pode ultrapassar os limites de IOPS que poderia ser impostas por um número limitado de discos que podem ser montados para alguns tipos VM mais pequenos. Isso funciona para bases de dados do utilizador no entanto, nem para bases de dados do sistema do SQL Server. Também funciona para os ficheiros de registo e de dados do SQL Server. Se gostaria de implementar uma base de dados do SQL Server do SAP desta forma, em vez de "encapsulamento" em VHDs, ter o seguinte em mente:

* A conta de armazenamento utilizado necessidades para ser na mesma região do Azure, como aquele que é utilizada para implementar o SQL Server da VM está a executar.
* São aplicáveis considerações listadas anteriormente sobre a distribuição de VHDs através de diferentes contas de armazenamento do Azure para que este método de implementações também. Significa que a contagem de operações de e/s contra os limites da conta de armazenamento do Azure.

[comment]: <> (MSSedusch TODO mas este procedimento irá utilizar largura de banda de rede e não armazenamento largura de banda, não é?)

Detalhes sobre este tipo de implementação estão listados aqui: <https://docs.microsoft.com/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure>

Para armazenar ficheiros de dados do SQL Server diretamente no armazenamento Premium do Azure, tem de ter uma versão de patch de SQL Server 2014 mínimo, o que foi documentada aqui: <https://support.microsoft.com/kb/3063054>. Armazenamento de ficheiros de dados do SQL Server no armazenamento padrão do Azure funciona com a versão de lançamento do SQL Server 2014. No entanto, os patches muito mesmos contenham outra série de correções, o que fazer com que a utilização direta do armazenamento de Blobs do Azure para ficheiros de dados do SQL Server e as cópias de segurança mais confiável. Portanto, recomendamos a utilização esses patches em geral.

### <a name="sql-server-2014-buffer-pool-extension"></a>Extensão do conjunto de memória intermédia do SQL Server 2014
SQL Server 2014 introduziu uma nova funcionalidade, o que é chamada de extensão do conjunto de memória intermédia. Esta funcionalidade estende o pool de buffers do SQL Server, que é mantida na memória com uma segunda cache no nível que é sustentada pelo SSD local de um servidor ou VM. Isto permite manter um conjunto de trabalho maior de dados "na memória'. Aceder ao armazenamento Standard do Azure em comparação com o acesso para a extensão do pool de buffers, o que é armazenado no SSDs locais de uma VM do Azure é muitos fatores mais rapidamente.  Por conseguinte, a tirar partido da unidade D:\ local dos tipos de VM que possuem excelente IOPS e débito pode ser uma forma bastante razoável para reduzir a carga IOPS no armazenamento do Azure e melhorar drasticamente os tempos de resposta das consultas. Isto aplica-se especialmente quando não utilizar o armazenamento Premium. No caso do armazenamento Premium e a utilização da Cache de leitura do Azure Premium no nó de computação, conforme recomendado para ficheiros de dados, não existem diferenças significativas espera. Motivo é que ambos os caches (extensão do conjunto de memória intermédia do SQL Server e Cache de leitura do armazenamento Premium) estão a utilizar os discos locais de nós de computação.
Para obter mais detalhes sobre esta funcionalidade, veja esta documentação: <https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Considerações sobre a cópia de segurança/recuperação para o SQL Server
Quando implementar o SQL Server para o Azure tem de ser revista sua metodologia de cópia de segurança. Mesmo que o sistema não é um sistema de produtividade, a base de dados do SAP hospedado pelo SQL Server deve ser cópias periodicamente. Uma vez que o armazenamento do Azure mantém três imagens, uma cópia de segurança agora é menos importante em relativamente a uma falha de armazenamento de compensação. O motivo de prioridade para a manutenção de um plano de cópia de segurança e recuperação apropriadas, é mais do que compensar erros de lógica/manual, fornecendo o ponto em recursos em tempo de recuperação. Assim, o objetivo é qualquer uma das cópias de segurança de uso para restaurar a base de dados para um determinado ponto no tempo ou para utilizar as cópias de segurança no Azure para efetuar o seeding outro sistema ao copiar a base de dados existente. Por exemplo, poderia transferir de uma configuração de SAP de 2 camadas para uma configuração de sistema de 3 camadas do mesmo sistema restaurando uma cópia de segurança.

Existem três formas diferentes de criar cópias de segurança do SQL Server para o armazenamento do Azure:

1. CU4 do SQL Server 2012 e superior pode nativamente fazer backup dos bancos de dados para um URL. Isso é detalhado no blog [nova funcionalidade no SQL Server 2014 – parte 5 - cópia de segurança/restauro aprimoramentos](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Consulte o capítulo [SQL Server 2012 SP1 CU4 e posterior][dbms-guide-5.5.1].
2. Versões do SQL Server antes do SQL 2012 CU4 podem utilizar uma funcionalidade de redirecionamento para fazer o backup num VHD e avançam, basicamente, o fluxo de escrita para uma localização de armazenamento do Azure que tenha sido configurada. Consulte o capítulo [SQL Server 2012 SP1 CU3 e versões anteriores][dbms-guide-5.5.2].
3. O método final é efetuar uma cópia de segurança do SQL Server convencional ao comando de disco para um dispositivo de disco. Isso é idêntico ao padrão de implementação no local e não é abordado em detalhe neste documento.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 e posterior
Esta funcionalidade permite-lhe diretamente cópia de segurança para o armazenamento de BLOBS do Azure. Sem esse método, deve criar cópias de segurança para outros discos, o que seriam consumir disco e a capacidade IOPS. A idéia é, basicamente, o seguinte:

 ![Utilizar a cópia de segurança do SQL Server 2012 para BLOBS de armazenamento do Microsoft Azure][dbms-guide-figure-400]

A vantagem neste caso é que um não precisa gastar discos para armazenar cópias de segurança do SQL Server no. Por isso, tem menos discos alocados e largura de banda total do disco que IOPS podem ser utilizados para ficheiros de registo e dados. Tenha em atenção que o tamanho máximo de uma cópia de segurança está limitado a um máximo de 1 TB, conforme indicado na secção **limitações** neste artigo: <https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url#limitations>. Se o tamanho da cópia de segurança, apesar de usar a compactação de backup do SQL Server iria exceder a 1 TB de tamanho, a funcionalidade descrito no capítulo [SQL Server 2012 SP1 CU3 e versões anteriores] [ dbms-guide-5.5.2] neste documento tem de ser utilizado.

[Documentação relacionada](https://docs.microsoft.com/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure) descrevendo o restauro de bases de dados de cópias de segurança em relação a Store de Blobs do Azure Recomendamos não para restaurar diretamente a partir do armazenamento de BLOBS do Azure, se a cópia de segurança > 25 GB. A recomendação neste artigo baseia-se em considerações de desempenho e não a devido a restrições de funcionais. Por conseguinte, as condições diferentes podem ser aplicadas no caso a caso.

Documentação sobre como este tipo de cópia de segurança está a configurar e aproveitado pode ser encontrada na [isso](https://docs.microsoft.com/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016) tutorial

Um exemplo de sequência de passos pode ser lido [aqui](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

Automatizar as cópias de segurança, é de importância mais alta para se certificar de que os BLOBs para cada cópia de segurança são nomeados de forma diferente. Caso contrário, estas são substituídas e a cadeia de restauro está quebrada.

Por ordem para não se misturem coisas entre os três tipos diferentes de cópias de segurança, é aconselhável criar diferentes contêineres abaixo a conta de armazenamento utilizada para cópias de segurança. Os contentores podem ser por VM apenas ou por tipo de cópia de segurança e de VM. O esquema pode ter o seguinte aspeto:

 ![Através de diferentes contentores na conta de armazenamento separado de cópia de segurança do SQL Server 2012 para BLOBS de armazenamento do Microsoft Azure-][dbms-guide-figure-500]

No exemplo acima, as cópias de segurança não seriam realizadas para a mesma conta de armazenamento onde as VMs são implementadas. Seria uma nova conta de armazenamento especificamente para as cópias de segurança. Dentro as contas de armazenamento, haveria diferentes contentores criados com uma matriz do tipo de cópia de segurança e o nome da VM. Tal segmentação torna mais fácil de administrar as cópias de segurança das VMs diferentes.

Os BLOBs um escreve diretamente as cópias de segurança, não a adicionar à contagem dos dados discos de uma VM. Por conseguinte, um pode maximizar o número máximo de discos de dados montada do SKU de VM específicos para os dados e ficheiro de registo de transação e ainda executar uma cópia de segurança em relação a um contentor de armazenamento. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 e versões anteriores
O primeiro passo é necessário executar para obter uma cópia de segurança diretamente no armazenamento do Azure seria transferir o msi, que é vinculado [isso](https://www.microsoft.com/download/details.aspx?id=40740) artigo KBA.

Ficheiro de instalação de download x64 e a documentação. O ficheiro instala um programa chamado: **cópia de segurança do Microsoft SQL Server para a ferramenta Microsoft Azure**. Leia a documentação do produto minuciosamente.  A ferramenta basicamente funciona da seguinte forma:

* Do lado do servidor de SQL, uma localização do disco para a cópia de segurança do SQL Server é definida (não utilizar a unidade D:\ como localização).
* A ferramenta permite-lhe definir regras, que podem ser utilizadas para direcionar a diferentes tipos de cópias de segurança para diferentes contentores de armazenamento do Azure.
* Assim que as regras estão em vigor, a ferramenta redireciona o fluxo de escrita da cópia de segurança para um dos VHDs/discos para a localização de armazenamento do Azure, que foi definida anteriormente.
* A ferramenta deixa um ficheiro pequeno stub de alguns de KB de tamanho do VHD/disco, que foi definida para o SQL Server cópia de segurança. **Este ficheiro deve ser deixado na localização de armazenamento, uma vez que é necessário para restaurar novamente do armazenamento do Azure.**
  * Se perder o ficheiro de stub (por exemplo por meio de perda de mídia de armazenamento que continha o ficheiro de stub) e tiver escolhido a opção de cópia de segurança para uma conta de armazenamento do Microsoft Azure, pode recuperar o ficheiro de stub por meio de armazenamento do Microsoft Azure, transferindo-a do contentor de armazenamento em que foi colocada. Coloque o arquivo de stub para uma pasta no computador local onde a ferramenta está configurada para detetar e carregar para o mesmo contentor com a mesma palavra-passe de encriptação, se foi usada criptografia com a regra original. 

Isso significa que o esquema, conforme descrito acima para versões mais recentes do SQL Server pode ser colocado em prática para também para versões do SQL Server, que não são permitir endereço direto uma localização de armazenamento do Azure.

Este método não deve ser utilizado com versões mais recentes do SQL Server, que suportam a segurança de cópia de segurança nativamente no armazenamento do Azure. As exceções são onde limitações da cópia de segurança nativa para o Azure estão a bloquear execução de cópia de segurança nativa para o Azure.

#### <a name="other-possibilities-to-back-up-sql-server-databases"></a>Outras possibilidades para fazer cópias de segurança de bases de dados do SQL Server
Outras possibilidades para fazer cópias de segurança de bases de dados é anexar discos de dados adicionais a uma VM que utiliza para armazenar cópias de segurança no. Nesse caso, terá de certificar-se de que os discos não estão em execução completos. Se for esse o caso, seria necessário desmontar os discos e por assim dizer "Arquivar'-la e substituí-lo com um disco novo vazio. Se descer nesse caminho, que pretende manter estas VHDs em contas de armazenamento do Azure separadas das que os VHDs com os ficheiros de base de dados.

Uma segunda possibilidade é usar uma VM grande que pode ter muitos discos ligados, por exemplo, um D14 com 32VHDs. Utilize espaços de armazenamento para criar um ambiente flexível onde pode criar partilhas que são utilizadas, em seguida, como destinos de backup para os diferentes servidores do DBMS.

Algumas práticas recomendadas de obteve documentadas [aqui](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) também. 

#### <a name="performance-considerations-for-backupsrestores"></a>Considerações de desempenho para backups/restaurações
Como nas implementações bare-metal, cópia de segurança/restaurar desempenho é dependente de quantos volumes podem ser lidos em paralelo e o que pode ser o débito desses volumes. Além disso, o consumo de CPU utilizado pela compressão de cópias de segurança pode desempenham um papel significativo em VMs com até oito threads de CPU. Portanto, pode assumir:

* O menos o número de discos utilizados para armazenar os dados ficheiros, menor o débito global em ler.
* Menor será que o número de CPU threads na VM, o mais grave o impacto da compactação de backup.
* Os destinos menos (BLOBs, VHDs ou discos) para gravar a cópia de segurança, o menor o débito.
* Menor o tamanho da VM, menor será a taxa de transferência quota de armazenamento de gravação e leitura do armazenamento do Azure. Independentemente de se as cópias de segurança diretamente são armazenadas em BLOBs do Azure ou se eles são armazenados em VHDs novamente armazenados nos Blobs do Azure.

Quando utiliza um BLOB de armazenamento do Microsoft Azure como o destino de cópia de segurança em versões mais recentes, são restritos para designar a apenas um URL de destino para cada cópia de segurança específica.

Mas, ao utilizar a "Microsoft SQL Server cópia de segurança para o Microsoft Azure ferramenta" versões mais antigas, pode definir mais do que um destino de ficheiro. Com mais do que um destino, pode dimensionar a cópia de segurança e a taxa de transferência da cópia de segurança é mais alta. Isso poderia resultar em seguida, em múltiplos ficheiros também na conta de armazenamento do Azure. No teste, com vários destinos de ficheiro pode definitivamente alcançar o débito, o que é possível atingir com as extensões de cópia de segurança implementadas do SQL Server 2012 SP1 CU4 em. Também não está bloqueadas pelo limite de 1TB, tal como a cópia de segurança nativa no Azure.

No entanto, tenha em mente, o débito também é dependente da localização da conta do Storage do Azure que utiliza para a cópia de segurança. Pode ser uma idéia de localizar a conta de armazenamento numa região diferente do que as VMs estão a executar. Por exemplo seria executar a configuração da VM na Europa Ocidental, mas colocar a conta de armazenamento que utilizar para cópia de segurança contra na Europa do Norte. Isso certamente tem um impacto sobre o débito de cópia de segurança e, não provavelmente gerar um débito de 150MB/s quanto parece ser possível em casos em que o armazenamento de destino e as VMs estão em execução no mesmo datacenter regional.

#### <a name="managing-backup-blobs"></a>Gerir BLOBs de cópia de segurança
Existe um requisito para gerir as cópias de segurança por conta própria. Uma vez que a expectativa é que os blobs são criados pela execução de backups de log de transação frequentes, administração desses blobs facilmente pode sobrecarregar o portal do Azure. Portanto, é recommendable para tirar partido de um Explorador de armazenamento do Azure. Existem vários bons perfis disponíveis, que podem ajudar a gerir uma conta de armazenamento do Azure

* Microsoft Visual Studio com o Azure SDK instalado (<https://azure.microsoft.com/downloads/>)
* Explorador de armazenamento do Microsoft Azure (<https://azure.microsoft.com/downloads/>)
* Ferramentas de terceiros

Para obter uma discussão mais completa da cópia de segurança e a SAP no Azure, consulte [guia de cópia de segurança do SAP](sap-hana-backup-guide.md) para obter mais informações.

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Utilizar uma imagem do SQL Server fora do Microsoft Azure Marketplace
A Microsoft oferece as VMs no Azure Marketplace, que já contêm as versões do SQL Server. Para clientes SAP que necessitam de licenças para SQL Server e Windows, pode ser uma oportunidade para cobrir basicamente a necessidade de licenças ao configurar as VMs com o SQL Server já instalado. Para usar essas imagens para SAP, as seguintes considerações precisam ser feitas:

* Versões de não avaliação do SQL Server adquirir custos superiores que uma VM de 'Windows-only' implementadas a partir do Azure Marketplace. Veja estes artigos para comparar preços: <https://azure.microsoft.com/pricing/details/virtual-machines/windows/> e <https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/>. 
* Apenas pode utilizar versões do SQL Server, que são suportadas pelo SAP, como o SQL Server 2012.
* O agrupamento de instância do SQL Server, o que é instalado nas VMs oferecidas no mercado do Azure não é o agrupamento que SAP NetWeaver requer a instância do SQL Server para ser executado. Pode alterar o agrupamento embora com as instruções na secção seguinte.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Alterar o agrupamento do SQL Server de uma VM do servidor do Microsoft Windows/SQL
Uma vez que as imagens do SQL Server no Azure Marketplace não são configuradas para utilizar o agrupamento, o que é exigido pela aplicações SAP NetWeaver, ele precisa ser alterada imediatamente após a implementação. Para o SQL Server 2012, isso pode ser feito com os passos seguintes assim que a VM tiver sido implementada e um administrador pode iniciar sessão na VM implementada:

* Abra uma janela de comando do Windows, como administrador.
* Altere o diretório para C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Execute o comando: /ACTION /QUIET de Setup.exe = REBUILDDATABASE /InstanceName. = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
  * `<local_admin_account_name`> é a conta, que foi definida como a conta de administrador ao implementar a VM pela primeira vez a partir da galeria.

O processo deverá demorar apenas alguns minutos. Para verificar se se o passo ficou com o resultado correto, execute os seguintes passos:

* Abra o SQL Server Management Studio.
* Abra uma janela de consulta.
* Execute o comando sp_helpsort na base de dados mestra do SQL Server.

O resultado desejado deve ser semelhante:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Se não for este o resultado, pare a implementação de SAP e investigar por que o comando de configuração não funcionar conforme esperado. Implementação de aplicações SAP NetWeaver em instância do SQL Server com diferentes páginas de código do SQL Server que mencionado acima é **não** suportado.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server elevada disponibilidade para SAP no Azure
Conforme mencionado anteriormente neste artigo, não existe nenhuma possibilidade de criar o armazenamento partilhado, o que é necessário para a utilização da funcionalidade de elevada disponibilidade mais antiga do SQL Server. Esta funcionalidade instalaria duas ou mais instâncias do SQL Server num servidor de ativação pós-falha Cluster Windows (WSFC) utilizando um disco partilhado para as bases de dados do utilizador (e, eventualmente, tempdb). Este é o método de padrão de elevada disponibilidade muito tempo, que também é suportado pelo SAP. Uma vez que o Azure não suporta o armazenamento partilhado, não podem ser percebidas configurações de elevada disponibilidade do SQL Server com uma configuração de cluster de disco partilhado. No entanto, muitos outros métodos de elevada disponibilidade ainda são possíveis e são descritos nas secções seguintes.

#### <a name="sql-server-log-shipping"></a>Envio de Log do SQL Server
Um dos métodos de elevada disponibilidade (HA) é o envio de registo do SQL Server. Se as VMs que participem na configuração de HA tem que trabalhar com resolução de nomes, não há problema e a configuração no Azure não diferem dos qualquer configuração que é feita no local. Não é recomendado dependem apenas a resolução IP. Com respeito a configuração de envio de registo e os princípios em torno de envio de registo, veja esta documentação:

<https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server>

Para obter qualquer elevada disponibilidade, é necessário implementar as VMs que estão dentro desse tipo uma configuração envio de registo entre o mesmo conjunto de disponibilidade de Azure.

#### <a name="database-mirroring"></a>Espelhamento de banco de dados
Espelhamento de banco de dados, como suportadas pelo SAP (consulte a nota SAP [965908]) baseia-se sobre como definir um parceiro de ativação pós-falha na cadeia de ligação de SAP. Nos casos em vários locais, partimos do princípio de que as duas VMs estão no mesmo domínio e que as instâncias de SQL Server do usuário contexto os dois estão a ser executado também um utilizador de domínio e tem privilégios suficientes em duas instâncias do SQL Server envolvidas. Por conseguinte, a configuração do espelhamento de banco de dados no Azure não diferem entre uma configuração típica no local.

A partir de implementações apenas na Cloud, o método mais fácil é ter outra configuração do domínio no Azure para que essas DBMS VMs (e VMs de SAP Idealmente dedicadas) dentro de um domínio.

Se um domínio não for possível, é possível usar certificados para a base de dados espelhamento pontos de extremidade, conforme descrito aqui: <https://docs.microsoft.com/sql/database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql>

Um tutorial para configurar o espelhamento de banco de dados no Azure pode ser encontrado aqui: <https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server> 

#### <a name="sql-server-always-on"></a>SQL Server AlwaysOn
Como Always On é suportada para o SAP no local (consulte a nota SAP [1772688]), é suportada a ser utilizado em combinação com o SAP no Azure. O fato de que não é possível criar discos partilhados no Azure não significa que um não é possível criar uma configuração de sempre no Windows Server Failover Cluster (WSFC) entre VMs diferentes. Isso apenas significa que não tem a possibilidade de utilizar um disco partilhado como um quórum na configuração do cluster. Por conseguinte, pode criar uma configuração de sempre WSFC no Azure e não selecionar o tipo de quórum que utiliza o disco partilhado. O ambiente do Azure essas VMs são implementadas nos deve resolver as VMs por nome e as VMs devem estar no mesmo domínio. Isso é verdadeiro para o Azure apenas e de Implantações em vários locais. Existem algumas considerações especiais sobre a implantação de escuta do grupo disponibilidade do SQL Server (não deve ser confundido com o conjunto de disponibilidade do Azure), uma vez que o Azure no momento não permite a para criar um objeto do AD/DNS, como é possível no local. Portanto, alguns passos de instalação diferentes são necessários para superar o comportamento específico do Azure.

Algumas considerações usando um serviço de escuta do grupo de disponibilidade são:

* Utilizar um serviço de escuta do grupo de disponibilidade só é possível com o Windows Server 2012 ou superior como sistema operacional convidado da VM. Para o Windows Server 2012 terá de certificar-se de que esta atualização é aplicada: <https://support.microsoft.com/kb/2854082> 
* Para o Windows Server 2008 R2, esta atualização não existe e Always On, seria necessário ser utilizado da mesma forma como espelhamento de banco de dados, especificando um parceiro de ativação pós-falha na cadeia de ligações (feito por meio do SAP default.pfl parâmetro bds/mss/server - consulte a nota SAP [965908]).
* Quando utilizar um serviço de escuta do grupo de disponibilidade, as VMs de base de dados tem de ser ligada a um balanceador de carga dedicado. A resolução de nomes em implementações apenas na Cloud ou requer que todas as VMs de um sistema SAP (servidores de aplicativos, servidor DBMS e servidor (A) SCS) estão na mesma rede virtual ou exigiria que a manutenção do arquivo etc\host na ordem de uma camada de aplicação SAP Para obter os nomes das VMS das VMs do SQL Server resolvido. Para evitar que o Azure é atribuir novos endereços IP nos casos em que ambas as VMs são encerramento por acaso, um deve atribuir endereços IP estáticos para as interfaces de rede essas VMs na configuração Always On (definir um endereço IP estático é descrito em [isso] [ virtual-networks-reserved-private-ip] artigo)

[comment]: <> (Blogs do antigo)
[comment]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx>, <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Existem passos especiais é necessários quando criar a configuração de cluster WSFC onde o cluster precisa de um endereço IP especial atribuído, porque o Azure com sua funcionalidade atual atribuiria o nome do cluster o mesmo endereço IP à medida que o nó do cluster é criado em. Isso significa que um passo manual deve ser executado para atribuir um endereço IP diferente para o cluster.
* O serviço de escuta do grupo de disponibilidade vai ser criada no Azure com pontos finais de TCP/IP, que estão atribuídos às VMs em execução as réplicas primárias e secundárias do grupo de disponibilidade.
* Pode haver uma necessidade de proteger estes pontos finais com ACLs.

[comment]: <> (Blog de antigo de tarefas)
[comment]: <> (Os passos detalhados e necessities de instalação de uma configuração de AlwaysOn no Azure são melhor teve ao comentar o tutorial disponível [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[comment]: <> (Pré-configurada de configuração de AlwaysOn através da galeria do Azure <https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[comment]: <> (A criação de um serviço de escuta do grupo de disponibilidade é descrito melhor em tutorial [this][virtual-machines-windows-classic-ps-sql-int-listener])
[comment]: <> (Proteger pontos finais de rede com ACLs são explicados melhor aqui:)
[comment]: <> (* <https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx> )
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

É possível implementar um servidor no grupo de disponibilidade Always em diferentes regiões do Azure também. Esta funcionalidade tira partido da conectividade do Azure VNet-para-Vnet ([mais detalhes][virtual-networks-configure-vnet-to-vnet-connection]).

[comment]: <> (Blog de antigo de tarefas)
[comment]: <> (A configuração de grupos de Disponibilidade AlwaysOn do SQL Server neste cenário é descrita aqui: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Resumo de alta disponibilidade do SQL Server no Azure
Tendo em conta o fato de que o armazenamento do Azure está a proteger o conteúdo, há um motivo menos insistir numa imagem de acesso frequente-em espera. Isso significa que o seu cenário de elevada disponibilidade precisa apenas proteger contra seguintes casos:

* Indisponibilidade da VM como um todo devido a manutenção no servidor de cluster no Azure ou outros motivos
* Problemas de software na instância do SQL Server
* Proteger contra manual erro em que dados são eliminados e é necessária uma recuperação ponto anterior no tempo

Procurando alguém pode argumentar que os dois primeiros casos podem ser abrangidos pelo espelhamento do banco de dados ou Always On, ao passo que o terceiro caso só pode ser abrangido pelo envio de Log de tecnologias de correspondentes.

Tem de equilibrar a configuração mais complexa de Always On, em comparação com o espelhamento de banco de dados, com as vantagens do Always On. Essas vantagens podem ser apresentadas como:

* Réplicas secundárias legíveis.
* Cópias de segurança de réplicas secundárias.
* Uma melhor escalabilidade.
* Mais do que um réplicas secundárias.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Geral SQL Server para o SAP no resumo do Azure
Existem muitas recomendações neste guia e recomendamos que lê-lo mais de uma vez antes de planear a implementação do Azure. Em geral, porém, não se esqueça de seguir o DBMS geral dez principal em pontos específicos do Azure:

[comment]: <> (2.3 um débito mais elevado do que? De um VHD?)
1. Utilize a versão mais recente do DBMS, como o SQL Server 2014, que tem as maioria das vantagens no Azure. Para o SQL Server, este é o SQL Server 2012 SP1 CU4, que inclui a funcionalidade de fazer cópias de segurança ao armazenamento do Azure. No entanto, em conjunto com o SAP é recomendado utilizar, pelo menos, SQL Server 2014 SP1 CU1 ou SQL Server 2012 SP2 e o valor de CU mais recente.
2. Planear cuidadosamente o seu ambiente de sistema SAP no Azure para balancear as restrições do Azure e o esquema do arquivo de dados:
   * Não tem demasiados discos, mas terá uma parte suficiente para garantir que pode entrar seu IOPS necessários.
   * Se não utilizar Managed Disks, lembre-se de que o IOPS também são limitados por conta de armazenamento do Azure e que as contas de armazenamento são limitadas em cada subscrição do Azure ([mais detalhes][azure-subscription-service-limits]). 
   * Apenas faixa entre discos se de que precisa para alcançar um débito mais elevado.
3. Nunca instalar software ou colocar todos os ficheiros que requerem persistência na unidade D:\, à medida que é não permanente e qualquer coisa nesta unidade é perdida numa reinicialização do Windows.
4. Não utilize a colocação em cache do disco Standard do armazenamento do Azure.
5. Não utilize contas de armazenamento georreplicado do Azure.  Utilize localmente redundante para cargas de trabalho do DBMS.
6. Utilize a solução de HA/DR do seu fornecedor DBMS para replicar dados de base de dados.
7. Sempre utilizar a resolução de nomes, não se baseiam em endereços IP.
8. Utilize a compressão de base de dados mais elevada possível. Qual é a compactação page para o SQL Server.
9. Tenha cuidado ao utilizar imagens do SQL Server no Azure Marketplace. Se utilizar um SQL Server, tem de alterar o agrupamento de instância antes de instalar qualquer sistema SAP NetWeaver no mesmo.
10. Instalar e configurar a monitorização de anfitrião de SAP para o Azure, conforme descrito em [guia de implementação][deployment-guide].

## <a name="specifics-to-sap-ase-on-windows"></a>Informações específicas para o SAP ASE no Windows
A partir do Microsoft Azure, pode migrar facilmente as aplicações SAP ASE existentes para máquinas de virtuais do Azure. SAP ASE numa máquina Virtual permite-lhe reduzir o custo total de propriedade de implantação, gerenciamento e manutenção de aplicativos de amplitude empresariais ao migrar facilmente esses aplicativos para o Microsoft Azure. Com o SAP ASE na máquina Virtual do Azure, os administradores e desenvolvedores podem continuar a utilizar as ferramentas de administração que estão disponíveis no local e o desenvolvimento mesmo.

Existe um SLA para máquinas virtuais do Azure, que pode ser encontrada aqui: <https://azure.microsoft.com/support/legal/sla/virtual-machines>

Estamos a certeza de que o Microsoft Azure efetua a máquinas virtuais alojadas bem em comparação com outras ofertas de Virtualização de nuvem pública, mas os resultados individuais podem variar. Certificação de SAP dimensionamento números SAPS do SAP diferente SKUs de VM é fornecido na nota SAP separado [1928533].

Instruções e recomendações em relação a utilização de armazenamento do Azure, a implementação de VMs do SAP ou a monitorização do SAP, aplicam-se às implementações do SAP ASE em conjunto com as aplicações SAP conforme indicado em todo os quatro primeiros capítulos deste documento.

### <a name="sap-ase-version-support"></a>Suporte para a versão de ASE SAP
SAP atualmente suporta SAP ASE versão 16.0 para uso com produtos SAP Business Suite. Todas as atualizações para o servidor SAP ASE ou os controladores ODBC e JDBC para ser utilizado com produtos SAP Business Suite, são fornecidas exclusivamente através do SAP Service Marketplace em: <https://support.sap.com/swdc>.

Como para as instalações no local, não transferir as atualizações para o servidor SAP ASE ou para os controladores ODBC e JDBC diretamente a partir de Web sites do Sybase. Para obter informações detalhadas sobre patches, que são suportadas para utilização com o SAP Business Suite produtos no local e em máquinas de virtuais do Azure consulte as notas de SAP seguintes:

* [1590719]
* [1973241]

Informações gerais sobre como executar o SAP Business Suite no SAP ASE podem ser encontradas no [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Diretrizes de configuração do SAP ASE para SAP relacionados com o SAP ASE instalações em VMs do Azure
#### <a name="structure-of-the-sap-ase-deployment"></a>Estrutura de implementação SAP ASE
Em conformidade com a descrição geral, os executáveis de SAP ASE devem ser localizado ou instalado numa unidade de sistema de disco do SO da VM (unidade c:\). Normalmente, a maioria das bases de dados do sistema e as ferramentas SAP ASE não é utilizada muito por carga de trabalho do SAP NetWeaver. Por conseguinte, as bases de dados e ferramentas de sistema (master, model, saptools, sybmgmtdb, sybsystemdb) podem permanecer na unidade C:\. 

Uma exceção pode ser a base de dados temporário que contém todas as tabelas de trabalho e as tabelas temporárias criadas pelo SAP ASE, que, no caso de alguns SAP ERP e todas as cargas de trabalho BW, poderão necessitar de mais alto volume de dados ou volume de operações de e/s, o que não é possível se encaixa em SO da VM original disco (a unidade c:\).

Dependendo da versão do SAPInst/SWPM utilizado para instalar o sistema, pode conter a base de dados:

* Um único tempdb SAP ASE, o que é criado ao instalar o SAP ASE
* Um tempdb SAP ASE criado ao instalar o SAP ASE e um saptempdb adicional criados pela rotina de instalação SAP
* Um tempdb SAP ASE criado ao instalar o SAP ASE e um tempdb adicionais que tenha sido criado manualmente (por exemplo a seguir a nota SAP [1752266]) para satisfazer os requisitos de tempdb específicos de ERP/BW

Em caso de ERP específico ou todas as cargas de trabalho BW, faz sentido, em relação ao desempenho, para manter os dispositivos de tempdb da tempdb adicionalmente criado (por SWPM ou manualmente) numa unidade que não seja C:\. Não se existir nenhuma tempdb adicionais, é recomendado para criar um (a nota SAP [1752266]).

Para esses sistemas, os seguintes passos devem ser efetuados para o tempdb adicionalmente criado:

* Mover o primeiro dispositivo de tempdb para o primeiro dispositivo de base de dados do SAP
* Adicionar dispositivos de tempdb para cada um dos VHDs que contém um dispositivo da base de dados SAP

Esta configuração permite tempdb ou consumir mais espaço do que a unidade do sistema é capaz de fornecer. Como uma referência um pode verificar os tamanhos de dispositivo de tempdb em sistemas já existentes, que são executadas no local. Ou essa configuração iria permitir que os números IOPS contra tempdb, que não pode ser fornecido com a unidade do sistema. Novamente, sistemas que estejam a executar no local podem ser utilizados para monitorizar a carga de trabalho de e/s em tempdb.

Nunca coloca todos os dispositivos SAP ASE na unidade D:\ da VM. Isso também se aplica a tempdb, mesmo que os objetos mantidos no tempdb são apenas temporários.

#### <a name="impact-of-database-compression"></a>Impacto da compressão de base de dados
Em configurações em que a largura de banda de e/s pode ser um fator restritivo, a cada medida, que reduz o IOPS poderá ajudar a Stretch Database a carga de trabalho é possível executar um cenário de IaaS, como o Azure. Por isso, recomenda-se para se certificar de que é utilizada compressão de SAP ASE antes de carregar uma base de dados existente do SAP para o Azure.

A recomendação para executar compactação antes de carregar para o Azure se não estiver já implementado é fornecida fora de vários motivos:

* A quantidade de dados a ser carregado para o Azure é mais baixa
* Durante a execução de compactação é mais curta, partindo do princípio que é possível usar o hardware mais forte com mais CPUs ou largura de banda de e/s superior ou menos e/s latência no local
* Tamanhos de base de dados menores podem originar menos os custos de alocação do disco

Compressão de dados e de LOB trabalhar numa VM alojada em máquinas de virtuais do Azure, como o faz no local. Para obter mais detalhes sobre como verificar se compressão já se encontra em utilização na base de dados existente do SAP ASE, veja a nota SAP [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Utilizar DBACockpit para monitorizar instâncias de base de dados
Para os sistemas SAP, o que estiver a utilizar o SAP ASE como plataforma de base de dados, o DBACockpit é acessível como janelas do browser incorporado na transação DBACockpit ou como Webdynpro. No entanto todas as funcionalidades de monitorização e administrar a base de dados estão disponível na implementação de Webdynpro de apenas o DBACockpit.

Como com sistemas no local vários passos são necessários para ativar todas as funcionalidades do SAP NetWeaver utilizada pela implementação Webdynpro do DBACockpit. Siga a nota SAP [1245200] para ativar a utilização de webdynpros e gerar aqueles necessários. Ao seguir as instruções nas notas para o acima, também configurar o Gestor de comunicação de Internet (icm), juntamente com as portas a utilizar para ligações http e https. A predefinição para http tem esta aparência:

> icm/server_port_0 = PROT=HTTP,PORT=8000,PROCTIMEOUT=600,TIMEOUT=600
> 
> icm/server_port_1 = PROT=HTTPS,PORT=443$$,PROCTIMEOUT=600,TIMEOUT=600
> 
> 

e os links gerados na transação DBACockpit é semelhante a este:

> https://`<fullyqualifiedhostname`>:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Dependendo se e como a Máquina Virtual do Azure que aloja o sistema SAP estejam ligados através do site a site, múltiplos sites ou ExpressRoute (implementação em vários locais), terá de certificar-se de que esse ICM está a utilizar um nome de anfitrião totalmente qualificado que pode ser resolvido na máquina onde está a tentar abrir o DBACockpit do. Consulte a nota SAP [773830] para compreender como ICM determina o nome de anfitrião totalmente qualificado, consoante os parâmetros de perfil e o parâmetro de conjunto icm/host_name_full explicitamente se necessário.

Se tiver de implementar a VM num cenário de apenas na Cloud sem conectividade em vários locais entre no local e o Azure, terá de definir um endereço IP público e um domainlabel. O formato do nome DNS público da VM tem o seguinte aspeto:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

Podem encontrar mais detalhes relacionados com o nome DNS [aqui][virtual-machines-azurerm-versus-azuresm].

Definir o parâmetro de perfil icm/host_name_full para o SAP para o nome DNS da VM do Azure a ligação poderá ter um aspeto semelhante:

> https://mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Neste caso, precisa certificar-se de que para:

* Adicionar regras de entrada para o grupo de segurança de rede no portal do Azure para as portas TCP/IP utilizada para comunicar com ICM
* Adicionar regras de entrada para a configuração da Firewall do Windows para as portas TCP/IP utilizada para comunicar com o ICM

Para um automatizada importados de todas as correções disponíveis, é recomendado para periodicamente aplicam-se a coleção de correção a nota SAP aplicáveis à sua versão SAP:

* [1558958]
* [1619967]
* [1882376]

Podem encontrar mais informações sobre o DBA Cockpit para SAP ASE nas notas para o SAP seguintes:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Considerações sobre a cópia de segurança/recuperação para o SAP ASE
Ao implementar o SAP ASE para o Azure, tem de ser revista sua metodologia de cópia de segurança. Mesmo que o sistema não é um sistema de produtividade, a base de dados do SAP hospedado pelo SAP ASE deve ser cópias periodicamente. Uma vez que o armazenamento do Azure mantém três imagens, uma cópia de segurança agora é menos importante em relativamente a uma falha de armazenamento de compensação. A principal razão para a manutenção de um plano de cópia de segurança e restauro adequado é mais do que compensar erros de lógica/manual, fornecendo o ponto em recursos em tempo de recuperação. Assim, o objetivo é qualquer uma das cópias de segurança de uso para restaurar a base de dados para um determinado ponto no tempo ou para utilizar as cópias de segurança no Azure para efetuar o seeding outro sistema ao copiar a base de dados existente. Por exemplo, poderia transferir de uma configuração de SAP de 2 camadas para uma configuração de sistema de 3 camadas do mesmo sistema restaurando uma cópia de segurança.

Fazer backup e restaurar uma base de dados do Azure funcionam da mesma forma como no local. Consulte as notas de SAP:

* [1588316]
* [1585981]

Para obter detalhes sobre a criação de despejo de configurações e agendamento de cópias de segurança. Dependendo de suas necessidades pode configurar e a estratégia de base de dados e registo de informações do Estado do disco em um dos discos existentes ou adicionar um disco adicional para a cópia de segurança. Para reduzir o risco de perda de dados em caso de erro, é recomendado para utilizar um disco onde não está localizado nenhum dispositivo de base de dados.

Além de dados e LOB compressão SAP ASE também oferece compactação de backup. Para ocupar menos espaço com informações de estado da base de dados e de registo é recomendado para utilizar a compressão de cópias de segurança. Para obter mais informações, consulte a nota SAP [1588316]. Comprimir a cópia de segurança também é crucial para reduzir a quantidade de dados a serem transferidos, se planear transferir cópias de segurança ou VHDs que contêm informações de cópia de segurança da máquina de Virtual do Azure para o local.

Não utilize a unidade D:\ como destino de despejo de base de dados ou log.

#### <a name="performance-considerations-for-backupsrestores"></a>Considerações de desempenho para Backups/restaurações
Como nas implementações bare-metal, cópia de segurança/restaurar desempenho é dependente de quantos volumes podem ser lidos em paralelo e o que pode ser o débito desses volumes. Além disso, o consumo de CPU utilizado pela compressão de cópias de segurança pode desempenham um papel significativo em VMs com até oito threads de CPU. Por conseguinte, um pode assumir:

* Menos o número de discos utilizados para armazenar os dispositivos de base de dados, menor será a produtividade geral em ler
* Menor será que o número de CPU threads na VM, o mais grave o impacto da compactação de backup
* Os destinos menos (diretórios do Stripe, discos) para gravar a cópia de segurança, o menor a taxa de transferência

Para aumentar o número de destinos para escrever existem duas opções, que podem ser utilizado/combinadas consoante as suas necessidades:

* Repartição de volume de destino de cópia de segurança ao longo de vários discos montados para melhorar o débito IOPS nesse volume repartidos
* Criação de uma configuração de informação no nível de SAP ASE, que utiliza a mais do que um diretório de destino para escrever o despejo para

Repartição de um volume ao longo de vários discos montados tem sido discutido anteriormente neste guia. Para obter mais informações sobre como utilizar vários diretórios na configuração de despejo do SAP ASE, consulte a documentação no sp_config_dump de procedimento armazenado, o que é utilizada para criar a configuração de informação sobre o [centros de informações do Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Recuperação após desastre com VMs do Azure
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replicação de dados com o servidor de replicação do SAP Sybase
Com o servidor de replicação de Sybase de SAP (SRS) SAP ASE fornece uma solução de reserva quente para transferir transações da base de dados para um local distante de forma assíncrona. 

A instalação e a operação de SRS também funciona funcionalmente numa VM alojada nos serviços de Máquina Virtual do Azure, como o faz no local.

SAP ASE HADR não necessita de um balanceador de carga interno do Azure e não tem dependências no nível clustering do SO e funciona no Windows Azure e VMs do Linux. Para obter detalhes sobre o SAP ASE HADR leia os [Guia do usuário do SAP ASE HADR](https://help.sap.com/viewer/efe56ad3cad0467d837c8ff1ac6ba75c/16.0.3.3/en-US/a6645e28bc2b1014b54b8815a64b87ba.html).

## <a name="specifics-to-sap-ase-on-linux"></a>Informações específicas para o SAP ASE no Linux
A partir do Microsoft Azure, pode migrar facilmente as aplicações SAP ASE existentes para máquinas de virtuais do Azure. SAP ASE numa máquina Virtual permite-lhe reduzir o custo total de propriedade de implantação, gerenciamento e manutenção de aplicativos de amplitude empresariais ao migrar facilmente esses aplicativos para o Microsoft Azure. Com o SAP ASE na máquina Virtual do Azure, os administradores e desenvolvedores podem continuar a utilizar as ferramentas de administração que estão disponíveis no local e o desenvolvimento mesmo.

Para implementar VMs do Azure é importante saber os SLAs oficiais, que podem ser encontrados aqui: <https://azure.microsoft.com/support/legal/sla>

Informações de tamanho do SAP e uma lista de SKUs de VM com certificação do SAP é fornecido na nota SAP [1928533]. SAP adicional documentos de dimensionamento de máquinas virtuais do Azure podem ser encontradas aqui <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> e aqui <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Instruções e recomendações em relação a utilização de armazenamento do Azure, a implementação de VMs do SAP ou a monitorização do SAP, aplicam-se às implementações do SAP ASE em conjunto com as aplicações SAP conforme indicado em todo os quatro primeiros capítulos deste documento.

As notas de SAP duas seguintes incluem informações gerais sobre o ASE em Linux e o ASE na Cloud:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>Suporte para a versão de ASE SAP
SAP atualmente suporta SAP ASE versão 16.0 para uso com produtos SAP Business Suite. Todas as atualizações para o servidor SAP ASE ou os controladores ODBC e JDBC para ser utilizado com produtos SAP Business Suite, são fornecidas exclusivamente através do SAP Service Marketplace em: <https://support.sap.com/swdc>.

Como para as instalações no local, não transferir as atualizações para o servidor SAP ASE ou para os controladores ODBC e JDBC diretamente a partir de Web sites do Sybase. Para obter informações detalhadas sobre patches, que são suportadas para utilização com o SAP Business Suite produtos no local e em máquinas de virtuais do Azure consulte as notas de SAP seguintes:

* [1590719]
* [1973241]

Informações gerais sobre como executar o SAP Business Suite no SAP ASE podem ser encontradas no [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Diretrizes de configuração do SAP ASE para SAP relacionados com o SAP ASE instalações em VMs do Azure
#### <a name="structure-of-the-sap-ase-deployment"></a>Estrutura de implementação SAP ASE
Em conformidade com a descrição geral, o SAP ASE executáveis devem ser localizado ou instalado no sistema de arquivos de raiz da VM (/sybase). Normalmente, a maioria das bases de dados do sistema e as ferramentas SAP ASE não é ser aproveitada duro por carga de trabalho do SAP NetWeaver. Por conseguinte, as bases de dados e ferramentas de sistema (master, model, saptools, sybmgmtdb, sybsystemdb) podem permanecer no também o sistema de ficheiros de raiz. 

Uma exceção pode ser a base de dados temporário que contém todas as tabelas de trabalho e as tabelas temporárias criadas pelo SAP ASE, que, no caso de alguns SAP ERP e todas as cargas de trabalho BW, pode exigir maior volume de dados ou operações de e/s, volume que não é possível se encaixa em SO da VM original disco.

Dependendo da versão do SAPInst/SWPM utilizado para instalar o sistema, pode conter a base de dados:

* Um único tempdb SAP ASE, o que é criado ao instalar o SAP ASE
* Um tempdb SAP ASE criado ao instalar o SAP ASE e um saptempdb adicional criados pela rotina de instalação SAP
* Um tempdb SAP ASE criado ao instalar o SAP ASE e um tempdb adicionais que tenha sido criado manualmente (por exemplo a seguir a nota SAP [1752266]) para satisfazer os requisitos de tempdb específicos de ERP/BW

Em caso de ERP específico ou todas as cargas de trabalho BW, faz sentido, em relação ao desempenho, manter os dispositivos de tempdb da tempdb adicionalmente criado (por SWPM ou manualmente) num sistema de arquivo separado, que pode ser representado por um disco de dados do Azure individual ou um spannin RAID de Linux g vários discos de dados do Azure. Não se existir nenhuma tempdb adicionais, é recomendado para criar um (a nota SAP [1752266]).

Para esses sistemas, os seguintes passos devem ser efetuados para o tempdb adicionalmente criado:

* Mova o diretório de tempdb primeiro para o primeiro sistema de ficheiros da base de dados SAP
* Adicionar diretórios de tempdb para cada um dos discos que contém um sistema de ficheiros da base de dados SAP

Esta configuração permite tempdb ou consumir mais espaço do que a unidade do sistema é capaz de fornecer. Como uma referência um pode verificar os tamanhos de diretório de tempdb em sistemas já existentes, que são executadas no local. Ou essa configuração iria permitir que os números IOPS contra tempdb, que não pode ser fornecido com a unidade do sistema. Novamente, sistemas que estejam a executar no local podem ser utilizados para monitorizar a carga de trabalho de e/s em tempdb.

Nunca coloca todos os diretórios SAP ASE no /mnt ou /mnt/resource da VM. Isso também se aplica a tempdb, mesmo que os objetos mantidos no tempdb são apenas temporários porque /mnt ou /mnt/resource é um espaço de temporário da VM do Azure predefinido, que não é persistente. Podem encontrar mais detalhes sobre o espaço temporário da VM do Azure no [neste artigo][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Impacto da compressão de base de dados
Em configurações em que a largura de banda de e/s pode ser um fator restritivo, a cada medida, que reduz o IOPS poderá ajudar a Stretch Database a carga de trabalho é possível executar um cenário de IaaS, como o Azure. Por isso, recomenda-se para se certificar de que é utilizada compressão de SAP ASE antes de carregar uma base de dados existente do SAP para o Azure.

A recomendação para executar compactação antes de carregar para o Azure se não estiver já implementado é fornecida fora de vários motivos:

* A quantidade de dados a ser carregado para o Azure é mais baixa
* Durante a execução de compactação é mais curta, partindo do princípio que é possível usar o hardware mais forte com mais CPUs ou largura de banda de e/s superior ou menos e/s latência no local
* Tamanhos de base de dados menores podem originar menos os custos de alocação do disco

Compressão de dados e de LOB trabalhar numa VM alojada em máquinas de virtuais do Azure, como o faz no local. Para obter mais detalhes sobre como verificar se compressão já se encontra em utilização na base de dados existente do SAP ASE, veja a nota SAP [1750510]. Para obter informações adicionais sobre a compressão de base de dados, consulte a nota SAP [2121797].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Utilizar DBACockpit para monitorizar instâncias de base de dados
Para os sistemas SAP, o que estiver a utilizar o SAP ASE como plataforma de base de dados, o DBACockpit é acessível como janelas do browser incorporado na transação DBACockpit ou como Webdynpro. No entanto todas as funcionalidades de monitorização e administrar a base de dados estão disponível na implementação de Webdynpro de apenas o DBACockpit.

Como com sistemas no local vários passos são necessários para ativar todas as funcionalidades do SAP NetWeaver utilizada pela implementação Webdynpro do DBACockpit. Siga a nota SAP [1245200] para ativar a utilização de webdynpros e gerar aqueles necessários. Ao seguir as instruções nas notas para o acima, também configurar o Gestor de comunicação de Internet (icm), juntamente com as portas a utilizar para ligações http e https. A predefinição para http tem esta aparência:

> icm/server_port_0 = PROT=HTTP,PORT=8000,PROCTIMEOUT=600,TIMEOUT=600
> 
> icm/server_port_1 = PROT=HTTPS,PORT=443$$,PROCTIMEOUT=600,TIMEOUT=600
> 
> 

e os links gerados na transação DBACockpit terá um aspeto semelhante a este:

> https://`<fullyqualifiedhostname`>:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Dependendo se e como a Máquina Virtual do Azure que aloja o sistema SAP estejam ligados através do site a site, múltiplos sites ou ExpressRoute (implementação em vários locais), terá de certificar-se de que esse ICM está a utilizar um nome de anfitrião totalmente qualificado que pode ser resolvido na máquina onde está a tentar abrir o DBACockpit do. Consulte a nota SAP [773830] para compreender como ICM determina o nome de anfitrião totalmente qualificado, consoante os parâmetros de perfil e o parâmetro de conjunto icm/host_name_full explicitamente se necessário.

Se tiver de implementar a VM num cenário de apenas na Cloud sem conectividade em vários locais entre no local e o Azure, terá de definir um endereço IP público e um domainlabel. O formato do nome DNS público da VM tem o seguinte aspeto:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

Podem encontrar mais detalhes relacionados com o nome DNS [aqui][virtual-machines-azurerm-versus-azuresm].

Definir o parâmetro de perfil icm/host_name_full para o SAP para o nome DNS da VM do Azure a ligação poderá ter um aspeto semelhante:

> https://mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Neste caso, precisa certificar-se de que para:

* Adicionar regras de entrada para o grupo de segurança de rede no portal do Azure para as portas TCP/IP utilizada para comunicar com ICM
* Adicionar regras de entrada para a configuração da Firewall do Windows para as portas TCP/IP utilizada para comunicar com o ICM

Para um automatizada importados de todas as correções disponíveis, é recomendado para periodicamente aplicam-se a coleção de correção a nota SAP aplicáveis à sua versão SAP:

* [1558958]
* [1619967]
* [1882376]

Podem encontrar mais informações sobre o DBA Cockpit para SAP ASE nas notas para o SAP seguintes:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Considerações sobre a cópia de segurança/recuperação para o SAP ASE
Ao implementar o SAP ASE para o Azure tem de ser revista sua metodologia de cópia de segurança. Mesmo que o sistema não é um sistema de produtividade, a base de dados do SAP hospedado pelo SAP ASE deve ser cópias periodicamente. Uma vez que o armazenamento do Azure mantém três imagens, uma cópia de segurança agora é menos importante em relativamente a uma falha de armazenamento de compensação. A principal razão para a manutenção de um plano de cópia de segurança e restauro adequado é mais do que compensar erros de lógica/manual, fornecendo o ponto em recursos em tempo de recuperação. Assim, o objetivo é qualquer uma das cópias de segurança de uso para restaurar a base de dados para um determinado ponto no tempo ou para utilizar as cópias de segurança no Azure para efetuar o seeding outro sistema ao copiar a base de dados existente. Por exemplo, poderia transferir de uma configuração de SAP de 2 camadas para uma configuração de sistema de 3 camadas do mesmo sistema restaurando uma cópia de segurança.

Fazer backup e restaurar uma base de dados do Azure funcionam da mesma forma como no local. Consulte as notas de SAP:

* [1588316]
* [1585981]

Para obter detalhes sobre a criação de despejo de configurações e agendamento de cópias de segurança. Dependendo de suas necessidades pode configurar e a estratégia de base de dados e registo de informações do Estado do disco em um dos discos existentes ou adicionar um disco adicional para a cópia de segurança. Para reduzir o risco de perda de dados em caso de erro, é recomendado para utilizar um disco onde não está localizado nenhum diretório/ficheiro de base de dados.

Além de dados e LOB compressão SAP ASE também oferece compactação de backup. Para ocupar menos espaço com informações de estado da base de dados e de registo é recomendado para utilizar a compressão de cópias de segurança. Para obter mais informações, consulte a nota SAP [1588316]. Comprimir a cópia de segurança também é crucial para reduzir a quantidade de dados a serem transferidos, se planear transferir cópias de segurança ou VHDs que contêm informações de cópia de segurança da máquina de Virtual do Azure para o local.

Não utilize o /mnt de espaço temporário da VM do Azure ou /mnt/resource como destino de despejo de base de dados ou log.

#### <a name="performance-considerations-for-backupsrestores"></a>Considerações de desempenho para Backups/restaurações
Como nas implementações bare-metal, cópia de segurança/restaurar desempenho é dependente de quantos volumes podem ser lidos em paralelo e o que pode ser o débito desses volumes. Além disso, o consumo de CPU utilizado pela compressão de cópias de segurança pode desempenham um papel significativo em VMs com até oito threads de CPU. Por conseguinte, um pode assumir:

* Menos o número de discos utilizados para armazenar os dispositivos de base de dados, menor será a produtividade geral em ler
* Menor será que o número de CPU threads na VM, o mais grave o impacto da compactação de backup
* Os destinos menos (Linux RAID de software, discos) para gravar a cópia de segurança, o menor a taxa de transferência

Para aumentar o número de destinos para escrever existem duas opções, que podem ser utilizado/combinadas consoante as suas necessidades:

* Repartição de volume de destino de cópia de segurança ao longo de vários discos montados para melhorar o débito IOPS nesse volume repartidos
* Criação de uma configuração de informação no nível de SAP ASE, que utiliza a mais do que um diretório de destino para escrever o despejo para

Repartição de um volume ao longo de vários discos montados tem sido discutido anteriormente neste guia. Para obter mais informações sobre como utilizar vários diretórios na configuração de despejo do SAP ASE, consulte a documentação no sp_config_dump de procedimento armazenado, o que é utilizada para criar a configuração de informação sobre o [centros de informações do Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Recuperação após desastre com VMs do Azure
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replicação de dados com o servidor de replicação do SAP Sybase
Com o servidor de replicação de Sybase de SAP (SRS) SAP ASE fornece uma solução de reserva quente para transferir transações da base de dados para um local distante de forma assíncrona. 

A instalação e a operação de SRS também funciona funcionalmente numa VM alojada nos serviços de Máquina Virtual do Azure, como o faz no local.

ASE HADR através do servidor de replicação de SAP não é suportado no momento. Pode ser testado com e lançado no futuro para plataformas do Microsoft Azure.

## <a name="specifics-to-oracle-database-on-windows"></a>Informações específicas para a base de dados do Oracle no Windows
Oracle software é suportado pelo Oracle para ser executado no Microsoft Windows Hyper-V e o Azure. 

Após o suporte geral, o cenário específico de aplicações SAP tirar partido das bases de dados Oracle também é suportado. Detalhes são nomeados nesta parte do documento.

### <a name="oracle-version-support"></a>Suporte da versão Oracle
Versões do Oracle e versões de SO correspondentes, que são suportadas para executar o SAP no Oracle em máquinas de virtuais do Azure pode ser encontrado na nota SAP [2039619].

Informações gerais sobre como executar o SAP Business Suite no Oracle podem ser encontradas no 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Diretrizes de configuração do Oracle para instalações de SAP em VMs do Azure
#### <a name="storage-configuration"></a>Configuração do armazenamento
Apenas única instância suportada do Oracle usando discos formatados com NTFS. Todos os ficheiros de base de dados devem ser armazenados no sistema de ficheiros NTFS com base em VHDs ou Managed Disks. Estes discos estão montados à VM do Azure e baseiam-se no armazenamento de BLOBS de página do Azure (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) ou Managed Disks (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Qualquer tipo de unidades de rede ou de partilhas remotas, como os serviços de ficheiros do Azure:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

são **não** suportado para ficheiros de base de dados Oracle!

Utilizar discos com base no armazenamento de BLOBS de página do Azure ou as declarações feitas neste documento capítulo do Managed Disks [colocação em cache para discos de dados e VMs] [ dbms-guide-2.1] e [dearmazenamentodoMicrosoftAzure] [ dbms-guide-2.3] aplicam-se a implementações com a base de dados do Oracle também.

Tal como explicado anteriormente, na parte geral do documento, existem quotas no débito IOPS de discos do Azure. As quotas exatas são dependendo do tipo VM utilizadas. Pode encontrar uma lista dos tipos VM com as respetivas quotas [aqui (Linux)] [ virtual-machines-sizes-linux] e [aqui (Windows)][virtual-machines-sizes-windows].

Para identificar os tipos de VM do Azure suportados, consulte a nota SAP [1928533].

Desde que a quota atual de IOPS por disco cumprir os requisitos, é possível armazenar todos os ficheiros de DB num único disco montado. 

Se mais IOPS forem necessárias, recomenda-se para utilizar a janela de agrupamentos de armazenamento (apenas disponíveis no Windows Server 2012 e superior) ou Windows repartição para Windows 2008 R2 para criar um dispositivo lógico grande ao longo de vários discos montados (Consulte também o capítulo [ RAID de software] [ dbms-guide-2.2] deste documento). Esta abordagem simplifica a sobrecarga de administração para gerir o espaço em disco e evita o esforço necessário para distribuir manualmente os arquivos em vários discos montados.

#### <a name="backup--restore"></a>Cópia de Segurança / Restauro
Para cópia de segurança / restaurar a funcionalidade, o SAP BR * ferramentas para Oracle são suportadas da mesma forma que no padrão de sistemas operacionais Windows Server e Hyper-V. O Gestor de recuperação de Oracle (RMAN) também é suportado para cópias de segurança em disco e restaurar a partir de disco.

#### <a name="high-availability"></a>Elevada Disponibilidade
Oracle Data Guard é suportada para elevada disponibilidade e fins de recuperação após desastre. Detalhes podem ser encontrados na [isso] [ virtual-machines-windows-classic-configure-oracle-data-guard] documentação.

#### <a name="other"></a>Outros
Todas as outras áreas gerais, como conjuntos de disponibilidade do Azure ou SAP monitorização aplicam-se conforme descrito nos primeiros três capítulos deste documento para implementações de VMs com a base de dados do Oracle também.

## <a name="specifics-to-oracle-database-on-oracle-linux"></a>Informações específicas para a base de dados do Oracle no Oracle Linux
Oracle software é suportado pelo Oracle para ser executado no Microsoft Windows Hyper-V e o Azure. 

Após o suporte geral, o cenário específico de aplicações SAP tirar partido das bases de dados Oracle também é suportado. Detalhes são nomeados nesta parte do documento.

### <a name="oracle-version-support"></a>Suporte da versão Oracle
Versões do Oracle e versões de SO correspondentes, que são suportadas para executar o SAP no Oracle em máquinas de virtuais do Azure pode ser encontrado na nota SAP [2039619].

Informações gerais sobre como executar o SAP Business Suite no Oracle podem ser encontradas no 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Diretrizes de configuração do Oracle para instalações de SAP em VMs do Azure
#### <a name="storage-configuration"></a>Configuração do armazenamento
Só é suportada a única instância de Oracle através de ext3, ext4 xfs formatadas e discos. Todos os ficheiros de base de dados devem ser armazenados nestes sistemas de ficheiros com base em VHDs ou Managed Disks. Estes discos estão montados à VM do Azure e baseiam-se no armazenamento de BLOBS de página do Azure (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) ou Managed Disks (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Qualquer tipo de unidades de rede ou de partilhas remotas, como os serviços de ficheiros do Azure:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

são **não** suportado para ficheiros de base de dados Oracle!

Utilizar discos com base no armazenamento de BLOBS de página do Azure ou as declarações feitas neste documento capítulo do Managed Disks [colocação em cache para discos de dados e VMs] [ dbms-guide-2.1] e [dearmazenamentodoMicrosoftAzure] [ dbms-guide-2.3] aplicam-se a implementações com a base de dados do Oracle também.

Tal como explicado anteriormente, na parte geral do documento, existem quotas no débito IOPS de discos do Azure. As quotas exatas são dependendo do tipo VM utilizadas. Pode encontrar uma lista dos tipos VM com as respetivas quotas [aqui (Linux)] [ virtual-machines-sizes-linux] e [aqui (Windows)][virtual-machines-sizes-windows].

Para identificar os tipos de VM do Azure suportados, consulte a nota SAP [1928533]

Desde que a quota atual de IOPS por disco cumprir os requisitos, é possível armazenar todos os ficheiros de DB num único disco montado. 

Se mais IOPS forem necessárias, é recomendado utilizar LVM (Gestor de volumes lógicos) ou MDADM para criar um grande volume lógico através de vários discos montados. Consulte também o capítulo [RAID de Software] [ dbms-guide-2.2] deste documento. Esta abordagem simplifica a sobrecarga de administração para gerir o espaço em disco e evita o esforço necessário para distribuir manualmente os arquivos em vários discos montados.

#### <a name="backup--restore"></a>Cópia de Segurança / Restauro
Para cópia de segurança / restaurar a funcionalidade, o SAP BR * ferramentas para Oracle são suportadas da mesma forma que no bare-metal e Hyper-V. O Gestor de recuperação de Oracle (RMAN) também é suportado para cópias de segurança em disco e restaurar a partir de disco.

#### <a name="high-availability"></a>Elevada Disponibilidade
Oracle Data Guard é suportada para elevada disponibilidade e fins de recuperação após desastre. Detalhes podem ser encontrados na [isso] [ virtual-machines-windows-classic-configure-oracle-data-guard] documentação.

#### <a name="other"></a>Outros
Todas as outras áreas gerais, como conjuntos de disponibilidade do Azure ou SAP monitorização aplicam-se conforme descrito nos primeiros três capítulos deste documento para implementações de VMs com a base de dados do Oracle também.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Informações específicas para a base de dados do SAP MaxDB no Windows
### <a name="sap-maxdb-version-support"></a>Suporte para a versão de MaxDB SAP
SAP atualmente suporta SAP MaxDB versão 7.9 para uso com produtos com base em SAP NetWeaver no Azure. Todas as atualizações para o servidor SAP MaxDB ou os controladores ODBC e JDBC para ser utilizado com produtos baseados em SAP NetWeaver são fornecidas exclusivamente através do SAP Service Marketplace em <https://support.sap.com/swdc>.
Informações gerais sobre como executar o SAP NetWeaver no SAP MaxDB podem ser encontradas em <https://www.sap.com/community/topic/maxdb.html>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Tipos de versões do Microsoft Windows e a VM do Azure suportados para SAP MaxDB DBMS
Para obter a versão suportada do Microsoft Windows para SAP MaxDB DBMS no Azure, veja:

* [Matriz de disponibilidade de produto para SAP (PAM)][sap-pam]
* A nota SAP [1928533]

É altamente recomendado para utilizar a versão mais recente do sistema operacional Microsoft Windows, que é o Microsoft Windows 2012 R2.

### <a name="available-sap-maxdb-documentation"></a>Documentação de MaxDB SAP disponíveis
Pode encontrar a lista atualizada de documentação do SAP MaxDB na nota SAP seguintes [767598]

### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Diretrizes de configuração do SAP MaxDB para instalações de SAP em VMs do Azure
#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Configuração de armazenamento
Melhores práticas do storage do Azure para SAP MaxDB siga as recomendações gerais mencionadas no capítulo [estrutura de uma implementação de RDBMS][dbms-guide-2].

> [!IMPORTANT]
> Como outras bases de dados, o SAP MaxDB também tem ficheiros de registo e dados. No entanto, na terminologia de SAP MaxDB o termo correto é "volume" (não "file"). Por exemplo, existem SAP MaxDB volumes de dados e volumes de registo. Não confunda com volumes de disco do SO. 
> 
> 

Em resumo é necessário:

* Se utilizar contas de armazenamento do Azure, configure a conta de armazenamento do Azure que contém os volumes de dados e de registo do SAP MaxDB (ou seja, ficheiros) como **Local o armazenamento redundante (LRS)** conforme especificado no capítulo [dearmazenamentodoMicrosoftAzure][dbms-guide-2.3].
* Separe o caminho de e/s para volumes de dados SAP MaxDB (ou seja, ficheiros) do caminho de e/s para volumes de registo (ou seja, ficheiros). Isso significa que os volumes de dados SAP MaxDB (ou seja, ficheiros) tem de ser instalado numa unidade lógica e volumes de registo de SAP MaxDB (ou seja, ficheiros) tem de ser instalado em outra unidade lógica.
* Definir o tipo de colocação em cache adequado para cada disco, dependendo se o utilizar para SAP MaxDB dados ou log de volumes (por exemplo, ficheiros) e, se utilizar do Azure Standard ou Premium Storage do Azure, conforme descrito no capítulo [colocação em cache para discos de dados e VMs] [dbms-guide-2.1].
* Desde que a quota atual de IOPS por disco cumprir os requisitos, é possível armazenar todos os volumes de dados num único disco montado, e também armazenar todos os volumes de registo de base de dados noutro disco montado único.
* Se forem necessário, mais IOPS e/ou espaço, é recomendado utilizar agrupamentos de armazenamento a janela da Microsoft (apenas disponíveis no Microsoft Windows Server 2012 e superior) ou a repartição de Microsoft Windows para o Microsoft Windows 2008 R2 para criar um dispositivo lógico grande ao longo de vários discos montados. Consulte também o capítulo [RAID de Software] [ dbms-guide-2.2] deste documento. Esta abordagem simplifica a sobrecarga de administração para gerir o espaço em disco e evita o esforço de distribuição de arquivos manualmente em vários discos montados.
* Para os requisitos mais altos de IOPS, pode utilizar o armazenamento Premium do Azure, que está disponível em VMs de série GS e de série DS.

![Referência de configuração da VM de IaaS do Azure para SAP MaxDB DBMS][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Cópia de segurança e restauro
Ao implementar SAP MaxDB no Azure, tem de rever sua metodologia de cópia de segurança. Mesmo que o sistema não é um sistema de produtividade, a base de dados do SAP hospedado pelo SAP MaxDB deve ser cópias periodicamente. Uma vez que o armazenamento do Azure mantém três imagens, uma cópia de segurança agora é menos importante em termos de proteger seu sistema contra falhas de armazenamento e mais importantes falhas operacionais ou administrativas. O principal motivo para manter uma cópia de segurança adequada e um plano de restauro é para que pode compensar erros de lógicos ou manuais, fornecendo capacidades de restauro para ponto anterior no tempo. Assim, o objetivo é utilizar cópias de segurança para restaurar a base de dados para um determinado ponto no tempo ou utilizar as cópias de segurança no Azure para efetuar o seeding outro sistema ao copiar a base de dados existente. Por exemplo, poderia transferir de uma configuração de SAP de 2 camadas para uma configuração de sistema de 3 camadas do mesmo sistema restaurando uma cópia de segurança.

Fazer backup e restaurar uma base de dados do Azure funciona da mesma forma como o faz para os sistemas no local, pelo que pode utilizar ferramentas de cópia de segurança/restauro SAP MaxDB padrão, que são descritas em um dos documentos de documentação de SAP MaxDB listados na nota SAP [767598]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Considerações de desempenho para a cópia de segurança e restauro
Como nas implementações bare-metal, o desempenho de cópia de segurança e restauro é dependente de quantos volumes podem ser lidos em paralelo e o débito desses volumes. Além disso, o consumo de CPU utilizado pela compressão de cópias de segurança pode desempenham um papel significativo em VMs com até oito threads de CPU. Por conseguinte, um pode assumir:

* Menos o número de discos utilizados para armazenar os dispositivos de base de dados, menor o débito de leitura global
* Menor será que o número de CPU threads na VM, o mais grave o impacto da compactação de backup
* Os destinos menos (diretórios do Stripe, discos) para escrever a cópia de segurança para a inferior a taxa de transferência

Para aumentar o número de destinos para escrever, existem duas opções que pode utilizar, possivelmente em combinação, consoante as suas necessidades:

* Dedicar volumes separados para cópia de segurança
* Repartição de volume de destino de cópia de segurança ao longo de vários discos montados para melhorar o débito IOPS nesse volume de disco repartido
* A ser dispositivos de disco lógico dedicado separado para:
  * SAP MaxDB volumes de cópia de segurança (ou seja, ficheiros)
  * SAP MaxDB volumes de dados (ou seja, ficheiros)
  * SAP MaxDB volumes de registo (ou seja, ficheiros)

Repartição de um volume ao longo de vários discos montados já foi discutida anteriormente no capítulo [RAID de Software] [ dbms-guide-2.2] deste documento. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Outros
Todas as outras áreas gerais, como a monitorização de conjuntos de disponibilidade do Azure ou SAP também se aplicam, tal como descrito nos primeiros três capítulos deste documento para implementações de VMs com a base de dados do SAP MaxDB.
Outras configurações de SAP MaxDB específicas são transparentes para VMs do Azure e são descritas em documentos diferentes listados na nota SAP [767598] e nestas notas SAP:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Especificações da SAP liveCache no Windows
### <a name="sap-livecache-version-support"></a>Suporte para a versão do SAP liveCache
Versão mínima do liveCache SAP suportada em máquinas de virtuais do Azure está **SAP LC/LCAPPS 10.0 SP 25** incluindo **liveCache 7.9.08.31** e **25 de compilação de LCA**, lançada para **EhP 2 para o SAP SCM 7.0** e superior.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Tipos de versões do Microsoft Windows e a VM do Azure suportados para liveCache do SAP DBMS
Para obter a versão suportada do Microsoft Windows para liveCache SAP no Azure, veja:

* [Matriz de disponibilidade de produto para SAP (PAM)][sap-pam]
* A nota SAP [1928533]

É altamente recomendado para utilizar a versão mais recente do sistema operacional Microsoft Windows Server. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache diretrizes de configuração para instalações de SAP em VMs do Azure
#### <a name="recommended-azure-vm-types"></a>Recomendado tipos de VM do Azure
Como o SAP liveCache é um aplicativo que realiza cálculos enormes, a quantidade e velocidade de RAM e da CPU tem uma grande influência sobre o desempenho de liveCache SAP. 

Para os tipos de VM do Azure suportados pelo SAP (nota SAP [1928533]), todos os recursos de CPU virtual atribuídos à VM são apoiados por recursos de CPU físicos dedicados do hipervisor. Ocorre sem excesso (e, portanto, nenhuma concorrência de recursos da CPU).

Da mesma forma, para tipos de instância do todas as VM do Azure suportados pelo SAP, a memória da VM é 100% mapeado para a memória física - aprovisionar em excesso (alocação excessiva), por exemplo, não é utilizado.

Sob essa perspectiva, é altamente recomendado para utilizar o novo tipo de VM do Azure de série DS (em combinação com o armazenamento Premium do Azure) ou de série D, pois têm 60% processadores mais rápidos do que as de série. Para a carga de RAM e da CPU mais elevada, pode utilizar a série G e as VMs da série GS (em combinação com o armazenamento Premium do Azure) com o Intel mais recente?? Xeon?? família de processadores E5 v3, que têm duas vezes a memória e quatro vezes o armazenamento de unidade de estado sólido (SSDs) da D/DS série.

#### <a name="storage-configuration"></a>Configuração do Armazenamento
Como o SAP liveCache baseia-se na tecnologia de SAP MaxDB, o armazenamento do Azure melhores práticas recomendadas mencionadas para SAP MaxDB capítulo [configuração de armazenamento] [ dbms-guide-8.4.1] também são válidas para SAP liveCache. 

#### <a name="dedicated-azure-vm-for-livecache"></a>VM do Azure dedicada para liveCache
Como SAP liveCache forma usa a capacidade computacional, para utilização produtiva Recomendamos para implementar no Azure Máquina Virtual dedicada. 

![Dedicado a VM do Azure para liveCache caso de utilização produtivos][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Cópia de Segurança e Restauro
cópia de segurança e restauro, incluindo as considerações de desempenho, já são descritos os capítulos relevantes do SAP MaxDB [backup e restauração] [ dbms-guide-8.4.2] e [considerações de desempenho para cópia de segurança e restaurar][dbms-guide-8.4.3]. 

#### <a name="other"></a>Outros
Todas as outras áreas gerais já estão descritas em MaxDB de SAP relevantes [isso] [ dbms-guide-8.4.4] capítulo. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>Informações específicas para o servidor de conteúdo de SAP no Windows
O servidor de conteúdo de SAP é um componente separado, com base no servidor para armazenar o conteúdo, como documentos eletrônicos em diferentes formatos. O servidor de conteúdo de SAP é fornecido pelo desenvolvimento da tecnologia e deve ser utilizada em várias aplicações para todas as aplicações SAP. Ele é instalado num sistema separado. Conteúdo típico é o material de treinamento e a documentação do armazém de dados de conhecimento ou técnicos desenhos provenientes do mySAP PLM sistema de gerenciamento de documentos. 

### <a name="sap-content-server-version-support"></a>Suporte de versão do servidor de conteúdo de SAP
SAP atualmente suporta:

* **Servidor de conteúdo de SAP** com a versão **6.50 (e versões posteriores)**
* **SAP MaxDB versão 7.9**
* **Microsoft IIS (Internet Information Server) versão 8.0 (e superior)**

É altamente recomendável usar a versão mais recente de SAP do servidor de conteúdo, que, no momento da escrita deste documento é **6.50 SP4**e a versão mais recente do **Microsoft IIS 8.5**. 

Verifique as mais recentes versões suportadas do servidor de conteúdo do SAP e o IIS na Microsoft da [matriz de disponibilidade de produto do SAP (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Tipos de Microsoft Windows e a VM do Azure com suporte para servidor de conteúdo do SAP
Para obter uma versão suportada do Windows para o servidor de conteúdo de SAP no Azure, veja:

* [Matriz de disponibilidade de produto para SAP (PAM)][sap-pam]
* A nota SAP [1928533]

É altamente recomendado para utilizar a versão mais recente do Microsoft Windows Server.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Diretrizes de configuração do servidor de conteúdo de SAP para instalações de SAP em VMs do Azure
#### <a name="storage-configuration"></a>Configuração do Armazenamento
Se configurar o servidor de conteúdo do SAP para armazenar os ficheiros da base de dados do SAP MaxDB, todo o armazenamento do Azure melhores práticas recomendação mencionada para SAP MaxDB capítulo [configuração de armazenamento] [ dbms-guide-8.4.1] também são válidos para o cenário de servidor de conteúdo de SAP. 

Se configurar o servidor de conteúdo do SAP para armazenar os ficheiros no sistema de arquivos, recomenda-se para utilizar uma unidade lógica dedicada. Utilizar espaços de armazenamento do Windows permite-lhe também aumentar o tamanho de disco lógico e o débito IOPS, conforme descrito no capítulo [RAID de Software][dbms-guide-2.2]. 

#### <a name="sap-content-server-location"></a>Localização do servidor de conteúdo de SAP
Servidor de conteúdo de SAP tem de ser implementada na mesma região do Azure e a VNET do Azure em que o sistema SAP é implementado. É livre para decidir se pretende implementar componentes de servidor de conteúdo de SAP numa VM do Azure dedicado ou na mesma VM onde está a executar o sistema SAP. 

![VM do Azure dedicada para o servidor de conteúdo do SAP][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>Localização do servidor de Cache SAP
O servidor de Cache do SAP é um componente baseado em servidor adicional para fornecer acesso a documentos de (em cache) localmente. O servidor de Cache do SAP armazena em cache os documentos de um servidor de conteúdo de SAP. Isso serve para otimizar o tráfego de rede se documentos tem de ser obtido mais do que uma vez a partir de localizações diferentes. A regra geral é que o servidor de Cache do SAP tem estivesse fisicamente próximo do cliente que acessa o servidor de Cache do SAP. 

Aqui tem duas opções:

1. **Cliente é um sistema SAP de back-end** se um sistema SAP de back-end estiver configurado para aceder ao servidor de conteúdo do SAP, esse sistema SAP é um cliente. Como o sistema SAP e o servidor de conteúdo de SAP são implementadas na mesma região do Azure, no mesmo datacenter do Azure, estão fisicamente perto uns dos outros. Por conseguinte, não é necessário ter um servidor de Cache dedicada do SAP. Clientes de interface do Usuário do SAP (navegador da web ou de GUI do SAP), acessar o sistema SAP diretamente e o sistema SAP obtém os documentos do servidor de conteúdo de SAP.
2. **Cliente é um navegador da web no local** o servidor de conteúdo de SAP pode ser configurado para ser acedido diretamente pelo browser. Neste caso, um navegador da web em execução no local é um cliente do servidor de conteúdo do SAP. Datacenter no local e datacenter do Azure são colocados em diferentes localizações físicas (Idealmente perto uns dos outros). Seu datacenter no local está ligado ao Azure através do Azure Site-Site VPN ou ExpressRoute. Embora as duas opções oferecem a ligação de rede VPN segura para o Azure, ligação de rede de site a site não oferece um SLA de largura de banda e latência de rede entre o datacenter no local e do datacenter do Azure. Para acelerar o acesso a documentos, pode fazer o seguinte:
   1. Instalar o servidor de Cache de SAP no local, fechar para navegador da web no local (opção [isso] [ dbms-guide-900-sap-cache-server-on-premises] figura)
   2. Configure o Azure ExpressRoute, que oferece uma ligação de rede dedicada de alta velocidade e baixa latência entre o datacenter no local e o datacenter do Azure.

![Opção para instalar o servidor de Cache de SAP no local][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Cópia de Segurança / Restauro
Se configurar o servidor de conteúdo do SAP para armazenar os ficheiros da base de dados do SAP MaxDB, as considerações de desempenho e o procedimento de cópia de segurança/restauro já estão descritas no capítulo de SAP MaxDB [Backup e restauração] [ dbms-guide-8.4.2]e o capítulo [considerações sobre o desempenho de Backup e restauração][dbms-guide-8.4.3]. 

Se configurar o servidor de conteúdo do SAP para armazenar os ficheiros no sistema de arquivos, uma opção é a execução manual de cópia de segurança/restauro da estrutura de todo o ficheiro onde estão localizados os documentos. Assim como o SAP MaxDB cópia de segurança/restauro, é recomendado ter um volume de disco dedicado para fins de cópia de segurança. 

#### <a name="other"></a>Outros
Outras definições específicas do servidor SAP conteúdo são transparentes para VMs do Azure e são descritas em vários documentos e notas de SAP:

* <https://service.sap.com/contentserver> 
* A nota SAP [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>Informações específicas ao IBM DB2 para LUW no Windows
Com o Microsoft Azure, pode migrar facilmente a sua aplicação existente de SAP em execução no IBM DB2 para Linux, UNIX e Windows (LUW) para máquinas virtuais do Azure. Com o SAP no IBM DB2 para LUW, os administradores e desenvolvedores podem continuar a utilizar o mesmo desenvolvimento e ferramentas de administração, que estão disponíveis no local.
Informações gerais sobre como executar o SAP Business Suite no IBM DB2 para LUW pode ser encontrada na rede de Comunidade do SAP (SCN) em <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Para obter mais informações e atualizações sobre o SAP no DB2 para LUW no Azure, consulte a nota SAP [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 para suporte da versão do Windows, Linux e UNIX
SAP no IBM DB2 para LUW nos serviços de Máquina Virtual do Microsoft Azure é suportado a partir da versão de DB2 10.5.

Para obter informações sobre produtos SAP suportados e tipos de VM do Azure, consulte a nota SAP [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 para Linux, UNIX e diretrizes de configuração do Windows para instalações de SAP em VMs do Azure
#### <a name="storage-configuration"></a>Configuração do Armazenamento
Todos os ficheiros de base de dados devem ser armazenados no sistema de ficheiros NTFS com base em discos ligados diretamente. Estes discos estão montados à VM do Azure e baseiam-se no armazenamento de BLOBS de página do Azure (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) ou Managed Disks (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Qualquer tipo de unidades de rede ou partilhas remotas, como são os seguintes serviços de ficheiros do Azure **não** suportado para ficheiros de base de dados: 

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

Se estiver a utilizar discos com base no armazenamento de BLOBS de página do Azure ou Managed Disks, as declarações feitas neste documento capítulo [estrutura de uma implementação de RDBMS] [ dbms-guide-2] também se aplicam a implementações com o IBM DB2 para LUW Base de dados. 

Tal como explicado anteriormente, na parte geral do documento, existem quotas no débito IOPS para discos. As quotas exatas dependem o tipo de VM utilizado. Pode encontrar uma lista dos tipos VM com as respetivas quotas [aqui (Linux)] [ virtual-machines-sizes-linux] e [aqui (Windows)][virtual-machines-sizes-windows].

Desde que a quota atual de IOPS por disco é suficiente, é possível armazenar todos os ficheiros de base de dados num único disco montado. 

Para obter um desempenho considerações também consulte capítulo "Segurança de dados e considerações de desempenho para os diretórios de base de dados" em guias de instalação SAP.

Em alternativa, pode utilizar agrupamentos de armazenamento do Windows (apenas disponíveis no Windows Server 2012 e superior) ou repartição do Windows para o Windows 2008 R2, como descrito no capítulo [RAID de Software] [ dbms-guide-2.2] deste documento para Crie um dispositivo lógico grande ao longo de vários discos.
Para os discos que contém os caminhos de armazenamento de DB2 para os diretórios sapdata e saptmp, tem de especificar um tamanho de setor de disco físico de 512 KB. Quando utilizar agrupamentos de armazenamento do Windows, tem de criar agrupamentos de armazenamento manualmente por meio de interface de linha de comandos utilizando o parâmetro `-LogicalSectorSizeDefault`. Para obter mais informações, consulte <https://technet.microsoft.com/itpro/powershell/windows/storage/new-storagepool>.

#### <a name="backuprestore"></a>Backup/Restauro
A funcionalidade de cópia de segurança/restauro para IBM DB2 para LUW é suportada da mesma forma que no padrão sistemas operacionais Windows Server e Hyper-V.

Deve certificar-se de que tem uma estratégia de cópia de segurança da base de dados válido em vigor. 

Como nas implementações bare-metal, cópia de segurança/restaurar desempenho depende quantos volumes podem ser lidos em paralelo e o que pode ser o débito desses volumes. Além disso, o consumo de CPU utilizado pela compressão de cópias de segurança pode desempenham um papel significativo em VMs com até oito threads de CPU. Por conseguinte, um pode assumir:

* Menos o número de discos utilizados para armazenar os dispositivos de base de dados, menor será a produtividade geral em ler
* Menor será que o número de CPU threads na VM, o mais grave o impacto da compactação de backup
* Os destinos menos (diretórios do Stripe, discos) para escrever a cópia de segurança para a inferior a taxa de transferência

Para aumentar o número de destinos para escrever, as duas opções podem ser utilizado/combinadas consoante as suas necessidades:

* Repartição de volume de destino de cópia de segurança ao longo de vários discos para aumentar o débito IOPS nesse volume repartidos
* Utilizar mais do que um diretório de destino para escrever a cópia de segurança

#### <a name="high-availability-and-disaster-recovery"></a>Elevada disponibilidade e recuperação após desastre
Microsoft Cluster Server (MSCS) não é suportada.

DB2 elevada disponibilidade a recuperação após desastre (HADR) é suportada. Se as máquinas virtuais da configuração HA tiver que trabalhar com resolução de nomes, a configuração no Azure não diferem dos qualquer configuração que é feita no local. Não é recomendado dependem apenas a resolução IP.

Não utilize a Georreplicação para as contas de armazenamento que armazenam os discos de base de dados. Para obter mais informações, consulte o capítulo [armazenamento do Microsoft Azure] [ dbms-guide-2.3] e o capítulo [elevada disponibilidade e recuperação após desastre com VMs do Azure] [ dbms-guide-3].

#### <a name="other"></a>Outros
Todas as outras áreas gerais, como conjuntos de disponibilidade do Azure ou SAP monitorização aplicam-se conforme descrito nos primeiros três capítulos deste documento para implementações de VMs com o IBM DB2 para LUW também. 

Consulte também o capítulo [gerais SQL Server para o SAP no Azure resumo][dbms-guide-5.8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>Informações específicas ao IBM DB2 para LUW no Linux
Com o Microsoft Azure, pode migrar facilmente a sua aplicação existente de SAP em execução no IBM DB2 para Linux, UNIX e Windows (LUW) para máquinas virtuais do Azure. Com o SAP no IBM DB2 para LUW, os administradores e desenvolvedores podem continuar a utilizar o mesmo desenvolvimento e ferramentas de administração, que estão disponíveis no local. Informações gerais sobre como executar o SAP Business Suite no IBM DB2 para LUW pode ser encontrada na rede de Comunidade do SAP (SCN) em <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Para obter mais informações e atualizações sobre o SAP no DB2 para LUW no Azure, consulte a nota SAP [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 para suporte da versão do Windows, Linux e UNIX
SAP no IBM DB2 para LUW nos serviços de Máquina Virtual do Microsoft Azure é suportado a partir da versão de DB2 10.5.

Para obter informações sobre produtos SAP suportados e tipos de VM do Azure, consulte a nota SAP [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 para Linux, UNIX e diretrizes de configuração do Windows para instalações de SAP em VMs do Azure
#### <a name="storage-configuration"></a>Configuração do Armazenamento
Todos os ficheiros de base de dados devem ser armazenados num sistema de ficheiros com base em discos ligados diretamente. Estes discos estão montados à VM do Azure e baseiam-se no armazenamento de BLOBS de página do Azure (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) ou Managed Disks (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Qualquer tipo de unidades de rede ou partilhas remotas, como são os seguintes serviços de ficheiros do Azure **não** suportado para ficheiros de base de dados:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

Se estiver a utilizar discos com base no armazenamento de BLOBS de página do Azure, as declarações feitas neste documento capítulo [estrutura de uma implementação de RDBMS] [ dbms-guide-2] também se aplicam a implementações com o IBM DB2 para LUW base de dados.

Tal como explicado anteriormente, na parte geral do documento, existem quotas no débito IOPS para discos. As quotas exatas dependem o tipo de VM utilizado. Pode encontrar uma lista dos tipos VM com as respetivas quotas [aqui (Linux)] [ virtual-machines-sizes-linux] e [aqui (Windows)][virtual-machines-sizes-windows].

Desde que a quota atual de IOPS por disco é suficiente, é possível armazenar todos os ficheiros de base de dados num único disco.

Para obter um desempenho considerações também consulte capítulo "Segurança de dados e considerações de desempenho para os diretórios de base de dados" em guias de instalação SAP.

Em alternativa, pode utilizar LVM (Gestor de volumes lógicos) ou MDADM conforme descrito no capítulo [RAID de Software] [ dbms-guide-2.2] deste documento para criar um dispositivo lógico grande ao longo de vários discos.
Para os discos que contém os caminhos de armazenamento de DB2 para os diretórios sapdata e saptmp, tem de especificar um tamanho de setor de disco físico de 512 KB.

#### <a name="backuprestore"></a>Backup/Restauro
A funcionalidade de cópia de segurança/restauro para IBM DB2 para LUW é suportada da mesma forma que no padrão Linux instalação no local.

Deve certificar-se de que tem uma estratégia de cópia de segurança da base de dados válido em vigor.

Como nas implementações bare-metal, cópia de segurança/restaurar desempenho depende quantos volumes podem ser lidos em paralelo e o que pode ser o débito desses volumes. Além disso, o consumo de CPU utilizado pela compressão de cópias de segurança pode desempenham um papel significativo em VMs com até oito threads de CPU. Por conseguinte, um pode assumir:

* Menos o número de discos utilizados para armazenar os dispositivos de base de dados, menor será a produtividade geral em ler
* Menor será que o número de CPU threads na VM, o mais grave o impacto da compactação de backup
* Os destinos menos (diretórios do Stripe, discos) para escrever a cópia de segurança para a inferior a taxa de transferência

Para aumentar o número de destinos para escrever, as duas opções podem ser utilizado/combinadas consoante as suas necessidades:

* Repartição de volume de destino de cópia de segurança ao longo de vários discos para aumentar o débito IOPS nesse volume repartidos
* Utilizar mais do que um diretório de destino para escrever a cópia de segurança

#### <a name="high-availability-and-disaster-recovery"></a>Elevada disponibilidade e recuperação após desastre
DB2 elevada disponibilidade a recuperação após desastre (HADR) é suportada. Se as máquinas virtuais da configuração HA tiver que trabalhar com resolução de nomes, a configuração no Azure não diferem dos qualquer configuração que é feita no local. Não é recomendado dependem apenas a resolução IP.

Não utilize a Georreplicação para as contas de armazenamento que armazenam os discos de base de dados. Para obter mais informações, consulte o capítulo [armazenamento do Microsoft Azure] [ dbms-guide-2.3] e o capítulo [elevada disponibilidade e recuperação após desastre com VMs do Azure] [ dbms-guide-3].

#### <a name="other"></a>Outros
Todos os outros tópicos gerais, como conjuntos de disponibilidade do Azure ou SAP monitorização aplicam-se conforme descrito nos primeiros três capítulos deste documento para implementações de VMs com o IBM DB2 para LUW também.

Consulte também o capítulo [gerais SQL Server para o SAP no Azure resumo][dbms-guide-5.8].


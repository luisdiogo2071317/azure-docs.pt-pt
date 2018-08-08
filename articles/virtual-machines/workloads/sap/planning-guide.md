---
title: Máquinas de virtuais de planeamento e implementação para o SAP NetWeaver do Azure | Documentos da Microsoft
description: Máquinas de virtuais de planeamento e implementação para o SAP NetWeaver do Azure
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: da7d3898e1385119c8241efc89c68a6a60c29994
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39619095"
---
# <a name="azure-virtual-machines-planning-and-implementation-for-sap-netweaver"></a>Máquinas de virtuais de planeamento e implementação para o SAP NetWeaver do Azure
[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
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
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/planning-monitoring-overview-2502.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-2901]:media/virtual-machines-shared-sap-planning-guide/2901-azure-ha-sap-ha-md.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-3201]:media/virtual-machines-shared-sap-planning-guide/3201-sap-ha-with-sql-md.png
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
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md
[storage-premium-storage-preview-portal]:../../windows/premium-storage.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../extensions/agent-linux.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../extensions/agent-windows.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-windows-capture-image-resource-manager]:../../windows/capture-image.md
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-create-upload-vhd-oracle]:../../linux/oracle-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
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
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/template-samples.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/manage-virtual-network.md#create-a-virtual-network
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics-windows]:../../windows/multiple-nics.md
[virtual-networks-multiple-nics-linux]:../../linux/multiple-nics.md
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
[capture-image-linux-step-2-create-vm-image]:../../linux/capture-image.md#step-2-create-vm-image

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Microsoft Azure permite às empresas a adquirir recursos de computação e armazenamento no tempo mínimo sem ciclos de aprovisionamento demorada. Máquinas virtuais do Azure permitem que as empresas implantem aplicativos clássicas, como o SAP NetWeaver, com base em aplicações para o Azure e expandir seus fiabilidade e disponibilidade sem ter ainda mais recursos disponíveis no local. Serviços de Máquina Virtual do Azure também suporta a conectividade em vários locais, que permite às empresas a integrar ativamente máquinas virtuais do Azure em seus domínios no local, suas nuvens privadas e seu ambiente de sistema SAP.
Este white paper descreve os conceitos básicos da Máquina Virtual do Microsoft Azure e fornece uma orientação de considerações de planeamento e implementação para instalações do SAP NetWeaver no Azure e como tal, deve ser o documento para ler antes de iniciar real implementações do SAP NetWeaver no Azure.
O documento complementa a documentação de instalação de SAP e SAP Notes, que representam os recursos principais para instalações e implementações de SAP software em determinadas plataformas.

## <a name="summary"></a>Resumo
Informática em nuvem é um termo bastante utilizado, o que está a ganhar cada vez mais importância no setor de TI, de pequenas empresas até multinacionais e grandes corporações.

Microsoft Azure é a plataforma de serviços Cloud da Microsoft, que oferece um amplo espetro de novas possibilidades. Agora os clientes podem rapidamente aprovisionar e desaprovisionar aplicativos como um serviço na cloud, pelo que não estão limitadas às restrições técnicas ou orçamentais. Em vez de investirem tempo e dinheiro na infraestrutura de hardware, as empresas podem se concentrar no aplicativo, os processos de negócios e seus benefícios para os clientes e utilizadores.

Com os Serviços da Máquina Virtual do Microsoft Azure, a Microsoft oferece uma plataforma completa de Infraestrutura como um Serviço (IaaS). As aplicações baseadas em SAP NetWeaver são suportadas em Máquinas Virtuais do Azure (IaaS). Este White Paper descreve como planear e implementar aplicações baseadas em SAP NetWeaver no Microsoft Azure como plataforma de eleição.

O documento em si se concentra em dois aspectos principais:

* A primeira parte descreve dois padrões de implementação suportadas para aplicações baseadas em SAP NetWeaver no Azure. Também descreve processamento geral do Azure com implementações de SAP em mente.
* Os detalhes da segunda parte implementando os dois cenários diferentes, descritos na primeira parte.

Para obter recursos adicionais consulte o capítulo [recursos] [ planning-guide-1.2] neste documento.

### <a name="definitions-upfront"></a>Definições de início
Em todo o documento, podemos utilizar os seguintes termos:

* IaaS: Infraestrutura como serviço
* PaaS: Plataforma como um serviço
* SaaS: Software como serviço
* ARM: O Azure Resource Manager
* Componente SAP: um SAP aplicativos individuais como ECC, BW, Gestor de solução ou EP.  Componentes SAP podem basear-se em tecnologias tradicionais de ABAP ou Java ou um aplicativo de não com base em NetWeaver, como objetos comerciais.
* Ambiente de SAP: um ou mais componentes SAP logicamente agrupados para executar uma função de negócio, como o desenvolvimento, QAS, treinamento, DR ou de produção.
* Ambiente SAP: Refere-se para os ativos SAP inteiros num cliente paisagem IT. A paisagem SAP inclui todos os ambientes de não produção e produção.
* Sistema SAP: A combinação de camada DBMS e a camada de aplicação de, por exemplo, um sistema de desenvolvimento do SAP ERP, sistema de teste de SAP BW, sistema de produção do SAP CRM, etc. Nas implementações do Azure, não é suportada para dividir estas duas camadas entre no local e o Azure. Isso significa que um sistema SAP é implementado no local ou está implementado no Azure. No entanto, pode implantar os sistemas diferentes de um ambiente SAP para o Azure ou no local. Por exemplo, pode implementar o desenvolvimento de CRM do SAP e testar sistemas no Azure, mas a SAP CRM produção sistema no local.
* Implementação apenas na cloud: uma implementação em que a subscrição do Azure não está ligada através de um site a site ou a ligação do ExpressRoute para a infraestrutura de rede no local. Em comum documentação do Azure esses tipos de implementações também são descritos como implementações de "Apenas na Cloud". Máquinas virtuais implementadas com este método são acedidas através da internet e um endereço IP público e/ou um nome DNS público atribuído para as VMs no Azure. Para o diretório de Active Directory no local (AD) do Microsoft Windows e o DNS não estiver expandido para o Azure nesses tipos de implementações. Por conseguinte, as VMs não fazem parte do Active Directory no local. Mesmo se aplica para implementações de Linux utilizar, por exemplo, OpenLDAP + Kerberos.

> [!NOTE]
> Uma implementação apenas na cloud neste documento é definida como completas panoramas SAP em execução exclusivamente no Azure sem a extensão do Active Directory / OpenLDAP ou da resolução do nome do local para a cloud pública. Configurações de apenas na cloud não são suportadas para sistemas SAP de produção ou configurações onde SAP STMS ou a outros recursos no local tem de ser usada entre sistemas SAP alojados no Azure e recursos que residem no local.
>
>

* Em vários locais: Descreve um cenário em que as VMs são implementadas para uma subscrição do Azure que tem conectividade do ExpressRoute entre o datacenter(s) no local e o Azure, múltiplos sites ou site a site. Documentação do Azure em comum, esses tipos de implementações também são descritos como cenários de vários locais. O motivo para a ligação é para expandir os domínios no local/OpenLDAP de diretório Active Directory no local e no local DNS para o Azure. O Panorama de no local é expandido para recursos do Azure da subscrição. Com esta extensão, as VMs podem ser parte do domínio no local. Os utilizadores de domínio do domínio no local podem aceder aos servidores e podem executar serviços nessas VMS (como o DBMS serviços). Resolução de nomes e de comunicação entre as VMs implementadas no local e as VMs implementadas do Azure é possível. Este é o cenário que Esperamos que a maioria dos ativos SAP para ser implantado em. Para obter mais informações, consulte [isso] [ vpn-gateway-cross-premises-options] artigo e [isso][vpn-gateway-site-to-site-create].

> [!NOTE]
> Implementações em vários locais dos sistemas SAP em máquinas virtuais do Azure com sistemas SAP são membros de um domínio no local são suportadas para sistemas SAP de produção. Configurações em vários locais são suportadas para a implementação de partes ou concluir cenários SAP no Azure. Até mesmo executar o ambiente completo do SAP no Azure requer ter essas VMs a fazer parte do domínio no local e anúncios/OpenLDAP. Nas versões anteriores da documentação, falamos sobre cenários de TI híbrida, em que o termo *híbrida* está enraizada no fato de que existe uma conectividade em vários locais entre no local e o Azure. Além disso, o fato de que as VMs no Azure fazem parte do Active Directory no local / OpenLDAP.
>
>

Alguma documentação de Microsoft descreve os cenários de em vários locais de forma um pouco diferente, especialmente para configurações de HA do DBMS. No caso dos documentos relacionados com o SAP, o cenário em vários locais apenas se resume a ter uma site a site ou conectividade privada (ExpressRoute) e o fato de que o ambiente SAP é distribuído entre no local e o Azure.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Recursos
Os guias de adicionais seguintes estão disponíveis para o tópico de implementações de SAP no Azure:

* [Máquinas de virtuais de planeamento e implementação para o SAP NetWeaver (neste documento) do Azure][planning-guide]
* [Implementação de máquinas virtuais do Azure para SAP NetWeaver][deployment-guide]
* [Implementação de DBMS de máquinas virtuais do Azure para SAP NetWeaver][dbms-guide]

> [!IMPORTANT]
> Sempre que possível uma ligação para o guia de instalação SAP referência é utilizada (referência InstGuide-01, consulte <http://service.sap.com/instguides>). Quando se trata os pré-requisitos e o processo de instalação, os guias de instalação do SAP NetWeaver devem sempre ser lidos com cuidado, como este documento cobre apenas a tarefas específicas para sistemas SAP NetWeaver instalados numa máquina de Virtual do Microsoft Azure.
>
>

As seguintes notas de SAP estão relacionadas ao tópico de SAP no Azure:

| Número de nota | Cargo |
| --- | --- |
| [1928533] |Aplicações SAP no Azure: produtos suportados e dimensionamento |
| [2015553] |SAP no Microsoft Azure: pré-requisitos do suporte |
| [1999351] |Resolução de problemas avançada a monitorização do Azure para SAP |
| [2178632] |Chave de métricas de monitorização para o SAP no Microsoft Azure |
| [1409604] |Virtualização no Windows: avançada de monitorização |
| [2191498] |SAP no Linux com o Azure: avançada de monitorização |
| [2243692] |Linux no Microsoft Azure (IaaS) VM: problemas de licença do SAP |
| [1984787] |SUSE LINUX Enterprise Server 12: Observações de instalação |
| [2002167] |Red Hat Enterprise Linux 7.x: instalação e atualização |
| [2069760] |Instalação de SAP do Oracle Linux 7.x e atualização |
| [1597355] |Recomendação de espaço de comutação para Linux |

Leia também a [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) que contém todas as anotações do SAP para o Linux.

Limitações gerais predefinida e as limitações de máximas de subscrições do Azure podem ser encontradas na [este artigo][azure-subscription-service-limits-subscription].

## <a name="possible-scenarios"></a>Cenários possíveis
SAP é normalmente vista como um dos aplicativos mais críticos nas empresas. A arquitetura e as operações desses aplicativos principalmente é muito complexo e é importante garantir que cumpre os requisitos de disponibilidade e desempenho.

Assim, as empresas têm de considerar cuidadosamente quais são os aplicativos podem ser executados num ambiente de cloud pública, independente do provedor na nuvem escolhida.

Tipos de sistemas possíveis para implementar o SAP NetWeaver com base em aplicações na cloud pública ambientes estão listados abaixo:

1. Sistemas de produção de médio porte
2. Sistemas de desenvolvimento
3. Sistemas de testes
4. Sistemas de protótipo
5. Aprendizado / sistemas de demonstração

Para implementar com êxito o sistemas SAP no Azure IaaS ou IaaS em geral, é importante compreender as diferenças significativas entre as ofertas de outsourcers tradicionais ou hosters e ofertas de IaaS. Ao passo que o fornecedor de alojamento tradicional ou controversa adapta-se a infraestrutura (tipo de rede, armazenamento e servidor) para um cliente quiser alojar a carga de trabalho, em vez disso, é responsabilidade do cliente para escolher a carga de trabalho correta para implementações de IaaS.

Como primeiro passo, os clientes têm de verificar os seguintes itens:

* O SAP suportados tipos de VM do Azure
* O SAP suportados/versões de produtos no Azure
* O sistema operacional e o DBMS suportadas versões para as versões específicas do SAP no Azure
* Débito de SAPS fornecido pelo diferentes SKUs do Azure

As respostas a essas perguntas podem ser lidos na nota SAP [1928533].

Como uma segunda etapa, limitações de largura de banda e de recursos do Azure tem de ser comparadas para consumo de recursos reais dos sistemas no local. Por conseguinte, os clientes têm de estar familiarizado com os recursos diferentes dos tipos do Azure suportados com o SAP na área de:

* Recursos de CPU e memória de diferentes tipos VM e
* Largura de banda IOPS de diferentes tipos VM e
* Recursos de rede de diferentes tipos VM.

A maioria desses dados pode ser encontrada [aqui (Linux)] [ virtual-machines-sizes-linux] e [aqui (Windows)][virtual-machines-sizes-windows].

Lembre-se de que os limites listados no link acima são os limites superiores. Não significa que os limites para qualquer um dos recursos, por exemplo IOPS podem ser fornecidos em todas as circunstâncias. Porém, as exceções são os recursos de CPU e memória de um tipo VM escolhido. Para os tipos VM suportados pelo SAP, os recursos de CPU e memória são reservados e como tal, disponível em qualquer ponto no tempo para consumo dentro da VM.

A plataforma Microsoft Azure, como de outras plataformas IaaS é uma plataforma de multi-inquilino. Isso significa que o armazenamento, rede e outros recursos são partilhados entre inquilinos. Lógica de limitação e quota inteligente é utilizada para impedir que um inquilino afetar o desempenho de outro inquilino (vizinhos ruidosos) de forma significativa. Embora a lógica no Azure tenta manter as variações na largura de banda que impediu que plataformas de pequenas e altamente partilhadas tendem introduzir maior variações na disponibilidade de recursos/largura de banda que muitos clientes são utilizados numa suas implementações no local. Como resultado, podem ocorrer diferentes níveis de largura de banda em relação à rede ou armazenamento e/s (o volume como latência) de um minuto a minuto. A probabilidade de que um sistema SAP no Azure pode experimentar as variações maiores do que num sistema no local tem de ser levadas em conta.

Uma última etapa é avaliar requisitos de disponibilidade. Isso pode acontecer, que a infraestrutura do Azure subjacente tem de ser atualizado e requer que os anfitriões a executar VMs de ser reiniciado. Nestes casos, as VMs em execução nesses anfitriões poderiam ser desligadas e reiniciadas também. A temporização da tal manutenção é feita durante o horário comercial de não essenciais para uma região em particular, mas a janela de potenciais de algumas horas durante o qual será a ocorrência de um reinício é relativamente grande. Existem várias tecnologias dentro da plataforma do Azure que podem ser configurados para atenuar alguns ou todos o impacto de tais atualizações. Melhoramentos futuros da plataforma do Azure, DBMS e SAP aplicação foram concebidos para minimizar o impacto de tais reinicializações.

Para poder implementar com êxito um sistema SAP no Azure, no local SAP sistemas sistema operacional, base de dados e aplicações SAP tem de aparecer na matriz de suporte do Azure do SAP, se encaixa nos recursos do Azure pode fornecer a infraestrutura e que podem trabalhar com as ofertas de disponibilidade SLAs Microsoft Azure. Como esses sistemas são identificados, precisa decidir em um dos seguintes cenários de implementação de dois.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Apenas na cloud - implementações de máquinas virtuais no Azure sem dependências na rede de cliente no local
![VM única com o cenário de treinamento implementadas no Azure ou de demonstração SAP][planning-guide-figure-100]

Neste cenário é comum para treinamentos ou sistemas de demonstração, onde todos os componentes do SAP e software não SAP, são instalados dentro de uma única VM. Sistemas SAP de produção não são suportados neste cenário de implementação. Em geral, este cenário cumpre os seguintes requisitos:

* As VMs propriamente ditas são acessíveis através da rede pública. Conectividade de rede direta para as aplicações em execução nas VMs à rede no local da empresa de qualquer proprietário do conteúdo de demonstrações ou treinamentos ou com o cliente não é necessária.
* Em caso de várias VMs que representa o cenário de demonstração ou treinamentos, resolução de nomes e de comunicações de rede tem de funcionar entre as VMs. Mas tem de ser isolados, para que vários conjuntos de VMs podem ser implementados lado a lado, sem interferência comunicações entre o conjunto de VMs.  
* Conectividade com a Internet é necessária para o utilizador final para início de sessão remoto em para as VMs alojadas no Azure. Dependendo do convidado SO, serviços de Terminal/RDS ou VNC/ssh é utilizado para aceder à VM para satisfazer as tarefas de formação ou realizar as demonstrações. Se o SAP portas, tais como 3200, 3300 & 3600 podem também ser exposto a instância da aplicação SAP pode ser acessada em qualquer área de trabalho conectada de Internet.
* Os sistemas SAP (e VM(s)) representam um cenário de autónomo no Azure, que apenas necessita de conectividade de internet pública para acesso do utilizador final e não exige uma conexão com outras VMs no Azure.
* SAPGUI e um browser instalados e executados diretamente na VM.
* É necessário uma reposição rápida de uma VM para o estado original e a nova implementação desse estado original novamente.
* No caso e cenários de treinamento de demonstração, que é percebido em várias VMs, um Active Directory / serviço OpenLDAP e/ou o DNS é necessário para cada conjunto de VMs.

![Grupo de VMS que representa um cenário de treinamento num serviço Cloud do Azure ou de demonstração][planning-guide-figure-200]

É importante ter em mente que a VM (s) em cada um dos conjuntos têm de ser implementada em paralelo, onde os nomes das VMS em cada do conjunto são os mesmos.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Em vários locais - implantação de uma única ou várias VMs de SAP no Azure com o requisito de que está a ser totalmente integrados à rede no local
![VPN com a conectividade de Site a Site (em vários locais)][planning-guide-figure-300]

Este cenário é um cenário em vários locais com muitos padrões de implementação possível. Pode ser descrito simplesmente como executar algumas partes do SAP paisagem no local e a paisagem de outras partes do SAP no Azure. Todos os aspetos do fato de que parte dos componentes do SAP estão em execução no Azure devem ser transparentes para os utilizadores finais. Por conseguinte, o sistema de correção de transporte de SAP (STMS), comunicação de RFC, impressão, segurança (como SSO), etc. funcionam perfeitamente para os sistemas SAP em execução no Azure. Mas o cenário em vários locais também descreve um cenário em que o ambiente SAP completado é executada no Azure com o domínio do cliente e expandidos de DNS para o Azure.

> [!NOTE]
> Este é o cenário de implementação, o que é suportado para a execução de sistemas SAP produtivos.
>
>

Leia [este artigo] [ vpn-gateway-create-site-to-site-rm-powershell] para obter mais informações sobre como ligar a sua rede no local para o Microsoft Azure

> [!IMPORTANT]
> Quando estamos a falar sobre os cenários em vários locais entre implementações de cliente do Azure e no local, estamos à procura na granularidade do todos sistemas SAP. Cenários que são *nepodporuje* para em vários locais cenários são:
>
> * Executar diferentes camadas de aplicações SAP em métodos de implementação diferentes. Por exemplo em execução o DBMS camada no local, mas a camada de aplicação SAP em VMs implementadas como VMs do Azure ou vice versa.
> * Alguns componentes de uma camada SAP no Azure e alguns no local. Por exemplo a divisão de instâncias de camada de aplicação SAP no local e VMs do Azure.
> * Não é suportada a distribuição de VMs que executam o SAP nas instâncias de um sistema ao longo de várias regiões do Azure.
>
> O motivo para essas restrições é o requisito para uma rede de alto desempenho de latência muito baixa dentro de um sistema SAP, especialmente entre instâncias da aplicação e a camada do DBMS de um sistema SAP.
>
>

### <a name="supported-os-and-database-releases"></a>Sistema operacional e as versões de base de dados suportados
* Software de servidor Microsoft suportado para serviços de Máquina Virtual do Azure está listado neste artigo: <http://support.microsoft.com/kb/2721672>.
* Suporte para versões do sistema de operativo, versões de sistema de base de dados suportadas nos serviços de Máquina Virtual do Azure em conjunto com o SAP software estão documentadas na nota SAP [1928533].
* Aplicações SAP e versões suportados nos serviços de Máquina Virtual do Azure estão documentados na nota SAP [1928533].
* Imagens de 64 bits apenas são suportadas para executar como máquinas virtuais convidadas no Azure para cenários SAP. Isso também significa que apenas aplicações de SAP de 64 bits e bases de dados são suportados.

## <a name="microsoft-azure-virtual-machine-services"></a>Serviços de Máquina Virtual do Microsoft Azure
A plataforma Microsoft Azure é uma plataforma de serviços de nuvem à escala da internet hospedado e operado nos centros de dados da Microsoft. A plataforma inclui os serviços de Máquina Virtual do Microsoft Azure (infraestrutura como um serviço ou o IaaS) e um conjunto de uma plataforma multifacetada como recursos de serviço (PaaS).

A plataforma do Azure reduz a necessidade de tecnologia inicial e compras de infraestrutura. Ele simplifica a manutenção e operar aplicativos ao fornecer computadores sob demanda e armazenamento para hospedar, dimensionar e gerir a aplicação web e aplicativos conectados. Gerenciamento da infra-estrutura é automatizado com uma plataforma que foi concebida para elevada disponibilidade e dimensionamento para satisfazer as necessidades de utilização com a opção de um modelo de preços pay as you go dinâmico.

![Posicionamento de serviços de Máquina Virtual do Microsoft Azure][planning-guide-figure-400]

Com os serviços de Máquina Virtual do Azure, Microsoft é permitir que a implementação de imagens de servidor personalizado para o Azure como instâncias de IaaS (consulte a figura 4). As máquinas virtuais no Azure são baseados em unidades de disco rígido virtual (VHD) de Hyper-V e podem executar sistemas operativos diferentes como o SO convidado.

Do ponto de vista operacional, o serviço de Máquina Virtual do Azure oferece experiências semelhantes, como máquinas virtuais implementadas no local. No entanto, ele tem a vantagem significativa que não precisa de aprovisionar, administrar e gerir a infraestrutura. Os desenvolvedores e administradores têm controlo total sobre a imagem de sistema operativo dentro destas máquinas virtuais. Os administradores podem iniciar sessão remotamente essas máquinas virtuais para efetuar a manutenção e solução de problemas de tarefas, bem como tarefas de implementação de software. Em relação à implementação, as restrições só são os tamanhos e as capacidades de VMs do Azure. Podem não ser tão boa granular na configuração, como isso poderia ser feito no local. Há uma opção de tipos VM que representam uma combinação de:

* Número de vCPUs,
* Memória,
* Número de VHDs que podem ser anexados,
* Larguras de banda de rede e armazenamento.

O tamanho e limitações de vários tamanhos de máquinas virtuais diferentes oferecidos podem ser vistos numa tabela no [deste artigo (Linux)] [ virtual-machines-sizes-linux] e [neste artigo (Windows)] [virtual-machines-sizes-windows].

Como percebe, há diferentes famílias ou série das máquinas virtuais. Pode distinguir as seguintes famílias de VMs:

* Tipos de VM a0-A7: nem todos são certificados para SAP. Primeira série VM IaaS do Azure foi introduzido com o.
* Tipos de VM a8-A11: instâncias de computação de alto desempenho. Anfitriões de outras VMs de série A de computação de execução sobre a execução melhor diferentes.
* Tipos de VM da série D/Dv2: de melhor desempenho de A0 a A7. Nem todos os tipos VM são certificados com o SAP.
* Tipos de VM da série DS/DSv2: semelhante da série D/Dv2, mas são consegue ligar ao armazenamento Premium do Azure (consulte o capítulo [armazenamento Premium do Azure] [ planning-guide-3.3.2] deste documento). Novamente nem todos os tipos VM com certificação SAP.
* Tipos de VM da série G: tipos de VM de memória elevada.
* Tipos de VM da série GS: como a série G mas incluindo a opção para utilizar o armazenamento Premium do Azure (consulte o capítulo [armazenamento Premium do Azure] [ planning-guide-3.3.2] deste documento). Ao utilizar VMs da série GS como servidores de base de dados, é obrigatório para utilizar o armazenamento Premium para ficheiros de registo de transações e dados do DB

Pode achar a mesma CPU e configurações de memória de série VM diferente. No entanto, quando procurar pelo desempenho de taxa de transferência estas VMS fora de série de diferente eles poderão diferir significativamente. Apesar de terem a mesma configuração de CPU e memória. Motivo é que o hardware de servidor de anfitrião subjacente na introdução de diferentes tipos VM tinha as características de débito diferentes.  Normalmente, a diferença mostrada no desempenho da produtividade também será refletida no preço das VMs diferentes.

Nem todas as séries VM diferentes, poderão ser disponibilizadas em cada uma das regiões do Azure (para o capítulo seguinte consulte de regiões do Azure). Lembre-se também de que nem todas as VMs ou série de VM é certificada para SAP.

> [!IMPORTANT]
> A utilização de aplicações baseadas em SAP NetWeaver, apenas o subconjunto de tipos de VM e as configurações listadas na nota SAP [1928533] são suportados.
>
>

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Regiões do Azure
Microsoft permite implementar máquinas virtuais em, então, chamada *regiões do Azure*. Uma região do Azure pode ser um ou vários centros de dados que estão localizados próximos. Para a maioria das regiões geopolíticas do mundo, a Microsoft tem, pelo menos, duas regiões do Azure. Por exemplo, na Europa, é uma região do Azure de *Europa do Norte* e uma das *Europa Ocidental*. Essas duas regiões do Azure numa região geopolítica são separados pela distância significativa o suficiente para que os desastres naturais ou técnicas não afetam ambas as regiões do Azure na mesma região geopolítica. Uma vez que a Microsoft é forma constante a criação de novas regiões do Azure em diferentes regiões geopolíticas globalmente, o número de nestas regiões é com crescimento sustentável e a partir de Dezembro de 2015 atingido o número de 20 regiões do Azure com outras regiões anunciadas já. Como um cliente pode implementar sistemas SAP em todas as essas regiões, incluindo duas regiões do Azure na China. Para obter informações atualizadas sobre as regiões do Azure atuais, consulte este Web site: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>O conceito de Máquina Virtual do Microsoft Azure
Microsoft Azure oferece uma infra-estrutura como uma solução de serviço (IaaS) para alojar máquinas virtuais com funcionalidades semelhantes como uma solução de virtualização no local. É possível criar máquinas virtuais a partir do portal do Azure, PowerShell ou a CLI, que também oferecem a implementação e as capacidades de gestão.

O Gestor de Recursos do Azure permite utilizar modelos declarativos para aprovisionar as suas aplicações. Num único modelo, pode implementar vários serviços, bem como as respetivas dependências. Utilize o mesmo modelo para implementar repetidamente a sua aplicação durante cada fase do ciclo de vida do aplicativo.

Obter mais informações sobre como utilizar modelos do Resource Manager podem ser encontradas aqui:

* [Implementar e gerir máquinas virtuais utilizando modelos Azure Resource Manager e a CLI do Azure] [./../linux/create-ssh-secured-vm-from-template.md]
* [Gerir máquinas virtuais utilizando o Azure Resource Manager e PowerShell][virtual-machines-deploy-rmtemplates-powershell]
* <https://azure.microsoft.com/documentation/templates/>

Outro recurso interessante é a capacidade de criar imagens de máquinas virtuais, que permite-lhe preparar determinados repositórios a partir do qual é possível implementar rapidamente as instâncias de Máquina Virtual, o que cumprem os requisitos.

Podem encontrar mais informações sobre a criação de imagens de máquinas virtuais no [deste artigo (Linux)] [ virtual-machines-linux-capture-image-resource-manager] e [neste artigo (Windows)] [ virtual-machines-windows-capture-image-resource-manager].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Domínios de falha
Domínios de falha representam uma unidade física de falha, intimamente relacionada à infraestrutura física contida nos centros de dados, e, embora um blade ou rack físico possa ser considerado um domínio de falha, não há nenhum mapeamento direto entre os dois.

Ao implementar várias máquinas virtuais como parte de um sistema SAP nos serviços de Máquina Virtual do Microsoft Azure, pode influenciar o controlador de recursos de infraestrutura do Azure para implementar a aplicação em diferentes domínios de falha, assim a cumprir os requisitos do SLA do Microsoft Azure. No entanto, a distribuição dos domínios de falha através de uma unidade de escala do Azure (coleção de centenas de nós de computação ou nós de armazenamento e rede) ou a atribuição de VMs para um domínio de falha específica é algo sobre o qual não tem controle direto. Para direcionar o controlador de malha do Azure para implementar um conjunto de VMs através de diferentes domínios de falha, terá de atribuir um conjunto de disponibilidade do Azure para as VMs no momento da implementação. Para obter mais informações sobre conjuntos de disponibilidade do Azure, consulte o capítulo [conjuntos de disponibilidade do Azure] [ planning-guide-3.2.3] neste documento.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Domínios de atualização
Domínios de atualização representam uma unidade lógica que ajudam a determinar a forma como uma VM dentro de um sistema SAP, que consiste em instâncias SAP em execução em várias VMs, é atualizada. Quando uma atualização ocorre, Microsoft Azure executa o processo atualiza esses domínios de atualização individualmente. Ao propagar as VMs no momento da implementação através de diferentes domínios de atualização, pode proteger seu sistema SAP em parte de potenciais períodos de inatividade. Para forçar o Azure para implementar as VMs de um sistema SAP espalhados em diferentes domínios de atualização, terá de definir um atributo específico no momento da implementação de cada VM. Assim como os domínios de falha, uma unidade de escala do Azure está dividida em vários domínios de atualização. Para direcionar o controlador de malha do Azure para implementar um conjunto de VMs através de diferentes domínios de atualização, terá de atribuir um conjunto de disponibilidade do Azure para as VMs no momento da implementação. Para obter mais informações sobre conjuntos de disponibilidade do Azure, consulte o capítulo [conjuntos de disponibilidade do Azure] [ planning-guide-3.2.3] abaixo.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Conjuntos de disponibilidade do Azure
Máquinas virtuais do Azure dentro de um conjunto de disponibilidade do Azure são distribuídas pelo controlador de malha do Azure através de diferentes domínios de atualização e de falha. O objetivo da distribuição através de diferentes domínios de atualização e de falha é impedir que todas as VMs de um sistema SAP a ser encerrado no caso de manutenção da infraestrutura ou de uma falha de dentro de um domínio de falha. Por predefinição, as VMs não fazem parte de um conjunto de disponibilidade. A participação de uma VM num conjunto de disponibilidade é definida no momento da implantação ou, mais tarde, por uma reconfiguração e a nova implementação de uma VM.

Para compreender o conceito de conjuntos de disponibilidade do Azure e a forma como os conjuntos de disponibilidade relacionadas com a falha e domínios de atualização, leia [neste artigo][virtual-machines-manage-availability]

Para definir conjuntos de disponibilidade para ARM por meio de um modelo json ver [as especificações de api de rest](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) e procure "availability".

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Armazenamento: Armazenamento do Microsoft Azure e discos de dados
As máquinas virtuais do Microsoft Azure utilizar tipos de armazenamento diferentes. Ao implementar o SAP nos serviços de Máquina Virtual do Azure, é importante compreender as diferenças entre estes dois tipos principais de armazenamento:

* Armazenamento não persistente e voláteis.
* Armazenamento persistente.

O armazenamento não persistentes é ligado diretamente a máquinas virtuais em execução e reside em nós de computação, o armazenamento de instância local (armazenamento temporário). O tamanho depende do tamanho da Máquina Virtual escolhida quando a implementação iniciada. Este tipo de armazenamento é volátil e, portanto, o disco é inicializado quando uma instância de Máquina Virtual é reiniciada. Normalmente, o ficheiro de paginação para o sistema operativo está localizado neste disco temporário.

- - -
> ![Windows][Logo_Windows] Windows
>
> Em VMs do Windows, a unidade temporária está montada como unidade D:\ numa VM implementada.
>
> ![Linux][Logo_Linux] Linux
>
> Em VMs do Linux, este está montado como /mnt/resource ou /mnt. Ver mais detalhes aqui:
>
> * [Como anexar um disco de dados a uma Máquina Virtual do Linux][virtual-machines-linux-how-to-attach-disk]
> * <https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux#temporary-disk>
>
>

- - -
A unidade real é volátil, porque esta está a obter armazenada no próprio servidor de anfitrião. Se a VM é movido numa reimplantação (por exemplo, devido a manutenção no anfitrião ou encerramento e reinício) o conteúdo da unidade é perdido. Por conseguinte, não é uma opção para armazenar os dados importantes nesta unidade. O tipo de suporte de dados utilizado para este tipo de armazenamento é diferente entre séries VM diferentes com características de desempenho muito diferentes que, a partir de Junho de 2015, o seguinte aspeto:

* A5 a A7: Desempenho muito limitado. Não é recomendado para qualquer coisa além do ficheiro de paginação
* A8-A11: Características de desempenho muito bom, com algumas IOPS de dez mil e > débito de 1GB/seg.
* Série de D: Características de desempenho muito bom com alguns, em seguida, milhares de IOPS e > débito de 1GB/seg.
* Séries de DS: Características de desempenho muito bom, com algumas IOPS de dez mil e > débito de 1GB/seg.
* Série G: Características de desempenho muito bom, com algumas IOPS de dez mil e > débito de 1GB/seg.
* Série GS: Características de desempenho muito bom, com algumas IOPS de dez mil e > débito de 1GB/seg.

Instruções acima são válidos para os tipos VM com certificação SAP. A série de VM com o excelente IOPS e débito se qualificam para tirar partido por alguns recursos do DBMS. Para obter mais informações, consulte a [guia de implementação de DBMS][dbms-guide].

Armazenamento do Microsoft Azure fornece armazenamento persistente e os níveis de típicos de proteção e redundância visto no armazenamento de SAN. Discos com base no armazenamento do Azure são o disco rígido virtual (VHDs) localizado nos serviços de armazenamento do Azure. O disco de SO local (c do Windows:\, Linux/desenvolvimento/sda1) são armazenados no armazenamento do Azure, e Volumes/discos adicionais montados para a VM são armazenados Ali, demasiado.

É possível carregar um VHD existente no local ou criar segmentos vazios de dentro do Azure e anexe-os para as VMs implementadas.

Depois de criar ou carregar um VHD para o armazenamento do Azure, é possível montar e ligá-las para uma Máquina Virtual existente e para copiar o VHD (desmontado) existente.

Como os VHDs são mantidas, dados e as alterações nas mesmas são seguras quando reiniciar o computador e recriar uma instância de Máquina Virtual. Mesmo que uma instância for eliminada, estes VHDs e mantenha-se seguro podem ser implementado novamente ou em caso de discos não SO podem ser montadas às outras VMs.

Dentro da rede de armazenamento do Azure podem ser configuradas níveis de redundância diferentes:

* Nível mínimo que pode ser selecionado é *redundância local*, que é equivalente a três-réplica dos dados no mesmo centro de dados de uma região do Azure (consulte o capítulo [regiões do Azure] [ planning-guide-3.1]).
* Armazenamento com redundância de zona, o que se propaga três imagens através de diferentes dados centra-se na mesma região do Azure.
* Nível de redundância de padrão é a redundância geográfica, o que replica assincronamente o conteúdo em outro três imagens dos dados para outra região do Azure, que está alojado na mesma região geopolítica.

Consulte também a tabela na parte superior deste artigo sobre as opções de redundância diferentes: <https://azure.microsoft.com/pricing/details/storage/>

Obter mais informações sobre o armazenamento do Azure podem ser encontradas aqui:

* <https://azure.microsoft.com/documentation/services/storage/>
* <https://azure.microsoft.com/services/site-recovery>
* <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>
* <https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview.aspx>

#### <a name="azure-standard-storage"></a>Armazenamento Standard do Azure
Armazenamento Standard do Azure era do tipo de armazenamento disponível quando o IaaS do Azure foi lançado. Foram impostas por disco único de quotas IOPS. Latência registada não estava na mesma classe como dispositivos SAN/NAS normalmente implementados para sistemas SAP high-end alojado no local. No entanto, o armazenamento padrão do Azure se mostrou suficiente para centenas de muitos sistemas SAP enquanto isso é implementados no Azure.

Os discos que estão armazenados em contas de armazenamento Standard do Azure são cobrados os dados reais que são armazenados, o volume de transações de armazenamento, transferências de dados de saída e opção de redundância escolhida. Muitos discos podem ser criados no máximo 1TB de tamanho, mas, desde que estas permanecem vazias não existe nenhum custo associado. Se, em seguida, preencher um VHD com 100GB, é cobrado para 100GB de armazenamento e não para o VHD foi criado com o tamanho do nominal.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Armazenamento Premium do Azure
Em Abril de 2015, a Microsoft introduziu o armazenamento Premium do Azure. O armazenamento Premium tem introduzido com o objetivo de fornecer:

* Latência de e/s melhor.
* Taxa de transferência melhor.
* Menos variação na latência de e/s.

Para essa finalidade, muitas alterações introduzidas-se de que as duas mais importantes são:

* Utilização de discos SSD em nós de armazenamento do Azure
* Uma nova cache de leitura que é sustentada pelo SSD local de um nó de computação do Azure

Em diferentes dos padrões convencionais onde não foram alterado capacidades de armazenamento Standard depende do tamanho do disco (ou VHD), o armazenamento Premium atualmente tem três categorias de disco diferente, que são apresentadas neste artigo: <https://azure.microsoft.com/pricing/details/storage/unmanaged-disks/>

Verá que o disco e IOPS/disco débito/disco são dependentes na categoria do tamanho dos discos

Custo base no caso do armazenamento Premium não é o volume de dados reais armazenado nesses discos, mas a categoria de tamanho de tal um disco, independentemente da quantidade de dados que são armazenados no disco.

Também pode criar discos no armazenamento Premium que não são diretamente mapeamento para as categorias de tamanho mostradas. Isto pode ser o caso, especialmente quando copiar os discos de armazenamento Standard para o armazenamento Premium. Nesses casos, é executado um mapeamento para a próxima opção de disco de armazenamento Premium maior.

Lembre-se de que apenas determinadas séries VM podem se beneficiar do Premium Storage do Azure. A partir de Dezembro de 2015, estes são DS - e -série GS. A série DS é basicamente o mesmo que a série D com a exceção que série DS, tem a capacidade de armazenamento Premium de montagem com base em VMs além para discos que estão alojados no armazenamento padrão do Azure. Mesmo é válido para a série G em comparação comparada a série GS.

Se estiver dar uma olhada a parte das VMs de série DS no [deste artigo (Linux)] [ virtual-machines-sizes-linux] e [neste artigo (Windows)][virtual-machines-sizes-windows], percebe que Existem limitações de volume de dados para discos de armazenamento Premium na granularidade do nível da VM. Diferentes de séries de DS ou VMs da série GS também têm limitações de diferentes no que respeita ao número de discos de dados que podem ser montadas. Estes limites são documentados no artigo mencionado acima, também. Mas, essencialmente significa que, se, por exemplo, montar 32 x P30 discos para uma única VM DS14 não pode obter 32 x o débito máximo de um disco de P30. Em vez disso, o débito máximo no nível VM, conforme documentado no artigo limita a taxa de transferência de dados.

Obter mais informações sobre o armazenamento Premium podem ser encontradas aqui: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="c55b2c6e-3ca1-4476-be16-16c81927550f"></a>Discos geridos
Discos geridos são um novo tipo de recurso no Azure Resource Manager que pode ser utilizado em vez de VHDs que são armazenadas em contas de armazenamento do Azure. Discos geridos automaticamente se alinham com o conjunto de disponibilidade da máquina virtual que estão ligados a e, portanto, aumentam a disponibilidade da sua máquina virtual e os serviços que estão em execução na máquina virtual. Para obter mais informações, leia os [artigo de descrição geral](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

Recomendamos que utilize o disco gerido, uma vez que simplificam a implementação e gestão das suas máquinas virtuais.
Atualmente o SAP suporta apenas os Managed Disks Premium. Para obter mais informações, leia a nota SAP [1928533].

#### <a name="azure-storage-accounts"></a>Contas de Armazenamento do Azure
Quando implementar VMs no Azure ou de serviços, a implementação de VHDs e imagens de VM pode ser organizada em unidades denominadas contas de armazenamento do Azure. Quando planear a implementação do Azure, terá de considerar cuidadosamente as restrições do Azure. Do lado, há um número limitado de contas de armazenamento por subscrição do Azure. Embora cada conta de armazenamento do Azure pode conter um grande número de ficheiros VHD, há um limite fixo sobre o IOPS total por conta de armazenamento. Ao implementar centenas de VMs de SAP com os sistemas DBMS, criação de chamadas de e/s significativas, recomenda-se para distribuir as VMs da alta DBMS de IOPS entre várias contas de armazenamento do Azure. Deve ter cuidado para não exceder o limite atual de contas de armazenamento do Azure por subscrição. Como o armazenamento é essencial para a implementação de base de dados para um sistema SAP, esse conceito é abordado em mais detalhes no já referenciado [guia de implementação de DBMS][dbms-guide].

Podem encontrar mais informações sobre as contas de armazenamento do Azure no [este artigo][storage-scalability-targets]. Ler este artigo, percebe que há diferenças nas limitações entre as contas de armazenamento Standard do Azure e contas de armazenamento Premium. Principais diferenças são o volume de dados que podem ser armazenados dentro de uma conta de armazenamento. No armazenamento Standard, o volume é uma magnitude maior do que com o armazenamento Premium. No outro lado, a conta de armazenamento padrão é extremamente limitada em ESP (consulte a coluna **taxa Total de pedidos de**), ao passo que a conta de armazenamento Premium do Azure tem sem essa limitação. Discutiremos detalhes e os resultados dessas diferenças ao discutir as implementações de sistemas SAP, especialmente os servidores do DBMS.

Dentro de uma conta de armazenamento, tem a possibilidade de criar diferentes contentores com o objetivo de organizar e categorizar os VHDs de diferentes. Estes contentores são normalmente utilizadas para, por exemplo, separados VHDs de diferentes VMs. Não há nenhum implicações de desempenho usando apenas um contentor ou de vários contentores por baixo de uma única conta de armazenamento do Azure.

No Azure um nome VHD segue-se a seguinte ligação de nomenclatura que tem de fornecer um nome exclusivo para o VHD no Azure:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Como mencionado, a cadeia acima tem de identificar exclusivamente o VHD que é armazenado no armazenamento do Azure.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Redes do Microsoft Azure
O Microsoft Azure fornece uma infraestrutura de rede, que permite o mapeamento de todos os cenários, o que queremos para concretizar com SAP software. Os recursos são:

* Acesso a partir de fora, diretamente para as VMs através de ssh/VNC ou de serviços de Terminal do Windows
* Acesso a serviços e portas específicas utilizadas por aplicações nas VMs
* Comunicação interna e a resolução de nomes entre um grupo de VMs implementadas como VMs do Azure
* Conectividade em vários locais entre a rede no local de um cliente e a rede do Azure
* Cruzar a região do Azure ou de conectividade do Centro de dados entre sites do Azure

Pode encontrar mais informações aqui: <https://azure.microsoft.com/documentation/services/virtual-network/>

Há muitas possibilidades diferentes para configurar o nome e a resolução IP no Azure. Neste documento, cenários apenas na Cloud contam com a predefinição da utilização do DNS do Azure (ao contrário de definir um serviço DNS próprio). Também é um novo serviço de DNS do Azure, que pode ser utilizado em vez de definir seu próprio servidor DNS. Podem encontrar mais informações em [este artigo] [ virtual-networks-manage-dns-in-vnet] e, na [esta página](https://azure.microsoft.com/services/dns/).

Para cenários em vários locais, podemos da entidade confiadora no fato de que no local foi estendido via VPN ou ligação privada AD/OpenLDAP/DNS para o Azure. Para determinados cenários, conforme documentado aqui, poderá ser necessário ter uma réplica do AD/OpenLDAP instalada no Azure.

Porque o sistema de rede e resolução de nomes é essencial para a implementação de base de dados para um sistema SAP, esse conceito é abordado em mais detalhes a [guia de implementação de DBMS][dbms-guide].

##### <a name="azure-virtual-networks"></a>Redes Virtuais do Azure
Ao criar a rede Virtual do Azure, pode definir o intervalo de endereços dos endereços IP privados alocada por uma funcionalidade do DHCP do Azure. Em cenários em vários locais, o intervalo de endereços IP definido ainda está alocado a utilizar o DHCP pelo Azure. No entanto, a resolução de nomes de domínio é feita no local (partindo do princípio de que as VMs são uma parte de um domínio no local) e, por conseguinte, pode resolver os endereços para além dos serviços de Cloud do Azure diferente.

Todas as máquinas virtuais no Azure precisa de estar ligado a uma rede Virtual.

Podem encontrar mais detalhes no [este artigo] [ resource-groups-networking] e, na [esta página](https://azure.microsoft.com/documentation/services/virtual-network/).

[comment]: <> (Não foi possível de lista de tarefas de MShermannd encontrar um artigo que inclui o tópico de OpenLDAP + ARM; )
[comment]: <> (MSSedusch <https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [!NOTE]
> Por predefinição, depois de implementada uma VM não é possível alterar a configuração de rede Virtual. As definições de TCP/IP tem de ser deixadas para o servidor DHCP do Azure. O comportamento padrão é a atribuição de IP dinâmico.
>
>

O endereço MAC da placa de rede virtual pode alterar, por exemplo depois de redimensionamento e o convidado Windows ou Linux, sistema operacional seleciona a nova placa de rede e utiliza automaticamente o DHCP para atribuir os endereços IP e DNS neste caso.

##### <a name="static-ip-assignment"></a>Atribuição de IP estático
É possível atribuir endereços IP reservados ou fixos para as VMs dentro de uma rede Virtual do Azure. Executar as VMs numa rede Virtual do Azure, é aberta uma grande possibilidade aproveitar esta funcionalidade se necessárias ou obrigatórias para alguns cenários. A atribuição de IP permanece válida em toda a existência da VM, independentemente de se a VM está em execução ou encerramento. Como resultado, precisa tomar o número geral de VMs (VMS em execução e paradas) em conta ao definir o intervalo de endereços IP para a rede Virtual. O endereço IP permanece atribuído até que a VM e sua Interface de rede seja eliminado ou até que o endereço IP anular seja atribuído a novamente. Para obter mais informações, leia [este artigo][virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>Várias NICs por VM
Pode definir várias placas de interface de rede virtual (vNIC) para uma Máquina Virtual. Com a capacidade de ter múltiplos vNICs que pode começar a configurar o tráfego de rede separação onde, por exemplo, o tráfego de cliente é encaminhado através de um vNIC e back-end o tráfego é encaminhada através de um segundo vNIC. Dependentes do tipo de VM são diferentes limitações no que respeita ao número de vNICs. Obter detalhes exatos, funcionalidade e as restrições podem ser encontradas nestes artigos:

* [Criar uma VM do Windows com vários NICs][virtual-networks-multiple-nics-windows]
* [Criar uma VM do Linux com vários NICs][virtual-networks-multiple-nics-linux]
* [Implementar VMs de NIC de várias através de um modelo][virtual-network-deploy-multinic-arm-template]
* [Implementar várias VMs de NIC com o PowerShell][virtual-network-deploy-multinic-arm-ps]
* [Implementar as VMs de NIC de várias com a CLI do Azure][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Conetividade site a Site
Em vários locais é que as VMs do Azure e no local ligado com uma ligação de VPN transparente e permanente. Espera-se para se tornar o padrão mais comum para a implementação de SAP no Azure. A pressuposição é de que processos com instâncias SAP no Azure e de procedimentos operacionais devem trabalhar transparentemente. Este significa que deve ser capaz de imprimir fora desses sistemas, bem como a utilização de sistema de gerenciamento de transporte (TMS) o SAP para o transporte é alterado de um sistema de desenvolvimento no Azure para um sistema de teste, o que é implementado no local. Mais documentação em torno de site a site pode ser encontrada na [neste artigo][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>Dispositivo de túnel VPN
Para criar uma ligação site a site (Centro de dados no local para o Centro de dados do Azure), terá de obter e configurar um dispositivo VPN ou utilizar o encaminhamento e acesso remoto (RRAS) que foi introduzida como um componente de software com o Windows Server 2012.

* [Criar uma rede virtual com uma ligação de VPN de site a site com o PowerShell][vpn-gateway-create-site-to-site-rm-powershell]
* [Acerca dos dispositivos VPN para ligações de Gateway de VPN de Site a Site][vpn-gateway-about-vpn-devices]
* [FAQ do Gateway de VPN][vpn-gateway-vpn-faq]

![Ligação de site a site entre no local e o Azure][planning-guide-figure-600]

A figura acima mostra duas subscrições do Azure têm subintervalos de endereço IP reservado para utilização em redes virtuais no Azure. A conectividade da rede no local para o Azure é estabelecida através de VPN.

#### <a name="point-to-site-vpn"></a>VPN ponto a Site
VPN Point-to-site requer que todas as máquinas de cliente para estabelecer ligação com o seu próprio VPN para o Azure. Para os cenários SAP que está a visualizar, conectividade de ponto a site não é prática. Por conseguinte, sem referências adicionais são fornecidas para a conectividade VPN ponto a site.

Podem encontrar mais informações aqui
* [Configurar uma ligação de Ponto a Site a uma VNet com o portal do Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Configurar uma ligação de Ponto a Site a uma VNet com o PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

#### <a name="multi-site-vpn"></a>VPN multilocal
Também hoje em dia, o Azure oferece a possibilidade de criar a conectividade de VPN multilocal para uma subscrição do Azure. Anteriormente uma única subscrição foi limitada a uma ligação de VPN de site a site. Esta limitação desapareceram com ligações de VPN multilocal para uma única subscrição. Isto torna possível tirar partido de mais do que uma região do Azure para uma subscrição específica por meio de configurações em vários locais.

Para obter mais documentação, consulte [neste artigo][vpn-gateway-create-site-to-site-rm-powershell]

[comment]: <> (MShermannd TODO foi encontrado nenhum link de docu ARM)

#### <a name="vnet-to-vnet-connection"></a>Ligação VNet a VNet
Através da VPN multilocal, tem de configurar uma rede Virtual do Azure separada em cada uma das regiões. No entanto com muita frequência tem o requisito de que os componentes de software em diferentes regiões devem comunicar entre si. O ideal é que esta comunicação não deve ser encaminhada a partir de uma região do Azure para o local e a partir daí para outra região do Azure. Para o atalho, o Azure oferece a possibilidade de configurar uma ligação de uma rede Virtual do Azure numa região a outra rede Virtual do Azure alojada noutra região. Esta funcionalidade é chamada de ligação de VNet a VNet. Obter mais detalhes sobre esta funcionalidade podem ser encontradas aqui: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-to-azure-expressroute"></a>Ligação privada com o Azure ExpressRoute
O Microsoft Azure ExpressRoute permite a criação de ligações privadas entre datacenters do Azure e infraestrutura no local do cliente ou num ambiente de colocalização. ExpressRoute é oferecido por MPLS vários fornecedores de VPN (pacote de comutadores) ou de outros fornecedores de serviços de rede. As ligações do ExpressRoute não passam para a Internet pública. As ligações ExpressRoute oferecem maior segurança, mais fiabilidade através de vários circuitos paralelos, velocidades mais rápidas e latências mais baixas que as ligações normais pela Internet.

Encontre mais detalhes sobre o Azure ExpressRoute e ofertas aqui:

* <https://azure.microsoft.com/documentation/services/expressroute/>
* <https://azure.microsoft.com/pricing/details/expressroute/>
* <https://azure.microsoft.com/documentation/articles/expressroute-faqs/>

Expressroute permite várias subscrições do Azure através de um circuito do ExpressRoute, conforme documentado aqui

* <https://azure.microsoft.com/documentation/articles/expressroute-howto-linkvnet-arm/>
* <https://azure.microsoft.com/documentation/articles/expressroute-howto-circuit-arm/>

#### <a name="forced-tunneling-in-case-of-cross-premises"></a>Imposição de túnel em caso de em vários locais
Para VMs a associar domínios no local através do site a site, ponto a site ou ExpressRoute, terá de certificar-se de que as definições de proxy de Internet são introdução implementadas para todos os utilizadores também essas VMs. Por predefinição, o software em execução nas VMs ou os utilizadores a utilizar um browser para aceder à internet não iria através do proxy da empresa, mas deve ligar diretamente através do Azure para a internet. Mas, até mesmo a definição de proxy não é uma solução de 100% para direcionar o tráfego através do proxy da empresa, uma vez que é responsabilidade do software e serviços para procurar o proxy. Se não está a ser fazendo que software em execução na VM ou um administrador manipula as definições, o tráfego para a Internet pode ser detoured novamente diretamente através do Azure para a Internet.

Para evitar isso, pode configurar o túnel forçado com conectividade de site a site entre no local e o Azure. A descrição detalhada da funcionalidade de túnel forçado é publicada aqui <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

Túnel forçado com o ExpressRoute está ativado por clientes anunciar uma rota predefinida por meio de sessões de peering de BGP de ExpressRoute.

#### <a name="summary-of-azure-networking"></a>Resumo da rede do Azure
Este capítulo continha muitos pontos importantes sobre o funcionamento em rede do Azure. Aqui está um resumo dos objetivos principais:

* Redes virtuais do Azure permite configurar a rede, de acordo com suas necessidades
* Redes virtuais do Azure podem ser utilizados para atribuir intervalos de endereços IP a VMs ou atribuir endereços IP fixos para VMs
* Para configurar uma ligação Site a Site ou ponto a Site tem de criar primeiro uma rede Virtual do Azure
* Assim que tiver sido implementada uma máquina virtual, já não é possível alterar a rede Virtual atribuído à VM

### <a name="quotas-in-azure-virtual-machine-services"></a>Quotas nos serviços de Máquina Virtual do Azure
É necessário ser claro sobre o fato de que a infraestrutura de rede e de armazenamento é partilhada entre VMs em execução uma variedade de serviços na infraestrutura do Azure. E, assim como nos centros de dados do cliente, aprovisionamento excessivo de alguns dos recursos de infraestrutura ocorrem num certo grau. A plataforma do Microsoft Azure utiliza o disco, CPU, rede e outras quotas para limitar o consumo de recursos e preservar o desempenho consistente e determinístico.  Os diferentes tipos VM (A5, A6, etc.) tem quotas diferentes para o número de discos, CPU, RAM e de rede.

> [!NOTE]
> Recursos de CPU e memória dos tipos VM suportados pelo SAP são pré-alocada em nós de host. Isso significa que, quando a VM estiver implementada, os recursos no anfitrião estão disponíveis, conforme definido pelo tipo de VM.
>
>

Ao planear e dimensionamento SAP em soluções do Azure as quotas para cada tamanho de máquina virtual tem de ser consideradas. As quotas VM são descritas [aqui (Linux)] [ virtual-machines-sizes-linux] e [aqui (Windows)][virtual-machines-sizes-windows].

As quotas descritas representam os valores máximos teóricos.  O limite de IOPS por disco pode ser obtido com o IOs pequeno (8kb), mas possivelmente não pode ser obtido com o IOs grandes (1Mb).  O limite de IOPS é aplicado a granularidade do único disco.

Como uma árvore de decisão estimada para decidir se um sistema SAP se encaixa no Serviços de Máquina Virtual do Azure e seus recursos, ou se um sistema existente tem de ser configuradas de forma diferente para implementar o sistema no Azure, pode ser usada da árvore de decisão abaixo:

![Árvore de decisões para decidir qual a capacidade de implementar o SAP no Azure][planning-guide-figure-700]

**Passo 1**: as informações mais importantes para começar são o requisito de SAPS para um determinado sistema SAP. Os requisitos de SAPS precisam ser separado no DBMS e parte de aplicação SAP, mesmo que o sistema SAP já está a ser implementado no local numa configuração de camada 2. Para os sistemas existentes, SAPS relacionados com o hardware em utilização, muitas vezes, podem ser determinados ou estimadas com base em parâmetros de comparação SAP existentes. Os resultados podem ser encontrados aqui: <https://sap.com/about/benchmark.html>.
Para sistemas SAP recentemente implementados, deve passar por um exercício de dimensionamento, o que deve determinar os requisitos de SAPS do sistema.
Consulte também este blogue e o documento anexado para o dimensionamento de SAP no Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**Passo 2**: para sistemas existentes, o volume de e/s e operações de e/s por segundo no servidor do DBMS devem ser medidas. Para sistemas recentemente planeados, o exercício de dimensionamento para o novo sistema também deve dar idéias aproximadas dos requisitos de e/s no lado do DBMS. Se não souber, eventualmente, terá de realizar uma prova de conceito.

**Passo 3**: comparar o requisito de SAPS para o servidor do DBMS com SAPS podem fornecer os diferentes tipos VM do Azure. As informações sobre SAPS os diferentes tipos de VM do Azure estão documentadas na nota SAP [1928533]. O foco deve ser em primeiro lugar na DBMS VM, uma vez que a camada de base de dados é a camada num sistema SAP NetWeaver que, na maioria das implementações, não aumentar horizontalmente. Por outro lado, é possível ampliar a camada de aplicação SAP. Se nenhuma do SAP suportados tipos de VM do Azure podem fornecer os SAPS necessárias, a carga de trabalho do sistema SAP planeada não pode ser executada no Azure. Que seja precisa para implementar o sistema no local ou tiver de alterar o volume de carga de trabalho para o sistema.

**Passo 4**: conforme documentado [aqui (Linux)] [ virtual-machines-sizes-linux] e [aqui (Windows)][virtual-machines-sizes-windows], Azure impõe uma quota IOPS por disco independentemente se utiliza o armazenamento Standard ou o armazenamento Premium. Depende do tipo VM, o número de discos de dados, que podem ser montadas varia. Como resultado, pode calcular um número IOPS máximo que pode ser alcançado com cada um dos diferentes tipos VM. Depende do esquema do ficheiro de base de dados, pode da faixa de discos para se tornar um volume no SO convidado. No entanto, se o volume IOPS atual de um sistema SAP implantado exceder os limites calculados do maior tipo VM do Azure e se não há nenhuma chance para compensar com mais memória, a carga de trabalho do sistema SAP pode gravemente afetada. Nesses casos, pode usar a um ponto em que não deve implementar o sistema no Azure.

**Passo 5**: especialmente em sistemas SAP, que são implementado no local em configurações de 2 camadas, as chances são que o sistema poderá ter de ser configurada no Azure numa configuração de camada 3. Neste passo, terá de verificar se há um componente na camada de aplicação SAP, que não pode ser ampliado e que não se encaixa nos recursos de CPU e memória que oferecem os diferentes tipos de VM do Azure. Se existir, de fato, esse componente, o sistema SAP e a sua carga de trabalho não podem ser implementados no Azure. Mas se pode aumentar horizontalmente os componentes de aplicações SAP em várias VMs do Azure, o sistema pode ser implementado no Azure.

**Passo 6**: se os componentes de camada de aplicativo DBMS e SAP podem ser executados em VMs do Azure, a configuração tem de ser definido com para:

* Número de VMs do Azure
* Tipos VM para os componentes individuais
* Número de VHDs na VM do DBMS para fornecer suficiente IOPS

## <a name="managing-azure-assets"></a>Gerir recursos do Azure
### <a name="azure-portal"></a>Portal do Azure
O portal do Azure é um dos três interfaces para gerir implementações de VM do Azure. As tarefas de gestão básicas, como implementar VMs a partir de imagens, podem ser feitas através do portal do Azure. Além disso, a criação de contas de armazenamento, redes virtuais e outros componentes do Azure também são tarefas que o portal do Azure pode processar muito bem. No entanto, a funcionalidade, como carregar o VHD no local para o Azure ou copiar um VHD no Azure são tarefas, que requerem as ferramentas de terceiros ou administração através do PowerShell ou a CLI.

![Portal do Microsoft Azure - descrição geral de Máquina Virtual][planning-guide-figure-800]

[comment]: <> (MSSedusch * <https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[comment]: <> (MSSedusch * <https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Tarefas de administração e configuração para a instância de Máquina Virtual são possíveis a partir do portal do Azure.

Além de reiniciar e encerrar uma Máquina Virtual também pode anexar, desligar e criar os discos de dados para a instância de Máquina Virtual, para capturar a instância para preparação de imagem e configurar o tamanho da instância de Máquina Virtual.

O portal do Azure fornece funcionalidades básicas para implementar e configurar as VMs e muitos outros serviços do Azure. No entanto não todas as funcionalidade disponível é abordada pelo portal do Azure. No portal do Azure, não é possível realizar tarefas como:

* A carregar o VHD para o Azure
* Copiar VMs

[comment]: <> (MShermannd TODO o que sobre a automatização de serviço para VMs de SAP? )
[comment]: <> (Implementação de MSSedusch do SO de várias VMs ao mesmo tempo possível)
[comment]: <> (MSSedusch também qualquer tipo de automação sobre implementação não é possível com o portal do Azure. Tarefas como a implementação de várias VMs não é possível através do portal do Azure.)

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Gestão através de cmdlets do PowerShell do Microsoft Azure
Windows PowerShell é uma estrutura poderosa e extensível que foi amplamente adotada pelos clientes que implementam o maior número de sistemas no Azure. Após a instalação de cmdlets do PowerShell num desktop, laptop ou uma estação de gerenciamento dedicado, os cmdlets do PowerShell podem ser executados remotamente.

O processo para ativar o ambiente de trabalho/portátil local para a utilização de cmdlets do PowerShell do Azure e como configurar os para utilização com as subscrições do Azure é descrita em [este artigo][powershell-install-configure].

Obter passos sobre como instalar, atualizar e configurar o Azure PowerShell cmdlets também podem ser encontrados em mais detalhadas [neste capítulo do guia de implementação][deployment-guide-4.1].

Experiência do cliente até aqui foi que o PowerShell (PS), certamente, é a ferramenta mais potente para implementar VMs e criar passos personalizados na implementação de VMs. Todos os clientes a executar instâncias do SAP no Azure estão a utilizar os cmdlets de PS para complementar as tarefas de gestão que fazer no portal do Azure ou até mesmo estiver a utilizar os cmdlets de PS exclusivamente para gerir as respetivas implementações no Azure. Como os cmdlets específicos do Azure compartilham a mesma Convenção de nomenclatura, como os cmdlets relacionados com o Windows mais de 2000, é uma tarefa fácil para os administradores do Windows aproveitar esses cmdlets.

Veja o exemplo aqui: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[comment]: <> (Lista de tarefas MShermannd descrevem novo comando da CLI quando testado )
Implementação da extensão de monitorização do Azure para SAP (consulte o capítulo [solução de monitorização do Azure para SAP] [ planning-guide-9.1] neste documento) só é possível através do PowerShell ou CLI. Por isso é obrigatório para definir e configurar o PowerShell ou a CLI ao implementar ou administrar um sistema SAP NetWeaver no Azure.  

Como o Azure fornece mais funcionalidade, novos cmdlets de PS vai ser adicionado requer uma atualização dos cmdlets. Por isso faz sentido para verificar o site de Download do Azure, pelo menos, uma vez, o mês <https://azure.microsoft.com/downloads/> para uma nova versão dos cmdlets. A nova versão é instalada sobre a versão mais antiga.

Para obter uma lista geral do relacionados com o Azure PowerShell comandos verifique aqui: <https://docs.microsoft.com/powershell/azure/overview>.

### <a name="management-via-microsoft-azure-cli-commands"></a>Gestão através dos comandos da CLI do Microsoft Azure
Para os clientes que utilizam o Linux e pretendem gerir o Azure Powershell de recursos podem não ser uma opção. A Microsoft oferece o CLI do Azure como uma alternativa.
A CLI do Azure fornece um conjunto de código-fonte aberto, comandos de várias plataformas para trabalhar com a plataforma do Azure. A CLI do Azure proporciona a mesma funcionalidade encontrada no portal do Azure.

Para obter informações sobre instalação, configuração e como utilizar a CLI Consulte comandos para executar tarefas do Azure

* [Instalar a CLI do Azure][xplat-cli]
* [Implementar e gerir máquinas virtuais utilizando modelos Azure Resource Manager e a CLI do Azure] [./../linux/create-ssh-secured-vm-from-template.md]
* [Utilizar a CLI do Azure para Mac, Linux e Windows com o Azure Resource Manager][xplat-cli-azure-resource-manager]

Leia também o capítulo [CLI do Azure para VMs do Linux] [ deployment-guide-4.5.2] no [guia de implementação] [ planning-guide] sobre como utilizar a CLI do Azure para implementar a monitorização do Azure Extensão para SAP.

## <a name="different-ways-to-deploy-vms-for-sap-in-azure"></a>Diferentes formas de implementar as VMs para o SAP no Azure
Este capítulo, irá aprender as diferentes formas de implementar uma VM no Azure. Procedimentos de preparação adicional, bem como manipulação de VHDs e VMs no Azure está abrangidas neste capítulo.

### <a name="deployment-of-vms-for-sap"></a>Implementação de VMs para SAP
O Microsoft Azure oferece várias formas de implementar VMs e os discos associados. Portanto, é muito importante compreender as diferenças, uma vez que as preparações das VMs poderão diferir consoante o método de implementação. Em geral, vamos dar uma olhada nos seguintes cenários:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Mover uma VM no local para o Azure com um disco não generalizada
Planeia mover um sistema específico do SAP no local para o Azure. Isso pode ser feito ao carregar o VHD, que contém o sistema operacional, os binários do SAP e os binários do DBMS e ainda os VHDs com os arquivos de dados e de registo do DBMS no Azure. Em comparação com [cenário #2 abaixo][planning-guide-5.1.2], mantenha o nome de anfitrião, o SID do SAP, e contas de usuário SAP na VM do Azure conforme foram configurados no ambiente no local. Por conseguinte, a generalizar a imagem não é necessária. Consulte os capítulos [preparação para mover uma VM no local para o Azure com um disco não generalizada] [ planning-guide-5.2.1] deste documento para obter passos de preparação de no local e o carregamento de VMs não generalizada ou VHDs para o Azure. Capítulo de leitura [cenário 3: mover uma VM no local com um VHD de Azure não generalizado com o SAP] [ deployment-guide-3.4] no [guia de implementação] [ deployment-guide] para passos detalhados de implementação tal uma imagem no Azure.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Implementar uma VM com uma imagem de específicas do cliente
Devido a requisitos de patch específico da sua versão de SO ou DBMS, as imagens fornecidas no Azure Marketplace não podem satisfazer suas necessidades. Por conseguinte, poderá ter de criar uma VM com a sua própria imagem de VM de SO/DBMS privada, o que pode ser implementada posteriormente várias vezes. Para preparar uma imagem privada para duplicação, os seguintes itens têm de ser considerados:

- - -
> ![Windows][Logo_Windows] Windows
>
> Ver mais detalhes aqui: <https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed> definições do Windows (como o SID do Windows e o nome de anfitrião) tem de ser abstraídos/generalizada na VM no local através do comando sysprep.
>
>
> ![Linux][Logo_Linux] Linux
>
> Siga os passos descritos nestes artigos para [SUSE][virtual-machines-linux-create-upload-vhd-suse], [Red Hat][virtual-machines-linux-redhat-create-upload-vhd], ou [Oracle Linux] [ virtual-machines-linux-create-upload-vhd-oracle], para preparar um VHD para ser carregado para o Azure.
>
>

- - -
Se já tiver instalado o conteúdo SAP na sua VM no local (especialmente para sistemas de camada 2), pode adaptar as definições do sistema SAP após a implementação da VM do Azure por meio da instância mudar o nome do procedimento suportado pelo SAP Software aprovisionamento Manager (SAP Tenha em atenção [1619720]). Consulte os capítulos [preparação para implementar uma VM com uma imagem de específicas do cliente para o SAP] [ planning-guide-5.2.2] e [carregar um VHD no local para o Azure] [ planning-guide-5.3.2]deste documento para obter passos de preparação de no local e o carregamento de uma VM generalizada no Azure. Capítulo de leitura [cenário 2: implementar uma VM com uma imagem personalizada para SAP] [ deployment-guide-3.3] no [guia de implementação] [ deployment-guide] para obter passos detalhados de implementação tal uma imagem no Azure.

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Implementar uma VM do Azure Marketplace
Gostaria de utilizar um Microsoft ou de terceiros fornecida a imagem de VM do Azure Marketplace para implementar a sua VM. Depois de implementada a VM no Azure, siga as mesmas diretrizes e ferramentas para instalar o SAP software e/ou DBMS dentro da sua VM, como faria num ambiente no local. Para obter mais descrição da implementação, consulte o capítulo [cenário 1: implementar uma VM do Azure Marketplace para SAP] [ deployment-guide-3.2] no [guia de implantação] [deployment-guide].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Preparar VMs SAP para o Azure
Antes de carregar VMs para o Azure tem de certificar-se de que as VMs e os VHDs satisfazem determinados requisitos. Existem pequenas diferenças consoante o método de implementação que é utilizado.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Preparação para mover uma VM no local para o Azure com um disco não generalizada
Um método de implementação comum é mover uma VM existente que é executado um sistema SAP no local para o Azure. Nessa VM e o sistema SAP na VM apenas devem ser executado no Azure com o mesmo nome de anfitrião e, muito provavelmente, o mesmo SID SAP. Neste caso, a sistema operacional de VM do convidado não deve ser generalizada para várias implementações. Se a rede no local foi expandida para o Azure (consulte o capítulo [em vários locais - implantação de uma única ou várias VMs de SAP no Azure com o requisito de que está a ser totalmente integrados à rede no local] [ planning-guide-2.2] neste documento), em seguida, até mesmo as mesmas contas de domínio podem ser utilizadas dentro da VM, como aqueles foram utilizados antes no local.

Requisitos ao preparar o seu próprio disco de VM do Azure são:

* Originalmente, o VHD que contém o sistema operacional pode ter um tamanho máximo de 127GB apenas. Esta limitação foi eliminada no final de Março de 2015. Agora o VHD que contém o sistema operativo pode ser até 1TB de tamanho como qualquer outro armazenamento do Azure alojada VHD também.
* Ele precisa estar no formato VHD fixo. Os VHDs ou VHDs no formato VHDx dinâmica ainda não são suportadas no Azure. VHDs dinâmicos serão convertidos em VHDs estáticos ao carregar o VHD com commandlets do PowerShell ou CLI
* VHDs que estão montados para a VM e devem ser montados novamente no Azure para a necessidade VM para estar num formato VHD fixo. Leia [deste artigo (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) e [este artigo (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) para limites de tamanho de discos de dados. VHDs dinâmicos serão convertidos em VHDs estáticos ao carregar o VHD com commandlets do PowerShell ou CLI
* Adicione outra conta local com privilégios de administrador que pode ser utilizada pelo suporte da Microsoft ou que podem ser atribuído como contexto para serviços e aplicações sejam executadas em até que a VM é implementada e os utilizadores mais adequados pode ser utilizada.
* Para o caso de utilizar um cenário de implementação apenas na Cloud (consulte o capítulo [apenas na Cloud - implementações de máquinas virtuais no Azure sem dependências na rede de cliente no local] [ planning-guide-2.1] deste documento) em a combinação com este método de implementação, as contas de domínio podem não funcionar quando o disco do Azure estiver implementado no Azure. Isso é especialmente verdadeiro para as contas que são utilizadas para executar serviços como os aplicativos DBMS ou SAP. Por isso terá de substituir essas contas de domínio com as contas locais de VM e eliminar as contas de domínio no local na VM. Manter os utilizadores de domínio no local na imagem VM não é um problema quando a VM é implementada no cenário em vários locais, conforme descrito no capítulo [em vários locais - implantação de uma única ou várias VMs de SAP no Azure com o requisito de ser totalmente integrado à rede no local] [ planning-guide-2.2] neste documento.
* Se as contas de domínio usadas como inícios de sessão do DBMS ou os utilizadores quando executar o sistema no local e essas VMs devem ser implementados em cenários de apenas na Cloud, os utilizadores de domínio têm de ser eliminadas. Tem de certificar-se de que o administrador local e ainda outro utilizador local de VM é adicionado como um início de sessão/utilizador para o DBMS como administradores.
* Adicione outras contas locais, como aqueles podem ser necessários para o cenário de implantação específicos.

- - -
> ![Windows][Logo_Windows] Windows
>
> Neste cenário, não generalização (sysprep) da VM é necessário para carregar e implementar a VM no Azure.
> Certifique-se de que essa unidade que d:\ não é utilizado.
> Definir automontar de disco para discos anexados conforme descrito no capítulo [definição automontar para discos anexados] [ planning-guide-5.5.3] neste documento.
>
> ![Linux][Logo_Linux] Linux
>
> Neste cenário sem generalização (waagent-desaprovisionamento) da VM, é necessário para carregar e implementar a VM no Azure.
> Certifique-se de que o recurso/mnt/não é utilizado e que todos os discos estão montados via uuid. Para o disco do SO, certifique-se de que a entrada do carregador de inicialização também reflete a montagem com base em uuid.
>
>

- - -
#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Preparação para implementar uma VM com uma imagem de específicas do cliente para o SAP
Ficheiros VHD que contêm um SO generalizado são armazenados em contentores em contas de armazenamento do Azure ou como imagens de disco gerido. Pode implementar uma nova VM a partir de uma imagem desse tipo referenciando a imagem VHD ou disco gerido como uma origem em seus arquivos de modelo de implementação, conforme descrito no capítulo [cenário 2: implementar uma VM com uma imagem personalizada para SAP] [ deployment-guide-3.3]do [guia de implementação][deployment-guide].

Requisitos ao preparar a sua própria imagem de VM do Azure são:

* Originalmente, o VHD que contém o sistema operacional pode ter um tamanho máximo de 127GB apenas. Esta limitação foi eliminada no final de Março de 2015. Agora o VHD que contém o sistema operativo pode ser até 1TB de tamanho como qualquer outro armazenamento do Azure alojada VHD também.
* Ele precisa estar no formato VHD fixo. Os VHDs ou VHDs no formato VHDx dinâmica ainda não são suportadas no Azure. VHDs dinâmicos serão convertidos em VHDs estáticos ao carregar o VHD com commandlets do PowerShell ou CLI
* VHDs que estão montados para a VM e devem ser montados novamente no Azure para a necessidade VM para estar num formato VHD fixo. Leia [deste artigo (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) e [este artigo (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) para limites de tamanho de discos de dados. VHDs dinâmicos serão convertidos em VHDs estáticos ao carregar o VHD com commandlets do PowerShell ou CLI
* Uma vez que todos os utilizadores de domínio registado os utilizadores na VM vão existir num cenário de apenas na Cloud (consulte o capítulo [apenas na Cloud - implementações de máquinas virtuais no Azure sem dependências na rede de cliente no local] [ planning-guide-2.1] deste documento), serviços a utilizar esse domínio contas poderão não funcionar depois da imagem é implementada no Azure. Isso é especialmente verdadeiro para as contas que são utilizadas para executar serviços como o DBMS ou SAP aplicações. Por isso terá de substituir essas contas de domínio com as contas locais de VM e eliminar as contas de domínio no local na VM. Manter os utilizadores de domínio no local na imagem VM poderão não ser um problema quando a VM é implementada no cenário em vários locais, conforme descrito no capítulo [em vários locais - implantação de uma única ou várias VMs de SAP no Azure com o requisito de que está a ser totalmente integrado na rede no local] [ planning-guide-2.2] neste documento.
* Adicione outra conta local com privilégios de administrador que pode ser utilizada pelo suporte da Microsoft em investigações de problema ou que podem ser atribuído como contexto para serviços e aplicações sejam executadas em até que a VM é implementada e usuários mais adequados pode ser utilizada.
* Em implementações apenas na Cloud e em que as contas de domínio foram usadas como inícios de sessão do DBMS ou os utilizadores ao executar o sistema no local, os utilizadores de domínio devem ser eliminados. Tem de certificar-se de que o administrador local e ainda outro utilizador local de VM é adicionado como um início de sessão do utilizador do DBMS como administradores.
* Adicione outras contas locais, como aqueles podem ser necessários para o cenário de implantação específicos.
* Se a imagem tiver uma instalação do SAP NetWeaver e mudar o nome do nome do anfitrião, o nome original no ponto da implementação do Azure, é provável que, é recomendado para copiar as versões mais recentes do DVD do Gestor de SAP de aprovisionamento de Software para o modelo. Isso permitirá que facilmente usar a funcionalidade de mudança de nome de SAP fornecido para adaptar o nome de anfitrião foi alterado e/ou alterar o SID do sistema SAP dentro da imagem VM implementada assim que uma nova cópia é iniciada.

- - -
> ![Windows][Logo_Windows] Windows
>
> Certifique-se de que unidade D:\ não é utilizada automontar de disco do conjunto para discos anexados, conforme descrito no capítulo [definição automontar para discos anexados] [ planning-guide-5.5.3] neste documento.
>
> ![Linux][Logo_Linux] Linux
>
> Certifique-se de que o recurso/mnt/não é utilizado e que todos os discos estão montados via uuid. Para o disco do SO, certificar-se de que a entrada do carregador de inicialização também reflete a montagem com base em uuid.
>
>

- - -
* GUI de SAP (para administrativo e fins de configuração) podem ser instalados previamente nesse modelo.
* Pode ser instalado outro software necessário para executar as VMs com êxito em cenários em vários locais, desde que este software pode trabalhar com a mudança de nome da VM.

Se a VM é preparada suficientemente ser genérico e, eventualmente, independente de contas/utilizadores não está disponíveis no cenário de implementação do Azure de destino, a última etapa de preparação de generalizar uma imagem desse tipo é realizada.

##### <a name="generalizing-a-vm"></a>Generalizar uma VM
- - -
> ![Windows][Logo_Windows] Windows
>
> A última etapa é iniciar sessão a uma VM com uma conta de administrador. Abra uma janela de comando do Windows como *administrador*. Vá para %windir%\windows\system32\sysprep e executar sysprep.exe.
> Será apresentada uma pequena janela. É importante verificar a **Generalize** opção (a predefinição é desmarcada) e altere a opção de encerrar seu padrão de 'Reinício' para 'shutdown'. Este procedimento parte do princípio de que o processo de sysprep é executado no local no SO convidado de uma VM.
> Se quiser executar o procedimento com uma VM já em execução no Azure, siga os passos descritos em [este artigo](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource).
>
> ![Linux][Logo_Linux] Linux
>
> [Como capturar uma máquina virtual do Linux para utilizar como um modelo do Resource Manager][capture-image-linux-step-2-create-vm-image]
>
>

- - -
### <a name="transferring-vms-and-vhds-between-on-premises-to-azure"></a>Transferência de VMs e VHDs entre no local para o Azure
Uma vez que o carregamento de imagens VM e os discos para o Azure não é possível através do portal do Azure, terá de utilizar cmdlets do Azure PowerShell ou a CLI. Outra possibilidade é a utilização da ferramenta 'AzCopy'. A ferramenta pode copiar VHDs entre no local e o Azure (em ambas as direções). Também pode copiar VHDs entre regiões do Azure. Consulte a [esta documentação] [ storage-use-azcopy] para download e a utilização do AzCopy.

Uma terceira alternativa seria usar várias ferramentas de terceiros de orientada a GUI. No entanto, certifique-se de que essas ferramentas encontram-se que os Blobs de página do Azure. Para nossos propósitos, precisamos usar o armazenamento de BLOBs de página do Azure (as diferenças são descritas aqui: <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>). Além disso, as ferramentas fornecidas pelo Azure são muito eficientes na compactação das VMs e os VHDs que precisam de ser carregados. Isso é importante porque essa eficiência na compactação reduz o tempo de carregamento (que varia de qualquer forma, consoante a ligação de carregamento para a internet das instalações no local e a região de implementação do Azure visada). É uma suposição justa que carregar uma VM ou VHD a partir do European localização para os dados do Azure baseadas nos E.U.A. centros irão demorar mais do que os mesmos VHDs/VMs a carregar para o Azure europeus centros de dados.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Carregar um VHD no local para o Azure
Para carregar uma VM existente ou VHD a partir da rede no local VM ou VHD tem de cumprir os requisitos, tal como indicado no capítulo [preparação para mover uma VM no local para o Azure com um disco não generalizada] [ planning-guide-5.2.1]deste documento.

Tal uma VM não tem de ser generalizado e pode ser carregada no Estado e forma tem após encerramento no lado no local. O mesmo é verdadeiro para os VHDs adicionais que não contêm a qualquer sistema operativo.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Carregar um VHD e tornando um disco do Azure
Neste caso, queremos carregar um VHD, com ou sem um sistema operacional e montá-la a uma VM como um disco de dados ou utilizá-lo como disco do SO. Este é um processo de vários passo

**PowerShell**

* Inicie sessão na sua subscrição com *Connect-AzureRmAccount*
* Definir a subscrição do seu contexto com *Set-AzureRmContext* e parâmetro SubscriptionId ou SubscriptionName - veja <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Carregar o VHD com o *Add-AzureRmVhd* para uma conta de armazenamento do Azure - veja <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Opcional) Criar um disco gerido a partir de VHD com o *New-AzureRmDisk* -veja <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk>
* Definir o disco do SO de uma nova configuração VM para o VHD ou um disco gerido com *Set-AzureRmVMOSDisk* -veja <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
* Criar uma nova VM a partir da configuração VM com *New-AzureRmVM* -veja <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>
* Adicionar um disco de dados a uma VM nova com *Add-AzureRmVMDataDisk* -veja <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk>

**CLI 2.0 do Azure**

* Inicie sessão na sua subscrição com *início de sessão az*
* Selecione a sua subscrição com *set de conta de az--subscrição `<subscription name or id`>*
* Carregar o VHD com o *carregamento de BLOBs de armazenamento az* -veja [utilizar a CLI do Azure com o armazenamento do Azure][storage-azure-cli]
* (Opcional) Criar um disco gerido a partir de VHD com o *criar disco de az* -veja https://docs.microsoft.com/cli/azure/disk#az_disk_create
* Criar uma nova VM, especificando o VHD carregado ou o disco gerido como disco do SO com *az vm crie* e o parâmetro *– anexar-disco de SO*
* Adicionar um disco de dados a uma VM nova com *anexar o disco da vm az* e o parâmetro *-novo*

**Modelo**

* Carregar o VHD com a CLI do Azure ou do Powershell
* (Opcional) Criar um disco gerido a partir de VHD com o Powershell, CLI do Azure ou o portal do Azure
* Implementar a VM com um modelo JSON que referencia o VHD, conforme mostrado na [este modelo JSON de exemplo](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json) ou utilizar discos geridos, conforme mostrado na [este modelo JSON de exemplo](https://github.com/Azure/azure-quickstart-templates/blob/master/sap-2-tier-user-image-md/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>Implantação de uma imagem VM
Para carregar uma VM existente ou VHD a partir da rede no local para usá-lo como uma imagem de VM do Azure VM ou VHD tem de cumprir os requisitos listados no capítulo [preparação para implementar uma VM com uma imagem de específicas do cliente para o SAP] [ planning-guide-5.2.2] deste documento.

* Uso *sysprep* no Windows ou *waagent-desaprovisionamento* no Linux para generalizar a VM - veja [referência técnica de Sysprep](https://technet.microsoft.com/library/cc766049.aspx) para Windows ou [como capturar um Máquina virtual do Linux para utilizar como um modelo do Resource Manager] [ capture-image-linux-step-2-create-vm-image] para Linux
* Inicie sessão na sua subscrição com *Connect-AzureRmAccount*
* Definir a subscrição do seu contexto com *Set-AzureRmContext* e parâmetro SubscriptionId ou SubscriptionName - veja <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Carregar o VHD com o *Add-AzureRmVhd* para uma conta de armazenamento do Azure - veja <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Opcional) Criar uma imagem de disco gerido a partir de VHD com o *New-AzureRmImage* -veja <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimage>
* Configurar o disco do SO de uma nova configuração VM para o
  * VHD com o *Set-AzureRmVMOSDisk - SourceImageUri - CreateOption fromImage* -veja <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
  * Geridos a imagem de disco *Set-AzureRmVMSourceImage* -veja <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage>
* Criar uma nova VM a partir da configuração VM com *New-AzureRmVM* -veja <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>

**CLI 2.0 do Azure**

* Uso *sysprep* no Windows ou *waagent-desaprovisionamento* no Linux para generalizar a VM - veja [referência técnica de Sysprep](https://technet.microsoft.com/library/cc766049.aspx) para Windows ou [como capturar um Máquina virtual do Linux para utilizar como um modelo do Resource Manager] [ capture-image-linux-step-2-create-vm-image] para Linux
* Inicie sessão na sua subscrição com *início de sessão az*
* Selecione a sua subscrição com *set de conta de az--subscrição `<subscription name or id`>*
* Carregar o VHD com o *carregamento de BLOBs de armazenamento az* -veja [utilizar a CLI do Azure com o armazenamento do Azure][storage-azure-cli]
* (Opcional) Criar uma imagem de disco gerido a partir de VHD com o *criar imagem de az* -veja https://docs.microsoft.com/cli/azure/image#az_image_create
* Criar uma nova VM, especificando o VHD carregado ou a imagem de disco gerido como disco do SO com *az vm crie* e o parâmetro *– imagem*

**Modelo**

* Uso *sysprep* no Windows ou *waagent-desaprovisionamento* no Linux para generalizar a VM - veja [referência técnica de Sysprep](https://technet.microsoft.com/library/cc766049.aspx) para Windows ou [como capturar um Máquina virtual do Linux para utilizar como um modelo do Resource Manager] [ capture-image-linux-step-2-create-vm-image] para Linux
* Carregar o VHD com a CLI do Azure ou do Powershell
* (Opcional) Criar uma imagem de disco gerido a partir de VHD com o Powershell, CLI do Azure ou o portal do Azure
* Implementar a VM com um modelo JSON que referencia a imagem de VHD, conforme mostrado na [este modelo JSON de exemplo](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sap-2-tier-user-image/azuredeploy.json) ou ao utilizar a imagem de disco gerido, como mostrado na [este modelo JSON de exemplo](https://github.com/Azure/azure-quickstart-templates/blob/master/sap-2-tier-user-image-md/azuredeploy.json).

#### <a name="downloading-vhds-or-managed-disks-to-on-premises"></a>Download de VHDs ou discos geridos no local
Infraestrutura como um serviço do Azure não é uma rua unidirecional de apenas poder carregar VHDs e SAP sistemas. Pode mover SAP fazer uma cópia de sistemas do Azure no mundo no local.

Durante o período de tempo da transferência os VHDs ou os discos geridos não pode estar ativos. Mesmo quando a transferência de discos que estão montados para VMs, a VM tem de ser desligado e desalocada. Se apenas pretender transferir o conteúdo da base de dados que, em seguida, deve ser utilizado para configurar um novo sistema no local e se for aceitável durante o tempo de download e a configuração do novo sistema que o sistema no Azure, ainda pode estar operacional , poderia evitar um longo período de indisponibilidade, efetuando uma cópia de segurança do banco de dados compactado para um disco e acabou de baixar esse disco utilizado em vez de transferirem também a VM de base do sistema operacional.

#### <a name="powershell"></a>PowerShell

  * Transferir um disco gerido  
  Tem primeiro de obter acesso ao blob subjacente do disco gerido. Em seguida, pode copiar o blob subjacente para uma nova conta de armazenamento e transferir o blob a partir desta conta de armazenamento.

  ```powershell
  $access = Grant-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name> -Access Read -DurationInSecond 3600
  $key = (Get-AzureRmStorageAccountKey -ResourceGroupName <resource group> -Name <storage account name>)[0].Value
  $destContext = (New-AzureStorageContext -StorageAccountName <storage account name -StorageAccountKey $key)
  Start-AzureStorageBlobCopy -AbsoluteUri $access.AccessSAS -DestContainer <container name> -DestBlob <blob name> -DestContext $destContext
  # Wait for blob copy to finish
  Get-AzureStorageBlobCopyState -Container <container name> -Blob <blob name> -Context $destContext
  Save-AzureRmVhd -SourceUri <blob in new storage account> -LocalFilePath <local file path> -StorageKey $key
  # Wait for download to finish
  Revoke-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name>
  ```

  * Transferir um VHD  
  Depois do sistema SAP está parado e a VM é encerrada, pode utilizar o cmdlet do PowerShell Save-AzureRmVhd no destino no local para transferir os discos VHD para o mundo no local. Para fazer isso, é necessário o URL do VHD que pode ser encontrado na "secção armazenamento" do portal do Azure (é necessário navegar para a conta de armazenamento e o contentor de armazenamento onde o VHD foi criado) e precisa saber onde o VHD deve ser copiado para.

  Em seguida, pode aproveitar o comando simplesmente definindo o parâmetro SourceUri como o URL de VHD para transferir e a LocalFilePath como a localização física do VHD (incluindo o respetivo nome). O comando foi o seguinte aspeto:

  ```powerhell
  Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
  ```

  Para obter mais detalhes sobre o cmdlet Save-AzureRmVhd, verifique aqui <https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvhd>.

#### <a name="cli-20"></a>CLI 2.0
  * Transferir um disco gerido  
  Tem primeiro de obter acesso ao blob subjacente do disco gerido. Em seguida, pode copiar o blob subjacente para uma nova conta de armazenamento e transferir o blob a partir desta conta de armazenamento.
  ```
  az disk grant-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --duration-in-seconds 3600
  az storage blob download --sas-token "<sas token>" --account-name <account name> --container-name <container name> --name <blob name> --file <local file>
  az disk revoke-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>"
  ```

  * Transferir um VHD   
  Depois do sistema SAP está parado e a VM é encerrada, pode utilizar o comando da CLI do Azure _download de blob de armazenamento do azure_ no destino no local para transferir o VHD de discos de volta para o mundo no local. Para fazer isso, precisa o nome e o contentor do VHD que pode encontrar em 'seção de armazenamento"do portal do Azure (é necessário navegar para a conta de armazenamento e o contentor de armazenamento onde o VHD foi criado) e precisa saber onde o VHD deverá ser copi Ed para.

  Em seguida, pode aproveitar o comando simplesmente definindo o blob de parâmetros e contentor de VHD para o download e o destino como a localização de destino físico do VHD (incluindo o respetivo nome). O comando foi o seguinte aspeto:

  ```
  az storage blob download --name <name of the VHD to download> --container-name <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --file <destination of the VHD to download>
  ```

### <a name="transferring-vms-and-disks-within-azure"></a>Transferência de VMs e os discos no Azure
#### <a name="copying-sap-systems-within-azure"></a>Copiar sistemas SAP no Azure
Um sistema SAP ou até mesmo um DBMS servidor dedicado que suporta uma camada de aplicação SAP provavelmente consistirá em vários discos que contêm o sistema operacional com os binários ou o ficheiro ou ficheiros de dados e de registo da base de dados SAP. Nem a funcionalidade do Azure de cópia de discos nem a funcionalidade do Azure de salvar os discos para um disco local tem um mecanismo de sincronização, o que seria vários discos de instantâneo de forma síncrona. Por conseguinte, o estado dos discos guardados ou copiados, mesmo que os estão montados contra a mesma VM deve ser diferente. Isso significa que no caso de ter dados diferentes e logfile(s) contidos em diferentes discos concreto, a base de dados no final seria inconsistente.

**Conclusão: Para copiar ou guardar discos que fazem parte de uma configuração de sistema SAP terá de parar o sistema SAP e também tem de encerrar a VM implementada. Só que, em seguida, pode copiar ou transferir o conjunto de discos para criar uma cópia do sistema SAP no Azure ou no local.**

Discos de dados podem ser armazenados como arquivos VHD numa conta de armazenamento do Azure e podem ser anexados diretamente a uma máquina virtual ou ser utilizados como uma imagem. Neste caso, o VHD é copiado para outra localização antes de estar ligado à máquina virtual. O nome completo do ficheiro VHD no Azure tem de ser exclusivo no Azure. Como anteriormente, já mencionado, o nome é o tipo de um nome de três partes que se parece com:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Discos de dados também podem ser discos geridos. Neste caso, o disco gerido é utilizado para criar um novo disco gerido antes de estar ligado à máquina virtual. O nome do disco gerido tem de ser exclusivo dentro de um grupo de recursos.

##### <a name="powershell"></a>PowerShell
Pode utilizar os cmdlets do Azure PowerShell para copiar um VHD, conforme mostrado na [este artigo][storage-powershell-guide-full-copy-vhd]. Para criar um novo disco gerido, utilize New-AzureRmDiskConfig e New-AzureRmDisk, conforme mostrado no exemplo a seguir.

```powershell
$config = New-AzureRmDiskConfig -CreateOption Copy -SourceUri "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" -Location <location>
New-AzureRmDisk -ResourceGroupName <resource group name> -DiskName <disk name> -Disk $config
```

##### <a name="cli-20"></a>CLI 2.0
Pode utilizar a CLI do Azure para copiar um VHD, conforme mostrado na [este artigo][storage-azure-cli-copy-blobs]. Para criar um novo disco gerido, utilize *criar disco de az* conforme mostrado no exemplo a seguir.

```
az disk create --source "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --name <disk name> --resource-group <resource group name> --location <location>
```

##### <a name="azure-storage-tools"></a>Ferramentas de armazenamento do Azure
* <http://storageexplorer.com/>

As edições Professional do exploradores de armazenamento do Azure podem ser encontradas aqui:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>

A cópia de um VHD numa conta de armazenamento em si é um processo que demora apenas alguns segundos (semelhante ao hardware de SAN, criação de instantâneos com a cópia lenta e copiar ao gravar). Depois de ter uma cópia do ficheiro VHD pode anexá-lo a uma máquina virtual ou utilizá-la como uma imagem para anexar cópias do VHD para máquinas virtuais.

##### <a name="powershell"></a>PowerShell
```powershell
# attach a vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path to vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a managed disk to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -ManagedDiskId <managed disk id> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of the vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name <disk name> -VhdUri <new path of vhd> -SourceImageUri <path to image vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption fromImage
$vm | Update-AzureRmVM

# attach a copy of the managed disk to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$diskConfig = New-AzureRmDiskConfig -Location $vm.Location -CreateOption Copy -SourceUri <source managed disk id>
$disk = New-AzureRmDisk -DiskName <disk name> -Disk $diskConfig -ResourceGroupName <resource group name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Caching <caching option> -Lun <lun, for example 0> -CreateOption attach -ManagedDiskId $disk.Id
$vm | Update-AzureRmVM
```
##### <a name="cli-20"></a>CLI 2.0
```

# attach a vhd to a vm
az vm unmanaged-disk attach --resource-group <resource group name> --vm-name <vm name> --vhd-uri <path to vhd>

# attach a managed disk to a vm
az vm disk attach --resource-group <resource group name> --vm-name <vm name> --disk <managed disk id>

# attach a copy of the vhd to a vm
# this scenario is currently not possible with Azure CLI. A workaround is to manually copy the vhd to the destination.

# attach a copy of a managed disk to a vm
az disk create --name <new disk name> --resource-group <resource group name> --location <location of target virtual machine> --source <source managed disk id>
az vm disk attach --disk <new disk name or managed disk id> --resource-group <resource group name> --vm-name <vm name> --caching <caching option> --lun <lun, for example 0>
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Copiar os discos entre contas de armazenamento do Azure
Não é possível efetuar esta tarefa no portal do Azure. Pode utilizar cmdlets do Azure PowerShell, CLI do Azure ou um navegador de armazenamento de terceiros. Os cmdlets do PowerShell ou comandos da CLI, podem criar e gerir blobs, que incluem a capacidade de forma assíncrona copiar blobs entre contas de armazenamento e entre regiões dentro da subscrição do Azure.

##### <a name="powershell"></a>PowerShell
Também pode copiar VHDs entre subscrições. Para obter mais informações, leia [este artigo][storage-powershell-guide-full-copy-vhd].

O fluxo básico de lógica de cmdlet PS tem esta aparência:

* Criar um contexto de conta de armazenamento para o **origem** conta de armazenamento com *New-AzureStorageContext* -veja <https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext>
* Criar um contexto de conta de armazenamento para o **destino** conta de armazenamento com *New-AzureStorageContext* -veja <https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext>
* Iniciar a cópia com

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Verificar o estado da cópia num loop com

```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Anexe novo VHD para uma máquina virtual, conforme descrito acima.

Para exemplos, consulte [este artigo][storage-powershell-guide-full-copy-vhd].

##### <a name="cli-20"></a>CLI 2.0
* Iniciar a cópia com

```
az storage blob copy start --source-blob <source blob name> --source-container <source container name> --source-account-name <source storage account name> --source-account-key <source storage account key> --destination-container <target container name> --destination-blob <target blob name> --account-name <target storage account name> --account-key <target storage account name>
```

* Verificar o estado se a cópia num loop com

```
az storage blob show --name <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* Anexe novo VHD para uma máquina virtual, conforme descrito acima.

Para exemplos, consulte [este artigo][storage-azure-cli-copy-blobs].

### <a name="disk-handling"></a>Manipulação de disco
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>Estrutura VM/disco para implementações de SAP
Idealmente, o processamento da estrutura de uma VM e os discos associados deve ser muito simples. Em instalações no local, os clientes desenvolveu várias maneiras de estruturar uma instalação de servidor.

* Um base disco que contém o sistema operacional e todos os binários do DBMS e/ou do SAP. Desde Março de 2015, este disco pode ser até 1TB de tamanho, em vez de restrições anteriores, que se limitando ele a 127 GB.
* Ficheiro da base de dados SAP de registo de um ou vários discos, que contém o DBMS e o ficheiro de registo do DBMS armazenamento na área temp (se o DBMS oferece suporte a isso). Se os requisitos de IOPS de registo de base de dados são elevados, precisa da faixa de vários discos para atingir o volume IOPS necessário.
* Um número de discos que contém um ou dois arquivos de banco de dados da base de dados SAP e também os arquivos de dados temporário do DBMS (se o DBMS oferece suporte a isso).

![Referência de configuração da VM de IaaS do Azure para SAP][planning-guide-figure-1300]

[comment]: <> (MShermannd TODO descrever a estrutura do Linux  )

- - -
> ![Windows][Logo_Windows] Windows
>
> Com muitos clientes que vimos configurações onde, por exemplo, SAP e DBMS binários não foram instalados na unidade c:\ em que o sistema operacional foi instalado. Havia vários motivos para isso, mas quando fomos voltar para a raiz, normalmente era que as unidades eram pequenas e as atualizações de SO necessário espaço adicional de 10 a 15 anos. Ambas as condições não se aplicam atualmente demasiado, muitas vezes, deixa de poder. Hoje em dia, a unidade c:\ pode ser mapeada em discos de grande volume ou VMs. Para simplificar as implementações na sua estrutura, é recomendado que seguem o padrão de implementação seguinte para sistemas SAP NetWeaver no Azure
>
> O ficheiro de paginação do sistema operativo do Windows deve estar na unidade d: (disco não persistentes)
>
> ![Linux][Logo_Linux] Linux
>
> Coloque o swapfile Linux em /mnt/mnt/recurso no Linux, conforme descrito em [este artigo][virtual-machines-linux-agent-user-guide]. O ficheiro de troca pode ser configurado no ficheiro de configuração de /etc/waagent.Conf. o agente do Linux. Adicionar ou alterar as seguintes definições:
>
>

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

Para ativar as alterações, tem de reiniciar o agente do Linux com

```
sudo service waagent restart
```

Leia a nota SAP [1597355] para obter mais detalhes sobre o tamanho do ficheiro de troca recomendada

- - -
O número de discos utilizados para os ficheiros de dados do DBMS e o tipo de armazenamento do Azure, estes discos estão alojados no deve ser determinado pelos requisitos de IOPS e a latência necessário. Quotas exatas são descritas nas [deste artigo (Linux)] [ virtual-machines-sizes-linux] e [neste artigo (Windows)][virtual-machines-sizes-windows].

Experiência de implementações de SAP nos últimos 2 anos ensinado-nos algumas lições que podem ser resumidas como:

* Tráfego IOPS para ficheiros de dados diferente nem sempre é o mesmo, uma vez que os sistemas de cliente existentes podem ter forma diferente em tamanho normal ficheiros de dados que representa seus bancos de dados do SAP. Como resultado descobriu-se melhor usar uma configuração RAID ao longo de vários discos para colocar os arquivos de dados que LUNs criados fora aqueles. Havia situações, especialmente com o armazenamento padrão do Azure, onde uma taxa IOPS atingir a quota de um único disco contra o registo de transações do DBMS. Em tais cenários é recomendada a utilização do armazenamento Premium ou agregar em alternativa o armazenamento Standard vários discos com um software de RAID.

- - -
> ![Windows][Logo_Windows] Windows
>
> * [Melhores práticas de desempenho para o SQL Server em máquinas de virtuais do Azure][virtual-machines-sql-server-performance-best-practices]
>
> ![Linux][Logo_Linux] Linux
>
> * [Configurar o RAID de Software no Linux][virtual-machines-linux-configure-raid]
> * [Configurar LVM numa VM do Linux no Azure][virtual-machines-linux-configure-lvm]
> * [Segredos do armazenamento do Azure e otimizações de e/s do Linux](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
>
>

- - -
* O armazenamento Premium está mostrando significativo um melhor desempenho, especialmente para gravações de log de transações críticas. Para cenários SAP que espera-se para fornecer a produção, como desempenho, recomendamos utilizar série de VM que pode tirar partido do armazenamento Premium do Azure.

Tenha em atenção que o disco que contém o sistema operacional, e como é recomendável, os binários do SAP e a base de dados (base VM), não é mais limitada a 127GB. Agora pode ter até 1TB de tamanho. Deve ser espaço suficiente para manter todos os ficheiros necessários, incluindo, por exemplo, registos de tarefa de lote SAP.

Para obter mais sugestões e obter mais detalhes, especificamente para VMs do DBMS, consulte o [guia de implementação de DBMS][dbms-guide]

#### <a name="disk-handling"></a>Manipulação de disco
Na maioria dos cenários, precisa criar discos adicionais para poder implementar a base de dados SAP na VM. Falamos sobre as considerações de no número de discos no capítulo [estrutura/disco da VM para implementações de SAP] [ planning-guide-5.5.1] deste documento. O portal do Azure permite anexar e desanexar discos depois de implementada uma VM base. Os discos podem ser ligado/desligado quando a VM está a funcionar e em execução, bem como quando ele é interrompido. Ao anexar um disco, o portal do Azure oferece para anexar um disco vazio ou um disco existente que no momento não está ligado a outra VM.

**Tenha em atenção**: discos só podem ser anexados a uma VM em qualquer momento.

![Anexar / desanexar discos de armazenamento Standard do Azure][planning-guide-figure-1400]

Durante a implementação de uma nova máquina virtual, pode decidir se pretende utilizar Managed Disks ou coloque os discos nas contas de armazenamento do Azure. Se pretender utilizar o armazenamento Premium, recomendamos que utilize o Managed Disks.

Em seguida, terá de decidir se pretende criar um disco novo e vazio ou se pretender selecionar um disco existente que foi carregado anteriormente e deve ser anexado à VM agora.

**IMPORTANTE**: **fazer não** pretende utilizar a colocação em cache do anfitrião com o armazenamento padrão do Azure. Deve deixar a preferência de Cache do anfitrião com a predefinição de nenhuma. Armazenamento Premium do Azure deverá ativar a colocação em cache de leitura se a característica de e/s é lida principalmente como o tráfego de e/s típico em relação a arquivos de dados do banco de dados. Em caso de arquivo de log de transação de base de dados sem colocação em cache é recomendada.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Como anexar um disco de dados no portal do Azure][virtual-machines-linux-attach-disk-portal]
>
> Se os discos estão anexados, tem de iniciar sessão VM para abrir o Gestor de discos do Windows. Se não estiver ativada automontar conforme recomendado nas capítulo [definição automontar para discos anexados][planning-guide-5.5.3], o volume anexado recentemente é necessário realizar online e ser inicializados.
>
> ![Linux][Logo_Linux] Linux
>
> Se os discos estão anexados, tem de iniciar sessão VM e inicializar os discos, conforme descrito em [neste artigo][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]
>
>

- - -
Se o novo disco for um disco vazio, precisa de formatar o disco também. Para a formatação, especialmente para arquivos de dados e de registo do DBMS as mesmas recomendações de e para implementações bare-metal do DBMS aplicam-se.

Como já foi mencionado no capítulo [o conceito de Máquina Virtual do Azure de Microsoft][planning-guide-3.2], uma conta de armazenamento do Azure não fornece recursos infinitos em termos de volume de e/s, IOPS e volumes de dados. Normalmente, as VMs do DBMS mais são afetadas por este. Talvez seja melhor usar uma conta de armazenamento separada para cada VM, se tiver o volume de e/s elevado algumas VMs para implementar, para se manter dentro do limite do volume de conta de armazenamento do Azure. Caso contrário, terá de ver como equilibrar estas VMs entre diferentes contas de armazenamento sem atingir o limite de cada conta de armazenamento única. Obter mais detalhes são discutidas a [guia de implementação de DBMS][dbms-guide]. Também deve considerar essas limitações em mente para o aplicativo SAP puro VMs de servidor ou de outras VMs que, eventualmente, podem exigir VHDs adicionais. Estas restrições não se aplicam se utilizar o disco gerido. Se planeia utilizar o armazenamento Premium, recomendamos que utilize o disco gerido.

Outro tópico, que é relevante para as contas de armazenamento é se estiver recebendo os VHDs numa conta de armazenamento georreplicado. A georreplicação está ativada ou desabilitada no nível da conta de armazenamento e não no nível da VM. Se estiver ativada a georreplicação, os VHDs na conta de armazenamento seriam replicados no outro Datacenter do Azure na mesma região. Antes de decidir sobre isso, deve pensar sobre a restrição seguinte:

Georreplicação do Azure funciona localmente em cada VHD numa VM e não é replicado do IOs em ordem cronológica em vários VHDs numa VM. Por conseguinte, o VHD que representa a VM base, bem como todos os VHDs adicionais anexados à VM são replicados independentes umas das outras. Isso significa que não existe nenhuma sincronização entre as alterações nos VHDs de diferentes. O fato de que o IOs são replicados independentemente a ordem na qual eles são escritos significa que a georreplicação não é de valor para os servidores de base de dados com seus bancos de dados distribuídos por vários VHDs. Para além do DBMS, também pode haver outros aplicativos em que os processos de escreverem ou manipulam dados em VHDs de diferentes e em que é importante manter a ordem das alterações. Se for um requisito, georreplicação no Azure não deve ser ativada. Depende de se precisam ou desejam georreplicação para um conjunto de VMs, mas não para outro conjunto, pode já categorizar VMs e seus VHDs relacionados em diferentes contas de armazenamento que têm a georreplicação ativada ou desativada.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Definição automontar para discos anexados
- - -
> ![Windows][Logo_Windows] Windows
>
> Para as VMs que são criadas a partir do próprio imagens ou discos, é necessário verificar e, possivelmente, defina o parâmetro de automontar. Definir este parâmetro permite que a VM após um reinício ou a nova implementação do Azure para montar as unidades anexadas/montado novamente automaticamente.
> O parâmetro está definido para as imagens fornecidas pela Microsoft no Azure Marketplace.
>
> Para definir o automontar, verifique a documentação de linha de comandos executável diskpart.exe aqui:
>
> * [Opções de linha de comandos DiskPart](https://technet.microsoft.com/library/bb490893.aspx)
> * [Automontar](http://technet.microsoft.com/library/cc753703.aspx)
>
> A janela de linha de comandos do Windows deve ser aberta como administrador.
>
> Se os discos estão anexados, tem de iniciar sessão VM para abrir o Gestor de discos do Windows. Se não estiver ativada automontar conforme recomendado nas capítulo [definição automontar para discos anexados][planning-guide-5.5.3], a recentemente anexado volume > seguimento online e ser inicializados.
>
> ![Linux][Logo_Linux] Linux
>
> Tem de inicializar um disco vazio anexado recentemente, conforme descrito em [este artigo][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> Também terá de adicionar novos discos para o /etc/fstab.
>
>

- - -
### <a name="final-deployment"></a>Implantação final
Para a implantação final e os passos exatos, particularmente com respeito a implementação de SAP estendido monitorização, consulte a [guia de implementação][deployment-guide].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Aceder a sistemas SAP em execução em VMs do Azure
Para cenários de apenas na Cloud, pode querer ligar a esses sistemas SAP através da internet pública usando a GUI do SAP. Nesses casos, os procedimentos seguintes têm de ser aplicadas.

Mais tarde o documento, abordaremos o outro cenário principais, ligar a sistemas SAP em implementações de em vários locais que tenham uma ligação site a site (túnel VPN) ou a ligação do ExpressRoute do Azure entre os sistemas no local e os sistemas do Azure.

### <a name="remote-access-to-sap-systems"></a>Acesso remoto aos sistemas SAP
Com o Azure Resource Manager existem não padrão pontos finais mais, como no antigo modelo clássico. Todas as portas de uma VM de ARM do Azure estão abertas, desde:

1. Nenhum grupo de segurança de rede está definido para a sub-rede ou a interface de rede. Tráfego de rede para as VMs do Azure pode ser protegido por meio de chamadas "grupos de segurança de rede". Para obter mais informações consulte [o que é um grupo de segurança de rede (NSG)?][virtual-networks-nsg]
2. Nenhum Balanceador de carga do Azure é definido para a interface de rede   

Ver a diferença de arquitetura entre ARM e o modelo clássico, conforme descrito em [este artigo][virtual-machines-azure-resource-manager-architecture].

#### <a name="configuration-of-the-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>Configuração de sistema SAP e SAP GUI de conectividade para o cenário de apenas na Cloud
Consulte este artigo que descreve os detalhes a esse tópico: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### <a name="changing-firewall-settings-within-vm"></a>Alterar as definições da Firewall na VM
Poderá ser necessário configurá-la nas suas máquinas virtuais para permitir tráfego de entrada para o seu sistema SAP.

- - -
> ![Windows][Logo_Windows] Windows
>
> Por predefinição, o Firewall do Windows dentro de uma VM implementada do Azure está ativada. Agora tem de permitir que a porta de SAP sejam abertas, caso contrário, a GUI do SAP não será capaz de se ligar.
> Para efetuar este procedimento:
>
> * Abrir controlo\sistema e segurança \ Firewall para **definições avançadas**.
> * Agora, com o botão direito em regras de entrada e escolheu **nova regra**.
> * No assistente seguinte optar por criar um novo **porta** regra.
> * No próximo passo do assistente, deixe o valor em TCP e escreva o número de porta que pretende abrir. Uma vez que nossa ID de instância do SAP é 00, tiramos 3200. Se a sua instância tem um número de instância diferente, a porta que definiu anteriormente com base no número de instância deve ser aberta.
> * A parte seguinte do assistente, tem de deixar o item **permitir a ligação** marcada.
> * No próximo passo do assistente, precisa definir se a regra aplica-se para a rede de domínio, privado e público. Ajuste-lo se for necessário para as suas necessidades. No entanto, com GUI de SAP a ligar de fora através da rede pública, tem de ter a regra aplicada com a rede pública.
> * No último passo do assistente, nomear a regra e guardar, premindo **concluir**.
>
> A regra entra em vigor imediatamente.
>
> ![Definição da regra de porta][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> As imagens do Linux no Azure Marketplace não ativar a firewall de iptables por predefinição e a ligação ao seu sistema SAP deve funcionar. Se ativou iptables ou outro firewall, consulte a documentação de iptables ou a firewall utilizada para permitir tráfego tcp de entrada para porta 32xx (onde xx é o número de sistema do seu sistema SAP).
>
>

- - -
#### <a name="security-recommendations"></a>Recomendações de segurança
A GUI do SAP não liga imediatamente a qualquer uma das instâncias do SAP (porta 32xx) que estão em execução, mas se liga inicialmente por meio da porta aberta para o processo de servidor de mensagens SAP (porta 36xx). No passado a mesma porta foi utilizada pelo servidor de mensagem para a comunicação interna para instâncias da aplicação. Para impedir que os servidores no local aplicação inadvertidamente comunique com um servidor de mensagens no Azure, as portas de comunicação interno podem ser alteradas. É altamente recomendado para alterar a comunicação interna entre o servidor de mensagens do SAP e suas instâncias da aplicação para um número de porta diferente em sistemas que foram clonados desde sistemas no local, como um clone de desenvolvimento para etc de teste do projeto. Isso pode ser feito com o parâmetro de perfil predefinido:

> rdisp/msserv_internal
>
>

conforme documentado no [definições de segurança para o servidor de mensagens do SAP ](https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm)

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>Conceitos de implementação apenas na Cloud de instâncias do SAP
### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>VM única com o SAP NetWeaver cenário de demonstração/treinamento
![Com os mesmos nomes VM a executar sistemas de demonstração de SAP de VM únicos, isolados nos serviços Cloud do Azure][planning-guide-figure-1700]

Neste cenário (consulte o capítulo [apenas na Cloud] [ planning-guide-2.1] deste documento), estamos implementando um cenário de sistema de treinamento/demonstração típico em que o cenário de treinamento/demonstração completo está contido numa única VM. Partimos do princípio de que a implementação é feita por meio de modelos de imagem VM. Vamos também assumir esse múltiplo destes demonstração/treinamentos necessidade de VMs a serem implantados com as VMs com o mesmo nome.

A pressuposição é de que criou uma imagem de VM, conforme descrito em algumas seções do capítulo [preparar VMs SAP para o Azure] [ planning-guide-5.2] neste documento.

A sequência de eventos para implementar o cenário tem esta aparência:

##### <a name="powershell"></a>PowerShell
* Criar um novo grupo de recursos para cada cenário de treinamento/demonstração

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```
* Criar uma nova conta de armazenamento, se não pretender utilizar os Managed Disks

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Crie uma nova rede virtual para cada cenário de treinamento/demonstração ativar a utilização de endereços IP e o mesmo nome de anfitrião. A rede virtual está protegida por um grupo de segurança de rede que só permite o tráfego para a porta 3389 para ativar o acesso de ambiente de trabalho remoto e a porta 22 para SSH.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Criar um novo endereço IP público que pode ser utilizado para aceder à máquina virtual a partir da internet

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Criar uma nova interface de rede para a máquina virtual

```powershell
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip
```

* Cria uma máquina virtual. Para o cenário de apenas na Cloud, todas as VMS terão o mesmo nome. O SID SAP das instâncias do SAP NetWeaver nessas VMs serão os mesmos também. Dentro do grupo de recursos do Azure, o nome da VM tem de ser exclusivo, mas em diferentes grupos de recursos do Azure podem executar as VMs com o mesmo nome. A conta de "Administrador" predefinidas do Windows ou de raiz para Linux não são válidos. Por conseguinte, um novo nome de utilizador de administrador tem de ser definido, juntamente com uma palavra-passe. O tamanho da VM também tem de ser definido.

```powershell
#####
# Create a new virtual machine with an official image from the Azure Marketplace
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES-SAP" -Skus "12-SP1" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "Oracle" -Offer "Oracle-Linux" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a Managed Disk Image
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -Id <Id of Managed Disk Image>
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Opcionalmente, adicione discos adicionais e restaurar o conteúdo necessário. Lembre-se de que todos os nomes de BLOBs (URLs para os blobs) tem de ser exclusivos no Azure.

```powershell
# Optional: Attach additional VHD data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM

# Optional: Attach additional Managed Disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -DiskSizeInGB 1023 -CreateOption empty -Lun 0 | Update-AzureRmVM
```

##### <a name="cli"></a>CLI
O código de exemplo a seguir pode ser utilizado no Linux. Para Windows, tente utilizar PowerShell conforme descrito acima ou adaptar o exemplo para utilizar % rgName % em vez de $rgName e defina a variável de ambiente com o comando do Windows *definir*.

* Criar um novo grupo de recursos para cada cenário de treinamento/demonstração

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
az group create --name $rgName --location "North Europe"
```

* Criar uma nova conta de armazenamento

```
az storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku Standard_LRS --name $rgNameLower
```

* Crie uma nova rede virtual para cada cenário de treinamento/demonstração ativar a utilização de endereços IP e o mesmo nome de anfitrião. A rede virtual está protegida por um grupo de segurança de rede que só permite o tráfego para a porta 3389 para ativar o acesso de ambiente de trabalho remoto e a porta 22 para SSH.

```
az network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

az network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
az network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group SAPERPDemoNSG
```

* Criar um novo endereço IP público que pode ser utilizado para aceder à máquina virtual a partir da internet

```
az network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --dns-name $rgNameLower --allocation-method Dynamic
```

* Criar uma nova interface de rede para a máquina virtual

```
az network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-address SAPERPDemoPIP --subnet Subnet1 --vnet-name SAPERPDemoVNet
```

* Cria uma máquina virtual. Para o cenário de apenas na Cloud, todas as VMS terão o mesmo nome. O SID SAP das instâncias do SAP NetWeaver nessas VMs serão os mesmos também. Dentro do grupo de recursos do Azure, o nome da VM tem de ser exclusivo, mas em diferentes grupos de recursos do Azure podem executar as VMs com o mesmo nome. A conta de "Administrador" predefinidas do Windows ou de raiz para Linux não são válidos. Por conseguinte, um novo nome de utilizador de administrador tem de ser definido, juntamente com uma palavra-passe. O tamanho da VM também tem de ser definido.

```
#####
# Create virtual machines using storage accounts
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password

#####
# Create virtual machines using Managed Disks
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
```

```
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path to image vhd>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path to image vhd> --authentication-type password

#####
# Create a new virtual machine with a Managed Disk Image
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id> --authentication-type password
```

* Opcionalmente, adicione discos adicionais e restaurar o conteúdo necessário. Lembre-se de que todos os nomes de BLOBs (URLs para os blobs) tem de ser exclusivos no Azure.

```
# Optional: Attach additional VHD data disks
az vm unmanaged-disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --vhd-uri https://$rgNameLower.blob.core.windows.net/vhds/data.vhd  --new

# Optional: Attach additional Managed Disks
az vm disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --disk datadisk --new
```

##### <a name="template"></a>Modelo
Pode utilizar os modelos de exemplo no repositório de modelos de início rápido do azure no github.

* [VM do Linux simples](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Windows simples VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [VM a partir da imagem](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-which-need-to-communicate-within-azure"></a>Implementar um conjunto de VMs que têm de comunicar-se no Azure
Este cenário apenas na Cloud é um cenário típico de formação e demonstração fins onde o software que representa o treinamento/demonstração cenário está distribuído por várias VMs. Os diferentes componentes instalados nas VMs diferentes precisam se comunicar entre si. Novamente, este cenário não locais comunicação de rede ou cenário entre locais é necessária.

Este cenário é uma extensão da instalação descrita no capítulo [única VM com o SAP NetWeaver demonstração/treinamento cenário] [ planning-guide-7.1] deste documento. Neste caso mais máquinas virtuais serão adicionadas a um grupo de recursos existente. No exemplo a seguir o cenário de treinamento consiste numa VM do SAP ASCS/SCS, uma VM a executar um DBMS e uma instância de servidor de aplicações SAP VM.

Antes de criar este cenário tem de pensar sobre as definições básicas, como já usufruiu de cenário antes.

#### <a name="resource-group-and-virtual-machine-naming"></a>Grupo de recursos e a atribuição de nomes de Máquina Virtual
Todos os nomes de grupo de recursos tem de ser exclusivos. Desenvolver seu próprio esquema de nomeação dos seus recursos, tais como `<rg-name`>-sufixo.

Nome da máquina virtual tem de ser exclusivo no grupo de recursos.

#### <a name="set-up-network-for-communication-between-the-different-vms"></a>Configurar a rede para comunicação entre as diferentes VMs
![Conjunto de VMs dentro de uma rede Virtual do Azure][planning-guide-figure-1900]

Para evitar conflitos com clones de cenários de treinamento/demonstração mesmo de nomenclatura, tem de criar uma rede Virtual do Azure para cada cenário. Resolução de nomes DNS será fornecida pelo Azure ou pode configurar seu próprio servidor DNS fora do Azure (não a ainda mais ser discutido aqui). Neste cenário, não podemos configurar nosso próprio DNS. Todas as máquinas virtuais dentro de uma rede Virtual do Azure, a comunicação através de nomes de anfitrião será ativada.

Os motivos para separar o treinamento ou demonstração panoramas por redes virtuais e não apenas os grupos de recursos podem ser:

* Tem do ambiente SAP como configurar o seu próprio AD/OpenLDAP e um servidor de domínio deve fazer parte de cada um as panoramas.  
* A paisagem SAP como conjunto de cópia de segurança tem os componentes que precisam para trabalhar com endereços IP fixos.

Podem encontrar mais detalhes sobre redes virtuais do Azure e como defini-las no [este artigo][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>Implementar VMs SAP com a conectividade de rede da empresa (em vários locais)
Executar um ambiente SAP e pretende dividir a implementação de bare-metal de servidores de grande porte DBMS, ambientes locais virtualizados para camadas do aplicativo e menor de 2 camadas configurada sistemas SAP e IaaS do Azure. A pressuposição de base é que sistemas SAP dentro de um ambiente SAP precisam se comunicar entre si e com vários outros componentes de software implementados na empresa, independentemente de sua forma de implementação. Também não deverá haver nenhum diferenças introduzidas pelo formulário de implementação para o usuário final a ligar ao SAP GUI ou outras interfaces. Estas condições apenas podem ser atendidas quando temos as/OpenLDAP de diretório Active Directory no local e os serviços DNS estendidos para os sistemas do Azure através de ligações privadas, como o Azure ExpressRoute ou de conectividade de site-para-site/várias-site.

Para obter mais informações sobre os detalhes da implementação de SAP no Azure, é recomendável que leia capítulo [implementação de conceitos de Cloud-Only de instâncias do SAP] [ planning-guide-7] deste documento que explica alguns dos as construções de noções básicas do Azure e como eles devem ser usados com aplicações SAP no Azure.

### <a name="scenario-of-an-sap-landscape"></a>Cenário de um ambiente SAP
O cenário em vários locais pode ser aproximadamente descrito como em gráficos abaixo:

![Conectividade de site a Site entre no local e de recursos do Azure][planning-guide-figure-2100]

O cenário mostrado acima descreve um cenário em que no local AD/OpenLDAP e DNS é expandido para o Azure. No lado no local, um determinado intervalo de endereços IP é reservado por subscrição do Azure. O intervalo de endereços IP será atribuído a uma rede Virtual do Azure no lado do Azure.

#### <a name="security-considerations"></a>Considerações de segurança
O requisito mínimo é a utilização de protocolos de comunicação segura, como o SSL/TLS para acesso ao browser ou conexões de VPN de acesso ao sistema para os serviços do Azure. A pressuposição é de que as empresas processam de forma muito diferente a ligação VPN entre a sua rede corporativa e o Azure. Algumas empresas um olhar vazio podem abrir todas as portas. Algumas outras empresas podem querer muito precisas no que portas que precisam para abrir, etc.

Na tabela a seguir SAP típico portas de comunicação são listadas. Basicamente, é suficiente abrir a porta de gateway do SAP.

| Serviço | Nome de porta | Exemplo `<nn`> = 01 | Intervalo predefinido (mínima-máxima) | Comentário |
| --- | --- | --- | --- | --- |
| Dispatcher |sapdp`<nn>` consulte * |3201 |3200 - 3299 |SAP Dispatcher, utilizado pelo SAP GUI para Windows e o Java |
| Servidor de mensagens |sapms`<sid`> consulte * * |3600 |sapms gratuita`<anySID`> |SID = o ID de sistema de SAP |
| Gateway |sapgw`<nn`> consulte * |3301 |gratuito |Gateway SAP, utilizado para comunicação CPIC e RFC |
| Router SAP |sapdp99 |3299 |gratuito |Nomes de serviço de CI (instância central) apenas podem ser reatribuídos no /etc/services para um valor arbitrário após a instalação. |

*) nn = número de instância do SAP

*) sid = o ID de sistema de SAP

Informações mais detalhadas sobre as portas necessárias para diferentes produtos SAP ou serviços por produtos SAP podem ser encontrados aqui <http://scn.sap.com/docs/DOC-17124>.
Com este documento deve ser capaz de abrir portas dedicadas no dispositivo de VPN é necessário para cenários e produtos específicos do SAP.

Outra segurança mede quando implementar VMs num cenário como esse poderia ser criar um [grupo de segurança de rede] [ virtual-networks-nsg] para definir as regras de acesso.

### <a name="dealing-with-different-virtual-machine-series"></a>Lidar com diferentes de máquinas virtuais de série
No decorrer da duração de 12 meses a Microsoft adicionou muitos mais tipos VM que diferem no número de vCPUs, memória ou o mais importante no hardware que ele é executado em. Nem todas as essas VMs são suportadas com o SAP (consulte suportado tipos de VM na nota SAP [1928533]). Algumas dessas VMS a executar no gerações de hardware do anfitrião diferente. Estes gerações de hardware anfitrião introdução implementadas na granularidade de uma Azure unidade de escala. Casos de significa que podem surgir em que os diferentes tamanhos de VM que selecionou não podem ser executados na mesma unidade de escala. Um conjunto de disponibilidade está limitado a capacidade de span com a base de um hardware diferente de unidades de escala.  Por exemplo se quiser executar o DBMS em VMs das séries A5 – A11 e a camada de aplicação SAP em VMs de série G, seria forçado a implementar um único sistema SAP ou sistemas SAP diferentes dentro de diferentes conjuntos de disponibilidade.

#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Impressão numa impressora de rede local da instância do SAP no Azure
##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>Impressão sobre TCP/IP no cenário entre locais
Configurar suas impressoras de rede de TCP/IP com base no local na VM do Azure em geral é o mesmo como na sua rede corporativa, pressupondo que tenha um túnel de VPN Site a Site ou uma ligação de ExpressRoute estabelecida.

- - -
> ![Windows][Logo_Windows] Windows
>
> Para efetuar este procedimento:
>
> * Algumas impressoras de rede são fornecidos com um Assistente de configuração que torna fácil de configurar a sua impressora numa VM do Azure. Se nenhum software de assistente tiver sido distribuído com a impressora, a forma manual para configurar a impressora é criar uma nova porta de impressora TCP/IP.
> * Abra o painel de controlo -> dispositivos e impressoras -> Adicionar uma impressora
> * Selecione adicionar uma impressora com um endereço de TCP/IP ou nome de anfitrião
> * Escreva o endereço IP da impressora
> * Porta de impressora 9100 padrão
> * Se for necessário instale manualmente o controlador de impressora apropriados.
>
> ![Linux][Logo_Linux] Linux
>
> * como para seguimento apenas do Windows, o procedimento padrão para instalar uma impressora de rede
> * Basta seguir os guias de Linux públicos para [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) ou [Red Hat e a Oracle Linux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) sobre como adicionar uma impressora.
>
>

- - -
![Impressão de rede][planning-guide-figure-2200]

##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Baseado em Host impressora através de SMB (impressora partilhada) no cenário entre locais
Impressoras baseados em Host não são compatíveis rede por design. Mas uma impressora baseados em host pode ser partilhada entre os computadores numa rede, desde que a impressora está ligada a um computador ligado. Ligar a sua empresa e partilhar a impressora local da rede Site a Site ou ExpressRoute. O protocolo SMB utiliza NetBIOS em vez de DNS como serviço de nomes. O nome de anfitrião NetBIOS pode ser diferente do nome de anfitrião DNS. O padrão caso é que o nome de anfitrião NetBIOS e o nome de anfitrião DNS são idênticos. O domínio DNS não faz sentido o espaço de nomes NetBIOS. Da mesma forma, o nome completamente qualificado do anfitrião DNS com o nome de anfitrião DNS e o domínio DNS não pode ser utilizado no espaço de nomes de NetBIOS.

A partilha de impressoras é identificada por um nome exclusivo da rede:

* Nome de anfitrião do anfitrião SMB (sempre necessário).
* Nome da partilha (sempre necessária).
* Nome do domínio se partilhar a impressora não está no mesmo domínio que o sistema SAP.
* Além disso, um nome de utilizador e uma palavra-passe podem ser necessário para aceder à partilha de impressoras.

Como:

- - -
> ![Windows][Logo_Windows] Windows
>
> Partilhe a impressora local.
> Na VM do Azure, abra o Explorador do Windows e escreva o nome da partilha da impressora.
> Um Assistente de instalação de impressoras orientará pelo processo de instalação.
>
> ![Linux][Logo_Linux] Linux
>
> Aqui estão alguns exemplos de documentação sobre a configuração de impressoras de rede no Linux ou incluindo um capítulo sobre impressão no Linux. Ele funcionará da mesma forma numa VM do Linux do Azure, desde que a VM faz parte de uma VPN:
>
> * SLES <https://en.opensuse.org/SDB:Printing_via_SMB_(Samba)_Share_or_Windows_Share>
> * RHEL ou no Oracle Linux <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sec-Printer_Configuration.html#s1-printing-smb-printer>
>
>

- - -
##### <a name="usb-printer-printer-forwarding"></a>Impressora de USB (encaminhamento de impressora)
No Azure a capacidade dos serviços de ambiente de trabalho remoto para fornecer aos utilizadores o acesso aos seus dispositivos de impressora local numa sessão remota não está disponível.

- - -
> ![Windows][Logo_Windows] Windows
>
> Obter mais detalhes sobre impressão com o Windows podem ser encontradas aqui: <http://technet.microsoft.com/library/jj590748.aspx>.
>
>

- - -
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>Integração do SAP sistemas do Azure para correção e o sistema de transporte (TMS) em vários locais
A alteração do SAP e o sistema de transporte (TMS) tem de ser configurado para exportar e importar o pedido de transporte em todos os sistemas no cenário. Partimos do princípio de que as instâncias de desenvolvimento de um sistema SAP (desenvolvimento) estão localizadas no Azure, ao passo que o controle de qualidade (QA) e os sistemas produtivos (PRD=IIS&sbp=&PVER=5.0&ID=500;100&cat) estão no local. Além disso, partimos do princípio de que existe um diretório de transporte central.

##### <a name="configuring-the-transport-domain"></a>Configurar o domínio de transporte
Configurar o seu domínio de transporte no sistema tiver designado como o controlador de domínio de transporte, conforme descrito em [configurar o controlador de domínio de transporte](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm). Um utilizador de sistema que TMSADM será criado e o destino da RFC necessário serão gerados. Pode verificar estas ligações de RFC com a transação SM59. Resolução de nome de anfitrião tem de estar ativada no seu domínio de transporte.

Como:

* Em nosso cenário decidimos que o sistema QAS no local vai ser o controlador de domínio CTS. Chame transação STMS. É apresentada a caixa de diálogo TMS. É apresentada uma caixa de diálogo de configurar o domínio de transporte. (Esta caixa de diálogo só aparece se ainda não tiver configurado a um domínio de transporte.)
* Certifique-se de que o utilizador criado automaticamente TMSADM está autorizado (SM59 -> ligação ABAP -> TMSADM@E61.DOMAIN_E61 -> detalhes -> Utilities(M) -> teste de autorização). A tela inicial de transação STMS deverá mostrar a que este sistema SAP está agora a funcionar como o controlador de domínio de transporte como mostrado aqui:

![Ecrã inicial de transação STMS no controlador de domínio][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-the-transport-domain"></a>Incluindo sistemas SAP no domínio de transporte
A sequência de inclusão de um sistema SAP num domínio de transporte será semelhante ao seguinte:

* No sistema de desenvolvimento no Azure, vá para o sistema de transporte (cliente 000) e chamar transação STMS. Escolher a configuração de outros na caixa de diálogo e continue com incluem o sistema no domínio. Especificar o controlador de domínio como anfitrião de destino ([incluindo sistemas de SAP no domínio transporte](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). O sistema agora está a aguardar a serem incluídos no domínio de transporte.
* Por motivos de segurança, em seguida, tem de voltar para o controlador de domínio para confirmar o seu pedido. Escolha a descrição geral do sistema e aprovar do sistema de espera. Em seguida, confirmar a linha de comandos e a configuração serão distribuídos.

Este sistema SAP agora contém as informações necessárias sobre todos os outros sistemas SAP no domínio de transporte. Ao mesmo tempo, os dados de endereço do novo sistema SAP são enviados para todos os outros sistemas SAP e o sistema SAP é inserido no perfil de transporte do programa de controlo de transporte. Verifique se RFCs e acesso para o diretório de transporte do domínio de trabalho.

Continuar com a configuração do seu sistema de transporte como de costume, tal como descrito na documentação do [alteração e o sistema de transporte](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

Como:

* Certifique-se de que sua STMS no local está configurado corretamente.
* Certifique-se de que o nome de anfitrião do controlador de domínio de transporte pode ser resolvido através da sua máquina virtual no Azure e vice visa.
* Chamada de transação STMS -> Configuração de outros -> sistema incluem no domínio.
* Certifique-se a ligação do sistema de TMS no local.
* Configure as rotas de transporte, grupos e as camadas como de costume.

No site a site ligados em vários locais cenários, a latência entre locais e o Azure ainda podem ser substanciais. Se nós seguir a sequência de transportar objetos por meio de sistemas de desenvolvimento e teste para produção ou pense sobre como aplicar transportes ou pacotes de suporte para os diferentes sistemas, percebe que, dependente na localização do diretório central de transporte, Alguns dos sistemas irão se deparar com latência elevada ler ou escrever dados no diretório central de transporte. A situação é semelhante a configurações de paisagem SAP em que os diferentes sistemas encontram-se distribuídas por meio de centros de dados diferentes com substancial distância entre os centros de dados.

Para contornar essa latência e ter os sistemas a trabalhar rapidamente em ler ou escrever ou do diretório de transporte, pode configurar dois domínios de transporte STMS (um para um com os sistemas no Azure e no local e vincular os domínios de transporte. Verifique esta documentação que explica os princípios por trás desse conceito no TMS SAP: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/38dd924eb711d182bf0000e829fbfe/frameset.htm>.

Como:

* Configurar um domínio de transporte em cada localização (no local e o Azure) com a transação STMS <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Ligar os domínios com uma ligação de domínio e confirme a ligação entre os dois domínios.
  <http://help.sap.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/content.htm>
* Distribua a configuração para o sistema ligado.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>Tráfego RFC entre instâncias do SAP localizado no Azure e no local (em vários locais)
Tráfego RFC entre sistemas que estão no local e no Azure tem de trabalhar. Para configurar uma transação de chamada de ligação SM59 num sistema de origem em que precisa definir uma ligação de RFC para o sistema de destino. A configuração é semelhante à configuração padrão de uma ligação de RFC.

Partimos do princípio de que o cenário em vários locais, as VMs que são de execução sistemas SAP que precisam de comunicar entre si no mesmo domínio. Portanto, a configuração de uma ligação de RFC entre os sistemas SAP não divergir os passos de configuração e as entradas em cenários no local.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Existem fileshares ao aceder de locais de instâncias do SAP localizadas no Azure ou vice versa
Instâncias SAP localizadas no Azure precisam de aceder a partilhas de ficheiros que estão dentro das instalações empresariais. Além disso, as instâncias SAP no local tem de aceder a partilhas de ficheiros que estão localizadas no Azure. Para ativar as partilhas de ficheiros tem de configurar as permissões e as opções de partilha no sistema local. Certifique-se abrir as portas na ligação VPN ou ExpressRoute entre o Azure e o seu datacenter.

## <a name="supportability"></a>Suportabilidade
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Solução de monitorização para SAP do Azure
Para ativar a monitorização dos sistemas SAP críticos para a atividade no Azure SAP ferramentas SAPOSCOL ou SAP anfitrião de agente de monitorização obter dados fora do host do serviço de Máquina Virtual do Azure através de uma extensão de monitorização do Azure para SAP. Uma vez que as demandas da SAP foram muito específicas para aplicações SAP, a Microsoft decidiu não genericamente implementar a funcionalidade necessária para o Azure, mas deixá-lo para os clientes implementar as configurações de monitorização de componentes necessários e seus Virtual Máquinas em execução no Azure. No entanto, gestão de implementação e o ciclo de vida dos componentes de monitorização irá ser automatizado principalmente pelo Azure.

#### <a name="solution-design"></a>Design da solução
A solução desenvolvida para ativar a monitorização de SAP baseia-se a arquitetura do agente de VM do Azure e a estrutura de extensão. A idéia da estrutura de extensão e o agente de VM do Azure é permitir a instalação de aplicações de software disponíveis na Galeria de extensão de VM do Azure dentro de uma VM. A idéia de princípio por trás desse conceito é permitir que (em casos como a extensão de monitorização do Azure para SAP), a implementação de funcionalidade especial numa VM e a configuração desse software no momento da implementação.

O "Azure VM Agent" que permite o processamento de extensões de VM do Azure específicas dentro da VM é injetado no Windows VMs por predefinição aquando da criação de VM no portal do Azure. Em caso de SUSE, Red Hat ou Oracle Linux, o agente da VM já faz parte da imagem do Azure Marketplace. No caso de um poderia carregar uma VM do Linux no local para o Azure tem de ser instalado manualmente o agente da VM.

Os blocos modulares básicos da solução de monitorização no Azure para SAP é assim:

![Componentes de extensão do Microsoft Azure][planning-guide-figure-2400]

Conforme mostrado no diagrama de bloco acima, uma parte da solução de monitorização para o SAP está alojada na imagem de VM do Azure e Galeria de extensão do Azure, que é um repositório replicado globalmente, que é gerido pelo Azure Operations. É da responsabilidade da Equipe do SAP/MS conjunta trabalhar em relação à implementação do Azure SAP para trabalhar com as operações do Azure para publicar as novas versões da extensão de monitorização do Azure para SAP.

Ao implementar uma nova VM do Windows, é adicionado automaticamente o agente da VM do Azure na VM. A função deste agente é coordenar o carregamento e a configuração das extensões do Azure para a monitorização de sistemas do SAP NetWeaver. Para VMs do Linux o agente da VM do Azure já faz parte da imagem do SO do Azure Marketplace.

No entanto, há uma etapa que ainda precisa ser executado pelo cliente. Esta é a ativação e a configuração de recolha de desempenho. O processo relacionados com a configuração é automatizado por um script do PowerShell ou comando da CLI. O script do PowerShell pode ser baixado no Azure Microsoft Script Center, conforme descrito no [guia de implementação][deployment-guide].

A arquitetura geral da solução de monitorização do Azure para SAP é semelhante a:

![Solução de monitorização para o SAP NetWeaver do Azure][planning-guide-figure-2500]

**Os procedimentos exatos e para obter passos detalhados de utilizar estes cmdlets do PowerShell ou comando da CLI durante as Implantações, siga as instruções na [guia de implementação][deployment-guide].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Integração do Azure localizados a instância SAP em SAProuter
Instâncias SAP em execução no Azure tem de ser acessível a partir do SAProuter também.

![Ligação de rede do roteador de SAP][planning-guide-figure-2600]

Um SAProuter permite a comunicação de TCP/IP entre sistemas participantes se não houver nenhuma ligação direta do IP. Isso fornece a vantagem que não é necessária nenhuma ligação de ponto a ponto entre os parceiros de comunicação no nível de rede. O SAProuter está à escuta na porta 3299 por predefinição.
Para ligar instâncias de SAP através de um SAProuter terá de fornecer o nome de cadeia de caracteres e anfitrião SAProuter com qualquer tentativa de ligação.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java
Até aqui o foco do documento tiver sido SAP NetWeaver em geral ou do SAP NetWeaver ABAP de pilha. Nesta secção pequeno, considerações específicas para a pilha de SAP Java estão listadas. Uma das aplicações SAP NetWeaver Java exclusivamente com base em mais importantes é o Portal da empresa de SAP. Outros SAP NetWeaver com base em aplicações como o PI da SAP e SAP solução Manager utilizam o SAP NetWeaver ABAP e pilhas de Java. Portanto, certamente há a necessidade de considerar os aspectos específicos relacionados com a pilha do SAP NetWeaver Java.

### <a name="sap-enterprise-portal"></a>Portal da empresa SAP
A configuração de um Portal de SAP numa máquina Virtual não diferem de uma instalação local no se estiver a implementar em cenários em vários locais. Uma vez que o DNS é feito no local, as definições de porta das instâncias individuais podem ser feitas como configurado no local. As recomendações e restrições descritas até aqui neste documento aplicam-se para uma aplicação, como SAP Enterprise Portal ou a pilha de Java do SAP NetWeaver em geral.

![Portal SAP expostos][planning-guide-figure-2700]

Um cenário de implementação especial por alguns clientes é a exposição direta do Portal da empresa do SAP para a Internet, ao passo que o anfitrião de máquina virtual está ligado à rede da empresa através do túnel VPN de site a site ou ExpressRoute. Para um cenário, terá de certificar-se de que as portas específicas são abertos e não está bloqueado por grupo de segurança de rede ou firewall. A mecânica do mesmo têm de ser aplicada quando pretende ligar a uma instância de Java de SAP no local num cenário de apenas na Cloud.

O URI de portal inicial é http (s):`<Portalserver`>: 5XX00/irj onde a porta é formada por mais de 50000 (Systemnumber?? 100). É o sistema de URI do SAP portal padrão 00 `<dns name`>.`<azure region` >.Cloudapp.azure.com:PublicPort/irj. Para obter mais detalhes, dê uma olhada <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.

![Configuração do ponto final][planning-guide-figure-2800]

Se desejar personalizar o URL de e/ou as portas do Portal da empresa do SAP, verifique esta documentação:

* [URL do Portal de alteração](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Alterar os números de porta predefinidos, números de porta do Portal](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Elevada disponibilidade (HA) e recuperação de desastre (DR) para o SAP NetWeaver em máquinas de virtuais do Azure em execução
### <a name="definition-of-terminologies"></a>Definição de terminologias
O termo **elevada disponibilidade (HA)** é geralmente relacionadas a um conjunto de tecnologias que minimiza as interrupções de IT, proporcionando a continuidade dos serviços de TI por meio de redundantes, tolerante a falhas ou ativação pós-falha protegido de componentes dentro de **mesmo** Centro de dados. No nosso caso, numa região do Azure.

**Recuperação após desastre (DR)** também visa minimizar interrupções de serviços IT e sua recuperação, mas na **diferentes** centros de dados, que são normalmente localizado de centenas de quilômetros de distância. No nosso caso, normalmente, entre diferentes regiões do Azure na mesma região geopolítica ou conforme estabelecida por si como um cliente.

### <a name="overview-of-high-availability"></a>Descrição geral de elevada disponibilidade
É possível separar a discussão sobre a disponibilidade elevada SAP no Azure em duas partes:

* **Elevada disponibilidade da infraestrutura do Azure**, por exemplo HA de (VMs) de computação, rede, armazenamento, etc. e a seus benefícios para aumentar a disponibilidade de aplicações SAP.
* **Disponibilidade elevada da aplicação SAP**, por exemplo os componentes de software do HA de SAP:
  * Servidores de aplicações SAP
  * Instância do SAP ASCS/SCS
  * Servidor de DB

e como pode ser combinada com a infraestrutura do Azure HA.

Elevada disponibilidade do SAP no Azure tem algumas diferenças em comparação com elevada disponibilidade do SAP num ambiente no local físico ou virtual. O documento seguinte do SAP descreve as configurações padrão de elevada disponibilidade de SAP em ambientes virtualizados no Windows: <http://scn.sap.com/docs/DOC-44415>. Não existe nenhum sapinst integrada SAP-HA configuração para Linux, como ele existe para o Windows. Em relação a HA do SAP no local para Linux encontrar mais informações aqui: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Elevada disponibilidade da infraestrutura do Azure
Atualmente, não existe um SLA de VM única de 99,9%. Para ter uma idéia de como a disponibilidade de uma VM única poderá parecer pode simplesmente criar o produto dos SLA do Azure disponíveis diferentes: <https://azure.microsoft.com/support/legal/sla/>.

A base para o cálculo é 30 dias por mês, ou 43200 minutos. Por conseguinte, 0,05% tempo de inatividade corresponde a 21.6 minutos. Como sempre, a disponibilidade dos serviços diferentes serão multiplicar da seguinte forma:

(Serviço de disponibilidade n. º 1/100) * (serviço de disponibilidade n. º 2/100) * (serviço de disponibilidade #3/100) 

Como:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 ou um de 99.75% de disponibilidade geral.

#### <a name="virtual-machine-vm-high-availability"></a>Elevada disponibilidade de máquina virtual (VM)
Existem dois tipos de eventos de plataforma do Azure que podem afetar a disponibilidade das suas máquinas virtuais: a manutenção e a manutenção não planeada.

* Eventos de manutenção planeada são atualizações periódicas feitas pela Microsoft na plataforma Azure subjacente para melhorar a confiabilidade, desempenho e segurança da infraestrutura de plataforma que suas máquinas virtuais são executadas em geral.
* Eventos de manutenção não planeada ocorrerem quando o hardware ou a infraestrutura física subjacente à máquina virtual falharam de alguma forma. Isto pode incluir falhas de rede local, falhas de disco local ou outras falhas estruturais. Quando é detetada uma falha deste tipo, a plataforma do Azure será migrado automaticamente a máquina virtual do servidor físico mau estado de funcionamento que aloja a máquina virtual para um servidor físico em bom estado. Estes eventos são raros, mas também podem provocar o reinício da máquina virtual.

Podem encontrar mais detalhes nesta documentação: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Redundância de armazenamento do Azure
Os dados na sua conta de armazenamento do Microsoft Azure são sempre replicados para garantir a durabilidade e elevada disponibilidade, cumprindo o SLA de armazenamento do Azure, mesmo diante de falhas de hardware transitórias.

Uma vez que o armazenamento do Azure é manter três imagens dos dados por predefinição, a RAID5 ou RAID1 em vários discos do Azure não são necessários.

Podem encontrar mais detalhes neste artigo: <http://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-sap-applications"></a>Utilização de reinício VM de infraestrutura do Azure para alcançar a elevada disponibilidade de aplicações SAP
Se decidir não utilizar funcionalidades como o Clustering de ativação pós-falha de servidor ao Windows ' (WSFC) ou Pacemaker no Linux (atualmente apenas suportado para o SLES 12 e superior), reiniciar de VM do Azure é utilizada para proteger um sistema SAP contra períodos de indisponibilidade planeado e imprevisto do Azure infraestrutura de servidor físico e geral da plataforma Azure subjacente.

> [!NOTE]
> É importante mencionar que principalmente reinício de VM do Azure protege as VMs e não aplicações. Reinicie a VM não oferece elevada disponibilidade para aplicações SAP, mas oferece um certo nível de disponibilidade da infraestrutura e, portanto, indiretamente maior disponibilidade dos sistemas SAP. Também não há nenhum SLA para o tempo que será necessário reiniciar uma VM após uma falha de anfitrião não planeada ou não planeada. Por conseguinte, este método de elevada disponibilidade não é adequado para componentes críticos de um sistema SAP como (A) SCS ou DBMS.
>
>

Outro elemento de infra-estrutura importante para elevada disponibilidade é um armazenamento. Por exemplo o SLA de armazenamento do Azure é uma disponibilidade de 99,9%. Se um implementa todas as VMs com os respetivos discos para uma conta de armazenamento do Azure única, potenciais armazenamento do Azure indisponibilidade fará com que a indisponibilidade de todas as VMs que são colocadas nessa conta de armazenamento do Azure e também todos os SAP componentes em execução nessas VMs.  

Em vez de colocar todas as VMs numa única conta de armazenamento do Azure, também pode utilizar o armazenamento dedicado contas para cada VM e, dessa forma aumentam a disponibilidade geral de VM e a SAP da aplicação ao utilizar várias contas de armazenamento do Azure independentes.

Managed Disks do Azure são automaticamente colocados no domínio de falha da máquina virtual que estão ligados a. Se colocar duas máquinas virtuais na disponibilidade definir e utilizar Managed Disks, a plataforma se encarregará de distribuição de Managed Disks em diferentes domínios de falha também. Se planeia utilizar o armazenamento Premium, é altamente recomendável utilizar discos de gerir também.

Um exemplo de arquitetura de um sistema SAP NetWeaver, que utiliza as contas HA e armazenamento da infraestrutura do Azure pode ter um aspeto semelhante a esta:

![Utilização de HA para assegurar disponibilidade mais elevada do aplicativo SAP de infraestrutura do Azure][planning-guide-figure-2900]

Um exemplo de arquitetura de um sistema SAP NetWeaver, que utiliza a infraestrutura do Azure HA e Managed Disks poderia ter esta aparência:

![Utilização de HA para assegurar disponibilidade mais elevada do aplicativo SAP de infraestrutura do Azure][planning-guide-figure-2901]

Para componentes críticos de SAP obtivemos o seguinte até agora:

* Elevada disponibilidade dos servidores de aplicações SAP (AS)

  Instâncias de servidor de aplicações SAP são componentes redundantes. Cada SAP como instância for implementada na sua própria VM, o que está a executar de forma diferente falhas do Azure e o domínio de atualização (consulte capítulos [domínios de falha] [ planning-guide-3.2.1] e [os domínios de atualização] [ planning-guide-3.2.2]). Isso é garantido, utilizando conjuntos de disponibilidade do Azure (consulte o capítulo [conjuntos de disponibilidade do Azure][planning-guide-3.2.3]). Potencial indisponibilidade planeada ou não planeada de um índice de falhas do Azure ou a atualizar domínio fará com que a indisponibilidade de um número restrito de VMs com AS respetivas SAP instâncias.

  Cada SAP como instância é colocada na sua própria conta de armazenamento do Azure - indisponibilidade potencial de uma conta de armazenamento do Azure fará com que indisponibilidade de apenas uma VM com AS respetivas SAP instância. No entanto, lembre-se de que existe um limite de contas de armazenamento do Azure dentro de uma subscrição do Azure. Para garantir o início automático de instância (A) SCS após o reinício VM, certifique-se definir o parâmetro de início automático na instância (A) SCS iniciar perfil descrito no capítulo [utilizar o início automático para as instâncias do SAP][planning-guide-11.5].
  Leia também capítulo [elevada disponibilidade para servidores de aplicações SAP] [ planning-guide-11.4.1] para obter mais detalhes.

  Mesmo que use Managed Disks, esses discos também são armazenados numa conta de armazenamento do Azure e podem não estar disponíveis em caso de uma falha no armazenamento.

* *Superior* instância de disponibilidade do SAP (A) SCS

  Aqui vamos utilizar o reinício de VM do Azure para proteger a VM com a instância do SAP (A) SCS instalada. No caso de um período de indisponibilidade não planeado ou não planeada do Azure servidores, as VMs serão reiniciadas noutro servidor disponível. Como mencionado anteriormente, principalmente reinício de VM do Azure protege as VMs e não de aplicativos, nesta instância do caso o (A) SCS. Por meio de reiniciar a VM que irá aceder indiretamente maior disponibilidade da instância do SAP (A) SCS. Para garantir o início automático de instância (A) SCS após o reinício VM, certifique-se definir o parâmetro de início automático num (A) perfil de início de instância SCS descrito no capítulo [utilizar o início automático para as instâncias do SAP][planning-guide-11.5]. Isso significa que a (A) SCS de instância como um único ponto de falha (SPOF) em execução numa única VM vai ser o fator determinative a disponibilidade de ambiente SAP todo.

* *Superior* disponibilidade de servidor DBMS

  Aqui, assim como o caso de utilização de instância do SAP (A) SCS, utiliza-se o reinício de VM do Azure para proteger a VM com software instalado do DBMS, e podemos alcançar a elevada disponibilidade do software do DBMS por meio de reiniciar a VM.
  DBMS em execução numa única VM também é um SPOF e é o fator determinative a disponibilidade de ambiente SAP todo.

### <a name="sap-application-high-availability-on-azure-iaas"></a>Disponibilidade elevada de aplicações SAP no IaaS do Azure
Para obter total SAP sistema elevada disponibilidade, é necessário proteger os componentes do sistema SAP todos críticos, para servidores de aplicações SAP com redundância de exemplo e componentes exclusivos (por exemplo único ponto de falha), como a instância do SAP (A) SCS e DBMS.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Elevada disponibilidade para servidores de aplicações SAP
Para as instâncias de servidores/caixa de diálogo de aplicação SAP não é necessário pensar numa solução específica de elevada disponibilidade. Elevada disponibilidade é simplesmente obtida pela redundância e, portanto, ter canso em máquinas virtuais diferentes. Eles devem todos ser colocados no mesmo Azure conjunto de disponibilidade para evitar que as VMs podem ser atualizadas ao mesmo tempo durante o período de indisponibilidade de manutenção planeada. A funcionalidade básica que baseia-se nas diferentes de atualização e domínios de falhas dentro de uma unidade de escala do Azure já foi introduzida no capítulo [domínios de atualização][planning-guide-3.2.2]. Conjuntos de disponibilidade do Azure eram apresentados no capítulo [conjuntos de disponibilidade do Azure] [ planning-guide-3.2.3] deste documento.

Existe um número infinito de falhas e domínios de atualização de mensagens em fila que pode ser utilizado por um conjunto de disponibilidade do Azure dentro de uma unidade de escala do Azure. Isso significa que colocar um número de VMs num conjunto de disponibilidade, cedo ou tarde mais do que uma que VM acaba no mesmo domínio de atualização ou falhas.

A implementar algumas instâncias de servidor de aplicações SAP em suas VMs dedicadas e partindo do princípio de que temos cinco domínios de atualização, surge a imagem seguinte no final. O número máximo real de domínios de falha e de atualização num conjunto de disponibilidade podem ser alteradas no futuro:

![HA de servidores de aplicações SAP no Azure][planning-guide-figure-3000]

Podem encontrar mais detalhes nesta documentação: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="high-availability-for-the-sap-ascs-instance-on-windows"></a>Elevada disponibilidade para a instância do SAP (A) SCS no Windows
Cluster de ativação pós-falha de servidor do Windows (WSFC) é uma solução mais usada para proteger a instância do SAP (A) SCS. Ele também está integrado em sapinst na forma de uma instalação"HA". No momento da infraestrutura do Azure não é capaz de fornecer a funcionalidade para configurar o Cluster de ativação pós-falha do Windows Server necessárias da mesma forma como ele tem feito no local.

A partir de Janeiro de 2016 a plataforma de cloud do Azure com o sistema operativo do Windows não fornece a possibilidade de utilizar um volume partilhado de cluster num disco partilhado entre duas VMs do Azure.

No entanto, uma solução válida é a utilização do software de terceiros 3rd que fornece um volume partilhado através da replicação síncrona e transparente de disco que pode ser integrada de WSFC. Essa abordagem implica que o nó de cluster ativo é capaz de aceder uma das cópias de disco num ponto no tempo. A partir de Janeiro de 2016 este HA configuração é suportada para proteger a instância do SAP (A) SCS no Windows SO convidado em VMs do Azure em combinação com o software de terceiros do 3rd o SIOS DataKeeper.

A solução de SIOS DataKeeper fornece um recurso de cluster de disco partilhado para Clusters de ativação pós-falha do Windows, fazendo com que:

* Um VHD do Azure adicionais anexados a cada uma das máquinas virtuais (VMs) que estão numa configuração de Cluster do Windows
* O SIOS DataKeeper Cluster Edition em execução em ambos os nós VM
* O SIOS DataKeeper Cluster Edition configurado de forma a que forma síncrona Espelha o conteúdo do VHD adicional de ter ligado ao volume de VMs de origem para VHD adicional anexados volume de VM de destino.
* O SIOS DataKeeper é abstrair os volumes de locais de origem e de destino e apresentando-os ao Cluster de ativação pós-falha do Windows como um único disco partilhado.

Pode encontrar todos os detalhes sobre como instalar um Cluster de ativação pós-falha do Windows com o SIOS DataKeeper e a SAP na [Clustering SAP ASCS instância com o Cluster de ativação pós-falha do Windows Server no Azure com o SIOS DataKeeper] [ ha-guide-classic] white paper.

#### <a name="high-availability-for-the-sap-ascs-instance-on-linux"></a>Elevada disponibilidade para a instância do SAP (A) SCS no Linux
A partir de Dezembro de 2015 também não existe nenhum equivalente ao disco partilhado WSFC para VMs do Linux no Azure. Soluções alternativas, utilizar o software de 3rd, como o SIOS para Windows não são validadas ainda para executar o SAP em Linux no Azure.

#### <a name="high-availability-for-the-sap-database-instance"></a>Elevada disponibilidade para a instância de base de dados do SAP
Da SAP DBMS HA configuração típica baseia-se em duas VMs do DBMS em que a funcionalidade de elevada disponibilidade do DBMS é utilizada para replicar os dados da instância ativa do DBMS para a segunda VM para uma instância do DBMS passiva.

Funcionalidade de recuperação após desastre e elevada disponibilidade para DBMS em geral são descritos os DBMS também o mais específicos no [guia de implementação de DBMS][dbms-guide].

#### <a name="end-to-end-high-availability-for-the-complete-sap-system"></a>Elevada disponibilidade de ponto-a-ponto para o sistema SAP concluída
Aqui estão dois exemplos de um SAP NetWeaver HA arquitetura completa no Azure - um para Windows e outro para Linux.

Apenas os discos não geridos: os conceitos, conforme explicado abaixo poderão ter de ser um pouco comprometidos ao implementar muitos sistemas SAP e o número de VMs implementadas está a exceder o limite máximo de contas de armazenamento por subscrição. Nesses casos, VHDs de VMs tem de ser combinados numa mesma conta de armazenamento. Normalmente, seria fazê-lo ao combinar a camada de aplicativo de VHDs do SAP VMs de diferentes sistemas SAP.  Também podemos combinadas de diferentes VHDs de diferentes DBMS VMs de diferentes sistemas SAP numa conta de armazenamento do Azure. Mantendo os limites IOPS das contas de armazenamento do Azure em mente (<https://azure.microsoft.com/documentation/articles/storage-scalability-targets>)


##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] HA no Windows
![Arquitetura de HA do SAP NetWeaver aplicativos com o SQL Server no Azure IaaS][planning-guide-figure-3200]

As construções do Azure seguintes são utilizadas para o sistema SAP NetWeaver, para minimizar o impacto por problemas de infraestrutura e aloje a aplicação de patches:

* O sistema completo é implementado no Azure (é necessário - camada do DBMS, (A) a instância SCS e a camada de aplicativo completo tem de executar na mesma localização).
* O sistema completo é executado dentro de uma subscrição do Azure (obrigatória).
* O sistema completo é executado dentro de uma Azure Virtual Network (obrigatório).
* A separação das VMs de um sistema SAP em três conjuntos de disponibilidade é possível até mesmo com todas as VMs que pertencem à mesma rede Virtual.
* Todas as VMs a executar instâncias do DBMS de um sistema SAP estão num conjunto de disponibilidade. Partimos do princípio de que existe mais do que uma VM a executar instâncias do DBMS por sistema desde disponibilidade elevada nativa do DBMS, recursos são usados, como o AlwaysOn do SQL Server ou o Oracle Data Guard.
* Todas as VMs a executar instâncias do DBMS usam sua própria conta de armazenamento. Arquivos de dados e de log do DBMS são replicados a partir de uma conta de armazenamento, para outra conta de armazenamento com as funções de elevada disponibilidade do DBMS que sincronizam os dados. Indisponibilidade de uma conta de armazenamento fará com que a indisponibilidade de um nó de cluster do Windows de SQL, mas não o serviço inteiro do SQL Server.
* Todas as VMs a executar a instância de um sistema SAP (A) SCS estão num conjunto de disponibilidade. Um Cluster de ativação pós-falha do servidor de Windows (WSFC) está configurado dentro dessas VMs para proteger a (A) a instância SCS.
* Todas as VMs a executar instâncias (A) SCS usam sua própria conta de armazenamento. (A) Ficheiros de instância SCS e pasta global de SAP são replicadas a partir de uma conta de armazenamento para outra conta de armazenamento com o SIOS DataKeeper replicação. Indisponibilidade de uma conta de armazenamento fará com que a indisponibilidade de um nó de cluster (A) SCS Windows, mas não o todo serviço (A) SCS.
* TODAS as VMs que representa a camada de servidor de aplicação SAP é um terceiro conjunto de disponibilidade.
* TODAS as VMs em execução em servidores de aplicação SAP usam sua própria conta de armazenamento. Indisponibilidade de uma conta de armazenamento fará com que a indisponibilidade de um servidor de aplicação SAP, onde AS outras SAP continua a ser executado.

A figura a seguir ilustra o cenário mesmo utilizar discos geridos.

![Arquitetura de HA do SAP NetWeaver aplicativos com o SQL Server no Azure IaaS][planning-guide-figure-3201]

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] HA no Linux
A arquitetura do SAP HA no Linux no Azure é basicamente igual de Windows, tal como descrito acima. Consulte a nota SAP [1928533] para obter uma lista das soluções de elevada disponibilidade suportadas.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Utilizar o início automático para as instâncias do SAP
SAP oferecido a funcionalidade para iniciar as instâncias SAP imediatamente após o início do sistema operacional dentro da VM. Os passos exatos documentados no artigo da Base de dados de SAP [1909114]. No entanto, a SAP não é recomendando para utilizar a definição mais uma vez que não é possível controlar pela ordem dos reinícios de instância, partindo do princípio de mais de uma VM de obteve afetado ou foi executada várias instâncias por VM. Supondo que um cenário típico do Azure de uma instância de servidor de aplicações SAP numa VM e o caso de uma única VM, eventualmente, obter reiniciado, o início automático não é realmente fundamental e pode ser ativado ao adicionar este parâmetro:

    Autostart = 1

Para o perfil de início da instância do SAP ABAP e/ou Java.

> [!NOTE]
> O parâmetro de início automático pode ter também algumas desvantagens. Em mais detalhes, o parâmetro aciona o início de uma instância de Java ou SAP ABAP quando o serviço do Windows/Linux relacionado da instância é iniciado. Certamente, o que acontece quando o sistema operacional é inicializado. No entanto, os reinícios de serviços SAP também são uma coisa comum para funcionalidades de gestão de ciclo de vida do Software SAP como soma ou outras atualizações ou atualiza. Estas funcionalidades não estão esperando por uma instância será reiniciado automaticamente em todas. Por conseguinte, o parâmetro de início automático deverá ser desativado antes de executar essas tarefas. O parâmetro de inicialização automática também não deve ser utilizado para as instâncias do SAP estão agrupados em cluster, como ASCS/SCS/CI.
>
>

Ver informações adicionais sobre a inicialização automática para SAP instâncias aqui:

* [Iniciar/parar SAP, juntamente com o seu servidor Unix iniciar/parar](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Iniciar e parar os agentes de gestão do SAP NetWeaver](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Como ativar automaticamente iniciar de base de dados HANA](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

### <a name="larger-3-tier-sap-systems"></a>Sistemas grandes de SAP de 3 camadas
Aspectos de elevada disponibilidade das configurações de SAP de 3 camadas tem já discutidos nas seções anteriores. Mas e quanto a sistemas em que os requisitos de servidor do DBMS são demasiado grandes para o localizados no Azure, mas a camada de aplicação SAP poderiam ser implementados no Azure?

#### <a name="location-of-3-tier-sap-configurations"></a>Localização das configurações de SAP de 3 camadas
Não é suportada para dividir a camada de aplicativo em si ou a aplicação e o DBMS de camada entre no local e o Azure. Um sistema SAP é implementado completamente no local ou no Azure. Ele também não é suportado ter alguns dos servidores de aplicações em execução no local e outros no Azure. Que é o ponto de partida da discussão. Nós também não estiver sustentando para que os componentes do DBMS de um sistema SAP e a camada de servidor de aplicação SAP implementado em duas regiões diferentes do Azure. Por exemplo DBMS no Oeste dos E.U.A. e a SAP camada de aplicação na região E.U.A. Central. Motivo para não oferecer suporte a essas configurações é a sensibilidade de latência da arquitetura do SAP NetWeaver.

No entanto, ao longo do ano passado parceiros desenvolvidos conjunta localizações de Datacenter para regiões do Azure. Estas localizações conjunta são, muitas vezes, muito próximos os dados do Azure físicos centros de dentro de uma região do Azure. A distância de curta e a ligação dos ativos em colocalização através do ExpressRoute para o Azure podem resultar numa latência de que é inferior a 2ms. Nesses casos, para localizar a camada do DBMS (incluindo o armazenamento SAN/NAS) em tal uma localização conjunta e o SAP camada de aplicação no Azure é possível. A partir de Dezembro de 2015, não temos todas as implementações como esse. Mas diferentes clientes com implementações de aplicações não pertencentes à SAP estão a utilizar já dessas abordagens.

### <a name="offline-backup-of-sap-systems"></a>Sistemas de cópia de segurança do SAP offline
Dependendo da configuração de SAP escolhido lá (camada 2 ou 3 camadas) pode ser uma necessidade de fazer cópias de segurança. O conteúdo do próprio adição VM ter uma cópia de segurança da base de dados. As cópias de segurança relacionados com o DBMS devem ser feito com métodos de base de dados. Uma descrição detalhada das bases de dados diferentes, pode ser encontrado na [Guia do DBMS][dbms-guide]. Por outro lado, os dados do SAP podem ser copiados de um modo offline (incluindo o conteúdo de base de dados também) conforme descrito nesta secção ou online, tal como descrito na secção seguinte.

A cópia de segurança offline basicamente exigiria um encerramento da VM através do portal do Azure e uma cópia do disco VM base além discos todos ligados à VM. Isso seria preservar um ponto na imagem no tempo da VM e o respetivo disco associado. Recomenda-se para copiar as cópias de segurança para uma conta de armazenamento do Azure diferente. Por conseguinte, o procedimento descrito no capítulo [copiar discos entre contas de armazenamento do Azure] [ planning-guide-5.4.2] deste documento seriam aplicáveis.
Além do encerramento com o portal do Azure, um pode também fazê-lo através do Powershell ou CLI, conforme descrito aqui: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Uma restauração de estado consistiria a eliminar a VM base, bem como os discos originais da base VM e montado discos, copiar os discos guardados de volta para o grupo de conta de armazenamento ou o recurso original para discos geridos e, em seguida, voltar a implementar o sistema.
Este artigo mostra um exemplo de como criar um script este processo no Powershell: <http://www.westerndevs.com/azure-snapshots/>

Certifique-se instalar uma nova licença do SAP, uma vez que o restauro de uma cópia de segurança VM, tal como descrito acima cria uma nova chave de hardware.

### <a name="online-backup-of-an-sap-system"></a>Cópia de segurança online de um sistema SAP
Cópia de segurança do DBMS é executada, com métodos específicos do DBMS, conforme descrito no [Guia do DBMS][dbms-guide].

Outras VMs no sistema SAP podem ser copiadas de uso da funcionalidade de Backup de Máquina Virtual do Azure. Cópia de segurança de Máquina Virtual do Azure foi introduzida no início de 2015 e enquanto isso, é um método padrão para fazer backup de uma VM completa no Azure. Cópia de segurança do Azure armazena as cópias de segurança no Azure e permite que um restauro de uma VM.

> [!NOTE]
> A partir de Dezembro de 2015 com cópia de segurança de VM não manter o ID exclusivo da VM que é utilizada para SAP de licenciamento. Isso significa que um restauro a partir de uma cópia de segurança VM requer a instalação de uma nova chave de licença do SAP como à VM restaurada é considerada como uma nova VM e não um substituto do antigo que foi guardado.
>
> ![Windows][Logo_Windows] Windows
>
> Teoricamente, as VMs que bases de dados de execução podem ser uma cópia de segurança de maneira consistente também se o sistema do DBMS suporta o VSS do Windows (serviço de cópia de sombra de volumes <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx>) como, por exemplo, SQL Server faz.
> No entanto, lembre-se de que com base em cópias de segurança de VM do Azure restaura o ponto no tempo de bases de dados não são possíveis. Por isso, recomenda-se efetuar cópias de segurança de bases de dados com a funcionalidade do DBMS em vez de depender de cópia de segurança do Azure VM.
>
> Para se familiarizar com o Backup de Máquina Virtual do Azure, comece por aqui: <https://docs.microsoft.com/azure/backup/backup-azure-vms>.
>
> Outras possibilidades são usar uma combinação do Microsoft Data Protection Manager instalado numa VM do Azure e cópia de segurança do Azure para bases de dados de cópia de segurança/restauro. Podem encontrar mais informações aqui: <https://docs.microsoft.com/azure/backup/backup-azure-dpm-introduction>.  
>
> ![Linux][Logo_Linux] Linux
>
> Não existe nenhum equivalente para o Windows VSS no Linux. Por isso apenas as cópias de segurança consistente com ficheiros são cópias de segurança possíveis, mas não consistentes. A cópia de segurança do SAP DBMS deve ser feita usando a funcionalidade do DBMS. O sistema de ficheiros que inclui os dados relacionados com o SAP pode ser guardado, por exemplo, usando tar conforme descrito aqui: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Azure como site de DR para cenários SAP de produção
Desde Mid 2014, extensões para vários componentes do Hyper-V, System Center e do Azure permitem a utilização do Azure como site de DR para as VMs em execução no local com base no Hyper-V.

Um blog com detalhes sobre como implementar esta solução foi documentado aqui: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>.

## <a name="summary"></a>Resumo
Os pontos principais de elevada disponibilidade para os sistemas SAP no Azure são:

* No momento, o ponto de falha único SAP não pode estar protegido exatamente da mesma forma como ele pode ser feito em implementações no local. O motivo é que ainda não não possível criar clusters no Azure sem o uso do software de terceiros 3º de disco partilhados.
* Para a camada do DBMS terá de utilizar a funcionalidade do DBMS que não se baseiam na tecnologia de cluster de disco partilhado. Detalhes estão documentados no [Guia do DBMS][dbms-guide].
* Para minimizar o impacto dos problemas em domínios de falha na manutenção de infraestrutura ou o anfitrião do Azure, deve usar conjuntos de disponibilidade do Azure:
  * É recomendado ter um conjunto de disponibilidade para a camada de aplicação SAP.
  * É recomendado ter um conjunto de disponibilidade separados para a camada do SAP DBMS.
  * Não é recomendado para aplicar a mesmo conjunto de disponibilidade para VMs de diferentes sistemas SAP.
  * Recomenda-se para utilizar Premium Managed Disks.
* Para fins de cópia de segurança de camada do SAP DBMS, verifique os [Guia do DBMS][dbms-guide].
* Backup de instâncias de caixa de diálogo de SAP não faz muito sentido, uma vez que é normalmente mais rápido voltar a implementar instâncias de caixa de diálogo simples.
* Cópia de segurança da VM que contém o diretório global do sistema SAP e com o mesmo todos os perfis de diferentes instâncias, faz sentido e deve ser realizada com a cópia de segurança do Windows ou, por exemplo, tar no Linux. Uma vez que existem diferenças entre o Windows Server 2008 (R2) e Windows Server 2012 (R2), que o torna mais fácil de criar cópias de segurança com o Windows Server mais recentes versões, recomendamos executar o Windows Server 2012 (R2) como sistema operativo Windows.

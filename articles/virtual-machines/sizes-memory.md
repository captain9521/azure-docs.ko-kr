---
title: Azure VM 크기 - 메모리 | Microsoft Docs
description: Azure의 가상 머신에 사용할 수 있는 다양한 메모리 최적화 크기를 나열합니다. 이 시리즈의 크기에 대한 스토리지 처리량 및 네트워크 대역폭뿐만 아니라 vCPU, 데이터 디스크 및 NIC의 수에 대한 정보를 제공합니다.
services: virtual-machines
documentationcenter: ''
author: mimckitt
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
keywords: VM 격리, 격리된 VM, 격리, 격리됨
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: mimckitt
ms.openlocfilehash: 1e27ee2c045404d13b10945ac65484281b0cd967
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92019718"
---
# <a name="memory-optimized-virtual-machine-sizes"></a>메모리 최적화 가상 머신 크기

메모리 액세스에 최적화 된 VM 크기는 관계형 데이터베이스 서버, 중간에서 큰 캐시 및 메모리 내 분석에 적합 한 높은 메모리 대 CPU 비율을 제공 합니다. 이 문서에서는 이 그룹화에서 각 크기에 대한 스토리지 처리량 및 네트워크 대역폭뿐만 아니라 vCPU, 데이터 디스크 및 NIC의 수에 대한 정보를 제공합니다.

- 원래 D 시리즈의 후속판인 [Dv2 및 DSv2 시리즈](dv2-dsv2-series-memory.md)는 더 강력한 CPU가 특징입니다. Dv2 시리즈는 D 시리즈보다 약 35% 빠릅니다. Intel Turbo Boost Technology 2.0과 함께 Intel&reg; Xeon &reg; 8171M 2.1GHz(Skylake) 또는 Intel&reg; Xeon&reg; E5-2673 v4 2.3GHz(Broadwell) 또는 Intel&reg; Xeon&reg; E5-2673 v3 2.4GHz(Haswell)에서 실행됩니다. Dv2 시리즈는 D 시리즈와 메모리 및 디스크 구성이 같습니다.

    Dv2 및 DSv2 시리즈는 더 빠른 vCPU와 더 좋은 임시 스토리지 성능을 요구하거나 더 높은 메모리 요구량을 가진 애플리케이션에 이상적입니다. 이들은 많은 엔터프라이즈급 애플리케이션을 위한 강력한 조합을 제공합니다.

- [Eav4 및 Easv4 시리즈](eav4-easv4-series.md)는 최대 256MB의 L3 캐시를 갖춘 다중 스레드 구성에서 AMD의 2.35Ghz EPYC<sup>TM</sup> 7452 프로세서를 활용하여 대부분의 메모리 최적화 워크로드 실행 옵션을 늘립니다. Eav4 시리즈와 Easv4 시리즈는 Ev3 및 Esv3 시리즈와 동일한 메모리 및 디스크 구성을 가집니다.

- [Ev3 및 Esv3 시리즈](ev3-esv3-series.md)는 하이퍼 스레드 구성에서 Intel&reg; Xeon&reg; 8171M 2.1GHz(Skylake) 또는 Intel&reg; Xeon&reg; E5-2673 v4 2.3GHz(Broadwell) 프로세서를 사용하며, 대부분의 범용 워크로드에 더 나은 가치 제안을 제공하고 Ev3을 다른 대부분 클라우드의 범용 VM과 맞춥니다. 하이퍼 스레딩으로 이동하기 위해 디스크 및 네트워크 제한이 코어 단위로 조정되는 동안 메모리가 확장되었습니다(7GiB/vCPU에서 8GiB/vCPU로). Ev3는 D/Dv2 제품군의 상위 메모리 VM 크기에 대한 후속 시리즈입니다.

- [Ev4 및 Esv4 시리즈](ev4-esv4-series.md) 는 &reg; 하이퍼 스레드 구성의 두 번째 세대 Intel Xeon &reg; Platinum 8272Cl (케스케이드 Lake) 프로세서에서 실행 되며, 다양 한 메모리 집약적 엔터프라이즈 응용 프로그램에 적합 하 고 최대 504 GiB RAM에 적용 됩니다. [Intel &reg; 터보 부스트 기술 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html), [Intel &reg; Hyper-Threading 기술](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) 및 [INTEL &reg; 고급 벡터 확장 512 (intel AVX-512)을 제공](https://www.intel.com/content/www/us/en/architecture-and-technology/avx-512-overview.html)합니다. Ev4 및 Esv4 시리즈는 로컬 임시 디스크를 포함 하지 않습니다. 자세한 내용은  [로컬 임시 디스크가 없는 AZURE VM 크기](azure-vms-no-temp-disk.md)를 참조 하세요.

- [Edv4 및 Edsv4 시리즈](edv4-edsv4-series.md) 는 2 세대 Intel &reg; Xeon &reg; Platinum 8272Cl (Cascade Lake) 프로세서에서 실행 되며, 매우 큰 데이터베이스 또는 대용량의 메모리를 활용 하는 기타 응용 프로그램에 적합 합니다. 또한 이러한 VM 크기에는 대기 시간이 짧고 고속 로컬 저장소를 활용 하는 응용 프로그램에 대 한 빠르고 큰 로컬 SSD 저장소가 포함 됩니다. 3.4 g h z, [intel &reg; 터보 부스트 기술 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html), [Intel &reg; Hyper-Threading 기술](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) 및 [INTEL &reg; Advanced Vector Extensions 512 (Intel AVX-512)](https://www.intel.com/content/www/us/en/architecture-and-technology/avx-512-overview.html)의 모든 코어 터보 클록 속도를 제공 합니다.

- [M 시리즈](m-series.md)는 높은 vCPU 수(최대 128개의 vCPU)와 많은 양의 메모리(최대 3.8TiB)를 제공합니다. 높은 vCPU 수와 많은 양의 메모리를 활용하는 매우 큰 데이터베이스나 다른 애플리케이션에도 이상적입니다.

- [Mv2 시리즈](mv2-series.md)는 클라우드의 VM 중에서 가장 높은 vCPU 수(최대 416개의 vCPU)와 가장 큰 메모리(최대 11.4TiB)를 제공합니다. 높은 vCPU 수와 많은 양의 메모리를 활용하는 매우 큰 데이터베이스나 다른 애플리케이션에 이상적입니다.

Azure Compute는 특정 하드웨어 유형에서 격리되고 단일 고객 전용인 가상 머신 크기를 제공합니다. 이러한 가상 머신 크기는 규정 준수 및 규정 요구 사항과 같은 요소를 포함하는 작업에 대해 다른 고객으로부터 높은 수준의 격리가 필요한 작업에 가장 적합합니다. 고객은 [중첩된 가상 머신에 대한 Azure 지원](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)을 사용하여 이러한 격리된 가상 머신의 리소스를 보다 세분화할 수도 있습니다. 격리된 VM 옵션은 아래 가상 머신 제품군에 대한 페이지를 참조하세요.

## <a name="other-sizes"></a>기타 크기

- [범용](sizes-general.md)
- [컴퓨팅 최적화](sizes-compute.md)
- [Storage에 최적화](sizes-storage.md)
- [GPU에 최적화](sizes-gpu.md)
- [고성능 컴퓨팅](sizes-hpc.md)
- [이전 세대](sizes-previous-gen.md)

## <a name="next-steps"></a>다음 단계

[ACU(Azure 컴퓨팅 단위)](acu.md)가 Azure SKU 간의 Compute 성능을 비교하는 데 어떻게 도움을 줄 수 있는지 알아봅니다.

Azure에서 Vm의 이름을 지정 하는 방법에 대 한 자세한 내용은 [azure virtual machine 크기 명명 규칙](./vm-naming-conventions.md)을 참조 하세요.

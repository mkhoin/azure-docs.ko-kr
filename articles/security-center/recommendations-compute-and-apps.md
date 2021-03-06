---
title: 앱 & 컴퓨터에 대 한 Azure Security Center 권장 사항
description: 가상 컴퓨터, 컴퓨터, 웹 앱 및 App Service 환경을 보호 하는 데 도움이 되는 Azure Security Center의 보안 권장 사항입니다.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2019
ms.author: memildin
ms.openlocfilehash: c0cf5951daf004a0c805b688f08d1ccfa4679615
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74782165"
---
# <a name="compute-and-app-recommendations---reference-guide"></a>계산 및 앱 권장 사항-참조 가이드

이 문서에서는 다음 리소스 유형과 관련 하 여 Azure Security Center에서 볼 수 있는 권장 사항에 대 한 전체 목록을 제공 합니다.

* VM 및 컴퓨터
* VM 확장 집합
* Cloud Services
* 앱 서비스
* 컨테이너
* Compute 리소스

이러한 항목을 찾고 해결 하는 방법에 대 한 자세한 내용은 [여기](security-center-virtual-machine-protection.md)를 참조 하세요.

## 계산 및 앱 권장 사항<a name="compute-and-app-recs"></a>
|리소스 종류|보안 점수|권장 사항|설명|
|----|----|----|----|
|App Service|20|웹 애플리케이션에 HTTPS를 통해서만 액세스 가능|웹 애플리케이션의 액세스를 HTTPS 사용으로 제한합니다.|
|App Service|20|함수 앱에 HTTPS를 통해서만 액세스 가능|함수 앱의 액세스를 HTTPS 사용으로 제한합니다.|
|App Service|5|App Services의 진단 로그를 사용 하도록 설정 해야 합니다.|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다. |
|App Service|10|웹 애플리케이션에 대해 원격 디버깅을 해제해야 함|더 이상 사용할 필요가 없으면 웹 애플리케이션에 대해 디버깅을 해제합니다. 원격 디버깅에 함수 앱에서 열리는 인바운드 포트가 필요합니다.|
|App Service|10|함수 애플리케이션에 대해 원격 디버깅을 해제해야 함|더 이상 사용할 필요가 없는 경우 함수 앱에 대한 디버깅을 해제합니다. 원격 디버깅에 함수 앱에서 열리는 인바운드 포트가 필요합니다.|
|App Service|10|애플리케이션에 액세스하는 모든('*') 리소스를 허용하지 마십시오.| WEBSITE_LOAD_CERTIFICATES 매개 변수의 "" 설정을 허용하지 않습니다. 매개 변수를 “로 설정하는 것은 모든 인증서가 웹 애플리케이션 개인 인증서 저장소로 로드되는 것을 의미합니다. 사이트는 런타임 시 모든 인증서에 대한 액세스가 필요하지 않으므로 최소 권한의 원칙이 악용될 수 있습니다.|
|App Service|20|CORS에서 모든 리소스가 웹 애플리케이션에 액세스하도록 허용하지 않아야 함|필요한 도메인만 웹 애플리케이션과 상호 작용할 수 있도록 허용합니다. CORS(교차 원본 리소스 공유)는 웹 애플리케이션에 액세스하는 모든 도메인을 허용하지 않아야 합니다.|
|App Service|20|CORS에서 모든 리소스가 함수 앱에 액세스하도록 허용하지 않아야 함| 필요한 도메인만 함수 애플리케이션과 상호 작용할 수 있도록 허용합니다. CORS(교차 원본 리소스 공유)는 함수 애플리케이션에 액세스하는 모든 도메인을 허용하지 않아야 합니다.|
|Compute 리소스(Batch)|1|Batch 계정에서 메트릭 경고 규칙을 구성 해야 합니다.|Batch 계정에 메트릭 경고 규칙을 구성하고 메트릭 풀 삭제 완료 이벤트 및 풀 삭제 시작 이벤트를 사용하도록 설정합니다.|
|Compute 리소스(Service Fabric)|10|Service Fabric 클러스터는 클라이언트 인증에 대해서만 Azure Active Directory를 사용해야 합니다.|Service Fabric에서 Azure Active Directory를 통해서만 클라이언트 인증을 수행합니다.|
|Compute 리소스(Automation 계정)|5|Automation 계정 변수는 암호화 되어야 합니다.|중요한 데이터를 저장할 때 Automation 계정 변수 자산의 암호화를 사용하도록 설정합니다.|
|Compute 리소스(검색)|5|Search 서비스에 대 한 진단 로그 사용 감사|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다. |
|Compute 리소스(Service Bus)|5|Service Bus의 진단 로그를 사용 하도록 설정 해야 합니다.|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다. |
|Compute 리소스(스트림 분석)|5|Azure Stream Analytics의 진단 로그를 사용 하도록 설정 해야 합니다.|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다. |
|Compute 리소스(Batch)|5|Batch 계정에서 진단 로그 사용|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다. |
|Compute 리소스(이벤트 허브)|5|이벤트 허브의 진단 로그를 사용 하도록 설정 해야 합니다.|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다. |
|Compute 리소스(Logic Apps)|5|Logic Apps에서 진단 로그 사용|로그를 사용하도록 설정하고 최대 1년 간 보존합니다. 이렇게 하면 보안 인시던트가 발생하거나 네트워크가 손상된 경우 조사 목적으로 활동 내역을 다시 만들 수 있습니다. |
|Compute 리소스(Service Fabric)|15|Service Fabric에서 ClusterProtectionLevel 속성을 EncryptAndSign으로 설정|Service Fabric은 기본 클러스터 인증서를 사용하여 노드 간 통신을 위한 3단계 보호(None, Sign 및 EncryptAndSign)를 제공합니다. 모든 노드 간 메시지가 암호화되고 디지털로 서명될 수 있게 보호 수준을 설정합니다. |
|Compute 리소스(Service Bus)|1|Service Bus 네임스페이스에서 RootManageSharedAccessKey를 제외한 모든 권한 부여 규칙 제거 |Service Bus 클라이언트는 네임스페이스의 모든 큐 및 토픽에 대한 액세스를 제공하는 네임스페이스 수준 액세스 정책을 사용하지 않아야 합니다. 최소 권한 보안 모델에 맞게 큐 및 토픽에 대한 엔터티 수준의 액세스 정책을 만들어 특정 엔터티에만 액세스를 제공해야 합니다.|
|Compute 리소스(이벤트 허브)|1|RootManageSharedAccessKey를 제외한 모든 권한 부여 규칙은 Event Hub 네임 스페이스에서 제거 해야 합니다.|이벤트 허브 클라이언트는 네임스페이스의 모든 큐 및 토픽에 대한 액세스를 제공하는 네임스페이스 수준 액세스 정책을 사용하지 않아야 합니다. 최소 권한 보안 모델에 맞게 큐 및 토픽에 대한 엔터티 수준의 액세스 정책을 만들어 특정 엔터티에만 액세스를 제공해야 합니다.|
|Compute 리소스(이벤트 허브)|5|이벤트 허브 엔터티에 대 한 권한 부여 규칙을 정의 해야 합니다.|최소 액세스 권한을 부여하기 위해 이벤트 허브 엔터티에 대한 권한 부여 규칙을 감사합니다.|
|컴퓨터|50|머신에 모니터링 에이전트 설치|모니터링 에이전트를 설치하여 각 머신에서 데이터 수집, 업데이트 검사, 기준 검사 및 엔드포인트 보호를 사용하도록 설정합니다.|
|컴퓨터|50|구독에 대한 자동 프로비전 및 데이터 수집을 사용하도록 설정 |구독에 추가된 각 머신에서 데이터 수집, 업데이트 검사, 기준 검사 및 엔드포인트 보호를 사용하기 위해 구독의 머신에 대해 자동 프로비전 및 데이터 수집을 사용하도록 설정합니다.|
|컴퓨터|40|머신의 모니터링 에이전트 상태 문제 해결|완벽한 Security Center 보호를 위해 문제 해결 가이드의 지침에 따라 머신에서 모니터링 에이전트 문제를 해결합니다.| 
|컴퓨터|40|머신의 엔드포인트 보호 상태 문제 해결|완벽한 Security Center 보호를 위해 문제 해결 가이드의 지침에 따라 머신에서 모니터링 에이전트 문제를 해결합니다.|
|컴퓨터|40|머신의 검사 데이터 누락 문제 해결|가상 머신 및 컴퓨터에서 검사 데이터 누락 문제를 해결합니다. 머신의 검사 데이터 누락은 업데이트 검사, 기준 검사 같은 보안 평가 누락과 엔드포인트 보호 솔루션 검사 누락을 초래합니다.|
|컴퓨터|40|시스템 업데이트를 머신에 설치해야 합니다.|누락된 시스템 보안 및 중요 업데이트를 설치하여 Windows 및 Linux 가상 머신과 컴퓨터를 보호합니다.
|컴퓨터|15|웹 애플리케이션 방화벽 추가| WAF(웹 애플리케이션 방화벽) 솔루션을 배포하여 웹 애플리케이션을 보호합니다. |
|컴퓨터|40|클라우드 서비스 역할에 대한 OS 버전 업데이트|클라우드 서비스 역할의 OS(운영 체제) 버전을 OS 제품군의 사용 가능한 최신 버전으로 업데이트합니다.|
|컴퓨터|35|머신 보안 구성의 취약성을 수정해야 합니다.|머신의 보안 구성에서 취약성을 수정하여 공격으로부터 보호합니다.|
|컴퓨터|35|컨테이너의 보안 구성에서 취약성 수정|Docker가 설치된 머신의 보안 구성에서 취약성을 수정하여 공격으로부터 보호합니다.|
|컴퓨터|25|적응형 애플리케이션 제어 사용|애플리케이션 제어를 사용하도록 설정하여 Azure에 있는 VM에서 실행할 수 있는 애플리케이션을 제어합니다. 이렇게 하면 VM을 강화하여 맬웨어로부터 보호할 수 있습니다. Security Center는 기계 학습을 통해 각 VM에서 실행 중인 애플리케이션을 분석하고 이러한 인텔리전스를 사용하여 허용 목록 규칙을 적용할 수 있습니다. 이 기능은 애플리케이션 허용 목록의 구성 및 유지 관리 프로세스를 간소화합니다.|
|컴퓨터|20|머신에 Endpoint Protection 솔루션 설치|가상 머신에 엔드포인트 보호 솔루션을 설치하여 위협 및 취약성으로부터 보호합니다.|
|컴퓨터|20|시스템 업데이트를 적용하려면 머신 다시 시작|시스템 업데이트를 적용하고 취약성으로부터 머신을 보호하려면 머신을 다시 시작합니다.|
|컴퓨터|15|가상 머신에서 디스크 암호화를 적용해야 합니다.|Windows 및 Linux 가상 머신 모두에 Azure Disk Encryption를 사용하여 가상 머신 디스크를 암호화합니다. ADE(Azure Disk Encryption)는 OS 및 데이터 암호화를 제공하는 Windows의 업계 표준 BitLocker 기능 및 Linux의 DM-Crypt 기능을 활용하여 데이터를 보호하고 고객 Azure Key Vault에서 조직 보안 및 규정 준수 약정을 충족하는 데 도움이 됩니다. 규정 준수 및 보안 요구 사항에서 임시(로컬로 임시 연결) 디스크 암호화를 포함하여 암호화 키를 사용한 데이터의 종단 간 암호화를 요구할 경우 Azure Disk Encryption을 사용합니다. 또는 기본적으로 Managed Disks는 Azure Storage Service Encryption을 사용하여 암호화되며 암호화 키가 Azure에서 Microsoft 관리 키입니다. 이것이 규정 준수 및 보안 요구 사항을 충족한다면 기본 관리 디스크 암호화를 사용하여 요구 사항에 부합할 수 있습니다.|
|컴퓨터|30|가상 머신에 취약성 평가 솔루션 설치|가상 머신에 취약성 평가 솔루션 설치|
|컴퓨터|15|웹 애플리케이션 방화벽 추가| WAF(웹 애플리케이션 방화벽) 솔루션을 배포하여 웹 애플리케이션을 보호합니다. |
|컴퓨터|30|취약성 평가 솔루션으로 취약성을 수정해야 합니다.|취약성 평가 타사 솔루션이 배포된 가상 머신이 애플리케이션 및 OS 취약성에 대해 연속적으로 평가됩니다. 이러한 취약성 항목이 발견될 때마다 이 내용을 자세한 권장 구성의 일부로 사용할 수 있습니다.|
|컴퓨터|30|가상 머신에 취약성 평가 솔루션 설치|가상 머신에 취약성 평가 솔루션 설치|
|컴퓨터|1|가상 컴퓨터를 새 AzureRM 리소스에 마이그레이션해야 합니다.|가상 컴퓨터에 대 한 Azure Resource Manager를 사용 하 여 더 강력한 액세스 제어 (RBAC), 향상 된 감사, 리소스 관리자 기반 배포 및 거 버 넌 스, 관리 되는 id에 대 한 액세스, 비밀에 대 한 key vault 액세스 등의 향상 된 보안 기능 제공 보다 쉬운 보안 관리를 위해 Azure AD 기반 인증 및 태그 및 리소스 그룹에 대 한 지원을 제공 합니다. |
|컴퓨터|30|취약성 평가 솔루션으로 취약성을 수정해야 합니다.|취약성 평가 타사 솔루션이 배포된 가상 머신이 애플리케이션 및 OS 취약성에 대해 연속적으로 평가됩니다. 이러한 취약성 항목이 발견될 때마다 이 내용을 자세한 권장 구성의 일부로 사용할 수 있습니다.|
|가상 머신 크기 집합 |4|Virtual Machine Scale Sets의 진단 로그를 사용 하도록 설정 해야 합니다.|로그를 사용하도록 설정하고 최대 1년간 보존합니다. 이 옵션을 사용하면 조사를 위해 작업 내역을 다시 만들 수 있습니다. 이 옵션은 보안 인시던트가 발생하거나 네트워크가 손상된 경우에 유용합니다.|
|가상 머신 크기 집합|35|가상 머신 확장 집합에서 보안 구성의 취약성을 수정해야 합니다.|가상 머신 확장 집합의 보안 구성에서 취약성을 수정하여 공격으로부터 보호합니다. |
|가상 머신 크기 집합|5|가상 머신 확장 집합에 대한 엔드포인트 보호 상태 오류 수정|가상 머신 확장 집합에 대한 엔드포인트 보호 상태 오류를 수정하여 위협 및 취약성으로부터 보호합니다. |
|가상 머신 크기 집합|10|가상 컴퓨터에 Endpoint protection을 설치 해야 합니다.|가상 머신 확장 집합에 엔드포인트 보호 솔루션을 설치하여 위협 및 취약성으로부터 보호합니다. |
|가상 머신 크기 집합|40|가상 머신 확장 집합에 대한 시스템 업데이트를 설치해야 합니다.|누락된 시스템 보안 및 중요 업데이트를 설치하여 Windows 및 Linux 가상 머신 확장 집합을 보호합니다. |
|


## <a name="next-steps"></a>다음 단계
다른 Azure 리소스 유형에 적용되는 권장 사항에 대해 자세히 알아보려면 다음을 참조하세요.

* [Azure Security Center에서 ID 및 액세스 모니터링](security-center-identity-access.md)
* [Azure Security Center에서 네트워크 보호](security-center-network-recommendations.md)
* [Azure Security Center에서 Azure SQL 서비스 보호](security-center-sql-service-recommendations.md)

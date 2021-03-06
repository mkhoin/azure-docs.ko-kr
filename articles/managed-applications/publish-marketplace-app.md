---
title: Marketplace에서 Azure 관리되는 애플리케이션 | Microsoft Docs
description: Marketplace를 통해 사용할 수 있는 Azure 관리형 애플리케이션에 대해 설명합니다.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.date: 07/17/2019
ms.author: tomfitz
ms.openlocfilehash: 372c65f1ac9a08d066f26e637f3af86807b35f11
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70182817"
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Marketplace에서 Azure 관리되는 애플리케이션

공급업체는 Azure 관리되는 애플리케이션을 사용하여 모든 Azure Marketplace 고객에게 솔루션을 제공할 수 있습니다. 이러한 공급업체에는 MSP(관리되는 서비스 제공업체), 독립 소프트웨어 공급업체(ISP), 시스템 통합 사업자(SI) 등이 포함될 수 있습니다. 관리되는 애플리케이션은 고객에 대한 유지 관리 및 서비스 오버헤드를 줄입니다. 공급업체는 마켓플레이스를 통해 인프라 및 소프트웨어를 판매할 수 있습니다. 서비스 및 작업 지원을 관리형 애플리케이션에 연결할 수 있습니다. 자세한 내용은 [관리형 애플리케이션 개요](overview.md)를 참조하세요.

이 문서에서는 애플리케이션을 Marketplace에 게시하고 고객에게 광범위하게 제공하는 방법을 설명합니다.

## <a name="prerequisites-for-publishing-a-managed-application"></a>관리형 애플리케이션을 게시하기 위한 필수 조건

이 문서를 완료하려면 먼저 관리되는 애플리케이션 정의에 대한 .zip 파일이 필요합니다. 자세한 내용은 [서비스 카탈로그 애플리케이션 만들기](publish-service-catalog-app.md)를 참조하세요.

여러 가지 비즈니스 사전 요구 사항이 있습니다. 아래에 이 계정과 키의 예제가 나와 있습니다.

* 회사 또는 해당 자회사는 Marketplace에서 지원하는 판매처 국가/지역에 위치해야 합니다.
* 제품은 마켓플레이스에서 지원하는 청구 모델과 호환되는 방식으로 허가를 받아야 합니다.
* 상업적이고 합리적인 방식으로 고객이 이용할 수 있는 기술 지원을 구축합니다. 이러한 지원은 평가판, 유료 또는 커뮤니티 지원을 통해 이루어질 수 있습니다.
* 소프트웨어 및 타사 소프트웨어 종속성에 대해 사용 허가를 받습니다.
* Marketplace 및 Azure Portal에 등록할 제품에 대한 기준을 충족하는 콘텐츠를 제공합니다.
* Azure Marketplace 참가 정책 및 게시자 약관의 조건에 동의합니다.
* 사용 약관, Microsoft 개인정보처리방침 및 Microsoft Azure Certified 프로그램 계약을 준수한다는 데 동의합니다.

또한 Marketplace 계정이 있어야 합니다. 계정을 만들려면 [파트너 센터에서 상용 Marketplace 계정을 만드는 방법](..//marketplace/partner-center-portal/create-account.md)을 참조하세요.

## <a name="create-a-new-azure-application-offer"></a>새 Azure 애플리케이션 제품 만들기

파트너 포털 계정을 만들었으면 관리되는 애플리케이션 제품을 만들 준비가 된 것입니다.

### <a name="set-up-an-offer"></a>제품 설정

관리되는 애플리케이션에 대한 제품은 게시자가 제공하는 제품의 클래스에 해당합니다. 새로운 유형의 애플리케이션을 마켓플레이스에서 사용할 수 있도록 하려는 경우 새 제품으로 설정할 수 있습니다. 제품은 SKU의 컬렉션입니다. 모든 제품은 마켓플레이스에 고유 엔터티로 나타납니다.

1. [클라우드 파트너 포털](https://cloudpartner.azure.com/)에 로그인합니다.

1. 왼쪽의 탐색 창에서 **+ 새 제품** > **Azure 애플리케이션**을 선택합니다.

1. **편집기** 보기에 필요한 양식이 표시됩니다. 각각의 양식은 이 문서의 뒷부분에서 설명합니다.

## <a name="offer-settings-form"></a>제품 설정 양식

**제품 설정** 양식의 필드는 다음과 같습니다.

* **제품 ID**: 이 고유 식별자는 게시자 프로필 내에서 제품을 식별합니다. 이 ID는 제품 URL, Resource Manager 템플릿 및 청구 보고서에서 볼 수 있습니다. 소문자 영숫자 문자 또는 대시(-)로만 구성할 수 있습니다. ID는 대시로 끝날 수 없습니다. 최대 50문자로 제한됩니다. 제품이 라이브 상태가 되면 이 필드는 잠깁니다.
* **게시자 ID**: 이 드롭다운 목록을 사용하여 이 제품을 게시하려는 게시자 프로필을 선택합니다. 제품이 라이브 상태가 되면 이 필드는 잠깁니다.
* **이름**: 제품에 대한 이 표시 이름이 Marketplace 및 포털에 표시됩니다. 최대 50문자를 포함할 수 있습니다. 제품의 인식 가능한 브랜드 이름을 포함합니다. 회사 이름을 포함하는 것이 마케팅 전략인 경우를 제외하고 여기에 회사 이름을 포함하지 마세요. 이 제품을 자신의 고유 웹 사이트에 마케팅하는 경우 이 이름은 웹 사이트에 표시되는 이름과 같아야 합니다.

마치면 **저장**을 클릭하여 진행 상황을 저장합니다.

## <a name="skus-form"></a>SKU 양식

다음 단계는 제품에 대한 SKU를 추가하는 것입니다.

SKU는 제품의 가장 작은 구매 가능 단위입니다. 같은 제품 클래스(제품) 내에서 SKU를 사용하여 다음을 구별할 수 있습니다.

* 제공되는 다양한 기능
* 제품이 관리되는지 관리되지 않는지 여부
* 지원되는 요금 청구 모델

SKU는 마켓플레이스의 부모 제품 아래에 표시됩니다. Azure Portal에는 고유 구입 가능 엔터티로 표시됩니다.

1. **SKU** > **새 SKU**를 선택합니다.

1. **SKU ID**를 입력합니다. SKU ID는 제품 내의 SKU에 대한 고유 식별자입니다. 이 ID는 제품 URL, Resource Manager 템플릿 및 청구 보고서에서 볼 수 있습니다. 소문자 영숫자 문자 또는 대시(-)로만 구성할 수 있습니다. ID는 대시로 끝내면 안 되며 최대 50문자로 제한됩니다. 제품이 라이브 상태가 되면 이 필드는 잠깁니다. 제품 내에 여러 SKU를 포함할 수 있습니다. 게시하려는 각 이미지에 대해 SKU가 필요합니다.

1. 다음 양식의 **SKU 세부 정보** 섹션을 작성합니다.

   다음 필드를 작성합니다.

   * **제목**: 이 SKU에 사용할 제목을 입력합니다. 이 제목은 이 항목에 대한 갤러리에 나타납니다.
   * **요약**: 이 SKU에 대한 간단한 요약을 입력합니다. 이 텍스트는 제목 아래에 나타납니다.
   * **설명**: SKU에 대한 자세한 설명을 입력합니다.
   * **SKU 형식**: 허용된 값은 *관리되는 애플리케이션* 및 *솔루션 템플릿*입니다. 이 경우 *관리되는 애플리케이션*을 선택합니다.
   * **국가/지역 가용성**: 관리형 애플리케이션을 사용할 수 있는 국가/지역을 선택합니다.
   * **가격 책정**: 애플리케이션의 관리 가격을 제공합니다. 가격을 설정하기 전에 사용 가능한 국가/지역을 선택합니다.

1. 새 패키지를 추가합니다. 다음 양식의 **패키지 세부 정보** 섹션을 작성합니다.

   다음 필드를 작성합니다.

   * **버전**: 업로드하는 패키지에 대한 버전을 입력합니다. `{number}.{number}.{number}{number}` 형식이어야 합니다.
   * **패키지 파일(.zip)** : 이 패키지에는 .zip 패키지로 압축된 두 개의 필수 파일이 포함되어 있습니다. 한 파일은 관리되는 애플리케이션에 배포할 리소스를 정의하는 Resource Manager 템플릿입니다. 다른 한 파일은 포털을 통해 관리되는 애플리케이션을 배포하는 소비자를 위한 [사용자 인터페이스](create-uidefinition-overview.md)를 정의합니다. 사용자 인터페이스에서 소비자가 매개 변수 값을 제공할 수 있도록 하는 요소를 지정합니다.
   * **테넌트 ID**: 계정에 액세스할 수 있는 테넌트 ID입니다.
   * **JIT 액세스 사용**: **예**를 선택하여 계정에 대한 [Just-In-Time 액세스 제어](request-just-in-time-access.md)를 활성화합니다. 사용하도록 설정하면 지정된 기간 동안 소비자의 계정에 대한 액세스를 요청합니다. 관리형 애플리케이션의 소비자 계정에 영구 액세스 권한을 부여하도록 요구하려면 **아니요**를 선택합니다.
   * **허용되는 고객 작업을 사용자 지정하시겠습니까?** : 관리되는 리소스에서 소비자가 수행할 수 있는 작업을 지정하려면 **예**를 선택합니다.
   * **허용되는 고객 작업**: 이전 설정에서 **예**를 선택하는 경우 [Azure 리소스에 대한 거부 할당](../role-based-access-control/deny-assignments.md)을 사용하여 소비자에게 허용되는 작업을 지정할 수 있습니다.

     사용 가능한 작업에 대해서는 [Azure Resource Manager 리소스 공급자 작업](../role-based-access-control/resource-provider-operations.md)을 참조하세요. 예를 들어 소비자가 가상 머신을 다시 시작하도록 허용하려면 허용되는 작업에 `Microsoft.Compute/virtualMachines/restart/action`을 추가합니다. `*/read` 작업은 자동으로 허용되므로 해당 설정을 포함할 필요가 없습니다.
   * **PrincipalId**: 이 속성은 고객 구독에서 리소스에 대한 액세스 권한이 부여된 사용자, 사용자 그룹 또는 애플리케이션의 Azure AD(Azure Active Directory) 식별자입니다. 역할 정의는 권한을 설명합니다.
   * **역할 정의**: 이 속성은 Azure AD가 제공한 모든 기본 제공 RBAC(역할 기반 액세스 제어) 역할 목록입니다. 고객을 대신하여 리소스를 관리하는 데 사용할 가장 적합한 역할을 선택할 수 있습니다.
   * **정책 설정**: 배포된 솔루션에 대한 규정 준수 요구 사항을 지정하려면 [Azure Policy](../governance/policy/overview.md)를 관리되는 애플리케이션에 적용합니다. 사용 가능한 옵션에서 적용할 정책을 선택합니다. **정책 매개 변수**의 경우 JSON 문자열에 매개 변수 값을 제공합니다. 매개 변수 값의 형식 및 정책 정의는 [Azure Policy 샘플](../governance/policy/samples/index.md)을 참조하세요.

여러 권한 부여를 추가할 수 있습니다. AD 사용자 그룹을 만들고 **PrincipalId**에서 해당 ID를 지정하는 것이 좋습니다. 이러한 방식으로 SKU를 업데이트할 필요 없이 사용자 그룹에 더 많은 사용자를 추가할 수 있습니다.

RBAC에 대한 자세한 내용은 [Azure Portal에서 RBAC 시작](../role-based-access-control/overview.md)을 참조하세요.

## <a name="marketplace-form"></a>Marketplace 양식

Marketplace 양식에서 [Azure Marketplace](https://azuremarketplace.microsoft.com) 및 [Azure Portal](https://portal.azure.com/)에 표시되는 필드를 요청합니다.

### <a name="preview-subscription-ids"></a>미리 보기 구독 ID

게시된 후 제안에 액세스할 수 있는 Azure 구독 ID의 목록을 입력합니다. 이러한 허용 목록 구독을 사용하여 제안을 라이브 상태로 만들기 전에 미리 본 제안을 테스트할 수 있습니다. 파트너 포털에서 최대 100개의 구독 허용 목록을 컴파일할 수 있습니다.

### <a name="suggested-categories"></a>권장 범주

목록에서 제품이 가장 잘 연결될 수 있는 범주를 5개까지 선택합니다. 이러한 범주는 고객의 제품을 [Azure Marketplace](https://azuremarketplace.microsoft.com) 및 [Azure Portal](https://portal.azure.com/)에서 사용할 수 있는 제품 범주에 매핑하는 데 사용됩니다.

#### <a name="azure-marketplace"></a>Azure Marketplace

관리되는 애플리케이션 요약은 다음 필드를 표시합니다.

![Marketplace 요약](./media/publish-marketplace-app/publishvm10.png)

관리되는 애플리케이션에 대한 **개요** 탭은 다음 필드를 표시합니다.

![Marketplace 개요](./media/publish-marketplace-app/publishvm11.png)

관리되는 애플리케이션에 대한 **계획 + 가격** 탭은 다음 필드를 표시합니다.

![Marketplace 계획](./media/publish-marketplace-app/publishvm15.png)

#### <a name="azure-portal"></a>Azure portal

관리되는 애플리케이션 요약은 다음 필드를 표시합니다.

![포털 요약](./media/publish-marketplace-app/publishvm12.png)

관리되는 애플리케이션에 대한 개요는 다음 필드를 표시합니다.

![포털 개요](./media/publish-marketplace-app/publishvm13.png)

#### <a name="logo-guidelines"></a>로고 지침

클라우드 파트너 포털에 업로드하는 모든 로고에 대해 이 지침을 따릅니다.

*   Azure 디자인은 단순한 색 팔레트를 사용합니다. 로고의 기본 색상과 보조 색상 수를 제한합니다.
*   포털의 테마 색은 흰색과 검은색입니다. 로고의 배경색으로 이러한 색을 사용하지 마십시오. 포털에서 로고가 돋보이도록 하는 색을 사용합니다. 간단한 기본 색을 사용하는 것이 좋습니다. *투명한 배경을 사용하는 경우 로고 및 텍스트는 흰색, 검정색 또는 파란색이 아니어야 합니다.*
*   로고의 배경에 그라데이션 효과를 사용하지 마십시오.
*   로고에 회사 또는 브랜드 이름을 포함한 텍스트를 놓지 마십시오. 로고의 모양과 느낌은 평면적이어야 하며 그라데이션은 사용하지 마십시오.
*   로고가 늘어나지 않았는지 확인합니다.

#### <a name="hero-logo"></a>대표 로고

대표 로고는 선택 사항입니다. 게시자는 대표 로고를 업로드하지 않아도 됩니다. 대표 아이콘을 업로드한 후에 삭제할 수 없습니다. 이때 파트너는 대표 아이콘에 대한 Marketplace 지침을 따라야 합니다.

대표 로고 아이콘에 대한 이러한 지침을 따릅니다.

*   게시자 표시 이름, 계획 제목 및 제안의 자세한 요약은 흰색으로 표시됩니다. 따라서 대표 아이콘의 배경색으로 밝은 색을 사용하지 마십시오. 대표 아이콘에는 검은색, 흰색 또는 투명한 배경이 허용되지 않습니다.
*   제품이 나열된 후에 hero 로고 내에 프로그래밍 방식으로 요소가 포함됩니다. 포함된 요소에는 게시자 표시 이름, 계획 제목, 자세한 제안 요약 및 **만들기** 단추가 포함됩니다. 따라서 대표 로고를 디자인하는 동안 텍스트를 입력하지 마십시오. 텍스트는 프로그래밍 방식으로 해당 공간에 포함되므로 오른쪽의 빈 공간을 남겨 둡니다. 오른쪽의 텍스트에 대한 빈 공간은 415x100픽셀이어야 합니다. 왼쪽에서 370픽셀로 오프셋됩니다.

    ![대표 로고 예제](./media/publish-marketplace-app/publishvm14.png)

## <a name="support-form"></a>지원 양식

회사의 지원 연락처로 **지원** 양식을 작성합니다. 이 정보는 엔지니어링 연락처와 고객 지원 연락처일 수 있습니다.

## <a name="publish-an-offer"></a>제안 게시

모든 섹션을 작성한 후 **게시**를 선택하여 고객에게 제품을 제공하는 과정을 시작합니다.

## <a name="next-steps"></a>다음 단계

* **게시**를 클릭한 후 발생하는 상황에 대한 자세한 내용은 [Azure 애플리케이션 제품 게시](../marketplace/cloud-partner-portal/azure-applications/cpp-publish-offer.md)를 참조하세요.
* 관리형 애플리케이션에 대한 소개는 [관리형 애플리케이션 개요](overview.md)를 참조하세요.
* 서비스 카탈로그 관리형 애플리케이션을 게시하는 방법에 대한 자세한 내용은 [서비스 카탈로그 관리형 애플리케이션 만들기 및 게시](publish-service-catalog-app.md)를 참조하세요.

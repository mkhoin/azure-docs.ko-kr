---
title: Azure 구독의 청구 소유권 얻기
description: 다른 사용자에게 Azure 구독의 청구 소유권을 요청하는 방법을 알아봅니다.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 2ac2d552aed5b11eaf5a8e4708b2cabc33ba1bb3
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74223804"
---
# <a name="get-billing-ownership-of-azure-subscriptions-from-other-accounts"></a>다른 계정에서 Azure 구독의 청구 소유권 얻기

기존 청구 소유자가 조직을 떠나는 경우 또는 청구 계정을 통해 구독 요금을 지불하려는 경우 Azure 구독의 소유권을 가져올 수 있습니다. 소유권을 획득하면 구독에 대한 청구 책임이 사용자 계정에 전송됩니다.

이 문서는 Microsoft 고객 계약에 대한 청구 계정에 적용됩니다. [Microsoft 고객 계약에 액세스할 수 있는지 확인합니다](#check-for-access).

청구 소유권을 요청하려면 **청구서 섹션 소유자** 또는 **청구서 섹션 기여자**여야 합니다. 자세히 알아보려면 [청구서 섹션 역할 작업](billing-understand-mca-roles.md#invoice-section-roles-and-tasks)을 참조하세요.

## <a name="request-billing-ownership"></a>청구 소유권 요청

1. Microsoft 고객 계약의 청구 계정에 대해 청구서 섹션 소유자 또는 기여자로 [Azure Portal](https://portal.azure.com)에 로그인합니다.

2. **Cost Management + 청구**를 검색합니다.

   ![Cost Management + 청구에 대한 Azure Portal 검색을 보여 주는 스크린샷](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. 왼쪽에서 **청구서 섹션**을 선택합니다. 액세스 권한에 따라 청구 계정 또는 청구 프로필을 선택해야 할 수도 있습니다. 청구 계정 또는 프로필에서 **청구서 섹션**을 선택합니다.
   
   ![청구서 섹션 선택을 보여 주는 스크린샷](./media/billing-mca-request-billing-ownership/mca-select-invoice-sections.png)        

4. 목록에서 청구서 섹션을 선택합니다. 구독의 소유권을 가져온 다음에는 이 청구서 섹션으로 청구됩니다.

5. 왼쪽 아래에서  **요청**을 선택한 다음, **추가**를 선택합니다.
 
   ![전송 요청 선택을 보여 주는 스크린샷](./media/billing-mca-request-billing-ownership/mca-select-transfer-requests.png)

6. 청구 소유권 요청 대상 사용자의 이메일 주소를 입력합니다. 해당 사용자는 Microsoft Online Service Program 청구 계정의 계정 관리자이거나 기업계약의 계정 소유자여야 합니다. 자세한 내용은 [Azure Portal에서 청구 계정 보기](billing-view-all-accounts.md)를 참조하세요. **요청 보내기**를 선택합니다.

   ![전송 요청 전송을 보여 주는 스크린샷](./media/billing-mca-request-billing-ownership/mca-send-transfer-requests.png)

7. 사용자는 전송 요청을 검토하는 지침이 포함된 이메일을 받습니다.

   ![전송 요청 이메일 검토를 보여 주는 스크린샷](./media/billing-mca-request-billing-ownership/mca-review-transfer-request-email.png)

8. 전송 요청을 승인하려면 사용자는 이메일의 링크를 선택하고 지침을 따릅니다.

    ![전송 요청 이메일 검토를 보여 주는 스크린샷](./media/billing-mca-request-billing-ownership/mca-review-transfer-request.png)

## <a name="check-the-transfer-request-status"></a>전송 요청 상태 확인

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.

2. **Cost Management + 청구**를 검색합니다.

   ![Cost Management + 청구에 대한 Azure Portal 검색을 보여 주는 스크린샷](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)


3. 왼쪽에서 **청구서 섹션**을 선택합니다. 액세스 권한에 따라 청구 계정 또는 청구 프로필을 선택해야 할 수도 있습니다. 청구 계정 또는 프로필에서 **청구서 섹션**을 선택합니다.
   
   ![청구서 섹션 선택을 보여 주는 스크린샷](./media/billing-mca-request-billing-ownership/mca-select-invoice-sections.png)        

4. 목록에서 전송 요청을 보낸 청구서 섹션을 선택합니다.

5. 왼쪽 아래에서 **전송 요청**을 선택합니다. 전송 요청 페이지에는 다음 정보가 표시됩니다.

    ![전송 요청 목록을 보여 주는 스크린샷](./media/billing-mca-request-billing-ownership/mca-select-transfer-requests-for-status.png)

   |열|정의|
   |---------|---------|
   |요청 날짜|전송 요청이 전송된 날짜입니다.|
   |받는 사람|청구 소유권 전송 요청을 보낸 사용자의 이메일 주소입니다.|
   |만료 날짜|요청이 만료되는 날짜|
   |상태|전송 요청의 상태|

    전송 요청은 다음 상태 중 하나를 가질 수 있습니다.

   |상태|정의|
   |---------|---------|
   |진행 중|사용자가 전송 요청을 수락하지 않았습니다.|
   |처리 중|사용자가 전송 요청을 승인했습니다. 사용자가 선택한 구독에 대한 청구가 청구서 섹션으로 이전됩니다.|
   |Completed| 사용자가 선택한 구독에 대한 청구가 청구서 섹션으로 이전되었습니다.|
   |완료되었지만 오류 발생|요청을 완료했지만 사용자가 선택한 일부 구독에 대한 청구를 이전하지 못했습니다.|
   |만료됨|사용자가 제때에 요청을 수락하지 않아서 만료되었습니다.|
   |Canceled|전송 요청에 대한 액세스 권한이 있는 다른 사용자가 요청을 취소했습니다.|
   |거부함|사용자가 전송 요청을 거부했습니다.|

7. 세부 정보를 볼 전송 요청을 선택합니다. 전송 세부 정보 페이지에는 다음 정보가 표시됩니다.
   
   ![이전된 구독 목록을 보여 주는 스크린샷](./media/billing-mca-request-billing-ownership/mca-transfer-completed.png)
    
   |열  |정의|
   |---------|---------|
   |전송 요청 ID|전송 요청에 대한 고유 ID입니다. 지원 요청을 신속하게 처리할 수 있도록 지원 요청을 제출할 때 Azure 지원에 ID를 알려주세요.|
   |전송 요청한 날짜|전송 요청이 전송된 날짜입니다.|
   |전송 요청한 사람|전송 요청을 보낸 사용자의 이메일 주소입니다.|
   |전송 요청 만료 날짜| 전송 요청이 만료되는 날짜입니다.|
   |받는 사람의 이메일 주소|청구 소유권 전송 요청을 보낸 사용자의 이메일 주소입니다.|
   |받는 사람에게 보낸 전송 링크|전송 요청을 검토하도록 사용자에게 보낸 URL입니다.|

## <a name="supported-subscription-types"></a>지원되는 구독 유형

아래에 나열된 구독 유형의 청구 소유권을 요청할 수 있습니다.

- [Action Pack](https://azure.microsoft.com/offers/ms-azr-0025p/)\* 
- [Azure in Open 라이선스](https://azure.microsoft.com/offers/ms-azr-0111p/)\*
- [Azure Pass 스폰서쉽](https://azure.microsoft.com/offers/azure-pass/)\*
- [Enterprise 개발/테스트](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [평가판](https://azure.microsoft.com/offers/ms-azr-0044p/)\*
- [종량제](https://azure.microsoft.com/offers/ms-azr-0003p/) 
- [종량제 개발/테스트](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Microsoft Azure 플랜](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*
- [Microsoft Azure 스폰서 제안](https://azure.microsoft.com/offers/ms-azr-0036p/)\*
- [Microsoft 기업계약](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Microsoft 파트너 네트워크](https://azure.microsoft.com/offers/ms-azr-0025p/)\*
- [MSDN 플랫폼](https://azure.microsoft.com/offers/ms-azr-0062p/)\*
- [Visual Studio Enterprise(BizSpark) 구독자](https://azure.microsoft.com/offers/ms-azr-0064p/)\*
- [Visual Studio Enterprise(MPN) 구독자](https://azure.microsoft.com/offers/ms-azr-0029p/)\*
- [Visual Studio Enterprise 구독자](https://azure.microsoft.com/offers/ms-azr-0063p/)\*
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)\*
- [Visual Studio Test Professional 구독자](https://azure.microsoft.com/offers/ms-azr-0060p/)\*

\* 구독에서 사용할 수 있는 모든 크레딧은 이전 후 새 계정에서 사용할 수 없습니다.

\*\* Azure 웹 사이트에서 가입하는 동안 생성된 계정에만 구독이 지원됩니다.


## <a name="additional-information"></a>추가 정보

다음 섹션에서는 구독 이전에 대한 추가 정보를 제공합니다.

### <a name="no-service-downtime"></a>서비스 가동 중지 시간이 없습니다.

구독에 포함된 Azure 서비스는 계속해서 중단 없이 실행됩니다. 사용자가 이전하기 위해 선택하는 Azure 구독에 대한 청구 관계만 전환됩니다.

### <a name="disabled-subscriptions"></a>사용하지 않도록 설정된 구독

사용하지 않도록 설정된 구독은 이전할 수 없습니다. 청구 소유권을 이전하려면 구독이 활성 상태여야 합니다.

### <a name="azure-resources-transfer"></a>Azure 리소스 이전

VM, 디스크 및 웹 사이트와 같은 구독의 모든 리소스가 이전됩니다.

### <a name="azure-marketplace-products-transfer"></a>Azure Marketplace 제품 이전

Azure Marketplace 제품이 각 구독과 함께 이전됩니다.

### <a name="azure-reservations-transfer"></a>Azure Reservations 이전

Azure Reservations는 구독과 함께 자동으로 이동되지 않습니다. Reservations를 이동하려면 [Azure 지원에 문의](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)하세요.

### <a name="access-to-azure-services"></a>Azure 서비스에 대한 액세스

(Azure RBAC(역할 기반 액세스 제어))[../role-based-access-control/overview.md]를 사용하여 할당된 기존 사용자, 그룹 또는 서비스 사용자에 대한 액세스는 이전 중에 영향을 받지 않습니다.

### <a name="azure-support-plan"></a>Azure 지원 플랜

Azure 지원은 구독과 함께 이전되지 않습니다. 사용자가 모든 Azure 구독을 이전하는 경우 지원 계획을 취소하도록 요청하세요.

### <a name="charges-for-transferred-subscription"></a>이전된 구독의 요금

구독의 원래 청구 소유자는 이전이 완료된 시점까지 보고된 모든 요금에 대한 책임이 있습니다. 청구서 섹션은 이전 시점부터 보고된 요금에 대해 지불 책임이 있습니다. 이전하기 전에 발생하지만 나중에 보고되는 일부 요금이 있을 수 있습니다. 이러한 요금은 청구서 섹션에 표시됩니다.

### <a name="cancel-a-transfer-request"></a>전송 요청 취소

요청이 승인되거나 거부될 때까지는 전송 요청을 취소할 수 있습니다. 전송 요청을 취소하려면 [전송 세부 정보 페이지](#check-the-transfer-request-status)로 이동하여 페이지 아래쪽에서 취소를 선택합니다.

### <a name="software-as-a-service-saas-transfer"></a>SaaS(Software as a Service) 전송

SaaS 제품은 구독과 함께 전송되지 않습니다. [Azure 지원에 문의](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)하여 SaaS 제품의 청구 소유권을 이전하도록 사용자에게 요청하세요. 사용자는 청구 소유권과 함께 리소스 소유권도 이전할 수 있습니다. 리소스 소유권을 사용하면 제품의 세부 정보 삭제 및 보기와 같은 관리 작업을 수행할 수 있습니다. 리소스 소유권을 이전하기 위해서는 사용자가 해당 SaaS 제품에 대한 리소스 소유자여야 합니다.

## <a name="check-for-access"></a>액세스 확인
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>도움 필요 시 지원에 문의

도움이 필요한 경우 [지원에 문의](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)하여 문제를 신속하게 해결하세요.

## <a name="next-steps"></a>다음 단계

- Azure 구독의 청구 소유권이 청구서 섹션으로 이전됩니다. [Azure Portal](https://portal.azure.com)에서 이러한 구독에 대한 요금을 추적합니다.
- 이러한 구독에 대한 청구를 보고 관리할 수 있는 권한을 다른 사용자에게 부여합니다. 자세한 내용은 [청구서 섹션 역할 및 작업](billing-understand-mca-roles.md#invoice-section-roles-and-tasks)을 참조하세요.

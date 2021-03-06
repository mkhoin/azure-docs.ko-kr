---
title: Azure 사용량 및 요금 보기 및 다운로드
description: Azure 일일 사용량 및 요금을 다운로드하거나 보는 방법을 설명합니다.
keywords: 청구 사용량, 사용 요금, 사용량 다운로드, 사용량 보기, azure 청구서, azure 사용량
author: bandersmsft
manager: jureid
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: dcf4e3b9d85909c8f1d149c9d1940a6755b431a1
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74224004"
---
# <a name="view-and-download-your-azure-usage-and-charges"></a>Azure 사용량 및 요금 보기 및 다운로드

Azure Portal에서 Azure 사용량 및 요금에 대한 일별 분석을 다운로드할 수 있습니다. 계정 관리자 또는 엔터프라이즈 관리자와 같은 특정 역할만 Azure 사용량 정보를 확인할 수 있는 권한이 있습니다. 청구 정보에 액세스하는 방법에 대한 자세한 내용은 [역할을 사용하여 Azure 청구에 대한 액세스 관리](billing-manage-access.md)를 참조하세요.

MCA(Microsoft 고객 계약)가 있는 경우 청구 프로필 소유자, 기여자, 독자 또는 청구서 관리자여야 Azure 사용량과 요금을 확인할 수 있습니다.  MPA(Microsoft 파트너 계약)가 있는 경우 파트너 조직의 글로벌 관리자 및 관리 에이전트 역할만 Microsoft에서 Azure 사용량과 요금을 보고 다운로드할 수 있습니다. [Azure Portal에서 청구 계정 유형을 확인합니다](#check-your-billing-account-type).

## <a name="download-usage-from-the-azure-portal-csv"></a>Azure Portal에서 사용량 다운로드(.csv)

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. *Cost Management + 청구*를 검색합니다.

    ![Azure Portal 검색을 보여 주는 스크린샷](./media/billing-download-azure-usage/portal-cm-billing-search.png)

1. 액세스 권한에 따라 청구 계정 또는 청구 프로필을 선택해야 할 수도 있습니다.
1. 왼쪽 메뉴에서 **청구** 아래의 **청구서**를 선택합니다.
1. 청구서 그리드에서 다운로드하려는 사용량에 해당하는 청구 기간의 행을 찾습니다.
1. 다운로드 아이콘 또는 오른쪽에 있는 줄임표(`...`)를 클릭합니다.
1. 다운로드한 메뉴에서 **Azure 사용량 및 요금 다운로드**를 선택합니다.

## <a name="download-usage-for-ea-customers"></a>EA 고객의 사용량 다운로드

EA 고객이 사용량 데이터를 살펴보고 다운로드하려면 요금 보기 정책을 사용하도록 설정된 엔터프라이즈 관리자, 계정 소유자 또는 부서 관리자여야 합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. *Cost Management + 청구*를 검색합니다.

    ![Azure Portal 검색을 보여 주는 스크린샷](./media/billing-download-azure-usage/portal-cm-billing-search.png)

1. **사용량 + 요금**을 선택합니다.
1. 다운로드하려는 월의 **다운로드**를 선택합니다.

## <a name="download-usage-for-pending-charges"></a>보류 중인 요금에 대한 사용량 다운로드

Microsoft 고객 계약이 있는 경우 현재 청구 기간에 대한 월간 누계 사용량을 다운로드할 수 있습니다. 이러한 사용량 요금은 아직 청구되지 않았습니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
2. *Cost Management + 청구*를 검색합니다.
3. 청구 프로필을 선택합니다. 액세스 권한에 따라 청구 계정을 먼저 선택해야 할 수도 있습니다.
4. **개요** 영역에서 월간 누계 요금 아래의 다운로드 링크를 찾습니다.
5. **Azure 사용량 및 요금**을 선택합니다.

    ![개요에서 다운로드를 보여 주는 스크린샷](./media/billing-download-azure-usage/open-usage.png)

## <a name="check-your-billing-account-type"></a>청구 계정 유형 확인
[!INCLUDE [billing-check-account-type](../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>도움 필요 시 문의하세요.

질문이 있거나 도움이 필요한 경우 [지원 요청을 만드세요](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>다음 단계

청구서 및 사용량 요금에 대한 자세한 내용은 다음을 참조하세요.

- [Microsoft Azure 세부 사용량의 용어 이해](billing-understand-your-usage.md)
- [Microsoft Azure 요금 청구 방식 이해](billing-understand-your-bill.md)
- [Microsoft Azure 청구서 보기 및 다운로드](billing-download-azure-invoice.md)
- [조직의 Azure 가격 책정 보기 및 다운로드](billing-ea-pricing.md)

Microsoft 고객 계약이 있는 경우 다음을 참조하세요.

- [Microsoft 고객 계약 Azure 세부 사용량의 용어 이해](billing-mca-understand-your-usage.md)
- [Microsoft 고객 계약 청구서의 요금 이해](billing-mca-understand-your-bill.md)
- [Microsoft Azure 청구서 보기 및 다운로드](billing-download-azure-invoice.md)
- [Microsoft 고객 계약에 대한 세금 문서 보기 및 다운로드](billing-mca-download-tax-document.md)
- [조직의 Azure 가격 책정 보기 및 다운로드](billing-ea-pricing.md)

---
title: FAQ-Azure DNS
description: 이 문서에서는 Azure DNS에 대 한 질문과 대답을 알아봅니다.
services: dns
author: asudbring
ms.service: dns
ms.topic: article
ms.date: 6/15/2019
ms.author: allensu
ms.openlocfilehash: c194d47842c927d3f8049a424dc08a34424a8a31
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74212024"
---
# <a name="azure-dns-faq"></a>Azure DNS FAQ

## <a name="about-azure-dns"></a>Azure DNS 정보

### <a name="what-is-azure-dns"></a>Azure DNS란?

DNS(Domain Name System)는 웹 사이트 또는 서비스 이름을 해당 IP 주소로 변환하거나 확인합니다. Azure DNS는 DNS 도메인에 대한 호스팅 서비스입니다. 이 서비스는 Microsoft Azure 인프라를 사용하여 이름 확인을 제공합니다. Azure에 도메인을 호스트하면 다른 Azure 서비스와 동일한 자격 증명, API, 도구 및 대금 청구를 사용하여 DNS 레코드를 관리할 수 있습니다.

Azure DNS의 DNS 도메인은 DNS 이름 서버의 Azure 글로벌 네트워크에 호스트됩니다. 이 시스템은 사용 가능한 가장 가까운 DNS 서버에서 각 DNS 쿼리에 응답하도록 애니캐스트 네트워킹을 사용합니다. Azure DNS는 도메인에 대해 빠른 성능과 고가용성을 제공합니다.

Azure DNS는 Azure Resource Manager를 기반으로 합니다. Azure DNS는 역할 기반 액세스 제어, 감사 로그 및 리소스 잠금과 같은 리소스 관리자 기능의 이점을 누릴 수 있습니다. 도메인과 레코드는 Azure 포털, Azure PowerShell cmdlet 및 플랫폼 간 Azure CLI를 통해 관리할 수 있습니다. 자동 DNS 관리가 필요한 애플리케이션은 REST API 및 SDK를 통해 서비스와 통합할 수 있습니다.

### <a name="how-much-does-azure-dns-cost"></a>Azure DNS 비용은 얼마입니까?

Azure DNS 요금 청구 모델은 Azure DNS에 호스트되는 DNS 영역의 수를 기준으로 합니다. 또한 수신되는 DNS 쿼리 수도 기준으로 합니다. 할인은 사용량에 따라 제공됩니다.

자세한 내용은 [Azure DNS 가격 책정 페이지](https://azure.microsoft.com/pricing/details/dns/)를 참조하세요.

### <a name="what-is-the-sla-for-azure-dns"></a>Azure DNS에 대한 SLA는 무엇인가요?

Azure는 유효한 DNS 요청에 대해 100%의 시간 동안 하나 이상의 Azure DNS 이름 서버에서 응답을 제공할 것을 보장합니다.

자세한 내용은 [Azure DNS SLA 페이지](https://azure.microsoft.com/support/legal/sla/dns)를 참조하세요.

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>DNS 영역이란 무엇인가요? DNS 도메인과 같은 것인가요? 

도메인은 Domain Name System의 고유 이름입니다. 예를 들어 contoso.com이 있습니다.

DNS 영역은 특정 도메인에 대한 DNS 레코드를 호스트하는 데 사용됩니다. 예를 들어 도메인 contoso.com에는 여러 DNS 레코드가 포함될 수 있습니다. 이 레코드에는 메일 서버에 대 한 mail.contoso.com 및 웹 사이트의 www\.contoso.com 포함 될 수 있습니다. 이러한 레코드는 DNS 영역 contoso.com에서 호스트됩니다.

도메인 이름은 *단지 이름일 뿐입니다*. DNS 영역은 도메인 이름의 DNS 레코드를 포함하는 데이터 리소스입니다. Azure DNS를 사용하여 DNS 영역을 호스트하고 Azure에서 도메인에 대한 DNS 레코드를 관리할 수 있습니다. 또한 인터넷의 DNS 쿼리에 응답하기 위해 DNS 이름 서버를 제공합니다.

### <a name="do-i-need-to-buy-a-dns-domain-name-to-use-azure-dns"></a>Azure DNS를 사용하기 위해 DNS 도메인 이름을 구입해야 하나요? 

그럴 필요는 없습니다.

Azure DNS에서 DNS 영역을 호스트하기 위해 도메인을 구입할 필요는 없습니다. 도메인 이름을 소유하지 않고도 언제든지 DNS 영역을 만들 수 있습니다. 이 영역에 대한 DNS 쿼리는 해당 영역에 할당된 Azure DNS 이름 서버로 전송되는 경우에만 확인됩니다.

DNS 영역을 전역 DNS 계층 구조에 연결하려면 도메인 이름을 구입해야 합니다. 그런 후 전 세계 어디에서나 DNS 쿼리를 사용하여 DNS 영역을 찾고 DNS 레코드를 통해 답변합니다.

## <a name="azure-dns-features"></a>Azure DNS 기능

### <a name="are-there-any-restrictions-when-using-alias-records-for-a-domain-name-apex-with-traffic-manager"></a>Traffic Manager에서 도메인 이름에 별칭 레코드를 사용할 경우 제한 사항이 있나요?

예. Azure Traffic Manager에서는 정적 공용 IP 주소를 사용해야 합니다. 정적 IP 주소를 사용하여 **외부 엔드포인트** 대상을 구성합니다. 

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Azure DNS는 DNS 기반 트래픽 라우팅 또는 엔드포인트 장애 조치(failover)를 지원하나요?

DNS 기반 트래픽 라우팅 및 엔드포인트 장애 조치(failover)는 Traffic Manager에 의해 제공됩니다. Traffic Manager는 Azure DNS와 함께 사용할 수 있는 별도 Azure 서비스입니다. 자세한 내용은 [Traffic Manager 개요](../traffic-manager/traffic-manager-overview.md)를 참조하세요.

Azure DNS는 지정된 DNS 레코드에 대한 각 DNS 쿼리가 항상 동일한 DNS 응답을 수신하는 고정 DNS 도메인 호스트만 지원합니다.

### <a name="does-azure-dns-support-domain-name-registration"></a>Azure DNS는 도메인 이름 등록을 지원하나요?

아니요. 현재 Azure DNS는 도메인 이름을 구입하는 옵션을 지원하지 않습니다. 도메인을 구입하려면 타사 도메인 이름 등록자를 사용해야 합니다. 등록 기관은 일반적으로 소액의 연간 요금을 부과합니다. 그런 다음, DNS 레코드의 관리를 위해 Azure DNS에 해당 도메인을 호스트할 수 있습니다. 자세한 내용은 [Azure DNS에 도메인 위임](dns-domain-delegation.md)을 참조하세요.

도메인 이름을 구입하는 기능은 Azure 백로그에서 추적됩니다. 피드백 사이트를 사용하여 [이 기능에 대한 지원을 등록](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar)합니다.

### <a name="does-azure-dns-support-dnssec"></a>Azure DNS에서는 DNSSEC를 지원하나요?

아니요. 현재 Azure DNS는 DNSSEC(Domain Name System 보안 확장)를 지원하지 않습니다.

DNSSEC 기능은 Azure DNS 백로그에서 추적됩니다. 피드백 사이트를 사용하여 [이 기능에 대한 지원을 등록](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support)합니다.

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Azure DNS에서는 영역 전송(AXFR/IXFR)을 지원하나요?

아니요. Azure DNS는 현재 영역 전송을 지원하지 않습니다. DNS 영역은 [Azure CLI를 사용하여 Azure DNS로 가져올 수 있습니다](dns-import-export.md). [Azure DNS 관리 포털](dns-operations-recordsets-portal.md), [REST API](https://docs.microsoft.com/powershell/module/az.dns), [SDK](dns-sdk.md), [PowerShell cmdlet](dns-operations-recordsets.md) 또는 [CLI 도구](dns-operations-recordsets-cli.md)를 통해 DNS 레코드를 관리할 수 있습니다.

영역 전송 기능은 Azure DNS 백로그에서 추적됩니다. 피드백 사이트를 사용하여 [이 기능에 대한 지원을 등록](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c)합니다.

### <a name="does-azure-dns-support-url-redirects"></a>Azure DNS에서는 URL 리디렉션을 지원하나요?

아니요. URL 리디렉션 서비스는 DNS 서비스가 아닙니다. DNS 수준이 아닌 HTTP 수준에서 작동합니다. URL을 번들로 묶는 일부 DNS 공급자는 서비스를 전체 제품의 일부로 리디렉션합니다. 이 서비스는 현재, Azure DNS에서 지원되지 않습니다.

URL 리디렉션 기능은 Azure DNS 백로그에 추적됩니다. 피드백 사이트를 사용하여 [이 기능에 대한 지원을 등록](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape)합니다.

### <a name="does-azure-dns-support-the-extended-ascii-encoding-8-bit-set-for-txt-record-sets"></a>Azure DNS는 TXT 레코드 집합에 대해 확장된 ASCII 인코딩(8비트) 집합을 지원하나요?

예. Azure DNS는 TXT 레코드 집합에 대해 확장된 ASCII 인코딩(8비트) 집합을 지원합니다. 하지만 최신 버전의 Azure REST API, SDK, PowerShell 및 CLI를 사용해야 합니다. 2017년 10월 1일보다 오래된 버전 또는 SDK 2.1은 확장된 ASCII 집합을 지원하지 않습니다. 

예를 들어 확장된 ASCII 문자 \128이 있는 TXT 레코드에 대한 값으로 문자열을 제공할 수 있습니다. 예를 들면 "abcd\128efgh"와 같습니다. Azure DNS는 내부 표현에 이 문자의 바이트 값 128을 사용합니다. DNS 확인 시에도 이 바이트 값이 응답에 반환됩니다. 또한 확인이 주 목적이라면 "abc"와 "\097\098\099"를 바꿔서 사용해도 됩니다. 

TXT 레코드에 대해 [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt) 영역 파일 마스터 형식 이스케이프 규칙을 따릅니다. 예를 들어, 이제 `\`는 실제로 RFC에 대해 모든 항목을 이스케이프합니다. TXT 레코드 값으로 `A\B`를 지정하면 단지 `AB`로 표시되고 확인됩니다. 실제로 확인 시 TXT 레코드에 `A\B`가 포함되게 하려면 `\`를 다시 이스케이프해야 합니다. 예를 들어 `A\\B`를 지정합니다.

현재, 이 지원은 Azure Portal에서 만든 TXT 레코드에는 사용할 수 없습니다.

## <a name="alias-records"></a>별칭 레코드

### <a name="what-are-some-scenarios-where-alias-records-are-useful"></a>별칭 레코드는 어떤 시나리오에서 유용한가요?

[Azure DNS 별칭 레코드 개요](dns-alias.md)의 시나리오 섹션을 참조하세요.

### <a name="what-record-types-are-supported-for-alias-record-sets"></a>별칭 레코드 집합에 지원되는 레코드 유형은 무엇인가요?

Azure DNS 영역에서 다음 레코드 유형의 별칭 레코드 집합이 지원됩니다.
 
- 변수를 잠그기 위한 
- AAAA
- CNAME 

### <a name="what-resources-are-supported-as-targets-for-alias-record-sets"></a>별칭 레코드 집합의 대상으로 지원되는 리소스는 무엇인가요?

- **DNS A/AAAA 레코드 집합의 공용 IP 리소스를 가리킵니다**. A/AAAA 레코드 집합을 만들고 공용 IP 리소스를 가리키는 별칭 레코드 집합으로 설정할 수 있습니다.
- **DNS A/AAAA/CNAME 레코드 집합의 Traffic Manager 프로필을 가리킵니다**. DNS CNAME 레코드 집합에서 Traffic Manager 프로필의 CNAME을 가리킬 수 있습니다. 예로 contoso.trafficmanager.net이 있습니다. 이제 DNS 영역의 A 또는 AAAA 레코드 집합에서 외부 엔드포인트가 있는 Traffic Manager 프로필을 가리킬 수도 있습니다.
- **Azure Content Delivery Network (CDN) 끝점을 가리킵니다**. 이는 Azure storage 및 Azure CDN를 사용 하 여 정적 웹 사이트를 만드는 경우에 유용 합니다.
- **동일한 영역 내의 다른 DNS 레코드 집합을 가리킵니다**. 별칭 레코드는 동일한 유형의 다른 레코드 집합을 참조할 수 있습니다. 예를 들어 DNS CNAME 레코드 집합을 동일한 유형의 다른 CNAME 레코드 집합에 대한 별칭으로 설정할 수 있습니다. 이 정렬은 일부 레코드 집합은 별칭으로, 일부는 비별칭으로 지정하려는 경우에 유용합니다.

### <a name="can-i-create-and-update-alias-records-from-the-azure-portal"></a>Azure Portal에서 별칭 레코드를 만들고 업데이트할 수 있나요?

예. Azure REST API, PowerShell, CLI 및 SDK와 함께 Azure Portal에서 별칭 레코드를 만들거나 관리할 수 있습니다.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-deleted-when-the-underlying-public-ip-is-deleted"></a>별칭 레코드를 사용하면 기본 공용 IP가 삭제될 때 DNS 레코드 집합이 삭제되는지 확인할 수 있나요?

예. 이것은 별칭 레코드의 핵심 기능 중 하나입니다. 애플리케이션의 사용자를 위한 잠재적인 중단을 방지하는 데 도움이 됩니다.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-updated-to-the-correct-ip-address-when-the-underlying-public-ip-address-changes"></a>별칭 레코드를 사용하면 기본 공용 IP 주소가 변경될 때 DNS 레코드가 올바른 IP 주소로 업데이트되는지 확인할 수 있나요?

예. 이것은 별칭 레코드의 핵심 기능 중 하나입니다. 애플리케이션의 사용자를 위한 잠재적인 중단 또는 보안 위험을 방지하는 데 도움이 됩니다.

### <a name="are-there-any-restrictions-when-using-alias-record-sets-for-a-or-aaaa-records-to-point-to-traffic-manager"></a>A 또는 AAAA 레코드에 대한 별칭 레코드 집합을 사용하여 Traffic Manager를 가리키는 경우 제한 사항이 있나요?

예. A 또는 AAAA 레코드 집합의 별칭으로 Traffic Manager 프로필을 가리키려면 Traffic Manager 프로필이 외부 엔드포인트만 사용해야 합니다. Traffic Manager에서 외부 엔드포인트를 만들 때 엔드포인트의 실제 IP 주소를 제공해야 합니다.

### <a name="is-there-an-additional-charge-to-use-alias-records"></a>별칭 레코드를 사용하기 위한 추가 비용이 있나요?

별칭 레코드는 유효한 DNS 레코드 집합으로 한정됩니다. 별칭 레코드에 대한 추가 비용은 청구되지 않습니다.

## <a name="use-azure-dns"></a>Azure DNS 사용

### <a name="can-i-co-host-a-domain-by-using-azure-dns-and-another-dns-provider"></a>Azure DNS 및 다른 DNS 공급자를 사용 하 여 도메인을 공동 호스트할 수 있나요?

예. Azure DNS에서는 다른 DNS 서비스와의 도메인 공동 호스팅을 지원합니다.

공동 호스팅을 설정 하려면 두 공급자의 이름 서버를 가리키도록 도메인의 NS 레코드를 수정 합니다. NS(이름 서버) 레코드는 도메인에 대한 DNS 쿼리를 수신하는 공급자를 제어합니다. Azure DNS, 다른 공급자 및 부모 영역에서 이러한 NS 레코드를 수정할 수 있습니다. 부모 영역은 일반적으로 도메인 이름 등록자를 통해 구성됩니다. DNS 위임에 대한 자세한 내용은 [DNS 도메인 위임](dns-domain-delegation.md)을 참조하세요.

또한 도메인에 대한 DNS 레코드가 두 DNS 공급자 간에 동기화되는지도 확인해야 합니다. Azure DNS는 현재 DNS 영역 전송을 지원하지 않습니다. DNS 레코드는 [Azure DNS 관리 포털](dns-operations-recordsets-portal.md), [REST API](https://docs.microsoft.com/powershell/module/az.dns), [SDK](dns-sdk.md), [PowerShell cmdlet](dns-operations-recordsets.md) 또는 [CLI 도구](dns-operations-recordsets-cli.md)를 사용해서 DNS 동기화해야 합니다.

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>4개의 Azure DNS 이름 서버 모두에 도메인을 위임해야 하나요?

예. Azure DNS는 각 DNS 영역에 4개의 이름 서버를 할당합니다. 이 정렬은 오류 격리 및 복원력 증가를 위한 것입니다. Azure DNS SLA를 충족하려면 4개의 이름 서버 모두에 도메인을 위임합니다.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Azure DNS에 대한 사용량 제한은 얼마나 되나요?

Azure DNS를 사용할 경우 다음과 같은 기본 제한이 적용됩니다.

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>리소스 그룹 또는 구독 간에 Azure DNS 영역을 이동할 수 있나요?

예. 리소스 그룹 또는 구독 간에 DNS 영역을 이동할 수 있습니다.

DNS 영역을 이동할 때 DNS 쿼리에 영향을 미치지 않습니다. 영역에 할당된 이름 서버는 동일하게 유지됩니다. DNS 쿼리는 전체적으로 일반적인 방식대로 처리됩니다.

DNS 영역을 이동하는 방법에 대한 자세한 내용 및 지침을 보려면 [새 리소스 그룹 또는 구독으로 리소스 이동](../azure-resource-manager/resource-group-move-resources.md)을 참조하세요.

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>DNS 변경 내용이 적용되는 데 시간이 얼마나 걸리나요?

새 DNS 영역 및 DNS 레코드는 일반적으로 Azure DNS 이름 서버에 빠르게 표시됩니다. 즉, 몇 초 안에 표시됩니다.

기존 DNS 레코드 변경의 경우 시간이 좀 더 오래 걸릴 수 있습니다. 일반적으로 이러한 변경 내용은 60초 이내에 Azure DNS 이름 서버에 표시됩니다. Azure DNS 외부의 DNS 클라이언트 및 DNS 재귀 확인자에 의한 DNS 캐싱도 타이밍에 영향을 줄 수 있습니다. 이러한 캐시 기간을 제어하려면 각 레코드 집합의 TTL(Time-To-Live) 속성을 사용합니다.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>실수로 삭제되지 않도록 내 DNS 영역을 보호하려면 어떻게 해야 하나요?

Azure DNS는 Azure Resource Manager를 사용하여 관리됩니다. Azure DNS는 Azure Resource Manager가 제공하는 Access Control 기능을 활용합니다. 역할 기반 액세스 제어는 DNS 영역 및 레코드 집합에 대한 읽기 또는 쓰기 액세스 권한이 있는 사용자를 제어합니다. 리소스 잠금은 실수로 DNS 영역 및 레코드 집합을 수정하거나 삭제하지 않도록 합니다.

자세한 내용은 [DNS 영역 및 레코드 보호](dns-protect-zones-recordsets.md)를 참조하세요.

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Azure DNS에서 SPF 레코드를 설정하려면 어떻게 해야 하나요?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-name-servers-resolve-over-ipv6"></a>Azure DNS 이름 서버가 IPv6를 통해 확인하나요? 

예. Azure DNS 이름 서버는 이중 스택입니다. 이중 스택이라는 것은 IPv4 및 IPv6 주소를 둘 다 보유함을 의미합니다. DNS 영역에 할당된 Azure DNS 이름 서버에 대한 IPv6 주소를 찾으려면 nslookup과 같은 도구를 사용합니다. 예는 `nslookup -q=aaaa <Azure DNS Nameserver>`입니다.

### <a name="how-do-i-set-up-an-idn-in-azure-dns"></a>Azure DNS에서 IDN을 설정하려면 어떻게 해야 하나요?

IDN(국제 도메인 이름)은 [punycode](https://en.wikipedia.org/wiki/Punycode)를 사용하여 각 DNS 이름을 인코딩합니다. DNS 쿼리는 이러한 punycode로 인코딩된 이름을 사용하여 수행됩니다.

Azure DNS에서 IDN을 구성하려면 영역 이름 또는 레코드 집합 이름을 punycode로 변환합니다. Azure DNS는 현재 punycode로의 또는 punycode로부터의 기본 제공 변환을 지원하지 않습니다.

## <a name="next-steps"></a>다음 단계

- [Azure DNS에 대해 자세히 알아봅니다](dns-overview.md).

- [프라이빗 도메인에 Azure DNS를 사용하는 방법에 대해 자세히 알아봅니다](private-dns-overview.md).

- [DNS 영역 및 레코드에 대해 자세히 알아봅니다](dns-zones-records.md).

- [Azure DNS 시작](dns-getstarted-portal.md)

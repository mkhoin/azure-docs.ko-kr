---
title: '자습서: Azure Active Directory를 사용 하 여 자동 사용자 프로 비전을 위한 StarLeaf 구성 Microsoft Docs'
description: 사용자 계정을 StarLeaf로 자동으로 프로 비전 및 프로 비전 해제 하도록 Azure Active Directory를 구성 하는 방법에 대해 알아봅니다.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2019
ms.author: zhchia
ms.openlocfilehash: 0676eb2830c2e0233eb182cfec0be3f39c6a39e9
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68737746"
---
# <a name="tutorial-configure-starleaf-for-automatic-user-provisioning"></a>자습서: 자동 사용자 프로 비전을 위한 StarLeaf 구성

이 자습서에서는 사용자 및/또는 그룹을 StarLeaf로 자동으로 프로 비전 및 프로 비전 해제 하도록 Azure AD를 구성 하기 위해 StarLeaf 및 Azure Active Directory (Azure AD)에서 수행 하는 단계를 보여 줍니다.

> [!NOTE]
>  이 자습서에서는 Azure AD 사용자 프로비저닝 서비스에 기반하여 구축된 커넥터에 대해 설명합니다. 이 서비스의 기능, 작동 방법 및 질문과 대답에 대한 중요한 내용은 [Azure Active Directory를 사용하여 SaaS 애플리케이션의 사용자를 자동으로 프로비저닝 및 프로비저닝 해제](../manage-apps/user-provisioning.md)를 참조하세요.
>
> 이 커넥터는 현재 미리 보기 상태입니다. 미리 보기 기능에 대 한 일반 Microsoft Azure 사용 약관에 대 한 자세한 내용은 [Microsoft Azure 미리 보기에 대 한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조 하세요.

## <a name="prerequisites"></a>전제 조건

이 자습서에 설명된 시나리오에서는 사용자에게 이미 다음 필수 구성 요소가 있다고 가정합니다.

* Azure AD 테넌트.
* [StarLeaf 테 넌 트](https://www.starleaf.com/solutions/)입니다.
* 관리자 권한이 있는 StarLeaf의 사용자 계정

## <a name="assign-users-to-starleaf"></a>StarLeaf에 사용자 할당
Azure Active Directory는 할당 이라는 개념을 사용 하 여 선택한 앱에 대 한 액세스 권한을 받아야 하는 사용자를 결정 합니다. 자동 사용자 프로 비전의 컨텍스트에서는 Azure AD의 응용 프로그램에 할당 된 사용자 및/또는 그룹만 동기화 됩니다.

자동 사용자 프로비저닝을 구성 하 고 사용 하도록 설정 하기 전에 Azure AD에서 StarLeaf에 대 한 액세스 권한이 필요한 사용자 및 그룹을 결정 해야 합니다. 그런 다음 [이러한 지침](../manage-apps/assign-user-or-group-access-portal.md)에 따라 사용자 및 그룹을 starleaf에 할당할 수 있습니다.

## <a name="important-tips-for-assigning-users-to-starleaf"></a>StarLeaf에 사용자를 할당 하기 위한 주요 팁

* 자동 사용자 프로 비전 구성을 테스트 하기 위해 단일 Azure AD 사용자를 StarLeaf에 할당 하는 것이 좋습니다. 추가 사용자 및 그룹은 나중에 할당할 수 있습니다.

* StarLeaf에 사용자를 할당 하는 경우 할당 대화 상자에서 유효한 응용 프로그램별 역할 (사용 가능한 경우)을 선택 해야 합니다. 기본 액세스 역할이 있는 사용자는 프로 비전에서 제외 됩니다.

## <a name="set-up-starleaf-for-provisioning"></a>프로 비전을 위한 StarLeaf 설정

Azure AD를 사용 하 여 자동 사용자 프로 비전을 위한 StarLeaf를 구성 하기 전에 StarLeaf에서 SCIM 프로 비전을 구성 해야 합니다.

1. [Starleaf 관리 콘솔](https://portal.starleaf.com/#page=login)에 로그인 합니다. 통합 > **통합 추가**로 이동 합니다.

    ![StarLeaf 추가 SCIM](media/starleaf-provisioning-tutorial/image00.png)

2. Microsoft Azure Active Directory **형식을** 선택 합니다. **이름**에 적절 한 이름을 입력 합니다. **적용**을 클릭합니다.

    ![StarLeaf 추가 SCIM](media/starleaf-provisioning-tutorial/image01.png)

3.  그러면 **Scim 기준 URL** 및 **액세스 토큰** 값이 표시 됩니다. 이러한 값은 Azure Portal에서 StarLeaf 응용 프로그램의 프로 비전 탭에 있는 **테 넌 트 URL** 및 **비밀 토큰** 필드에 입력 됩니다. 

    ![StarLeaf 토큰 만들기](media/starleaf-provisioning-tutorial/image02.png)

## <a name="add-starleaf-from-the-gallery"></a>갤러리에서 StarLeaf 추가

Azure AD를 사용한 자동 사용자 프로 비전을 위한 StarLeaf를 구성 하려면 Azure AD 응용 프로그램 갤러리의 StarLeaf를 관리 되는 SaaS 응용 프로그램 목록에 추가 해야 합니다.

**Azure AD 응용 프로그램 갤러리에서 StarLeaf를 추가 하려면 다음 단계를 수행 합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 패널에서 **Azure Active Directory**를 선택 합니다.

    ![Azure Active Directory 단추](common/select-azuread.png)

2. **엔터프라이즈 애플리케이션**으로 이동한 다음, **모든 애플리케이션**을 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

3. 새 응용 프로그램을 추가 하려면 창의 위쪽에 있는 **새 응용 프로그램** 단추를 선택 합니다.

    ![새 애플리케이션 단추](common/add-new-app.png)

4. 검색 상자에서 **starleaf**를 입력 하 고 결과 패널에서 **starleaf** 를 선택 합니다.
    ![결과 목록의 StarLeaf](common/search-new-app.png)

## <a name="configure-automatic-user-provisioning-to-starleaf"></a>StarLeaf에 대 한 자동 사용자 프로 비전 구성

이 섹션에서는 azure ad의 사용자 및/또는 그룹 할당을 기반으로 StarLeaf에서 사용자 및/또는 그룹을 만들고, 업데이트 하 고, 비활성화 하도록 Azure AD 프로 비전 서비스를 구성 하는 단계를 안내 합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다. **엔터프라이즈 응용 프로그램**을 선택한 다음 **모든 응용 프로그램**을 선택 합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

2. 응용 프로그램 목록에서 **Starleaf**를 선택 합니다.

    ![응용 프로그램 목록의 StarLeaf 링크](common/all-applications.png)

3. **프로비전** 탭을 선택합니다.

    ![프로 비전 탭](common/provisioning.png)

4. **프로비전 모드**를 **자동**으로 설정합니다.

    ![프로 비전 탭](common/provisioning-automatic.png)

5. 관리자 자격 증명 섹션 아래에서 먼저 **테 넌 트 url** 및 **암호 토큰** 에서 검색 된 **Scim 기준 url** 및 **액세스 토큰** 값을 입력 합니다. **연결 테스트** 를 클릭 하 여 Azure AD가 starleaf에 연결할 수 있는지 확인 합니다. 연결에 실패 하면 StarLeaf 계정에 관리자 권한이 있는지 확인 하 고 다시 시도 합니다.

    ![테 넌 트 URL + 토큰](common/provisioning-testconnection-tenanturltoken.png)

6. **알림 전자 메일** 필드에 프로 비전 오류 알림을 받을 개인 또는 그룹의 전자 메일 주소를 입력 하 고 **오류가 발생 하면 전자 메일 알림 보내기** 상자를 선택 합니다.

    ![알림 전자 메일](common/provisioning-notification-email.png)

7. **Save**을 클릭합니다.

8. **매핑** 섹션에서 **사용자 Azure Active Directory Starleaf에 동기화를**선택 합니다.

    ![StarLeaf 토큰 만들기](media/starleaf-provisioning-tutorial/usermapping.png)

9. **특성 매핑** 섹션에서 Azure AD에서 starleaf로 동기화 되는 사용자 특성을 검토 합니다. **일치** 속성으로 선택한 특성은 업데이트 작업을 위해 starleaf의 사용자 계정을 일치 시키는 데 사용 됩니다. **저장** 단추를 선택하여 변경 내용을 커밋합니다.

    ![StarLeaf 토큰 만들기](media/starleaf-provisioning-tutorial/userattribute.png)


10. 범위 지정 필터를 구성하려면 [범위 지정 필터 자습서](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)에서 제공하는 다음 지침을 참조합니다.


11. StarLeaf에 대 한 Azure AD 프로 비전 서비스를 사용 하도록 **설정 하려면 설정** 섹션에서 **프로 비전 상태** 를 **켜기** 로 변경 합니다.

    ![프로 비전 상태 전환 설정](common/provisioning-toggle-on.png)

12. **설정** 섹션의 **범위** 에서 원하는 값을 선택 하 여 starleaf에 프로 비전 하려는 사용자 및/또는 그룹을 정의 합니다.

    ![프로 비전 범위](common/provisioning-scope.png)

13. 프로비전할 준비가 되면 **저장**을 클릭합니다.

    ![프로 비전 구성 저장 중](common/provisioning-configuration-save.png)

이 작업은 **설정**의 **범위** 섹션에 정의된 모든 사용자 및/또는 그룹의 초기 동기화를 시작합니다. 초기 동기화는 Azure AD 프로비전 서비스가 실행되는 동안 약 40분마다 발생하는 후속 동기화보다 더 많은 시간이 걸립니다. **동기화 세부 정보** 섹션을 사용 하 여 진행률을 모니터링 하 고 프로 비전 활동 보고서에 대 한 링크를 따라 Starleaf의 Azure AD 프로 비전 서비스에서 수행 하는 모든 작업을 설명 합니다.

Azure AD 프로 비전 로그를 읽는 방법에 대 한 자세한 내용은 [자동 사용자 계정 프로 비전에 대 한 보고](../manage-apps/check-status-user-account-provisioning.md) 를 참조 하세요.

## <a name="connector-limitations"></a>커넥터 제한 사항

* StarLeaf는 현재 그룹 프로 비전을 지원 하지 않습니다. 
* StarLeaf를 사용 하려면 **전자 메일** 및 **사용자 이름** 값이 동일한 원본 값을 가져야 합니다.

## <a name="additional-resources"></a>추가 자료

* [엔터프라이즈 앱에 대한 사용자 계정 프로비전 관리](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>다음 단계

* [로그를 검토 하 고 프로 비전 활동에 대 한 보고서를 가져오는](../manage-apps/check-status-user-account-provisioning.md)방법에 대해 알아봅니다.

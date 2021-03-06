---
title: Azure IoT Central 앱에 대 한 장치 템플릿 버전 관리 이해 Microsoft Docs
description: 라이브 연결 장치에 영향을 주지 않고 새 버전을 만들어 장치 템플릿 반복
author: sarahhubbard
ms.author: sahubbar
ms.date: 12/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 1f2ecb37ebe156b1eb092bda95f296c39c9e2baf
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74974967"
---
# <a name="create-a-new-device-template-version-preview-features"></a>새 장치 템플릿 버전 만들기 (미리 보기 기능)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Azure IoT Central는 IoT 응용 프로그램을 신속 하 게 개발할 수 있습니다. 장치 기능, 보기 및 사용자 지정을 추가, 편집 또는 삭제 하 여 장치 템플릿 디자인을 신속 하 게 반복할 수 있습니다. 장치 템플릿을 게시 한 후에는 장치 기능 모델이 모델 옆에 자물쇠 아이콘이 **게시** 된 것으로 표시 됩니다. 장치 기능 모델을 변경 하려면 새 버전의 장치 템플릿을 만들어야 합니다. 한편, 클라우드 속성, 사용자 지정 및 보기는 장치 템플릿 버전을 사용 하지 않고도 언제 든 지 편집할 수 있습니다. 이러한 변경 내용을 저장 한 후에는 장치 템플릿을 게시 하 여 운영자가 Device Explorer에서 볼 수 있는 최신 변경을 수행할 수 있습니다.

> [!NOTE]
> 장치 템플릿을 만드는 방법에 대해 자세히 알아보려면 [장치 템플릿 설정 및 관리](howto-set-up-template.md) 를 참조 하세요.

## <a name="add-customizations-to-the-device-template-without-versioning"></a>버전 관리 없이 장치 템플릿에 사용자 지정 추가

장치 기능의 특정 요소는 장치 템플릿 및 인터페이스의 버전이 없어도 편집할 수 있습니다. 예를 들어 이러한 필드 중 일부에는 표시 이름, 의미 체계 형식, 최소값, 최대값, 소수 자릿수, 색, 단위, 표시 단위, 주석 및 설명이 포함 됩니다. 다음 사용자 지정 중 하나를 추가 하려면:

1. **장치 템플릿** 페이지로 이동 합니다.
1. 사용자 지정 하려는 장치 템플릿을 선택 합니다.
1. **사용자 지정** 탭을 선택 합니다.
1. 장치 기능 모델에 정의 된 모든 기능이 여기에 나열 됩니다. 여기에서 편집할 수 있는 모든 필드는 장치 템플릿의 버전을 지정할 필요 없이 응용 프로그램 전체에 저장 하 고 사용할 수 있습니다. 읽기 전용 필드를 편집 하려는 경우 해당 필드를 변경 하려면 장치 템플릿의 버전을 변경 해야 합니다. 편집할 필드를 선택 하 고 새 값을 입력 합니다.
1. Kattintson a **Save** (Mentés) gombra. 이제 이러한 값은 장치 템플릿에 처음 저장 되었으며 응용 프로그램 전체에서 사용 되는 모든 항목을 재정의 합니다.

## <a name="versioning-a-device-template"></a>장치 템플릿 버전 관리

새 버전의 장치 템플릿을 만들면 장치 기능 모델을 편집할 수 있는 템플릿 초안 버전이 만들어집니다. 게시 된 모든 인터페이스는 개별적으로 버전이 지정 될 때까지 게시 된 상태로 유지 됩니다. 게시 된 인터페이스를 수정 하려면 먼저 새 장치 템플릿 버전을 만들어야 합니다.

장치 템플릿의 사용자 지정 섹션에서 편집할 수 없는 장치 기능 모델의 일부를 편집 하려는 경우에만 장치 템플릿을 버전으로 지정 해야 합니다. 

장치 템플릿 버전을 만들려면 다음을 수행 합니다.

1. **장치 템플릿** 페이지로 이동 합니다.
1. 버전을 만들려고 하는 장치 템플릿을 선택 합니다.
1. 페이지 위쪽의 **버전** 단추를 클릭 하 고 템플릿에 새 이름을 지정 합니다. 편집할 수 있는 새로운 이름을 제안 했습니다.
1. Kattintson a  **Create** (Létrehozás) gombra.
1. 이제 장치 템플릿이 초안 모드입니다. 인터페이스는 여전히 잠겨 있으며 편집 하려면 개별적으로 버전을 지정 해야 합니다. 

### <a name="versioning-an-interface"></a>인터페이스 버전 관리

인터페이스 버전 관리에서는 이미 만든 인터페이스 내에서 기능을 추가, 업데이트 및 제거할 수 있습니다. 

인터페이스 버전을 만들려면 다음을 수행 합니다.

1. **장치 템플릿** 페이지로 이동 합니다.
1. 초안 모드에서 장치 템플릿을 선택 합니다.
1. 버전을 선택 하 고 편집 하려는 게시 된 모드의 인터페이스를 선택 합니다.
1. 인터페이스 페이지 위쪽에서 **버전** 단추를 클릭 합니다. 
1. Kattintson a  **Create** (Létrehozás) gombra.
1. 이제 인터페이스가 초안 모드입니다. 기존 사용자 지정 및 보기를 중단 하지 않고 인터페이스에 기능을 추가 하거나 편집할 수 있습니다. 

> [!NOTE]
> Azure IoT에서 게시 된 표준 인터페이스는 버전을 만들거나 편집할 수 없습니다. 이러한 표준 인터페이스는 장치 인증에 사용 됩니다.

> [!NOTE]
> 인터페이스를 게시 한 후에는 초안 모드 에서도 해당 기능을 삭제할 수 없습니다. 기능은 초안 모드의 인터페이스에만 편집 하거나 추가할 수 있습니다.


## <a name="migrate-a-device-across-device-template-versions"></a>장치 템플릿 버전에서 장치 마이그레이션

여러 버전의 장치 템플릿을 만들 수 있습니다. 시간이 지남에 따라 이러한 장치 템플릿을 사용 하 여 여러 장치를 연결 합니다. 장치 템플릿의 한 버전에서 다른 버전으로 장치를 마이그레이션할 수 있습니다. 다음 단계는 장치를 마이그레이션하는 방법을 설명 합니다.

1. **Device Explorer** 페이지로 이동 합니다.
1. 다른 버전으로 마이그레이션해야 하는 장치를 선택 합니다.
1. **마이그레이션**을 선택 합니다.
1. 장치를 마이그레이션하려는 버전 번호가 포함 된 장치 템플릿을 선택 하 고 **마이그레이션**을 선택 합니다.

![장치를 마이그레이션하는 방법](media/howto-version-device-template/pick-version.png)

## <a name="next-steps"></a>Következő lépések

이제 Azure IoT Central 응용 프로그램에서 장치 템플릿 버전을 사용 하는 방법을 배웠으므로 제안 된 다음 단계는 다음과 같습니다.

> [!div class="nextstepaction"]
> [원격 분석 규칙을 만드는 방법](tutorial-create-telemetry-rules.md)

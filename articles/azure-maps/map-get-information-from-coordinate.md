---
title: Azure Maps를 사용하여 좌표에 대한 정보 표시 | Microsoft Docs
description: 사용자가 좌표를 선택할 경우 맵에서 주소에 대한 정보를 표시하는 방법
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: df0966569a753d5000414451a2b69f1e69449b2c
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638671"
---
# <a name="get-information-from-a-coordinate"></a>좌표에서 정보 가져오기

이 문서에서는 클릭한 팝업 위치의 주소를 표시하는 역방향 주소 검색을 수행하는 방법을 보여줍니다.

역방향 주소 검색을 수행하는 두 가지 방법이 있습니다. 한 가지 방법은 서비스 모듈을 통해 [Azure Maps 역방향 주소 검색 API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)를 쿼리하는 것입니다. 다른 방법은 [FETCH api](https://fetch.spec.whatwg.org/) 를 활용 하 여 [Azure Maps 역방향 주소 검색 api](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 를 요청 하 여 주소를 찾는 것입니다. 두 가지 방법 모두 아래에서 조사합니다.

## <a name="make-a-reverse-search-request-via-service-module"></a>서비스 모듈을 통해 역방향 검색 요청

<iframe height='500' scrolling='no' title='좌표에서 정보 가져오기(서비스 모듈)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Azure Maps(<a href='https://codepen.io/azuremaps'>@azuremaps</a>)로 펜 <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>좌표에서 정보 가져오기(서비스 모듈)</a>를 참조하세요.
</iframe>

위의 코드에서 첫 번째 코드 블록은 맵 개체를 생성 하 고 액세스 토큰을 활용 하도록 인증 메커니즘을 설정 합니다. 지침은 [지도 만들기](./map-create.md)를 참조하세요.

두 번째 코드 블록은 액세스 토큰 `TokenCredential` 을 사용 하 여 Azure Maps 하기 위해 HTTP 요청을 인증 하는을 만듭니다. 그런 다음 `TokenCredential` 를에 `atlas.service.MapsURL.newPipeline()` 전달 하 고 [파이프라인](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) 인스턴스를 만듭니다. `searchURL`은 Azure Maps [검색](https://docs.microsoft.com/rest/api/maps/search) 작업에 대한 URL을 나타냅니다.

세 번째 코드 블록은 마우스 커서의 스타일을 포인터로 업데이트 하 고 [popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) 개체를 만듭니다. [맵에서 팝업 추가](./map-add-popup.md)에서 지침을 확인할 수 있습니다.

네 번째 코드 블록은 마우스 클릭 [이벤트 수신기](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events)를 추가 합니다. 트리거되면 클릭 한 지점의 좌표를 사용 하 여 검색 쿼리를 만듭니다. 그런 다음 서비스 모듈의 [Getsearchaddressreverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest#searchaddressreverse-aborter--geojson-position--searchaddressreverseoptions-) 메서드를 사용 하 여 좌표 주소에 대 한 [Get SEARCH Address 역방향 API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 를 쿼리 합니다. 그런 다음 메서드를 `geojson.getFeatures()` 사용 하 여 응답에서 GeoJSON feature 컬렉션을 추출 합니다.

다섯 번째 코드 블록은 HTML 팝업 콘텐츠를 설정 하 여 클릭 한 좌표 위치의 응답 주소를 표시 합니다.

커서, 팝업 개체 및 클릭 이벤트의 변경은 좌표 정보를 검색하기 전에 지도가 완전히 로드될 수 있도록 지도의 [이벤트 수신기 로드](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events)에서 모두 수행됩니다.

## <a name="make-a-reverse-search-request-via-fetch-api"></a>Fetch API를 통해 역방향 검색 요청 만들기

Fetch를 사용 하 여 해당 위치에 대 한 역방향 geocode 요청을 만들려면 맵을 클릭 합니다.

<iframe height='500' scrolling='no' title='좌표에서 정보 가져오기' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen</a>에서 Azure Maps(<a href='https://codepen.io/azuremaps'>@azuremaps</a>)로 펜 <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>좌표에서 정보 가져오기</a>를 참조하세요.
</iframe>

위의 코드에서 첫 번째 코드 블록은 맵 개체를 생성 하 고 액세스 토큰을 활용 하도록 인증 메커니즘을 설정 합니다. 지침은 [지도 만들기](./map-create.md)를 참조하세요.

두 번째 코드 블록은 포인터 및 [popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) 개체에 마우스 커서의 스타일을 업데이트합니다. [맵에서 팝업 추가](./map-add-popup.md)에서 지침을 확인할 수 있습니다.

세 번째 코드 블록은 마우스 클릭에 대한 이벤트 수신기를 추가합니다. 마우스 클릭 시 [FETCH api](https://fetch.spec.whatwg.org/) 를 활용 하 여 클릭 한 좌표 주소에 대해 [Azure Maps 역방향 주소 검색 api](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 를 쿼리 합니다. 성공적인 응답의 경우 클릭한 위치에 대한 주소를 수집하고 팝업 클래스의 [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) 함수를 통해 팝업 콘텐츠 및 위치를 정의합니다.

커서, 팝업 개체 및 클릭 이벤트의 변경은 좌표 정보를 검색하기 전에 지도가 완전히 로드될 수 있도록 지도의 [이벤트 수신기 로드](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events)에서 모두 수행됩니다.

## <a name="next-steps"></a>다음 단계

이 문서에서 사용된 클래스 및 메서드에 대해 자세히 알아봅니다.

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

전체 코드 예제는 다음 문서를 참조하세요.

> [!div class="nextstepaction"]
> [A에서 B로의 방향 표시](./map-route.md)

> [!div class="nextstepaction"]
> [트래픽 표시](./map-show-traffic.md)

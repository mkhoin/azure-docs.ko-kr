---
title: 'CSV로 변환: 모듈 참조'
titleSuffix: Azure Machine Learning
description: Azure Machine Learning에서 CSV로 변환 모듈을 사용 하 여 R 또는 Python 스크립트 모듈에서 다운로드, 내보내기 또는 공유할 수 있는 CSV 형식으로 데이터 집합을 변환 하는 방법에 대해 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 999f78ab08e1a2c9dd6b28d853e49fbb559fab83
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73493843"
---
# <a name="convert-to-csv-module"></a>CSV 모듈로 변환

이 문서에서는 Azure Machine Learning designer (미리 보기)의 모듈을 설명 합니다.

이 모듈을 사용 하 여 R 또는 Python 스크립트 모듈에서 다운로드, 내보내기 또는 공유할 수 있는 CSV 형식으로 데이터 집합을 변환할 수 있습니다.

### <a name="more-about-the-csv-format"></a>CSV 형식에 대 한 자세한 정보 

"쉼표로 구분 된 값"을 의미 하는 CSV 형식은 여러 외부 기계 학습 도구에서 사용 하는 파일 형식입니다. CSV는 R 또는 Python과 같은 오픈 소스 언어로 작업할 때 일반적인 교환 형식입니다.

Azure Machine Learning에서 대부분의 작업을 수행 하는 경우에도 외부 도구에서 사용 하기 위해 데이터 집합을 CSV로 변환 하는 것이 유용할 수 있습니다. 예:

+ CSV 파일을 다운로드 하 여 Excel에서 열거나 관계형 데이터베이스로 가져옵니다.  
+ CSV 파일을 클라우드 저장소에 저장 하 고 Power BI에서 연결 하 여 시각화를 만듭니다.  
+ CSV 형식을 사용 하 여 R 및 Python에서 사용할 데이터를 준비 합니다. 모듈의 출력을 마우스 오른쪽 단추로 클릭 하 여 Python 또는 Jupyter 노트북에서 직접 데이터에 액세스 하는 데 필요한 코드를 생성 하면 됩니다. 

데이터 집합을 CSV로 변환 하면 파일이 Azure ML 작업 영역에 저장 됩니다. Azure storage 유틸리티를 사용 하 여 파일을 직접 열고 사용할 수 있습니다. 또는 모듈 출력을 마우스 오른쪽 단추로 클릭 하 고 CSV 파일을 컴퓨터에 다운로드 하거나 R 또는 Python 코드에서 사용할 수 있습니다.  

## <a name="how-to-configure-convert-to-csv"></a>CSV로 변환 구성 방법


1.  [CSV로 변환](./convert-to-csv.md) 모듈을 파이프라인에 추가 합니다. 이 모듈은 디자이너의 **데이터 형식 변환** 그룹에서 찾을 수 있습니다. 

2. 데이터 집합을 출력 하는 모듈에 연결 합니다.   
  
3.  파이프라인을 실행합니다.

### <a name="results"></a>결과
  

[CSV로 변환](./convert-to-csv.md)의 출력을 두 번 클릭 하 고 다음 옵션 중 하나를 선택 합니다.  

 + **결과 데이터 집합-> Download**: 로컬 폴더에 저장할 수 있는 CSV 형식의 데이터 복사본을 즉시 엽니다. 폴더를 지정 하지 않으면 기본 파일 이름이 적용 되 고 CSV 파일이 로컬 **다운로드** 라이브러리에 저장 됩니다.


 + **결과 데이터 집합-> 데이터 집합으로 저장**: CSV 파일을 별도의 데이터 집합으로 Azure ML 작업 영역에 다시 저장 합니다.

 + **데이터 액세스 코드 생성**: Azure ML은 Python 또는 R을 사용 하 여 데이터에 액세스 하는 두 가지 코드 집합을 생성 합니다. 데이터에 액세스 하려면 코드 조각을 응용 프로그램에 복사 합니다. (*데이터 액세스 코드 생성은 곧 제공 될 예정입니다.* )

## <a name="next-steps"></a>다음 단계

Azure Machine Learning [사용할 수 있는 모듈 집합](module-reference.md) 을 참조 하세요. 
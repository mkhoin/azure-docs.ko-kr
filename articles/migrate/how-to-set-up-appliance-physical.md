---
title: 물리적 서버에 대 한 Azure Migrate 어플라이언스 설정
description: 물리적 서버 평가를 위해 Azure Migrate 어플라이언스를 설정 하는 방법에 대해 알아봅니다.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/19/2019
ms.author: raynew
ms.openlocfilehash: db67defc72dcc7d913f897c6fb61548c5c33cf52
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74278324"
---
# <a name="set-up-an-appliance-for-physical-servers"></a>물리적 서버용 어플라이언스 설정

이 문서에서는 Azure Migrate: 서버 평가 도구를 사용 하 여 물리적 서버를 평가 하는 경우 Azure Migrate 어플라이언스를 설정 하는 방법을 설명 합니다.

> [!NOTE]
> Azure Migrate 포털에 표시 되지 않는 기능을 여기에서 설명 하는 경우에는 중단 합니다. 이는 다음 주 정도에 표시됩니다.

Azure Migrate 어플라이언스는 Azure Migrate Server 평가에서 다음을 수행 하는 데 사용 하는 경량 어플라이언스입니다.

- 온-프레미스 서버를 검색 합니다.
- 검색 된 서버에 대 한 메타 데이터 및 성능 데이터를 Azure Migrate 서버 평가로 보냅니다.

Azure Migrate 어플라이언스에 [대해 자세히 알아보세요](migrate-appliance.md) .


## <a name="appliance-deployment-steps"></a>어플라이언스 배포 단계

어플라이언스를 설정하려면 다음을 수행합니다.
- Azure Portal에서 Azure Migrate 설치 프로그램 스크립트가 포함된 압축 파일을 다운로드합니다.
- 압축 파일의 콘텐츠를 추출합니다. 관리자 권한으로 PowerShell 콘솔을 시작합니다.
- PowerShell 스크립트를 실행하여 어플라이언스 웹 애플리케이션을 시작합니다.
- 어플라이언스를 처음으로 구성하고, Azure Migrate 프로젝트에 등록합니다.

## <a name="download-the-installer-script"></a>설치 프로그램 스크립트 다운로드

어플라이언스에 대한 압축 파일을 다운로드합니다.

1. **마이그레이션 목표** > **서버** > **Azure Migrate: 서버 평가**에서 **검색**을 클릭 합니다.
2. **머신 검색** > **머신이 가상화되어 있습니까?** 에서 **가상화되지 않음/기타**를 클릭합니다.
3. **다운로드**를 클릭하여 압축 파일을 다운로드합니다.

    ![VM 다운로드](./media/how-to-set-up-appliance-hyper-v/download-appliance-hyperv.png)


### <a name="verify-security"></a>보안 확인

배포하기 전에 압축된 파일이 안전한지 확인합니다.

1. 파일을 다운로드한 컴퓨터에서 관리자 명령 창을 엽니다.
2. 다음 명령을 실행하여 VHD에 대한 해시를 생성합니다.
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - 사용 예: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3.  최신 어플라이언스 버전의 경우 생성 된 해시가 이러한 설정과 일치 해야 합니다.

  **알고리즘** | **해시 값**
  --- | ---
  MD5 | 96fd99581072c400aa605ab036a0a7c0
  SHA256 | f5454beef510c0aa38ac1c6be6346207c351d5361afa0c9cea4772d566fcdc36



## <a name="run-the-azure-migrate-installer-script"></a>Azure Migrate 설치 프로그램 스크립트 실행
설치 프로그램 스크립트에서 수행하는 작업은 다음과 같습니다.

- 물리적 서버 검색 및 평가를 위한 에이전트와 웹 애플리케이션을 설치합니다.
- Windows 정품 인증 서비스, IIS 및 PowerShell ISE를 비롯한 Windows 역할을 설치합니다.
- IIS 재작성 모듈을 다운로드하여 설치합니다. [자세히 알아봅니다](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure Migrate에 대한 영구적인 설정 세부 정보를 사용하여 레지스트리 키(HKLM)를 업데이트합니다.
- 지정된 경로에 다음 파일을 만듭니다.
    - **구성 파일**: %Programdata%\Microsoft Azure\Config
    - **로그 파일**: %Programdata%\Microsoft Azure\Logs

스크립트를 다음과 같이 실행합니다.

1. 어플라이언스를 호스팅할 서버의 폴더에 압축 파일을 추출합니다.
2. 위 서버에서 관리자(상승된) 권한을 사용하여 PowerShell을 시작합니다.
3. 다운로드한 압축 파일에서 콘텐츠를 추출한 폴더로 PowerShell 디렉터리를 변경합니다.
4. 다음 명령을 실행하여 스크립트를 실행합니다.
    ```
    PS C:\Users\Administrators\Desktop> AzureMigrateInstaller-physical.ps1
    ```
스크립트가 성공적으로 완료되면 어플라이언스 웹 애플리케이션이 시작됩니다.



### <a name="verify-appliance-access-to-azure"></a>Azure에 대한 어플라이언스 액세스 확인

어플라이언스 VM이 필요한 [Azure url](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access)에 연결할 수 있는지 확인 합니다.

## <a name="configure-the-appliance"></a>어플라이언스 구성

어플라이언스를 처음으로 설정합니다.

1. VM에 연결할 수 있는 모든 컴퓨터에서 브라우저를 열고 어플라이언스 웹 앱의 URL ( **https://*어플라이언스 이름 또는 IP 주소*: 44368**)을 엽니다.

   또는 바탕 화면에서 앱 바로 가기를 클릭하여 앱을 열 수 있습니다.
2. 웹앱 > **필수 구성 요소 설정**에서 다음을 수행합니다.
    - **라이선스**: 사용 조건에 동의 하 고 타사 정보를 읽습니다.
    - **연결**: 앱에서 VM이 인터넷에 연결 되어 있는지 확인 합니다. VM에서 프록시를 사용하는 경우:
        - **프록시 설정**을 클릭하고, 프록시 주소와 수신 포트를 http://ProxyIPAddress 또는 http://ProxyFQDN 형식으로 지정합니다.
        - 프록시에 인증이 필요한 경우 자격 증명을 지정합니다.
        - HTTP 프록시만 지원됩니다.
    - **시간 동기화**: 시간이 확인 됩니다. VM 검색이 제대로 작동하려면 어플라이언스의 시간이 인터넷 시간과 동기화되어야 합니다.
    - **업데이트 설치**: Azure Migrate 서버 평가는 어플라이언스에 최신 업데이트가 설치 되어 있는지 확인 합니다.

### <a name="register-the-appliance-with-azure-migrate"></a>Azure Migrate를 사용하여 어플라이언스 등록

1. **로그인**을 클릭합니다. 표시되지 않으면 브라우저에서 팝업 차단을 사용하지 않도록 설정했는지 확인합니다.
2. 새로 만들기 탭에서 Azure 자격 증명을 사용하여 로그인합니다.
    - 사용자 이름과 암호를 사용하여 로그인합니다.
    - PIN을 사용한 로그인은 지원되지 않습니다.
3. 성공적으로 로그인하면 웹앱으로 돌아갑니다.
4. Azure Migrate 프로젝트가 만들어진 구독을 선택합니다. 그런 다음, 해당 프로젝트를 선택합니다.
5. 어플라이언스의 이름을 지정합니다. 이름은 14자 이하의 영숫자여야 합니다.
6. **Register**를 클릭합니다.


## <a name="start-continuous-discovery"></a>연속 검색 시작

어플라이언스에서 물리적 서버로 연결 하 고 검색을 시작 합니다.

1. **자격 증명 추가**를 클릭하여 어플라이언스가 서버를 검색하는 데 사용할 계정 자격 증명을 지정합니다.  
2. **운영 체제**, 자격 증명의 식별 이름, **사용자 이름** 및 **암호**를 지정하고 **추가**를 클릭합니다.
Windows 및 Linux 서버에 대해 각각 자격 증명 집합 하나를 추가할 수 있습니다.
4. **서버 추가**를 클릭하고 서버에 연결하기 위한 서버 세부 정보(FQDN/IP 주소 및 자격 증명의 식별 이름)를 지정합니다(한 행에 한 항목).
3. **유효성 검사**를 클릭합니다. 유효성 검사 후 검색 가능한 서버 목록이 표시됩니다.
    - 서버에 대한 유효성 검사가 실패하면 마우스로 **상태** 열의 아이콘 위를 가리켜서 오류를 검토합니다. 문제를 해결하고, 유효성을 다시 검사합니다.
    - 서버를 제거하려면 > **삭제**를 선택합니다.
4. 유효성 검사가 완료되면 **저장 및 검색 시작**을 클릭하여 검색 프로세스를 시작합니다.

그러면 검색을 시작합니다. 검색된 VM의 메타데이터가 Azure Portal에 표시되는 데 약 15분이 걸립니다.

## <a name="verify-servers-in-the-portal"></a>포털에서 서버 확인

검색이 완료 되 면 서버가 포털에 표시 되는지 확인할 수 있습니다.

1. Azure Migrate 대시보드를 엽니다.
2. **Azure Migrate 서버** > **Azure Migrate: 서버 평가** 페이지에서 **검색 된 서버의**수를 표시 하는 아이콘을 클릭 합니다.


## <a name="next-steps"></a>다음 단계

Azure Migrate 서버 평가를 사용 하 여 [물리적 서버 평가](tutorial-assess-physical.md) 를 시험해 보세요.

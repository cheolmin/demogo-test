# Make Serverless Web Application

먼저 AWS Amplify를 이용하여 Static Website를 호스팅해보겠습니다. AWS Amplify 는 프론트엔드 웹 및 모바일 개발자가 다양한 AWS 서비스를 활용하는 유연성을 바탕으로 AWS에 풀 스택 애플리케이션을 손쉽게 구축, 배송 및 호스팅 할 수 있도록 지원하는 완전한 솔루션입니다. 본 세션에서는 AWS Amplify 에서 제공하는 Amplify Hosting을 이용하여 Static Website를 배포해보도록 하겠습니다.

1. 먼저 AWS Console에 로그인 후 AWS Amplify를 검색합니다.

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

2. AWS Amplify 페이지에 접속하면 소개 페이지를 확인할 수 있습니다. 시작하기 버튼을 누르면, Amplify Studio / Amplify Hosting을 선택할 수 있도록 redirection 됩니다. Amplify Studio는 웹 및 모바일 앱을 위한 프론트 및 백엔드 개발을 간소화하는 시각적 인터페이스이며, 인증, 데이터 및 스토리지를 사용하여 앱 백엔드를 구축하고 사용자 지정 UI 구성 요소를 생성한 다음 몇 단계만으로 앱에 통합합니다. 또한 금일 활용할 Amplify Hosting은 프론트엔드와 백엔드를 지속적으로 배포하고 전역에서 사용 가능한 CDN에서 호스팅하는 기능입니다. Amplify Hosting의 시작하기 버튼을 눌러 다음으로 이동합니다.

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

3. Amplify Hosting 시작하기를 클릭하면 아래와 같이 Github / Bitbucket / Codecommit 등의 레포지토리 및 직접 배포를 이용하여 웹 앱을 배포 및 호스팅할 수 있습니다. Git 공급자 없이 배포를 클릭하여 다음으로 이동합니다.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

4. 앱 이름에 구분할 수 있는 앱 이름을 부여하고, 환경 이름에는 리소스가 어떤 환경인지(dev , test, prod 등)를 입력합니다. 또한 프론트엔드를 구성하는 파일([app.zip](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7b668506-94d3-46f4-9627-76bbb40d8bcb/app.zip))을 다운로드하여 파일을 업로드하거나 S3에 저장한 파일의 경로를 지정하여 배포합니다. 환경 이름은 Optional 이므로 이번 세션에서는 prod로 설정하여 배포를 진행합니다.

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

5. 배포가 완료되면 호스팅한 브랜치의 빌드 세부 정보를 확인할 수 있고, 배포된 도메인의 주소를 확인할 수 있습니다.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

6. 도메인 주소를 클릭하거나 미리보기 화면을 클릭하면 호스팅된 웹페이지에 접속할 수 있습니다.

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

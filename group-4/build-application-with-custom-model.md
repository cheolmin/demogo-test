# Build application with custom model

## Demogo #4-1. overview

이번 세션에서는 이전 세션인 ##에서 생성한 pose estimation을 위한 sagemaker endpoint를 이용하여 웹 어플리케이션을 만들어보는 실습을 진행하겠습니다.

{% hint style="info" %}
이전 세션에서 MLOps까지 구성한 경우 ‘groupname-prod’ 라는 이름의 엔드포인트가 in-service 상태인지 확인이 필요합니다.

이전 세션에서 MLOps를 진행하지 않고 Sagemaker Notebook에서 Custom model을 바로 배포한 경우 배포 시 설정한 모델의 엔드포인트를 사용합니다.
{% endhint %}

이번 세션에서 생성할 웹 어플리케이션의 전체적인 아키텍처는 아래 그림과 같습니다.

(아키텍처 그림)

위 아키텍처의 흐름을 설명하면 아래와 같습니다.

> 1. 사용자는 AWS Amplify를 이용하여 호스팅한 웹앱에 Teacher / Student video를 입력한다.
> 2. 입력한 Video를 javascript SDK를 이용하여 S3 버킷에 업로드한다.
> 3. Teacher / Student 영상이 모두 업로드 되었다면, AWS Lambda 를 트리거하여 영상의 메타정보를 가져온다.
> 4. Lambda 함수는 영상을 로드하고, Sagemaker Endpoint에 프레임을 입력하여 자세를 추론한다.
> 5. 추론 결과를 이용하여 유사도를 계산하고, 값을 리턴한다.



각 단계에 해당하는 서비스를 하나씩 구성해보며, 웹 어플리케이션을 만들어보겠습니다.

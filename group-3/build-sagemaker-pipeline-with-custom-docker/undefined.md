# 사용자 지정 컨테이너 이미지 빌드

Amazon Sagemaker Studio Image Build CLI는 보조 빌드 환경을 설정해야 하는 필요성을 추상화하고 Docker 빌드를 위한 워크플로우를 만드는 대신 해결하려는 ML 문제에 집중하고 시간을 할해할 수 있도록 돕습니다. Sagemaker Studio Image Build CLI를 통해 기본 워크플로우에 대해 걱정할 필요 없이 이미지를 빌드하도록 CLI에 지시하면 출력이 Amazon Elastic Container Registry(Amazon ECR) 이미지 위치에 대한 링크가 됩니다. 아래 다이어그램은 위에서 설명한 Studio Image Build CLI의 아키텍처를 설명합니다.

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

CLI는 기본적으로 Amazon S3, AWS Codebuild, Amazon ECR을 사용합니다.

{% hint style="info" %}
Amazon S3 : CLI에서는 AWS Codebuild에서 사용하는 buildspec.yml 파일, Docker 파일 및 컨테이너 코드를 S3에 zip 파일 형태로 패키지화 합니다. 기본적으로 이 파일은 불필요한 저장 비용을 피하기 위해 빌드 후 자동으로 정리됩니다.
{% endhint %}

{% hint style="info" %}
AWS Codebuild : Codebuild는 일시적인 빌드 환경을 사용하여 Docker 이미지를 빌드할 수 있는 완전 관리형 빌드 환경입니다. Codebuild는 빌드를 실행하는 데 사용하는 빌드 명령 및 설정이 포함된 buildspec.yml 파일에 종속되며 cli는 이 파일을 자동으로 생성하며, S3에 저장된 패키지 파일을 사용하여 컨테이너 빌드를 자동으로 시작합니다.
{% endhint %}

{% hint style="info" %}
Amazon ECR : 생성된 Docker 이미지에 태그가 지정되어 Amazon ECR로 푸시됩니다. Amazon Sagemaker에서는 교육 및 추론 이미지가 Amazon ECR에 저장된 이미지를 사용하며, CLI는 Amazon Sagemaker 학습 및 호스팅 호출에 포함할 수 있는 이미지의 URL에 대한 링크를 반환합니다.
{% endhint %}



기본적인 정책 및 권한이 설정되어 있지 않다면, Sagemaker Studio에서 패키지를 사용할 수 없습니다. 설정되지 않았다면, [..](../../ "mention") 를 참조하여 권한을 설정해주세요.



이제 Amazon Sagemaker Studio Image Build CLI를 이용하여 학습 및 배포에 활용할 사용자 지정 컨테이너 이미지를 빌드해보겠습니다.



1. 아래 압축 파일을 다운로드합니다.

{% file src="../../.gitbook/assets/mlops.zip" %}

2. 업로드 버튼을 클릭하여 압축파일을 업로드합니다.

![](<../../.gitbook/assets/image (25).png>)

2. File - New - Terminal 을 클릭하고, 아래 코드를 입력합니다.

```
sudo yum install unzip
unzip mlops.zip
```

3. Sagemaker Studio Image Build CLI를 사용하기 위해 노트북 환경에 패키지를 설치해야 합니다. 압축 해제 후 docker-upload.ipynb 파일을 실행합니다.
4. 노트북을 실행하면 첫 코드 블록에 편의 패키지 설치를 위한 인스톨 커맨드가 있습니다. 첫번째, 두번째 코드블록을 실행하면 Sagemaker Studio Image Build CLI 패키지와 테스트를 위한 서브패키지를 설치하고, 설치가 완료되면 Sagemaker Studio에서 Sagemaker Studio Image Build CLI를 사용할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

5. 세번째 코드 블록을 실행하면 업로드한 train 폴더 내 컨테이너 빌드에 필요한 파일들을 이용하여 도커 이미지를 빌드하고 Amazon ECR에 푸시하는데 사용합니다. 사용자 지정 컨테이너를 빌드하고 배포하는데 몇 분 정도의 시간이 소요됩니다.

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

6. 업로드가 완료되면 Amazon ECR 페이지에서 사용자 지정 컨테이너를 확인할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

7. 컨테이너 업로드가 확인되면 다음 과정인 Build Training pipeline 과정으로 넘어갑니다.






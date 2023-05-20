# Build pipeline using sagemaker pipelines

Sagemaker Studio에는 MLOps를 위한 파이프라인을 구성하기 위해 Sagemaker Pipeline을 이용할 수 있습니다. Sagemaker pipeline을 사용하면 사용하기 쉬운 Python SDK로 ML 워크플로를 생성한 후, Amazon Sagemaker Studio를 사용하여 워크플로를 시각화하고 관리할 수 있습니다. 또한 기본 제공 템플릿을 사용해 빠르게 시작하여 모델을 구축, 테스트, 등록 및 배포할 수 있어, ML 환경에서 CI/CD, 즉 MLOps를 빠르게 시작할 수 있습니다.



&#x20;세이지메이커 파이프라인의 MLOps Best Practice는 아래 그림과 같습니다.

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

위 그림을 자세히 설명하면, 세이지메이커 MLOps 파이프라인은 크게 모델 빌드 파이프라인과 모델 배포 파이프라인으로 구성되어 있습니다.

모델 빌드 파이프라인의 경우 Sagemaker Pipeline을 이용하여 모델을 학습 및 평가하고, 모델 패키지는 Sagemaker Model Registry에 등록되어 관리자의 승인 후 배포 파이프라인을 진행합니다.

모델 배포 파이프라인의 경우 Codepipeline을 이용하여 Sagemaker endpoint를 배포하기 위한 컨테이너 빌드 후 Staging / Prod Endpoint를 배포합니다.



이번 과정에서는 Sagemaker Pipeline을 구성하여 모델 빌드 파이프라인을 구성하는 실습을 진행하겠습니다.

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

1. S3에 'datasets-yolo'의 버킷을 생성합니다.

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

2. 아래에 첨부한 데이터셋을 다운로드 합니다. 첨부된 데이터셋은 ms-coco 데이터셋에서 Sagemaker Training 테스트를 위해 Training Dataset 4장, Validation Dataset 4장으로 구성된 학습 데이터셋입니다.

{% file src="../../.gitbook/assets/coco8-pose (1).zip" %}

3. 압축 폴더의 구조는 아래 사진과 같습니다. 폴더 내 Train / Valid 폴더가 구분되어 있고, 각 폴더에는 이미지와 Keypoint Annotation 파일을 images 폴더와 labels 폴더로 구분지어 저장되어 있습니다.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

4. 위에서 다운로드한 파일을 압축해제 하여 설정한 S3 버킷에 업로드합니다.

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

5. Sagemaker Studio 내 pose-yolov8-sagemaker.ipynb 파일을 실행합니다.

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

6. pose-yolov8-sagemaker.ipynb 스크립트는 sagemaker pipeline을 구성하고 빌드하기 위한 스크립트이며, 일부 위치를 수정하여 사용해야합니다. 먼저 4번째 블록의 local\_path, data\_uri, train\_data\_uri, valid\_data\_uri, test\_data\_uri가 업로드한 데이터 경로와 일치하는지 확인하세요.
7. 다음으로 6번째 블록의 training\_image\_uri에 [undefined.md](undefined.md "mention")에서 빌드했던 컨테이너의 uri를 입력합니다.

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

8. 각 파이프라인에서 빌드에 사용할 인스턴스 타입을 설정합니다.

```
* training_instance_type : 학습 인스턴스
* FrameworkProcessor 내 instance_type : 평가 인스턴스
* model.register 내 inference_instance : 추론 인스턴스
```

{% hint style="info" %}
model.register 내 inference\_instances에 설정한 인스턴스는 추후 배포 파이프라인에서 사용할 인스턴스가 포함되어 있어야 합니다.
{% endhint %}

9. 모든 설정이 완료되면 run - Run All Cells를 클릭하여 모든 셀을 실행합니다.
10. pipeline.start() 함수가 실행되면 위에서 정의한 학습 파이프라인이 생성되고, 설정에 따라 자동으로 학습을 진행합니다. 실습에서 정의한 학습 파이프라인의 진행 상황 및 상세 정보를 확인할 때는 Home - Pipelines - group name 클릭 - 현재 진행중인 파이프라인을 더블클릭하면 아래와 같은 상세 화면을 확인할 수 있습니다.

![](<../../.gitbook/assets/image (1).png>)

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

11. 파이프라인의 각 단계가 끝나게 되면 초록색으로 완료된 상황을 확인할 수 있으며 자동으로 다음 단계가 진행되는 것을 확인할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

12. 학습 파이프라인이 완료되면 아래와 같이 파이프라인을 구성하는 모든 과정이 완료되고, 우상단의 Status가 초록색으로 변경되는 것을 확인할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

13. Home - Models - Model registry 탭을 클릭하면, 현재 배포했던 모델을 확인할 수 있으며, Status가 Manual Approval 상태로 표시되는 것을 확인할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

여기까지 Sagemaker pipelines를 이용하여 모델 빌드 파이프라인을 실습했습니다. 이어서 Sagemaker Endpoint 생성을 위한 배포 파이프라인 실습을 진행하겠습니다.

# Sagemaker Endpoint Test

이번 실습에서는 MLOps를 통해 배포한 Sagemaker Endpoint를 이용하여 추론 결과를 노트북에서 테스트해보겠습니다.



1. Sagemaker Studio에서 test-pose-model.ipynb 스크립트를 실행합니다.

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

2. 세번째 블록의 ENDPOINT\_NAME을 배포한 모델의 이름으로 설정 후 해당 위치까지 코드를 실행하면 Endpoint의 Status를 확인할 수 있습니다.

{% hint style="info" %}
기존 스크립트를 활용하여 진행했을 경우 아래와 같이 모델 이름이 설정됩니다.

Staging model을 사용하는 경우 pose-deploy-staging

Production model을 사용하는 경우 pose-deploy-prod
{% endhint %}

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

3. Sagemaker Endpoint를 사용한 모델 추론에는 sagemaker runtime의 invoke\_endpoint()함수를 사용하여 추론을 진행합니다. 마지막 셀의 추론 결과를 확인하면, 추론 시간, Keypoint list, Bounding box list, 결과 영상을 확인할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

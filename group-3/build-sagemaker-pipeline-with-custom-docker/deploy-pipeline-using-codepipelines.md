# Deploy pipeline using codepipelines

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

다음은 위 그림에 표시된 배포 파이프라인을 생성하는 실습을 진행하겠습니다.

배포 파이프라인은 앞에서 설명한 것 처럼 Codepipeline으로 구성되어 Sagemaker Endpoint를 배포하기 위한 모델 빌드 및 배포 과정을 수행합니다.

배포 파이프라인의 생성은 Sagemaker Studio의 프로젝트를 통해 Template을 사용하면 쉽게 생성할 수 있으며, Sagemaker project는 원하는 형태의 인프라 템플릿과 설정을 미리 저장해두고 재사용할 수 있게 도와주는 기능입니다.



1. Sagemaker Studio 좌측 사이드바의 home -> Deployments -> Projects로 이동합니다.

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

2. Create project를 클릭하고, Sagemaker Templates 탭에서 Model Deployments 템플릿을 클릭하고 Select project Template 버튼을 클릭합니다.

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

3. name에 pose-deploy를 입력하고 SourceModelPackageGroupName에는 PoseYOLOv8을 입력해서 프로젝트를 생성합니다.

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

4. 생성 후 사이드바의 Deployments -> Projects 메뉴에 가면 프로젝트가 생성되어 있으며, 성공적으로 생성이 완료 되면 Create in progress 상태에서 \~ 상태로 변경된 것을 확인할 수 있다.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

5. 이후 [build-pipeline-using-sagemaker-pipelines.md](build-pipeline-using-sagemaker-pipelines.md "mention")에서 생성했던 모델을 승인하고 배포 파이프라인을 작동 시키게 되는데, home -> Models -> Model Registry로 이동한다.

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

6. 배포했던 모델이 승인 대기 상태인 것을 확인할 수 있는데, 최신 버전을 클릭한 후 우 상단의 Actions -> Update status를 클릭하면 모델 승인 창을 확인할 수 있다.

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

7. 위 Status를 Pending -> Approved 상태로 변경 후 update status를 클릭하면 모델 배포 승인을 트리거하여 배포 파이프라인을 자동으로 실행하는 것을 확인할 수 있다.

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

8. AWS 콘솔 -> Codepipeline 검색 후 이동하면 배포용 파이프라인이 진행중인 것을 확인할 수 있다.

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

9. 현재 생성된 배포 파이프라인은 기본 설정상 staging, prod 추론 인스턴스 타입이 ml.m5.large로 배포되는데, 인스턴스 타입을 변경해야 파이프라인이 정상적으로 동작한다. 좌측 메뉴에서 CodeCommit-> 리포지토리를 클릭하면 배포 파이프라인의 리포지토리를 확인할 수 있다.

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

10. 해당 리포지토리로 이동하면, 모델 빌드를 위한 config 파일과 스크립트가 존재합니다. 이 중 staging-config.json 파일과 prod-config.json 파일을 클릭하여 편집을 누릅니다.
11. 아래 그림과 같은 json 파일을 확인할 수 있는데 "EndpointInstanceType"을 앞서 빌드 파이프라인에서 inference instance로 설정했던 타입으로 변경합니다.

{% hint style="info" %}
변경하지 않고 스크립트를 실행했다면, 'ml.p3.2xlarge', 'ml.g4dn.xlarge' 중 하나로 변경해야 합니다.&#x20;
{% endhint %}

10. InstanceType을 설정 및 변경 사항 커밋 정보를 입력한 후 변경 사항 커밋 버튼을 클릭합니다.

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

11. staging-config.json , prod-config.json 두 파일 모두 인스턴스를 변경하고 커밋을 완료하면 코드의 변경을 감지하고, 배포 파이프라인이 재작동 합니다. 파이프라인을 클릭하여 변경 사항 릴리스 버튼을 한번 클릭해주어 가장 최근 변경 사항을 감지하고 파이프라인을 동작시킬 수 있도록 합니다.

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

12. 배포 파이프라인은 앞서 설명한 것 처럼 Staing / production 으로 엔드포인트를 나눠서 배포하도록 자동 구성되어 있습니다. staging endpoint는 자동으로 배포되고, staging endpoint로 테스트가 완료되었다면 실제 production에서 적용할 수 있도록 production endpoint를 수동 승인할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

13. &#x20;(Optional) 검토 버튼을 클릭하여 수동 승인을 완료하면 Prod Endpoint를 배포하게 됩니다.



지금까지 Codepipeline을 이용하여 Sagemaker Endpoint를 배포하는 배포 파이프라인을 실습하였습니다. 다음으로 배포한 모델의 Endpoint를 이용하여 테스트를 진행하겠습니다.&#x20;

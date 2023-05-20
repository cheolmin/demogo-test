# Test Sagemaker Endpoint

&#x20;이전 과정에서 모델 배포가 완료되어 성공적으로 엔드포인트가 호스팅되었다면, 호스팅 된 엔드포인트를 사용하여 추론을 실행할 수 있습니다. 이 단계에서는 먼저 이미지를 읽고 바이트로 변환한 다음 바이트를 엔드포인트에 입력으로 전달하여 추론을 실행합니다.

생성된 결과는 호스팅에 사용된 YOLOv8 모델에 기반하여 Bounding Box와 Pose를 반환하고, 그에 따라 출력을 확인할 수 있습니다.



1. 2\_TestEndpoint.ipynb 노트북을 클릭합니다.
2. 노트북의 환경은 배포 노트북과 동일하게 PyTorch 1.12 Python 3.8 gpu optimized로 설정되어 있는지 확인이 필요합니다.

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

3. 테스트 노트북은 sagemaker runtime의 invoke\_endpoint를 사용하여 추론을 진행합니다. 마지막 셀을 제외한 모든 셀을 실행하세요.
4. 추론에 성공한 경우 추론 결과로 Inference Time, Bounding Box, Confidence, Skeleton을 확인할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

5. 이전 과정에서 INSTANCE\_TYPE을 안내와 동일하게 설정했다면, CPU 인스턴스로 추론을 진행한 결과입니다. 만약 GPU 인스턴스를 사용하여 추론을 진행하고자 한다면, 이전 과정의 인스턴스 타입을 변경하고, 컨테이너를 다시 배포 후 추론을 진행하면 GPU 인스턴스로 추론이 가능합니다.

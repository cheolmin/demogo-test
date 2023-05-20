# Hosting YOLOv8 pytorch model





1. Amazon S3에 yolov8-'name'으로 버킷을 생성합니다.

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

2. 이전 Lab에서 활용했던 Sagemaker Studio에 접속합니다.
3. 노트북 환경을 설정합니다. 배포하는 컨테이너 이미지가 PyTorch 1.12버전 이므로 PyTorch 1.12 Python 3.8 GPU Optimized image를 선택합니다.

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

3. 아래 소스코드를 다운로드 합니다.

{% file src="../../.gitbook/assets/sm-notebook.zip" %}

3. 업로드 버튼을 클릭하여 압축파일을 업로드합니다.

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

4. File - New - Terminal 을 클릭하고, 아래 코드를 입력합니다.

```
sudo yum install unzip
unzip sm-notebook.zip
```

5. 압축이 해제되면 1\_DeployEndpoint.ipynb 파일을 더블클릭하여 노트북을 엽니다.
6. 1\_DeployEndpoint.ipynb 노트북은 먼저 Ultralytics에서 제공하는 Pretrained 모델을 다운로드하게 됩니다. Pose model을 테스트해야 하므로, 2번째 블록에 있는 model\_name을 'yolov8n-pose.pt'로 변경합니다.

<pre><code>!pip3 install ultralytics
from ultralytics import YOLO

## Choose a model:
model_name = <a data-footnote-ref href="#user-content-fn-1">'yolov8n-pose.pt'</a> #replace pose model

YOLO(model_name)
os.system(f'mv {model_name} code/.')

bashCommand = "tar -cpzf  model.tar.gz code/"
process = subprocess.Popen(bashCommand.split(), stdout=subprocess.PIPE)
output, error = process.communicate()
</code></pre>

7. 노트북 내 Deploy the model on Sagemaker Endpoint 블록에 'INSTANCE\_TYPE'을 선택합니다.

```
INSTANCE_TYPE = 'ml.m5.4xlarge'
ENDPOINT_NAME = 'yolov8-pytorch-' + str(datetime.utcnow().strftime('%Y-%m-%d-%H-%M-%S-%f'))

# Store the endpoint name in the history to be accessed by 2_TestEndpoint.ipynb notebook
%store ENDPOINT_NAME
print(f'Endpoint Name: {ENDPOINT_NAME}')

predictor = model.deploy(initial_instance_count=1, 
                         instance_type=INSTANCE_TYPE,
                         deserializer=JSONDeserializer(),
                         endpoint_name=ENDPOINT_NAME)
```

8. Sagemaker Studio 메뉴 탭의 Run - Run All Cells를 클릭하면 노트북 내 모든 셀이 실행 되며 YOLOv8 모델이 Sagemaker Endpoint에 배포됩니다.

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

9. Home 탭 - Deployments - Endpoints 에 들어가면 배포된 YOLOv8 모델의 Endpoint를 확인할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

[^1]: 

#### AWS ECR 

> AWS 에서 기본적인 Docker 이미지 레포짓토리를 제공하기 때문에 이를 이용한 배포또한 가능함

##### 1. AWS IAM 설정 : ECR에 도커 이미지를 등록 하고 이 이미지를 정해진 EC2로 배포 하기 위해 권한을 가지고 있어야 함

##### 2. AWS ECR : ECR REPO 생성 -> Life Cycle 정책 결정

##### 3. 로컬 PC 에 AWS Profile 등록 : aws configure --profile '이름'
    (기본적으로 aws cli 설치)
    
    


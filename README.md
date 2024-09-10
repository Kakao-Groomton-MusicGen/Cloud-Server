
# M.U.T.E - Cloud Infrastructure

**M.U.T.E**는 사용자가 입력한 키워드를 기반으로 AI가 맞춤형 노래를 생성하여 교육적 이해와 학습을 돕는 플랫폼입니다. 이를 지원하기 위한 클라우드 인프라 구성은 다음과 같습니다.

---

## 프로젝트 개요

M.U.T.E는 **Music**(음악), **Understanding**(이해), **Teaching**(가르침), **Education**(교육)을 의미하며, 음악을 통해 더 나은 학습 경험을 제공하고 교육적 이해를 향상시키는 것을 목표로 합니다. 이를 지원하기 위해 안정적이고 확장 가능한 클라우드 인프라가 필요합니다.

---

## 목차
- [프로젝트 개요](#프로젝트-개요)
- [클라우드 기술 스택](#클라우드-기술-스택)
- [클라우드 인프라](#클라우드-인프라-구성)
- [VPC와 서브넷 구성](#vpc와-서브넷-구성)
- [Routing Tables](#routing-tables)
- [Gateways](#gateways)
- [Security Groups](#security-groups)
- [EC2 Instances](#ec2-instances)
- [ECR Repositories](#ecr-repositories)
- [S3 Bucket](#s3-bucket)
- [RDS Configuration](#rds-configuration)
- [IAM Users](#iam-users)

---

## 클라우드 기술 스택
| **CSP** | **Infra**  | **Image** | **CI/CD** |
| -------------- | -------- | ---------------- | ---------------- |
| ![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white) | ![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white) | ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white) | ![Jenkins](https://img.shields.io/badge/jenkins-%232C5263.svg?style=for-the-badge&logo=jenkins&logoColor=white) |

- **AWS**: 가상 서버(EC2 Instance), 이미지 저장(ECR), 스토리지(S3), 데이터베이스(RDS), 네트워크 관리(VPC) 등의 클라우드 서비스
- **Terraform**:Infrastructure as Code (IaC) 도구로, AWS 상의 인프라를 자동으로 구축하고 관리
- **Docker**: 컨테이너화된 애플리케이션을 배포하여 독립적이고 이식성 높은 컨테이너로 관리
- **Jenkins**: CI/CD 파이프라인을 관리하는 도구로, 지속적인 통합 및 배포를 자동화
---

## 클라우드 인프라 구성
![Cloud Architecture](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F8b6f698e-8a67-4ad1-94b0-53ee956264c9%2F91ff88c8-b547-46f6-befa-586985f3319c%2Fimage.png?table=block&id=1073ac76-91d3-4b56-aea4-36620996f3ae&spaceId=8b6f698e-8a67-4ad1-94b0-53ee956264c9&width=1800&userId=51dd97ed-4b7f-4f0a-bc29-4e7109794d96&cache=v2)

- **확장성(Scalability)**: 사용량에 따라 리소스를 유연하게 스케일 인/아웃할 수 있도록 설계했습니다. 이를 통해 사용자가 증가할 때에도 성능 저하 없이 안정적으로 서비스가 제공되도록 대비했습니다.  
- **가용성(Availability)**: 멀티 AZ(가용 영역) 구성을 통해 하나의 영역에 장애가 발생하더라도 다른 영역에서 서비스가 계속 운영될 수 있도록 가용성을 보장했습니다.
- **보안(Security)**: VPC(가상 사설 클라우드)와 보안 그룹(Security Groups)을 활용하여 서비스 간의 트래픽을 최소화하고, 필요에 따른 트래픽만 허용하는 방식으로 네트워크 보안을 강화했습니다.
- **자동화(Automation)**: 테라폼(Terraform)을 사용해 인프라를 코드로 관리(IaC)하며, 이를 통해 인프라 배포 및 변경 과정을 자동화했습니다. 이를 통해 인프라 관리의 일관성을 유지하고 오류를 줄였습니다.
- **지속적인 배포(CI/CD)**: 젠킨스(Jenkins)를 활용한 CI/CD 파이프라인을 구축하여, 코드 변경이 발생할 때마다 자동으로 빌드, 테스트, 배포가 진행되도록 설정했습니다. 이를 통해 개발 및 배포 속도를 높이고, 안정성을 강화했습니다.
- **컨테이너화(Containerization)**: 도커(Docker)를 사용하여 애플리케이션을 컨테이너화하고, 다양한 환경에서 일관된 성능을 제공할 수 있도록 했습니다. Private ECR(Elastic Container Registry)을 사용해 컨테이너 이미지를 안전하게 관리하고 배포했습니다.

---
## VPC와 서브넷 구성

| VPC Name               | CIDR            |
| ---------------------- | --------------- |
| hackathon_vpc_infra     | 192.168.0.0/16  |

| Subnet Name                    | Availability Zone  |
| ------------------------------ | ------------------ |
| hackathon_jenkins_public        | ap-northeast-2a    |
| hackathon_jenkins_private       | ap-northeast-2a    |
| hackathon_network_public1       | ap-northeast-2b    |
| hackathon_network_private1      | ap-northeast-2b    |
| hackathon_network_public2       | ap-northeast-2c    |
| hackathon_network_private2      | ap-northeast-2c    |

---

## Routing Tables

| Routing Table Name           |
| ---------------------------- |
| hackathon_rtb_public         |
| hackathon_rtb_private1       |
| hackathon_rtb_private2       |
| hackathon_rtb_private3       |

---

## Gateways

| IGW Name              | NAT Gateway Name      |
| --------------------- | --------------------- |
| hackathon_igw         | hackathon_nat         |

---

## Security Groups

| Security Group Name         | Inbound Ports            |
| --------------------------- | ------------------------ |
| hackathon_sg_jenkins         | 22, 80, 443, 8080        |
| hackathon_sg_backend         | 22, 80, 443, 3000        |
| hackathon_sg_frontend        | 22, 80, 443, 3000        |
| hackathon_sg_ai              | 22, 5000, 3000           |
| hackathon_sg_rds             | 5432                     |

---

## EC2 Instances

| Instance Name              | Type      | Security Group        | Subnet                      | OS                      |
| -------------------------- | --------- | --------------------- | --------------------------- | ------------------------ |
| hackathon_jenkins_master    | t3.medium | hackathon_sg_jenkins   | hackathon_jenkins_public     | ubuntu-noble-24.04        |
| hackathon_jenkins_agent     | t3.medium | hackathon_sg_jenkins   | hackathon_jenkins_private    | ubuntu-noble-24.04        |
| hackathon_frontend_nginx    | t2.micro  | hackathon_sg_frontend  | hackathon_network_public1    | Amazon Linux 2023         |
| hackathon_frontend_nextjs   | t3.small  | hackathon_sg_frontend  | hackathon_network_public1    | Amazon Linux 2023         |
| hackathon_backend_nginx     | t2.micro  | hackathon_sg_backend   | hackathon_network_public2    | Amazon Linux 2023         |
| hackathon_backend_nestjs    | t2.micro  | hackathon_sg_backend   | hackathon_network_private1   | Amazon Linux 2023         |
| hackathon_backend_ai        | t3.medium | hackathon_sg_ai        | hackathon_network_private1   | Amazon Linux 2023         |
| hackathon_backend_ai_music  | t3.large  | hackathon_sg_ai        | hackathon_network_private1   | ubuntu-noble-24.04        |

---

## ECR Repositories

| ECR Name                  |
| -------------------------- |
| mute-ai                    |
| mute-backend               |
| mute-frontend              |

---

## S3 Bucket

| S3 Bucket Name               | Settings                                     |
| ---------------------------- | -------------------------------------------- |
| hackathon-mute-backend        | Public access disabled, Public read access enabled via policy |

---

## RDS Configuration

| RDS Name                | Engine            | Type          | Storage      | AZ                  |
| ----------------------- | ----------------- | ------------- | ------------ | ------------------- |
| hackathon-mute-rds       | PostgreSQL 16.3-R2| db.t3.micro   | gp3 SSD      | ap-northeast-2b     |

---

## IAM Users

| User Name             | Permissions           |
| --------------------- | --------------------- |
| hackathon_s3_ai        | AmazonS3FullAccess    |


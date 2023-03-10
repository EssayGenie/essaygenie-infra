# EssayGenie TerraForm

## How it is implemented
**1. VPC(Virtual Private Cloud)**
- IP Range : `10.60.0.0/16`
    
**2. Subnet Configuration**
- Region : ap-northeast-2

  | Env | AZ | Subnet | IPv4 CIDR |
  | --- | --- | --- | --- |
  | Common | ap-northeast-2a | imok-corp-pub-sub-2a | 10.60.4.0/24 |
  | Common | ap-northeast-2c | imok-corp-pub-sub-2c | 10.60.5.0/24 |
  | Dev | ap-northeast-2a | imok-corp-dev-pri-sub-2a | 10.60.0.0/24 |
  | Dev | ap-northeast-2c | imok-corp-dev-pri-sub-2c | 10.60.1.0/24 |
  | Prod | ap-northeast-2a | imok-corp-prd-pri-sub-2a | 10.60.2.0/24 |
  | Prod | ap-northeast-2c | imok-corp-prd-pri-sub-2c | 10.60.3.0/24 |
  
**3. SG(Security Group)**
- On-Premise IP Inbound Source Info
  
  | CIDR | Desc |
  | --- | --- |
  | IP - HOME | ImOK Tower wireless ip |
  | IP - WORK | ImOK Tower lan |
  | IP - SCHOOL | ImOK Corp VPN |
    
**4. EC2**
- OS : Amazon Linux 2

  | 구분 | 용도 | Name | CPU | Memory | Disk | Type |
  | --- | --- | --- | --- | --- | --- | --- |
  | Common | Bastion Host | imok-corp-bastion | 2 | 2GiB | 10GiB | t3.small |
  | Dev | Management | imok-corp-dev-management-2a | 2 | 16GiB | 100GiB | r5.large |
  | Dev | Service | imok-corp-dev-service-2a | 2 | 4GiB | 50GiB | t3.medium |
  | Prd | Management | imok-corp-prd-management-2a | 4 | 16GiB | 100GiB | t3.xlarge |
  | Prd | Service | imok-corp-prd-service-2a | 4 | 16GiB | 50GiB | t3.xlarge |
  | Prd | Service | imok-corp-prd-service-2c | 4 | 16GiB | 50GiB | t3.xlarge |

**5. ALB(Application Load Balancer)**
  | ALB | LB Listener Port | Target Group port | Target Instance |
  | :---: | :---: | :---: | :---: |
  | imok-corp-dev-alb-management | 80 -> 443 Redirect | 8080 | imok-corp-dev-management-2a |
  | imok-corp-dev-alb-service | 80 -> 443 Redirect | 8085, 8086 | imok-corp-dev-service-2a |
  | imok-corp-prd-alb-management | 80 -> 443 Redirect | 8080 | imok-corp-prd-management-2a |
  | imok-corp-prd-alb-service | 80 -> 443 Redirect | 8085, 8086 | imok-corp-prd-service-2a</br> imok-corp-prd-service-2c |
    

## Architecture
<img src="https://user-images.githubusercontent.com/44595181/201484828-2bf02dec-2ef2-4f07-b62f-c4c3164906ee.png"/>


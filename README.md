# estudo-cloudformation

## Começando
```bash
# inicia o localstack
docker-compose up -d

# mostra a versão e serviços disponíveis
curl -v http://localhost:4566/health | jq
```
Para acessar a interface gráfica do localstack acesse [https://app.localstack.cloud.](https://app.localstack.cloud.)
## Comandos
```bash
# cria uma stack
aws cloudformation create-stack \
  --endpoint-url http://localhost:4566 \
  --stack-name VPC \
  --template-body file://main.yaml 

# lista todas as stacks
aws cloudformation describe-stacks \
  --endpoint-url http://localhost:4566

# deleta a stack VPC
aws cloudformation delete-stack \
  --endpoint-url http://localhost:4566 \
  --stack-name my-stack

# atualiza a stack VPC
aws cloudformation update-stack \
  --endpoint-url http://localhost:4566 \
  --stack-name VPC \
  --template-body file://main.yaml

# lista as vpcs
 aws ec2 describe-vpcs \
  --endpoint-url http://localhost:4566

# lista todas as subnets
aws ec2 describe-subnets \
  --endpoint-url http://localhost:4566

# lista todas as subnets filtrando pelo vpc-id
aws ec2 describe-subnets \
  --endpoint-url http://localhost:4566 \
  --filters "Name=vpc-id,Values=vpc-ee3bef58"

# lista as subnets que começam 10.0. no CidrBlock
# e exibe somentee 2 registros
aws ec2 describe-subnets \
  --endpoint-url http://localhost:4566 \ 
  --query "Subnets[?starts_with(CidrBlock,`10.0.`) == `true`] | [0:2]" | jq

# lista as route tables
aws ec2 describe-route-tables \
  --endpoint-url http://localhost:4566

# lista as route tables associadas
aws ec2 describe-route-tables \
  --endpoint-url http://localhost:4566 \
  --query 'RouteTables[].Associations[]'

# lista as instâncias ec2
aws ec2 describe-instances \
  --endpoint-url=http://localhost:4566 

# lista as instâncias ec2 exibindo status e ips
aws ec2 describe-instances \
  --endpoint-url http://localhost:4566 \
  --query "Reservations[*].Instances[*].\
  {State:State.Name,PublicIp:PublicIpAddress,PrivateIp:PrivateIpAddress}"

# lista os tópicos sns
aws sns list-topics \
  --endpoint-url http://localhost:4566

# lista os buckets 
aws s3 ls \
  --endpoint-url http://localhost:4566
```
## Exemplo de templates
VPC
```yaml
  Resources:
    VPCAula:
      Type: AWS::EC2::VPC
      Properties:
        CidrBlock: 10.0.0.0/16
        EnableDnsSupport: 'true'
        EnableDnsHostnames: 'true'
```

Subnet
```yaml
  Resources:
    VPCAula:
      Type: AWS::EC2::VPC
      Properties:
        CidrBlock: 10.0.0.0/16
        EnableDnsSupport: 'true'
        EnableDnsHostnames: 'true'

    PublicSubnetVPCA:
      Type: AWS::EC2::Subnet
      Properties:
        CidrBlock: 10.0.1.0/24
        VpcId: !Ref VPCAula
```
## Links
- [AWS CloudFormation](https://aws.amazon.com/pt/cloudformation/)
- [AWS CloudFormation Documentation](https://docs.aws.amazon.com/cloudformation/index.html)
- [Template anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)
- [Resource types](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)
- [Erro de CORS na GUI do dashboard do Localstack](https://pt.stackoverflow.com/questions/523577/erro-de-cors-no-commandeer-com-localstack)
- [AWS CLI Command Reference for CloudFormation](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/index.html)
- [AWS CLI Command Reference for EC2](https://docs.aws.amazon.com/cli/latest/reference/ec2/index.html#cli-aws-ec2)
- [AWS CLI Command Reference for SNS](https://docs.aws.amazon.com/cli/latest/reference/sns/index.html)
- [Filtering AWS CLI output](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-filter.html)
- [Filtering AWS CLI output advanced queries](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-filter.html#cli-usage-filter-client-side-advanced)
- [Filtering AWS CLI output advanced queries from issue in github](https://github.com/aws/aws-cli/issues/2206#issuecomment-250535857)
- [Treinamento AWS Cloud Practitioner Essentials](https://explore.skillbuilder.aws/learn/course/external/view/elearning/134/aws-cloud-practitioner-essentials?dt=tile&tile=fdt)
- [Pseudo parameters reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html)

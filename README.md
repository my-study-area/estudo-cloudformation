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
```

## Links
- [AWS CloudFormation](https://aws.amazon.com/pt/cloudformation/)
- [AWS CloudFormation Documentation](https://docs.aws.amazon.com/cloudformation/index.html)
- [Template anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)
- [Resource types](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)
- [Erro de CORS na GUI do dashboard do Localstack](https://pt.stackoverflow.com/questions/523577/erro-de-cors-no-commandeer-com-localstack)
- [AWS CLI Command Reference for CloudFormation](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/index.html)

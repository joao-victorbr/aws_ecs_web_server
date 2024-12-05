# aws_ecs_web_server

### Objetivo
Esse projeto consiste em uma demonstração de deploy de um servidor web através de um container no serviço Amazon ECS. O deploy é automatizado através de IaC com Terraform e Docker.
O usuário deve ser capaz de acessar um endereço em seu navegador a partir da URL informada no terminal do Docker.
O endereço apresenta uma imagem padrão nginx como demonstração da execução correta do projeto.

### Estrutura do projeto
Em primeiro lugar, o arquivo Dockerfile contém o script de criação e execução do container na máquina local. O script realiza o mapeamento de volume da pasta IaC para o container.
Essa pasta contém todos os scripts .tf necessários para o provisionamento de recursos no provedor AWS, dentr os quais:
- outputs.tf: definição da variável de saída, que será o endereço utilizado pelo usuário para acessar a imagem nginx no navegador;
- providers.tf: definição do provedor de cloud computing;
- terraform.tfvars: arquivo de valores de variáveis;
- variables.tf: arquivo de definição de variáveis;
- vpc.tf: configuração da VPC e sub-nets do projeto;
- security_group: configuração do grupo de segurança do container e do application load balancer, com suas respectivas regras de entrada e saída;
- main.tf: configuração dos recursos do projeto

### Preparação:
- Para que a bucket do projeto tenha um nome único, insira o número da conta do usuário da AWS nos arquivos main.tf e terraform.tfvars:
- Salve as alterações.
- Crie uma bucket no Amazon S3 com o nome websv-<numero_da_conta> e carregue o arquivo de variáveis de ambiente vars.env. O cluster utilizará esse arquivo para obter o ARN da imagem nginx.
- Para automatizar o aws cli, insira a chave primária e secundária da conta de usuário no Dockerfile:

### Execução:
1) Acesse a pasta onde está o Dockerfile através do terminal
2) Execute o comando abaixo:
docker build -t websv-terraform-image:test . &&\
docker run -dit --name websv -v ./IaC:/iac websv-terraform-image:test /bin/bash
3) Acesse a pasta iac no terminal do container Docker e execute os comandos:
terraform init && terraform apply --auto-approve
Nesse momento, os recursos serão provisionados. O processo terá uma duração entre 3 a 5 minutos.
4) Quando o provisionamento terminar, a URL do servidor web com imagem nginx aparecerá no output. Copie e cole esse endereço no navegador de sua escolha. O resultado deve ser esse:

5) Para destruir os recursos, execute o comando:
terraform destroy --auto-approve
# AWS Lambda + S3: Tarefas Automatizadas na Nuvem

Anotações e insights do laboratório prático de automação com **AWS Lambda** e **Amazon S3** — desafio da [DIO](https://www.dio.me/).

---

## O que é cada serviço

**AWS Lambda**
Serviço serverless que executa código em resposta a eventos, sem necessidade de gerenciar servidores. Você paga apenas pelo tempo de execução.

**Amazon S3**
Serviço de armazenamento de objetos (arquivos) na nuvem, organizado em buckets. Altamente durável e escalável.

---

## Como a automação funciona

```
Upload no S3  →  Evento disparado  →  Lambda executada  →  Logs no CloudWatch
```

Sempre que um arquivo é enviado ao bucket, o S3 notifica automaticamente a Lambda, que processa o evento sem nenhuma intervenção manual.

---

## O que foi praticado

- Criação de bucket no S3
- Configuração de IAM Role com as permissões corretas
- Criação de Lambda Function com runtime Python
- Configuração do trigger S3 → Lambda
- Teste via upload de arquivo e verificação dos logs no CloudWatch

---

## Anotações importantes

**IAM Role é o ponto mais crítico**
Sem a permissão correta, a Lambda simplesmente não acessa o S3. O erro mais comum é esquecer de associar a policy `AmazonS3ReadOnlyAccess` à role.

**Cuidado com loop infinito**
Se a Lambda escrever no mesmo bucket que a dispara, ela ficará se chamando indefinidamente. A solução é usar buckets diferentes para entrada e saída, ou filtrar por prefixo.

**Cold Start**
Na primeira execução após um período inativo, a Lambda pode demorar um pouco mais. Em cenários que exigem baixa latência, isso precisa ser considerado.

**Timeout padrão é 3 segundos**
Para processar arquivos maiores ou lógicas mais complexas, é necessário aumentar o timeout nas configurações da função.

---

## Insights

- O modelo serverless muda completamente a forma de pensar infraestrutura: em vez de "que servidor vou usar?", a pergunta passa a ser "que evento vai disparar meu código?"
- A integração S3 → Lambda é surpreendentemente simples de configurar, mas exige atenção às permissões IAM
- O CloudWatch é essencial para entender o que está acontecendo dentro da função — sem ele, debugar é muito difícil
- Lambda + S3 é uma combinação poderosa para processar arquivos automaticamente: CSVs, imagens, JSONs, relatórios, etc.

---

## Referências

- [Documentação AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
- [Documentação Amazon S3](https://docs.aws.amazon.com/s3/index.html)
- [Usando S3 como trigger da Lambda](https://docs.aws.amazon.com/lambda/latest/dg/with-s3.html)

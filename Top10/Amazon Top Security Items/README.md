### AWS 1: Accurate account information
**Descrição:** Manter os contatos da Amazon atualizados e teste se todos os destinatários estão funcionando, sempre que possível utilizar caixas de e-mail para que uma pessoa apenas fique responsável por essas informações.

### AWS 2: Use multi-factor authentication (MFA)
**Descrição:** Utilize sempre o autenticar de dois fatores, principalmente em contas com perfis elevados, isso ajuda que ocorra acessos inadequados.

### AWS 3: No hard-coding secrets
**Descrição:** Utilizar o _AWS Secrets Manager_ é uma das melhores formas de utilizar suas chaves de uma maneira segura, podendo não utilizar esse recurso é imprescindível que suas chaves não estejam codificadas dentro da sua aplicação.

### AWS 4: Limit security groups
**Descrição:** Manter seus grupos de seguranças configurados e atualizados, mantendo as comunicações desejadas entre cada tipo específico de aparelho ou situação.

### AWS 5: Intentional data policies
**Descrição:** Mantar a classificação dos dados configurados e atualizados, com isso você terá um ambiente seguro e ágil para que os desenvolvedores tenham acesso aos dados que necessitam sem uma burocracia.

### AWS 6: Centralize CloudTrail logs
**Descrição:** Para ter rastreio das atividades acontecendo no seu sistema, o ideal é guardar o máximo de registro possível. A AWS disponibiliza a ferramenta _CloudTrail_ para te auxiliar nesse processo.

### AWS 7: Validate IAM roles
**Descrição:** Para dar conformidade de suas políticas de acesso a AWS disponibiliza ferramentas para isso, utilizando o  AWS _IAM Access Analyzer_ você terá visibilidade das permissões podendo validar a conformidade com as suas políticas de segurança.

### AWS 8: Take actions on findings (This isn’t just GuardDuty anymore!)
**Descrição:** Utilizando as ferramentas de respostas da AWS, é possível manter um controle de todas as ações ocorrendo no seu sistema, já que ele consegue enviar mensagens e notificações sobre as atividades que acontecem dentro do seu sistema.

### AWS 9: Rotate keys
**Descrição:** No caso do seus usuários de AWS utilizarem credenciais de longo prazo, é recomendado que atualize as chaves a cada 90 dias. A AWS disponibiliza ferramentas para que você não tenha que utilizar credenciais  de longo prazo.

### AWS 10: Be involved in the dev cycle
**Descrição:** Como último ponto envolva a segurança em todos os pontos, todos são responsáveis pela segurança. A equipe de segurança tem como função ajuda e facilitar a segurança em muitos pontos, mas depende da comunidade auxiliar sempre que possível e sempre pensar na forma mais segura.

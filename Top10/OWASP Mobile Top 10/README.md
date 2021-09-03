### M1: Improper Platform Usage
**Descrição:** Utilização errada das ferramentas de segurança como Intenções do Android, permissões de plataforma, uso indevido de _TouchID_, _Keychain_ entre outros podem gerar vetores de ataque.

**Vetor de ataque:** Nesse caso os vetores de ataque são os mesmo do Top10 tradicional, como uma _API_ que pode ser usada como vetor de ataque.

**Prevenção:** Configurar os servidores do aplicativo de forma segura.

**Referência:** [M1: Improper Platform Usage | OWASP](https://owasp.org/www-project-mobile-top-10/2016-risks/m1-improper-platform-usage)

**Exemplo:** Muitas vezes em um sistema é necessário salvar algumas informações de autenticação, é possível realizar esse processo na memória do aparelho, porém 
essas informações podem ser acessadas por um _backup_ não criptografado.

**Correção:** Para que essas informações sensíveis sejam guardadas de forma segura, o IOS disponibilizao IOS _Keychain_ para que seus sistema não tenha essa vulnerabilidade.

### M2: Insecure Data Storage
**Descrição:** Dados sensíveis armazenados dentro da aplicação devem ser tratados, para que caso haja um _malware_, aparelho roubado/perdido ou alguma forma que o atacante tenha acesso ao aparelho esses dados não sejam vazados.

**Vetor de ataque:** Essas informações podem ser acessadas caso o atacante tenha acesso ao aparelho, seja por _malware_, ferramentas de desbloqueio de aparelho ou modificando um aplicativo legítimo.

**Prevenção:** Modelar as ameaças e entender como a aplicação lida com essas informações. Alguns pontos cruciais para essa prevenção: 

- Cache de _URL_;

- Cache de imprensa do teclado;

- Caching de _buffer_ de copiar/colar;

- Plano de fundo do aplicativo;

- Dados intermediários;

- _Logging_;

- Armazenamento de dados _HTML5_;

- _Cookies_ do navegador;

- Dados analíticos enviados a terceiros.

**Referência:** [M2: Insecure Data Storage | OWASP](https://owasp.org/www-project-mobile-top-10/2016-risks/m2-insecure-data-storage)

**Exemplo:** Em algumas ocasiões alguns dados são salvos no aparelho, entretando nem sempre são tratados da forma certa como por exemplo, salvar os dados do usuário sem uma criptografia, sendo possível recuperar esses dados com um _backup_ do aparelho.

**Correção:** Nesse caso para que os dados não estejam vulneráveis é necessários realizar um processo de criptografia desses dados.


### M3: Insecure Communication
**Descrição:** Os aplicativos comunicam com seus servidores para que haja a troca de informações, para isso é necessário que exista uma segurança nessa comunicação. Caso contrários todos os dados sensíveis serão expostos para os atacantes.

**Vetor de ataque:** A monitoração da rede em que o aparelho está conectada ou por um _malware_ instalado no aparelho.

**Prevenção:** Aplicar _SSL/TLS_, utilizar _tokens_ de sessão, evitar sessões de _SSL_ mista, utilizar criptografias fortes e certificados assinados por uma CA, entre outras.

 **Referência:** [M3: Insecure Communication | OWASP](https://owasp.org/www-project-mobile-top-10/2016-risks/m3-insecure-communication)
 
 **Exemplo:** Em um sistema onde o aparelho e um _enpoint_ estabeleção um canal de comunicação, o serviço para para inspecionar o certficado é fraco, resultando em um ataque onde o fraudador consegue passar qualquer certificado para o servidor.
 
 **Correção:** Para que não haja uma falsificação de um certificado, sempre utilizar certificados por um provedor de CA.


### M4: Insecure Authentication
**Descrição:** Utilizar fatores de autenticação seguros, para que o atacante não consiga realizar ataques de força bruta por exemplo.

**Vetor de ataque:** Muitas vezes aplicativos utilizam senhas simples, como PIN, isso facilita o processo para “adivinhar” a senha do usuário. O abuso da autenticação _offline_ é um dos vetores utilizados pelo atacante, já que as conexões mobile não são menos confiáveis. 

**Prevenção:** Utilizar 2FA, senhas fortes, certificar que a maioria das solicitações de autenticação sejam feitas no servidor, criptografia dos dados de acesso, entre outras.

**Referência:** [M4: Insecure Authentication | OWASP](https://owasp.org/www-project-mobile-top-10/2016-risks/m4-insecure-authentication)

**Exemplo:** Em alguns casos alguns serviços não necessitam de uma autenticação do usuário por ser uma requisição onde naturalmente o usuário já estaria logado, mas caso o atacante tenha acesso a essa requisição é possível realizar as chamadas sem um perfil associado.

**Correção:** Para corrigir essa falha é necessário realizar um autenticação em todas as requisições que necessitam, mesmo que elas sejam internas.


### M5: Insufficiente Cryptography
**Descrição:** Utilização de criptografia fraca, permitindo o atacante a informações sensíveis.

**Vetor de ataque:** Uma criptografia simples ou fraca permite com que o atacante tenha informações sensíveis, gerando vetores de ataque como um _API_ ou dados de um usuário.

**Prevenção:** Seguir as diretrizes do _NIST_, com uma criptografia que resista por pelo menos 10 anos. Armazenar o mínimo de informações sensíveis no aplicativo.

**Referência:** [M5: Insufficient Cryptography | OWASP](https://owasp.org/www-project-mobile-top-10/2016-risks/m5-insufficient-cryptography)

**Exemplo:** Utilizar uma criptografia do tipo _DES_.

**Correção:** Utilizar uma criptografia do tipo _SAFER_.


### M6: Insecure Authorization
**Descrição:** Um autorização insegura permite com que um atacante utilize de um usuário comum para realizar operações que requerem acesso elevado.

**Vetor de ataque:** Uma falha na autorização permite que o atacante realize processos de usuário elevado, obtendo informações sensíveis ou até comprometimento do sistema.

**Prevenção:** Validar as permissões dos usuários, evitar confiar em informações provenientes do aparelho e sempre que possível validar as ações do usuário.

**Referência:** [M6: Insecure Authorization | OWASP](https://owasp.org/www-project-mobile-top-10/2016-risks/m6-insecure-authorization)

**Exemplo:** Em um _enpoint_ é necessário de um ID e um _token_, é possível realizar a requisição para o _endpoint_, porém ele não valida se o _token_ pertence aquele usuário permintindo assim buscar por informações de outros usuários.

**Correção**: Utilizar não apenas _tokens_ para realizar requisições, mas validar as informações que estão sendo enviadas e utilziar ID + _token_ como chave, validar se as informações passadas são do mesmo usuário.


### M7: Poor Code Quality
**Descrição:** A má qualidade de um código em si não é uma vulnerabilidade, porém ela resulta em outras vulnerabilidades, como um estouro de _buffer_ por exemplo.

**Vetor de ataque:** Um estouro de _buffer_ é um vetor de ataque, como também uma falta de sanitização de dados enviados pelo usuário.

**Prevenção:** Manter códigos limpos, fácies de ler, validar informações que estão sendo enviadas pelos usuários, garantir que os dados enviados não vão estourar o _buffer_ de destino, manter o padrão de código da empresa, entre outras boas práticas.

**Referência:** [M7: Poor Code Quality | OWASP](https://owasp.org/www-project-mobile-top-10/2016-risks/m7-client-code-quality)

**Exemplo:** 
```C
include <stdio.h>

 int main(int argc, char **argv)
 {
    char buf[8]; // buffer for eight characters
    gets(buf); // read from stdio (sensitive function!)
    printf("%s\n", buf); // print out data stored in buf
    return 0; // 0 as return value
 }
```
**Correção:** Evitar utilizar funções como _get_ que permitem o estouro de memória, validar as informações enviadas pelos usuários.


### M8: Code Tampering
**Descrição:** Alteração do binário dos aplicativos, resultando em um aplicativo que o atacante tenha total controle do que está sendo executado, sendo distribuído provavelmente por _phishing_.

**Vetor de ataque:** Redirecionamento de _API_, alteração de binários principais e alteração dos binários dos recursos.

**Prevenção:** Em tempo de execução o aplicativo deve ser capaz de identificar as alterações e reagir ao ataque, verificar se o aparelho do usuário está com permissões de _root/jailbreak_.

**Referência:** [M8: Code Tampering | OWASP](https://owasp.org/www-project-mobile-top-10/2016-risks/m8-code-tampering)

**Exemplo:** A partir de um _phishing_ o atacante envia um aplicativo corrompido, a vitima utiliza o aplicativo tendo assim suas informações expostas para o atacante.

**Correção:** O aplicativo conseguir realizar algumas validações se o código está corrompido de alguma forma, validar também se o aparalho não está com permissões elevadas, para que o atacante não consiga realizar alterações no código para replicar para vítima ou alterar para benefício próprio.


### M9: Reverse Engineering
**Descrição:** Utilizar de ferramentas para decompilar os aplicativos e conseguir compreender as regras de negócio e também a busca por informações sensíveis.

**Vetor de ataque:** Utilizando ferramentas para esse processo atacantes vão conseguir compreender as regras de negócio do aplicativo, conseguindo extrair informações sensíveis, ou informações que auxiliem em outros ataques.

**Prevenção:** Para que a engenharia reversa seja menos suscetível é necessário utilizar de ferramentas para a ofuscação de código, uma boa prática é ofuscar e utilizar as ferramentas do mercado para desofuscar e ver a eficácia do seu ofuscador.

**Referência:** [M9: Reverse Engineering | OWASP](https://owasp.org/www-project-mobile-top-10/2016-risks/m9-reverse-engineering)

**Exemplo:** Em um ataque o invasor descobre que é possível realizar engenharia reversa por falta de criptografia e ofuscação, com isso ele consegue acesso a credenciais de um banco de dados, permitindo que ele busque por informações sobre outroso usuários.

**Correção:** Utilizar das ferramentas de ofuscação de código, assim o atacante não vai conseguir realizar a engenharia reversa no sistema, e sempre utilizar criptografia forte.


### M10: Extraneous Functionality
**Descrição:** Funcionalidades a parte dentro do código de produção, como ambientes de testes, demonstração ou preparação.

**Vetor de ataque:** Utilizar das funcionalidades extras dentro dos aplicativos de produção para buscar informações sensíveis que não foram feita sanitização dos dados.

**Prevenção:** Existem ferramentas que realizam essa validação, porém realizar essa validação manual é uma boa prática. Para a validação manual utilizar funcionários com mais experiências, tendo em vista que ele deve validar as seguintes informações: informações de configuração, código de teste não incluído, verificar endpoints de _API_ se estão documentados e públicos e examinar todas as saídas de _logs_ para avaliar que não existe nenhuma informação sensíveis vazada.

**Referência:** [M10: Extraneous Functionality | OWASP](https://owasp.org/www-project-mobile-top-10/2016-risks/m10-extraneous-functionality)

**Exemplo:** Em um teste os desenvolvedores utilizaram um painel adiministrativo para realizar algumas requisições, porém no momento da publicação foi retirado o painel mas os _endpoints_ não foram retirados, sendo possível realizar as requisições tendo acesso a informações sensíveis.

**Correção:** Utilizar ferramentas e funcionários mais experientes para garantir que nenhuma informação a mais está sendo enviada para produção.

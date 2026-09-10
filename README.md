<img width="100%" height="150" alt="Blabinha_API_@_2 0" src="https://github.com/user-attachments/assets/bb817cc5-b0ea-4ceb-9b15-0892dc8ad597" />

---
<p align=center><strong>Web API para o chatbot Blabinha.</strong></p>

## Sobre
Um servidor HTTP REST, que expõe o chat-bot Blabinha através da rede, como um serviço de API, permitindo a criação de conversas utilizando diferentes modelos de LLMs e estratégias de prompt. Ligado ao projeto _Blabinha 2.0: um agente conversacional baseado em inteligência artificial generativa, especialista na Amazônia Azul_, do laboratório _Center for Artificial Intelligence_ (C4AI) da Universidade de São Paulo.

## _Features_

- Sistema de autenticação e autorização para usuários; permitindo
- Criar chats: sessões de conversa com a Blabinha;
- Criar diálogos (_dialogs_): uma seção de conversa com um chat (pergunta e resposta);
- Recuperar dados sobre chats ou diálogos anteriores;
- Editar e apagar dados referentes a chats e diálogos;

## Como usar a API

1. Crie um novo usuário em `/users/register` e faça _log in_ em `/auth/token`, você receberá um token de acesso (`access_token`) e um token de recuperação (`refresh_token`), use o token de acesso para acessar os endpoints. Eventualmente o token de acesso irá expirar, requisite um novo token de acesso através de `/auth/refresh` utilizando o token de recuperação. Quando o token de recuperação expirar você terá que fazer _log in_ novamente. **Para os próximos passos é preciso passar o token de acesso no _header_ `Authorization`**.
2. Crie um chat em `/chats`, com o modelo de LLM desejado e a estratégia de prompt preferida, e - opcionalmente - a seção que se deseja começar, para iniciar do 'zero' o valor padrão é `100`. A resposta será em _json_ no _schema_ `Chat`, contendo o atributo `id`. **Armazene esse id** para referenciar o chat nas próximas interações;
3. Interaja com o chat por meio de diálogos, com requisições do tipo `POST /dialogs`, enviando o **id do chat** e o **input desejado**. A resposta será em _json_, no esquema `Dialog`, que contém a resposta gerada por IA, informações sobre aquela seção, e o `Chat` pertencente atualizado.

Para mais informações refira-se à [documentação](./docs/intro.md).

Ou utilize uma aplicação de software que testem requisições HTTP, como [Postman](https://www.postman.com/downloads/) ou [Insomnia](https://insomnia.rest/download).

## Como rodar a API

1. Clone este repositório: `git clone git@github.com:caio-bernardo/Blabinha2-API.git`;
1.2. **Recomenda-se fortemenete** o uso do package manager `uv`, a instalação é simples: `pip install uv`, se deseja mais informações [clique aqui](https://docs.astral.sh/uv/);
2. Crie um arquivo chamado `.env` seguindo o exemplo em `.env.example`. Preencha as variaveis de ambiente.
3. Crie a base de dados com o comando `uv run task migrate`. Verifique se um arquivo `db.sqlite3` foi criado na raíz do projeto.
3. Rode a API com o seguinte comando `uv run task run`

> Na primeira vez que você executar o _uv_ ele irá instalar todos os pacotes necessários e criar uma pasta _.venv_ na raíz do seu projeto. Essa pasta pode ser usada como ambiente virtual, e ativada com `source ./.venv/bin/activate` (e sua versão do Windows). Se decidir iniciar o ambiente virtual, pode executar todos os comandos demonstrados sem a necessidade do `uv run`.

### Windows

Antes de tudo verifique se o comando utilitário `curl` está instalado:
```bash
curl --version
```
Se sim, tudo está certo :smile, você pode seguir os exemplos anteriores. Caso contrário, siga este [link](https://curl.se/windows/), e instale o `curl` na sua máquina.

## Desenvolvendo a API

1. Siga as instruções do tópico anterior
1. Para rodar a aplicação em **modo de desenvolvimento**, execute `uv run task dev`

> Lembre-se de criar uma nova _branch_, quando for fazer alterações ao código.

### Fazendo migrações

Sempre que adicionar modelos à base de dados, ou quiser modificar seus atributos, é preciso criar uma migração. Faremos isso utilzando o comando `alembic`. Veja abaixo:

```bash
alembic upgrade head
```
Esse comando irá rodar todas as migrações, cria uma base de dados com as tabelas que precisamos para a API. Ao modificar, remover e adicionar modelos é preciso criar uma nova migração: 

```bash
alembic revision --autogenerate -m "Adicionar campo X ao modelo Y"
```
Isso criará um script na pasta _migrations_. O alembic cobre a maioria dos casos para migrações, porém pode ser que você precise escrever seu próprio script se estiver fazendo algo muito complicado. Após criar sua migração atualize para a _head_ com o comando anterior. Se quiser voltar uma migração (ou mais) use esse comando:
```bash
alembic upgrade -1
```
Onde $-1$ é a migração relativa à atual. Esse parâmetro também pode ser o id da migração.

> As vezes deletar a base de dados e rodar a migração de novo pode resolver conflitos de dados.
> 

## Licença

Este projeto está sobre a _MIT License_, para saber o que você pode ou não fazer com esse projeto veja a [licença](/LICENSE).

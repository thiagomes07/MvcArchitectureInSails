# Esboço da Arquitetura MVC do Edellcation 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;O Edellcation é uma Aplicação Web desenvolvida seguindo a arquitetura MVC (Model-View-Controller), com seu esboço elaborado no draw.io. Esta aplicação foi criada para proporcionar aos funcionários das linhas de montagem um acesso fácil e eficiente a materiais técnicos e manuais de montagem de produtos da empresa, como computadores, servidores e notebooks. A plataforma permite que os funcionários estudem, revisem e acompanhem os processos de montagem de forma individualizada, ao mesmo tempo em que os mantêm atualizados sobre quaisquer alterações nos procedimentos ou inclusão de novos manuais.

<img src="assets/MvcArchitectureDiagram.svg" style="max-width:100%; height:auto;" alt="Diagrama da arquitetura MVC do Edellcation">

### Modelos (Models):
1. **CURSO:**
   - **ID:** Identificador único do curso (chave primária).
   - **Nome:** Nome do curso.
   - **Tipo de material:** Tipo de material presente no curso (documento, imagem, vídeo, modelo 3D).
   - **Arquivo (ou link para o arquivo):** Caminho para o arquivo ou link para o material.
   - **Produto Alvo:** Produto ao qual o curso se refere.
   - **Data de Publicação:** Data em que o curso foi publicado.

2. **TODO:**
   - **ID:** Identificador único da tarefa (chave primária).
   - **ID do usuário:** Identificador do usuário associado à tarefa (chave estrangeira referenciando a entidade Usuário).
   - **ID do curso:** Identificador do curso associado à tarefa (chave estrangeira referenciando a entidade CURSO).
   - **Status:** Status da tarefa (pendente, concluído).
   - **Data de atribuição:** Data em que a tarefa foi atribuída ao usuário.

3. **FUNCIONARIO:**
   - **ID:** Identificador único do funcionário (chave primária).
   - **Nome:** Nome do funcionário.
   - **Email:** Endereço de e-mail do funcionário.
   - **Senha (criptografada):** Senha criptografada do funcionário para fins de autenticação.
   - **Tipo de usuário:** Indicação se o funcionário é um membro ou administrador.

4. **LINHAMONTAGEM_ALUNO:**
   - **IdAluno:** Identificador do aluno (chave estrangeira referenciando a entidade FUNCIONARIO).
   - **IdLinhaMontagem:** Identificador da linha de montagem (chave estrangeira referenciando a entidade LINHAMONTAGEM).

5. **LINHAMONTAGEM:**
   - **Id:** Identificador único da linha de montagem (chave primária).
   - **Nome:** Nome da linha de montagem.
   - **Descrição:** Descrição da linha de montagem.

#### Relacionamentos:
- Um curso pode ter muitas tarefas associadas a ele (relação 1 para N entre CURSO e TODO).
- Um funcionário pode ter muitas tarefas (relação 1 para N entre FUNCIONARIO e TODO).
- Um funcionário pode estar associado a uma ou várias linhas de montagem (relação 1 para N entre FUNCIONARIO e LINHAMONTAGEM_ALUNO).
- Uma linha de montagem pode ter muitos funcionários associados a ela (relação 1 para N entre LINHAMONTAGEM e LINHAMONTAGEM_ALUNO).

### Controladores (Controllers):
- Liste os controladores do seu projeto e suas responsabilidades.
- Descreva as ações (methods) de cada controlador e seus parâmetros de entrada e saída.
- Explique como os controladores interagem com os modelos e views.

### Views (Views):
- Liste as views do seu projeto e sua função.

### Infraestrutura:

- Descreva os componentes de infraestrutura do seu projeto, como bancos de dados, APIs externas e outras dependências.
- Explique como a infraestrutura se integra à arquitetura MVC.


### Justifique as escolhas feitas e como elas impactam o projeto.
#### Implicações da Arquitetura:
Descreva as implicações da arquitetura em termos de escalabilidade, manutenção, testabilidade e outros aspectos importantes.
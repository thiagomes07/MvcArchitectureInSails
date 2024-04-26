<h1 align="center">Arquitetura MVC do Edellcation</h1>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;O Edellcation é uma Aplicação Web desenvolvida seguindo a arquitetura MVC (Model-View-Controller), com seu esboço elaborado no draw.io. Esta aplicação foi criada para proporcionar aos funcionários das linhas de montagem um acesso fácil e eficiente a materiais técnicos e manuais de montagem de produtos da empresa, como computadores, servidores e notebooks. A plataforma permite que os funcionários estudem, revisem e acompanhem os processos de montagem de forma individualizada, ao mesmo tempo em que os mantêm atualizados sobre quaisquer alterações nos procedimentos ou inclusão de novos manuais.

<img src="assets/MvcArchitectureDiagram.png" style="max-width:100%; height:auto;" alt="Diagrama da arquitetura MVC do Edellcation">

> **Observação:** Não foi representada visualmente as relações entre as tabelas do Model e as tabelas do Controller para evitar poluir o diagrama e torná-lo confuso.

[Diagrama em PDF](assets/MvcArchitectureDiagram.pdf)</br>
[Diagrama em PNG](assets/MvcArchitectureDiagram.png)

## MVC
### Modelos (Models):🗄️
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
   - **E-mail:** Endereço de e-mail do funcionário.
   - **Senha (criptografada):** Senha criptografada do funcionário para fins de autenticação.
   - **Tipo de usuário:** Indicação se o funcionário é um membro ou administrador.

4. **LINHAMONTAGEM_ALUNO:**
   - **IdAluno:** Identificador do aluno (chave estrangeira referenciando a entidade FUNCIONARIO).
   - **IdLinhaMontagem:** Identificador da linha de montagem (chave estrangeira referenciando a entidade LINHAMONTAGEM).

5. **LINHAMONTAGEM:**
   - **Id:** Identificador único da linha de montagem (chave primária).
   - **Nome:** Nome da linha de montagem.
   - **Descrição:** Descrição da linha de montagem.
   - **Produto:** Produto montado na linha de montagem.

#### Relacionamentos:
- Um curso pode ter muitas tarefas associadas a ele (relação 1 para N entre CURSO e TODO).
- Um funcionário pode ter muitas tarefas (relação 1 para N entre FUNCIONARIO e TODO).
- Um funcionário pode estar associado a uma ou várias linhas de montagem (relação 1 para N entre FUNCIONARIO e LINHAMONTAGEM_ALUNO).
- Uma linha de montagem pode ter muitos funcionários associados a ela (relação 1 para N entre LINHAMONTAGEM e LINHAMONTAGEM_ALUNO).


### Controladores (Controllers):🖲️
#### AuthController (Controlador de Autenticação)
- **register(nome: string, email: string, senha: string, tipoUsuario: string)**: Registra um novo usuário.
  - Entrada: `nome`, `email`, `senha`, `Cargo`
  - Saída: Mensagem de sucesso ou erro, detalhes do usuário registrado.

  O AuthController gerencia as operações de autenticação e registro de usuários, interagindo com o modelo de Funcionário para registrar novos usuários e validar credenciais de acesso.

#### UserController (Controlador de Usuário)
- **viewProfile(idUsuario: int)**: Exibe o perfil do usuário.
  - Entrada: `idUsuario`
  - Saída: Detalhes do perfil do usuário.
- **editProfile(idUsuario: int, novosDados: object)**: Permite ao usuário editar seu perfil.
  - Entrada: `idUsuario`, `novosDados` (objeto contendo os campos editáveis)
  - Saída: Perfil atualizado, mensagem de sucesso ou erro.

  O UserController lida com as operações relacionadas aos perfis de usuário, permitindo a visualização e edição de perfis. Ele se comunica com o modelo de Funcionário para acessar e atualizar informações de perfil.

#### CourseController (Controlador de Curso)
- **listCourses(filtro: object)**: Lista todos os cursos disponíveis com opção de filtragem.
  - Entrada: `filtro` (objeto contendo critérios de filtragem)
  - Saída: Lista de cursos.
- **filterCourses(criterios: object)**: Aplica filtros específicos à lista de cursos.
  - Entrada: `criterios` (objeto contendo critérios de filtragem)
  - Saída: Lista de cursos filtrados.
- **viewCourse(idCurso: int)**: Exibe detalhes completos de um curso.
  - Entrada: `idCurso`
  - Saída: Detalhes do curso.
- **completeCourse(idCurso: int, idUsuario: int)**: Marca um curso como concluído.
  - Entrada: `idCurso`, `idUsuario`
  - Saída: Status do curso atualizado, mensagem de sucesso ou erro.

  O CoursoController é responsável pela gestão dos cursos disponíveis, permitindo a listagem, filtragem e visualização detalhada de cada curso. Ele interage com o modelo de Curso para acessar e exibir informações relevantes sobre os cursos.

#### TaskController (Controlador de Tarefas)
- **listTasks(idUsuario: int)**: Lista todas as tarefas associadas ao usuário.
  - Entrada: `idUsuario`
  - Saída: Lista de tarefas.
- **viewTask(idTarefa: int)**: Exibe detalhes de uma tarefa específica.
  - Entrada: `idTarefa`
  - Saída: Detalhes da tarefa.
- **updateTaskStatus(idTarefa: int, novoStatus: string)**: Atualiza o status de uma tarefa.
  - Entrada: `idTarefa`, `novoStatus`
  - Saída: Status da tarefa atualizado, mensagem de sucesso ou erro.

  O TaskController gerencia as tarefas atribuídas aos usuários, possibilitando a listagem, visualização e atualização do status de cada tarefa. Ele se conecta ao modelo de TODO para acessar e modificar as tarefas associadas aos usuários.

#### LineController (Controlador de Linha de Montagem)
- **listAssemblyLines()**: (Admin) Lista todas as linhas de montagem.
  - Entrada: Nenhuma
  - Saída: Lista de linhas de montagem.
- **viewAssemblyLine(idLinhaMontagem: int)**: (Admin) Exibe detalhes de uma linha de montagem.
  - Entrada: `idLinhaMontagem`
  - Saída: Detalhes da linha de montagem.
- **editAssemblyLine(idLinhaMontagem: int, novosDados: object)**: (Admin) Edita uma linha de montagem.
  - Entrada: `idLinhaMontagem`, `novosDados`
  - Saída: Linha de montagem atualizada, mensagem de sucesso ou erro.
- **deleteAssemblyLine(idLinhaMontagem: int)**: (Admin) Remove uma linha de montagem.
  - Entrada: `idLinhaMontagem`
  - Saída: Confirmação de remoção, mensagem de sucesso ou erro.

  O LineController é responsável pela administração das linhas de montagem, permitindo a listagem, visualização, edição e exclusão das linhas existentes. Ele interage com o modelo de Linhamontagem para acessar e manipular informações sobre as linhas de montagem.

#### FuncController (Controlador de Funcionários)
- **listEmployees()**: (Admin) Lista todos os funcionários.
  - Entrada: Nenhuma
  - Saída: Lista de funcionários.
- **viewEmployee(idFuncionario: int)**: (Admin) Exibe detalhes de um funcionário.
  - Entrada: `idFuncionario`
  - Saída: Detalhes do funcionário.
- **editEmployee(idFuncionario: int, novosDados: object)**: (Admin) Edita informações de um funcionário.
  - Entrada: `idFuncionario`, `novosDados`
  - Saída: Funcionário atualizado, mensagem de sucesso ou erro.
- **deleteEmployee(idFuncionario: int)**: (Admin) Remove um funcionário.
  - Entrada: `idFuncionario`
  - Saída: Confirmação de remoção, mensagem de sucesso ou erro.

  Por fim, o FuncController trata das operações relacionadas aos funcionários da empresa, possibilitando a listagem, visualização, edição e exclusão dos mesmos. Ele se comunica com o modelo de Funcionário para acessar e modificar os dados dos funcionários.

### Views (Views):👁️
- Tela de login e cadastro:
   - Função: Permitir que os usuários façam login na aplicação ou se cadastrem como novos usuários.
   - Contém campos para entrada de e-mail, senha e opção de registro.
- Homepage:
   - Função: Exibir uma visão geral dos cursos disponíveis, to-dos do usuário, cursos recentemente acessados e cursos favoritos.
   - Pode conter carrosséis ou listas de cursos e to-dos.
- Lista de cursos:
   - Função: Exibir uma lista de todos os cursos disponíveis na aplicação, possivelmente com opções de filtragem e ordenação.
   - Os cursos podem ser apresentados em forma de lista ou grades.
- Curso:
   - Função: Mostrar detalhes de um curso específico, incluindo materiais de estudo, descrição do curso, produto alvo, etc.
   - Pode conter links para download de materiais, visualização de vídeos, etc.
- Perfil:
   - Função: Exibir informações do perfil do usuário, como nome, e-mail, histórico de cursos concluídos, etc.
   - Pode incluir opções para edição de informações do perfil.
- Edição (somente para admin):
   - Função: Permitir que os administradores editem linhas de montagem, cursos, usuários e outras informações administrativas.
   - Pode conter formulários para edição de dados e opções de exclusão.


## Infraestrutura:

- &nbsp;&nbsp;&nbsp;&nbsp;Banco de Dados PostgreSQL: O banco de dados PostgreSQL será usado para armazenar informações sobre os cursos, usuários, tarefas pendentes, estatísticas e outras informações relacionadas à aplicação.
- &nbsp;&nbsp;&nbsp;&nbsp;API de Single Sign-On (SSO): Considerando a integração com sistemas internos de Single Sign-On (SSO), será necessário utilizar uma API de SSO que permita aos usuários autenticarem-se na aplicação utilizando as credenciais de suas contas corporativas.
- &nbsp;&nbsp;&nbsp;&nbsp;Serviço de Armazenamento de Arquivos: Para armazenar documentos, imagens, vídeos e modelos 3D associados aos cursos, pode ser necessário utilizar um serviço de armazenamento de arquivos, como Amazon S3 ou Google Cloud Storage.
- &nbsp;&nbsp;&nbsp;&nbsp;Microsserviços e Containers: De acordo com os requisitos não funcionais, o sistema deve ser distribuído em vários servidores e baseado em microsserviços. Portanto, será necessário configurar uma arquitetura de microsserviços usando containers, como Docker, e um orquestrador de containers, como Kubernetes, para gerenciar e escalar os serviços da aplicação.
- &nbsp;&nbsp;&nbsp;&nbsp;Sistema de Cache: Para melhorar o desempenho da aplicação, pode ser necessário implementar um sistema de cache para armazenar conteúdos mais acessados, como arquivos e vídeos, utilizando tecnologias como Redis ou Memcached.
- &nbsp;&nbsp;&nbsp;&nbsp;Serviço de Notificações: Para enviar notificações aos usuários sobre atualizações nos cursos ou outras informações importantes, pode ser necessário integrar um serviço de notificações por e-mail e push, como Amazon SES ou Firebase Cloud Messaging.
- &nbsp;&nbsp;&nbsp;&nbsp;Serviços de Monitoramento e Log: Para monitorar o desempenho e a disponibilidade da aplicação, bem como para rastrear eventos e registros, pode ser necessário integrar serviços de monitoramento e log, como Prometheus e Grafana, ou ELK Stack (Elasticsearch, Logstash, Kibana).
- &nbsp;&nbsp;&nbsp;&nbsp;Ambiente de Desenvolvimento e Implantação: Será necessário configurar um ambiente de desenvolvimento e implantação para o projeto, incluindo ferramentas de controle de versão, como Git, e ferramentas de integração contínua e implantação contínua (CI/CD), como Jenkins ou GitLab CI.

### Relação da infraestrutura com o MVC
- &nbsp;&nbsp;&nbsp;&nbsp;Model (Modelo):
O Modelo é responsável pela representação dos dados da aplicação. No caso deste projeto, o Modelo seria representado pelas entidades do banco de dados PostgreSQL, como Curso, Funcionário, LinhaMontagem, etc. Estas entidades refletem a estrutura dos dados da aplicação e são manipuladas pelos controladores para realizar operações de leitura, escrita, atualização e exclusão (CRUD).
- &nbsp;&nbsp;&nbsp;&nbsp;View (Visão):
A Visão é responsável pela apresentação dos dados aos usuários da aplicação. No contexto deste projeto, as Views seriam as interfaces de usuário (UI) implementadas em HTML, CSS e JavaScript. Elas são responsáveis por exibir os dados aos usuários de forma adequada, seguindo o layout responsivo e as diretrizes de design da empresa Dell.
- &nbsp;&nbsp;&nbsp;&nbsp;Controller (Controlador):
O Controlador é responsável por intermediar as interações entre o Modelo e a Visão. Ele recebe as requisições dos usuários, manipula os dados necessários no Modelo e determina qual Visão deve ser apresentada ao usuário como resposta. No contexto deste projeto, os Controladores seriam implementados em Node.js com o framework Sails.js, e seriam responsáveis por rotear as requisições HTTP para as ações adequadas, como autenticação de usuários, busca de cursos, criação de novos cursos, etc.


### Escolha de arquitetura
- Node.js com Sails.js:
Escolha justificada pela eficiência e escalabilidade do Node.js para lidar com operações de I/O intensivas, como as requisições HTTP em uma aplicação web. O framework Sails.js, por sua vez, proporciona uma estrutura MVC robusta e convenções de desenvolvimento que aceleram o processo de desenvolvimento, facilitando a criação de API RESTful e a integração com o banco de dados PostgreSQL.
   - &nbsp;&nbsp;&nbsp;&nbsp;Escalabilidade: Node.js é conhecido por sua capacidade de lidar com um grande número de conexões simultâneas devido ao seu modelo de I/O não bloqueante. Isso permite que a aplicação seja facilmente escalada horizontalmente para lidar com aumento de tráfego.
   - &nbsp;&nbsp;&nbsp;&nbsp;Manutenção: A estrutura MVC do Sails.js facilita a manutenção do código, pois organiza logicamente as diferentes partes da aplicação. Além disso, a comunidade ativa e os recursos de documentação do Node.js e do Sails.js facilitam a resolução de problemas e a implementação de novos recursos.
   - &nbsp;&nbsp;&nbsp;&nbsp;Testabilidade: O Node.js é altamente testável, com suporte para frameworks de teste como Mocha, Jest e Jasmine. A estrutura MVC também ajuda na separação de preocupações, o que facilita a escrita de testes unitários e de integração.
- PostgreSQL como Banco de Dados:
Escolha justificada pela robustez, confiabilidade e capacidade de escalabilidade do PostgreSQL. Além disso, o PostgreSQL oferece suporte a recursos avançados, como transações ACID, índices avançados e suporte a JSON, que são úteis para aplicativos de negócios complexos.
   - &nbsp;&nbsp;&nbsp;&nbsp;Escalabilidade: O PostgreSQL é altamente escalável, permitindo que a aplicação cresça à medida que a demanda aumenta. Estratégias como particionamento de tabelas e replicação podem ser implementadas para distribuir a carga de trabalho e melhorar o desempenho.
   - &nbsp;&nbsp;&nbsp;&nbsp;Manutenção: O PostgreSQL é amplamente utilizado e bem documentado, o que facilita a manutenção e o gerenciamento do banco de dados. Além disso, sua comunidade ativa e regularmente atualizada significa que problemas de segurança e bugs são rapidamente corrigidos.
   - &nbsp;&nbsp;&nbsp;&nbsp;Testabilidade: O PostgreSQL suporta testes de unidade e integração através de bibliotecas como pgTAP e pgTAP-like. Além disso, é possível configurar facilmente bancos de dados de teste para garantir que os testes não afetem o ambiente de produção.
- HTML5, CSS3, Bootstrap e JavaScript para Front-end:
Escolha justificada pela familiaridade e ampla adoção dessas tecnologias, o que facilita o desenvolvimento de uma interface de usuário intuitiva e responsiva. O uso do Bootstrap permite uma rápida prototipagem e desenvolvimento de UIs consistentes.
   - &nbsp;&nbsp;&nbsp;&nbsp;Escalabilidade: O front-end desenvolvido com essas tecnologias é facilmente escalável, especialmente quando combinado com uma estrutura back-end escalável como o Node.js. A otimização de recursos, como imagens e scripts, pode ajudar a melhorar o desempenho em casos de alta carga.
   - &nbsp;&nbsp;&nbsp;&nbsp;Manutenção: O desenvolvimento com HTML5, CSS3 e JavaScript permite uma manutenção simplificada, especialmente quando aplicadas boas práticas de organização de código e modularização. O uso do Bootstrap também facilita a manutenção ao fornecer componentes pré-construídos e estilos consistentes.
   - &nbsp;&nbsp;&nbsp;&nbsp;Testabilidade: As tecnologias front-end escolhidas são altamente testáveis, com suporte para frameworks de teste como Jest, Jasmine e Selenium para testes de unidade, integração e e2e.
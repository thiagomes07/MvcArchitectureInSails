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
#### AuthController:
- **login():** Método responsável por autenticar um usuário na aplicação.
   - Entrada: Nome de usuário (ou email) e senha.
   - Saída: Redirecionamento para a página inicial ou mensagem de erro.
- **register():** Método para registrar um novo usuário na aplicação.
   - Entrada: Dados do novo usuário (nome, email, senha, etc.).
   - Saída: Redirecionamento para a página de login ou mensagem de erro.

Interage com o modelo Funcionário para autenticar e registrar usuários. Retorna mensagens de erro ou redirecionamentos para as visões correspondentes.

#### HomeController:
- **index():** Método para exibir a página inicial da aplicação.
   - Entrada: Nenhum.
   - Saída: Renderização da página inicial com os cursos disponíveis, to-dos, etc.
- **show():** Método para exibir detalhes de um curso específico.
   - Entrada: ID do curso.
   - Saída: Renderização da página do curso com detalhes, materiais, etc.
- **search():** Método para realizar uma pesquisa por cursos.
   - Entrada: Termo de pesquisa.
   - Saída: Renderização da página de resultados da pesquisa.

Interage com o modelo Curso para obter informações sobre os cursos. Retorna os dados necessários para renderizar as visões correspondentes.

#### CursoController:
- **index():** Método para listar todos os cursos disponíveis.
   - Entrada: Nenhum.
   - Saída: Renderização da página com a lista de cursos.
- **show():** Método para exibir detalhes de um curso específico (mesmo método que HomeController.show()).
   - Entrada: ID do curso.
   - Saída: Renderização da página do curso com detalhes, materiais, etc.
- **create():** Método para criar um novo curso (somente para administradores).
   - Entrada: Dados do curso (nome, tipo de material, arquivo, produto alvo, etc.).
   - Saída: Redirecionamento para a página de edição ou mensagem de erro.
- **edit():** Método para editar um curso existente (somente para administradores).
   - Entrada: ID do curso e dados atualizados.
   - Saída: Redirecionamento para a página do curso ou mensagem de erro.
- **delete():** Método para excluir um curso existente (somente para administradores).
   - Entrada: ID do curso.
   - Saída: Redirecionamento para a página de lista de cursos ou mensagem de erro.

Interage com o modelo Curso para criar, editar, listar e excluir cursos. Retorna mensagens de erro ou redirecionamentos para as visões correspondentes.

#### PerfilController:
- **show():** Método para exibir o perfil do usuário.
   - Entrada: Nenhum (o perfil é obtido a partir do usuário logado).
   - Saída: Renderização da página de perfil com as informações do usuário.
- **edit():** Método para editar as informações do perfil do usuário.
   - Entrada: Dados atualizados do usuário.
   - Saída: Redirecionamento para a página de perfil ou mensagem de erro.

Interage com o modelo Funcionário para obter e atualizar as informações do perfil do usuário. Retorna mensagens de erro ou redirecionamentos para as visões correspondentes.

#### AdminController:
- **index():** Método para exibir o painel de administração com estatísticas e funcionalidades administrativas.
   - Entrada: Nenhum.
Saída: Renderização da página do painel de administração.
- **editLines():** Método para editar as linhas de montagem (somente para administradores).
Entrada: ID da linha de montagem e dados atualizados.
Saída: Redirecionamento para a página de edição ou mensagem de erro.
- **editCourses():** Método para editar os cursos (somente para administradores).
Entrada: ID do curso e dados atualizados.
Saída: Redirecionamento para a página de edição ou mensagem de erro.
- **editUsers():** Método para editar os usuários (somente para administradores).
Entrada: ID do usuário e dados atualizados.
Saída: Redirecionamento para a página de edição ou mensagem de erro.

Interage com os modelos LinhaMontagem, Curso e Funcionário para editar e gerenciar linhas de montagem, cursos e usuários. Retorna mensagens de erro ou redirecionamentos para as visões correspondentes.


### Views (Views):
- Tela de login e cadastro:
   - Função: Permitir que os usuários façam login na aplicação ou se cadastrem como novos usuários.
   - Contém campos para entrada de email, senha e opção de registro.
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
   - Função: Exibir informações do perfil do usuário, como nome, email, histórico de cursos concluídos, etc.
   - Pode incluir opções para edição de informações do perfil.
- Edição (somente para admin):
   - Função: Permitir que os administradores editem linhas de montagem, cursos, usuários e outras informações administrativas.
   - Pode conter formulários para edição de dados e opções de exclusão.


### Infraestrutura:

- Descreva os componentes de infraestrutura do seu projeto, como bancos de dados, APIs externas e outras dependências.
- Explique como a infraestrutura se integra à arquitetura MVC.


### Justifique as escolhas feitas e como elas impactam o projeto.
#### Implicações da Arquitetura:
Descreva as implicações da arquitetura em termos de escalabilidade, manutenção, testabilidade e outros aspectos importantes.








<!-- Estou desenvolvendo uma aplicação WEB em parceria com a dell. Analise o escopo do projeto:
"
PROJETO: Criação de uma aplicação web de treinamento para divulgar e a disponibilizar materiais técnicos (manuais) sobre o processo de
montagem de diferentes produtos da empresa. Através de uma plataforma de treinamento específica, os funcionários que trabalham em linhas de
produção podem aprender o processo eficiente de montagem de diversos produtos se mantendo atualizados sobre qualquer melhoria de processo.
A aplicação deve disponibilizar diferentes tipos de materiais, tais como documentos, imagens, gravações de video, e modelos 3D possibilitando que
os funcionários explorem todos os detalhes de montagem de cada tipo de produto. Através dessas funcionalidades, os funcionários podem aprender
rapidamente processos específicos de montagem além de se manter atualizados quando uma alteração ocorre ou novos manuais são incluídos. A
solução contempla tanto novos funcionários como aqueles que já trabalham na empresa por algum tempo e já tem conhecimento dos processos. Do
ponto de vista de novos funcionários, a solução possibilita o aprendizado concentrado e específico dos processos de montagens dos produtos em que
o funcionário irá trabalhar. Além disso, a solução permite que esses funcionários constantemente revisem e visualizem o processo de montagem
acelerando o seu aprendizado. Do ponto de vista de funcionários que já tem conhecimento e trabalham há mais tempo, a solução permite que esses
funcionários revisem manuais de produtos os quais eles já não trabalham há um tempo e possam ter esquecido algo. Além disso, processos podem
mudar ao longo do tempo e atualizações em manuais permite que funcionários já treinados tenham conhecimento das alterações sempre que elas
ocorrerem. Por fim, do ponto de vista da empresa, a solução contribui para a disseminação de informação e também para aumentar a eficiência das
linhas de produção. Funcionários mais treinados e com melhor conhecimento dos processos tendem a cometer menos erros reduzindo o tempo de
montagem dos produtos e aumentando a eficiência das linhas de montagem como um todo.
EMPRESA: DELL
luiz.brotto@dell.com
vinicius.facco1@dell.com

BREVE DESCRIÇÃO EMPRESA / MINI BIO:
A Dell desenvolve, produz, dá suporte e vende uma grande variedade de computadores pessoais, servidores, notebooks, dispositivos
de armazenamento, switches de rede, PDAs, software, periféricos e mais. Sua sede fica em Round Rock, Texas nos Estados Unidos. A
empresa abriu sua fábrica em solo brasileiro na cidade de Eldorado do Sul no Rio Grande do Sul em novembro de 1999. Conta também
com um centro de desenvolvimento de software sediado no polo Tecnopuc, da PUC-RS. No ano de 2006 foi anunciada a construção
de uma nova fábrica na cidade de Hortolândia, interior de São Paulo. A partir de Agosto de 2007, Eldorado do Sul passou a sediar
apenas a administração e toda a produção de (desktops, notebooks e servidores) foi transferida para Hortolândia.
PROFESSOR ORIENTADOR: Claudio André

OVERVIEW
PROBLEMA: Treinamento de funcionários que trabalham nas linhas de montagem da fábrica da empresa .
OBJETIVO: O objetivo desta iniciativa é disponibilizar uma plataforma de treinamento individual para os funcionários que trabalham nas linhas de
montagem da empresa. A empresa monta diversos tipos de produtos, os quais possuem manuais de montagem que descrevem minusiosamente cada
etapa de montagem e a ordem que elas devem ocorrer para aumentar a eficiência do processo. Através da solução proposta, os funcionários terão
uma plataforma que concentra todas as informações importantes sobre o processo de montagem de cada equipamento, onde eles podem aprender
novos processos e revisar conceitos já estudados para melhorar seu desempenho. Além disso, a solução também tem o objetivo de manter os
funcionários atualizados sobre melhorias nos processos de montagem e a inclusão de novos manuais e atualização de materiais.
BENEFÍCIOS ESPERADOS PARA O PARCEIRO: Esperamos que o projeto possa aumentar a eficiência das linhas de montagem e diminuir
o tempo de aprendizagem dos funcionários sobre alterações, melhorias, e ajustes nos processos de montagem.
MATERIAIS DE ESTUDOS ANEXADOS (detalhar):

ESCOPO MACRO
Público Alvo:
• Funcionários Dell Technologies que trabalham na fábrica da empresa e tem conhecimento das linhas de montagem de
produtos da empresa (computadores, servidores, notebooks).
• Gestores das linhas de produção da fábrica da Dell Technologies.
• Será validado por funcionários de diferentes cargos da mesma empresa (Dell), gestores e engenheiros.
Requisitos funcionais:

Internal Use - Confidential

• Permitir que os usuários se cadastrem e façam login na aplicação - considerar incluir integração Single Sign On (SSO).
• Permitir a criação de dois tipos de perfis: membros e administradores.
• Permitir a administradores a inclusão de novos manuais (não permitir a inclusão de manuais duplicados) ou a atualização
de conteúdos de manuais já cadastrados.
• Disponibilizar um catálogo de manuais com informações específicas sobre o produto a que se refere.
• Disponibilizar para os usuários uma lista de a fazer com manuais a serem estudados.
• Permitir que administradores incluam manuais nas listas de a fazer dos funcionários.
• Permitir que usuários façam buscas de manuais.
• Permitir que os usuários adicionem manuais a uma lista de favoritos.
• Exibir informações detalhadas de cada manual incluindo metadados (nome, data, versão, produto alvo, etc.).
• Possibilitar a inclusão de documentos (PDF, txt, Word, etc.), imagens, videos, e modelos 3D para cada manual.
• Permitir usuários marcarem manuais como estudados/visualizados.
• Notificar (email e através da aplicação web) usuários que já estudaram/visualizaram manuais quando houver alguma
atualização nos metadados ou materiais do manual.
• Disponibilizar histórico de estudo/visualização para cada usuário.
• Manter estatísticas para cada manual e apresentar junto com suas informações detalhadas (visualizações, funcionários que
estudaram, etc.).
• Página de perfil para cada funcionário com seus manuais publicados.
• Permitir a visualização dos materiais (documentos, videos, imagens, modelos 3D) incluídos para cada manual.
• Possibilitar administradores visualizarem as estatísticas de leitura/visualização dos manuais de forma consolidada.
Requisitos não funcionais:
• O tempo de carregamento das páginas deve ser rápido independente da quantidade de usuários acessando a aplicação.
• O sistema deve ser capaz de lidar com um grande número de solicitações simultâneas e ser facilmente escalado para lidar
com mais solicitações à medida que o tráfego aumenta.
• O sistema deve ser altamente disponível e resistente a falhas de hardware e software.
• O sistema deve ser distribuído em vários servidores e baseado em microsserviços.
• O sistema deve ser desenvolvido e implantado com o uso de containers, gerenciadores de containers, tecnologias serverless
e sistemas de mensageria.
• Deve ser possível escalar serviços da aplicação de acordo com o nível de utilização de recursos de cada serviço (CPU,
memória, rede, etc.).
Design:
• Interface de usuário intuitiva e fácil de navegar.
• Design limpo seguindo o design system da empresa. (https://www.delldesignsystem.com/)
• Layout responsivo que se adapta a diferentes dispositivos e tamanhos de tela.
Desenvolvimento:
• Desenvolvimento de uma aplicação web responsiva para hospedar e disponibilizar manuais de montagem de
computadores (PCs, servidores, notebooks).
• Implementação de um banco de dados para armazenar informações sobre manuais (metadados, arquivos, videos,
estatísticas) e usuários (informações pessoais, estatístics, favoritos).
• Implementação de sistema de cache para armazenar conteúdos mais acessados (arquivos, vídeos) para aumentar o
desempenho.

Internal Use - Confidential

• Desenvolvimento de funcionalidades para publicar, editar, e excluir manuais.
• Configuração de um sistema de notificações para manter os usuários informados sobre atualizações em manuais.
Testes:
• Teste do aplicativo web em diferentes navegadores e dispositivos para garantir que ele funcione corretamente em todos
os cenários.
• Testes de unidade para garantir que cada funcionalidade seja implementada corretamente.
• Testes de integração para garantir que as diferentes partes do sistema se integrem corretamente.
• Testes de aceitação do usuário para garantir que o aplicativo web atenda às necessidades dos usuários.

MVP
Aplicação web de treinamento.
Home: página inicial apresentando uma barra de busca de manuais e uma lista dos últimos manuais acessados pelo usuário.
Favoritos: funcionalidade listando todos os manuais adicionados como favoritos pelo usuário (se houver).
A fazer (to-do): lista os manuais pendentes de estudo que devem ser obrigatoriamente estudados pelo usuário. Administradores
podem editar essas listas adicionando ou removendo manuais a serem estudados.
Manuais (apenas administradores): funcionalidade que permite administradores visualizarem manuais cadastrados e suas
estatísticas de forma gráfica (quantidade de leituras/visualizações, acessos, quantidades por período – mês, semana, ou ano). Além
disso, a funcionalidade também permite cadastrar/editar manuais e também registrar o manual na lista de a fazer dos usuários (pode
ser por grupo de usuários).
Manual: funcionalidade que permite usuários acessarem todas as informações de um manual específico e fazer o consumo de todos
os materiais (documentos, videos, imagens, modelos 3D). Essa é o local de aprendizagem dos usuários que eles utilizam para estudar
um manual de montagem.
Perfil: funcionalidade que permite usuários visualizarem suas informações pessoais, histórico de manuais lidos/visualizados, e
estatísticas de forma gráfica (manuais lidos/visualizados ao longo do tempo, quantidades por período – mês, semana, ou ano).
Tendências: funcionalidade que lista os principais manuais (top X) do momento de acordo com a quantidade de acessos e
leituras/visualizações.
Notificações: funcionalidade que envia e lista todas as notificações lidas e não lidas pelo usuário. Além disso, as notificações devem
conter o link para acesso ao manual a que se refere.
Painel: administradores podem visualizar um painel que condensa diversas estatísticas de forma gráfica, incluindo os principais
manuais estudados, lista de pendências de manuais a fazer por funcionários, percentual de conclusão de manuais a fazer, etc.
ENTREGÁVEIS DESEJÁVEIS
Diagramas de design da solução incluindo a arquitetura conceitual e implantação adotada.
Tutorial descrevendo o projeto incluindo informações de como implantar (passo a passo para implantar a solução).
Código-fonte do protótipo incluindo manifestos de implantação dos containers.

PRÉ-REQUISITOS DO PROJETO
- A solução contempla integração com sistemas internos de single sign on (SSO);
"

Fiz o seguinte modelo conceitual do banco de dados:
"
CURSO:
ID: Identificador único do curso (chave primária).
Nome: Nome do curso.
Tipo de material: Tipo de material presente no curso (documento, imagem, vídeo, modelo 3D).
Arquivo (ou link para o arquivo): Caminho para o arquivo ou link para o material.
Produto Alvo: Produto ao qual o curso se refere.
Data de Publicação: Data em que o curso foi publicado.

TODO:
ID: Identificador único da tarefa (chave primária).
ID do usuário: Identificador do usuário associado à tarefa (chave estrangeira referenciando a entidade Usuário).
ID do curso: Identificador do curso associado à tarefa (chave estrangeira referenciando a entidade CURSO).
Status: Status da tarefa (pendente, concluído).
Data de atribuição: Data em que a tarefa foi atribuída ao usuário.

FUNCIONARIO:
ID: Identificador único do funcionário (chave primária).
Nome: Nome do funcionário.
Email: Endereço de e-mail do funcionário.
Senha (criptografada): Senha criptografada do funcionário para fins de autenticação.
Tipo de usuário: Indicação se o funcionário é um membro ou administrador.

LINHAMONTAGEM_ALUNO:
IdAluno: Identificador do aluno (chave estrangeira referenciando a entidade FUNCIONARIO).
IdLinhaMontagem: Identificador da linha de montagem (chave estrangeira referenciando a entidade LINHAMONTAGEM).

LINHAMONTAGEM:
Id: Identificador único da linha de montagem (chave primária).
Nome: Nome da linha de montagem.
Descrição: Descrição da linha de montagem.

Relacionamentos:
Um curso pode ter muitas tarefas associadas a ele (relação 1 para N entre CURSO e TODO).
Um funcionário pode ter muitas tarefas (relação 1 para N entre FUNCIONARIO e TODO).
Um funcionário pode estar associado a uma ou várias linhas de montagem (relação 1 para N entre FUNCIONARIO e LINHAMONTAGEM_ALUNO).
Uma linha de montagem pode ter muitos funcionários associados a ela (relação 1 para N entre LINHAMONTAGEM e LINHAMONTAGEM_ALUNO).
"

Vou te explicar o wireframe da aplicação:
"
Tela de login e cadastro - o usuário pode criar uma conta ou entrar com a sua no sistema

homepage - lista to-do dos cursos do usuário, carrossel com os cursos novos, carrossel com os acessados por último e carrossel com os cursos favoritos

lista de cursos - lista todos cursos possibilitando a filtragem por pesquisa ou outras características.

curso - exibição completa do curso, exibição dos materiais e possibilidade de marcar curso como concluído

perfil - exibição de informações do usuário além da possibilidade dele alterá-las

edição (tela exibida somente ao admin) - nela o admin vai poder visualizar, editar, alterar e apagar tanto as linhas de montagem quanto os cursos. Também será possível visualizar os funcionários e suas atividades
"

Ajude-me a fazer um diagrama MVC desta aplicação WEB. -->

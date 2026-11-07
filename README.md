# Domain Driven Design Using Java — Checklist GS 2026/2

**Tema:** CitizenTech • **Stack:** Java, Spring Boot, JDBC, Oracle FIAP, Docker, Render • **Fonte:** PDF oficial, págs. **47–56** • **Entrega:** 19/11/2026, 23h55 • **Apresentação:** 23–27/11.

## Objetivo e arquitetura

Desenvolver uma **API RESTful Spring Boot** de domínio próprio da solução, persistindo dados no **Oracle FIAP**, publicada via **Docker no Render** e consumida pelo Front-End da equipe.

**Fluxo:** `Front-End → HTTP/JSON → Controller → DTO/ModelMapper → DAO/JDBC/DataSource → Oracle FIAP`.

### Estrutura esperada

- [ ] Pacote `model`: entidades da solução.
- [ ] Pacote `dao`: acesso a dados com JDBC.
- [ ] Pacote `dto`: objetos de entrada e saída.
- [ ] Pacote `controller`: endpoints REST e status HTTP.
- [ ] Pacote `exception`: exceções específicas do domínio.
- [ ] Pacote `handler`: padronização das respostas de erro.
- [ ] Pacote `config`: configuração de CORS.

## Requisitos técnicos

### Domínio, banco e persistência

- [ ] **3 entidades ou mais** relacionadas a problemas reais da CitizenTech (não CRUD genérico de usuários/produtos).
- [ ] Pelo menos **um relacionamento 1:N** entre as entidades.
- [ ] Tabelas Oracle no padrão da disciplina: `tb_nome`, campos `cd_`, `nm_`, `ds_`, `dt_` etc.
- [ ] Sequences no padrão `sq_tb_nome` e FK representando o 1:N.
- [ ] Um **DAO por entidade**.
- [ ] Usar **JDBC + `PreparedStatement`**, recebendo `DataSource` pelo construtor do DAO.
- [ ] CRUD completo **para cada entidade**: cadastrar, listar, buscar por ID, atualizar e remover.
- [ ] Endpoints **aninhados** correspondentes ao relacionamento 1:N.
- [ ] DTOs de **entrada e saída**; nunca receber nem devolver diretamente os objetos `model`.
- [ ] Converter model/DTO com **ModelMapper**.
- [ ] Uma **regra de negócio além do CRUD**, aplicada no fluxo da API, com exceção própria.

### Validações, HTTP e implantação

- [ ] **Bean Validation** nos DTOs de entrada (`@NotBlank`, `@Size`, `@Email`, `@Positive`, `@Future` etc., conforme dados).
- [ ] `@Valid` no controller.
- [ ] Exceções próprias e **`@RestControllerAdvice`** com erro em **JSON padronizado**.
- [ ] Não expor `stack trace`, detalhes de SQL ou páginas HTML nos erros.
- [ ] Usar status corretos **201 + `Location`, 200, 204, 400, 404, 405, 409** (e 500 genérico para erros inesperados).
- [ ] Configurar **CORS** para acesso do Front-End.
- [ ] Criar **Dockerfile multi-stage**.
- [ ] Publicar API no **Render**, com URL pública funcionando.
- [ ] Integrar Front-End da equipe, **listando e cadastrando dados reais** pela API; o Front-End não precisa estar publicado para o critério específico de Java.
- [ ] Configurar credenciais Oracle via **variáveis de ambiente**; nunca salvar usuário/senha no código enviado.

### Contrato de endpoints (exemplo do PDF, adaptar aos recursos reais)

| Método | Rota ilustrativa | Sucesso | Erros esperados |
|---|---|---|---|
| POST | `/ocorrencias` | 201 + `Location` | 400, 409 |
| GET | `/ocorrencias` | 200 | — |
| GET | `/ocorrencias/{id}` | 200 | 404 |
| PUT | `/ocorrencias/{id}` | 200 | 400, 404, 409 |
| DELETE | `/ocorrencias/{id}` | 204 | 404, 409 |
| GET | `/ocorrencias/{id}/atualizacoes` | 200 | 404 |
| POST | `/ocorrencias/{id}/atualizacoes` | 201 + `Location` | 400, 404 |

`409` cobre violação de regra e remoção de registros com filhos; método não suportado usa `405`; erro inesperado usa `500` com mensagem genérica.

## Pontuação da entrega — 70 pontos

| Item | Pontos |
|---|---:|
| Modelagem de domínio, 1:N e SQL | 5 |
| JDBC + DAO + CRUD funcionando | 15 |
| DTOs e Bean Validation | 10 |
| Controllers, rotas e status HTTP | 10 |
| Exceções, handler e regra de negócio | 10 |
| CORS e integração com Front-End | 5 |
| API funcional no Render | 5 |
| Documentação técnica PDF | 10 |
| **Total da entrega** | **70** |

## Entregáveis do `.zip`

- [ ] Projeto **Spring Boot completo**, incluindo `Dockerfile`, **sem `target/`**.
- [ ] Script **SQL** das tabelas e sequences.
- [ ] **PDF de documentação técnica**.
- [ ] `README.md` **ou** `.txt` na raiz com nome/RM dos integrantes, como rodar, URL da API Render e link do vídeo.
- [ ] **Vídeo de até 5 minutos** no **YouTube não listado**, mostrando o Front-End funcionando integrado à API e explicação do sistema; colocar o link no README e na documentação.
- [ ] Compactar tudo em **um único `.zip`** e enviar na área de DDD.

### Documentação técnica PDF — todos os itens obrigatórios

- [ ] Capa: integrantes/RMs, link da API e do vídeo.
- [ ] Problema do cidadão, público usuário e função da API.
- [ ] **Diagrama de classes:** models, atributos, tipos e relações.
- [ ] **DER**: tabelas, PKs e FK do 1:N.
- [ ] Endpoints: método, rota, status e exemplos JSON de entrada e saída.
- [ ] Arquitetura do projeto: pacotes e responsabilidades.
- [ ] Regras de negócio e validações dos DTOs.
- [ ] Processo de deploy e **nomes** das variáveis de ambiente, **sem valores**.

## Apresentação presencial — 30 pontos

- [ ] **8 pts:** API online demonstrada no Postman ou Insomnia com CRUD.
- [ ] **4 pts:** erro ao vivo com `400` (cadastro inválido) e `404` (ID inexistente).
- [ ] **6 pts:** explicar o caminho da requisição entre controller, DTO, DAO e banco.
- [ ] **4 pts:** entidades, relacionamento 1:N e regra de negócio.
- [ ] **8 pts:** qualquer integrante responde a perguntas do professor.

## Penalidades / riscos

- **Nota zero:** entrega fora do prazo/Portal, ZIP vazio/corrompido/sem código ou cópia de outro grupo/projeto pronto.
- **−30 pts:** solução não relacionada à CitizenTech.
- **−20 pts:** usar **JPA/Hibernate/Spring Data** no lugar de JDBC (não aceitos).
- **−10 pts:** API fora do ar/link quebrado durante correção.
- **−10 pts:** credenciais do banco hardcoded no código entregue.
- **−5 pts:** erros expostos em HTML, `stack trace` ou SQL.

Os descontos se acumulam sobre a nota final, com piso de zero (pág. 53).

_Fonte: Global Solution 2026/2 — Domain Driven Design Using Java, págs. 47–56._

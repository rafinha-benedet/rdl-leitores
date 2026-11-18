# Repositório das Letras — Leitores

Microsserviço de **Leitores** do sistema [Repositório das Letras](https://github.com/kauanysartor/Repositorio-das-Letras). O documento de requisitos e o descritivo técnico completo estão no repositório principal.

Gerencia o cadastro de leitores, a situação (`Ativo`, `Inativo`, `Bloqueado`) e o valor da dívida.

## Endpoints

| Método | Rota                            | Descrição                                   |
|--------|---------------------------------|---------------------------------------------|
| GET    | `/api/leitores`                 | Lista os leitores                           |
| GET    | `/api/leitores/{id}`            | Busca um leitor                             |
| POST   | `/api/leitores`                 | Cadastra um leitor                          |
| PUT    | `/api/leitores/{id}`            | Atualiza um leitor                          |
| PATCH  | `/api/leitores/{id}/inativar`   | Inativa um leitor                           |
| PATCH  | `/api/leitores/{id}/divida`     | Altera dívida e situação (usado por Multas) |

Documentação interativa (Scalar) em `/scalar`.

## Integrações

| # | Tipo                   | Quem chama                                                           | Rota                              |
|---|------------------------|----------------------------------------------------------------------|-----------------------------------|
| 1 | Busca de dados simples | [Empréstimos](https://github.com/alexsanderantunesbarbosa/rdl-emprestimos) | `GET /api/leitores/{id}`          |
| 3 | Alteração de dados     | [Multas](https://github.com/alexsanderantunesbarbosa/rdl-multas)          | `PATCH /api/leitores/{id}/divida` |

- **Integração 1:** ao registrar ou consultar um empréstimo, o serviço de Empréstimos busca o leitor aqui para validar se está ativo e sem bloqueio.
- **Integração 3:** ao lançar uma multa, o serviço de Multas soma o valor à dívida do leitor e o bloqueia. No pagamento, reduz a dívida e libera o leitor se ela zerar.

## Configuração

| Variável                               | Descrição                                   | Padrão                    |
|----------------------------------------|---------------------------------------------|---------------------------|
| `ConnectionStrings__DefaultConnection` | Conexão com o banco SQLite                  | `Data Source=Leitores.db` |
| `PORT`                                 | Porta informada ao Render (usar `8080`)     | —                         |

## Como executar localmente

```
cd AppTemplate
dotnet run
```

Acessar `http://localhost:5002/scalar`.

## Docker

```
docker build -t rdl-leitores .
docker run -p 8080:8080 rdl-leitores
```

## Outros microsserviços

- [Livros](https://github.com/alexsanderantunesbarbosa/rdl-livros)
- [Empréstimos](https://github.com/alexsanderantunesbarbosa/rdl-emprestimos)
- [Multas](https://github.com/alexsanderantunesbarbosa/rdl-multas)

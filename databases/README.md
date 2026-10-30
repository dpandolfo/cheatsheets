# Databases

Guias de referência para ferramentas e comandos de banco de dados.

## 📚 Conteúdo

- **[PostgreSQL](./postgresql.md)** — Banco de dados SQL relacional
  - Connection e user management
  - Schema e table management
  - Data manipulation (INSERT, UPDATE, DELETE)
  - Queries e aggregations
  - Joins e views
  - Backup e restore
  - Performance monitoring

- **[Redis](./redis.md)** — Cache e data store in-memory
  - String operations
  - List, set, sorted set operations
  - Hash operations
  - Key management
  - Pub/Sub
  - Transactions
  - Useful patterns (rate limiting, sessions, leaderboards)

## 🔗 Próximas Adições Planejadas

- MySQL
- MongoDB
- DynamoDB
- Elasticsearch
- Cassandra

## 💡 Dicas

- PostgreSQL: Use `EXPLAIN ANALYZE` para otimizar queries
- Redis: Use `SCAN` ao invés de `KEYS` em bases grandes
- Sempre faça backup antes de operações destrutivas
- Monitore performance regularmente

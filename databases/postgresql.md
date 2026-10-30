# PostgreSQL Cheatsheet

## Connection

```bash
# Connect to local database
psql -U postgres

# Connect to specific database
psql -U postgres -d mydb

# Connect to remote host
psql -U postgres -h localhost -d mydb

# Connect with port
psql -U postgres -h localhost -p 5432 -d mydb

# Connect with password prompt
psql -U postgres -d mydb -W

# Connect and specify file to execute
psql -U postgres -d mydb -f script.sql

# Get connection info
\conninfo
```

## Database and User Management

```sql
-- List all databases
\l

-- Create database
CREATE DATABASE mydb;

-- Create database with encoding
CREATE DATABASE mydb ENCODING 'UTF8' LC_COLLATE 'en_US.UTF-8';

-- Drop database
DROP DATABASE mydb;

-- Connect to database
\c mydb

-- Show current database
SELECT current_database();

-- List all users/roles
\du

-- Create user
CREATE USER myuser WITH PASSWORD 'password';

-- Create superuser
CREATE USER admin WITH PASSWORD 'password' SUPERUSER;

-- Drop user
DROP USER myuser;

-- Grant privileges
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;

-- Revoke privileges
REVOKE ALL PRIVILEGES ON DATABASE mydb FROM myuser;
```

## Schema and Table Management

```sql
-- List schemas
\dn

-- Create schema
CREATE SCHEMA myschema;

-- Drop schema
DROP SCHEMA myschema;

-- List tables in schema
\dt myschema.*

-- List all tables
\dt

-- Create table
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create table with constraints
CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  user_id INT REFERENCES users(id),
  total DECIMAL(10, 2) NOT NULL CHECK (total > 0),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Show table structure
\d users

-- Add column
ALTER TABLE users ADD COLUMN phone VARCHAR(20);

-- Drop column
ALTER TABLE users DROP COLUMN phone;

-- Rename column
ALTER TABLE users RENAME COLUMN name TO full_name;

-- Rename table
ALTER TABLE users RENAME TO customers;

-- Drop table
DROP TABLE users;

-- Truncate table (remove all data)
TRUNCATE TABLE users;
```

## Indexes

```sql
-- Create index
CREATE INDEX idx_users_email ON users(email);

-- Create unique index
CREATE UNIQUE INDEX idx_users_email_unique ON users(email);

-- Create compound index
CREATE INDEX idx_orders_user_created ON orders(user_id, created_at);

-- Create index on expression
CREATE INDEX idx_users_lower_email ON users(LOWER(email));

-- List indexes
\di

-- Drop index
DROP INDEX idx_users_email;

-- Analyze index usage
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';
```

## Data Manipulation

```sql
-- Insert single row
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');

-- Insert multiple rows
INSERT INTO users (name, email) VALUES
  ('John Doe', 'john@example.com'),
  ('Jane Smith', 'jane@example.com');

-- Insert with returning
INSERT INTO users (name, email) VALUES ('Bob', 'bob@example.com') RETURNING id;

-- Update rows
UPDATE users SET email = 'newemail@example.com' WHERE id = 1;

-- Update with condition
UPDATE users SET updated_at = CURRENT_TIMESTAMP WHERE created_at < NOW() - INTERVAL '30 days';

-- Delete rows
DELETE FROM users WHERE id = 1;

-- Delete all rows
DELETE FROM users;
```

## Queries and Aggregations

```sql
-- Select all columns
SELECT * FROM users;

-- Select specific columns
SELECT id, name, email FROM users;

-- Select with WHERE
SELECT * FROM users WHERE email = 'john@example.com';

-- Select with LIKE
SELECT * FROM users WHERE name LIKE 'John%';

-- Select with IN
SELECT * FROM users WHERE id IN (1, 2, 3);

-- Select with ORDER BY
SELECT * FROM users ORDER BY created_at DESC;

-- Select with LIMIT
SELECT * FROM users LIMIT 10;

-- Select with OFFSET
SELECT * FROM users LIMIT 10 OFFSET 20;

-- Count rows
SELECT COUNT(*) FROM users;

-- Count distinct values
SELECT COUNT(DISTINCT email) FROM users;

-- Group by
SELECT email, COUNT(*) FROM users GROUP BY email;

-- Group by with HAVING
SELECT email, COUNT(*) as count FROM users GROUP BY email HAVING COUNT(*) > 1;

-- Aggregate functions
SELECT 
  COUNT(*) as total,
  AVG(total) as average,
  MAX(total) as maximum,
  MIN(total) as minimum,
  SUM(total) as sum
FROM orders;
```

## Joins

```sql
-- Inner join
SELECT u.name, o.id FROM users u INNER JOIN orders o ON u.id = o.user_id;

-- Left join
SELECT u.name, o.id FROM users u LEFT JOIN orders o ON u.id = o.user_id;

-- Right join
SELECT u.name, o.id FROM users u RIGHT JOIN orders o ON u.id = o.user_id;

-- Full outer join
SELECT u.name, o.id FROM users u FULL OUTER JOIN orders o ON u.id = o.user_id;

-- Cross join
SELECT u.id, p.id FROM users u CROSS JOIN products p;

-- Self join
SELECT a.name, b.name FROM users a, users b WHERE a.id != b.id;
```

## Views and Functions

```sql
-- Create view
CREATE VIEW active_users AS
  SELECT * FROM users WHERE deleted_at IS NULL;

-- Create materialized view
CREATE MATERIALIZED VIEW user_summary AS
  SELECT user_id, COUNT(*) as order_count FROM orders GROUP BY user_id;

-- Refresh materialized view
REFRESH MATERIALIZED VIEW user_summary;

-- Drop view
DROP VIEW active_users;

-- Create function
CREATE FUNCTION get_user_orders(user_id INT)
  RETURNS TABLE(order_id INT, total DECIMAL) AS $$
  SELECT id, total FROM orders WHERE user_id = $1;
$$ LANGUAGE SQL;

-- Call function
SELECT * FROM get_user_orders(1);
```

## Transactions

```sql
-- Start transaction
BEGIN;

-- Commit transaction
COMMIT;

-- Rollback transaction
ROLLBACK;

-- Savepoint
SAVEPOINT sp1;

-- Rollback to savepoint
ROLLBACK TO sp1;

-- Release savepoint
RELEASE sp1;

-- Example transaction
BEGIN;
  UPDATE accounts SET balance = balance - 100 WHERE id = 1;
  UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

## Backup and Restore

```bash
# Backup database
pg_dump -U postgres mydb > backup.sql

# Backup with format
pg_dump -U postgres -F c mydb > backup.dump

# Backup specific table
pg_dump -U postgres -t users mydb > users_backup.sql

# Backup all databases
pg_dumpall -U postgres > all_databases.sql

# Restore from dump
psql -U postgres mydb < backup.sql

# Restore from custom format
pg_restore -U postgres -d mydb backup.dump

# Restore specific table
pg_restore -U postgres -d mydb -t users backup.dump
```

## Performance and Monitoring

```sql
-- Show current connections
SELECT * FROM pg_stat_activity;

-- Show long-running queries
SELECT pid, query, query_start FROM pg_stat_activity 
  WHERE query != '<IDLE>' ORDER BY query_start;

-- Kill long-running query
SELECT pg_terminate_backend(pid) FROM pg_stat_activity 
  WHERE pid <> pg_backend_pid() AND query_start < NOW() - INTERVAL '10 minutes';

-- Check database size
SELECT pg_database.datname, pg_size_pretty(pg_database_size(pg_database.datname)) 
  FROM pg_database;

-- Check table size
SELECT tablename, pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) 
  FROM pg_tables WHERE schemaname = 'public';

-- Analyze query plan
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';

-- Show table statistics
SELECT * FROM pg_stat_user_tables;

-- Vacuum and analyze
VACUUM ANALYZE;
```

## Useful psql Commands

| Command | Purpose |
|---------|---------|
| `\l` | List databases |
| `\c database` | Connect to database |
| `\dt` | List tables |
| `\d table` | Show table structure |
| `\du` | List users |
| `\dn` | List schemas |
| `\di` | List indexes |
| `\dv` | List views |
| `\df` | List functions |
| `\e` | Open editor |
| `\i file.sql` | Execute SQL file |
| `\o file.txt` | Write output to file |
| `\q` | Quit |

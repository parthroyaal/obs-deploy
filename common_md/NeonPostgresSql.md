# Setup

## In Neon Query:

````sql
CREATE TABLE your_table (
    column1 VARCHAR(255),
    column2 VARCHAR(255),
    timestamp TIMESTAMP
);

````

````sql
SELECT datname FROM pg_database;
\dt
SELECT table_name FROM information_schema.tables WHERE table_schema = 'public'
SELECT * FROM trades



CREATE TABLE trades (
    id SERIAL PRIMARY KEY,  -- Unique auto-incrementing ID
    timestamp TIMESTAMP NOT NULL,  -- Timestamp for each trade
    price DECIMAL(18,8) NOT NULL  -- Storing price with precision
);

````

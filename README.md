# postgresql-ts-dict-ru

Russian full text search dictionary for PostgreSQL.

# Installation

Copy russian.affix and russian.dict files into PostgreSQL `share/tsearch_data` directory.

Create russian language dictionary.

```sql
create text search dictionary russian_ispell (
    template = ispell,
    dictfile = russian,
    afffile = russian,
    stopwords = russian
);
```

After that you can create new configuration or alter one provided by PostgreSQL.

# New configuration

Creation of new configuration named `ru`.

```sql
-- Create configuration based on existing one.
create text search configuration ru (
	copy = pg_catalog.russian
);

-- Alter configuration to use russian_ispell dictionary.
alter text search configuration ru
	alter mapping for hword, hword_part, word with russian_ispell, russian_stem;
```

Test configuration with this query.

```sql
select to_tsvector('ru', 'мама мыла раму'); -- 'мама':1 'мыло':2 'мыть':2 'рама':3
```

You need pass configuration name `ru` to every search query or you can set it as default configuration in `postgresql.conf` file.

```sql
set default_text_search_config = 'ru';
```

# Altering existing configuration

Instead of creating new configuration you can alter existed configuration `pg_catalog.russian`.

You will need additional rights for this.

```sql
alter text search configuration pg_catalog.russian
    alter mapping for hword, hword_part, word with russian_ispell, russian_stem;
```

# Anyquery

## Install
```
brew install julien040/anyquery/anyquery
```

## Querying Plugin
```
anyquery install <plugin-name>
```

### Github Plugin
> reference: https://github.com/julien040/anyquery/tree/main/plugins/github

```
anyquery install github
```

show my repo:
```
anyquery -q "SELECT * FROM github_my_repositories;"
```

show all tables:
```
anyquery -q "SHOW TABLES;"
```

## Querying Files
> reference: https://anyquery.dev/docs/usage/querying-files

### Read Json
From http:
```
anyquery -q "SELECT full_name FROM read_json('https://formulae.brew.sh/api/formula.json');"
```

output:
```
+------------------------------+
|          full_name           |
+------------------------------+
| a2ps                         |
| a52dec                       |
| aalib                        |
| aamath                       |
| aarch64-elf-binutils         |
| aarch64-elf-gcc              |
| aarch64-elf-gdb              |
| aardvark_shell_utils         |
| abcde                        |
| abcl                         |

...

+------------------------------+
7148 results

```

From path:
```
-- Query the whole JSON file
SELECT * FROM read_json('path/to/file.json');
-- Query a specific path in the JSON file
SELECT * FROM read_json('path/to/file.json', '$.items[*]');
-- You can also specify the parameters with named arguments.
SELECT * FROM read_json(path='path/to/file.json', json_path='$.items[*]');
```

### Read CSV
```
-- Query the whole CSV file
SELECT * FROM read_csv('path/to/file.csv');
-- Query a CSV file with a header
SELECT * FROM read_csv('https://csvbase.com/meripaterson/stock-exchanges.csv', header=true);
-- Query a CSV file with a header and a custom delimiter
SELECT * FROM read_csv('path/to/file.csv', header=true, delimiter=';');
-- You also specify a schema for the CSV file, and any query will make the best effort to parse the file according to the schema.
SELECT * FROM read_csv('https://csvbase.com/rmirror/us-covid-cases', schema='CREATE TABLE csv (id int, date varchar, cases int)', header=true) LIMIT 30;
```

### Read TSV
```
-- To query a TSV file, use the read_csv function with the delimiter set to \t.
SELECT * FROM read_csv('path/to/file.tsv', delimiter='\t');
```

### Read HTML
```
SELECT * FROM read_html('https://en.wikipedia.org/wiki/List_of_countries_by_GDP_(nominal)', '.wikitable', cache='3600');
```

### Read Parquet
```
SELECT * FROM read_parquet('https://csvbase.com/calpaterson/english-womens-football-matches.parquet');
```

### Read YAML
```
SELECT * FROM read_yaml('path/to/file.yaml');
```

### Read TOML
```
SELECT * FROM read_toml('path/to/file.toml');
```

## Querying log files
> reference: https://anyquery.dev/docs/usage/querying-log

```
-- Query redis log file
SELECT * FROM read_log('path/to/redis.log', '(?<process>%{WORD}:%{WORD}) %{DATA:time} \* %{GREEDYDATA:message}');
-- Query nginx log file
SELECT * FROM read_log('path/to/nginx.log', '%{IPORHOST:clientip} - %{DATA:ident} %{DATA:auth} \[%{HTTPDATE:timestamp}\] "%{WORD:verb} %{DATA:request} HTTP/%{NUMBER:httpversion}" %{NUMBER:response} %{NUMBER:bytes} "%{DATA:referrer}" "%{DATA:agent}"');
-- Passing a custom pattern file
SELECT * FROM read_log('path/to/nginx.log', '%{NGINX}', filePattern='path/to/patterns.grok');
-- Arguments can also be passed as named arguments.
SELECT * FROM read_log(path='path/to/redis.log', pattern='(?<process>%{WORD}:%{WORD}) %{DATA:time} \* %{GREEDYDATA:message}', filePattern='path/to/patterns.grok');
```

## Exporting Result
> reference: https://anyquery.dev/docs/usage/exporting-results

### Specify the format
```
# JSON
anyquery -q "SELECT * FROM table" --json
# CSV
anyquery -q "SELECT * FROM table" --csv
# Plain text
anyquery -q "SELECT * FROM table" --plain
# HTML
anyquery -q "SELECT * FROM table" --format html
```

### Redirecting the output
```
anyquery -q "SELECT * FROM table" --json > output.json
```


## Launching the MySQL server
To launch the MySQL server, you need to use the command server. It will start the server on `127.0.0.1:8070` without any authentication by default.
```
anyquery server
```

### Changing the address and port
```
anyquery server --host "127.0.0.1" --port 3306
```

## Query hub
> The query hub is a place where you can find pre-made SQL queries for Anyquery.
> Answer questions on your data easily using Anyquery and its hub of queries.

- Link: https://anyquery.dev/queries

example:
```
anyquery run hello_world
```


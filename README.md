# Connecting to PostgreSQL from Python

```
import psycopg2

hostname = 'localhost'
database = 'demo'
username = 'postgres'
pwd = 'postgres'
port_id = 5432

try:
    conn = psycopg2.connect(
        host = hostname,
        dbname = database,
        user = username,
        password = pwd,
        port = port_id
    )
    conn.close()
except Exception as error:
    print(error)
```

# Opening a cursor to perform databases operations

```
...
port_id = 5432
conn = None
cur = None

try:
    conn = psycopg2.connect(
...
    )
    cur = conn.cursor()
except Exception as error:
    print(error)
finally:
    if cur is not None:
        cur.close()
    if conn is not None:
        conn.close()

```

# Create table in PostgreSQL database from Pyhton program

```
...
    cur = conn.cursor()

    create_script = '''
    CREATE TABLE IF NOT EXISTS employee(
        id          in PRIMARY KEY,
        name        varchar(40) NOT NULL,
        salary      int,
        dept_id     varchar(30)
    )
    '''

    cur.execute(create_script)

    conn.commit()
except Exception as error:
...
```

# INSERT single record to table in PostgreSQL from Python program

```
...
    cur.execute(create_script)

    insert_script = 'INSERT INTO employee (id, name, salary, dept_id) VALUES (%s, %s, %s, %s)'
    insert_value = (1, 'James', 12000, 'D1')
    cur.execute(insert_script, insert_value)

    conn.commit()
...
```

# INSERT multiple records to PostgreSQL table from Python program

```
...
    cur = conn.cursor()

    cur.execute('DROP TABLE IF EXISTS employee')    # <----- add this

    create_script = '''
...
    '''
    cur.execute(create_script)

    insert_script = 'INSERT INTO employee (id, name, salary, dept_id) VALUES (%s, %s, %s, %s)'
    insert_values = [(1, 'James', 12000, 'D1'), (2, 'Robin', 15000, 'D1'), (3, 'Xavier', 20000, 'D2')]
    for record in insert_values:
        cur.execute(insert_script, record)

    conn.commit()
...
```

# FETCH data from PostgreSQL Table and display in Pyhton program

```
import psycopg2
import psycopg2.extras                                               # <---- add this
...
try:
    conn = psycopg2.connect(
...
    )
    cur = conn.cursor(cursor_factory = psycopg2.extras.DictCursor)      # <---- Edit

    cur.execute('DROP TABLE IF EXISTS employee')

    create_script = '''
...
    '''
...
    for record in insert_values:
        cur.execute(insert_script, record)

    cur.execute('SELECT * FROM EMPLOYEE')
    # print(cur.fetchall())                                     # <--
    for record in cur.fetchall():
        # print(record)                                         # <--
        # print(record[1], record[2])                           # <--
        # affter do finish about psycopg2.extras.DictCursor
        print(record['name'], record['salary'])

    conn.commit()
except Exception as error:
    print(error)
finally:
    if cur is not None:
        cur.close()
    if conn is not None:
        conn.close()

...
```

# UPDATE Table data in PostgreSQL database from Python program

```
...
    insert_script = 'INSERT INTO employee (id, name, salary, dept_id) VALUES (%s, %s, %s, %s)'
    insert_values = [(1, 'James', 12000, 'D1'), (2, 'Robin', 15000, 'D1'), (3, 'Xavier', 20000, 'D2')]
    for record in insert_values:
        cur.execute(insert_script, record)

    update_script = 'UPDATE employee SET salary = salary + (salary * 0.5)'          # <---
    cur.execute(update_script)                                                      # <---

    cur.execute('SELECT * FROM EMPLOYEE')
    for record in cur.fetchall():
        print(record['name'], record['salary'])
...
```

# DELETE Table record in PostgrSQL database from Python program

```
...
    update_script = 'UPDATE employee SET salary = salary + (salary * 0.5)'
    cur.execute(update_script)

    delete_script = 'DELETE FROM employee WHERE name = %s'                  # <---
    delete_record = ('James',)                                              # <---
    cur.execute(delete_script, delete_record)                               # <---

    cur.execute('SELECT * FROM EMPLOYEE')
    for record in cur.fetchall():
        print(record['name'], record['salary'])
...
```

# Using Context Manager to connect to PostgreSQL database from Python

last edit

Cr : https://www.youtube.com/watch?v=M2NzvnfS-hI

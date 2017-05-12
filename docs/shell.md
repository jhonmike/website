# Comando para dar redirect no shell e limpar mensagens

```bash
[:command] &>/dev/null
```

# Python pipe


With Python 2.6+ you can just do:

`echo '{"foo": "lorem", "bar": "ipsum"}' | python -m json.tool`

or, if the JSON is in a file, you can do:

`python -m json.tool my_json.json`

if the JSON is from an internet source such as an API, you can use

`curl http://my_url/ | python -m json.tool`

for convenience in all of these cases you can make an alias:

`alias prettyjson='python -m json.tool'`

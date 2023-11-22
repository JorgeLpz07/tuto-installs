# Comandos Windows

## Mostrar encabezado

```sh
gc log.txt | select -first 10 # head
```

## Mostrar pie

```sh
gc log.txt | select -last 10  # tail
```

## Mostar contenido

```sh
gc -TotalCount 10 log.txt     # also head
```

## Contar lineas de archivo

Opción 1:

```sh
c:\temp> type archivo.txt | find /v /c ""
41525
```

Opción 2:

```sh
c:\temp> find /v /c "" archivo.txt
---------- ARCHIVO.TXT: 41525
```

Opción 3: En Windows, puedes contar los saltos de línea (número de líneas) de un archivo usando PowerShell

```sh
# Contar saltos de línea en un archivo
(Get-Content -Path "archivo.txt" | Measure-Object -Line).Lines
```

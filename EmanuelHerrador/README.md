# Redes 2023 - Laboratorio 1

## Enlaces para testear [hget.py](hget.py) (provistos en el Zulip)

 - http://example.com/

 - http://httpbin.org/

 - http://www.textfiles.com/

 - http://web.archive.org/

 - http://www.webpagesthatsuck.com/

 - http://saludcba.com.ar/

## Formato PEP8

Para seguir las condiciones de estilo de código pedidas por la cátedra, se ha realizado un script que formatea el código de python en PEP8 automáticamente en Visual Studio Code (cuando lo guardamos).
El archivo se encuentra en [settings.json](.vscode/settings.json).

Los **requisitos** para usarlo y que funcione es instalar pep8 y pylint. Para ello, se pueden ejecutar los siguientes comandos:
```sh
pip install pep8
pip install pylint
```

## Punto estrella

### Planteo de la situación

Investigar qué mecanismos permiten funcionar a nombres de dominio como:

 - http://中文.tw/

 - https://💩.la

### Investigación

Los nombres de las direcciones dentro de internet se encuentran codificadas bajo el _estándar IDN_ (propuesto en 1998 y adoptado en 2005), el cual permite que un dominio de internet contenga carácteres no ASCII (sean acentos, letras griegas o chinas, emoticones, entre otros). El sistema que se utiliza es llamado IDNA (Internationalizing Domain Names in Aplicattions).

La forma en que funciona IDNA es mediante tres algoritmos principales:

 - Nameprep: normaliza la etiqueta dada (convierte todo a minúsculas y elimina puntos de código invisibles). Usa el estándar Unicode para la normalización NFKC.

 - ToASCII: Solo se usa cuando la etiqueta tiene algún caracter no ASCII. Aplica Nameprep y traduce el resultando usando Punycode antes de anteponer la cadena "xn--" (prefijo ACE para distinguir las etiquetas que pasaron por este proceso respecto a las ASCII que no lo necesitan).

 - ToUnicode: revierte la acción de ToASCII, sacando el prefijo ACE y aplicando el algoritmo de decodificación Punycode. Lo importante a saber es que no revierte el proceso hecho por Nameprep ya que es una normalización.

Varios ejemplos de la conversión de los enlaces pueden ser los siguientes:

 - http://中文.tw/ es **xn--fiq228c.tw**

 - https://💩.la es **xn--ls8h.la/**

(anteponer http:// o https:// a xn-- para ver que realmente son equivalentes).

En caso que tengamos un `server_name` del cual queramos saber su codificación en ASCII, lo que debemos hacer en python3 es lo siguiente:

```py
def see_ASCII_name(server_name):
  server_name_encoded = server_name.encode('idna')
  print(server_name_encoded)
```

### Webgrafía

 - _7.8. codecs — Codec registry and base classes — Python 2.7.18 documentation_. (s. f.). https://docs.python.org/2/library/codecs.html

 - _codecs — Codec registry and base classes — Python 3.10.10 documentation._ (s. f.). https://docs.python.org/3.10/library/codecs.html

 - _Python Convert punycode back to unicode._ (2022, 19 abril). Stack Overflow. https://stackoverflow.com/questions/71922124/python-convert-punycode-back-to-unicode

 - _quick example of encoding and decoding a international domain name in Python (from Unicode to Punycode or IDNA codecs and back). Pay attention to the Unicode versus byte strings._ (s. f.). Gist. https://gist.github.com/pessom/b6c8c4d55296e5439403ae3cc942fbbc

 - Wikipedia contributors. (2022, 23 enero). _Nameprep_. Wikipedia. https://en.wikipedia.org/wiki/Nameprep

 - Wikipedia contributors. (2023a, enero 8). _Unicode equivalence._ Wikipedia. https://en.wikipedia.org/wiki/Unicode_equivalence

 - Wikipedia contributors. (2023b, febrero 15). _Punycode_. Wikipedia. https://en.wikipedia.org/wiki/Punycode
 
 - Wikipedia contributors. (2023c, febrero 25). _Internationalized domain name_. Wikipedia. https://en.wikipedia.org/wiki/Internationalized_domain_name
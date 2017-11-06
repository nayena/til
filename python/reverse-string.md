# Reverse string

When learning to work with strings in any language, one will have to
learn about string reversing. While it may take a while until you will
need it in real-world applications, it does teach you about the complex
topic of encoding.

## Non-unicode version:

```python
>>> 'Hello World'[::-1]
'dlroW olleH'
```

## Unicode version:

```python
>>> a = 'æøå'
>>> print a.decode('utf8')[::-1].encode('utf8')
'åøæ'
```

It's easier to remember to define strings as unicode from the beginning.

```
>>> print u'æøå'[::-1]
'åøæ'
```

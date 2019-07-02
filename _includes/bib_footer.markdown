## References

{% bibliography --cited %}

## Cite this post

If you'd like to cite this post, please use the following bibtex:

{% assign num = page.url | size | minus: 1 %}
{% assign citekey = page.url | replace: "/", "_" | slice: 0, num %}

```
@misc{Rogers{{ citekey }},
  title = { {{ page.title }}},
  journal = {Hacking Semantics},
  url = { https://hackingsemantics.xyz{{page.url}} },
  author = {Rogers, Anna},
  day = { {{page.date | date: "%d"}} },
  month = { {{page.date | date: "%b"}} },
  year = { {{ page.date | date: "%Y" }} }
}
```


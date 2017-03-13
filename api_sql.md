#sqls used against api.db
##substitutions
*{mf}: ’0’ or ‘1’
*{ope} : ’=’ or ‘like’
*? : entered search

NB: 'like' does not use the index, and will be slower  
use like only if the user has entereed wildchars  

##search
```select entries from headword{mf} where orth {ope} ?"```
If this results in more than one row, list alternatives.
If no results:

```select headwords from inflection{mf} where orth {ope} ?```

If this results on more than one row list alternatives
otherwise we have a list of entry_ids to use.

Then use this sql for each entry:
```select entry from entry{mf} where id=?```

##list allternatives
```select * from (select cnt,orth sew,orth hews from headword{mf} where orth {ope} ? order by cnt,orth limit 500)```
````select * from (select cnt,orth sew,headwords hews from inflection{mf} where orth {ope} ? order by cnt,orth limit 500)```



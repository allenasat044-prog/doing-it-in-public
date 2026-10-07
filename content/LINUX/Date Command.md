[[LINUX/]]





```
%H → Hour (00–23)
%M → Minute (00–59)
%S → Second (00–59)
```

```
%Y → 4-digit year
%m → 2-digit month
%d → 2-digit day
```

```
number operations
date -j -v+NUMBERUNIT -f "INPUT_FORMAT" "DATE" "+OUTPUT_FORMAT"

~~examples~~ 
date -j -v+1d -f "%B %d %Y" "august 17  2026" "+%B %d %Y"

August 18 2026

#####################################################

```

```
Basic Format

%Y   Year       → 2026
%m   Month      → 07
%d   Day        → 17
%B   Full month → July
%b   Short month→ Jul
%A   Full day   → Friday
%a   Short day  → Fri
%H   Hour       → 14
%M   Minute     → 30
%S   Seconds    → 00
```



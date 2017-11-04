# Snippet: Script Loader

간단한 스크립트 로더를 작성해 봄.

```javascript
var include = (function(s, c, r, i, p, t) {
    return function(p, t) {
        r = s.createElement(c);
        i = s.getElementsByTagName(c)[0];
        r.src = p;
        if (t) r.async = t;
        i.parentNode.insertBefore(r, i);
    }
}(document, 'script'));
```

minified:

```javascript
var include=function(s,c,r,i,p,t){return function(p,t){r=s.createElement(c),i=s.getElementsByTagName(c)[0],r.src=p,t&&(r.async=t),i.parentNode.insertBefore(r,i)}}(document,'script')
```

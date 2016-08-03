# Snippet: Cookie

나는 난독화가 좋다.

```javascript
function cookie(n, v, e) { // name, value, expires
	var d,o,r,k,l,y;
	if (d = document, void 0 != v) {
		o = new Date;
		o.setDate(o.getDate() + e);
		d.cookie = n + '=' + escape(v) + ( (null == e) ? '' : ";expires=" + o.toGMTString() );
	} else {
		r = [], k = d.cookie.split(";");
		for (l = 0; l < k.length; l++)
			( y = k[l].match(RegExp("^\\s*" + n + "=\\s*(.*?)\\s*$")) ) && r.push(y[1]);
		return r[0] ? r[0] : !1;
	}
}
```

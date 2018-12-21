## 离线应用
### service workers

```js
if (navigator.serviceWorer) {
	window.addEventListener('DOMContentLoaded', function() {
		navigator.serviceWorer.register('/sw.js')
	})
}
```





[[Хуки]]

### !! Не использовать await внутри хука
```js
// правильное использование await вне хука
async function someAsync(){
  await fetch("...")
}
// ...
useEffect(() => {
	someAsync()
		.then(...)
}, [])
```
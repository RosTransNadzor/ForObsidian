[[FronteEnd/JS/React/React|React]]

### Пересоздает объект только при изменении зависимостей
```js
const handleScroll = useCallback(() => {
   console.log("some")
},[])
```
- [] - только в первый раз
- Нет массива - каждый рендер будет создаваться новая ссылка на функцию
- [numbers] - в перый раз и при изменении numbers
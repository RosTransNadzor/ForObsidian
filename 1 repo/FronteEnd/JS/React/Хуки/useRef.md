[[FronteEnd/JS/React/React|React]]

### Не пересоздает объект внутри при обновлении компонента
- Хранит ссылку на объект или DOM элемент
```jsx
// хранит ссылку на div
const ulRef = useRef()
...
return(
  <div ref={useRef}></div>
)
```
```jsx
const {numbers,setNumbers} = useState([1,2,3])
const numRef = useRef()
```
- useRef хранит сслыку в свойстве current


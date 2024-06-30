[[FronteEnd/JS/React/React|React]]

### Компонент ререндерится только при изменении props
```jsx
export default React.memo(
	// компонент с props
	function IsFive(props),
	//функция принимающая предыдущие и новые props
	// если возвращает true - компонент ререндерится
	//(необязательный парамерт)
	arePropsEqual?
)
```

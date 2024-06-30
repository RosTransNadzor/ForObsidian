- Из-за того что tailwind отображает классы некоректно в react компонентах для динамических классов следует использовать cn
```html
<div className={cn("text-blue-400",classNameVar)}></div>
```
- Она общая из пакетов tailwind merge и clls 
- В файле нужно создать это
```ts
import {twMerge} from 'tailwind-merge'
import {clsx, ClassValue} from 'clsx'

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(...inputs))
}
```
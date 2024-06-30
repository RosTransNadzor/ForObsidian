[[Tag helpers]]
- [[Csharp/Asp net core/Razor Pages/Маршрутизация/Маршрутизация]]
- [[Csharp/Asp net core/Razor Pages/Binding/Валидация]]
- Класс для формы здесь [[Код формы]]
```html
// aps-page - адрес отправления данных формы
// метод по умолчанию - post
<form asp-page="Index">
</form>
```
```html
// указание обработчика формы
<form asp-page-handler="edit" >
```
```html
// aps-for - указание значения для привязки
// label - производит имя из атрибута [Display("...")]
<label asp-for="Input.FirstName"></label>
// или чтобы label определять самому
<label asp-for="Email">Please enter your Email</label>

<input class="form-control" asp-for="Input.FirstName" />
```
```html
// asp-validation-for - при попытке отправки формы 
// если валидация не пройдена будет высвечено предупреждение
// из атрибута валидации
<span asp-validation-for="Input.LastName"></span>
//также список всех ошибок валидации
<div asp-validation-summary="All"></div>
```
```cs
// в обработчике
ModelState.AddModelError( string.Empty, "Cannot convert currency to itself");
//это соббщение также отразится в validation-summary
```
```html
// asp-route-* - где * имя параметра
// данный тэг используется для более динамических маршрутов
// product/5 - например
<form asp-page="Product" asp-route-id="@Model.ProductId"> 
```
- Соотношение  input и DataAnnotations
![[input.png]]


- элемент Select - для одного выбора используется string и IEnumerable 
для множественного выбора необходимо 2 IEnumerable
```cs
//здесь используется особый тип selecListItem
public IEnumerable Items { get; set; } = new List 
{ 
	new SelectListItem{Value= "csharp", Text="C#"}, 
	new SelectListItem{Value= "python", Text= "Python"}, 
	new SelectListItem{Value= "cpp", Text="C++"}, new SelectListItem{Value= "java", Text="Java"}, 
	new SelectListItem{Value= "js", Text="JavaScript"}, 
	new SelectListItem{Value= "ruby", Text="Ruby"}, 
};
```
```cs
public class InputModel 
{
	// для одного выбора
	public string SelectedValue1 { get; set; } 
	//для множественного
	public IEnumerable MultiValues { get; set; } 
}
```
html для выбора
```html
<select asp-for="Input.SelectedValue1"
 asp-items="Model.Items"> </select>
```
html для множественного выбора
```html
<select asp-for="Input.MultiValues"
 asp-items="Model.Items"></select>
```
группировка элементов в select
```cs
[BindProperty] 
public IEnumerable SelectedValues { get; set; } 
public IEnumerable Items { get; set; }
//для упрощения все делается в конструкторе
public SelectListsModel() 
{
    //определение групп
	var dynamic = new SelectListGroup { Name = "Dynamic" }; 
	var stat = new SelectListGroup { Name = "Static" }; 
	Items = new List 
	{
		new SelectListItem { Value= "js", Text="JavaScript", Group = dynamic }, 
		new SelectListItem { Value= "cpp", Text="C++", Group = stat }, 
		new SelectListItem { Value= "python", Text="Python", Group = dynamic }, 
		new SelectListItem { Value= "csharp", Text="C#", } 							};
}
```
при этом html не изменится
```html
<select asp-for="SelectedValues" asp-items="Model.Items"></select> 
```
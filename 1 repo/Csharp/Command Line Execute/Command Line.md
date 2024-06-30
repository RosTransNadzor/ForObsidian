[[Csharp]]

- Класс Proccesss - может запускать файлы (bat,exe,и тд)
- Cli Wrap (nuget) - более удобная библиотека для тех же действий
```cs
using CliWrap;

// записывает в файлы вывод исполняеиой программы
var writeMessageToFile = async (string message) =>
{
	using var sw = new StreamWriter("cli45exe.txt", true);
	await sw.WriteAsync(message);
};

// в конструкторе указываем исполняемый файл
await Cli.Wrap("C:\\Users\\Марина\\Desktop\\discordClone\\server\\ConsoleApp1\\45\\bin\\Debug\\net7.0\\45.exe")
	// указывает что для вывода нужно использовать делегат
	.WithStandardOutputPipe(PipeTarget.ToDelegate(writeMessageToFile))
	// выполняем асинхронно
	.ExecuteAsync();
```
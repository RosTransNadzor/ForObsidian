

- Библиотека Polly - реализация повторных попыток
- Полезная библиотека для аснхронности Nito.AsyncEx

- Завершенные задачи типа Task
	- Task.FromResult(T) - успешно завершенная с результатом
	- Task.FromException(Exception ex) - завершенная с определенным исключением
	- Task.FromCancelled(CancellationToken token) - возвращет отменненную задачу - если токен отменен
	  ```cs
		private static Task<int> Some(CancellationToken token)  
		{  
			// OperationCancelledException  
		    token.ThrowIfCancellationRequested();  
		    return Task.FromResult(5);  
		}  
		private static Task<int> Some2(CancellationToken token)  
		{  
		    if (token.IsCancellationRequested)  
		        // TaskCancelledException  
		        return Task.FromCanceled<int>(token);  
		    return Task.FromResult(5);  
		}
		```
- Сообщение о прогрессе выполнения задачи
	- Используйте типы IProgress и Progress
		```cs
		async Task MyMethodAsync(IProgress progress = null) 
		{ 
			bool done = false;
			double percentComplete = 0; 
			while (!done) { ... progress?.Report(percentComplete); } 
		} 
		 async Task CallMyMethodAsync() 
		 {
			var progress = new Progress();
			progress.ProgressChanged += (sender, args) => { ... };
			await MyMethodAsync(progress); 
		}
		```
	- <span style="color:#FFA5A5;font-weight:bold;">Progress</span> сохраняет текущий контекст при создании и активизирует свой обратный вызов в этом контексте. Это означает, что если Progress конструируется в UI-потоке, то вы сможете обновить пользовательский интерфейс из его обратного вызова, даже если асинхронный метод вызывает Report из фонового потока.
- Исключения в Task.WhenAll
	- Если нужно получить все исключения
		```cs
		async Task ObserveAllExceptionsAsync() { 
			var task1 = ThrowNotImplementedExceptionAsync(); 
			var task2 = ThrowInvalidOperationExceptionAsync(); 
			Task allTasks = Task.WhenAll(task1, task2); 
			try { await allTasks; } 
			catch { AggregateException allExceptions = allTasks.Exception; ... } 
		}
		```
- Исключения в Task.WhenAny
	- Возвращает первую завершенную задачу
	-  Исключение этой задачи выдается при ожидании 
		```cs
			Task<int> t1 = ThrowArgumentException();  
			Task<int> t2 = ThrowInvalidCastException(); 
			Task completedTask = await Task.WhenAny(t1, t2);  
			
			try  
			{  
			    await completedTask;  
			}  
			catch (Exception ex)  
			{  
				// исключение первой выполненной задачи
			    Console.WriteLine(ex.Message);  
			}
		```
	- При получении первой выполненной задачи лучше отменить все остальные,чтобы не расхоовать впустую ресурсы
- Обработка задач по завершению
	- Используя библиотеку Nito.AsyncEx (OrderByCompletion)
		```cs
		Task<int> t1 = Task.Run(async () =>  
		{  
		    await Task.Delay(1000);  
		    return 1;  
		});  
		Task<int> t2 = Task.Run(async () =>  
		{  
		    await Task.Delay(2000);  
		    return 2;  
		});  
		Task<int>[] tasks = [t2, t1];  
		
		foreach (var t in tasks.OrderByCompletion())  
		{  
		    var result = await t;  
		    Console.WriteLine(result);  
		}
		```
- Избежание продолжения в контексте 
	- По умолчанию async методы продолжают выполнение в сохранненом контексте (например UI) ,но  это очень часто не нужно и необходимо использовать ConfigureAwait(false) - должен использоваться во всех библиотечных методах где не нужен контекст
- Выдача исключений в Task
	- При содании Task исключение сохраняется в этом типе и не выбрасывается до применения await

<span style="color:HotPink;font-weight:bold;">Async void</span>

- Методы с async void 
	- Они не возвращают значение и Task в общем и также их нельзя ожидать
	- В итоге они выполняют только синхронную часть до вызова асинхнронного метода и возвращают управление потоку без ожидания окончания асинхронной операции
- Исключения в async void 
	- Из-за того что async void не возвращает Task, если и вызывать его ,то только на самом верхнем уровне приложения и оборачивать АБСОЛЮТНО весь код в try catch - потому что непонятно, когда выдастся исключение
- Другое решение
	- AsyncContext - SyncronizationContext из библиотеки Nito.AsyncEx - с помощью метода Run -> он запустит async void и будет ждать синхроннно
- Пример работы AsyncContext и любого контекста
	 - Всем внутренним вызовам  этот контекст передается - поэтому необходимо использовать ConfigureAwait 
		  ```cs
			public static void Main()  
			{  
			    AsyncContext.Run(Method);  
			}  
			  
			private static async Task Method()  
			{  
			    await Task.Delay(1000);  
			    // здесь продолжится в 1 потоке 
			    await Inner();  
			    // здесь продолжится в 1 потоке (т к есть SyncronizationContext)
			    Console.WriteLine(Thread.CurrentThread.ManagedThreadId);  
			}  
			  
			private static async Task Inner()  
			{  
			    await Task.Delay(100).ConfigureAwait(false);
			    // в потоке из пула тк configureAwait(false);  
			    Console.WriteLine(Thread.CurrentThread.ManagedThreadId);  
			}
		```

<span style="color:HotPink;font-weight:bold;">Value Task</span>

- Тип ValueTask
	- Структура, которая используется там где метод выполняется чаще синхронно, асинхронно
- Ограничения ValueTask
	- await на ней можно использовать  только 1 раз
	- Получить задачу можно с помощью метода AsTask() только один раз
	- Чтобы использовать в Task.WhenAll или Task.WhenAny - необходимо передать задачу - AsTask()
	- Нельзя в одной строчке использовать await и AsTask()
	- Ограничения связаны с тем, что ValueTask переиспоьзуются если не соблюдать ограничения, то можно получить другую задачу!!!
	- НИ В КОЕМ случае нельзя использовать .GetResult на ValueTask - она не всегда блокирует поток. Результирующий код имеет неопределенное поведение.
	- <span style="color:#7EFFDB;font-weight:bold;">При этом компилятор с этими ограничениями не поможет и код успешно скомпилируется и возможно нормально отработает,НО все-же эти ограничения вледует обязательно учитывать</span>
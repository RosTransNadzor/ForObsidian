[[Csharp/Asp net core/Background/Background Tasks|Background Tasks]]
- Создает фоновую задачу в приложении asp net core
- Пример
```cs
// наследование от  базового класса
public class Worker : BackgroundService
	{
		private readonly ILogger<Worker> _logger;

		public Worker(ILogger<Worker> logger)
		{
			_logger = logger;
		}
		// выполение фоновой задачи
		protected override async Task ExecuteAsync(CancellationToken stoppingToken)
		{
			int counter = 0;
			// до остановки приложения
			while (!stoppingToken.IsCancellationRequested)
			{
				_logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);
				// ожидание следущей итерации цикла
				await Task.Delay(1000, stoppingToken);
				if (counter == 3)
					throw new Exception("some exception");
				counter++;
			}
		}
		// вызывается при завершении приложения(освобождение ресурсов)
		public override async Task StopAsync(CancellationToken stopingToken)
		{
			_logger.LogWarning("application is stopped");
			await base.StopAsync(stopingToken);
		}
		// также лучше реализовать Dispose - если stopAsync - не успеет вызваться
	}
```
- Регистрация в DI (Singleton) 
```cs
services.AddHostedService<Worker>();
```
- Регистрируется как  singleton поэтому чтобы избежаь ошибок [[Решение проблемы жизненного цикла]]
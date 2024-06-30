[[Nest js]]

- Ответственны за принятие веб запросов
- <span style="color:yellow;">Принудительное использование express заставляет самостоятельно управлять запросом а не полагаться на nest (чтобы использовать express необходимо в методе-обработчике принять @Res() response: Response - Response из express )</span>
- Комбинация express и nest подхода(не вызывать res.send() - ошибка,лучше делать через возврат return)
- Express имеет приоритет над nest(например если они записывают 2 одинаковых заголовка)
```js
// passthrough - true - позволяет применять 2 подхода
@Get()
findAll(@Res({ passthrough: true }) res: Response) {
  res.status(HttpStatus.OK);
  return [];
}
```
- Простенький контроллер
```ts
import { Controller, Get } from '@nestjs/common';

// указывает путь /cats
@Controller('cats')
export class CatsController {
  // /cats/dot
  @Get("dot")
  findAll(): string {
    return 'This action returns all cats';
  }
}
```
- Возвращение return типа array or object - сериализует их 
- Просто возвращение дает 200 код или 201 если метод post
- Определение кода ответа
```js
@Post()
@HttpCode(204)
create() {
  return 'This action adds a new cat';
}
```
- Коды ошибки без express
```js
throw new BadRequestException(error);
throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
```
- Несколько кодов ответа с <span style="color:yellow">использованием express</span>
```js
import { Post, Res, HttpStatus } from '@nestjs/common';
import { Response } from 'express';
...
  @Post()
  save(@Res() response: Response) {
    response
      .status(HttpStatus.BAD_REQUEST)
      .send("saving " + JSON.stringify(body));
  }
```

- Получение запроса (body,headers,...)
```js
// body
@Get()
  getHello(@Body() body:Body): string {
    return this.appService.getHello();
  }
  // request - весь запрос
  @Get("findAll")
  findAll(@Req() request: Request): string {
    console.log(request.body)
    return 'This action returns all cats';
  }
// заголовки
  @Get("findOne")
  findOne({headers}: { headers: Headers }): string {
    console.log(headers)
    return 'This action returns all cats';
  }
```
- Метод Post
```js
@Post()
  create(): string {
    return 'This action adds a new cat';
  }
```
- Добавление заголовков
```js
@Post()
@Header('Cache-Control', 'none')
create() {
  return 'This action adds a new cat';
}
```
- Перенаправление
```js
// url - url контроллера или внешнего сервиса
@Redirect(url)
```
- Параметры запроса (есть проблема типов)
```js
// запрос /5/afdsasd - например
@Get(":id/:name")
  getHello(@Param("id") id:number,@Param("name") name:string ){
    console.log(id / 3)
    console.log(name)
  }
// при этом ограничений нет и вместо id number - может прийти строка 
// например /dsa/dafds
```
- Проверка хоста - контроллер только для определенного хоста
```js
@Controller({ host: 'admin.example.com' })
export class AdminController {
  @Get()
  index(): string {
    return 'Admin page';
  }
}
```
- Поддержка observable и async
```js
@Get()
findAll(): Observable<any[]> {
  return of([]);
}
@Get()
async findAll(): Promise<any[]> {
  return [];
}
```
- Тело Body
```js
// опять же nest - не проверяет body - только путь запроса и dto может прийти undefined
@Get("/create")
  getHello(@Body() dto:catCreateDto){
    console.log(dto.age)
  }
```
- В контроллере при запросах сохраняютя данные заключеные в полях
```js
@Controller()
export class AppController {
// этот массив будет изменятся при каждом запросе а не пересоздаваться
  public numbers:number[] = [1,2,3]
  constructor(private readonly appService: AppService) {}
  @Post("/create")
  getHello(@Body() dto:catCreateDto,@Req() request:Request){
    console.log(dto.age)
    this.numbers.push(dto.age)
    return request.body;
  }
  @Get("/all")
  getAll():number[]{
    return this.numbers;
  }
}
```
- Для регистрации контроллера его необходимо добавить в app.module
```js
//app.module.ts
import { Module } from '@nestjs/common';
// так же его надо импортировать
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController,SomeController],
  providers: [AppService],
})
export class AppModule {}

```
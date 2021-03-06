# Запускайте небезопасный код в песочнице

### Объяснение в один абзац

Как правило, нужно запускать только собственные файлы JavaScript. Помимо теорий, в реальных сценариях требуется выполнять файлы JavaScript, которые передаются динамически во время выполнения. Например, рассмотрим динамический фреймворк, такой как веб-пакет, который принимает пользовательские загрузчики и выполняет их динамически во время сборки. При существовании какого-либо вредоносного плагина мы хотим минимизировать ущерб и, возможно, даже позволить потоку успешно завершиться - для этого необходимо запустить плагины в среде изолированной программной среды, которая полностью изолирована с точки зрения ресурсов, сбоев и информации, которой мы делимся с ней. Три основных варианта могут помочь в достижении этой изоляции:

- выделенный дочерний процесс - это обеспечивает быструю изоляцию информации, но требует приручить дочерний процесс, ограничить время его выполнения и устранить ошибки
- облачная безсерверная инфраструктура отвечает всем требованиям песочницы, но развертывание и динамический вызов функции FaaS - это не прогулка в парке
- некоторые библиотеки npm, такие как [sandbox](https://www.npmjs.com/package/sandbox) и [vm2](https://www.npmjs.com/package/vm2) позволяют выполнять изолированный код в 1 одна строка кода. Хотя этот последний вариант выигрывает в простоте, он обеспечивает ограниченную защиту

### Пример кода - Использование библиотеки Sandbox для изолированного выполнения кода

```javascript
const Sandbox = require('sandbox');
const s = new Sandbox();

s.run('lol)hai', (output) => {
  console.log(output);
  //output='Syntax error'
});

// Example 4 - Restricted code
s.run('process.platform', (output) => {
  console.log(output);
  //output=Null
});

// Example 5 - Infinite loop
s.run('while (true) {}', (output) => {
  console.log(output);
  //output='Timeout'
});
```

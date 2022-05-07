# Smart Contracts with Solidity

## IDE
[Remix online](http://remix.ethereum.com/)  
[Remix desktop](https://remix-project.org/)

## Simplest solidity code example
```js
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract MyShop {
    address public owner; // декларация переменной типа address
    mapping(address => uint) public payments; // декларация key-value map 

    constructor() { // декларация конструктора
        owner = msg.sender; // msg.sender - тот кто вызвал функцию
    }

    function payForItem() public payable { // декларация функции, payable означает что в функции производится оплата
        // в теории payable функцию можно оставить пустой, 
        // при вызове этой функции пользователь все равно введет средства
        // и они зачислятся на определенный счет ассоциированный с данным контрактом
        // зачисления и списания производятся автоматически
        //  
        payments[msg.sender] = msg.value; // msg.value - сумма которую ввел отправитель
    }

    function withdrawAll() public {
        address payable _to = payable(owner); // приведение типа owner к payable
        address _thisContract = address(this); // получение адреса текущего контракта
        _to.transfer(_thisContract.balance);
    }
}
```

## Solidity data types
!!! В солидити нет концепции андейфайнд, налл, нилл и так далее  

Есть несколько типов переменных solidity
- переменные состояния (state variables) (декларация полей контракта) (может требоваться модификатор storage) - хранится в блокчейн, хранится до тех пор пока блокчейн впринципе существует
- локальные переменные (local variables) (декларируются внутри функций, как параметры функций) (может требоваться модификатор memory) - хранятся в памяти. 
  
[Link](https://docs.soliditylang.org/en/v0.8.12/types.html)

**bool**
Тоже что и в других языках. Операторы дискретной логики теже

**Числа**
На данный момент с дробными числами в solidity не срослось - ожидаем фиксы в новых версиях.  
Соотвественно сейчас имеет смысл говорить про целые числа.    
Собсветнно есть signed(int) и unsigned(uint) integers  
У чисел надо учитывать размерность - см документацию uint256 и проч.  
TIP - если достигается переполнение - то транзакция не проходит. Однако можно операцию с числом обернуть в конструкцию
unchecked - и в этом случае - в случае переполнения - каунтер начнет работать заново
  
**Строки**  
Ничего особенного кроме того что не стоит сохранять большие объемы строк в state variable 
Хранятся как байтовые массивы
У строк нет понятия длины  
Конактинации строк нет  
Сравнивать тоже особо не льзя - надо искать стороннее решенее  
По индексу - тоже обращаться - нельзя  
  
**Adress**
```js
// пример
address public myAdddr = 0x1jkfldjklafjfkldsajklf;
// свойства адреса:
//баланс
addr.balance
//Перевод. для перевода адресс должен быть помечен как payable.
//Если у нас адресс не payable - его можно сделать таковым через приведение типов: payable(address)
addr.transfer(addressTo)
```

**Mapping**
Аналог Map<K,V> в Java
```js
// декларация
mapping (adress => uint) public payments; 
// добавление
payments[key] = value;
```
При работе следует учесть что попытка запросить несуществующий адрес приведет к возврату значения по умолчанию у типа значения.  
У маппингов как и устрок нет понятия длины
  
  
**Enum**
Однако - это тоже структура данных.
```js
// пример
enum Status { Paid, Delivered, Received }
Status public currentStatus;
```
При инициализации ставится значение по умолчанию - равным первым значением из списка. В код снипе выше - это будет значение Paid.


**Array**
Массивы строго типизированы.  
Могут быть как с фиксированной длиной
```js
uint[10] public items; 
// инлайн инициализация
uint[10] public items = [1,2,3]
// присвоение
items[0] = 100;
items[1] = 200;
```
Так и с динамической длиной;
```js
uint[] public items;
// запись сюда
items.push(3);
items.push(6);
items.length;
```

Также могут быть вложенные массивы
```js
uint[10][5] public items;
// тут разработчики извернулись и переставили местами размерности внутреннего и внешнего массива.
// то есть:
uint[3,2] public items = [
    [1,2,3],
    [4,5,6]
]
```

Декларация массива в памяти несколько отличается
```js
uint[] memory tempArray = new uint[](10);
```

**Байтовые массивы**
```js
// 8 байт
bytes8 public data;
// динамический массив
bytes public data;
```
К байтовым массивам вполне можно присваивать строки.


**Структура данных - Struct**  
По факту это что-то типа Data класса
```js
struct Payment {
    uint amount;
    uint timestamp;
    adress from;
}
```
Однако внутри структуры запрещена ссылка самой на себя. В нашем примере структура Payment не может содержать Payment  













   




## Truffle
**[Truffle](https://trufflesuite.com/docs/truffle/)** - is one kind of one stop shop for development of theory in contract.  
Truffle Contains instruments for:
- contract creation
- local testing
- development

## Links
[Origin video lessons](https://www.youtube.com/watch?v=8A8-7Ks26yY&list=PLWlFXymvoaJ92awHVDO0oSy0z0ZFJifDV&index=8)
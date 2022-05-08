## События, модификаторы, require/revert

### require/revert/assert

Конструкции require/revert/assert - нужные для проверки некоторого условия, и если это условие не выполняется,
то выполняемая транзакция должна быть отменена и, соответственно, выполняемая функция не должна быть выполнена.
Например мы можем ввести концепцию владельца и соотвественно указать что конкретную данную функцию может выполнять только этот владелец.
  
  
**Пример**:
```js
contact Demo {
    address owner;
    
    constructor() {
        this.owner = msg.sender;
    } 
    
    function pay() external payable {
        
    }
    
    function withdraw(address _to) external {
        require(msg.sender == owner, "You are not an owner"); // если не тру - то выбрасывается 
        revert("You are not an owner")// также как require отктывает функцию - но условие надо написать самостоятельно
        assert(msg.sender == owner) // эту функцию используют реже. функция принимает только
        // условное выражение и нельзя указать сообщение. Породит ошибку - panic!
        _to.transfet(address(this).balance);
    } 
}
```

### Собственный модификатор

В солилиди есть такая концепция как собственный модификатор 

**Пример**:
```js
contact Demo {
    address owner;
    
    function pay() external payable {}
    
    modifier onlyOwner() { // также может иметь и параметры соотвествующие функции к которой присваивается 
        require(msg.sender == owner, "You are not an owner");
        require(...) // и еще проверка
        _;
        require(...) // проверки могут быть и после
    }

    function withdraw(address _to) external onlyOwner {
        _to.transfet(address(this).balance);
    }
```


### События

События нужны для того чтобы сообщить внешнему миру то что у нас что-то произошло в контракте.
Нарпимер можно создать событие которое будет сообщать о том что контракту были начислены денежные средства.
```js
contact Demo {
    address owner;
    
    event Paid(address _from, uint _amount, uint timestamp);
    
    function pay() external payable {
        emit Paid(msg.sender, msg.value, block.timestamp); // так выбрасывается событие
    }
}
```

при вызове emit Paid(msg.sender, msg.value, block.timestamp); в специальный журнал событий который хранится вместе с блокчейном
будет записано вот это замечательное событие со всей информацией.   
(hack - журналы событией можно использовать для дешевого хранения информации)  
(hack2 - можно написать фронтэнд подписавшись на события - так сказать добавляется динамика клиентским приложениям)  
Есть интересная особенность - внутри смарт котракта нельзы прочитать события из журнала событий.

**Индексация в журнале событий**
Поля в событиях можно поменять как индексируемые:
```js
event Paid(address indexed _from, uint _amount, uint timestamp);
```
Это дает возможность организовывать по ним поиск






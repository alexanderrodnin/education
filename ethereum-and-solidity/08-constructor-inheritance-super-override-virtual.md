## Constructor inheritance super override virtual

### Пример
```js
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract Ownable {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(owner == msg.sender, "not an owner!");
        _;
    }

    function withdraw(address payable _to) public virtual onlyOwner {
        payable(owner).transfer(address(this).balance);
    }
}
```
```js 
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import "./Ownable.sol";

abstract contract Balances is Ownable {
    function getBalance() public view onlyOwner returns(uint) {
        return address(this).balance;
    }

    function withdraw(address payable _to) public override virtual onlyOwner {
        _to.transfer(getBalance());
    }
}

contract MyContract is Ownable, Balances {
    constructor(address _owner) {
        owner = _owner;
    }

    function withdraw(address payable _to) public override(Ownable, Balances) onlyOwner {
        //Balances.withdraw(_to);
        //Ownable.withdraw(_to);
        require(_to != address(0), "zero addr");
        super.withdraw(_to);
    }
}
```

### Примечания
- solidity поддерживает множественное наследование контрактов  
  например:
  ```js
  contract Ownable { }
    
  contarct Balances is Ownable {}
  
  // при указании контрактов следует их прописывать от более общего к более частному
  contract MyContract is Ownable, Balances {}  
  ```
- наследование и использование super() - аналогично java  
  в декларации наследования можно указывать параметры для конструктора.  
  например:
  ```js
  contract Ownable { }
    
  abstract contarct Balances is Ownable {}
  
  contract MyContract is Ownable(0x123ewq321das2), Balances {}  
  
  // а можно еще и так:
  contract MyContract is Ownable, Balances {
    constructor(address owner) Ownable(owner) {}
  }
  ```
- модификаторы virtual и override - реализованы также как и в C#

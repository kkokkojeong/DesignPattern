### 디자인 패턴 소개
---

모든 패턴은 '시스템의 일부분을 다른 부분과 독립적으로 변화 시킬 수 있는' 방법을 제공
- 애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분으로부터 분리시킨다.
- 구현이 아닌 인터페이스에 맞춰서 프로그래밍한다.

#### 스트래티지 패턴(strategy pattern)
알고리즘군을 정의하고 각각을 캡슐화하여 교환해서 사용할 수 있도록 만든다. 

```javascript
//
// FlyBehavior
//
interface FlyBehavior {
  fly: Function;
}

class FlyWithWings implements FlyBehavior {
  fly = function () {
    console.log('날고 있다');
  };

  constructor() {
  }
}

class FlyNoway implements FlyBehavior {
  fly = function () {
    console.log('못 날다');
  };

  constructor() {
  }
}

class FlyRocketPowered implements FlyBehavior {
  fly = function () {
    console.log('로켓 추진으로 날다')

  };

  constructor() {
  }
}

//
// QuackBehavior
//
interface QuackBehavior {
  quack: Function;
}

class Quack implements QuackBehavior {
  quack = function () {
    console.log('꽥');
  };

  constructor() {
  }
}

class MuteQuack implements QuackBehavior {
  quack = function () {
    console.log('조용');
  };

  constructor() {
  }
}

class Squeak implements QuackBehavior {
  quack = function () {
    console.log('삑');
  };

  constructor() {
  }
}

//
// Duck
//
abstract class Duck {

  protected flyBehavior: FlyBehavior;
  protected quackBehavior: QuackBehavior;

  constructor() {
  }

  public performFly(): void {
    this.flyBehavior.fly();
  }

  public performQuack(): void {
    this.quackBehavior.quack();
  }

  public swim(): void {
    console.log('모든 오리는 물에...');
  }

  public setFlyBehavior(fb: FlyBehavior): void {
    this.flyBehavior = fb;
  }

  public getFlyBehavior(): FlyBehavior {
    return this.flyBehavior;
  }

  public setQuackBehavior(qb: QuackBehavior): void {
    this.quackBehavior = qb;
  }

  public getQuackBehavior(): QuackBehavior {
    return this.quackBehavior;
  }

  public abstract display(): void;
}

class MallardDuck extends Duck {
  constructor() {
    super();
    this.quackBehavior = new Quack();
    this.flyBehavior = new FlyWithWings();
  }

  public display(): void {
    console.log('저는 물오리입니다.');
  }
}

class ModelDuck extends Duck {
  constructor() {
    super();
    this.quackBehavior = new Quack();
    this.flyBehavior = new FlyNoway();
  }

  public display(): void {
    console.log('저는 모형오리입니다.');
  }
}


//
//  execute (p57)
//
const mallard: MallardDuck = new MallardDuck();
mallard.performQuack();
mallard.performFly();


//
//  execute (p59)
//
const model: ModelDuck = new ModelDuck();
model.performFly();
model.setFlyBehavior(new FlyRocketPowered());
model.performFly();
```

##### 디자인 원칙 : 상속보다는 구성(composition)을 활용

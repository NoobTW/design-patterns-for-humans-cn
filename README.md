![Design Patterns For Humans](https://cloud.githubusercontent.com/assets/11269635/23065273/1b7e5938-f515-11e6-8dd3-d0d58de6bb9a.png)


***
<p align="center">
🎉  设计模式的超简化描述! 🎉
</p>
<p align="center">
A topic that can easily make anyone's mind wobble. Here I try to make them stick in to your mind (and maybe mine) by explaining them in the <i>simplest</i> way possible. 
</p>
***

说明
====

This is the Simplified Chinese translation of [design-patterns-for-humans](https://github.com/kamranahmedse/design-patterns-for-humans). Thank [Kamran Ahmed](https://github.com/kamranahmedse) for his great work!

[design-patterns-for-humans](https://github.com/kamranahmedse/design-patterns-for-humans) 是由 [Kamran Ahmed](https://github.com/kamranahmedse) 发起和维护的项目，项目以简洁的语言对各种设计模式进行了描述和整理。

本项目是 [design-patterns-for-humans](https://github.com/kamranahmedse/design-patterns-for-humans) 项目的简体中文翻译版本。也欢迎您一起参与翻译和校审。

本项目的进度和贡献者将在文末列出。

当前翻译的原文版本是 [b9a53a8](https://github.com/kamranahmedse/design-patterns-for-humans/commit/b9a53a87a4026cb0b8e76feba72a91ecfcdf4f45)


🚀  简介
=================

设计模式是经常性问题的解决方案; **是如何解决特定问题的指导方针**。它们不是类、包或库，只要加到程序中就能起作用。它们是关于如何在特定的情况下解决特定问题的指导方针。

> 设计模式是经常性问题的解决方案; 是如何解决特定问题的指导方针。

Wikipedia 上描述为

> 在软件工程中，软件设计模式是在软件设计的给定上下文中，针对普遍问题的一种通用且可重用的解决方案。它不是一个已完成的设计，不能直接转化成源码或机器码。它是就如何解决某个问题，且能用于许多不同情况的一种描述或模板。

⚠️ 注意
-----------------
- 设计模式不是能解决所有问题的银弹。
- 不要强迫使用: 如果这样，可能会出问题。请记住设计模式是 **解决** 问题的方案，不是 **寻找** 问题的方案; 因此不要考虑过头了。
- 如果在适合的地方以正确的方式使用，它们会很有效; 否则它们可能会导致你的代码极度混乱。

> 同时注意下面的代码示例都是用 PHP-7 写的。但这问题也不会很大，因为思想都是一样的。另外 **支持其它语言的工作正在进行中**。

设计模式的类型
-----------------

* [创建型](#创建型设计模式)
* [结构型](#结构型设计模式)
* [行为型](#行为型设计模式)

创建型设计模式
==========================

简单来说
> 创建型模式关注于如何实例化一个或一组相关对象。

Wikipedia 上描述为
> 在软件工程中，创建型设计模式是处理对象创建机制，设法以适合当前情况的方式来创建对象的设计模式。对象创建时若使用一般形式可能会导致设计难题或增加设计的复杂度。创建型设计模式通过对对象创建过程的控制以解决此问题。
 
 * [简单工厂(Simple Factory)](#-简单工厂simple-factory)
 * [工厂方法(Factory Method)](#-工厂方法factory-method)
 * [抽象工厂(Abstract Factory)](#-抽象工厂abstract-factory)
 * [建造者(Builder)](#-建造者builder)
 * [原型(Prototype)](#-原型prototype)
 * [单例(Singleton)](#-单例singleton)
 
🏠 简单工厂(Simple Factory)
--------------
现实案例
> 假设你正在建房，需要用到门。如果每次需要门时，你都穿上木匠服在房子里亲自制作，肯定会导致一团糟。这种情况下你需要将门放在工厂里制作。

简单来说
> 简单工厂模式对客户隐藏了所有的实例化逻辑，只简单地为客户创建实例。

Wikipedia 上描述为
> 在面向对象编程 (OOP) 中，工厂就是一个用于创建其它对象的对象,  – 形式上它可以是一个函数或方法，它在被方法调用时（假设通过 "new"）会返回不同原型或类的对象。

**编程示例**

首先定义门的接口及其实现

```php
interface Door {
    public function getWidth() : float;
    public function getHeight() : float;
}

class WoodenDoor implements Door {
    protected $width;
    protected $height;

    public function __construct(float $width, float $height) {
        $this->width = $width;
        $this->height = $height;
    }
    
    public function getWidth() : float {
        return $this->width;
    }
    
    public function getHeight() : float {
        return $this->height;
    }
}
```

然后定义门的工厂，它创建并返回门实例

```php
class DoorFactory {
   public static function makeDoor($width, $height) : Door {
       return new WoodenDoor($width, $height);
   }
}
```

再这样使用

```php
$door = DoorFactory::makeDoor(100, 200);
echo 'Width: ' . $door->getWidth();
echo 'Height: ' . $door->getHeight();
```

**何时用？**

当创建对象不仅只是一些赋值操作，还涉及一些逻辑操作时，就适合将这些逻辑放到一个专门的工厂中，从而能避免代码重复。

🏭 工厂方法(Factory Method)
--------------

现实案例
> 考虑人事招聘经理的情况。一个人不可能参与对每个职位的面试。根据职位空缺，她必须决定将面试工作委派给不同的人来完成。

简单来说
> 它提供了一种能将实例化逻辑委派到子类中完成的方式。

Wikipedia 上描述为
> 在基于类的编程中，工厂方法模式是一种创建型模式，它无需指定将要创造的对象的具体类，只使用工厂中的各种方法就能处理对象创建的问题。对象的创建是通过调用工厂方法而非构造器来完成的，工厂方法—要么在接口中定义然后由子类实现，要么是在基类中实现然后被继承类重载。
 
 **编程示例**
 
继续上面的人事招聘经理的例子。首先定义面试接口并给出了几个实现

```php
interface Interviewer {
    public function askQuestions();
}

class Developer implements Interviewer {
    public function askQuestions() {
        echo 'Asking about design patterns!';
    }
}

class CommunityExecutive implements Interviewer {
    public function askQuestions() {
        echo 'Asking about community building';
    }
}
```

现在让我们创建 `HiringManager`

```php
abstract class HiringManager {
    
    // Factory method
    abstract public function makeInterviewer() : Interviewer;
    
    public function takeInterview() {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}
```

现在任何子类都可以扩展并提供所需的面试接口

```php
class DevelopmentManager extends HiringManager {
    public function makeInterviewer() : Interviewer {
        return new Developer();
    }
}

class MarketingManager extends HiringManager {
    public function makeInterviewer() : Interviewer {
        return new CommunityExecutive();
    }
}
```

然后可以这样使用

```php
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // Output: Asking about design patterns

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // Output: Asking about community building.
```

**何时使用？**

适合时当类中存在一些通用操作，但是所需的子类是在运行时才动态决定的情况。换句话说，即当客户无法知道所需的确切子类时。

🔨 抽象工厂(Abstract Factory)
----------------

现实案例
> 继续简单工厂模式中门的例子。基于你的需求，你可能要从木门店获取木门，从铁门店获取铁门，或者从 PVC 相关店获取 PVC 门。另外你可能还要找不同专长的人来安装门，例如找木匠来安装木门，找电焊工来安装铁门等等。可以看到现在门已经有了依赖性，比如木门依赖于木匠，铁门依赖于电焊工等。

简单来说
> 就是工厂的工厂; 该工厂将各个相关/相依赖的工厂组合起来，而无需指定他们具体的类。

Wikipedia 上描述为
> 抽象工厂模式提供了一种将具有相同风格的一组工厂封闭起来的方法，而无需指定各工厂具体的类。

**编程示例**

修改上面门的例子。首先定义 `Door` 接口并做出几个实现

```php
interface Door {
    public function getDescription();
}

class WoodenDoor implements Door {
    public function getDescription() {
        echo 'I am a wooden door';
    }
}

class IronDoor implements Door {
    public function getDescription() {
        echo 'I am an iron door';
    }
}
```

然后为每种门都定义相应的安装人员

```php
interface DoorFittingExpert {
    public function getDescription();
}

class Welder implements DoorFittingExpert {
    public function getDescription() {
        echo 'I can only fit iron doors';
    }
}

class Carpenter implements DoorFittingExpert {
    public function getDescription() {
        echo 'I can only fit wooden doors';
    }
}
```

现在定义我们的抽象工厂，它能为我们创建相关的一组对象，例如木门工厂将会创建木门及木门安装人员对象，而铁门工厂将会创建铁门及铁门安装人员对象。

```php
interface DoorFactory {
    public function makeDoor() : Door;
    public function makeFittingExpert() : DoorFittingExpert;
}

// 木门工厂将返回木匠及木门对象
class WoodenDoorFactory implements DoorFactory {
    public function makeDoor() : Door {
        return new WoodenDoor();
    }

    public function makeFittingExpert() : DoorFittingExpert{
        return new Carpenter();
    }
}

// 铁门工厂将返回铁门及相应的安装人员
class IronDoorFactory implements DoorFactory {
    public function makeDoor() : Door {
        return new IronDoor();
    }

    public function makeFittingExpert() : DoorFittingExpert{
        return new Welder();
    }
}
```

然后可以这样使用

```php
$woodenFactory = new WoodenDoorFactory();

$door = $woodenFactory->makeDoor();
$expert = $woodenFactory->makeFittingExpert();

$door->getDescription();  // Output: I am a wooden door
$expert->getDescription(); // Output: I can only fit wooden doors

// Same for Iron Factory
$ironFactory = new IronDoorFactory();

$door = $ironFactory->makeDoor();
$expert = $ironFactory->makeFittingExpert();

$door->getDescription();  // Output: I am an iron door
$expert->getDescription(); // Output: I can only fit iron doors
```

可以看到木门工厂已经封装了 `木匠` 和 `木门` 而铁门工厂已经封闭了 `铁门` 和 `电焊工`。这样它就能确保，每次创建了一个门对象后，我们也可以得到其相应的安装人员对象。

**何时使用？**

当创建逻辑有点复杂但内部又相互关联时使用。

👷 创造者(Builder)
--------------------------------------------

现实案例
> 假设你在 Harees(美国连锁快餐店)，你下了单，假定说要来份 "大份装"，然后店员 *无需再多问* 就直接为你送上 "大份装"; 像这样的就是简单工厂模式的例子。但是有些情况下创建逻辑可能要涉及多个步骤。例如你想要一份定制餐，你给出了如何做汉堡的具体要求，例如使用什么面包，使用何种酱汁，何种奶酪等。那么这种情况下就需要使用建造者模式。

简单来说
> 它允许你创建 ”不同口味" 的对象，同时又能避免 “污染” 构造函数的参数。适合当某对象可能会有多种 “口味"，或者对象的创建过程涉及多个步骤时使用。
 
Wikipedia 上描述为
> 建造者模式是一种对象创建的软件设计模式，它意在为重叠构造器这种反模式(telescoping constructor anti-pattern)找到一种解决方案。

既然说到了，那让我多说几句什么是重叠构造器反模式(telescoping constructor anti-pattern)。我们或多或少有看到过像这样的构造函数：
 
```php
public function __construct($size, $cheese = true, $pepperoni = true, $tomato = false, $lettuce = true) {
}
```

可以看到; 构造函数的参数个数很快会变得一发不可收拾，从而要理解参数布局会变得困难。另外假如以后还要添加更多功能的话，该参数列表还会继续增长。这就是所谓的重叠构造器反模式(telescoping constructor anti-pattern)。

**编程示例**

理智地选择是使用建造者模式。首先定义我们需要制作的汉堡类

```php
class Burger {
    protected $size;

    protected $cheese = false;
    protected $pepperoni = false;
    protected $lettuce = false;
    protected $tomato = false;
    
    public function __construct(BurgerBuilder $builder) {
        $this->size = $builder->size;
        $this->cheese = $builder->cheese;
        $this->pepperoni = $builder->pepperoni;
        $this->lettuce = $builder->lettuce;
        $this->tomato = $builder->tomato;
    }
}
```

然后定义建造者类

```php
class BurgerBuilder {
    public $size;

    public $cheese = false;
    public $pepperoni = false;
    public $lettuce = false;
    public $tomato = false;

    public function __construct(int $size) {
        $this->size = $size;
    }
    
    public function addPepperoni() {
        $this->pepperoni = true;
        return $this;
    }
    
    public function addLettuce() {
        $this->lettuce = true;
        return $this;
    }
    
    public function addCheese() {
        $this->cheese = true;
        return $this;
    }
    
    public function addTomato() {
        $this->tomato = true;
        return $this;
    }
    
    public function build() : Burger {
        return new Burger($this);
    }
}
```

然后可以这样使用:

```php
$burger = (new BurgerBuilder(14))
                    ->addPepperoni()
                    ->addLettuce()
                    ->addTomato()
                    ->build();
```

**何时使用？**

当某个对象可能会有多种 "口味"，或者想避免重叠构造器反模式(telescoping constructor anti-pattern) 时使用。它与工厂模式的主要区别在于：工厂模式适用于创建过程只有一个步骤的情况，而建造者模式适用于创建过程涉及多个步骤的情况。

🐑 原型(Prototype)
------------

现实案例
> 还记得多莉吗？那只克隆羊！我们先不要关注细节，但是这里的重点是克隆。

简单来说
> 根据某个现存的对象，通过克隆来创建对象。

Wikipedia 上描述为
> 原型模式是软件开发中的创建型设计模式。它用于当所需创建的对象的类型是由某个原型实例决定的情况，并通过克隆该原型实例来产生新的对象。

简单来说，它能让你创建某个现有对象的克隆版本，然后你可按需对其进行修改，从而避免了从新创建一个对象并对其进行设置的所有麻烦。

**编程示例**

在 PHP 中, 可以非常容易地使用 `clone` 实现
 
```php
class Sheep {
    protected $name;
    protected $category;

    public function __construct(string $name, string $category = 'Mountain Sheep') {
        $this->name = $name;
        $this->category = $category;
    }
    
    public function setName(string $name) {
        $this->name = $name;
    }

    public function getName() {
        return $this->name;
    }

    public function setCategory(string $category) {
        $this->category = $category;
    }

    public function getCategory() {
        return $this->category;
    }
}
```

然后像下面这样进行克隆

```php
$original = new Sheep('Jolly');
echo $original->getName(); // Jolly
echo $original->getCategory(); // Mountain Sheep

// Clone and modify what is required
$cloned = clone $original;
$cloned->setName('Dolly');
echo $cloned->getName(); // Dolly
echo $cloned->getCategory(); // Mountain sheep
```

另外你也可以通过特殊方法 `__clone` 来定制克隆行为。

**何时使用？**

当所需对象和某个现存对象非常相似时，或者当创建操作相比克隆花销更大时。

💍 单例(Singleton)
------------

现实案例
> 一个国家在同一时期只能有一位总统。当需要担起责任时，都是这位总统实施行动的。这里总统就是单例。

简单来说
> 它能确保某个类永远只能够创建一个对象。

Wikipedia 上描述为
> 在软件工程中，单例模式是一种软件设计模式，它限制某个类只能实例化成一个对象。当系统中需要确切一个对象来协调行为时，单例是很适合的。

单例模式实际上被认为是一种反模式，因此需避免过度使用。它不一定就是不好的，它有它的适用情况，但是使用时应当当心，因为它在你的程序中引用了一个全局状态，因此在某处对它的修改可能会影响其它地方，从而对它进行调试会变得相当困难。

**编程示例**

创建一个单例，将构造器设为私有，禁用克隆功能，禁止扩展，并创建一个静态变量来保存实例

```php
final class President {
    private static $instance;

    private function __construct() {
        // Hide the constructor
    }
    
    public static function getInstance() : President {
        if (!self::$instance) {
            self::$instance = new self();
        }
        
        return self::$instance;
    }
    
    private function __clone() {
        // Disable cloning
    }
    
    private function __wakeup() {
        // Disable unserialize
    }
}
```

然后这样使用

```php
$president1 = President::getInstance();
$president2 = President::getInstance();

var_dump($president1 === $president2); // true
```

结构型设计模式
==========================

简单来说
> 结构型模式主要关注对象的组合或者换句话说是实体间如何能够相互使用。或者也可以另外解释为，它们有助于回答 "如何构建一个软件组件?“。

Wikipedia 上描述为
> 在软件工程中，结构型设计模式是这样的一些设计模式，它们通过某种简明的方式来实现实体间的关系，从而减少设计的难度。
 
 * [适配器(Adapter)](#-适配器adapter)
 * [桥接(Bridge)](#-桥接bridge)
 * [组合(Composite)](#-组合composite)
 * [装饰器(Decorator)](#-装饰器decorator)
 * [外观(Facade)](#-外观facade)
 * [享元(Flyweight)](#-享元flyweight)
 * [代理(Proxy)](#-代理proxy)

🔌 适配器(Adapter)
-------

现实案例
> 假设你的内存卡里有一些照片，你需要将它们传到电脑上。要完成传输，你需要有与你的电脑接口兼容的适配器，这样你才能将内存卡与你的电脑连接。在这种情况下读卡器就是一个适配器。
> 另一个例子就是大家都知道的电源适配器; 一个三脚插头无法插到两口的插座上，需要使用一个电源适配器才能将它与两口插座连接。
> 还有一个例子就是翻译，他能将一个人说的话翻译给另一个听。

简单来说
> 适配器模式允许你在适配器中封装其它不兼容的对象，从而使它们与某些类兼容。

Wikipedia 上描述为
> 在软件工程中，适配器这种软件设计模式允许将现有类的接口转成另一种接口来使用。它通常用于使现有类在无需修改其源码的情况下，与其它类实现协作。

**编程示例**

假设现有一款关于猎人猎狮的游戏。

首先定义 `Lion` 接口并实现所有种类的狮子类

```php
interface Lion {
    public function roar();
}

class AfricanLion implements Lion {
    public function roar() {}
}

class AsianLion implements Lion {
    public function roar() {}
}
```

猎人只有当看到实现了 `Lion` 接口的猎物后才能狩猎。

```php
class Hunter {
    public function hunt(Lion $lion) {
    }
}
```

现假设我们需要在游戏中加入 `WildDog`，使猎人对它们也能进行狩猎。但是我们无法直接实现，因为狗具有不同的接口。要使它与我们的猎人兼容，我们需要创建一个兼容的适配器。
 
```php
// This needs to be added to the game
class WildDog {
    public function bark() {}
}

// Adapter around wild dog to make it compatible with our game
class WildDogAdapter implements Lion {
    protected $dog;

    public function __construct(WildDog $dog) {
        $this->dog = $dog;
    }
    
    public function roar() {
        $this->dog->bark();
    }
}
```

现在 `WildDog` 可能通过 `WildDogAdapter` 使用到我们的游戏中了。

```php
$wildDog = new WildDog();
$wildDogAdapter = new WildDogAdapter($wildDog);

$hunter = new Hunter();
$hunter->hunt($wildDogAdapter);
```

🚡 桥接(Bridge)
------

现实案例
> 假设你有一个由多个不同的页面组成的网站，然后你想让用户可以修改页面主题风格。那么你会怎么做？是为每一个页面针对每一个主题风格都创建一个复本，还是只创建分离的主题风格，然后根据用户的喜好加载主题风格？如果你想用第二种办法，那么桥接模式就是你的解决之道。

![With and without the bridge pattern](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

简单来说
> 桥接模式认为组合优于继承。它能将一个层级结构中的实现细节转到位于另一个分离的层级结构的对象中。

Wikipedia 上描述为
> 桥接模式是软件设计模式之一，它意在 ”将抽象与真实现分离，从而使它们可以各自独立的变化“。

**编程示例**

实现上面的网站的例子，这里定义了 `WebPage` 的层级结构

```php
interface WebPage {
    public function __construct(Theme $theme);
    public function getContent();
}

class About implements WebPage {
    protected $theme;
    
    public function __construct(Theme $theme) {
        $this->theme = $theme;
    }
    
    public function getContent() {
        return "About page in " . $this->theme->getColor();
    }
}

class Careers implements WebPage {
   protected $theme;
   
   public function __construct(Theme $theme) {
       $this->theme = $theme;
   }
   
   public function getContent() {
       return "Careers page in " . $this->theme->getColor();
   } 
}
```

然后是另外分离的主题风格层级结构

```php
interface Theme {
    public function getColor();
}

class DarkTheme implements Theme {
    public function getColor() {
        return 'Dark Black';
    }
}
class LightTheme implements Theme {
    public function getColor() {
        return 'Off white';
    }
}
class AquaTheme implements Theme {
    public function getColor() {
        return 'Light blue';
    }
}
```

最后将两个层级结构组合起来

```php
$darkTheme = new DarkTheme();

$about = new About($darkTheme);
$careers = new Careers($darkTheme);

echo $about->getContent(); // "About page in Dark Black";
echo $careers->getContent(); // "Careers page in Dark Black";
```

🌿 组合(Composite)
-----------------

现实案例
> 每个组织都由员工组成。每个员工都有相似的特征，如都有工资，都担负一些职责，需要（或者不需要）向某人汇报，有（或者没有）一些下属等。

简单来说
> 组合模式使得客户能以统一的方式对待每个对象。

Wikipedia 上描述为
> 在软件工程中，组合模式是一种分割式的设计模式。组合模式描述为：能以和对待单个对象实例相同的方式对待对象的组合。组合为的是将对象组织成树状结构，以表达 *部分-整体* 的层级关系。使用组合模式后，客户就能一致地对待单独对象和组合体了。

**编程示例**

使用上面的员工的例子。这里定义了不同类型的员工

```php
interface Employee {
    public function __construct(string $name, float $salary);
    public function getName() : string;
    public function setSalary(float $salary);
    public function getSalary() : float;
    public function getRoles()  : array;
}

class Developer implements Employee {

    protected $salary;
    protected $name;

    public function __construct(string $name, float $salary) {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName() : string {
        return $this->name;
    }

    public function setSalary(float $salary) {
        $this->salary = $salary;
    }

    public function getSalary() : float {
        return $this->salary;
    }

    public function getRoles() : array {
        return $this->roles;
    }
}

class Designer implements Employee {

    protected $salary;
    protected $name;

    public function __construct(string $name, float $salary) {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName() : string {
        return $this->name;
    }

    public function setSalary(float $salary) {
        $this->salary = $salary;
    }

    public function getSalary() : float {
        return $this->salary;
    }

    public function getRoles() : array {
        return $this->roles;
    }
}
```

再定义一个组织，它由不同类型的员工组成

```php
class Organization {
    
    protected $employees;

    public function addEmployee(Employee $employee) {
        $this->employees[] = $employee;
    }

    public function getNetSalaries() : float {
        $netSalary = 0;

        foreach ($this->employees as $employee) {
            $netSalary += $employee->getSalary();
        }

        return $netSalary;
    }
}
```

然后可以这样使用

```php
// Prepare the employees
$john = new Developer('John Doe', 12000);
$jane = new Designer('Jane', 10000);

// Add them to organization
$organization = new Organization();
$organization->addEmployee($john);
$organization->addEmployee($jane);

echo "Net salaries: " . $organization->getNetSalaries(); // Net Salaries: 22000
```

☕ 装饰器(Decorator)
-------------

现实案例

> 假设你运营一家能提供多种服务的汽车服务店。现在你怎样计算要收的费用？你会根据提供了的所有服务，将每项服务费用都动态叠加进去，直到算出总额。这里每种服务都是一种装饰器。

简单来说
> 装饰器模式通过将对象封装在装饰器类的对象中，从而使你能在运行时动态地修改原对象的行为。

Wikipedia 上描述为
> 在面向对象编程中，装饰器这种设计模式允许以静态或者动态的方式，将行为添加到某个对象中，而这种修改不会影响相同类中的其它实例对象的行为。装饰器模式通常对于遵循单一职责原则(Single Responsibility Principle)很有用, 因为它允许功能在类间进行划分，使得各个类只专注各自的功能领域。

**编程示例**

以咖啡为例。首先为简单咖啡实现咖啡接口

```php
interface Coffee {
    public function getCost();
    public function getDescription();
}

class SimpleCoffee implements Coffee {

    public function getCost() {
        return 10;
    }

    public function getDescription() {
        return 'Simple coffee';
    }
}
```

我们想使代码可扩展，允许在需要的时候能够修改选项。让我们增加一些添加物（装饰器）

```php
class MilkCoffee implements Coffee {
    
    protected $coffee;

    public function __construct(Coffee $coffee) {
        $this->coffee = $coffee;
    }

    public function getCost() {
        return $this->coffee->getCost() + 2;
    }

    public function getDescription() {
        return $this->coffee->getDescription() . ', milk';
    }
}

class WhipCoffee implements Coffee {

    protected $coffee;

    public function __construct(Coffee $coffee) {
        $this->coffee = $coffee;
    }

    public function getCost() {
        return $this->coffee->getCost() + 5;
    }

    public function getDescription() {
        return $this->coffee->getDescription() . ', whip';
    }
}

class VanillaCoffee implements Coffee {

    protected $coffee;

    public function __construct(Coffee $coffee) {
        $this->coffee = $coffee;
    }

    public function getCost() {
        return $this->coffee->getCost() + 3;
    }

    public function getDescription() {
        return $this->coffee->getDescription() . ', vanilla';
    }
}
```

现在可以制作咖啡了

```php
$someCoffee = new SimpleCoffee();
echo $someCoffee->getCost(); // 10
echo $someCoffee->getDescription(); // Simple Coffee

$someCoffee = new MilkCoffee($someCoffee);
echo $someCoffee->getCost(); // 12
echo $someCoffee->getDescription(); // Simple Coffee, milk

$someCoffee = new WhipCoffee($someCoffee);
echo $someCoffee->getCost(); // 17
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip

$someCoffee = new VanillaCoffee($someCoffee);
echo $someCoffee->getCost(); // 20
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip, vanilla
```

📦 外观(Facade)
----------------

现实案例
> 你是怎样开电脑的？ "按电源键" 你说！你相信那样一定可以，这是由于你正在使用电脑外部的一个简单接口，而其内部则需要完成大量工作才能实现开机。这个针对复杂子系统而设计的简单接口就是外观。

简单来说
> 外观模式为复杂子系统提供一个简化接口。

Wikipedia 上描述为
> 外观就是一个对象，它为更大规模的代码，如类库等提供简化的接口。

**编程示例**

使用上面的电脑的例子。现在先定义电脑类

```php
class Computer {

    public function getElectricShock() {
        echo "Ouch!";
    }

    public function makeSound() {
        echo "Beep beep!";
    }

    public function showLoadingScreen() {
        echo "Loading..";
    }

    public function bam() {
        echo "Ready to be used!";
    }

    public function closeEverything() {
        echo "Bup bup bup buzzzz!";
    }

    public function sooth() {
        echo "Zzzzz";
    }

    public function pullCurrent() {
        echo "Haaah!";
    }
}
```

这样定义外观

```php
class ComputerFacade
{
    protected $computer;

    public function __construct(Computer $computer) {
        $this->computer = $computer;
    }

    public function turnOn() {
        $this->computer->getElectricShock();
        $this->computer->makeSound();
        $this->computer->showLoadingScreen();
        $this->computer->bam();
    }

    public function turnOff() {
        $this->computer->closeEverything();
        $this->computer->pullCurrent();
        $this->computer->sooth();
    }
}
```

现在这样使用外观

```php
$computer = new ComputerFacade(new Computer());
$computer->turnOn(); // Ouch! Beep beep! Loading.. Ready to be used!
$computer->turnOff(); // Bup bup buzzz! Haah! Zzzzz
```

🍃 享元(Flyweight)
---------

现实案例
> 你有过在摊位上品尝过新茶吗？他们通常沏出比你所要的还要多的杯数，然后将多余的荼留给其他客人，从而起到节约资源（如燃气）的目的。享元模式的全部即共享。

简单来说
> 它能使相似对象间通过尽可能多地共享，以减少内存使用和计算花销。

Wikipedia 上描述为
> 在计算机编程中，享元是一种软件设计模式。一个享元就是一个对象，它通过与其它相似对象共享尽可能多的数据，以达到对内存的最少化使用;它适用于对象数量庞大的情况，此时简单地重复表示将需要过量的内存量。

**编程示例**

实现以上的茶的例子。首先定义各种茶和茶艺师

```php
// Anything that will be cached is flyweight. 
// Types of tea here will be flyweights.
class KarakTea {
}

// Acts as a factory and saves the tea
class TeaMaker {
    protected $availableTea = [];

    public function make($preference) {
        if (empty($this->availableTea[$preference])) {
            $this->availableTea[$preference] = new KarakTea();
        }

        return $this->availableTea[$preference];
    }
}
```

然后定义 `TeaShop`，提供饮茶服务

```php
class TeaShop {
    
    protected $orders;
    protected $teaMaker;

    public function __construct(TeaMaker $teaMaker) {
        $this->teaMaker = $teaMaker;
    }

    public function takeOrder(string $teaType, int $table) {
        $this->orders[$table] = $this->teaMaker->make($teaType);
    }

    public function serve() {
        foreach($this->orders as $table => $tea) {
            echo "Serving tea to table# " . $table;
        }
    }
}
```

可以如下使用

```php
$teaMaker = new TeaMaker();
$shop = new TeaShop($teaMaker);

$shop->takeOrder('less sugar', 1);
$shop->takeOrder('more milk', 2);
$shop->takeOrder('without sugar', 5);

$shop->serve();
// Serving tea to table# 1
// Serving tea to table# 2
// Serving tea to table# 5
```

🎱 代理(Proxy)
-------------------

现实案例
> 你有用过门禁卡开过门吗？打开门有多种方式，比如使用门禁门或者使用密码锁等。门的主要功能是打门，但在此之上还加了个代理，它增加了额外的一些功能。让我们使用以下的代码示例来更好地解释。

简单来说
> 使用代理模式，一个类可以代表其它类的功能。

Wikipedia 上描述为
> 一个代理，其最一般的形式，就是作为其它类的接口的一个类。代理就是一个包装或中介对象，客户通过调用它来访问幕后真正提供服务的对象。使用代理可以简单地转发到真实对象，也可以提供额外的逻辑。代理可以提供这些额外的功能，例如当在真实对象上的操作需要大量资源时进行缓存，或者对真实对象调用操作时先检查先决条件等。

**编程示例**

使用以上的安全门的例子。首先定义门的接口并实现门的类

```php
interface Door {
    public function open();
    public function close();
}

class LabDoor implements Door {
    public function open() {
        echo "Opening lab door";
    }

    public function close() {
        echo "Closing the lab door";
    }
}
```

然后定义代理，为门提供安全措施

```php
class Security {
    protected $door;

    public function __construct(Door $door) {
        $this->door = $door;
    }

    public function open($password) {
        if ($this->authenticate($password)) {
            $this->door->open();
        } else {
        	echo "Big no! It ain't possible.";
        }
    }

    public function authenticate($password) {
        return $password === '$ecr@t';
    }

    public function close() {
        $this->door->close();
    }
}
```

这里是如何使用

```php
$door = new Security(new LabDoor());
$door->open('invalid'); // Big no! It ain't possible.

$door->open('$ecr@t'); // Opening lab door
$door->close(); // Closing lab door
```

另一个例子是一些数据映射(data-mapper)的实现。例如，我（原作者）使用该模式为 MongoDB 做了一个对象数据映射器(ODM, Object Data Mapper)，我通过在 mongo 类外编写一个代理来调用特殊函数 `__call()`。对代理的所有方法函数都转到原 mongo 类上，并且取得的结果也都原样返回，除了 `find` 或 `findOne` 的数据会映射成所需的类对象并返回，而不以 `Cursor` 返回。

行为型设计模式
==========================

简单来说
> 它关注于对象间的职责分配。它们与结构型模式的区别不于：它们不只指定结构，还勾画出相互间的消息传送/通信模式。或者也可以说，它们有助于回答 "如何在软件组件中实现行为？"

Wikipedia 上描述为
> 在软件工程中，行为型设计模式标识了对象间的常见通信模式以及其实现模式。通过这样做，这些模式增加了实施此种通信的灵活性。

* [责任链(Chain of Responsibility)](#-责任链chain-of-responsibility)
* [命令(Command)](#-命令command)
* [迭代器(Iterator)](#-迭代器iterator)
* [中介者(Mediator)](#-中介者mediator)
* [备忘录(Memento)](#-备忘录memento)
* [观察者(Observer)](#-观察者observer)
* [访问者(Visitor)](#-访问者visitor)
* [策略(Strategy)](#-策略strategy)
* [状态(State)](#-状态state)
* [模板方法(Template Method)](#-模板方法template-method)

🔗 责任链(Chain of Responsibility)
-----------------------

现实案例
> 例如，你在你的账户中设置了三种支付方式 (`A`, `B` 和 `C`); 每个内都存有不同的金额。`A` 内有 100 元，`B` 内有 300 元以及 `C` 内有 1000 元，并且你的支付偏好选择设置为先 `A` 然后 `B` 最后 `C`。你想要习价格为 210 元的商品。如何使用责任链，那么首先会检查帐户 `A`，看它是否能足够支付，如何可以那么完成支付然后链条到此结束。如果不能，那么请求将转向检查帐户 `B` 中的金额，如果足够那么链条到此结构否则请求将继续传递直到找到合适的处理帐户。这里 `A`, `B` 和 `C` 都是链条中的链接点，而整个现象就是责任链。

简单来说
> 它用于创建一个对象链。请求从一端进入，并在一个对象传递到另一个，直至找到合适的处理对象。

Wikipedia 上描述为
> 在面向对象设计中，责任链设计模式由命令对象源来一系列的处理对象组成。每个处理对象中都包含有逻辑，用来定义它能处置的命令对象类型; 其它不同处置的都将传递给链条中的下一个处理对象。

**编程示例**

实现上面的帐户的例子。首先定义一个基本的帐户类，其中包含有将各帐户关联起来的逻辑功能，然后再实现几种帐户。

```php
abstract class Account {
    protected $successor;
    protected $balance;

    public function setNext(Account $account) {
        $this->successor = $account;
    }
    
    public function pay(float $amountToPay) {
        if ($this->canPay($amountToPay)) {
            echo sprintf('Paid %s using %s' . PHP_EOL, $amountToPay, get_called_class());
        } else if ($this->successor) {
            echo sprintf('Cannot pay using %s. Proceeding ..' . PHP_EOL, get_called_class());
            $this->successor->pay($amountToPay);
        } else {
            throw Exception('None of the accounts have enough balance');
        }
    }
    
    public function canPay($amount) : bool {
        return $this->balance >= $amount;
    }
}

class Bank extends Account {
    protected $balance;

    public function __construct(float $balance) {
        $this->balance = $balance;
    }
}

class Paypal extends Account {
    protected $balance;

    public function __construct(float $balance) {
        $this->balance = $balance;
    }
}

class Bitcoin extends Account {
    protected $balance;

    public function __construct(float $balance) {
        $this->balance = $balance;
    }
}
```

现在用上面定义的帐户（如 Bank, Paypal, Bitcoin) 准备一个链条

```php
// 创建如下的一个链接
//      $bank->$paypal->$bitcoin
//
// 优先使用银行帐户
//      如果银行帐户无法支付再用 paypal
//      如果 paypal 不能支持再用比特币

$bank = new Bank(100);          // Bank with balance 100
$paypal = new Paypal(200);      // Paypal with balance 200
$bitcoin = new Bitcoin(300);    // Bitcoin with balance 300

$bank->setNext($paypal);
$paypal->setNext($bitcoin);

// Let's try to pay using the first priority i.e. bank
$bank->pay(259);

// 输出会是
// ==============
// Cannot pay using bank. Proceeding ..
// Cannot pay using paypal. Proceeding ..: 
// Paid 259 using Bitcoin!
```

👮 命令(Command)
-------

现实案例
> 一个普通的例子就是在餐厅吃饭。你（即 `客户 Client`）要求服务员（即 `调用者 Invoker`）上菜（即 `命令 Command`），而服务员只是简单地将你的请求传达到厨师（即 `接收者 Receiver`），厨师知道做哪道菜及如何做。
> 另一个例子是你（即 `客户 Client`）使用遥控器（即 `调用者 Invoker`）打开（即 `命令 Command`）电视机（即 `接收者 Receiver`）。

简单来说
> 它允许你在对象中封装行为。该模式背后的主要思想就是提供将客户与接收者解耦的方法。

Wikipedia 上描述为
> 在面向对象编程中，命令模式是一种行为型设计模式，它用对象来封装执行一个动作或稍后触发一个事件所需的所有信息。这些信息包括方法名，拥有该方法的对象以及方法参数的值等。

**编程示例**

首先定义接收者，并且对它能够执行的每个行为都进行了实现

```php
// Receiver
class Bulb {
    public function turnOn() {
        echo "Bulb has been lit";
    }
    
    public function turnOff() {
        echo "Darkness!";
    }
}
```

然后定义一个每个命令都需要实现的接口，并定义一组命令

```php
interface Command {
    public function execute();
    public function undo();
    public function redo();
}

// Command
class TurnOn implements Command {
    protected $bulb;
    
    public function __construct(Bulb $bulb) {
        $this->bulb = $bulb;
    }
    
    public function execute() {
        $this->bulb->turnOn();
    }
    
    public function undo() {
        $this->bulb->turnOff();
    }
    
    public function redo() {
        $this->execute();
    }
}

class TurnOff implements Command {
    protected $bulb;
    
    public function __construct(Bulb $bulb) {
        $this->bulb = $bulb;
    }
    
    public function execute() {
        $this->bulb->turnOff();
    }
    
    public function undo() {
        $this->bulb->turnOn();
    }
    
    public function redo() {
        $this->execute();
    }
}
```

再定义一个 `调用者 Invoker`，客户与它交互来处理任何命令
```php
// Invoker
class RemoteControl {
    
    public function submit(Command $command) {
        $command->execute();
    }
}
```

最后看下客户如何使用

```php
$bulb = new Bulb();

$turnOn = new TurnOn($bulb);
$turnOff = new TurnOff($bulb);

$remote = new RemoteControl();
$remote->submit($turnOn); // Bulb has been lit!
$remote->submit($turnOff); // Darkness!
```

命令模式也可以用来实现事务型系统。当你一旦执行命令后就保存其执行记录。如果最后一个命令也执行成功了，那样很好，否则只需遍历历史记录，并在所有执行了的命令上执行 `undo` 即可。

➿ 迭代器(Iterator)
--------

现实案例
> 老式收音机是迭代器的很好的例子，用户可以先从某个频道开始，然后使用前后按键来遍历各个频道。或者以 MP3 播放器或电视机为例，也可以用前后按键来遍历各首歌曲或频道。换句话说，它们都提供了一个接口，来遍历各个频道，歌曲或广播电台。

简单来说
> 它提供了一种访问对象内所有元素的方法，而避免暴露低层是如何表示的。

Wikipedia 上描述为
> 在面向对象编程中，迭代器模式是一个设计模式，迭代器使用遍历容器以访问容器内的元素。迭代器模式将算法和容器进行了解耦; 但在某些情况下，算法必需是特定与容器的，因而无法解耦。

**编程示例**

在 PHP 很容易使用 SPL (标准 PHP 库) 来实现。实现上面的广播电台的例子。首先我们定义 `RadioStation`

```php
class RadioStation {
    protected $frequency;

    public function __construct(float $frequency) {
        $this->frequency = $frequency;    
    }
    
    public function getFrequency() : float {
        return $this->frequency;
    }
}
```

然后定义迭代器

```php
use Countable;
use Iterator;

class StationList implements Countable, Iterator {
    /** @var RadioStation[] $stations */
    protected $stations = [];
    
    /** @var int $counter */
    protected $counter;
    
    public function addStation(RadioStation $station) {
        $this->stations[] = $station;
    }
    
    public function removeStation(RadioStation $toRemove) {
        $toRemoveFrequency = $toRemove->getFrequency();
        $this->stations = array_filter($this->stations, function (RadioStation $station) use ($toRemoveFrequency) {
            return $station->getFrequency() !== $toRemoveFrequency;
        });
    }
    
    public function count() : int {
        return count($this->stations);
    }
    
    public function current() : RadioStation {
        return $this->stations[$this->counter];
    }
    
    public function key() {
        return $this->counter;
    }
    
    public function next() {
        $this->counter++;
    }
    
    public function rewind() {
        $this->counter = 0;
    }
    
    public function valid(): bool
    {
        return isset($this->stations[$this->counter]);
    }
}
```

然后可以这样使用

```php
$stationList = new StationList();

$stationList->addStation(new RadioStation(89));
$stationList->addStation(new RadioStation(101));
$stationList->addStation(new RadioStation(102));
$stationList->addStation(new RadioStation(103.2));

foreach($stationList as $station) {
    echo $station->getFrequency() . PHP_EOL;
}

$stationList->removeStation(new RadioStation(89)); // Will remove station 89
```

👽 中介者(Mediator)
========

现实案例
> 一个常见的例子就是当你在手机上与别人通话时，你们之间隔有一个网络服务提供商，你们的通话要通过它，而不是直接传送的。在这种情况下网络服务提供商就是一个中介者。

简单来说
> 中介者模式引入了一个第三方对象（叫中介者 mediator) 来控制两个对象（叫同事 colleagues) 间的交互。它有助于减少彼此通信的类之间的耦合性。因为现在它们无需了解对方的实现细节。

Wikipedia 上描述为
> 在软件工程中，中介者模式定义了一个对象，它对一组对象如何交互进行了封装。这种模式被认为是一种行为型模式，这是因为它能改变程序运行时的行为。

**编程示例**

这里是一个聊天室（即中介者）的最简单的例子，其中的用户（即同事）之间相互发送消息。

产自，我们定义中介者（即聊天室）

```php
// Mediator
class ChatRoom implements ChatRoomMediator {
    public function showMessage(User $user, string $message) {
        $time = date('M d, y H:i');
        $sender = $user->getName();

        echo $time . '[' . $sender . ']:' . $message;
    }
}
```

然后定义用户（即同事）

```php
class User {
    protected $name;
    protected $chatMediator;

    public function __construct(string $name, ChatRoomMediator $chatMediator) {
        $this->name = $name;
        $this->chatMediator = $chatMediator;
    }
    
    public function getName() {
        return $this->name;
    }
    
    public function send($message) {
        $this->chatMediator->showMessage($this, $message);
    }
}
```

使用

```php
$mediator = new ChatRoom();

$john = new User('John Doe', $mediator);
$jane = new User('Jane Doe', $mediator);

$john->send('Hi there!');
$jane->send('Hey!');

// Output will be
// Feb 14, 10:58 [John]: Hi there!
// Feb 14, 10:58 [Jane]: Hey!
```

💾 备忘录(Memento)
-------

现实案例
> 以计算器（即发起人 originator）为例，当你完成计算后，最后的结果会被保存在内存（即备忘录 memento）中，那样你就能取回它，也许也可以通过一些功能按键（即管理者 caretaker）来恢复它。

简单来说
> 备忘录模式就是关于用某种方式获取或保存对象当前状态的模式，从而使对象能在稍后顺序恢复。

Wikipedia 上描述为
> 备忘录模式是一种软件设计模式，它提供了将对象恢复到先前状态的能力（使用回滚来撤销操作）。

通常当你需要提供一些撤销功能时非常有用。

**编程示例**

以文本编辑器为例，它会不时地保存当前状态，从而当你需要时可以恢复。

首先定义我们的备忘录对象，它能用于保存编辑器的状态

```php
class EditorMemento {
    protected $content;
    
    public function __construct(string $content) {
        $this->content = $content;
    }
    
    public function getContent() {
        return $this->content;
    }
}
```

然后定义编辑器（即发起人 originator），它会用到备忘录对象

```php
class Editor {
    protected $content = '';
    
    public function type(string $words) {
        $this->content = $this->content . ' ' . $words;
    }
    
    public function getContent() {
        return $this->content;
    }
    
    public function save() {
        return new EditorMemento($this->content);
    }
    
    public function restore(EditorMemento $memento) {
        $this->content = $memento->getContent();
    }
}
```

然后可以这样使用

```php
$editor = new Editor();

// Type some stuff
$editor->type('This is the first sentence.');
$editor->type('This is second.');

// Save the state to restore to : This is the first sentence. This is second.
$saved = $editor->save();

// Type some more
$editor->type('And this is third.');

// Output: Content before Saving
echo $editor->getContent(); // This is the first sentence. This is second. And this is third.

// Restoring to last saved state
$editor->restore($saved);

$editor->getContent(); // This is the first sentence. This is second.
```

😎 观察者(Observer)
--------
Real world example
> A good example would be the job seekers where they subscribe to some job posting site and they are notified whenever there is a matching job opportunity.   

In plain words
> Defines a dependency between objects so that whenever an object changes its state, all its dependents are notified.

Wikipedia says
> The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.

**Programmatic example**

Translating our example from above. First of all we have job seekers that need to be notified for a job posting
```php
class JobPost {
    protected $title;
    
    public function __construct(string $title) {
        $this->title = $title;
    }
    
    public function getTitle() {
        return $this->title;
    }
}

class JobSeeker implements Observer {
    protected $name;

    public function __construct(string $name) {
        $this->name = $name;
    }

    public function onJobPosted(JobPost $job) {
        // Do something with the job posting
        echo 'Hi ' . $this->name . '! New job posted: '. $job->getTitle();
    }
}
```
Then we have our job postings to which the job seekers will subscribe
```php
class JobPostings implements Observable {
    protected $observers = [];
    
    protected function notify(JobPost $jobPosting) {
        foreach ($this->observers as $observer) {
            $observer->onJobPosted($jobPosting);
        }
    }
    
    public function attach(Observer $observer) {
        $this->observers[] = $observer;
    }
    
    public function addJob(JobPost $jobPosting) {
        $this->notify($jobPosting);
    }
}
```
Then it can be used as
```phpwei
// Create subscribers
$johnDoe = new JobSeeker('John Doe');
$janeDoe = new JobSeeker('Jane Doe');

// Create publisher and attach subscribers
$jobPostings = new JobPostings();
$jobPostings->attach($johnDoe);
$jobPostings->attach($janeDoe);

// Add a new job and see if subscribers get notified
$jobPostings->addJob(new JobPost('Software Engineer'));

// Output
// Hi John Doe! New job posted: Software Engineer
// Hi Jane Doe! New job posted: Software Engineer
```

🏃 访问者(Visitor)
-------
Real world example
> Consider someone visiting Dubai. They just need a way (i.e. visa) to enter Dubai. After arrival, they can come and visit any place in Dubai on their own without having to ask for permission or to do some leg work in order to visit any place here; just let them know of a place and they can visit it. Visitor pattern lets you do just that, it helps you add places to visit so that they can visit as much as they can without having to do any legwork.

In plain words
> Visitor pattern lets you add further operations to objects without having to modify them.
    
Wikipedia says
> In object-oriented programming and software engineering, the visitor design pattern is a way of separating an algorithm from an object structure on which it operates. A practical result of this separation is the ability to add new operations to existing object structures without modifying those structures. It is one way to follow the open/closed principle.

**Programmatic example**

Let's take an example of a zoo simulation where we have several different kinds of animals and we have to make them Sound. Let's translate this using visitor pattern 

```php
// Visitee
interface Animal {
    public function accept(AnimalOperation $operation);
}

// Visitor
interface AnimalOperation {
    public function visitMonkey(Monkey $monkey);
    public function visitLion(Lion $lion);
    public function visitDolphin(Dolphin $dolphin);
}
```
Then we have our implementations for the animals
```php
class Monkey implements Animal {
    
    public function shout() {
        echo 'Ooh oo aa aa!';
    }

    public function accept(AnimalOperation $operation) {
        $operation->visitMonkey($this);
    }
}

class Lion implements Animal {
    public function roar() {
        echo 'Roaaar!';
    }
    
    public function accept(AnimalOperation $operation) {
        $operation->visitLion($this);
    }
}

class Dolphin implements Animal {
    public function speak() {
        echo 'Tuut tuttu tuutt!';
    }
    
    public function accept(AnimalOperation $operation) {
        $operation->visitDolphin($this);
    }
}
```
Let's implement our visitor
```php
class Speak implements AnimalOperation {
    public function visitMonkey(Monkey $monkey) {
        $monkey->shout();
    }
    
    public function visitLion(Lion $lion) {
        $lion->roar();
    }
    
    public function visitDolphin(Dolphin $dolphin) {
        $dolphin->speak();
    }
}
```

And then it can be used as
```php
$monkey = new Monkey();
$lion = new Lion();
$dolphin = new Dolphin();

$speak = new Speak();
wei
$monkey->accept($speak);    // Ooh oo aa aa!    
$lion->accept($speak);      // Roaaar!
$dolphin->accept($speak);   // Tuut tutt tuutt!
```
We could have done this simply by having a inheritance hierarchy for the animals but then we would have to modify the animals whenever we would have to add new actions to animals. But now we will not have to change them. For example, let's say we are asked to add the jump behavior to the animals, we can simply add that by creating a new visitor i.e.

```php
class Jump implements AnimalOperation {
    public function visitMonkey(Monkey $monkey) {
        echo 'Jumped 20 feet high! on to the tree!';
    }
    
    public function visitLion(Lion $lion) {
        echo 'Jumped 7 feet! Back on the ground!';
    }
    
    public function visitDolphin(Dolphin $dolphin) {
        echo 'Walked on water a little and disappeared';
    }
}
```
And for the usage
```php
$jump = new Jump();

$monkey->accept($speak);   // Ooh oo aa aa!
$monkey->accept($jump);    // Jumped 20 feet high! on to the tree!

$lion->accept($speak);     // Roaaar!
$lion->accept($jump);      // Jumped 7 feet! Back on the ground! 

$dolphin->accept($speak);  // Tuut tutt tuutt! 
$dolphin->accept($jump);   // Walked on water a little and disappeared
```

💡 策略(Strategy)
--------

Real world example
> Consider the example of sorting, we implemented bubble sort but the data started to grow and bubble sort started getting very slow. In order to tackle this we implemented Quick sort. But now although the quick sort algorithm was doing better for large datasets, it was very slow for smaller datasets. In order to handle this we implemented a strategy where for small datasets, bubble sort will be used and for larger, quick sort.

In plain words
> Strategy pattern allows you to switch the algorithm or strategy based upon the situation.

Wikipedia says
> In computer programming, the strategy pattern (also known as the policy pattern) is a behavioural software design pattern that enables an algorithm's behavior to be selected at runtime.
 
**Programmatic example**

Translating our example from above. First of all we have our strategy interface and different strategy implementations

```php
interface SortStrategy {
    public function sort(array $dataset) : array; 
}

class BubbleSortStrategy implements SortStrategy {
    public function sort(array $dataset) : array {
        echo "Sorting using bubble sort";
         
        // Do sorting
        return $dataset;
    }
} 

class QuickSortStrategy implements SortStrategy {
    public function sort(array $dataset) : array {
        echo "Sorting using quick sort";
        
        // Do sorting
        return $dataset;
    }
}
```
 
And then we have our client that is going to use any strategy
```php
class Sorter {
    protected $sorter;
    
    public function __construct(SortStrategy $sorter) {
        $this->sorter = $sorter;
    }
    
    public function sort(array $dataset) : array {
        return $this->sorter->sort($dataset);
    }
}
```
And it can be used as
```php
$dataset = [1, 5, 4, 3, 2, 8];

$sorter = new Sorter(new BubbleSortStrategy());
$sorter->sort($dataset); // Output : Sorting using bubble sort

$sorter = new Sorter(new QuickSortStrategy());
$sorter->sort($dataset); // Output : Sorting using quick sort
```

💢 状态(State)
-----
Real world example
> Imagine you are using some drawing application, you choose the paint brush to draw. Now the brush changes its behavior based on the selected color i.e. if you have chosen red color it will draw in red, if blue then it will be in blue etc.  

In plain words
> It lets you change the behavior of a class when the state changes.

Wikipedia says
> The state pattern is a behavioral software design pattern that implements a state machine in an object-oriented way. With the state pattern, a state machine is implemented by implementing each individual state as a derived class of the state pattern interface, and implementing state transitions by invoking methods defined by the pattern's superclass.
> The state pattern can be interpreted as a strategy pattern which is able to switch the current strategy through invocations of methods defined in the pattern's interface.

**Programmatic example**

Let's take an example of text editor, it lets you change the state of text that is typed i.e. if you have selected bold, it starts writing in bold, if italic then in italics etc.

First of all we have our state interface and some state implementations

```php
interface WritingState {
    public function write(string $words);
}

class UpperCase implements WritingState {
    public function write(string $words) {
        echo strtoupper($words); 
    }
} 

class LowerCase implements WritingState {
    public function write(string $words) {
        echo strtolower($words); 
    }
}

class Default implements WritingState {
    public function write(string $words) {
        echo $words;
    }
}
```
Then we have our editor
```php
class TextEditor {
    protected $state;
    
    public function __construct(WritingState $state) {
        $this->state = $state;
    }
    
    public function setState(WritingState $state) {
        $this->state = $state;
    }
    
    public function type(string $words) {
        $this->state->write($words);
    }
}
```
And then it can be used as
```php
$editor = new TextEditor(new Default());

$editor->type('First line');

$editor->setState(new UpperCaseState());

$editor->type('Second line');
$editor->type('Third line');

$editor->setState(new LowerCaseState());

$editor->type('Fourth line');
$editor->type('Fifth line');

// Output:
// First line
// SECOND LINE
// THIRD LINE
// fourth line
// fifth line
```

📒 模板方法(Template Method)
---------------

现实案例
> 假设我们要造房子，造房子的步骤看起来像这样:
> - 打地基
> - 砌墙
> - 盖屋顶
> - 加其它层
> 
> 这些步骤的顺序永远都不会变，即你不可能先盖屋顶再砌墙等。但是每一步的具体操作都是可以修改的，比如说，你可以砌木墙，聚酯纤维墙或者石头墙。
 
简单来说
> 模板方法定义了某特定算法如何执行的框架，但执行步骤的具体实现则推迟到子类去完成。
 
Wikipedia 上描述为
> 在软件工程中，模板方法是行为型设计模式的一种，它定义了操作中的某个算法的程序框架，并将一些步骤推迟到子类中实现。它能在不修改算法结构的情况下，重新定义算法中的某些步骤。

**编程示例**

假设我们有一个构建工具，它能帮助我们进行测试，代码检查，构建，生成构建报告（比如代码覆盖率报告，代码检查报告等）以及部署应用至测试服务器。

首先我们定义一个基类用来指定构建算法的框架

```php
abstract class Builder {
    
    // Template method 
    public final function build() {
        $this->test();
        $this->lint();
        $this->assemble();
        $this->deploy();
    }
    
    public abstract function test();
    public abstract function lint();
    public abstract function assemble();
    public abstract function deploy();
}
```

然后进行各种实现。

```php
class AndroidBuilder extends Builder {
    public function test() {
        echo 'Running android tests';
    }
    
    public function lint() {
        echo 'Linting the android code';
    }
    
    public function assemble() {
        echo 'Assembling the android build';
    }
    
    public function deploy() {
        echo 'Deploying android build to server';
    }
}

class IosBuilder extends Builder {
    public function test() {
        echo 'Running ios tests';
    }
    
    public function lint() {
        echo 'Linting the ios code';
    }
    
    public function assemble() {
        echo 'Assembling the ios build';
    }
    
    public function deploy() {
        echo 'Deploying ios build to server';
    }
}
```

接着，可以这样使用

```php
$androidBuilder = new AndroidBuilder();
$androidBuilder->build();

// Output:
// Running android tests
// Linting the android code
// Assembling the android build
// Deploying android build to server

$iosBuilder = new IosBuilder();
$iosBuilder->build();

// Output:
// Running ios tests
// Linting the ios code
// Assembling the ios build
// Deploying ios build to server
```

## 🚦 Wrap Up Folks

And that about wraps it up. I will continue to improve this, so you might want to watch/star this repository to revisit. Also, I have plans on writing the same about the architectural patterns, stay tuned for it.

## 👬 Contribution

- Report issues
- Open pull request with improvements
- Spread the word
- Reach out to me directly at kamranahmed.se@gmail.com or on twitter [@kamranahmedse](http://twitter.com/kamranahmedse)

## License
MIT © [Kamran Ahmed](http://kamranahmed.info)


项目进度
======

- [x] 简介 (2017-02-24)
- [x] 创建型设计模式 - 简单工厂 (2017-02-24)
- [x] 创建型设计模式 - 工厂方法 (2017-02-24)
- [x] 创建型设计模式 - 抽象工厂 (2017-02-25)
- [x] 创建型设计模式 - 建造者 (2017-02-25)
- [x] 创建型设计模式 - 原型 (2017-02-25)
- [x] 创建型设计模式 - 单例 (2017-02-25)
- [x] 结构型设计模式 - 适配器 (2017-02-25)
- [x] 结构型设计模式 - 桥接 (2017-02-26，由 [DashShen](https://github.com/DashShen) 翻译，[haiiiiiyun](https://github.com/haiiiiiyun/) 校审)
- [x] 结构型设计模式 - 组合 (2017-02-26)
- [x] 结构型设计模式 - 装饰器 (2017-02-26)
- [x] 结构型设计模式 - 外观 (2017-02-26)
- [x] 结构型设计模式 - 享元 (2017-02-27)
- [x] 结构型设计模式 - 代理 (2017-02-27)
- [x] 行为型设计模式 - 责任链 (2017-02-27)
- [x] 行为型设计模式 - 命令 (2017-02-27)
- [x] 行为型设计模式 - 迭代器 (2017-02-27)
- [x] 行为型设计模式 - 中介者 (2017-02-27)
- [x] 行为型设计模式 - 备忘录 (2017-02-27)
- [x] 行为型设计模式 - 模板方法 (2017-02-27，由 [DashShen](https://github.com/DashShen) 翻译，[haiiiiiyun](https://github.com/haiiiiiyun/) 校审)

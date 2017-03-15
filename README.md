![Design Patterns For Humans](https://cloud.githubusercontent.com/assets/11269635/23065273/1b7e5938-f515-11e6-8dd3-d0d58de6bb9a.png)


***
<p align="center">
🎉  設計模式的超簡化描述！ 🎉
</p>
<p align="center">
A topic that can easily make anyone's mind wobble. Here I try to make them stick in to your mind (and maybe mine) by explaining them in the <i>simplest</i> way possible.
</p>
***

說明
====

This is the Simplified Chinese translation of [design-patterns-for-humans](https://github.com/kamranahmedse/design-patterns-for-humans). Thank [Kamran Ahmed](https://github.com/kamranahmedse) for his great work!

[design-patterns-for-humans](https://github.com/kamranahmedse/design-patterns-for-humans) 是由 [Kamran Ahmed](https://github.com/kamranahmedse) 發起和維護的項目，專案以簡潔的語言對各種設計模式進行了描述和整理。

本項目是 [design-patterns-for-humans](https://github.com/kamranahmedse/design-patterns-for-humans) 專案的繁體中文翻譯版本。也歡迎您一起參與翻譯和校審。

本專案的進度和貢獻者將在文末列出。

當前翻譯的原文版本是 [5cf37f7](https://github.com/kamranahmedse/design-patterns-for-humans/commit/b9a53a87a4026cb0b8e76feba72a91ecfcdf4f45)


🚀  簡介
=================

設計模式是經常性問題的解決方案; **是如何解決特定問題的指導方針**。它們不是類、包或庫，只要加到程式中就能起作用。它們是關於如何在特定的情況下解決特定問題的指導方針。

> 設計模式是經常性問題的解決方案; 是如何解決特定問題的指導方針。

Wikipedia 上描述為

> 在軟體工程中，軟體設計模式是在軟體設計的給定上下文中，針對普遍問題的一種通用且可重用的解決方案。它不是一個已完成的設計，不能直接轉化成源碼或機器碼。它是就如何解決某個問題，且能用於許多不同情況的一種描述或範本。

⚠️ 注意
-----------------
- 設計模式不是能解決所有問題的萬能鑰匙。
- 不要強迫使用: 如果這樣，可能會出問題。請記住設計模式是 **解決** 問題的方案，不是 **尋找** 問題的方案; 因此不要考慮過頭了。
- 如果在適合的地方以正確的方式使用，它們會很有效; 否則它們可能會導致你的程式碼極度混亂。

> 同時注意下面的程式碼範例都是用 PHP-7 寫的。但這問題也不會很大，因為概念都是一樣的。另外 **支援其它語言的工作正在進行中**。

設計模式的類型
-----------------

* [建立型](#建立型設計模式)
* [結構型](#結構型設計模式)
* [行為型](#行為型設計模式)

建立型設計模式
==========================

簡單來說
> 建立型模式關注於如何產生實體一個或一組相關物件。

Wikipedia 上描述為
> 在軟體工程中，建立型設計模式是處理物件建立機制，設法以適合當前情況的方式來建立物件的設計模式。物件建立時若使用一般形式可能會導致設計難題或增加設計的複雜度。建立型設計模式通過對物件建立過程的控制以解決此問題。

 * [簡單工廠(Simple Factory)](#-簡單工廠simple-factory)
 * [工廠方法(Factory Method)](#-工廠方法factory-method)
 * [抽象工廠(Abstract Factory)](#-抽象工廠abstract-factory)
 * [建造者(Builder)](#-建造者builder)
 * [原型(Prototype)](#-原型prototype)
 * [單例(Singleton)](#-單例singleton)

🏠 簡單工廠(Simple Factory)
--------------
現實案例
> 假設你正在蓋房子，需要用到門。如果每次需要門時，你都穿上木匠服在房子裡親自製作，肯定會導致一團糟。這種情況下你需要將門放在工廠裡製作。

簡單來說
> 簡單工廠模式對客戶端隱藏了所有的產生實體邏輯，只簡單地為客戶端建立實例。

Wikipedia 上描述為
> 在物件導向程式設計 (OOP) 中，工廠就是一個用於建立其它物件的物件,  – 形式上它可以是一個函數或方法，它在被方法調用時（假設通過 "new"）會返回不同原型或類的物件。

**程式設計範例**

首先定義門的介面及其實作

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

然後定義門的工廠，它建立並回傳門實例

```php
class DoorFactory {
   public static function makeDoor($width, $height) : Door {
       return new WoodenDoor($width, $height);
   }
}
```

再這樣使用

```php
$door = DoorFactory::makeDoor(100, 200);
echo 'Width: ' . $door->getWidth();
echo 'Height: ' . $door->getHeight();
```

**何時用？**

當建立物件不僅只是一些賦值操作，還涉及一些邏輯操作時，就適合將這些邏輯放到一個專門的工廠中，從而能避免程式碼重複。

🏭 工廠方法(Factory Method)
--------------

現實案例
> 考慮人事招聘經理的情況。一個人不可能參與對每個職位的面試。根據職位空缺，她必須決定將面試工作委派給不同的人來完成。

簡單來說
> 它提供了一種能將產生實體邏輯委派到子類中完成的方式。

Wikipedia 上描述為
> 在基於類別的程式設計中，工廠方法模式是一種建立型模式，它無需指定將要創造的物件的具體類別，只使用工廠中的各種方法就能處理物件建立的問題。物件的建立是通過呼叫工廠方法而非構造器來完成的，工廠方法—要嘛在介面中定義然後由子類實現，要嘛是在基類別中實作然後被繼承類重載。
 
 **程式設計範例**
 
繼續上面的人事招聘經理的例子。首先定義面試介面並給出了幾個實作

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

現在讓我們建立 `HiringManager`

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

現在任何子類都可以擴展並提供所需的面試介面

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

然後可以這樣使用

```php
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // Output: Asking about design patterns

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // Output: Asking about community building.
```

**何時使用？**

適合時當類中存在一些通用操作，但是所需的子類是在執行時才動態決定的情況。換句話說，即當客戶端無法知道所需的確切子類別時。

🔨 抽象工廠(Abstract Factory)
----------------

現實案例
> 繼續簡單工廠模式中門的例子。基於你的需求，你可能要從木門店獲取木門，從鐵門店獲取鐵門，或者從 PVC 相關店獲取 PVC 門。另外你可能還要找不同專長的人來安裝門，例如找木匠來安裝木門，找電焊工來安裝鐵門等等。可以看到現在門已經有了依賴性，比如木門依賴於木匠，鐵門依賴於電焊工等。

簡單來說
> 就是工廠的工廠; 該工廠將各個相關/相依賴的工廠組合起來，而無需指定他們具體的類別。

Wikipedia 上描述為
> 抽象工廠模式提供了一種將具有相同風格的一組工廠封閉起來的方法，而無需指定各工廠具體的類別。

**程式設計範例**

修改上面門的例子。首先定義 `Door` 介面並做出幾個實作

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

然後為每種門都定義相應的安裝人員

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

現在定義我們的抽象工廠，它能為我們建立相關的一組物件，例如木門工廠將會建立木門及木門安裝人員對象，而鐵門工廠將會建立鐵門及鐵門安裝人員對象。

```php
interface DoorFactory {
    public function makeDoor() : Door;
    public function makeFittingExpert() : DoorFittingExpert;
}

// 木門工廠將返回木匠及木門對象
class WoodenDoorFactory implements DoorFactory {
    public function makeDoor() : Door {
        return new WoodenDoor();
    }

    public function makeFittingExpert() : DoorFittingExpert{
        return new Carpenter();
    }
}

// 鐵門工廠將返回鐵門及相應的安裝人員
class IronDoorFactory implements DoorFactory {
    public function makeDoor() : Door {
        return new IronDoor();
    }

    public function makeFittingExpert() : DoorFittingExpert{
        return new Welder();
    }
}
```

然後可以這樣使用

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

可以看到木門工廠已經封裝了 `木匠` 和 `木門` 而鐵門工廠已經封閉了 `鐵門` 和 `電焊工`。這樣它就能確保，每次建立了一個門物件後，我們也可以得到其相應的安裝人員物件。

**何時使用？**

當建立邏輯有點複雜但內部又互相關聯時使用。

👷 創造者(Builder)
--------------------------------------------

現實案例
> 假設你在 Harees(美國連鎖速食店)，你下了單，假定說要來份 "大份裝"，然後店員 *無需再多問* 就直接為你送上 "大份裝"; 像這樣的就是簡單工廠模式的例子。但是有些情況下建立邏輯可能要涉及多個步驟。例如你想要一份定制餐，你給出了如何做漢堡的具體要求，例如使用什麼麵包，使用何種醬汁，何種乳酪等。那麼這種情況下就需要使用建造者模式。

簡單來說
> 它允許你建立「不同口味」的物件，同時又能避免「污染」構造函數的參數。適合當某物件可能會有多種「口味」，或者物件的建立過程涉及多個步驟時使用。
 
Wikipedia 上描述為
> 建造者模式是一種物件建立的軟體設計模式，它意在為重疊構造器這種反模式(telescoping constructor anti-pattern)找到一種解決方案。

既然說到了，那讓我多說幾句什麼是重疊構造器反模式(telescoping constructor anti-pattern)。我們或多或少有看到過像這樣的構造函數：
 
```php
public function __construct($size, $cheese = true, $pepperoni = true, $tomato = false, $lettuce = true) {
}
```

可以看到; 構造函數的參數個數很快會變得一發不可收拾，從而要理解參數佈局會變得困難。另外假如以後還要增加更多功能的話，該參數列表還會繼續增長。這就是所謂的重疊構造器反模式(telescoping constructor anti-pattern)。

**程式設計範例**

理智地選擇是使用建造者模式。首先定義我們需要製作的漢堡類別

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

然後定義建造者類別

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

然後可以這樣使用：

```php
$burger = (new BurgerBuilder(14))
                    ->addPepperoni()
                    ->addLettuce()
                    ->addTomato()
                    ->build();
```

**何時使用？**

當某個物件可能會有多種「口味」，或者想避免重疊構造器反模式(telescoping constructor anti-pattern) 時使用。它與工廠模式的主要區別在於：工廠模式適用於建立過程只有一個步驟的情況，而建造者模式適用於建立過程涉及多個步驟的情況。

🐑 原型(Prototype)
------------

現實案例
> 還記得桃莉嗎？那隻複製羊！我們先不要關注細節，但是這裡的重點是複製。

簡單來說
> 根據某個現存的物件，通過複製來建立對象。

Wikipedia 上描述為
> 原型模式是軟體發展中的建立型設計模式。它用於當所需建立的物件的類型是由某個原型實例決定的情況，並通過克隆該原型實例來產生新的物件。

簡單來說，它能讓你建立某個現有物件的複製版本，然後你可按需對其進行修改，從而避免了從新建立一個物件並對其進行設置的所有麻煩。

**程式設計範例**

在 PHP 中, 可以非常容易地使用 `clone` 實現
 
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

然後像下面這樣進行複製

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

另外你也可以通過特殊方法 `__clone` 來定制複製行為。

**何時使用？**

當所需物件和某個現存物件非常相似時，或者當建立操作相比複製花銷更大時。

💍 單例(Singleton)
------------

現實案例
> 一個國家在同一時期只能有一位總統。當需要擔起責任時，都是這位總統實施行動的。這裡總統就是單例。

簡單來說
> 它能確保某個類永遠只能夠建立一個物件。

Wikipedia 上描述為
> 在軟體工程中，單例模式是一種軟體設計模式，它限制某個類只能產生實體成一個物件。當系統中需要確切一個物件來協調行為時，單例是很適合的。

單例模式實際上被認為是一種反模式，因此需避免過度使用。它不一定就是不好的，它有它的適用情況，但是使用時應當當心，因為它在你的程式中引用了一個全域狀態，因此在某處對它的修改可能會影響其它地方，從而對它進行調試會變得相當困難。

**程式設計範例**

建立一個單例，將構造器設為私有，禁用複製功能，禁止擴展，並建立一個靜態變數來保存實例

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

然後這樣使用

```php
$president1 = President::getInstance();
$president2 = President::getInstance();

var_dump($president1 === $president2); // true
```

結構型設計模式
==========================

簡單來說
> 結構型模式主要關注物件的組合或者換句話說是實體間如何能夠相互使用。或者也可以另外解釋為，它們有助於回答 "如何構建一個軟體元件?“。

Wikipedia 上描述為
> 在軟體工程中，結構型設計模式是這樣的一些設計模式，它們通過某種簡明的方式來實現實體間的關係，從而減少設計的難度。
 
 * [適配器(Adapter)](#-適配器adapter)
 * [橋接(Bridge)](#-橋接bridge)
 * [組合(Composite)](#-組合composite)
 * [裝飾器(Decorator)](#-裝飾器decorator)
 * [外觀(Facade)](#-外觀facade)
 * [享元(Flyweight)](#-享元flyweight)
 * [代理(Proxy)](#-代理proxy)

🔌 適配器(Adapter)
-------

現實案例
> 假設你的記憶體卡裡有一些照片，你需要將它們傳到電腦上。要完成傳輸，你需要有與你的電腦介面相容的適配器，這樣你才能將記憶體卡與你的電腦連接。在這種情況下讀卡器就是一個適配器。
> 另一個例子就是大家都知道的電源適配器; 一個三腳插頭無法插到兩口的插座上，需要使用一個電源適配器才能將它與兩口插座連接。
> 還有一個例子就是翻譯，他能將一個人說的話翻譯給另一個聽。

簡單來說
> 適配器模式允許你在適配器中封裝其它不相容的物件，從而使它們與某些類相容。

Wikipedia 上描述為
> 在軟體工程中，適配器這種軟體設計模式允許將現有類的介面轉成另一種介面來使用。它通常用於使現有類在無需修改其源碼的情況下，與其它類實現協作。

**程式設計範例**

假設現有一款關於獵人獵獅的遊戲。

首先定義 `Lion` 介面並實現所有種類的獅子類別

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

獵人只有當看到實現了 `Lion` 介面的獵物後才能狩獵。

```php
class Hunter {
    public function hunt(Lion $lion) {
    }
}
```

現假設我們需要在遊戲中加入 `WildDog`，使獵人對它們也能進行狩獵。但是我們無法直接實現，因為狗具有不同的介面。要使它與我們的獵人相容，我們需要建立一個相容的適配器。
 
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

現在 `WildDog` 可能通過 `WildDogAdapter` 使用到我們的遊戲中了。

```php
$wildDog = new WildDog();
$wildDogAdapter = new WildDogAdapter($wildDog);

$hunter = new Hunter();
$hunter->hunt($wildDogAdapter);
```

🚡 橋接(Bridge)
------

現實案例
> 假設你有一個由多個不同的頁面組成的網站，然後你想讓使用者可以修改頁面主題風格。那麼你會怎麼做？是為每一個頁面針對每一個主題風格都建立一個複本，還是只建立分離的主題風格，然後根據使用者的喜好載入主題風格？如果你想用第二種辦法，那麼橋接模式就是你的解決之道。

![With and without the bridge pattern](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

簡單來說
> 橋接模式認為組合優於繼承。它能將一個層級結構中的實現細節轉到位於另一個分離的層級結構的物件中。

Wikipedia 上描述為
> 橋接模式是軟體設計模式之一，它意在 ”將抽象與真實現分離，從而使它們可以各自獨立的變化“。

**程式設計範例**

實現上面的網站的例子，這裡定義了 `WebPage` 的層級結構

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

然後是另外分離的主題風格層級結構

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

最後將兩個層級結構組合起來

```php
$darkTheme = new DarkTheme();

$about = new About($darkTheme);
$careers = new Careers($darkTheme);

echo $about->getContent(); // "About page in Dark Black";
echo $careers->getContent(); // "Careers page in Dark Black";
```

🌿 組合(Composite)
-----------------

現實案例
> 每個組織都由員工組成。每個員工都有相似的特徵，如都有工資，都擔負一些職責，需要（或者不需要）向某人彙報，有（或者沒有）一些下屬等。

簡單來說
> 組合模式使得客戶能以統一的方式對待每個物件。

Wikipedia 上描述為
> 在軟體工程中，組合模式是一種分割式的設計模式。組合模式描述為：能以和對待單個物件實例相同的方式對待物件的組合。組合為的是將物件組織成樹狀結構，以表達 *部分-整體* 的層級關係。使用組合模式後，客戶就能一致地對待單獨物件和組合體了。

**程式設計範例**

使用上面的員工的例子。這裡定義了不同類型的員工

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

再定義一個組織，它由不同類型的員工組成

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

然後可以這樣使用

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

☕ 裝飾器(Decorator)
-------------

現實案例

> 假設你經營一家能提供多種服務的汽車服務店。現在你怎樣計算要收的費用？你會根據提供了的所有服務，將每項服務費用都動態疊加進去，直到算出總額。這裡每種服務都是一種裝飾器。

簡單來說
> 裝飾器模式通過將物件封裝在裝飾器類的物件中，從而使你能在執行時動態地修改原物件的行為。

Wikipedia 上描述為
> 在物件導向程式設計中，裝飾器這種設計模式允許以靜態或者動態的方式，將行為添加到某個物件中，而這種修改不會影響相同類中的其它實例物件的行為。裝飾器模式通常對於遵循單一職責原則(Single Responsibility Principle)很有用, 因為它允許功能在類間進行劃分，使得各個類只專注各自的功能領域。

**程式設計範例**

以咖啡為例。首先為簡單咖啡實現咖啡介面

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

我們想使程式碼可擴展，允許在需要的時候能夠修改選項。讓我們增加一些添加物（裝飾器）

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

現在可以製作咖啡了

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

📦 外觀(Facade)
----------------

現實案例
> 你是怎樣開電腦的？「按電源鍵」你說！你相信那樣一定可以，這是由於你正在使用電腦外部的一個簡單介面，而其內部則需要完成大量工作才能實現開機。這個針對複雜子系統而設計的簡單介面就是外觀。

簡單來說
> 面板模式為複雜子系統提供一個簡化介面。

Wikipedia 上描述為
> 外觀就是一個物件，它為更大規模的程式碼，如類庫等提供簡化的介面。

**程式設計範例**

使用上面的電腦的例子。現在先定義電腦類

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

這樣定義外觀

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

現在這樣使用外觀

```php
$computer = new ComputerFacade(new Computer());
$computer->turnOn(); // Ouch! Beep beep! Loading.. Ready to be used!
$computer->turnOff(); // Bup bup buzzz! Haah! Zzzzz
```

🍃 享元(Flyweight)
---------

現實案例
> 你有過在攤位上品嘗過新茶嗎？他們通常沏出比你所要的還要多的杯數，然後將多餘的荼留給其他客人，從而起到節約資源（如燃氣）的目的。享元模式的全部即共用。

簡單來說
> 它能使相似物件間通過盡可能多地共用，以減少記憶體使用和計算花銷。

Wikipedia 上描述為
> 在電腦程式設計中，享元是一種軟體設計模式。一個享元就是一個物件，它通過與其它相似物件共用盡可能多的資料，以達到對記憶體的最少化使用;它適用于物件數量龐大的情況，此時簡單地重複表示將需要過量的記憶體量。

**程式設計範例**

實現以上的茶的例子。首先定義各種茶和茶藝師

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

然後定義 `TeaShop`，提供飲茶服務

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

現實案例
> 你有用過門禁卡開過門嗎？打開門有多種方式，比如使用門禁門或者使用密碼鎖等。門的主要功能是打門，但在此之上還加了個代理，它增加了額外的一些功能。讓我們使用以下的程式碼範例來更好地解釋。

簡單來說
> 使用代理模式，一個類可以代表其它類別的功能。

Wikipedia 上描述為
> 一個代理，其最一般的形式，就是作為其它類的介面的一個類。代理就是一個包裝或仲介物件，客戶通過調用它來訪問幕後真正提供服務的物件。使用代理可以簡單地轉發到真實物件，也可以提供額外的邏輯。代理可以提供這些額外的功能，例如當在真實物件上的操作需要大量資源時進行緩存，或者對真實物件調用操作時先檢查先決條件等。

**程式設計範例**

使用以上的安全門的例子。首先定義門的介面並實現門的類別

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

然後定義代理，為門提供安全措施

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

這裡是如何使用

```php
$door = new Security(new LabDoor());
$door->open('invalid'); // Big no! It ain't possible.

$door->open('$ecr@t'); // Opening lab door
$door->close(); // Closing lab door
```

另一個例子是一些資料映射(data-mapper)的實現。例如，我（原作者）使用該模式為 MongoDB 做了一個物件資料映射器(ODM, Object Data Mapper)，我通過在 mongo 類外編寫一個代理來調用特殊函數 `__call()`。對代理的所有方法函數都轉到原 mongo 類上，並且取得的結果也都原樣返回，除了 `find` 或 `findOne` 的資料會映射成所需的類物件並返回，而不以 `Cursor` 返回。

行為型設計模式
==========================

簡單來說
> 它關注於對象間的職責分配。它們與結構型模式的區別在於：它們不只指定結構，還勾畫出了相互間的消息傳送/通信模式。或者也可以說，它們有助於回答「如何在軟體元件中實現行為？」

Wikipedia 上描述為
> 在軟體工程中，行為型設計模式標識了物件間的常見通信模式及其實現模式。通過這樣，這些模式增加了實施此種通信的靈活性。

* [責任鏈(Chain of Responsibility)](#-責任鏈chain-of-responsibility)
* [命令(Command)](#-命令command)
* [反覆運算器(Iterator)](#-反覆運算器iterator)
* [仲介者(Mediator)](#-仲介者mediator)
* [備忘錄(Memento)](#-備忘錄memento)
* [觀察者(Observer)](#-觀察者observer)
* [訪問者(Visitor)](#-訪問者visitor)
* [策略(Strategy)](#-策略strategy)
* [狀態(State)](#-狀態state)
* [範本方法(Template Method)](#-範本方法template-method)

🔗 責任鏈(Chain of Responsibility)
-----------------------

現實案例
> 例如，你在你的帳戶中設置了三種支付方式 (`A`, `B` 和 `C`); 每個裡面都存有不同的金額。`A` 裡面有 100 元，`B` 裡面有 300 元以及 `C` 裡面有 1000 元，並且你的支付偏好選擇為先 `A` 再 `B` 最後 `C`。你想要購買價格為 210 元的商品。如何使用責任鏈，那麼首先會檢查帳戶 `A`，看它是否足夠支付，如何可以那麼完成支付並且鏈條到此結束。如果不能，那麼請求將傳向檢查帳戶 `B` 中的金額，如果足夠那麼鏈條到此結構否則請求將繼續傳遞直到找到合適的處理帳戶。這裡 `A`, `B` 和 `C` 都是鏈條中的連結點，而這整個現象就是責任鏈。

簡單來說
> 它用於建立一個物件鏈。請求從一端進入，並從一個物件傳遞到另一個，直至找到適合的處理物件。

Wikipedia 上描述為
> 在物件導向設計中，責任鏈設計模式由命令物件源和一系列的處理物件組成。每個處理物件中都包含有邏輯，用來定義它能處置的命令物件類型; 那些不能處置的其它命令物件都將傳遞給鏈條中的下一個處理物件。

**程式設計範例**

實現上面的帳戶的例子。首先定義一個基本的帳戶類，其中包含有將各帳戶關聯起來的邏輯功能，然後再實現幾種帳戶。

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

現在用上面定義的帳戶（如 Bank, Paypal, Bitcoin) 準備一個鏈條

```php
// 建立如下的一個連結
//      $bank->$paypal->$bitcoin
//
// 優先使用銀行帳戶
//      如果銀行帳戶無法支付再用 paypal
//      如果 paypal 不能支持再用比特幣

$bank = new Bank(100);          // Bank with balance 100
$paypal = new Paypal(200);      // Paypal with balance 200
$bitcoin = new Bitcoin(300);    // Bitcoin with balance 300

$bank->setNext($paypal);
$paypal->setNext($bitcoin);

// Let's try to pay using the first priority i.e. bank
$bank->pay(259);

// 輸出會是
// ==============
// Cannot pay using bank. Proceeding ..
// Cannot pay using paypal. Proceeding ..: 
// Paid 259 using Bitcoin!
```

👮 命令(Command)
-------

現實案例
> 一個常見的例子就是在餐廳吃飯。你（即 `客戶 Client`）要求服務員（即 `調用者 Invoker`）上菜（即 `命令 Command`），而服務員只是簡單地將你的請求傳達給廚師（即 `接收者 Receiver`），廚師知道做哪道菜及如何做。
> 另一個例子是你（即 `客戶 Client`）使用遙控器（即 `調用者 Invoker`）打開（即 `命令 Command`）電視機（即 `接收者 Receiver`）。

簡單來說
> 它允許你在對象中封裝行為。該模式背後的主要思想是：提供將客戶與接收者解耦的方法。

Wikipedia 上描述為
> 在物件導向程式設計中，命令模式是一種行為型設計模式，它用物件來封裝執行動作或稍後觸發事件所需的所有資訊。這些資訊包括方法名，擁有該方法的物件以及方法參數值等。

**程式設計範例**

首先定義接收者，並實現它支持的每個行為

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

然後定義每個命令都需要實現的介面，並實現一組命令

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

再定義一個 `調用者 Invoker`，客戶與它交互來處理任何命令

```php
// Invoker
class RemoteControl {
    
    public function submit(Command $command) {
        $command->execute();
    }
}
```

最後看下客戶如何使用

```php
$bulb = new Bulb();

$turnOn = new TurnOn($bulb);
$turnOff = new TurnOff($bulb);

$remote = new RemoteControl();
$remote->submit($turnOn); // Bulb has been lit!
$remote->submit($turnOff); // Darkness!
```

命令模式也可用以實現事務型系統。當一旦執行命令後就保存其執行記錄。如果最後一個命令也執行成功了，那樣很好，否則只需遍歷歷史記錄，並在所有完成了的命令上執行 `undo` 即可。

➿ 反覆運算器(Iterator)
--------

現實案例
> 老式收音機是反覆運算器的很好的例子，使用者可以先從某個頻道開始，然後使用前後按鍵來遍歷各個頻道。或者以 MP3 播放機或電視機為例，它們也可以使用前後按鍵來遍歷歌曲或頻道。換句話說，它們都提供了一個介面，來遍歷頻道，歌曲或廣播電臺。

簡單來說
> 它提供了一種訪問物件內所有元素的方法，而避免暴露低層的標記法。

Wikipedia 上描述為
> 在物件導向程式設計中，反覆運算器模式是一個設計模式，它使用反覆運算器來遍歷容器並訪問容器內的元素。反覆運算器模式將演算法和容器進行瞭解耦; 但在某些情況下，演算法必需是特定於容器的，因而無法解耦。

**程式設計範例**

在 PHP 中很容易使用 SPL (標準 PHP 庫) 來實現。實現上面的廣播電臺的例子。首先我們定義 `RadioStation`

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

然後定義反覆運算器

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

然後可以這樣使用

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

👽 仲介者(Mediator)
========

現實案例
> 一個常見的例子就是當你用手機與別人通話時，你們之間隔有一個網路服務提供商，你們的通話是要通過它，而不是直接傳送的。在這種情況下網路服務提供商就是一個仲介者。

簡單來說
> 仲介者模式引入了一個協力廠商物件（叫仲介者 mediator) 來控制兩個物件（叫同事 colleagues) 間的交互。它有助於減少彼此通信的類間的耦合性。因為現在它們無需瞭解對方的實現細節。

Wikipedia 上描述為
> 在軟體工程中，仲介者模式定義了一個物件，它對一組物件如何交互進行了封裝。這種模式被認為是一種行為型模式，因為它能改變程式運行時的行為。

**程式設計範例**

這裡是一個聊天室（即仲介者）的最簡單的例子，其中的用戶（即同事）之間會相互發送消息。

首先，我們定義仲介者（即聊天室）

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

然後定義用戶（即同事）

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

💾 備忘錄(Memento)
-------

現實案例
> 以計算器（即發起人 originator）為例，當你完成計算後，最後的結果會被保存在記憶體（即備忘錄 memento）中，那樣你就能取回它，或許也可以通過一些功能按鍵（即管理者 caretaker）來恢復它。

簡單來說
> 備忘錄模式就是關於用某種方式獲取或保存物件當前狀態的模式，從而使物件能在稍後順利恢復。

Wikipedia 上描述為
> 備忘錄模式是一種軟體設計模式，它提供了將物件恢復到先前狀態的能力（使用回滾來撤銷操作）。

通常當你需要提供一些撤銷功能時非常有用。

**程式設計範例**

以文字編輯器為例，它會不時地保存當前狀態，從而當你需要時可以恢復。

首先定義我們的備忘錄物件，它能用於保存編輯器的狀態

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

然後定義編輯器（即發起人 originator），它會用到備忘錄對象

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

然後可以這樣使用

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

😎 觀察者(Observer)
--------

現實案例
> 一個不錯的案例是求職者，他們訂閱到一些職位發佈網站，然後當出現匹配的工作機會時，他們就會得到通知。

簡單來說
> 它在物件間定義了一種依賴關係，從而當某個物件的狀態改變後，它的所有依賴物件都將得到通知。

Wikipedia 上描述為
> 觀察者模式是一種軟體設計模式，其內的一個物件（稱為主題），會維護一組依賴物件（稱為觀察者），當物件的狀態改變後，它通常通過調用依賴物件的某個函數來自動通知它們。

**程式設計範例**

實現以上的例子。首先定義求職者，他需要得到工作職位的發佈通知

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

再定義工作職位發佈網站，求職者將會訂閱

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

然後這樣使用

```php
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

🏃 訪問者(Visitor)
-------

現實案例
> 考慮到杜拜旅遊的例子。遊客只需通過某種途徑（例如簽證）進入杜拜。抵達後，他們就可以自己去參觀杜拜的任何地方，要參觀這裡的任何一個地方，都無需再獲得許可或做一些跑腿的工作; 只需告訴他們地址，他們就能去參觀。訪問者模式也允許你那樣做，它能幫你增加要前往的地點，從而使你能參觀盡可能多的地方，而無需另做額外的工作。

簡單來說
> 訪問者模式允許你無需進行修改就能將進一步的操作增加到物件中。
 
Wikipedia 上描述為
> 在物件導向程式設計和軟體工程中，訪問者設計模式是將演算法與其所操作的物件結構進行分離的一種方法。這種分離的實際結果是：具有在不修改現有物件結構的情況下，將新操作加入到物件結構中的能力。

**程式設計範例**

以一個模擬動物園為例，裡面有多種動物，並且我們需要它們發出聲音。我們將用訪問者模式實現這個例子

```php
// 被訪問者
interface Animal {
    public function accept(AnimalOperation $operation);
}

// 訪問者
interface AnimalOperation {
    public function visitMonkey(Monkey $monkey);
    public function visitLion(Lion $lion);
    public function visitDolphin(Dolphin $dolphin);
}
```

再實現一些動物類別

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

實現訪問者

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

然後可以這樣使用

```php
$monkey = new Monkey();
$lion = new Lion();
$dolphin = new Dolphin();

$speak = new Speak();

$monkey->accept($speak);    // Ooh oo aa aa!    
$lion->accept($speak);      // Roaaar!
$dolphin->accept($speak);   // Tuut tutt tuutt!
```

我們本可以只通過動物類的繼承結構實現上面的功能，但是那麼的話，當我們需要將新動作增加到動物類別時，就必須修改動物類別。而現在我們無需修改它們。例如，當要求將跳的行為加入動物類別時，我們只需簡單地建立一個新訪問者即可。

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

再這樣使用

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

現實案例
> 考慮排序的例子，我們實現了冒泡排序，但是隨著資料增多，冒泡排序變得越來越慢。為了解決這個問題，我們又實現了快速排序。但是現在雖然快速排序演算法在大資料集中運行很好，在小資料集上卻很慢。為了處理這種情況，我們實現了一種策略：小資料集時用冒泡排序，大資料集時用快速排序。

簡單來說
> 策略模式允許你根據情況切換演算法或策略。

Wikipedia 上描述為
> 在電腦程式設計中，策略模式（也稱為政策模式）是一種行為型軟體設計模式，它使得能在運行時選擇演算法的行為。
 
**程式設計範例**

實現上面的例子。首先定義策略介面，並實現不同的策略

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
 
然後定義客戶，它能使用任何一個策略

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

然後這樣使用

```php
$dataset = [1, 5, 4, 3, 2, 8];

$sorter = new Sorter(new BubbleSortStrategy());
$sorter->sort($dataset); // Output : Sorting using bubble sort

$sorter = new Sorter(new QuickSortStrategy());
$sorter->sort($dataset); // Output : Sorting using quick sort
```

💢 狀態(State)
-----

現實案例
> 假設你正在使用繪畫程式，你選擇畫筆繪畫。現在畫筆會根據所選的顏色改變其行為，比如當你選擇紅色後它將畫出紅色，選擇藍色後將畫出藍色等。

簡單來說
> 它能使你在狀態改變後修改類的行為。

Wikipedia 上描述為
> 狀態模式是一種行為型軟體設計模式，它用物件導向的方式實現了一個狀態機。在狀態模式中，通過將每個單獨狀態實現為狀態模式介面的一個繼承類，而狀態間的轉變通過調用在模式的父類中定義的函數來實現，從而實現一個狀態機。
> 狀態模式可以解釋為是一種策略模式，它能通過調用在模式介面中定義的方法來切換當前策略。

**程式設計範例**

以文字編輯器為例，它能讓我們修改輸入文字的狀態，比如選擇粗體後，它就會用粗體書寫，選擇斜體就會用斜體等。

首先定義狀態介面，並實現一些狀態類

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

再定義編輯器

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

然後這樣使用

```php
$editor = new TextEditor(new Default());

$editor->type('First line');

$editor->setState(new UpperState());

$editor->type('Second line');
$editor->type('Third line');

$editor->setState(new LowerState());

$editor->type('Fourth line');
$editor->type('Fifth line');

// Output:
// First line
// SECOND LINE
// THIRD LINE
// fourth line
// fifth line
```

📒 範本方法(Template Method)
---------------

現實案例
> 假設我們蓋房子，蓋房子的步驟看起來像這樣:
> - 打地基
> - 砌牆
> - 蓋屋頂
> - 加蓋其它層
> 
> 這些步驟的順序永遠都不會變，即你不可能先蓋屋頂再砌牆等。但是每一步的具體操作都是可以修改的，比如說，你可以砌木牆，聚酯纖維牆或者石頭牆。
 
簡單來說
> 範本方法定義了某特定演算法如何執行的框架，但執行步驟的具體實現則推遲到子類中去完成。
 
Wikipedia 上描述為
> 在軟體工程中，範本方法是行為型設計模式的一種，它定義了操作中的某個演算法的程式框架，並將一些步驟推遲到子類中去實現。它能在不修改演算法結構的情況下，重新定義演算法中的某些步驟。

**程式設計範例**

假設我們有一個構建工具，它能幫助我們進行測試，程式碼檢查，構建，生成構建報告（比如程式碼覆蓋率報告，程式碼檢查報告等）以及部署應用至測試伺服器。

首先我們定義一個基類用來指定構建演算法的框架

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

然後進行各種實現

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

接著，可以這樣使用

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


專案進度
======

- [x] 簡介 (2017-02-24)
- [x] 建立型設計模式 - 簡單工廠 (2017-02-24)
- [x] 建立型設計模式 - 工廠方法 (2017-02-24)
- [x] 建立型設計模式 - 抽象工廠 (2017-02-25)
- [x] 建立型設計模式 - 建造者 (2017-02-25)
- [x] 建立型設計模式 - 原型 (2017-02-25)
- [x] 建立型設計模式 - 單例 (2017-02-25)
- [x] 結構型設計模式 - 適配器 (2017-02-25)
- [x] 結構型設計模式 - 橋接 (2017-02-26，由 [DashShen](https://github.com/DashShen) 翻譯，[haiiiiiyun](https://github.com/haiiiiiyun/) 校審)
- [x] 結構型設計模式 - 組合 (2017-02-26)
- [x] 結構型設計模式 - 裝飾器 (2017-02-26)
- [x] 結構型設計模式 - 外觀 (2017-02-26)
- [x] 結構型設計模式 - 享元 (2017-02-27)
- [x] 結構型設計模式 - 代理 (2017-02-27)
- [x] 行為型設計模式 - 責任鏈 (2017-02-27)
- [x] 行為型設計模式 - 命令 (2017-02-27)
- [x] 行為型設計模式 - 反覆運算器 (2017-02-27)
- [x] 行為型設計模式 - 仲介者 (2017-02-27)
- [x] 行為型設計模式 - 備忘錄 (2017-02-27)
- [x] 行為型設計模式 - 觀察者 (2017-02-27)
- [x] 行為型設計模式 - 訪問者 (2017-02-28)
- [x] 行為型設計模式 - 策略 (2017-02-27)
- [x] 行為型設計模式 - 狀態 (2017-02-27)
- [x] 行為型設計模式 - 範本方法 (2017-02-27，由 [DashShen](https://github.com/DashShen) 翻譯，[haiiiiiyun](https://github.com/haiiiiiyun/) 校審)

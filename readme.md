# 太空船與遊戲場景

### 變數、常數、方法（難度：★☆☆☆）：
#### 我們想設計一個簡單的遊戲架構：一艘太空船類別、一個太空的遊戲場景，然後太空船再遊戲場景上面飛行、並且可以發射飛彈。Objective-C 的程式碼如下：

圖片1：SpaceShip.h

![SpaceShip.h](spaceShip_h.png)

圖片2：SpaceShip.m

![SpaceShip.m](spaceShip_m.png)

#### 問題1：請問「圖片1」，第 28 行的 ```+(NSString *)getManufacturerName;``` 以及第 33 行的 ```-(id)init;``` 中 + 和 - 的用法有什麼差別？
<br>
<br>
<br>

#### 問題2：承問題1，請完成「圖片2」第 20 行的 ```+(NSString *)getManufacturerName;``` 的內容。（ PS：廠商名稱可以用「圖片2」定義好的 NSString 來使用。 ）
<br>
<br>
<br>

圖片3: SpaceScene01.h

![SpaceScene01.h](spaceScene01_h.png)

圖片4: SpaceScene01.m

![SpaceScene01.m](spaceScene01_m.png)

#### 問題3：請完成「圖片4」內的第 18 行，太空船初始化。
<br>
<br>
<br>

#### 問題4：「圖片2」 的第12行，應該也可以寫成 ```#define``` 吧？雖然寫成 ```#define``` 也是可行的，但使用 ```static NSString *const``` 的好處是什麼？讓我們有會偏好使用 ```static NSString *const```更勝過  ```#define``` ？ 
<br>
<br>
<br>

#### 問題4.1：（加分題）同上，那麼反過來，使用 ```#define``` 也有他的好處，如果知道原因的話，說明一下使用 ```#define``` 的好處。（ 提示：遊戲製作上常常會用到的技巧。 ）
<br>
<br>
<br>

### Protocol、Delegate、Extension（難度：★★☆☆）：

#### 我們可以從「圖片1」看到 SpaceShip （ 太空船 ）有 bomb1 以及 bomb2 、兩顆炸彈的 property ，既然有炸彈，因此就可以發射炸彈，對吧？當玩家按下發射彈按鈕的時候，遊戲場景會出現飛彈物件發射出去，那是因為我們在遊戲場景上寫了個方法，讓飛機物件裡的砲彈物件飛移動。所以我們可以把炸彈移動 ```bombFlying``` 這個方法寫死在 SpaceScene01 （太空場景）上，然後讓飛彈從飛機發射出去後，飛彈就可以在太空場景上移動。
#### 但因為太空船可能除了 SpaceScene01 這張地圖以外，還可能運用在其他場景（打飛機遊戲總不能只有一個場景吧？），或是我們太空場景這次不是發射炸彈，而是發射雷射，那麼這個寫死的方法是不是就要拿掉不能用了？因此我們希望每個場景，在加入太空船後，都會「被動的」「被要求」去實作太空船本身的 ```bombFlying``` 這個方法，以達成類別與類別之間「耦合度降低」、又能達成類別可以「重複利用」的效果......

#### 問題1：接續上面的描述。因此在 Objective-C 中，我們都是怎麼做到「耦合度降低」以及「重複利用」的？簡單說明他的設計模式或是他的作法即可。（ 提示1：和「圖1」的第 28 行的 delegate 有關。 ）（ 提示2：官方一般 UI 元件都是用此方法設計的 ）
<br>
<br>
<br>

#### 問題2：接續問題1，請簡易的寫出如何實作的程式碼。

```

#import <SpriteKit/SpriteKit.h>

/**
 * @brief - 請在此實作 
 */
/************************/
// TODO: 降低耦合度
@___________ SpaceShipDelegate <NSObject>

// 飛彈移動的方法，可以使用題目給的方法名稱。
____________________;

@___

/**
 * @brief - 太空船
 * @properties - bomb1 : 一號飛彈
 * @properties - bomb2 : 二號飛彈
 * @properties - delegate : 
 */
@interface SpaceShip : SKSpriteNode

@property (nonatomic , strong) SKSpriteNode *bomb1;
@property (nonatomic , strong) SKSpriteNode *bomb2;

/************************/
// TODO: 降低耦合度
@property (nonatomic , weak) id <______________>delegate;

/**
 * @brief - 取得製造廠商名字
 */
+(NSString *)getManufacturerName;

/** 
 * @brief -
 */
-(id)init;

-(void)fireBomb:(int)bombIndex;

@end

```


```
#import "SpaceShip.h"

// 製造廠商
static NSString *const kManufacturer = "NASA ©";

@implementation SpaceShip

...

-(void)fireBomb:(int)bombIndex{
	[_delegate _____________________];
}

@end
```

遊戲場景

```
#import "SpaceScene01.h"

/************************/
// TODO: 降低耦合度
@interface SpaceScene01() < ________________ >
@end

@implementation SpaceScene01

-(instancetype)init{
    self = [super init];
    if( self ){
        
        // 問題：太空船初始化
        self.playerSpaceShip = 
        [self addChild:self.playerSpaceShip];
        
        // 問題：
        self.playerSpaceShip.delegate = 
        
    }
    return self;
}

/************************/
// TODO: 降低耦合度

@end

```


#### 問題3：請問什麼是 delegate？什麼是 id ？為何 delegate 要用 weak？

#### 問題3.1：（加分題）除了 delegate （ 代理 ）外，其實還有 Objective-C 的 block 的方式來解偶，可以簡單描述一下要怎麼達成？相較於 delegate 使用 block 又有什麼優缺點？
<br>
<br>
<br>

#### 問題3.2：（加分題）同上、那麼使用 delegate 又有什麼好處、與壞處（與 Block 相比較的話）？
<br>
<br>
<br>

#### 問題4：你知道 objective-c 的 category 嗎？簡單的說明一下 category 、以及他可以達成什麼樣的功效？你什麼時候會想要使用 category。
<br>
<br>
<br>

#### 問題4.1：（加分題）Swift 的 extension ( 也就是 Obj-C 的 category ) 跟 Objective-C 使用 category 兩個其實都能達成相同的作用，但 swift 的 extension 使用有什麼的限制？該限制有沒有什麼方式來解決？
<br>
<br>
<br>


### Framework / Library（難度：★★★☆）(加分題）：

#### 問題1：一般來說 Apple 的 library 的檔案格式是 *.a 、 *.framwrork ，這兩個有什麼差別？
<br>
<br>
<br>

#### 問題2：依據您的經驗，打包 framework 時，最常遇到的問題是什麼？怎麼解決？
<br>
<br>
<br>

#### 問題3：我們常常會需要邊開發主專案、一邊測試 Framework 修正的項目，但因為打包 Framework 會花很多時間，要及時更改也很麻煩，一般來說我們會用什麼方法來邊開發專案邊修改 Framework 的內容？
<br>
<br>
<br>

#### 問題4：你再建立 Framework 或是 Library 時，有遇到什麼狀況嗎？分享一下。（可口頭詳聊）

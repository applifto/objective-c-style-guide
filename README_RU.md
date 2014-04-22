# Applifto Objective-C Style Guide

Рекомендации по написанию кода в iOS команде Applifto.

## Вступление
Ниже представлены рекомендации Apple по оформлению кода. Если какие-то моменты не упомянуты в этом документе, то они скорее всего подробно описаны в одной следующих статей:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Содержание

* [Точечная нотация](#Точечная-нотация)
* [Отступы](#отступы)
* [Условные операторы](#Условные-операторы)
  * [Тернарный оператор](#Тернарный-оператор)
* [Методы](#Методы)
* [Переменные](#Переменные)
* [Именование переменных](#Именование-переменных)
  * [Нижние подчеркивания](#Нижние-подчеркивания)
* [Комментарии](#Комментарии)
* [Init и Dealloc](#init-и-dealloc)
* [Литералы](#Литералы)
* [Методы класса CGRect](#Методы-класса-CGRect)
* [Константы](#Константы)
* [Перечислительные типы](#Перечислительные-типы)
* [Приватные свойства](#Приватные-свойства)
* [Именование картинок](#Именование-картинок)
* [Булевский тип](#Булевский-тип)
* [Синглтоны](#Синглтоны)
* [Организация проектов в Xcode](#Организация-проектов-в-Xcode)

## Точечная нотация

Точечная нотация **всегда** должна использоваться для доступа и изменения значений свойств. Скобочная нотация предпочтительна во всех остальных случаях.

**Например:**  
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**А не:**

```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Отступы

* Делайте отступы 4 пробелами. Никогда не используйте табуляцию. Проверьте, что соответствующие настройки выставлены в Xcode.
* Все фигурные скобки (`if`/`else`/`switch`/`while` etc.)  всегда открываются на той же линии, что и выражение, и закрываются на новой строке.

**Например:**  
```objc
if (user.isHappy) {
//Do something
} else {
//Do something else
}
```

* Между методами должна быть ровно одна пустая строка. Это добавит вашему коду чистоты и читабельности. Пустая строка внутри метода должна разделять функциональность. В таких случаях задумайтесь над созданием отдельного метода.
* `@synthesize` и `@dynamic` должны быть объявлены с новой строки.

## Условные операторы

Тело условного оператора должно всегда содержаться внутри фигурных скобок даже в том случае, когда можно обойтись без них (например, если тело состоит из одной строки). Это делается в целях предотвращения [ошибок](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). Примером такой ошибки может служить добавление строки в тело условного оператора. Другой, [более опасный побочный эффект](http://programmers.stackexchange.com/a/16530), может возникнуть, когда строку, находящуюся "внутри" условного оператора комментируют и следующая за ней строка невольно становится частью предшествуюего ей условного оператора. Вдобавок, такой стиль является более совместимым с другими условными операторами, а потому более читабельный. 


**Например:**
```objc
if (!error) {
    return success;
}
```

**А не:**
```objc
if (!error)
    return success;
```

или  

```objc
if (!error) return success;
```

### Тернарный оператор

Тернарный оператор, ? , должен использоваться только в тех случаях, когда он увеличивет аккуратность и прозрачность кода. В большинстве случаев он должен вычислять только одно выражение. При необходимости работы с несколькими выражениями лучше использовать условный оперратор или заранее сохранить необходимые значения.

заранее вычислить необходимые значения и сохранить их значения в переменные.
Evaluating multiple conditions is usually more understandable as an if statement, or refactored into instance variables.

**Например:**
```objc
result = a > b ? x : y;
```

**А не:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Методы

В прототипе метода после индикатора scope(доступа-области) (-/+) должен идти пробел. Также один пробел должен присутствовать между частями имени метода.

**Например**:  
```objc  
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```
## Переменные

Переменные должны носить как можно более описательные имена. Нужно избегать однобуквенных переменных, за исключением итераторов в цикле `for()`.

При объявлении указателей, звездочки должны относиться к имени переменной, например, `NSString *text`, а не `NSString* text` или `NSString * text`. Исключения составляют объявления констант.

Старайтесь по возможности использовать свойства вместо переменных экземпляра (instance variable). Прямой доступ к переменным экземпляра допускается лишь в методах инициализации (`init`, `initWithCoder:`, и пр.), методах `dealloc` и кастомных геттерах и сеттерах. Более подробная информация об аксессорах (getter/setters) доступна по [ссылке](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Например:**  

```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**А не:**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

## Именование переменных

Необходимо придерживаться концепций именования переменных, предложенных Apple, особенно в отношении [правил работы с памятью](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Поощряется использование длинных и описательных имен переменных.

**Например:**  

```objc
UIButton *settingsButton;
```

**А не:**  

```objc
UIButton *setBut;
```

Трехбуквенные префиксы (например `NYT`) должны всегда использоваться для именования классов и констант. Однако их можно опускать при работе с сущностями CoreData. Имена констант должны быть описаны в стиле UpperCamelCase, когда каждое слово начинается с заглавной буквы. Для ясности в префиксе должно быть указано имя класса, к которому они относятся.


**Например:**  

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**А не:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Имена свойств должны быть описаны в стиле lowerCamelCase, когда первое слово в названии начинается с маленькой буквы. **Если Xcode может самостоятельно создать переменную, то не мешайте ему, пусть создает.** Если вы-таки создаете переменную объекта самостоятельно, то первое слово в имени должно начинаться с маленькой буквы, а имени всей переменной должен предшествовать знак нижнего подчеркивания. Именно такой формат используется в Xcode при автоматическом создании переменных.

**Например:**  

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**А не:**

```objc
id varnm;
```

### Нижние подчеркивания

При использовании свойств, доступ к ним должен осуществляться через `self.`. В таком случае все свойства будут визуально выделяться, т.к. перед каждым свойством будет присутствоать `self.`. Локальные переменные не должны содержать нижнего подчеркивания.

## Комментарии

Комментарии, если они необходимы, должны пояснять **почему** комментируемый кусок кода делает что-либо. Все используемые комментарии должны быть актуальными, в противном случае их следует удалять.

Старайтесь по возможности избегать больших блоков комментариев. Ваш код должен быть самодокументированым. При необходимости допускаются поясняющие комментарии в несколько строк. Это правило не относится к комментариям, используемым для создания документации. 


## init и dealloc

Методы `dealloc` должны находиться в самом верху реализции класса, сразу после `@synthesize` и `@dynamic`.
Группа методов `init` помещается сразу после `dealloc`.

Методы `init` должны быть оформлены следующим образом: 

```objc
- (instancetype)init {
    self = [super init]; // или вызов designated initalizer
    if (self) {
        // Custom initialization    
    }
    return self;
}
```


## Литералы

`NSString`, `NSDictionary`, `NSArray`, и `NSNumber` должны использоваться в случае создания неизменяемых (immutable) обектов соответствующих классов. Обратите внимание, что `nil` не может храниться ни в `NSArray`, ни в `NSDictionary`. Это приведет к ошибке.


**Например:**  

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**А не:**  

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *ZIPCode = [NSNumber numberWithInteger:10018];
```

## Методы класса CGRect

При работе со свойствами `x`, `y`, `width`, или `height` класса `CGRect` всегда используйте [методы `CGGeometry`](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) вместо прямого доступа к значениям свойств. Из документации Apple:


> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Например:**  

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**А не:**  

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Константы

При выборе между константами и in-line значенями строк и чисел предпочтение отдается константам. Их удобнее использовать при необходимости частых повторяющихся вычислений, их значения можно быстро изменить без необходимости редактирования всего проекта. Объявляйте константы с использованием `static`, а не `define`. Исключение составляют явные объявления макросов. 


**Например:**  

```objc
static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";  

static const CGFloat NYTImageThumbnailHeight = 50.0;
```

**А не:**  

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## Перечислительные типы

При использовании `enum`-ов рекомендуется использование нового (iOS 6/Mac OS X 10.8) макроса `NS_ENUM()`. При этом используется более строгий контроль типов и автодополнение кода. 

**Например:**  

```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

## Приватные свойства

Приватные свойства должны объявляться в расширениях класса (анонимные категории) внутри файла реализации. Именованые категории (`NYTPrivate` или	 `private`) никогда не должны использоваться, за исключением случаев расширения внешних классов. 

**Например:**  

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Именование картинок

Картинки должны именоваться так, чтобы поддерживать общую организацию проекта, а также отражать высокий уровень здравомыслия разработчика.  Имя картинки должно представлять собой одну строку в стиле CamelCase, содержать описание своего назначения, имя соответствующего класса, цвет/положение, состояние. 


**Например:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` и `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` и `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`. 

Картинки, используемые для одной цели должны быть сгруппированы соответствующим образом в папке Images. 

## Булевский тип

Так как  `nil` соответствует `NO`, то необходимость сравнивать эти значения отпадает. Никогда проводите сравнение напрямую с `YES`, т.к. `YES` определяется как 1, а переменная типа `BOOL` может содержать в себе до 8 бит. 

Также эти рекомендации повысят визуальную чистоту вашего кода.

**Например:**  

```objc
if (!someObject) {
}
```
		
**А не:**  

```objc
if (someObject == nil) {
}
```

-----

**Примеры использования `BOOL`**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**А не:** 

```objc 
if ([someObject boolValue] == NO)
if (isAwesome == YES) // Never do this.
```

-----

Если имя `BOOL` свойства выражено прилагательным, то префикс “is” в имени свойства можно опустить, но необходимо указать имя соответствующего геттера, например:


```objc
@property (assign, getter=isEditable) BOOL editable;
```

Текст и пример взяты из [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Синглтоны

Объекты-синглтоны должны использовать следующий поточно-безопасный паттерн для получения экземпляра объекта.

Singleton objects should use a thread-safe pattern for creating their shared instance.
```objc
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```
Использование этого паттерна предотвратит [большое количество возможных ошибок](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).


## Организация проектов в Xcode

Физические файлы должны быть синхронизированы с проектом Xcode во избежание беспорядка в файлах проекта. Каждой группе файлов в Xcode должна соответствовать папка в файловой системе. Файлы должны быть сгрупированы не только по типу, но и по их функциональности.

При возможности всегда выставляйте флаг "Treat Warnings as Errors" внутри Build Settings и выставляйте как можно больше [дополнительных предупреждений](http://boredzo.org/blog/archives/2009-11-07/warnings). Если вам необходимо игнорировать определенное предупреждение, используйте [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).


# Другие рекомендации по оформлению кода на Objective-C

Если приведенные выше рекомендации пришлись вам не по душе, взгляните на следующие статьи:

* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)

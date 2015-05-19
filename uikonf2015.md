# Lesssons

^ Hello! So I'm pretty poor at naming, and I figured what I lack in creativity, I hopefully can make up for in clarity. Today I am going to be talking about Lessons I learned.

---

# in Swift

^ In our favorite new programming language Swift

---

# through Haskell

^ through a purely functional programming language you've probably heard of called Haskell! 

--- 

# Me

  * Joe Burgess
  * iOS course at The Flatiron School
  * 6 semesters!

^ Before I go any further though, let me introduce myself. My name is Joe Burgess and I started and ran the iOS course at The Flatiron School. The Flatiron School is a school in New York City that teaches Adults through full time immersive programs in Web and iOS as well as High School students with After School and Summer programs. We've been doing this for a while now! It's going to be my 6th semester coming up in a few weeks.

---

# [fit]Why?!

![](http://thechicagoimprovden.com/wp-content/uploads/2014/01/blog-one-pic.png)

^ Part of teaching, I've been making the transition to teaching swift. We are still mostly Objective-C more and more students are starting to write code in Swift. As students do this, I am getting questions in Swift.

---

# [fit]I Don't Want<br> To Be A N00B

^ And I don't want to be a noobie. Students are quitting their jobs, paying money for my advice and guidance. 

---

# New Features!

^ Students are discovering a lot of the new features of swift

---

# New Questions!

^ And with these new features comes a plethora of new questions.

---

# New "Best Answer"

^ Which have a new Best Answer

---

# Objects

![fit](biased.jpg)

^ Up until now my best answers have always been based around objects. I've been coding in an Object Orientated language for all of my career! A lot of the features of swift like first class functions, maping, a strong type system and others are pretty new to me. I spent some time in ruby with a few of these but almost everything I've done has been based in Objects

----

# The Deep End

![fit](http://4.bp.blogspot.com/-jCGh64uO-28/VBmz4HegKaI/AAAAAAAADr4/1h58Ve9JiD0/s1600/lyah.png)

^ In an attempt to not bias my students, and have them leave the school poised to understand the trends that are coming I went into the deep end. In the same way learning Java forced me into objects, I wanted Haskell to force me to learn how to solve problems in an FP style. Even though Swift is not a purely functional language. Which you can really feel because it doesn't have a super easy way to separate a collection into it's first element and another collection of the rest of the elements. Knowing how to do things functionally would provide me the tools to give informed answers.

---

# [fit]How do I solve <br>Problems with FP

^ So How do I solve problems with Functional Programming

---

# [fit]With Functions

### Obvi

^ With Functions. I knew this fact intellectually, but I couldn't quite describe or feel that I really understood it deep down in a genuine way that I could transfer to my students. It felt like I was going backwards on a snowboard down a hill. I knew intellectually how to shift my weight to make a turn, but I just couldn't get my body to actually do it.

---

# More Specifically

  * Splices in Heist
  * Enums/Presenter in Yesod Apps
  * Types as Documentation in Blaze and Yesod
  * Continuations in Yesod Apps
  * Dependency Injection using partial application

^ So I started reading code. I didn't want to make the same mistakes that haskell experts have been making for a long time. I wanted to shortcut my way to a bit of best practices and opinioned development. I looked at probably 20 different Haskell, Scala or F# applications but I took examples from Heist, a HTML templating framework, Blaze, another HTML templating Framework as well as from a few Yesod applications. Yesod is effectively a rails of Haskell. The first thing I want to cover is Splices in Heist

---

# Heist

  * ERB for Haskell
  * Pulls from the Lift web framework in Scala
  * Generally used with the [Snap Web Framework](http://snapframework.com/)
  * Simpler than Yesod
  * [More Info](http://snapframework.com/docs/tutorials/heist)

^ As I said Heist is a templating language for Haskell based off of the Lift web framework in Scala. It's generally used with the Snap Web Framework which is just a simpler Yesod. Simpler, means easier for me to read!

---

# General Purpose Templating With...

^ The biggest feature Heist provides is general purpose templating with....

---

# [fit]Functions!

^ Functions of course! But you knew that was going to be the answer.

---

# Splices

Factorial Function

```haskell
factSplice :: Splice Snap
factSplice = do
    input <- getParamNode
    let text = T.unpack $ X.nodeText input
        n = read text :: Int
    return [X.TextNode $ T.pack $ show $ product [1..n]]
```

^ If you want any sort of logic in your views we don't actually want that logic to exist in the view code. So, we define a function outside of the view code like this. This is a function called factSplice and it pretty much takes an input from the template, then calculates the factorial.

---

# Using the Splice

```haskell
bindSplice "fact" factSplice templateState
```

### In your template: 

```
<fact>5</fact>
```

### Spits out 

```
120
```

^ Then to use this in Heist we connect that function to the "fact" keyword that can then be used in your html template. If we put in 5, we then get 120. I love this! I love this for a few reasons. The first is that a constraint in Haskell is a major reason this style came about. 

---

# Never Subclass

### Because you can't really in Haskell

^ In Haskell, you can't really subclass. That's not even an option. Over the years we've definitely learned some of the pitfalls of excessive subclassing. I remember a great talk by the team at Spotify and they removed most of the subclassing from their app after a re-design. Sub-Classing tends to lead to very tightly coupled classes and by having these simple functions we are able to create super general purpose views. Let's look at what this looks like in Swift.

--- 

# In Swift

```swift
class myView: UIView {
    let splice: () -> (String)
}
```

^ This should be pretty straight forward. Here we have a simple UIView that takes in a single property that is a function! It's a function that has no inputs, and returns a String. Obviously you can modify this splice to the specific usage you need. Maybe this returns an AttributedString. Maybe it has a UITextField and it takes in a parameter.

---

# In Swift

```swift
let theView = myView(frame: frame) { () -> (String) in
  return "This is a test"
}
```

^ Then we create our view and pass in the splice. Obviously this is a bit simplistic, but imagine all of the things you can do! You now have a general purpose view that can have logic associated with it. Best thing is

---

# So What?

  * No logic in our views
  * Pass static values wrapped in anonymous functions `{ customer.name }`
  * Works well with MVVM
  * Presenter Pattern

^ There is no logic in our views. This has been a scourge of UI programming for a long time. Keeps our Views just caring about display while keeping them general. Thank to Swift's super simple anonymous function syntax, passing in variables is really simple with just a few curly braces. In the end this seems like the functional equivalent to the presenter pattern. Once I started thinking more about presenter pattern.

---

# Speaking of the Presenter Pattern

![](http://www.atdnal.org/Resources/Pictures/presenter-guidelines.jpg)

^ I realized that Haskell does more with this Presenter Pattern. They end up doing it with their version of enums

---

# Enums and Functions

```haskell
data SortBy
    = SortByAZ
    | SortByCountUp   -- lowest count at top
    | SortByCountDown -- highest count at top
    | SortByYearUp    -- earliest year at top
    | SortByYearDown  -- latest year at top
    deriving Eq

instance Show SortBy where
    show SortByAZ        = "a-z"
    show SortByCountUp   = "count-up"
    show SortByCountDown = "count-down"
    show SortByYearUp    = "year-up"
    show SortByYearDown  = "year-down"
```

[source](https://github.com/mitchellwrosen/dohaskell/blob/master/src/Model/Browse.hs#L16-L29)

^ This is effectively an enum. It comes from the dohaskell app which is a Yesod app. It's known as an algabraic type in haskell, but they are used all over the place in haskell! This one lists all the different ways you can sort by. But one of my favorite things is the bottom half of this. In the bottom half they are overriding the definition of the Show function in Haskell, which is very similar to the describe function in objective c. Let's look at another examples of this.

---- 

# Enums and Functions

```haskell
data ResourceType
    = BlogPost
    | CommunitySite
    | Dissertation
    | Documentation
    deriving (Bounded, Enum, Eq, Ord, Read, Show)

-- Describe a resource type in a short sentence.
descResourceType :: ResourceType -> Text
descResourceType BlogPost          = "Blog post"
descResourceType CommunitySite     = "Community website"
descResourceType Dissertation      = "Dissertation"
```

[source](https://github.com/mitchellwrosen/dohaskell/blob/20b3fdc1a06f35ef87b5ef99ff06f499ce6a9d06/src/Model/Resource/Internal.hs#L9-L57)

^ This one is also from the dohaskell app. It is an type on the different types of resources you can have. At the bottom, is again more functions! This function takes a ResourceType and converts that to Text. The Text can then be used in splices in your views! And I only show this function but if you click on the source, there are dozens of functions. This is the presenter! 

---

# Last Example of Enums Power

```haskell
data Shape = Circle Float Float Float | Rectangle Float Float Float Float
```

```swift
enum Shape {
  case Circle(Float,Float,Float)
  case Rectangle(Float,Float,Float,Float)
}

var myShape = Shape.Circle(5.0,5.0,5.0)
```

[source](http://learnyouahaskell.com/making-our-own-types-and-typeclasses)

^ One other thing I didn't know was possible about enums is diff constructors. In Haskell and in swift you can create different constructors that accept different arguments. This is just something, I didn't know possible but you see it in Haskell, and it's also possible in Swift!

---

# So What?

  * Define Business Logic easily!
  * Compile time checks for all options
  * State Machines are Simple!

^ So What! When I was told in Swift that they want you to use these value types I was kept asking why? A lot of answers where a long the lines of "because apple said so" and "because it's better". Once I saw this usage of enums with functions I realized a really tangible reason to use enums. All of the presenter functions I write for my enums are now compile time checked for completeness. Not only does that stop me from making stupid mistakes, it also means I well-define my logic pretty early on. 

---

# Business Logic in Enums

```swift
enum Barcode {
    case UPCA(Int, Int, Int, Int)
    case QRCode(String)
}
```

^ The other reason I love these enums is you can define parts of business logic. For example here I have Barcode enum. It can take either a QRCode or standard barcode. Now! whenever I use Barcodes I know ensure that I cover all cases. The last thing I saw people using enums with functions for were state machines. For a very long time I though state machines were over-complicated.

---

# [fit]State Machines Feel

# [fit]Hard

^ They are perceived as hard because they require you to rigorously define all the transitions and possible states. But! This is a good thing.

---

# [fit]State Machines Are

# [fit]Simple

^ Enums with functions make state machines very simple. Start small and build up your different states. Best part is that everything is compile time checked. I saw a handful of these state machines: Checkout Flow, Login Flow, Account Signup, Loading. As well as anything that has a submitted_at type field 

---

# State Machines

```swift
enum Light {
    case Off, On
    func flipedSwitch() -> Light {
        switch self {
        case On: return Light.Off
        case Off: return Light.On
        }
    }
}

var light = Light.On
light = light.flippedSwitch()
if light == .Off { println("Hello?") }
```

^ This is just a simple example. Pretty much a light that can turn on and off. Use these.

---

# So Many Types!

^ So you'll notice that we've been making a lot of types. Haskell loves types, and my favorite feature of Swift is it's type system.

---

# Types as Documentation

```haskell
paperInfoModalWithData :: PaperId -> Paper -> Widget
```

```haskell
type PaperId = Int
```

[source](https://github.com/hirokai/PaperServer/blob/b577955af08660253d0cd11282cf141d1c174bc0/Handler/Widget.hs#L51)

^  In Haskell, these types are used all over the place. The basic types of Int, String, etc are almost never used. The reason is Look How Readable that is! It documents itself! Also, you get some more lovely Compile time checks.

---

# Types as Documentation

```haskell
type Attributes = [(String, String)]
```

[source](https://github.com/jaspervdj/blaze-html/blob/a0272268527a4fb10edf74154564879b06a8d76f/src/Util/BlazeFromHtml.hs#L23)

^ Here is another example. It's just a Key/Value pair in an array but it means so much!

---

# Type Lessons For Swift

  * Be a bit silly
  * Easier then documentation
  * COMPILE TIME CHECKING

^ Sometimes this can feel a bit silly, but in the end I've been really enjoying using this to give me some great checks and easy documentations

---

# [fit]Back To Functions

^ And now I didn't have a good transition here sooooo back to functions!

---

# Completion Blocks to the

# [fit]EXTREME

^ We've seen completion blocks all the time in obj-c code. Pretty simply why do we use them in async code?

--- 

# Continuation

```haskell
runValidation :: Validation a -> a -> (a -> Handler b) -> Handler b
runValidation validate thing onSuccess =
    case validate thing of
        Right v -> onSuccess v
        Left es -> sendResponseStatus status400 $ object
            ["errors" .= map toJSON es]
```

[source](https://github.com/thoughtbot/carnival/blob/master/Helper/Validation.hs#L5-L10)

^ There is a concept called continuation. Continuation is effectively just applying the async completion block concept a bit more. Here it's being used to validate code. Instead of passing in two different messages, it runs the validation and then just keeps going on. This is used later down the road with the onSuccess method being the code that actually inserts the item into the database.

---

# Continuation

  * Allow us to punt on what to do

```haskell
unitAttack :: Target -> IO ()
unitAttack target = do
  swingAxeBack 60
  valid <- isTargetValid target
  if valid
  then ??? target
  else sayUhOh
```

  * I don't know what I'm going to want to do in `???`

^ Continuations are useful for any time you are unsure as to what to do in in your code! This is just an example, we have a haskell function here called unitAttack that takes in a target. We swing our axe back, we then make sure if it's a valid target, and then we don't know what to do!

---

# Punt!

```haskell
unitAttack :: Target -> (Target -> IO ()) -> IO ()
unitAttack target todo = do
    swingAxeBack 60
    valid <- isTargetValid target
    if valid
    then todo target
    else sayUhOh
```

^ So we punt! Now we take in a function and punt that decision making onwards. Speaking of punting decision making onwards.

---

# [fit]Dependency Injection

^ Let's talk a bit about Dependency Injection. Dependency Injection is a great way to show off another fancy new feature of swift, partial application

---

# Partial Application!

```haskell
add     :: Int -> Int -> Int
add x y = x + y
```
Which means

```haskell
add :: Int -> (Int -> Int)
```

```haskell
addOne = add 1 // addOne :: Int -> Int
```

^ This came out of researching practical usages of partial application. Sadly, I haven't spotted it in public but I'm including it here. This was such a boring example! So I googled and googled and found an example of partial applications in F sharp

---

# In Swift!

```
func getCustomer (customerID: CustomerID) -> Customer?
```

```
func getCustomerFromMem (source: [Cust])(custID: CustID) -> Cust?
```

```
func getCustomerFromDB (source: DBHandler)(custID: CustID) -> Cust?
```

```
let getMemCustomer = getCustomerFromMem(source: customersArray)
let foundCustomer = getMemCustomer(customerID: 2)
```

[source](http://mikehadlow.blogspot.com/2010/03/functional-dependency-injection.html)

^ Look at that! If we have a method takes a getCustomer function. Feel free to typealias this bad boy we can then pass in different functions that satisfy that type signature. We can write two different methods. One is getCustomerFromMem that has a source of an array of Customer objects, and another has a source of some sort of DataBase Handler. Then depending on what we want we can use partial application to remove set the source and then we are left with a function that goes from customerID to Customer.

--- 

# [Fit]Tid-Bits

^ Alright, now are wrapping up and a few tid-bits that didn't fit anyhwere because every has that random stuff drawer in their kitchen.

---

# Cool Tidbit

```haskell
-- Reuse instance ToMarkup Text
instance ToMarkup ResourceType where
    toMarkup = toMarkup . descResourceType
    preEscapedToMarkup = toMarkup . descResourceType
```

[source](https://github.com/mitchellwrosen/dohaskell/blob/20b3fdc1a06f35ef87b5ef99ff06f499ce6a9d06/src/Model/Resource/Internal.hs#L32-L35)

^ The dot is function composition. Pretty much this means it redefines toMarkup (HTML) as the original toMarkup, but passes in this functions specific resource description. Junior yesterday showed a ton about this, but if you are reading haskell, that dot is important!

---

# Inferred Types

  * Used in type signatures of functions *always*
    * Because it's documentation!
  * Pretty much never used anywhere else

---

# infix notation

```haskell
elem 'a' "camogie"
```

```haskell
'a' `elem` "camogie"
```

## No more crazy operator symbols!

---

# Deriving

```haskell
data LocalCopyStatus = LocalAvailable 
                        | NotYet 
                        | Failed 
                        | Unknown 
                        deriving (Show,Read,Eq,Typeable)
```

## Protocols with basic implementations

---

# [fit]Thanks!
<br />
  Joe Burgess
  
  @jmburges

---

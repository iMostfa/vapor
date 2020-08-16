Vapor is a web framework for Swift. It provides a beautifully expressive and easy to use foundation for your next website, API, or cloud project.

Take a look at some of the [awesome stuff](https://github.com/Cellane/awesome-vapor) created with Vapor.

### 💻 Technincal Background 

Vapor is built on SwiftNIO, which provide a great performance and a great way to handle asynchronous work on servers, with features like 
for Futures and Promises. 

while it's already working good for vapor and other server frameworks , writing asynchronous code for vapor applications without swiftNIO will leave the developers with two options: 

1- writing asynchronous code in the tradational way of callbacks and clousres
2- writing asynchronous code using the avaliable frameworks for RFP like RxSwift and Combine

using 2nd option seems a better idea since it would really fit in the environemnt, But for some reasons, as disuccssed [here](https://github.com/vapor/vapor/issues/1238) RxSwift won't be the best option because it's a big depdency

 i believe, that combine is a better alternative, also swiftNIO is going to have better support for [Combine] (https://forums.swift.org/t/will-swiftnio-adapt-to-the-new-combine-framework/25166/2) 


### 🧩 Combine Support 

This's an expermintal fork from Vapor, with combine Support added to it.
 ```swift
 func routes(_ app: Application) throws {
        
    app.get{ req -> EventLoopFuture<String> in
        
       return Just("AwesomePublisher").promise(on: req)
    }
 }
    
 
```

or more complex usage will be: 


```swift

func routes(_ app: Application) throws {
    
    
    app.get{ req -> EventLoopFuture<View> in //normal route in vapor 4
        
        Just("Awesome Combine Publisher") //Combine Publisher, could be any publisher like PassThroughPublisher, or AnyPublisher,
          
            .flatMap({ (value) -> AnyPublisher<String,Never>  in //flat map in Combine to process data
                return Just(value + "Some operation").eraseToAnyPublisher()
            })
            
            .promise(on: req) //Combine Publisher tranformed to EventLoopFuture
            
            .flatMap { (outputFromCombine) -> EventLoopFuture<View> in //Flat map in SwiftNIO to process data recieved to a view
                return req.view.render("index",outputFromCombine)
        }
        
    }
  }
  ```

### 📝 To-do 
 a lot of things can be done to have a better support for Combine and swiftNio, for example
 i would love to remove the usage of flatMap(SwiftNio) one, for better readability and less boilerplate code.  



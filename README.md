![alt text](https://raw.githubusercontent.com/gmazzei/VaporTutorial/master/vapor_logo.png)

## Quickstart
[How to set up a Vapor 3 project](https://medium.com/@martinlasek/tutorial-how-to-set-up-a-vapor-3-project-75466394cf2e)
<br/><br/>

## Useful tutorials
* [How to write Models using Fluent](https://medium.com/@martinlasek/tutorial-how-to-write-models-using-fluent-e9482d335a5f) <br/>
  Fluent is a Swift ORM framework (queries, models, and relations) for building SQL and NoSQL database integrations.

* [How to build a Basic Auth](https://medium.com/@martinlasek/tutorial-how-to-build-basic-auth-5e618a656999)
* [How to build one-to-many relations](https://medium.com/@martinlasek/tutorial-how-to-build-one-to-many-relations-84dd37e464a6)
* [Write a CRUD API using JSON](https://medium.com/@martinlasek/tutorial-write-a-crud-api-using-json-c1edb1439d8a)



Source: [Vapor University](https://vapor.university/?medium=article). Here you will find many tutorials regarding the basics of any Vapor project (important: check that the version of Vapor used in the tutorial is Vapor 3).
<br/><br/>

## Other tutorials
* [Deploying to Heroku](https://www.twilio.com/blog/2016/11/how-to-deploy-vapor-apps-to-heroku.html)
<br/><br/>

## Official Documentation
[Vapor Docs](https://docs.vapor.codes/3.0/)
<br/><br/>

## Swift Package Manager

Vapor uses [Swift Package Manager](https://swift.org/package-manager/), a tool that automates the process of downloading, compiling and linking dependencies. 

Inside Package.swift you will find all the info related to dependencies. An example is shown below:
```Swift

// swift-tools-version:4.0
import PackageDescription

let package = Package(
    name: "swift-training-backend",
    dependencies: [
        .package(url: "https://github.com/vapor/vapor.git", from: "3.0.0"),
        .package(url: "https://github.com/vapor/fluent-postgresql.git", from: "1.0.0"),
        .package(url: "https://github.com/vapor-community/pagination.git", from: "1.0.0")
    ],
    targets: [
        .target(name: "App", dependencies: ["Vapor", "FluentPostgreSQL", "Pagination"]),
        .target(name: "Run", dependencies: ["App"]),
        .testTarget(name: "AppTests", dependencies: ["App"])
    ]
)

```

<br/><br/>

# VaporSamples
Code samples of the Vapor framework.
<br/><br/>

## Model

Models are entities that carry the data through our app. Here is an example of a Book model class:

```Swift
import FluentPostgreSQL
import Vapor


final class Book: PostgreSQLModel {
    
    var id: Int?
    var author: String
    var title: String
    var image: String?
    var year: String
    var genre: String
    
    
    init(id: Int? = nil, author: String, title: String, image: String?, year: String, genre: String) {
        self.id = id
        self.author = author
        self.title = title
        self.image = image
        self.year = year
        self.genre = genre
    }
    
}


// One-to-many relationships
extension Book {

    var comments: Children<Book, Comment> {
        return children(\.bookID)
    }
    
    var rents: Children<Book, Rent> {
        return children(\.bookID)
    }
    
    var wishes: Children<Book, Wish> {
        return children(\.bookID)
    }
}


extension Book: Content {}
extension Book: Migration {}
extension Book: Parameter {}

```

Every property is not-null by default (we must provide a value). If we want it to be nullable, we should write it as a optional field. This is the case of the 'image' property.

More info: [Models](https://docs.vapor.codes/3.0/fluent/models/)

<br/>

## Controller

Controllers manage every request/response and they normally use the models to fetch or save data. They are often responsible for validation too. Here is an example of a BookController: 

```Swift
import Vapor
import FluentPostgreSQL

final class BookController: BaseController {
    
    func list(_ req: Request) throws -> Future<[Book]> {
        return Book.query(on: req).all()
    }
    
    
    func show(_ req: Request) throws -> Future<Book> {
        return try req.parameters.next(Book.self)
    }
    
    
    func create(_ req: Request) throws -> Future<Book> {
        return try req.content.decode(Book.self).flatMap { book in
            return book.save(on: req)
        }
    }
    
    
    func update(_ req: Request) throws -> Future<Book> {
        return try req.parameters.next(Book.self).flatMap { book in
            return try req.content.decode(Book.self).flatMap { newBook in
                book.title = newBook.title
                book.author = newBook.author
                return book.save(on: req)
            }
        }
    }
    
    
    func delete(_ req: Request) throws -> Future<HTTPStatus> {
        return try req.parameters.next(Book.self).flatMap { book in
            return book.delete(on: req)
        }.transform(to: .ok)
    }    
}
```

More info: [Controllers](https://docs.vapor.codes/3.0/getting-started/controllers/)

<br/>

## Routes

The file named routes.swift binds each endpoint with its corresponding controller. An example is shown below:

```Swift
import Vapor

public func routes(_ router: Router) throws {

    let bookController = BookController()
    router.get("books", use: bookController.list)                           // GET /books
    router.get("books", Book.parameter, use: bookController.show)           // GET /books/$id
    router.post("books", use: bookController.create)                        // POST /books
    router.patch("books", Book.parameter, use: bookController.update)       // PATCH /books/$id
    router.delete("books", Book.parameter, use: bookController.delete)      // DELETE /books/$id
}
```
<br/>

More info: [Routing](https://docs.vapor.codes/3.0/getting-started/routing/)

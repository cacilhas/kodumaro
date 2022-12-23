![Gopher](//cacilhas.info/img/golang.png)

Now I’m revisiting an [old post of mine](/2017/06/golang.html) about my personal impressions about the [Go](https://golang.org/) language.

### Outflanking

My first impression on Go was very wrong. I flanked Go from the [Python](https://www.python.org/) and [Node.js](https://nodejs.org/) perspective, which was very disappointing. When you try to use a language that offers powerful tools to access system resources with an open-and-shut mind, the result is frustrating.

As soon as I understood Go is closer to [C++](http://www.cplusplus.com/) and [Objective-C](https://www.gnu.org/software/gnustep/resources/documentation/Developer/Base/ProgrammingManual/manual_toc.html), I realised it’s a cool, powerful, and fun programming language.

At that time, a friend vainly tried to explain me Go pointers as read-write parameters against read-only ones. If he had simply told me “pointers like in C,” I’d get at once.

My disapproval to Go is that [Alphabet](https://abc.xyz/) might not have recreated the wheel so hard, it could take advantage of other well-tested platforms, like [Vala](https://wiki.gnome.org/Projects/Vala) and [C♯](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/) did. It would entice lotta god programmers by the soft learning curve (despite [Linus Tovarlds disagrees](https://lwn.net/Articles/249460/)).

### Interfaces

In Go, [interfaces](https://www.golang-book.com/books/intro/9) look like [structural types](https://docs.scala-lang.org/style/types.html#structural-types) in Scala.

In other hand, [structs](https://gobyexample.com/structs) are much like [Objective-C `@interface`s](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/DefiningClasses/DefiningClasses.html).

An instance of any struct that matches an interface is instance of the inteface too, just like structural types.

For sample, take the following interface:

    type Person interface {
      FirstName() string
      LastName() string
      FullName() string
      Birth() time.Time
    }

If one has defined a struct:

    type personType struct {
      firstName, lastName string
      birth               time.Time
    }

And then every function from `Person` is implemented for `personType`:

    func (p personType) FirstName() string {
      return p.firstName
    }
    
    func (p personType) LastName() string {
      return p.lastName
    }
    
    func (p personType) FullName() string {
      return strings.Trim(fmt.Sprintf("%v %v", p.firstName, p.lastName), " ")
    }
    
    func (p personType) Birth() time.Time {
      return p.birth
    }

Then it’s possible to return a `personType` as `Person`:

    func NewPerson(firstName, lastName string, birth time.Time) Person {
      return personType{firstName, lastName, birth}
    }

### Unit tests

For testing, Go’s got a built-in [testing](https://golang.org/pkg/testing/) library, that’s very easy to use.

Being very verbose, a yet simple example may be:

    func TestPerson(t *testing.T) {
      timeForm := "2006-01-02"
      birth, _ := time.Parse(timeForm, "2017-06-12")
      p := NewPerson("John", "Doe", birth)
    
      t.Run("primary methods", func(t *testing.T) {
        t.Run("FirstName", func(t *test.T) {
          if got := p.FirstName(); got != "John" {
            t.Fatalf("expected John, got %v", got)
          }
        })
    
        t.Run("LastName", func(t *test.T) {
          if got := p.LastName(); got != "Doe" {
            t.Fatalf("expected Doe, got %v", got)
          }
        })
    
        t.Run("Birth", func(t *test.T) {
          expected, _ := time.Parse(timeForm, "2017-02-12")
          if got := p.Birth(); got != expected {
            t.Fatalf("expected %v, got %v", expected, got)
          }
        })
      })
    
      t.Run("secondary methods", func(t *test.T) {
        t.Run("FullName", func(t *test.T) {
          if got := p.FullName(); got != "John Doe" {
            t.Fatalf("expected John Doe, got %v", got)
          }
        })
      })
    }

Enough for most cases.

In order to understand Go time format: [Format a time or date](https://yourbasic.org/golang/format-parse-string-time-date-example/).

### Catching exceptions

The Go’s exception catching pattern is returning the error as second argument for every function that can go wrong.

In the sample above, one can found this line:

    birth, _ := time.Parse(timeForm, "2017-06-12")

The underscore (`_`) means the exception is discarded. If one wanna deal with the exception, one can do something like:

    birth, err := time.Parse(timeForm, "2017-06-12")
    if err != nil {
      // Deal with the exception
      ...
    }

Another way Go can throw an exception is by a panic attack. One can do it by calling the `panic()` function.

The only way to catch this kind of exception is calling `recover()` inside a `defer` block.

One can catch the exception and return it through a writable channel (`chan` or `chan<-`).

It smells like:

    func DoSomethingDangerous(res chan<- error) {
      defer res <- recover()
    
      // Do somenthing that can raise a panic attack.
      ...
    }

### Afterword

Some useful resources:

*   [A Tour of Go](https://tour.golang.org/)
*   [An Introduction to Programming in Go](https://www.golang-book.com/books/intro)
*   [The Go Blog](https://blog.golang.org/)
*   [Going Go Programming](https://www.goinggo.net/) (looks unreachable 😟)
*   [The Go Playground](https://play.golang.org/)

* * *

Also in [DEV.to](https://dev.to/cacilhas/about-go-51c1).
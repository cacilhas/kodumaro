![Scala](//cacilhas.info/img/scala.png)

When I started working with [Scala](https://www.scala-lang.org/), I did it all wrong. I learned Scala from _oakies_, so my team used Scala just as a syntax sugar for Java (like [Kotlin](https://kotlinlang.org/) or [Xtend](https://www.eclipse.org/xtend/)).

That’s a great way to **strongly underuse** Scala.

Scala isn’t Java. Scala binds to Java, but goes forth. Programming in Scala, you can access Java resources, but even the feeling smells different.

Both languages are [object-oriented](https://www.amazon.com/gp/product/0136291554/), but Java is an [imperative language](https://rosettacode.org/wiki/Category:Programming_paradigm/Imperative) in the most strict sense, while Scala is impure [functional](https://rosettacode.org/wiki/Category:Programming_paradigm/Functional), tending tightly to the pureness.

Another notable difference is that, even though both languages are general-purpose, Scala makes it possible (and easy) to create microlanguages for specific domains, called [domain-specific languages](https://martinfowler.com/books/dsl.html), or just **DSL** – which’s very hard to do using Java.

### Example

Image we got some [BibTeX](http://www.bibtex.org/) files, and need to import them into the system. (Let’s work only with books, for simplicity’s sake.)

But we don’t want simply import the files, we wanna write BibTeX models using a syntax as close as possible to BibTeX.

I take from [Wikipedia](https://en.wikipedia.org/wiki/BibTeX#Bibliographic_information_file) this sample:

    @Book{abramowitz+stegun,
     author    = "Milton {Abramowitz} and Irene A. {Stegun}",
     title     = "Handbook of Mathematical Functions with
                  Formulas, Graphs, and Mathematical Tables",
     publisher = "Dover",
     year      =  1964,
     address   = "New York City",
     edition   = "ninth Dover printing, tenth GPO printing"
    }

Let’s start on the trait representing the generic BibTeX model:

    trait BibTeX {
      def id: String
    }

So the book:

    case class Book(override val id: String,
                    author: String,
                    title: String,
                    publisher: String,
                    year: Int,
                    volume: String = "",
                    series: String = "",
                    address: String = "",
                    edition: String = "",
                    month: String = "",
                    note: String = "",
                    key: String = "",
                    url: String = "",
                   ) extends BibTeX {
    
      private lazy val output = {
        val res = new StringBuilder
        res append "@Book{"
        res append id
        res append ",\n"
        res append s""" author    = "$author""""
        res append ",\n"
        res append s""" title     = "$title""""
        res append ",\n"
        res append s""" publisher = "$publisher""""
        res append ",\n"
        res append s""" year      =  $year"""
        if (!volume.isEmpty) {
          res append ",\n"
          res append s""" volume    = "$volume""""
        }
        if (!series.isEmpty) {
          res append ",\n"
          res append s""" series    = "$series""""
        }
        if (!address.isEmpty) {
          res append ",\n"
          res append s""" address   = "$address""""
        }
        if (!edition.isEmpty) {
          res append ",\n"
          res append s""" edition   = "$edition""""
        }
        if (!month.isEmpty) {
          res append ",\n"
          res append s""" month     = "$month""""
        }
        if (!note.isEmpty) {
          res append ",\n"
          res append s""" note      = "$note""""
        }
        if (!key.isEmpty) {
          res append ",\n"
          res append s""" key       = "$key""""
        }
        if (!url.isEmpty) {
          res append ",\n"
          res append s""" url       = "$url""""
        }
        res append "\n}\n"
        res.toString
      }
    
      override def toString: String = output
    }

You must have noticed this class is quite long. The DSL describing models usually are long, but they enable us to write short code therence. The longer part aims to serialise into the BibTeX file.

For instance, the book described below:

    val book =
      Book("abramowitz+stegun",
        author    = "Milton {Abramowitz} and Irene A. {Stegun}",
        title     = "Handbook of Mathematical Functions with Formulas, Graphs, and Mathematical Tables",
        publisher = "Dover",
        year      =  1964,
        address   = "New York City",
        edition   = "ninth Dover printing, tenth GPO printing",
      )

Eventually, it’s possible to write DSL much more elegant, just design the syntax you want, and code it.
![Scala](//cacilhas.info/img/scala.png)

When I started working with [Scala](https://www.scala-lang.org/), I did it all wrong. I learned Scala from _oakies_, so my team used Scala just as a syntax sugar for Java.

That’s a great way to underuse Scala strongly.

Scala isn’t Java. Scala binds to Java, but goes forth. Programming in Scala, you can access Java resources, but even the programming feeling is different.

Both languages are [object-oriented](https://www.amazon.com/gp/product/0136291554/), but Java is an [imperative language](https://rosettacode.org/wiki/Category:Programming_paradigm/Imperative) in the most strict sense, while Scala is [impure functional](https://rosettacode.org/wiki/Category:Programming_paradigm/Functional).

Another notable difference is that, even though both languages are general-purpose, Scala enables creating microlanguages for specific domains, called [domain-specific languages](https://martinfowler.com/books/dsl.html), or just **DSL** – which’s very hard to do using Java.

### The problem

I got a schemaful inbound JSON payload, and I needed to respond an outbound XML. Midst of format transformation chaos, some keys wasn’t the same, even some values.

Take the following payload:

    {
      "invoice": {
        "general": {
          "date_time": "2020-05-23T01:32:16Z",
          "number": 23,
          "serial": 12,
          "access": "20200523123455012000231X",
        },
        "products": {
            "description": "garlic",
            "ean": "5018605003253",
            "unit_cost": 1.95,
            "amount": 2.50,
            "discount": 0.005,
            "total_cost": 4.87
        }
      }
    }

The expected output is:

    <invoice id="20200523123455012000231X">
      <ident>
        <dateTime>2020-05-23T01:32:16Z</dateTime>
        <set>12</set>
        <number>23</number>
      </ident>
      <product>
        <name>garlic</name>
        <EAN>5018605003253</EAN>
        <uPrice>1.95</uPrice>
        <amount>2.50</amount>
        <discountValue>0.005</discountValue>
        <subTotal>4.87</subTotal>
      </product>
    </invoice>
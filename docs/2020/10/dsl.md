![Scala](//cacilhas.info/img/scala.png)

Some days ago I wrote an post about DSL, but it was so sloppy, that I’ve found myself in shame. ’Cause that, I’m removing that post and replacing it with this.

When I started working with [Scala](https://www.scala-lang.org/), I did it all wrong. I learned Scala from _oakies_, so my team used Scala just as a syntax sugar for Java (like [Kotlin](https://kotlinlang.org/) or [Xtend](https://www.eclipse.org/xtend/)).

That’s a great way to **strongly underuse** Scala.

Scala isn’t Java. Scala binds to Java, but goes forth. Programming in Scala, you can access Java resources, but even the feeling smells different.

Both languages are [object-oriented](https://www.amazon.com/gp/product/0136291554/), but Java is an [imperative language](https://rosettacode.org/wiki/Category:Programming_paradigm/Imperative) in the most strict sense, while Scala is impure [functional](https://rosettacode.org/wiki/Category:Programming_paradigm/Functional), tending tightly to the pureness.

Another notable difference is that, even though both languages are general-purpose, Scala makes it possible (and easy) to create microlanguages for specific domains, called [domain-specific languages](https://martinfowler.com/books/dsl.html), or just **DSL** – which’s very hard to do using Java.

### Example

Just for instance, we’re gonna implement a very flat and incomplete set of SQL, in fact, just some `SELECT` resources. For the sake of simplicity, we aren’t taking care of sql injection or complex queries; The idea is enabling to represent a simple `SELECT` statement in a syntax as close as possible to SQL.

The target is:

    Select("name", "surname") from "t_users" where Condition("surname") == "Doe" limit 10

It must serialise to:

    SELECT name, surname FROM t_users WHERE surname = 'Doe' LIMIT 10

### The condition

In order to build the `Select`, we need the `From`. To build the `From`, we need the `Condition`, so let’s start there.

However, in order to build the `Condition`, we need the `Criteria`, that represent “equals”, “not equals”, etc.

Let’s leave other criteria out and deal only with comparisons, “is null”, “not null”. The `Criteria` trait doesn’t need to be accessible outside of the `Condition` class, so it can be private:

    private sealed trait Criteria {def format(key: String, value: String): String}
    
    private object EQUALS     extends Criteria {def format(key:  String, value: String): String = s"$key = $value"}
    private object GE         extends Criteria {def format(key:  String, value: String): String = s"$key >= $value"}
    private object GT         extends Criteria {def format(key:  String, value: String): String = s"$key > $value"}
    private object ISNULL     extends Criteria {def format(key:  String, value: String): String = s"$key ISNULL"}
    private object LE         extends Criteria {def format(key:  String, value: String): String = s"$key <= $value"}
    private object LT         extends Criteria {def format(key:  String, value: String): String = s"$key < $value"}
    private object NOT_EQUALS extends Criteria {def format(key:  String, value: String): String = s"$key <> $value"}
    private object NOTNULL    extends Criteria {def format(key:  String, value: String): String = s"$key NOTNULL"}

Now, the `Condition` must hold the field name, the comparing value, and the criteria. Since some conditions have no value, the value may be optional.

Let’s use a valueless criteria for default:

    case class Condition[A](field: String, value: Option[A] = None) {
    
      private var criteria: Criteria = NOTNULL
    
      def ==(value: A): Condition[A] = {
        val res = Condition(field, Option(value))
        res.criteria = EQUALS
        res
      }
    
      def !=(value: A): Condition[A] = {
        val res = Condition(field, Option(value))
        res.criteria = NOT_EQUALS
        res
      }
    
      def >(value: A): Condition[A] = {
        val res = Condition(field, Option(value))
        res.criteria = GT
        res
      }
    
      def >=(value: A): Condition[A] = {
        val res = Condition(field, Option(value))
        res.criteria = GE
        res
      }
    
      def <(value: A): Condition[A] = {
        val res = Condition(field, Option(value))
        res.criteria = LT
        res
      }
    
      def <=(value: A): Condition[A] = {
        val res = Condition(field, Option(value))
        res.criteria = LE
        res
      }
    
      def isNull: Condition[A] = {
        val res = Condition(field, value)
        res.criteria = ISNULL
        res
      }
    
      def notNull: Condition[A] = {
        val res = Condition(field, value)
        res.criteria = NOTNULL
        res
      }
    
      override def toString: String = criteria format (field, value match {
        case None                => "NULL"
        case Some(value: String) => s"'$value'"
        case Some(value: Number) => value.toString
        case _                   => ???
      })
    }

### Going down to `SELECT`

The `Select` class is the simpliest of all, it just build a simple `SELECT`:

    case class Select private(fields: String*) {
    
      def from(tables: String*): From = new From(this,tables: _*)
    
      override def toString: String = "SELECT %s" format fields.mkString(", ")
    }

### Creating the `FROM`

Now we got the condition, let’s deal with the `From` class. It must be able to deal with the `SELECT` syntax. We’re implementing only `WHERE`, `GROUP BY`, `ORDER BY`, `HAVING`, `LIMIT`, and `OFFSET`.

Those parameters are represented by attributes:

*   `WHERE` → `conditions`
*   `GROUP BY` → `_groupBy`
*   `ORDER BY` → `_orderBy`
*   `HAVING` → `_having`
*   `LIMIT` → `_limit`
*   `OFFSET` → `_offset`

And the respective methods `where`, `groupBy`, `orderBy`, `having`, `limit`, and `offset`, each one returning a new instance of `From`.

For making it possible, we need to override the `clone` method too.

Most of the logic is gonna be in the `toString` method, responsible for build the SQL statement.

    class From(val select: Select, val tables: String*) {
    
      private var conditions: Seq[Condition[_]] = Nil
      private var groupBy: Seq[String] = Nil
      private var orderBy: Seq[String] = Nil
      private var _having: Seq[Condition[_]] = Nil
      private var _offset: Int = 0
      private var _limit: Int = 0
    
    
      def where(conditions: Condition[_]*): From = {
        val res = clone
        res.conditions = conditions
        res
      }
    
      def groupBy(fields: String*): From = {
        val res = clone
        res.groupBy = fields
        res
      }
    
      def having(conditions: Condition[_]*): From = {
        val res = clone
        res._having = conditions
        res
      }
    
      def orderBy(fields: String*): From = {
        val res = clone
        res.orderBy = fields
        res
      }
    
      def offset(value: Int): From = {
        val res = clone
        res._offset = value
        res
      }
    
      def limit(value: Int): From = {
        val res = clone
        res._limit = value
        res
      }
    
      override def clone: From = {
        val res = new From(select, tables: _*)
        res.conditions = conditions
        res.groupBy = groupBy
        res.orderBy = orderBy
        res._offset = _offset
        res._limit = _limit
        res
      }
    
      override def toString: String = {
        val res = new StringBuilder
        res append select.toString
        res append " FROM "
        res append tables.mkString(", ")
        if (conditions.nonEmpty) {
          res append " WHERE "
          res append conditions.map {_.toString}.mkString(" AND ")
        }
        if (groupBy.nonEmpty) {
          res append " GROUP BY "
          if (groupBy.size == 1)
            res append groupBy.head.toString
          else {
            res append "("
            res append groupBy.map {_.toString}.mkString(", ")
            res append ")"
          }
    
          if (_having.nonEmpty) {
            res append " HAVING "
            res append _having.map {_.toString}.mkString(" AND ")
          }
        }
        if (orderBy.nonEmpty) {
          res append " ORDER BY "
          if (orderBy.size == 1)
            res append orderBy.head.toString
          else {
            res append "("
            res append orderBy.map {_.toString}.mkString(", ")
            res append ")"
          }
        }
        if (_limit > 0) {
          res append " LIMIT "
          res append _limit.toString
        }
        if (_offset > 0) {
          res append " OFFSET "
          res append _offset.toString
        }
        res.toString
      }
    }

### Conclusion

And it’s done! We’ve just created a very basic implementation of `SELECT` statement.

You can go on and implement all the SQL features just for fun, but that’s not this post’s target; I just wanna show you how Scala is powerful to design DSL.
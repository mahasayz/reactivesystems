## Functional Programming

Uncle Bob's take on FP:
> Functional programming is programming without assignment statements

`Imperative Programming` tries to deal with entities evolving their state by mutating values. Now, this becomes troublesome in multi-threaded environment leading to unwanted complexities and race-conditions.

Moreover, `method`s in Imperative Programming are tied to specific family of objects

* so, limits reuse of similar behaviour across broader variety of objects
	* _Inheritance_ partly solves this problem
* it's quite common to see `public static` methods in `utils` package

### Tips:

```scala
def computeYearlyAggregates(clickRepo: ClickRepo): Map[Long, Seq[(Month, Int)]] = {
	
	val pastClicks = clickRepo.getClicksSince(DateTime.now.minusYears(1))
	pastClicks.groupBy(_.advertisementId).mapValues {
		case clicks =>
			val monthlyClicks = clicks.groupBy(click => 
				Month(
					click.timestamp.getYear,
					click.timestamp.getMonthOfYear
				)
			).map { case (month, groupedClicks) =>
				month -> groupedClicks.length
			}.toSeq
			monthlyClicks
	}
}
```
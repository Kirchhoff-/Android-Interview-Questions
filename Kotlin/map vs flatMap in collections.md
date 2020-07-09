# Map() vs FlatMap()
`map()` - Returns a list containing the results of applying the given transform function to each element in the original array.
```
val numbers = listOf(1, 2, 3)
println(numbers.map { it * it }) // [1, 4, 9]
```

`flatMap()` - Returns a single list of all elements yielded from results of transform function being invoked on each element of original array.
```
val list = listOf("123", "45")
println(list.flatMap { it.toList() }) // [1, 2, 3, 4, 5]
```

Consider the example. Let’s first create class Player:
```
data class Player(val name: String)
```

Then create class Team, which contains list of the players:
```
data class Team(val players: List<Player>)
```

Let’s call `flatMap()` and `map()` on this data and print the outputs:
```
fun main(args: Array<String>) {
    val tournament =
        listOf(
            Team(listOf(Player("Team1 first player"), Player("Team1 second player"))),
            Team(listOf(Player("Team2 first player"), Player("Team2 second player")))
        )

    val flatMapResult = tournament.flatMap { it.players }
    println(flatMapResult)

    val mapResult = tournament.map { it.players }
    println(mapResult)
}
```

Output:
```
[Player(name=Team1 first player), Player(name=Team1 second player), Player(name=Team2 first player), Player(name=Team2 second player)]

[[Player(name=Team1 first player), Player(name=Team1 second player)], [Player(name=Team2 first player), Player(name=Team2 second player)]]
```

`flatMap()` merges the two collections into a single one, `map()` simply results in a list of lists.

## Links
https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map.html  
https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/flat-map.html  
https://stackoverflow.com/questions/52078207/what-is-the-use-case-for-flatmap-vs-map-in-kotlin  
https://medium.com/@sachinkmr375/flatmap-vs-map-in-kotlin-aef771a4dae4

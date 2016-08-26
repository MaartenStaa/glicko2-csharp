# Glicko2

C# implementation of the Glicko-2 rating algorithm. This is a C# port of [goochjs/glicko2](https://github.com/goochjs/glicko2).

## Usage

```c#
using Glicko2;

// Instantiate a RatingCalculator object.
// At instantiation, you can set the default rating for a player's volatility and
// the system constant for your game ("τ", which constrains changes in volatility
// over time) or just accept the defaults.
var calculator = new RatingCalculator(/* initVolatility, tau */);

// Instantiate a Rating object for each player.
var player1 = new Rating(calculator/* , rating, ratingDeviation, volatility */);
var player2 = new Rating(calculator/* , rating, ratingDeviation, volatility */);
var player3 = new Rating(calculator/* , rating, ratingDeviation, volatility */);

// Instantiate a RatingPeriodResults object.
var results = new RatingPeriodResults();

// Add game results to the RatingPeriodResults object until you reach the end of your rating period.
// Use addResult(winner, loser) for games that had an outcome.
results.AddResult(player1, player2);
// Use addDraw(player1, player2) for games that resulted in a draw.
results.AddDraw(player1, player2);
// Use addParticipant(player) to add players that played no games in the rating period.
results.AddParticipant(player3);

// Once you've reached the end of your rating period, call the updateRatings method
// against the RatingCalculator; this takes the RatingPeriodResults object as argument.
//  * Note that the RatingPeriodResults object is cleared down of game results once
//    the new ratings have been calculated.
//  * Participants remain within the RatingPeriodResults object, however, and will
//    have their rating deviations recalculated at the end of future rating periods
//    even if they don't play any games. This is in-line with Glickman's algorithm.
calculator.UpdateRatings(results);

// Access the getRating, getRatingDeviation, and getVolatility methods of each
// player's Rating to see the new values.
var players = new[] {player1, player2, player3};
for (var index = 0; index < players.Length; index++)
{
	var player = players[index];
	Console.WriteLine("Player #" + index + " values: " + player.GetRating() + ", " +
		player.GetRatingDeviation() + ", " + player.GetVolatility();
}
```

## Building on .NET Core

You can also build glicko2-csharp for .NET Core. To do this, run:
```dotnet restore
dotnet build -c Release```

---
date: 2018-11-22 18:50
tags: Swift Games Botanicula
title: Solving the anemone puzzle in Botanicula
---

[Botanicula](https://en.wikipedia.org/wiki/Botanicula) is a whimsical
graphical adventure game for the iPad and other computers. One of the puzzles
near the end of the game requires a bit of thinking to solve. When I came upon
it, after a couple of hours of play, I was too tired to think. So I wrote some
code to brute-force the solution. I'm unreasonably pleased that it worked the
first time. Here's the code, cleaned up and commented:

```swift
// Solve the anemone Botanicula puzzle.
// Number the anenome 1 to 7, left-to-right.
// Encode a list of anemones into a single integer, where bit 0 is anemone 1, etc.
func encode(_ d:[Int])->Int {
	return d.reduce(0, {x, y in x | (1 << (y-1)) })
}

// A list of which anemones are pulled down when a given anemone is tapped.
let moves = [
	encode([5,7]), // Tap anemone 1
	encode([3,6]),
	encode([2,4]),
	encode([3,5]),
	encode([4,1]),
	encode([2]),
	encode([1]), // Tap anemone 7
]

// Calculate the effect of tapping a given anemone |m|.
func move(state: Int, tap m: Int)->Int {
	return (state | // original state
		encode([m])) & // raise the item that was tapped
		~moves[m-1] // lower the items that need to be lowered
}

// Brute force solver. Try all moves to depth |depth|.
func solve(state: Int, goal: Int, depth:Int) -> [Int]? {
	if state == goal {
		return [] // We've reached our goal, don't have to make any moves.
	}
	if depth <= 0 {
		return nil // Ran out of depth, give up.
	}
	for m in 1...7 {
		let newState = move(state:state, tap:m)
		if let soln = solve(state:newState, goal:goal, depth:depth-1) {
			// If we could solve the newState, prepend our move to that solution.
			return [m] + soln
		}
	}
	// Couldn't find a solution.
	return nil
}

// Produce the shortest solution, if one exists.
func tryToSolve(state:Int, goal:Int, depth:Int)-> [Int]? {
	// Do a breadth first search by searching successively deeper.
	for d in 0...depth {
		if let soln = solve(state:state, goal:goal, depth:d) {
			return soln
		}
	}
	return nil
}

let start = encode([1,6]) // This will vary depending upon the state of the puzzle.
let goal = encode([7]) // This is the desired state (just anenome 7 raised.)
let solution = tryToSolve(state:start, goal:goal, depth:12)

print(solution ?? "no solution found")
```

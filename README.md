# Mastermind

Mastermind is a Vue game based on the Mastermind board game, which came about because I tried it with my partner and got absolutely trounced. As you can tell, I really like to play and make games. I designed it as a way to practice, and inadvertently spent numerous hours playing since I finished making it. As with Set, I do not intend for this game to make money or replace the official version.

## Goals

The goal of this project was to give me a way to practice the game, since I started off so miserably at it. I also wanted it to have a short design time, since by this point I had already started working on my portfolio. It ended up taking about two days. I also wanted to add customizable difficulty options (because why not?) and I've still yet to solve any of the most challenging codes.

## Development

- I styled the board so that the pegs maintained aspect-ratio upon any changes to the board's appearance.
- I wrote an algorithm that randomly generates a code based on the user preferences, so the game can be played entirely solo.
- I used CSS radial gradients to shade the pegs, using template literals to apply them through inline styling.
- All data was tracked using refs and computed variables, which are both a part of Vue.

## Next Steps
  
Since this was a fast project with small goals, there aren't many things that I want to change. If anything, I would like to make the game more mobile friendly and further style to make it more exciting to look at.

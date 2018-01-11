---
layout: page
title: Sky-shooter
---

## The good ol' space-shooter game, built in ReactJS.

To start clone the repo and run 
```bash
cd sky-shooter
npm install
npm start
``` 
To visit the live version of the app, visit https://sky-shooter.herokuapp.com/.      
The app does not use any game engine, it's purely built on React and Javascript.    
___
### Motivation
After doing some amount of textual projects on ReactJS, I wanted to to explore it for making games. Although I am not a pro-gamer, I wanted to see how it could be made on a barebone basis, and React felt the apt one to make something like this.
___
### How
1. I started by figuring out the template for the game, how the Gameplay screen should approximately look like. It had a black background on 80% of the screen and rest would be to display things like instructions and score.     
![Sky-shooter-1]({{ site.baseurl }}/images/sky-shooter/sky-shooter-1.png)   

2. Next I wanted to make the player move and I thought that mouse movement would be a good source for that. So I used Javascript's [onMouseMove](https://www.w3schools.com/jsref/event_onmousemove.asp) to detect user's mouse movements. Its movement had to be restricted only to the GameArea and that too only laterally. So I took out the event.clientX value from the event and used it to set the state variable for the player's position. This variable is then used inside player's style.left to shift player position  

    ```
    <div ref="playerRegion" className="playerRegion" >
        <div ref="player" className="player" style={{ alignContent: 'center', left:(this.state.playerStyle.left + "px").toString() }}>
            <img src={"assets/images/" + this.state.shipImage} className="playerImage" alt="P" />
        </div>
    </div>
    ``` 
    ```css
    .playerRegion {
        /*Keep position=relative to let the children move with respect to parent*/
        position: relative;
    }
    .player {
        /* Keep position=absolute to specifically position the element*/
        position: absolute;     
    }
    ``` 

3. After completing the player's movement, it required some enemies to be bombarded towards the player. The enemies had two parts: Generating enemy object and updating it.     
#### 3.1 Generating Enemy: 
   1. The enemies had to be bombarded at reqular intervals, so I used a setInterval function to continously create enemies, it only stopped when game was paused else it would keep on generating them. It would call getEnemyCoordinates everytime, and add that to the state array enemiesX and enemiesY.
   2. For getEnemyCoordinates, the vertical coordinate of the enemy was the Y value of the top of the GameArea, but lateral coordinate of the enemy had to be decided randomly. This was pretty straight-forward, I used the Math.random() function of Javascript to do that.







 <!-- So I made the Gamearea's div to be at tabIndex="0", so that it gets all priority  -->

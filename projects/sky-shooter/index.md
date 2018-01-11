---
layout: page
comments: true
title: Sky-shooter
---

## The good ol' space-shooter game, built in ReactJS.

To start clone the repo and run 
```bash
git clone git@github.com:mauryaavinash95/sky-shooter.git
cd sky-shooter
npm install
npm start
``` 
To visit the live version of the app, visit [https://sky-shooter.herokuapp.com/](https://sky-shooter.herokuapp.com/).      
The app does not use any game engine, it's purely built on React and Javascript.     

### Motivation
After doing some amount of textual projects on ReactJS, I wanted to to explore it for making games. Although I am not a pro-gamer, I wanted to see how it could be made on a barebone basis, and React felt the apt one to make something like this.    

### The making
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
   
    3.1 **Generating Enemy**    
    The enemies had to be bombarded at reqular intervals, so I used a setInterval function to continously create enemies, it only stopped when game was paused else it would keep on generating them. It would call getEnemyCoordinates everytime, and add that to the state array enemiesX and enemiesY.       
    For getEnemyCoordinates, the vertical coordinate of the enemy was the Y value of the top of the GameArea, but lateral coordinate of the enemy had to be decided randomly. This was pretty straight-forward, I used the Math.random() function of Javascript to do that.     

    ```javascript
    /* To generate enemies. */
    setInterval(() => {
        if (!this.state.pause)
            this.generateEnemies();
    }, enemiesThrowInterval);

    function generateEnemies() {
        let { enemiesX, enemiesY, enemyCount } = this.state;
        let width = Math.floor(this.getBoundaries().width);
        enemiesX.push(Math.floor(Math.random() * width) + 1);
        enemiesY.push(0);
        aliveEnemies.push(1);
        enemyCount++;
        this.setState({ enemiesX, enemiesY, enemyCount });
    }
    ```     
    
    ![Sky-shooter-2]({{ site.baseurl }}/images/sky-shooter/sky-shooter-2.png) 


    3.2 **Updating Enemy**     
    With that done, Enemies started popping up and the Player started to move. Now we need to bring the enemies down i.e. towards the player. Again a simple setInterval was great to do that, so whenever the game wasn't paused, it would call updateEnemiesY to increment Y-coorinate of each enemy by a given distance. I had to take care that the setInterval time and the Y-distance shifted in each call was such that the transition looked smooth, else it would create a jarring movement of the enemies.     
    ```javascript
        // To update the Y-coordinate of the enemies.
        setInterval(() => {
            if (!this.state.pause)
                this.updateEnemiesY();
        }, enemiesSpeedInterval);

        updateEnemiesY() {
            let { enemiesY, enemiesX, playerStyle, bottom, lives } = this.state;
            for (let i = 0; i < enemiesY.length; i++) {
                if (enemiesY[i] > -enemiesSpeedSize) {
                    enemiesY[i] = enemiesY[i] + enemiesSpeedSize;
                    // Soeme logic to check collisions, Enemies crashing up Spaceship
                }
            }
            this.setState({ enemiesY });
        }

    ````

4. Phewww... that's a lot of work just to make the enemies move, but we got to do the same for bullets. The bullets fired on button click of the user, so `generateBullets()` was taken care without setInterval this time, but to update their Y-coorinates (firing them upwards this time), we needed to call `updatebulletY()` with the setInterval, again minding the time and distance covered by bullet to make transitioning smooth.
    ```javascript
        updatebulletY() {
            let { bulletY, bulletX, enemiesX, enemiesY, enemyCount, aliveEnemies, score } = this.state;
            for (let i = 0; i < bulletY.length; i++) {
                if (bulletY[i] > -bulletSpeedSize) {
                    bulletY[i] = bulletY[i] - bulletSpeedSize;
                    // Some logic to check collisions, bullets shooting enemies
                }
            }
            this.setState({ bulletY, enemiesY, enemyCount, score, aliveEnemies });
        }
    ```

5. Now that bullets are moving and enemies are rolling, I had to check for collisions.   
    5.1. **First checking when bullets are destroying the enemies**: To do this, we just need to check for each bullet, each enemy, if {bulletX, bulletY} are in proximity of {enemiesX, enemiesY}. Instead of doing another function call, I did this in the `updatebulletY()` function itself.    
    ```javascript
        function updatebulletY() {
            let { bulletY, bulletX, enemiesX, enemiesY, enemyCount, aliveEnemies, score } = this.state;
            for (let i = 0; i < bulletY.length; i++) {
                if (bulletY[i] > -bulletSpeedSize) {
                    bulletY[i] = bulletY[i] - bulletSpeedSize;
                    let bx = bulletX[i];
                    let by = bulletY[i];
                    // Checking for all enemies present.
                    for (let j = 0; j < enemiesX.length; j++) {
                        let ex = enemiesX[j];
                        let ey = enemiesY[j];
                        // Check if x-coordinate distance difference between two is less than 50 and
                        // y-coordinate distance difference is smaller than 10.
                        if (aliveEnemies[j] === 1 && (bx >= ex) && (bx - ex) <= 50 && Math.abs(by - ey) <= 10) {
                            bulletY[i] = -bulletSpeedSize;
                            enemiesY[j] = this.state.bottom + enemiesSpeedSize;
                            aliveEnemies[j] = 0;
                            console.log(`Enemy ${i} dying`);
                            enemyCount--;
                            score++;
                        }
                    }
                }
            }
            this.setState({ bulletY, enemiesY, enemyCount, score, aliveEnemies });
        }
    ```

    5.2. **Check if enemies are hitting up the spaceship**: This was accomplished by putting up a check in `updateEnemiesY()`, again by the similar proximity logic.    
    ```javascript
    function updateEnemiesY() {
        let { enemiesY, enemiesX, playerStyle, bottom, lives } = this.state;
        for (let i = 0; i < enemiesY.length; i++) {
            if (enemiesY[i] > -enemiesSpeedSize) {
                enemiesY[i] = enemiesY[i] + enemiesSpeedSize;
                // Check if it collides with spaceship
                let ex = enemiesX[i];
                let ey = enemiesY[i];
                let px = playerStyle.left;
                if (ey <= bottom - 10 && ey > bottom - 20 && Math.abs(ex - px) <= 60) {
                    console.log("--------------------Spaceship Hit-----------------");
                    this.showShipBlast();
                    this.gamePause();
                    lives--;
                    this.setState({
                        lives: lives,
                        snackBarOpen: true,
                    });
                    if (lives <= 0) {
                        this.gameOver();
                    }
                }
            }
        }
        this.setState({ enemiesY });
    }
    ```

6. Last part, just render these state arrays beautifully. This is done by calling `renderBullets()` and `renderEnemies()` function in the `render()` call.
    ```javascript
    function renderBullets() {
        return this.state.bulletX.map((value, index, array) => {
            let top = ((this.state.bulletY[index]) + "px").toString();
            let left = ((this.state.bulletX[index]) + "px").toString();
            let bulletYIndex = this.state.bulletY[index];
            if (bulletYIndex > 0) {
                return this.createBullet(index, left, top);
            } else {
                return undefined;
            }
        }, this);
    }
    ```
    ```javascript
    function renderEnemies() {
        let { bottom, aliveEnemies } = this.state;
        return this.state.enemiesX.map((value, index, array) => {
            let top = (this.state.enemiesY[index] + "px").toString();
            let left = (value + "px").toString();
            let enemiesYIndex = this.state.enemiesY[index];
            if (enemiesYIndex < bottom) {
                if (aliveEnemies[index] === 1) {
                    return (
                        <div key={`enemy_${index}`} style={{ position: 'absolute', left: left, top: top, alignContent: 'center' }}>
                            <img src="assets/images/enemy.png" width="50px" alt='e' />
                        </div>
                    )
                }
            }
            else if (aliveEnemies[index] === 1 && enemiesYIndex >= bottom) {
                aliveEnemies[index] = 0;
                this.setState({
                    aliveEnemies,
                })
            }
        }, this);
    }
    ```








 <!-- So I made the Gamearea's div to be at tabIndex="0", so that it gets all priority  -->

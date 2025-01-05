# Flappy Bird Game Explanation
![image](https://github.com/user-attachments/assets/532dd9e4-e421-4044-be69-fc118cb382cd)

## Overview
This game recreates the popular **Flappy Bird** using Python's `turtle` graphics module. The player controls a bird that moves upward when the spacebar is pressed and is constantly pulled downward by gravity. The goal is to navigate through gaps in vertically aligned pipes while avoiding collisions.

---

## Key Components and Code Walkthrough

### 1. **Setup and Initialization**

- **Window Setup**
  ```python
  wn = turtle.Screen()
  wn.title("Flappy Bird by @TokyoEdTech")
  wn.bgcolor("blue")
  wn.bgpic("background.gif")
  wn.setup(width=500, height=800)
  wn.tracer(0)
  ```
  - Creates a game window with specified dimensions.
  - Sets a background color (`blue`) and a background image (`background.gif`).
  - The `tracer(0)` call turns off automatic screen updates for smoother animations.

- **Register Shapes**
  ```python
  wn.register_shape("bird.gif")
  ```
  - Registers a custom shape for the bird using an image file.

---

### 2. **Game Objects**

- **Score Display**
  ```python
  pen = turtle.Turtle()
  pen.speed(0)
  pen.hideturtle()
  pen.penup()
  pen.color("white")
  pen.goto(0, 250)
  pen.write("0", move=False, align="left", font=("Arial", 32, "normal"))
  ```
  - Creates a `pen` turtle to display the player's score on the screen.

- **Player (Bird)**
  ```python
  player = turtle.Turtle()
  player.speed(0)
  player.penup()
  player.color("yellow")
  player.shape("bird.gif")
  player.goto(-200, 0)
  player.dx = 0
  player.dy = 1
  ```
  - Creates the bird, sets its initial position (`-200, 0`), and assigns attributes for vertical movement (`dy`).

- **Pipes**
  ```python
  pipe1_top = turtle.Turtle()
  pipe1_bottom = turtle.Turtle()
  pipe2_top = turtle.Turtle()
  pipe2_bottom = turtle.Turtle()
  pipe3_top = turtle.Turtle()
  pipe3_bottom = turtle.Turtle()
  ```
  - Each pipe is represented by two turtles: a top and a bottom part.
  - Pipes are green rectangles created using the `shapesize()` method.
  - They move leftward with a speed defined by `dx = -2`.

---

### 3. **Gravity and Jump Mechanics**

- **Gravity**
  ```python
  gravity = -0.3
  ```
  - Gravity pulls the bird downward at a constant rate, reducing its vertical speed (`dy`).

- **Jump Function**
  ```python
  def go_up():
      player.dy += 8
      if player.dy > 8:
          player.dy = 8
  ```
  - Increases the bird's vertical speed (`dy`) when the spacebar is pressed, simulating a jump.
  - Limits the upward speed to prevent excessive jumping.

- **Keyboard Binding**
  ```python
  wn.listen()
  wn.onkeypress(go_up, "space")
  ```
  - Listens for the spacebar press and calls the `go_up()` function.

---

### 4. **Game Logic**

#### (a) **Main Game Loop**
```python
while True:
    time.sleep(0.02)
    wn.update()
```
- Runs the game continuously with a small delay (`0.02s`) for smooth animations.
- The screen updates are handled manually using `wn.update()`.

#### (b) **Player Movement**
```python
player.dy += gravity
y = player.ycor()
y += player.dy
player.sety(y)
```
- Updates the bird's vertical speed (`dy`) by adding gravity.
- Moves the bird by updating its vertical position (`ycor`).

#### (c) **Collision Detection**
```python
if player.ycor() < -390:
    player.dy = 0
    player.sety(-390)
```
- Prevents the bird from falling off the screen by stopping it at the bottom border.

#### (d) **Pipe Movement and Reset**
```python
for pipe_pair in pipes:
    pipe_top, pipe_bottom = pipe_pair
    x = pipe_top.xcor() + pipe_top.dx
    pipe_top.setx(x)
    pipe_bottom.setx(x)
    
    if pipe_top.xcor() < -350:
        pipe_top.setx(600)
        pipe_bottom.setx(600)
        pipe_top.value = 1
```
- Moves the pipes leftward by updating their `xcor`.
- Resets the pipes to their starting position when they move off-screen.

#### (e) **Collision with Pipes**
```python
if (player.xcor() + 10 > pipe_top.xcor() - 30) and (player.xcor() - 10 < pipe_top.xcor() + 30):
    if (player.ycor() + 10 > pipe_top.ycor() - 180) or (player.ycor() - 10 < pipe_bottom.ycor() + 180):
        pen.write("Game Over", align="center")
```
- Checks if the bird collides with any pipe by comparing their coordinates.
- Displays "Game Over" and resets the game if a collision occurs.

#### (f) **Scoring**
```python
if pipe_top.xcor() + 30 < player.xcor() - 10:
    player.score += pipe_top.value
    pipe_top.value = 0
    pen.clear()
    pen.write(player.score, align="center", font=("Arial", 32, "normal"))
```
- Increments the player's score when the bird successfully passes a pipe.
- Updates the score display.

---

## Conclusion

This game demonstrates a great blend of Python programming concepts, including:
- **Animation using `turtle`**
- **Keyboard event handling**
- **Collision detection**
- **Scorekeeping and game reset mechanics**

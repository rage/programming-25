---
path: '/part-14/2-robot-and-boxes'
title: 'Robot and boxes'
hidden: false
---

The most difficult thing to implement in a Sokoban style game tends to be moving the robot so that it can push boxes in the desired direction. The game should be able to tell when the robot can move in a direction specified, and be able to handle any situation where a box should move also. Let's tackle this challenge now.

## Handling key events

The player guides the robot with the four arrow keys, so our event handler should also be able to react to the appropriate key events:

```python
    def check_events(self):
        for event in pygame.event.get():
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    self.move(0, -1)
                if event.key == pygame.K_RIGHT:
                    self.move(0, 1)
                if event.key == pygame.K_UP:
                    self.move(-1, 0)
                if event.key == pygame.K_DOWN:
                    self.move(1, 0)

            if event.type == pygame.QUIT:
                exit()
```

Now whenever the player presses an arrow key, the method `move` is called with an appropriate pair of arguments. The first argument contains the movement in the vertical direction, while the second contains the movement in the horizontal direction.

## Searching for the robot

The game has to know the location of the robot in order to move it correctly. Let's add the method `find_robot` which figures out the location of the robot:

```python
    def find_robot(self ):
        for y in range(self.height):
            for x in range(self.width):
                if self.map[y][x] in [4, 6]:
                    return (y, x)
```

The method goes through all the squares in the game grid and returns the coordinates of the square which contains either the number 4 (the robot on its own) or the number 6 (the robot on a target square).

The idea is that whenever the player presses an arrow key, first the location of the robot is established by going through the squares of the grid. This may seem a bit slow and superfluous, as we could just as well keep the location of the robot in a separate variable or two. The advantage of this search approach is that we are not storing the location of the robot in two different locations (in the game grid _and_ separate variables), but instead we just have to worry about the one location (the game grid), which means that the state of the game in computer memory is simpler to handle.

## Changes to the game grid

We already called the method `move` above, but we haven't actually defined it yet. Let's do that now.

The `move` method takes the direction the player wants to move to as its arguments. It then updates the game grid accordingly, or determines the move is not allowed and leaves the grid unchanged.

```python
    def move(self, move_y, move_x):
        robot_old_y, robot_old_x = self.find_robot() 
        robot_new_y = robot_old_y + move_y
        robot_new_x = robot_old_x + move_x

        if self.map[robot_new_y][robot_new_x] == 1:
            return

        if self.map[robot_new_y][robot_new_x] in [3, 5]:
            box_new_y = robot_new_y + move_y
            box_new_x = robot_new_x + move_x

            if self.map[box_new_y][box_new_x] in [1, 3, 5]:
                return

            self.map[robot_new_y][robot_new_x] -= 3
            self.map[box_new_y][box_new_x] += 3

        self.map[robot_old_y][robot_old_x] -= 4
        self.map[robot_new_y][robot_new_x] += 4
```

The method has quite a lot of different stages, so let's take a look at each one in turn:

### The old and new location of the robot

```python
        robot_old_y, robot_old_x = self.find_robot() 
        robot_new_y = robot_old_y + move_y
        robot_new_x = robot_old_x + move_x
```

First, the method calls the `find_robot` in order to find the current location of the robot, before the move. This is stored in the variables `robot_old_y` and `robot_old_x`.

Then the new location of the robot after the prospective move is stored in the variables `robot_new_y` and `robot_new_x`. The new coordinates can be easily calculated by adding the values passed as arguments to the old location of the robot, as both contained vertical and horizontal values.

### Did the robot hit a wall?

```python
        if self.map[robot_new_y][robot_new_x] == 1:
            return
```

The `if` statement above takes care of the situation where the robot would hit a wall as a result of the move. Remember, 1 was the position of a wall square in the list of images. This is not allowed, so the method simply returns without any further ado.

### Moving a box

```python
        if self.map[robot_new_y][robot_new_x] in [3, 5]:
            box_new_y = robot_new_y + move_y
            box_new_x = robot_new_x + move_x

            if self.map[box_new_y][box_new_x] in [1, 3, 5]:
                return

            self.map[robot_new_y][robot_new_x] -= 3
            self.map[box_new_y][box_new_x] += 3
```

If the new prospective location of the robot contains a number 3 (a box on its own) or a number 5 (a box in a target square), the robot attempts to move the box to the next square along. For this purpose we need two new variables: `box_new_y` and `box_new_x`, which contain the location of the box after the move.

Similarly to the robot, the box cannot be moved to a wall square, with the identifier 1. Neither can the box move onto another box, or a target square with a box on it. If this would happen as a result of the move, the method again simply returns without making any changes to the grid.

In any other case the box can move. The value in the box's current grid location is decreased by 3, and the value in its new grid location is increased by 3. Because of the clever ordering of the items in the `images` list, this works out correctly both when the squares involved are floor squares and target squares.

### Moving the robot

```python
        self.map[robot_old_y][robot_old_x] -= 4
        self.map[robot_new_y][robot_new_x] += 4
```

If the execution of the method has reached this point without returning, it is time to move the robot as well. The procedure is similar to moving the box, but the value subtracted from and added to the appropriate locations in the grid is 4 this time around. This ensures, again through the clever ordering of the items in the `images` list, that the final result on the grid is correct both when floor and target squares are involved in the move.

## Refactoring?

Using only the grid to store the state of the game at all times is very handy in the sense that only one variable is permanently involved in the whole process, and it is relatively easy to update the state of the grid through simple additions and subtractions.

The downside is that it can be a tad difficult to understand the program code of the game. If someone unfamiliar with the logic used saw this following line of code, they would likely be a bit perplexed:

```python
            if self.map[box_new_y][box_new_x] in [1, 3, 5]:
```

The code snippet above makes use of _magic numbers_ to represent the squares in the grid. Anyone reading the code would have to know that 1 means wall, 3 means a box and 5 means a box in a target square.

The lines involving the clever subtractions and additions would look even more baffling:

```python
            self.map[robot_new_y][robot_new_x] -= 3
```

The number 3 meant a box just previously, but now it is subtracted from the value of a square on the grid. This works in the context of our numbering scheme, as it changes a box (3) into a normal floor square (0), or a target square with a box (5) into an empty target square (2), but understanding this requiares a primer in the numbering scheme used.

We could make it easier for anyone reading the code by _refactoring_ our implementation. That means improving the structure and readability of the code. One way to achieve this would be to use the names of the squares instead of the numbers 0 to 6, even though this would still not explain how and why numbers can be added and subtracted while maintaining the integrity of the grid. 

Making the program code truly accessible would likely require much more fundamentally transformative refactoring. For example, we could keep the structure of the game map in one location, and store the locations of the robot and the boxes in some separate data structure. The downside of _this_ would be that this would likely result in a lot more code, and the internal structure of the game would become much more complicated.

Refactoring and code quality is a subject for some subsequent courses, such as _Software Development Methods_ and _Software Engineering_.

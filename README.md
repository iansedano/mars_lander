# Mars Lander

![](https://www.dropbox.com/s/pmxvey44i0dqywn/lander.png?raw=1)

A lander AI made for a codingame competition.

[landing on high ground](https://www.codingame.com/replay/467872620)  
[landing in canyon](https://www.codingame.com/replay/467872587)  
[landing with high inital horizontal speed](https://www.codingame.com/replay/467872533)  

## The Math
The formula used to obtain the projected range of the lander based on current speed and angle of attack.

![](https://wikimedia.org/api/rest_v1/media/math/render/svg/6cdb6b0652addd09d3b722bfb7e107dff0f1364a)

Taken from this wikipedia [article](https://en.wikipedia.org/wiki/Range_of_a_projectile)

This is how I implemented it in code:
```
aoa = math.atan2(v_speed, h_speed) # angle of attack

y0 = y - target_y1  # height from target

vector_mag = math.hypot(h_speed, v_speed)

theta = aoa

g = 3.711

distance = (
    (vector_mag**2 / 2 * g) *
    (
        1 + (
            1 + (
                (2 * g * y0) / (
                    vector_mag**2 * math.sin(theta)**2)
            )
        ) ** 0.5
    )
) * (math.sin(2 * theta))

distance = (int(distance) / 10) * -1
```

The last line in theory shouldn't be necessary but I needed it to calibrate the result! I must have got my units mixed up somewhere.

The strange indentation is my way of making it more readable, but its still a bit of a mess.

## Sequence of code

1. During initlialization, the coordinates for each of the planes are given. For this challenge there is only one possible flat plane, and so the code returns the y-height and the x coordinates of the first flat plane it finds.

2. With the landing plane identified, the code checks to see if the lander is going in the right direction at a minimum velocity. If it is not, then it accelerates towards its target. Once it is going at the required velocity, then it enters the next stage.

3. The code makes the lander maintain its horizontal velocity but increases its height ever so slightly during this phase. It is also calculating its projected range using the formula above. Once the range predicts it will land in the landing zone, then the next stage is activated.

4. The code directs the lander to thrust in the direction opposite its current angle of attack. So if the lander has a lot of horizontal velocity, it starts out thrusting almost horizontally. As the horizontal velocity is diminished, the lander gradually turns vertically. It will carry on in this way until it reaches a threshold for horizontal velocity, then it enters the final stage.

5. The final stage just involves the lander pointing vertically and keeping its velocity below a threshold to ensure safe landing.

# snake-game
python game snake
import turtle
import time
import random


delay = 0.1
queue =[]

pen = turtle.Turtle()
pen.shape('square')
pen.penup()
pen.speed(0)
pen.color('White')

pen.hideturtle()
pen.goto(-290,210)
pen.write("Score: 0 " ,font=('Arial',25,'bold'))


window = turtle.Screen()
window.bgcolor('Black')
window.title('Snake Game')
window.setup(width=600,height=500)
window.tracer(0)


Snake_Head = turtle.Turtle()
Snake_Head.speed(0)
Snake_Head.shape('circle')
Snake_Head.color('white')
Snake_Head.penup()
Snake_Head.goto(0,0) 
Snake_Head.direction ='stop'


Snake_Food = turtle.Turtle()
Snake_Food.speed(0)
Snake_Food.shape('circle')

Snake_Food.color('red')
Snake_Food.penup()
Snake_Food.goto(0,150) 



def Move_Snake():
    if Snake_Head.direction == 'up':
        y = Snake_Head.ycor()
        Snake_Head.sety(y + 10)
    if Snake_Head.direction == 'down':
        y = Snake_Head.ycor()
        Snake_Head.sety(y - 10)
    if Snake_Head.direction == 'left':
        x = Snake_Head.xcor()
        Snake_Head.setx(x - 10)
    if Snake_Head.direction == 'right':
        x = Snake_Head.xcor()
        Snake_Head.setx(x + 10)


def go_up():
    Snake_Head.direction = 'up'


def go_down():
    Snake_Head.direction = 'down'


def go_left():
    Snake_Head.direction = 'left'


def go_right():
    Snake_Head.direction = 'right'


def Food_Collision():
    if Snake_Head.distance(
            Snake_Food) < 15:  
        Snake_Food.goto(random.randint(-290, 290), random.randint(-249, 249))
        Snake_body = turtle.Turtle()  
        Snake_body.speed(0)
        Snake_body.shape('circle')
        Snake_body.color('white')
        Snake_body.penup()
        queue.append(Snake_body)
        return True
    return False


def Border_Collision():
    if Snake_Head.xcor() > 290 or Snake_Head.xcor() < -290 or Snake_Head.ycor() > 249 or Snake_Head.ycor() < -249:
        time.sleep(1)
        Snake_Head.goto(0, 0)
        Snake_Head.direction = 'stop'
        for segment in queue:
            segment.goto(1000, 1000)
        queue.clear()
        return True

    return False
def Body_Collision():
    for segment in queue:
        if segment.distance(Snake_Head) < 10:
            time.sleep(1)
            Snake_Head.goto(0, 0)
            Snake_Head.direction = 'stop'
            for segment in queue:
                segment.goto(1000, 1000)
            queue.clear()
            return True
    return False


def Add_Snake_Body():
    for i in range(len(queue) - 1, 0, -1):
        if i % 5 == 0:
            queue[i].goto(queue[i - 1].xcor(), queue[i - 1].ycor())
            queue[i].color('red')
            continue
        queue[i].goto(queue[i - 1].xcor(), queue[i - 1].ycor())
    if len(queue) > 0:
        queue[0].goto(Snake_Head.xcor(), Snake_Head.ycor())



window.listen()
window.onkeypress(go_up, 'Up')
window.onkeypress(go_down, 'Down')
window.onkeypress(go_left, 'Left')
window.onkeypress(go_right, 'Right')


score = 0
while True:

    window.update()
    Move_Snake()
    if Food_Collision():
        score += 10
        pen.clear()
        pen.write('Score:{}'.format(score), font=('Arial', 25, 'bold'))

    if Body_Collision() or Border_Collision():
        score = 0
        pen.clear()
        pen.write('Score:{}'.format(score), font=('Arial', 25, 'bold'))
    time.sleep(delay)
    Add_Snake_Body()

window.mainloop()

import pygame
from Class import Zmeika
from random import randint
from time import time

pygame.init()

screen_height = 800
screen_width = 800
win = pygame.display.set_mode((screen_width, screen_height))

dotSize = 10

all_x = screen_width // dotSize
all_y = screen_height // dotSize
all_time=0
x = all_x // 3
y = all_y // 3
mut = randint(0,1)
zmei=[]
new_zmei=[]
death_list=[]
death_reason=[]
kfood=0
step=0

insert=open("insert.txt")
all_food=int(insert.readline())
energy=int(insert.readline())
insert.close()

for i in range(20):
    if x<all_x-13:
        x+=10
    else:
        y+=5
        x=all_x//5
    zmei.append(Zmeika(x,y))
    zmei[i].energy=energy

food = [] * all_y
block = [] * all_y
death=False

for i in range(all_y):
    food.append([False] * all_x)
    block.append([False] * all_x)

while kfood<all_food:
    x = randint(0, all_x - 1)
    y = randint(0, all_y - 1)
    if not food[y][x]:
        food[y][x] = True
        kfood+=1

for zmeika in zmei:
        for i in zmeika.body:
            block[i[1]][i[0]]=True

for y in range(all_y):
    block[y][0]=True
    block[y][all_x-1]=True

for x in range(all_x):
    block[0][x]=True
    block[all_y-1][x]=True

while len(zmei)!=0:
    death_list=[]
    new_zmei=[]
    insert=open("insert.txt")
    all_food=int(insert.readline())
    energy=int(insert.readline())
    disp=int(insert.readline())
    insert.close()
    if disp==1:
        pygame.time.delay(100)

    for i in range(len(zmei)):
        death=False
        dtop=block[zmei[i].head_pos[1]-1][zmei[i].head_pos[0]]
        ddown=block[zmei[i].head_pos[1]+1][zmei[i].head_pos[0]]
        dright=block[zmei[i].head_pos[1]][zmei[i].head_pos[0]+1]
        dleft=block[zmei[i].head_pos[1]][zmei[i].head_pos[0]-1]
        zmei[i].sensor_food(food)
        zmei[i].sensor_block(block)
        zmei[i].test_direction()
        zmei[i].set_direction()
        for j in zmei[i].body:
            block[j[1]][j[0]]=False
        zmei[i].change_head_pos()
        zmei[i].body_mech(food[zmei[i].head_pos[1]][zmei[i].head_pos[0]],energy)
        if food[zmei[i].head_pos[1]][zmei[i].head_pos[0]]:
            food[zmei[i].head_pos[1]][zmei[i].head_pos[0]]=False
            kfood-=1
        if zmei[i].direction=="Вверх" and dtop:
            death=True
            death_reason.append("Block")
        elif zmei[i].direction=="Вниз" and ddown:
            death=True
            death_reason.append("Block")
        elif zmei[i].direction=="Вправо" and dright:
            death=True
            death_reason.append("Block")
        elif zmei[i].direction=="Влево" and dleft:
            death=True
            death_reason.append("Block")
        elif zmei[i].head_pos[0] > x - 2 or zmei[i].head_pos[0] < 0 or zmei[i].head_pos[1] > y - 2 or zmei[i].head_pos[1] < 0:
            death=True
            death_reason.append("Block")
        elif zmei[i].energy==0:
            zmei[i].body.pop()
            zmei[i].energy=energy
            if len(zmei[i].body)==1:
                death=True
                death_reason.append("Not enough energy")
        dupl=zmei[i].mitoz()
        dupl=False
        for j in zmei[i].body:
            block[j[1]][j[0]]=True
        if step < 3:
            death= False
        if death:
            death_list.append(i)
        elif dupl:
            e=zmei[i]
            a=Zmeika(e.head_pos[0],e.head_pos[1])
            a.body=[e.body[0],e.body[1],e.body[2]]
            a.eye_food=e.eye_food
            a.eye_block=e.eye_block
            a.direction=e.direction
            if randint(0,1)==1:
                a.mutation()
            b=Zmeika(e.body[4][0],e.body[4][1])
            b.body=[e.body[4],e.body[5],e.body[6]]
            b.eye_food=e.eye_food
            b.eye_block=e.eye_block
            b.direction=e.direction
            if randint(0,1)==1:
                b.mutation()
            new_zmei.append(a)
            new_zmei.append(b)
            death_list.append(i)
            death_reason.append("Mitoz")
    death_list.reverse()
    death_reason.reverse()
    print(death_list)
    for i in new_zmei:
        zmei.append(i)
        for j in i.body:
            block[j[1]][j[0]]=True
    pygame.draw.rect(win,(255,0,0),(zmei[i].head_pos[0]*dotSize,zmei[i].head_pos[1]*dotSize,dotSize,dotSize))

    for i in range(len(death_list)):
        print(death_reason[i],end=" ")
        for j in zmei[death_list[i]].body:
            block[j[1]][j[0]]=False
        zmei.pop(i)
    print()
    
    while kfood<all_food:
        x = randint(0, all_x - 1)
        y = randint(0, all_y - 1)
        if not food[y][x]:
            food[y][x] = True
            kfood+=1

    if disp==1:
        win.fill((255,255,255))
        for y in range(all_y):
            for x in range(all_x):
                if food[y][x]:
                    pygame.draw.rect(win,(129,97,233),(x*dotSize,y*dotSize,dotSize,dotSize))
                if block[y][x]:
                    pygame.draw.rect(win,(0,0,0),(x*dotSize,y*dotSize,dotSize,dotSize))
        for x in range(0,all_x*dotSize,dotSize):
            pygame.draw.line(win,(0,0,0),(x,0),(x,all_y*dotSize))
            pygame.draw.line(win,(0,0,0),(0,x),(all_x*dotSize,x))
        for i in pygame.event.get():
            if i.type == pygame.QUIT:
                r=randint(0,len(zmei))
                a=open("output.txt","w")
                a.write(str(zmei[r].eye_block))
                a.write(str(zmei[r].eye_food))
                a.close()
                break
                exit()
        pygame.display.update()
    a=open("output.txt","w")
    a.write(str(len(zmei)))
    a.close()
    print(len(zmei),end=" ")
    step+=1
for y in range(all_y):
        for x in range(all_x):
            if food[y][x]:
                pygame.draw.rect(win,(129,97,233),(x*dotSize,y*dotSize,dotSize,dotSize))
            if block[y][x]:
                pygame.draw.rect(win,(0,0,0),(x*dotSize,y*dotSize,dotSize,dotSize))
print()
print(step)


pygame.display.quit()

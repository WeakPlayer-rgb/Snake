from random import randint

class Zmeika():
    def __init__(self, x, y):  ##Для иницалиазации змейки нужны координаты головы 
        self.head_pos = [x, y]
        self.body = [[x, y], [x, y + 1], [x, y + 2]]
        self.food = [0] * 4
        self.block = [0] * 4
        self.eye_food = [] * 8  ##0-Вверх,1-Вправо,2-Вниз,3-Влево
        self.eye_block = [] * 8  ##0-Вверх,1-Вправо,2-Вниз,3-Влево
        for x in range(11):
            self.eye_food.append([] * 8)
            self.eye_block.append([] * 8)
            for i in range(10):
                eye=[randint(0,50),randint(0,50),randint(0,50),randint(0,50)]
                block=[randint(0,50),randint(0,50),randint(0,50),randint(0,50)]
                self.eye_food[x].append(eye)
                self.eye_block[x].append(block)
        self.direction = ""
        self.set_to = ""
        self.energy = 20

    def sensor_food(self,food):
        y1,x1=0,0
        self.food=[0]*4
        for y in range(self.head_pos[1]-5,self.head_pos[1]+4):
            try:
                food[y]
                for x in range(self.head_pos[0]-5,self.head_pos[0]+4):
                    try:
                        if food[y][x]:
                            self.food[0]+=self.eye_food[y1][x1][0]
                            self.food[1]+=self.eye_food[y1][x1][1]
                            self.food[2]+=self.eye_food[y1][x1][2]
                            self.food[3]+=self.eye_food[y1][x1][3]
                    except IndexError:
                        print("",end="")
                    x1+=1
            except IndexError:
                print("",end="")
            x1=0
            y1+=1
                

    def sensor_block(self,block):  ##передаётся массив блоков (каждый блок обозначается True)
        y1,x1=0,0
        self.block=[0]*4
        for y in range(self.head_pos[1]-5,self.head_pos[1]+4):
            try:
                block[y]
                for x in range(self.head_pos[0]-5,self.head_pos[0]+4):
                    try :
                        if block[y][x]:
                            self.block[0]+=self.eye_block[y1][x1][0]
                            self.block[1]+=self.eye_block[y1][x1][1]
                            self.block[2]+=self.eye_block[y1][x1][2]
                            self.block[3]+=self.eye_block[y1][x1][3]
                    except IndexError:
                        print("",end="")
                    x1+=1
            except IndexError:
                print("",end="")
            y1+=1
            x1=0
                    
                

    def test_direction(self):
        summ = []
        arr = []
        for i in range(4):
            summ.append(self.food[i] - self.block[i])
        m=max(summ)
        for i in range(4):
            if summ[i] == m:
                arr.append(i)
        k = randint(0, len(arr) - 1)
        if arr[k] == 0:
            self.set_to = "Вверх"
        elif arr[k] == 1:
            self.set_to = "Вправо"
        elif arr[k] == 2:
            self.set_to = "Вниз"
        elif arr[k] == 3:
            self.set_to = "Влево"

    def set_direction(self):
        self.direction = self.set_to

    def change_head_pos(self):  ##Перемещение головы
        if self.direction == "Вправо":
            self.head_pos[0] += 1
        elif self.direction == "Влево":
            self.head_pos[0] -= 1
        elif self.direction == "Вверх":
            self.head_pos[1] -= 1
        elif self.direction == "Вниз":
            self.head_pos[1] += 1

    def body_mech(self, food,step):  ##Передаётся True если положение головы совпадает с положением еды
        self.body.insert(0, list(self.head_pos))
        if food:
            self.energy = step
        else:
            self.body.pop()
            self.energy -= 1

    def mitoz(self):
        if len(self.body) == 7:
            return True
        return False

    def mutation(self):
        if randint(0,1)==1:
            self.eye_food[randint(0, 9)][randint(0, 9)][randint(0, 3)] = randint(0, 99)
        else:
            self.eye_block[randint(0, 9)][randint(0, 9)][randint(0, 3)] = randint(0, 99)

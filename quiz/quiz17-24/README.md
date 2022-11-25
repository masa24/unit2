# quiz17
```.py
def ave (a):
    count = 0
    for i in a:
        for x in i:
            if x != ' ':
                count += 1
    print(count/len(a))
ave(['hello','main'])
ave(['peru','france','nepal'])
ave(['computer science','art'])
ave(['one','two'])
ave(['ba na na','ap pl e'])
```
![solution to the quiz](quiz9.png)

# quiz18

```.py
import math

def numberMatches(l,s):
    a = l/s*100/5
    num = ''
    for i in str(a):
        if i == '.':
            break
        else:
            num += i
    mod = a-int(num)
    if mod == 0:
        return int(a)
    else:
        return int(a - (a-int(num)) + 1)

print(numberMatches(100,100),'matches')
print(numberMatches(250,110),'matches')
print(numberMatches(500,150),'matches')
print(numberMatches(12345,123),'matches')
```
# quiz19
```.py
def change(word):
    new = ''
    for i in word:
        if i == 'a':
            new += '4'
        elif i == 'e':
            new += '3'
        elif i == 'i':
            new += '1'
        elif i == 'o':
            new += '0'
        elif i == ' ':
            new += '_'
        else:
            new += str(i)
    print(new)

change('Hello World')
change('Why did I choose CS?')
change('Remember the figure Caption')
```
# quiz20
```.py
def convert(a):
    if a == True:
        return '0'
    if a == False:
        return '1'
def base():
    a = False
    b = False
    c = False

    print('| A | B | C |')
    for i in range(8):
        if i % 4 == 0:
            a = not a
        if i % 2 == 0:
            b = not b
        c = not c
        print('|', convert(a), '|', convert(b), '|', convert(c), '|')
base()
```
# quiz21
```.py
def formula(a,b,c):
    return ((a and b) or (not b) or not(c and b))

def base():
    a = True
    b = True
    c = True

    print('  | A | B | C |')
    for i in range(8):
        if i % 4 == 0:
            a = not a
        if i % 2 == 0:
            b = not b
        c = not c
        print(i,'|', int(a), '|', int(b), '|', int(c), '|', int(formula(a, b, c)))

base()
```

# quiz22-23
```.py
import random
random.seed(1234)

def produce(n,m,s):
    xout = []
    yout = []
    for i in range(n):
        x = random.randint(0,100)
        xout.append(x)
        y = x**(1/2*((m/s)**2))
        yout.append(y)
    a = ' x |   y(x)\n'

    for x in range(5):
        a += str(xout[x])
        a += (' '*(3-len(str(xout[x]))))
        a += ('|  ')
        a += str(round(yout[x],2))
        a += '\n'


    return a
sample = produce(n=5, m=3, s=2)
print(sample)
```
# quiz24
```.py
from matplotlib import pyplot as plt
x = []
y = []
for i in range(-10,11):
    x.append(i)
    y.append(2*((i+5)**2))

plt.plot(x,y)
plt.xlabel('x')
plt.ylabel('y')
plt.title('quiz 24')
plt.show()
```

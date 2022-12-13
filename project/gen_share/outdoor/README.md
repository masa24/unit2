## code
```.py
from matplotlib import pyplot as plt
import requests
import math


req = requests.get('http://192.168.6.142/readings')
data = req.json()
readings = data["readings"][0]
temp = []
hum = []
count = 0

for i in readings:
    if i['sensor_id'] == 4:
        hum.append(i)
    if i['sensor_id'] == 5:
        temp.append(i)
#        count +=1

data = {'temp': temp[-576:], 'hum': hum[-576:]}
value = {'temp': [], 'hum': []}

for i in data['temp']:
    value['temp'].append(i['value'])
for i in data['hum']:
    value['hum'].append(i['value'])
print(value)


def mean(a):
    sum = 0
    for i in value[str(a)]:
        sum += i
    mean = round(sum/len(value[str(a)]),1)
    print(f'mean of {a}: {mean}')
    sumdiv = 0
    for i in value[str(a)]:
        sumdiv += (i - mean)**2
    standad_deviation = math.sqrt(sumdiv/len(value[str(a)]))
    print(f'standad deviation of {a}: {round(standad_deviation,2)}')
mean('temp')
mean('hum')

print()

def min(a):
    min = 50
    for i in value[str(a)]:
        if i < min:
            min = i
    print(f'minimum value of {a}: {min}')
min('temp')
min('hum')

def max(a):
    max = 0
    for i in value[str(a)]:
        if i > max:
            max = i
    print(f'maximum value of {a}: {max}')
max('temp')
max('hum')

def median(a):
    median = sorted(value[str(a)])
    print(f'the median of {a}: {median[288]}')
median('temp')
median('hum')



def plot(a):
    if a == 'temp':
        c = 'r'
    elif a == 'hum':
        c = 'b'
    plt.plot(value[str(a)],color = c)

plot('temp')
plot('hum')
plt.ylabel('red = tempareture\nblue = humidity')
plt.title('indoor data')
plt.show()
```
## result
mean of temp: 24.5

standad deviation of temp: 2.19

mean of hum: 27.2

standad deviation of hum: 5.39



minimum value of temp: 18.0

minimum value of hum: 20.0

maximum value of temp: 29.0

maximum value of hum: 44.0

the median of temp: 25.0

the median of hum: 25.0

## graph
![image](https://user-images.githubusercontent.com/49806189/207244077-02157885-4bce-4f81-8671-88a8d96850e5.png)


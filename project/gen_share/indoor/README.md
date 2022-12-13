## code
```.py
from matplotlib import pyplot as plt
import requests
import math
import datetime
import time

user = {'username': 'masamu','password':'123'}
req = requests.post('http://192.168.6.142/login',json = user)
access_token = req.json()['access_token']
#print(req.json())
auth = {"Authorization": f"Bearer {access_token}"}

r = requests.get('http://192.168.6.142/user/readings', headers=auth)

#print(len(r.json())/6)
value_temp = {'t1':[],'t2':[],'t3':[],'t4':[]}
value_hum = {'h1':[],'h2':[],'h3':[],'h4':[]}

for i in r.json():
    if int(i['id']) >0:
        if int(i['sensor_id']) == 472:
            value_hum['h1'].append(i['value'])
        elif int(i['sensor_id']) == 473:
            value_hum['h2'].append(i['value'])
        elif int(i['sensor_id']) == 474:
            value_hum['h3'].append(i['value'])
        elif int(i['sensor_id']) == 475:
            value_hum['h4'].append(i['value'])
        elif int(i['sensor_id']) == 476:
            value_temp['t1'].append(i['value'])
        elif int(i['sensor_id']) == 477:
            value_temp['t2'].append(i['value'])
        elif int(i['sensor_id']) == 478:
            value_temp['t3'].append(i['value'])
        elif int(i['sensor_id']) == 479:
            value_temp['t4'].append(i['value'])

for i in value_temp:
    value_temp[str(i)] = value_temp[str(i)][-576:]
for i in value_hum:
    value_hum[str(i)] = value_hum[str(i)][-576:]

print(value_hum)
print(value_temp)
ave = {'temp':[],'hum':[]}


for i in range(0,576):
    sum = 0
    for x in value_temp:
        sum += value_temp[x][i]
    ave['temp'].append(round(sum/4,2))
for i in range(0, 576):
    sum = 0
    for x in value_hum:
        sum += value_hum[x][i]
    ave['hum'].append(round(sum / 4, 2))
print(ave)

def plot_ave():
    for i in ave:
        plt.plot(ave[str(i)])
        plt.title(f'average {str(i)}')
        plt.show()
plot_ave()

def mean():
    for i in ave:
        sum = 0
        for x in ave[i]:
            sum += x
        mean = sum/len(ave[i])
        print(f'the average of {i} is {mean}')
    sumdiv = 0
    for i in ave:
        sumdiv += (i - mean) ** 2
        standad_deviation = math.sqrt(sumdiv / len(i))
    print(f'the standad deviation of {i} is {standad_deviation}')
mean()

def min():
    for i in ave:
        min = 50
        for x in ave[i]:
            if x < min:
                min = x
        print(f'the minimum value of {i} is {min}')
min()

def max():
    for i in ave:
        max = 0
        for x in ave[i]:
            if x > max:
                max = x
        print(f'the maximum value of {i} is {max}')
max()

def median():
    for i in ave:
        median = sorted(i)
        print(f'the median of {i} is {median[288]}')
median()

def graph(a):
    for i in a:
        plt.plot(a[str(i)])
    plt.title('temperature for each sensor')
    plt.ylabel('temperature(C)')
    plt.show()
graph(value_temp)
```

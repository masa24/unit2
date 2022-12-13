## code
```.py
from matplotlib import pyplot as plt
import requests
import math
import datetime
import time
import numpy as np

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
        for x in ave[i]:
            sumdiv += (x - mean) ** 2
            standad_deviation = math.sqrt(sumdiv / len(i))
    print(f'the standad deviation of average {i} is {standad_deviation}')
mean()

def min():
    for i in ave:
        min = 50
        for x in ave[i]:
            if x < min:
                min = x
        print(f'the minimum value of average {i} is {min}')
min()

def max():
    for i in ave:
        max = 0
        for x in ave[i]:
            if x > max:
                max = x
        print(f'the maximum value of average {i} is {max}')
max()

def median():
    for i in ave:
        median = sorted(ave[i])
        print(f'the median of average {i} is {median[288]}')
median()

def graph(a):
    if a == value_temp:
        name = 'temperature'
        unit = 'C'
    if a == value_hum:
        name = 'humidity'
        unit = '%'
    for i in a:
        plt.plot(a[str(i)])
    plt.title(f'{name} for each sensor')
    plt.ylabel(f'{name}({unit})')
    for i in a:
        z = []
        for x in range(0,576):
            z.append(x)
        m, b, o, k = np.polyfit(z, a[i], 3)
        print(f'function for {i}')
        print(m, b, o, k)
        print('')
        zy = []
        for x in range(len(z)):
            zy.append(m * x ** 3 + b * x ** 2 + o * x ** 1 + k)
        plt.plot(zy)
        pre_x = []
        pre_y = []
        for i in range(576, 576 + 12 * 12):
            pre_x.append(i)
        for i in pre_x:
            pre_y.append(m * i ** 3 + b * i ** 2 + o * i ** 1 + k)
        plt.plot(pre_x, pre_y)

    plt.show()
graph(value_temp)
graph(value_hum)

   #     print(f'\n{datetime.datetime.now()}\n\n')

```
## result

the average of temp is 24.6875
the average of hum is 21.786024305555557
the standad deviation of average hum is 82.452322081714
the minimum value of average temp is 18.5
the minimum value of average hum is 20.0
the maximum value of average temp is 28.0
the maximum value of average hum is 52.0
the median of average temp is 25.0
the median of average hum is 20.0
function for t1
-1.3366302570644902e-07 0.00013040309800325963 -0.02947645666034394 25.263200665509498

function for t2
-1.4960101604489952e-07 0.00014012467054260225 -0.03003877805614878 25.241554162508606

function for t3
-1.2605386785874063e-07 0.00011463948793833798 -0.021809425632876692 24.065657445186066

function for t4
-1.8524444141981163e-07 0.00015913163539969327 -0.03046560323093323 24.587966351823543

function for h1
1.590675798723074e-07 -0.00017329649078573966 0.048258985284268356 19.884673144319873

function for h2
1.799170895239079e-07 -0.0001606946446169988 0.039072404116739734 18.943481157589392

function for h3
-7.804013933912029e-08 0.00010278678629683801 -0.03177265137300652 22.680720274635206

function for h4
1.4753813620259307e-07 -7.08959508039189e-05 0.014504431361656557 19.36712353683



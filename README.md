# Python code

import numpy as np
import matplotlib.pyplot as plt
import random

# Построение оси абсцисс
x = np.linspace(0,4,num=400)
# Построение исходной функции
n = len(x)
nquart = int(np.floor(n/4))
f = np.zeros_like(x)
f[0:nquart] = 2*x[0:nquart]
f[nquart:2*nquart] = 2-x[nquart:2*nquart]
f[2*nquart:3*nquart] = x[2*nquart:3*nquart]-4
f[3*nquart:4*nquart] = 0
# Выбор числа гармоник
NK = 15
# Подготовка к вычислению коэффициентов при армониках
K = np.arange(0.0,NK,1)
A = np.zeros_like(K)
B = np.zeros_like(K)
dx = 0.01
L = 2*np.pi
# Вычисление "нулевого" коэффициента
print('№ гармоники','\t','Коэф. при cos','\t','Коэф. при sin')
A0 = np.sum(f*np.ones_like(x))*dx*2/L
print(0,'\t\t','%.5f'%A0)
fFS = A0/2*np.ones_like(f)
# Вычисление коэффициентов при гармониках
A[0]=A0
for k in range(1,NK):
    Ak = np.sum(f*np.cos(k*x))*dx*2/L
    Bk = np.sum(f*np.sin(k*x))*dx*2/L
    fFS = fFS + Ak*np.cos(k*x) + Bk*np.sin(k*x)
    A[k] = Ak
    B[k] = Bk
    print(k,'\t\t','%.5f'%Ak,'\t\t','%.5f'%Bk)
# Расчет СКО от значения функции и Фурье образа
print('СКО значений востановленной функции от исходной %.5f'%(np.std(f-fFS)))
# Подготовка к построению графиков
plt.rcParams['figure.figsize'] = [8, 6]
plt.rcParams.update({'font.size': 14})
# Построение графиков коэффициентов разложения от гармоник
plt.plot(K,A,color='b',LineWidth=2, label='cos - Ak')
plt.plot(K,B,color='r',LineWidth=2, label='sin - Bk')
plt.legend()
plt.xlabel('Номер гармоники')
plt.ylabel('Величина коэффициента')
plt.show()
# Построение графика исходной функции и результата востановления из Фурье образа
plt.plot(x,f,color='k',LineWidth=2, label='Исходная функция')
plt.plot(x,fFS,'-',color='r',LineWidth=1.5, label='Восстановленная функция')
plt.legend()
plt.xlabel('Координата х')
plt.ylabel('Значение функции')
plt.show()

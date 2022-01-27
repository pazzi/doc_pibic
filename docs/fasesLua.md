## Geração de dados sobre fases da lua em determinado período de tempo

Utilizou-se código *python* (abaixo) e a biblioteca *skyfield* para a geração de dados.
Informações sobre o uso da biblioteca *skyfield* podem ser obtidos em [https://rhodesmill.org/skyfield/](https://rhodesmill.org/skyfield/).

```
from skyfield import api
from skyfield import almanac

ts = api.load.timescale()
eph = api.load('de421.bsp')

t0 = ts.utc(1917, 1, 1)
t1 = ts.utc(2019, 12, 31)
t, y = almanac.find_discrete(t0, t1, almanac.moon_phases(eph))

lista=t0
i=0

with open('saida.csv', 'w') as f:
    while (i <  len(y)) :
        f.write(f'{(t.utc_iso()[i])[0:10]};{y[i]}')
        f.write('\n')
        i+=1
```

No código o período de tempo a ser obtido foi de 01/01/1917 até 31/12/2019. 
O *script* gera como saida o arquivo saida.csv com campos separados por ";" e com o seguinte *layout*:

| data | lua |
|------|-----|
|1917-01-08|2|

Onde o campo lua tem a seguinte correspondência:

- 0 - Nova

- 1 - Crescente

- 2 - Cheia

- 3 - Minguante


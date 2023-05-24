# KPI 
### Reducir en un 5% la tasa de mortalidad a nivel anual en los accidentes aéreos 

Es un indicador clave que busca medir la eficacia de las medidas de seguridad y la gestión de riesgos en la industria de la aviación. La importancia de este KPI radica en varios aspectos:

- Seguridad de los pasajeros: La principal preocupación de la industria aérea es garantizar la seguridad de los pasajeros. Reducir la tasa de mortalidad en los accidentes aéreos implica minimizar el riesgo de pérdida de vidas humanas, lo cual es fundamental para mantener la confianza de los viajeros y preservar la reputación de las aerolíneas.

- Mejora continua: Establecer la meta de reducir en un 5% la tasa de mortalidad anual en los accidentes aéreos implica un enfoque de mejora continua. Esto impulsa a las aerolíneas y a las autoridades reguladoras a trabajar en conjunto para implementar medidas de seguridad más efectivas, investigar accidentes y aprender de ellos para evitar su repetición en el futuro.

- Eficiencia operativa: La reducción de la tasa de mortalidad en los accidentes aéreos también puede tener un impacto positivo en la eficiencia operativa de las aerolíneas. Los accidentes aéreos pueden tener consecuencias económicas significativas, como daños a la propiedad y aeronaves, interrupción de las operaciones y demandas legales. Al reducir la tasa de mortalidad, se pueden minimizar estos impactos y mantener la continuidad de las operaciones de manera más efectiva.

- Cumplimiento de regulaciones: La aviación es una industria altamente regulada, y la reducción de la tasa de mortalidad en los accidentes aéreos puede ser un indicador importante para cumplir con los estándares y regulaciones establecidos por las autoridades de aviación civil. Además, la mejora en la seguridad puede llevar a una mayor armonización de los estándares internacionales y fortalecer la cooperación entre las diferentes partes interesadas en la industria de la aviación.



```python
import pandas as pd
import numpy as np
import matplotlib.pylab as pl
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec
import seaborn as sns
import calendar
import locale

df_1 = pd.read_csv('C:\\Henry\\Segundo proyecto individual\\PI03-Analytics\\datos_complementarios_finales.csv', delimiter=';')
df_1 = df_1.drop(['Unnamed: 0'], axis = 1)
df_1['Fecha'] = pd.to_datetime(df_1['Fecha'])

# Establecer la configuración regional en español
locale.setlocale(locale.LC_ALL, 'es_ES.UTF-8')

# Analsis por año
plt.figure(figsize=(12, 6))
df_1['Fecha'].dt.year.value_counts().sort_index().plot(kind='line', marker='o')
plt.xlabel('Año')
plt.ylabel('Cantidad')
plt.title('Número de accidentes por año')

# Analisis por mes
plt.figure(figsize=(12, 6))
df_1['Fecha'].dt.month.value_counts().sort_index().plot(kind='line', marker='o')
plt.xlabel('Mes')
plt.ylabel('Cantidad')
plt.title('Número de accidentes por mes')

# Cambiar los nombres de los meses a español
meses = [calendar.month_name[i] for i in range(1, 13)]
plt.xticks(ticks=np.arange(12), labels=meses)

plt.tight_layout()
plt.show()

# Analisis por día
plt.figure(figsize=(12, 6))
df_1['Fecha'].dt.dayofweek.value_counts().sort_index().plot(kind='line', marker='o')
plt.xlabel('Día')
plt.ylabel('Cantidad')
plt.title('Número de incidentes por día')

# Cambiar los nombres de los días de la semana a español
dias_semana = list(calendar.day_name)
plt.xticks(ticks=np.arange(7), labels=dias_semana)

plt.tight_layout()
plt.show()



```


```python
import pandas as pd
df = pd.read_csv('datos_complementarios_finales.csv', delimiter=';')
df['Fecha'] = pd.to_datetime(df['Fecha'])
```


```python
# Agregar la columna de año
df['Año'] = pd.to_datetime(df['Fecha']).dt.year

# Calcular el total de pasajeros sumando las columnas correspondientes
df['Total_pasajeros'] = (
    df['Total de heridos graves'] +
    df['Total de heridos leves'] +
    df['Total de no heridos'] +
    df['Total heridos mortales']
)

# Agrupar por año y calcular el porcentaje de heridos mortales
df_agrupado = df.groupby('Año').sum()
df_agrupado['Porcentaje_heridos_mortales'] = (
    df_agrupado['Total heridos mortales'] / df_agrupado['Total_pasajeros']
) * 100

# Crear el nuevo dataframe para Power BI
df_powerbi = pd.DataFrame({
    'Año': df_agrupado.index,
    'Porcentaje_heridos_mortales': df_agrupado['Porcentaje_heridos_mortales']
})

# Mostrar el nuevo dataframe
print(df_powerbi)
df_powerbi.to_csv('datos_kpi_1.csv', index=False)


```

### Mantener el involucramiento de cualquier aeropuerto en accidentes aéreos por debajo del 5%

El establecimiento de un KPI (indicador clave de rendimiento) que busca mantener el involucramiento de cualquier aeropuerto en accidentes aéreos por debajo del 5% es de suma importancia por  razones como:

- Seguridad de los pasajeros: El objetivo principal de cualquier aeropuerto es garantizar la seguridad de los pasajeros y la tripulación. Al establecer un KPI que busca minimizar el involucramiento de los aeropuertos en accidentes aéreos, se está priorizando la seguridad y se busca reducir los riesgos asociados a las operaciones aéreas.

- Preservación de vidas: Los accidentes aéreos pueden tener consecuencias trágicas, tanto en términos de pérdida de vidas humanas como de daños materiales. Al mantener un bajo porcentaje de implicación de los aeropuertos en accidentes, se contribuye a preservar vidas y evitar tragedias.

- Reputación de la industria: Los accidentes aéreos pueden tener un impacto negativo significativo en la reputación de la industria de la aviación. Establecer y alcanzar un KPI que busca mantener un bajo porcentaje de implicación de los aeropuertos en accidentes demuestra el compromiso de la industria con la seguridad y genera confianza tanto en los pasajeros como en las aerolíneas y otros actores involucrados.

- Cumplimiento de normativas y regulaciones: La aviación está sujeta a una estricta regulación y normativas que buscan garantizar la seguridad operacional. Al establecer un KPI relacionado con la minimización de accidentes aéreos, los aeropuertos demuestran su compromiso con el cumplimiento de dichas regulaciones y contribuyen al mantenimiento de estándares de seguridad adecuados.


```python

resultados = pd.DataFrame(columns=['Año', 'Aeropuerto', 'Porcentaje'])

for year in df['Fecha'].dt.year.unique():
    df_año = df[df['Fecha'].dt.year == year]
    total_aeropuertos = df_año['Aeropuerto'].nunique()
    aeropuertos_con = df_año['Aeropuerto'].value_counts()
    aeropuertos_por = aeropuertos_con / total_aeropuertos * 100
    
    aeropuerto_mas_alto = aeropuertos_por.idxmax()
    porcentaje_mas_alto = aeropuertos_por.max()
    
    resultados = resultados.append({'Año': year, 'Aeropuerto': aeropuerto_mas_alto, 'Porcentaje': porcentaje_mas_alto}, ignore_index=True)

```


```python
resultados.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Año</th>
      <th>Aeropuerto</th>
      <th>Porcentaje</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1982</td>
      <td>BOISE AIR TERMINAL</td>
      <td>2.439024</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1992</td>
      <td>LAGUARDIA</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1993</td>
      <td>PORT LIONS</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2005</td>
      <td>Teterboro NJ</td>
      <td>50.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2008</td>
      <td>None</td>
      <td>0.681431</td>
    </tr>
  </tbody>
</table>
</div>




```python
resultados.to_csv('datos_kpi_2.csv', index=False)

```

### Mantener el involucramiento de cualquier compañia en accidentes aéreos por debajo del 5%

Para las compañías aéreas, también es de vital importancia establecer KPIs relacionados con la seguridad y la prevención de accidentes aéreos. Algunas razones por las cuales estos KPIs son importantes incluyen:

- Seguridad de los pasajeros y la tripulación: Al igual que los aeropuertos, las compañías aéreas tienen la responsabilidad de garantizar la seguridad de sus pasajeros y tripulación. Establecer KPIs que busquen reducir el número de accidentes aéreos es esencial para proteger la vida y la integridad de las personas a bordo de las aeronaves.

- Cumplimiento de regulaciones y estándares: La industria de la aviación está sujeta a regulaciones estrictas y estándares de seguridad establecidos por las autoridades aeronáuticas y organismos internacionales. El establecimiento de KPIs relacionados con la prevención de accidentes aéreos ayuda a las compañías aéreas a cumplir con estas regulaciones y a mantener altos estándares de seguridad operacional.

- Protección de la reputación de la compañía: Los accidentes aéreos pueden tener un impacto devastador en la reputación de una compañía aérea. Establecer y lograr KPIs que demuestren un bajo índice de accidentes aéreos ayuda a proteger la reputación de la compañía, generando confianza tanto en los pasajeros como en los inversores y otros actores del mercado.

- Eficiencia y rentabilidad: La prevención de accidentes aéreos también puede tener un impacto positivo en la eficiencia y la rentabilidad de las compañías aéreas. Los accidentes aéreos implican costos significativos, tanto en términos de indemnizaciones y responsabilidades legales como en términos de daños a la flota de aeronaves. Al establecer KPIs que busquen reducir los accidentes, las compañías aéreas pueden evitar estos costos y mantener una operación más eficiente y rentable.

- Competitividad en el mercado: La seguridad es un factor crucial que los pasajeros consideran al elegir una compañía aérea. Establecer KPIs orientados a la prevención de accidentes aéreos puede ayudar a las compañías aéreas a diferenciarse y ser más competitivas en el mercado, al demostrar su compromiso con la seguridad y la protección de sus clientes.


```python
compañia = pd.DataFrame(columns=['Año', 'Compañía aérea', 'Porcentaje'])

for year in df['Fecha'].dt.year.unique():
    df_año = df[df['Fecha'].dt.year == year]
    total_companias = df_año['Compañía aérea'].nunique()
    compañias_con = df_año['Compañía aérea'].value_counts()
    compañias_por = compañias_con / total_companias * 100
    
    compania_mas_alta = compañias_por.idxmax()
    porcentaje_mas_alto = compañias_por.max()
    
    compañia = compañia.append({'Año': year, 'Compañía aérea': compania_mas_alta, 'Porcentaje': porcentaje_mas_alto}, ignore_index=True)

```


```python
compañia.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Año</th>
      <th>Compañía aérea</th>
      <th>Porcentaje</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1982</td>
      <td>United Airlines</td>
      <td>1.626016</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1992</td>
      <td>Usair, Inc.</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1993</td>
      <td>Peninsula Airways (dba: Penair)</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2005</td>
      <td>Platinum Jet Managment</td>
      <td>50.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2008</td>
      <td>Pilot</td>
      <td>1.451613</td>
    </tr>
  </tbody>
</table>
</div>




```python
compañia.to_csv('datos_kpi_3.csv', index=False)
```

Los tres KPI tienen una importancia crucial en la seguridad y la gestión de riesgos en la industria de la aviación. Al considerarlos en conjunto, se logra una evaluación más completa y efectiva de la seguridad aérea. A continuación, se explica la importancia de cada uno:

1. Reducción de la tasa de mortalidad en accidentes aéreos: Este KPI se enfoca en minimizar el impacto humano de los accidentes aéreos. La reducción de la tasa de mortalidad anual en un 5% implica implementar medidas preventivas y mejoras en la seguridad de las operaciones aéreas. Esto incluye aspectos como la mejora de la capacitación de pilotos y tripulación, el fortalecimiento de los sistemas de mantenimiento de aeronaves, la implementación de tecnologías avanzadas de seguridad y la adopción de mejores prácticas operativas. La reducción de la tasa de mortalidad muestra el compromiso de la industria de la aviación en garantizar la seguridad y la protección de los pasajeros y la tripulación.

2. Mantener el involucramiento de cualquier aeropuerto en accidentes aéreos por debajo del 5%: Este KPI se enfoca en evaluar la seguridad y las medidas preventivas implementadas en los aeropuertos. Un aeropuerto seguro es esencial para garantizar operaciones aéreas seguras. Mantener el involucramiento de cualquier aeropuerto en accidentes aéreos por debajo del 5% implica una gestión efectiva de la seguridad en el diseño y mantenimiento de la infraestructura aeroportuaria, así como en la implementación de procedimientos de seguridad y emergencia robustos. Esto incluye la capacitación adecuada del personal del aeropuerto, la implementación de tecnologías de detección y seguridad, y el cumplimiento de normas y regulaciones internacionales de aviación. 

3. Mantener el involucramiento de cualquier compañía en accidentes aéreos por debajo del 5%: Este KPI se enfoca en evaluar la seguridad y el desempeño de las compañías aéreas. Las aerolíneas desempeñan un papel crucial en la seguridad de la aviación al asegurarse de que sus aeronaves estén en buenas condiciones de funcionamiento y que los procedimientos de seguridad se cumplan rigurosamente. Mantener el involucramiento de cualquier compañía en accidentes aéreos por debajo del 5% implica una gestión eficaz de la seguridad, la implementación de programas de mantenimiento sólidos, la inversión en tecnología y sistemas de seguridad avanzados, y la capacitación continua del personal. Además, se deben establecer mecanismos de monitoreo y supervisión para garantizar el cumplimiento de los estándares de seguridad establecidos.

En conjunto, estos tres KPI refuerzan la cultura de seguridad en la industria de la aviación, fomentando la adopción de mejores prácticas y la implementación de medidas preventivas para reducir los accidentes aéreos y minimizar su impacto. Al centrarse en la tasa de mortalidad, el involucramiento de aeropuertos y el involucramiento de compañías en accidentes aéreos, se abordan diferentes aspectos de la seguridad aérea y se prom

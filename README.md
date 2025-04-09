# Proyecto final. Telecomunicaciones: Identificación de operadores ineficaces.

El servicio de telefonía virtual CallMeMaybe está desarrollando una nueva función que brindará a los supervisores y las supervisores información sobre los operadores menos eficaces. Esto, con el fin de implementar nuevas estrategias para aumentar la productividad de los operadores y a su vez la satisfacción en los clientes.

## Objetivo General: Identificar operadores ineficaces
Para detectar los operadores ineficaces se tomaron lo siguientes criterios: 

- Si tiene una gran cantidad de llamadas entrantes perdidas (internas y externas).
- Un tiempo de espera prolongado para las llamadas entrantes. 
- Un número reducido de llamadas salientes.

## Descripción de los datos: 

Los datasets contienen información sobre el uso del servicio de telefonía virtual CallMeMaybe. Sus clientes son organizaciones que necesitan distribuir gran cantidad de llamadas entrantes entre varios operadores, o realizar llamadas salientes a través de sus operadores. Los operadores también pueden realizar llamadas internas para comunicarse entre ellos. Estas llamadas se realizan a través de la red de CallMeMaybe.

El dataset comprimido `telecom_dataset_us.csv` contiene las siguientes columnas:

- `user_id`: ID de la cuenta de cliente
- `date`: fecha en la que se recuperaron las estadísticas
- `direction`: "dirección" de llamada (`out` para saliente, `in` para entrante)
- `internal`: si la llamada fue interna (entre los operadores de un cliente o clienta)
- `operator_id`: identificador del operador
- `is_missed_call`: si fue una llamada perdida
- `calls_count`: número de llamadas
- `call_duration`: duración de la llamada (sin incluir el tiempo de espera)
- `total_call_duration`: duración de la llamada (incluido el tiempo de espera)

 

El conjunto de datos `telecom_clients_us.csv` tiene las siguientes columnas:

- `user_id`: ID de usuario/a
- `tariff_plan`: tarifa actual de la clientela
- `date_start`: fecha de registro de la clientela

## EDA
### Análisis Exploratorio de Datos

En esta sección, tal como lo indica el objetivo, se busca identificar operadores ineficaces mediante el análisis de tres métricas clave: 

1. **Cantidad de llamadas perdidas**  
   Para esta métrica, se analizarán las llamadas perdidas, diferenciándolas en tres categorías:  
   - Llamadas perdidas internas por operador.  
   - Llamadas perdidas externas por operador.  
   - Llamadas perdidas totales por operador.  

   Se debe generar una gráfica que destaque a los operadores con mayor cantidad de llamadas perdidas, enfocándose en la columna de llamadas totales. Además, es fundamental identificar los parámetros normales para esta métrica, por lo que se elaborará un diagrama de caja. Esto permitirá visualizar a los operadores que se encuentran fuera del rango habitual y establecer a partir de qué valores se les puede considerar ineficaces.  

2. **Tiempo de espera prolongado en llamadas entrantes**  
   En esta métrica, se analizará únicamente el tiempo de espera de las llamadas entrantes, excluyendo las realizadas por los operadores. Para ello, se aplicará un filtro que elimine las llamadas salientes. Luego, se graficarán los operadores con los mayores tiempos de espera en las llamadas entrantes. Finalmente, se generará un diagrama de caja para evaluar el comportamiento de los operadores en esta métrica y determinar los valores aceptables, a fin de identificar a quienes se consideren ineficaces.
3. **Cantidad de llamadas realizadas por los operadores**  
   En esta métrica, se distinguirá entre llamadas realizadas y recibidas. Una vez aplicado el filtro correspondiente, se analizarán los datos y se generará una gráfica que muestre a los operadores con menor cantidad de llamadas realizadas. Posteriormente, se utilizará un diagrama de caja para visualizar los valores típicos esperados en esta métrica.  

### Conclusión  
Al completar el análisis de estas métricas, se podrá crear un nuevo DataFrame que identifique a los operadores que incumplen con los tres criterios mencionados. Esto permitirá determinar el tipo de capacitación que los supervisores deberán proporcionar para mejorar su desempeño.

### Procesamiento de Datos  

Para el procesamiento de los datos, se llevarán a cabo las siguientes acciones:  

1. **Eliminación de datos duplicados**  
   Se eliminarán los datos duplicados, ya que podrían generar incoherencias en el análisis. Dado que la columna 'date' incluye la hora en que se realizó cada llamada, un dato duplicado podría indicar un error en la recopilación de información. Es recomendable consultar con los ingenieros de datos para determinar la causa de estos duplicados y confirmar cómo manejarlos.  

2. **Manejo de valores nulos**  
   El tratamiento de los valores nulos dependerá de la columna en cuestión:  
   - En algunas métricas, se puede completar con el valor más común de la columna.  
   - Para columnas como la de operadores, se puede asignar un valor como "desconocido", ya que al ser un string, no afectará el análisis.  

3. **Uso del tipo de datos adecuado**  
   - Cada columna deberá tener un tipo de dato correcto. Por ejemplo, las columnas de fechas deben transformarse a formatos adecuados para facilitar su manipulación.  
   - Si es necesario, se crearán columnas adicionales para representar el día y el mes. Esto permitirá analizar el comportamiento de operadores que hayan sido agregados en el transcurso del estudio. 

4. **Transformación de columnas categóricas**  
   - Las columnas que contienen valores como 'in' y 'out' (para llamadas entrantes y salientes) serán transformadas a valores binarios (1 y 0).  
   - Se utilizará una documentación adecuada (por ejemplo, un markdown) para explicar estas transformaciones. Esto simplificará la estructura de los datos y facilitará la creación de filtros y análisis posteriores.  
   - Estos valores transformados también se emplearán para generar nuevas columnas y realizar operaciones matemáticas, como sumas, restas o multiplicaciones.  

### Objetivo  
Estas acciones garantizarán que los datos estén listos para el análisis, reduciendo errores y facilitando el manejo de las métricas clave. 

## Pruebas de Hipótesis  

Las pruebas de hipótesis se realizarán de la siguiente manera:  

#### **Hipótesis 1:**  
No existe una relación significativa entre la duración total de una llamada y el tiempo de espera de un cliente. En otras palabras, la duración de una llamada no influye en el tiempo que un cliente pasa esperando para ser atendido.  

- **Hipótesis nula (H₀):** No existe una relación significativa entre la duración total de las llamadas y el tiempo de espera. Es decir, la duración de una llamada no afecta el tiempo de espera de los clientes.  
- **Hipótesis alternativa (H₁):** Existe una relación significativa entre la duración total de las llamadas y el tiempo de espera de los clientes.  

#### **Hipótesis 2:**  
El aumento en el volumen de llamadas genera un incremento en los tiempos de espera, lo que sugiere que un mayor número de llamadas podría saturar el sistema y alargar los tiempos de espera.  

- **Hipótesis nula (H₀):** No existe una relación entre el número de llamadas (calls_count) y el tiempo de espera (waiting_time).  
- **Hipótesis alternativa (H₁):** A mayor número de llamadas, mayor tiempo de espera, indicando una correlación positiva entre estas variables.  

### Objetivo  
Estas hipótesis serán evaluadas mediante pruebas estadísticas apropiadas, como análisis de correlación o regresión, para determinar si existe evidencia significativa que respalde o rechace las hipótesis nulas.
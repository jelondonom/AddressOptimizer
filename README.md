# AddressOptimizer
Optimización de rutas para paradas de doctores. Similar al clásico problema matemático conocido como 'Problema del Viajero' o Travelling Salesman Problem (TSP). Usando las librerias 'Pandas' y la API de Google Maps.

En este repositorio podremos encontrar tanto la fase inicial como la fase final de una optimización de rutas la cual comenzamos a diseñar con la ayuda de algunos compañeros de trabajo, con el fin de optimizar los tiempos en ruta, al igual que la cantidad de paradas, para doctores que ofrecen servicios en casa a múltiples pacientes en múltiples zonas de Houston, Texas.

# Lógica del código
## Fase 1
La idea principal es calcular para cada area segmentada en la cual los doctores visitan a los pacientes, la ruta más eficiente u óptima en términos de tiempo en ruta o cantidad de paradas. Esto logrado al tomar cada dirección exacta de cada paciente, y luego convirtiendo estas mismas a *latitud y longitud*. 

Una vez lograda la conversión, se puede aplicar una función creada donde, con la ayuda de la **fórmula Haversine**, resolvemos el problema del viajero (También conocido como Travelling Salesman Problem o por sus siglas, TSP).

## Fase 2
Seguidamente, considerando que muchos de los pacientes visitados pueden vivir en centros de atención u hogares de hospicio, agrupamos por dirección. De manera tal que el TSP operando sobre puntos geográficos, al encontrar dos pacientes que habiten en una misma dirección, los agrupa a *un solo punto y no dos*.

## Fase 3
Habiendo ya limpiado el dataset original. Implementamos la fórmula Haversine y no una fórmula más común (Como la fórmula Euclidiana que deriva del teorema de Pitagorás y que tiene un planteamiento muy válido) ya que las distancias que comprenden todo el mapa de servicio cuentan con más de 100.000 kilómetros, es por esto que en este caso específico, la fórmula de Haversine es nuestra opción escogida.
Luego aplicamos la fórmula en diferentes partes de la **nueva función 'solveTSP'. Esta función nos permite optimizar por la menor distancia de latitud y longitud encontrada entre múltiples direcciones.

## Fase 4
Finalmente se reconstruye un nuevo dataset en donde **todas las zonas** que se visitan son separadas por 'color único' creando una hoja para cada uno de estos colores (subgrupos). En donde podemos encontrar el orden de la ruta ideal que al visitar en ese dado orden, maximiza la eficiencia en las visitas.

Inicialmente comenzamos con la fase de 'Batched' la cual ha servido como el piloto para implementar el código y adaptarlo a la necesidad de una pequeña cantidad de una base de datos más grande.
Esto se hace con el fin de no *'malgastar' tantos tokens de la API de Google Maps* para no generar un cobro innecesariamente. Es por esto que inicialmente ha sido implementado con tan solo el 5% del total de direcciones / pacientes.

En el repositorio se encontrarán ambos tipos de archivos, **se pueden identificar por el nombre, aquellos que digan *batched* serán entonces las pruebas iniciales y piloto del proyecto de optimización de rutas.**

# Descripción del dataset
'Encrypted Chart Number': Dado que debemos velar por la privacidad y seguridad de la información contenida en este dataset, encriptamos por segunda vez el número identificador del paciente de manera que solo el equipo de trabajo cuenta con la solución al encriptado y puedan ser aplicados los descubrimientos de este proyecto. 

'Patient Zip Code': Número de 'zipcode' específico (con 4 dígitos extras).

'Appointment Date': Última fecha en la que el paciente fue visto. Esto nos permite asegurarnos de que no habrán pacientes con más de una cita registrada al limpiar el dataset.

'color': Es un nombre de color que asignamos deliberadamente para identificar un segmento específico comprendido en Houston, Texas. Este nos permite enviar doctores a cada una de estas 'sub-áreas'.

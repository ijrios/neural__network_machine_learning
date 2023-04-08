# Red Neuronal
### _Recomendaciones para validación de modelos de Aprendizaje de Máquina_

Aprender los parámetros de una función de predicción y probarla en los mismos datos es un error metodológico: un modelo que simplemente repitiera las etiquetas de las muestras que acaba de ver tendría una puntuación perfecta pero no sería capaz de predecir nada útil en datos aún no vistos. Esta situación se llama sobreajuste (overfitting). Para evitarlo, es práctica común en el aprendizaje automático supervisado reservar una parte de los datos disponibles como conjunto de prueba (X_test, y_test)

Al evaluar diferentes configuraciones ("hiperparámetros") para un clasificador (por ejemplo, el valor de la tasa de aprendizaje para una regresión logística), que deebe establecerse manualmente, todavía existe el riesgo de sobreajuste en el conjunto de prueba porque los parámetros se pueden ajustar hasta que el estimador funcione de manera óptima. De esta manera, el conocimiento sobre el conjunto de prueba puede "filtrarse" en el modelo y las métricas de evaluación ya no informan sobre el rendimiento de generalización. Para resolver este problema, se puede retener otra parte del conjunto de datos como un llamado "conjunto de validación": el entrenamiento continúa en el conjunto de entrenamiento, después de lo cual se realiza la evaluación en el conjunto de validación, y cuando el experimento parece ser exitoso, se puede hacer la evaluación final en el conjunto de prueba.

Sin embargo, al dividir los datos disponibles en tres conjuntos, reducimos drásticamente el número de muestras que se pueden usar para aprender el modelo y los resultados pueden depender de una elección aleatoria particular para el par de conjuntos (entrenamiento, validación).

Una solución a este problema es un procedimiento llamado validación cruzada (CV por sus siglas).

![N|Solid](https://i.ibb.co/NLXWVqV/descarga.png)

Es importante tener clara la diferencia entre parametros e hiperparametros.

En el contexto del aprendizaje automático, los parámetros se refieren a los valores que el modelo aprende a partir de los datos durante el proceso de entrenamiento. Estos parámetros están asociados con la estructura del modelo y son ajustados por el algoritmo de aprendizaje a medida que se expone a los datos de entrenamiento. Por ejemplo, en una regresión lineal, los parámetros pueden ser los coeficientes de las variables predictoras.

Por otro lado, los hiperparámetros son valores que se establecen antes del entrenamiento del modelo y que controlan cómo se aprenden los parámetros del modelo. Estos hiperparámetros están fuera de la estructura del modelo en sí y deben ser establecidos por el usuario. Los hiperparámetros pueden incluir cosas como la tasa de aprendizaje, la cantidad de iteraciones de entrenamiento, etc.

La elección correcta de los hiperparámetros es crucial para el rendimiento del modelo, y se puede hacer mediante la prueba de diferentes valores y la selección de los que produzcan los mejores resultados. En general, los hiperparámetros no se aprenden durante el entrenamiento del modelo, sino que se ajustan antes del entrenamiento y se mantienen fijos durante el proceso de entrenamiento (o se prueban variaciones por medio de un esquema de validación cruzada).

Ejemplo:

> En un modelo de regreción lineal del tipo _y=w0+w1x1+...+wnxn_. Los pesos   > wk  son los parametros. Los hiperparametros son los ajustes utilizados en el > entrenamiento del modelo, por ejemplo la tasa de aprendizaje.

### Validación cruzada (cross validation)
La validación cruzada es una técnica estadística comúnmente utilizada en aprendizaje de máquina para evaluar y seleccionar modelos de manera objetiva.

En esencia, la validación cruzada implica dividir el conjunto de datos en conjuntos de entrenamiento y prueba múltiples veces para obtener una estimación más precisa del rendimiento del modelo. En lugar de entrenar y evaluar un modelo en un solo conjunto de entrenamiento y prueba, se entrena el modelo en múltiples subconjuntos de entrenamiento y se evalúa su rendimiento en múltiples subconjuntos de prueba.

### Importancia de la validación cruzada
La validación cruzada es una técnica importante en el aprendizaje automático porque nos ayuda a evaluar la capacidad de generalización de un modelo de manera objetiva y precisa. La capacidad de generalización se refiere a la capacidad del modelo de hacer predicciones precisas sobre nuevos datos que no ha visto durante el entrenamiento.

Si evaluamos el rendimiento de un modelo solo en el conjunto de datos de entrenamiento, es probable que el modelo esté sobreajustando (overfitting) el conjunto de datos de entrenamiento, es decir, el modelo está memorizando el conjunto de datos de entrenamiento en lugar de aprender patrones que se puedan generalizar a datos nuevos. Como resultado, el modelo puede tener un rendimiento deficiente en datos nuevos.

La validación cruzada aborda este problema al evaluar el rendimiento del modelo en conjuntos de datos de prueba independientes que no se han utilizado en el entrenamiento del modelo. Al evaluar el rendimiento del modelo en múltiples conjuntos de prueba, la validación cruzada proporciona una evaluación más objetiva y precisa del rendimiento del modelo en datos nuevos e invisibles.

Además, la validación cruzada ayuda a ajustar los parámetros del modelo y seleccionar el modelo final que tenga mejor rendimiento en datos nuevos. Al evaluar múltiples modelos utilizando la validación cruzada, podemos identificar qué modelo tiene mejor rendimiento y seleccionar ese modelo para su uso en la aplicación.

### K Fold Cross Validation
El tipo más común de validación cruzada es la validación cruzada k-fold, que implica dividir el conjunto de datos en k subconjuntos (llamados pliegues), donde k es un número entero positivo. El modelo se entrena k veces, cada vez utilizando un pliegue diferente como conjunto de prueba y los k-1 pliegues restantes como conjunto de entrenamiento. El rendimiento del modelo se evalúa promediando los resultados de las k iteraciones.

#### Ejemplo con 5-fold cross validation:
En este caso se divide el conjunto de datos en 5 subconjuntos y en cada iteración se utilizan 4 subconjuntos para entrenar y el otro para validar. De esta forma el proceso se repite 5 veces validando con 5 subconjuntos diferentes.

![N|Solid](https://i.ibb.co/55F0CDf/descarga-1.png)

Con este proceso se tienen 5 medidas de rendimiento. Esto permite verificar la varianza entre estos redimientos. De esta forma, si se tienen resultados con alta varianza, se puede sospechar de un problema con la generalización del modelo. De esta forma, cuando se hace validación cruzada los resultados se deben reportar con la media y la desviación estandar.

Por otro lado, la validación cruzada ayuda a abordar el problema de la sobreajuste (overfitting) y el subajuste (underfitting), y proporciona una estimación más precisa del rendimiento del modelo en datos nuevos e invisibles.

### Leave one out
Leave-one-out cross-validation (LOOCV) es una variante de la validación cruzada k-fold, donde k es igual al número de muestras en el conjunto de datos. Es decir, en lugar de dividir el conjunto de datos en k pliegues, se divide en n pliegues, donde n es el número de muestras en el conjunto de datos. Cada iteración del LOOCV deja una única muestra como conjunto de prueba y el resto de las muestras se utilizan como conjunto de entrenamiento. El modelo se entrena en los n-1 ejemplos de entrenamiento y se evalúa en la única muestra de prueba. Este proceso se repite n veces, de modo que cada muestra se utiliza una vez como conjunto de prueba.

LOOCV es una técnica de validación cruzada muy rigurosa, ya que utiliza la mayoría de las muestras para el entrenamiento y solo una muestra para la evaluación. Por lo tanto, puede proporcionar una evaluación precisa del rendimiento del modelo, pero es computacionalmente costoso, especialmente para conjuntos de datos grandes.

LOOCV se utiliza a menudo en problemas de regresión y clasificación, y puede ser especialmente útil cuando el conjunto de datos es pequeño y el rendimiento del modelo es crítico. Sin embargo, es importante tener en cuenta que LOOCV puede ser sensible a muestras atípicas (outliers) en el conjunto de datos, y puede ser susceptible al sobreajuste en problemas con un gran número de características.

![N|Solid](https://i.ibb.co/HB6x6Bw/descarga-2.png)

# Red Neuronal
Están inspiradas en la estructura del cerebro, pero no necesariamente en un modelo exacto de la misma. Todavía hay mucho que no sabemos sobre el cerebro y cómo funciona, pero ha servido de inspiración en muchas áreas científicas debido a su capacidad para desarrollar inteligencia. Y aunque existen redes neuronales que se crearon con el único propósito de comprender cómo funciona el cerebro, el aprendizaje profundo, tal como lo conocemos hoy, no pretende replicar cómo funciona el cerebro. En cambio, Deep Learning se enfoca en habilitar sistemas que aprenden múltiples niveles de composición de patrones

Y, como ocurre con cualquier progreso científico, el aprendizaje profundo no comenzó con las estructuras complejas y las aplicaciones generalizadas que se ven en la literatura reciente.

Todo comenzó con una estructura básica, que se asemeja a la neurona del cerebro .

![N|Solid](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*KAJBM4Fq-f96pibxNjusbA.png)

Este modelo de computación se denominó intencionalmente neurona , porque intentaba imitar cómo funcionaba el bloque de construcción central del cerebro. Al igual que las neuronas del cerebro reciben señales eléctricas, la neurona de McCulloch y Pitts recibió entradas y, si estas señales eran lo suficientemente fuertes, las transmitió a otras neuronas.

La primera aplicación de la neurona replicó una puerta lógica , donde tiene una o dos entradas binarias y una función booleana que solo se activa con las entradas y los pesos correctos

![N|Solid](https://miro.medium.com/v2/resize:fit:828/format:webp/1*H50XJ8vJvDoRi35Qq37BbA.png)

Sin embargo, este modelo tenía un problema. No podía aprender como el cerebro. La única forma de obtener el resultado deseado era si los pesos, que funcionan como catalizadores en el modelo, se establecieron de antemano.

> El sistema nervioso es una red de neuronas, cada una de las cuales tiene un > soma y un axón […] En cualquier instante una neurona tiene un umbral, que la > excitación debe superar para iniciar un impulso.

Solo una década después , Frank Rosenblatt amplió este modelo y creó un algoritmo que podía aprender los pesos para generar una salida.

Basándose en la neurona de McCulloch y Pitt, Rosenblatt desarrolló el Perceptrón .

## Hiperparametros de las redes neuronales
En la red neuronal tenemos una mayor cantidad de hiperparametros, ya que las diferentes combinaciones del numero de nueronas y número de capas son conciderados hiperparametros.

Los hiperparametros de las redes neuronales son:

*   El número de capas y de neuronas por capas.
*   La función de activación.
*   El algoritmo de optimización (solver)
*   El termino de regularización (alpha)
*   La tasa de aprendizaje
*   El batch_size
*   El número maximo de iteraciones

# Perceptrón
Aunque hoy en día el Perceptron es ampliamente reconocido como un algoritmo, inicialmente se pensó como una máquina de reconocimiento de imágenes. Recibe su nombre de realizar la función humana de percepción , ver y reconocer imágenes.

> En particular, el interés se ha centrado en la idea de una máquina que sería > capaz de conceptualizar entradas que inciden directamente desde el entorno  > físico de luz, sonido, temperatura, etc., el "mundo fenoménico" con el que 
> todos estamos familiarizados, en lugar de requiriendo la intervención de un > agente humano para digerir y codificar la información necesaria.

La máquina perceptrón de Rosenblatt se basaba en una unidad básica de computación, la neurona . Al igual que en los modelos anteriores, cada neurona tiene una celda que recibe una serie de pares de entradas y pesos.

La principal diferencia en el modelo de Rosenblatt es que las entradas se combinan en una suma ponderada y, si la suma ponderada supera un umbral predefinido, la neurona se dispara y produce una salida.

![N|Solid](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*FBJqWE6AHt8HMZ7XHW9uWg.png)

El umbral T representa la función de activación . Si la suma ponderada de las entradas es mayor que cero, la neurona emite el valor 1; de lo contrario, el valor de salida es cero.

# Perceptrón multicapa
El Perceptrón multicapa fue desarrollado para hacer frente a esta limitación. Es una red neuronal donde el mapeo entre entradas y salidas no es lineal.

Un perceptrón multicapa tiene capas de entrada y salida, y una o más capas ocultas con muchas neuronas apiladas juntas. Y mientras que en el Perceptrón la neurona debe tener una función de activación que imponga un umbral, como ReLU o sigmoides, las neuronas en un Perceptrón multicapa pueden usar cualquier función de activación arbitraria.

![N|Solid](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*MF1q2Q3fbpYlXX8fZUiwpA.png)

El Perceptrón multicapa entra en la categoría de algoritmos feedforward , porque las entradas se combinan con los pesos iniciales en una suma ponderada y se someten a la función de activación, al igual que en el Perceptrón. Pero la diferencia es que cada combinación lineal se propaga a la siguiente capa.

Cada capa alimenta a la siguiente con el resultado de su cálculo, su representación interna de los datos. Esto recorre todo el camino a través de las capas ocultas hasta la capa de salida.

Pero tiene más.

Si el algoritmo solo calculara las sumas ponderadas en cada neurona, propagara los resultados a la capa de salida y se detuviera allí, no podría aprender los pesos que minimizan la función de costo. Si el algoritmo solo calculara una iteración, no habría aprendizaje real.





  
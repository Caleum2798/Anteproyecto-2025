---
title: "Riesgo Crediticio mediante una Arquitectura Híbrida de Redes Neuronales de Grafos y Aprendizaje por Refuerzo Profundo"
subtitle: "Anteproyecto de trabajo de grado"
author:
  - "[Juan Sebastian Gonzalez Marroquin]"
  - "[Nombre del Director]"
institute: "Universidad Santo Tomás - Facultad de Estadística"
degree: "Maestría en Estadística Aplicada"
date: "Diciembre 2025"
output: pdf_document
header-includes:
  - \usepackage{setspace}
  - \onehalfspacing
---

# Credit Risk Assessment using a Hybrid Architecture of Graph Neural Networks and Deep Reinforcement Learning

# Resumen

Este trabajo presenta una metodología híbrida para la evaluación del riesgo crediticio utilizando el conjunto de datos de *Home Credit Default Risk*. El objetivo principal es mejorar la precisión en la detección de incumplimiento mediante la integración de Redes Neuronales de Grafos (GNN), que capturan las relaciones complejas y la topología de los datos de los clientes, con algoritmos de Aprendizaje por Refuerzo Profundo (DRL) para la optimización de la toma de decisiones. Se propone un diseño experimental que compara este enfoque frente a modelos tradicionales de *scoring* y técnicas de *machine learning* estándar. Los resultados esperados apuntan a una mayor robustez en la clasificación de perfiles de riesgo y una mejora significativa en métricas de desempeño como el área bajo la curva (AUC), demostrando la eficacia de combinar información relacional con estrategias de aprendizaje secuencial.

**Palabras clave:** Riesgo crediticio, Redes Neuronales de Grafos, Aprendizaje por Refuerzo, Home Credit, Aprendizaje Profundo.

\newpage

# Abstract

This paper presents a hybrid methodology for credit risk assessment using the *Home Credit Default Risk* dataset. The main objective is to improve default detection accuracy by integrating Graph Neural Networks (GNN), which capture complex relationships and data topology of clients, with Deep Reinforcement Learning (DRL) algorithms for decision-making optimization. An experimental design comparing this approach against traditional scoring models and standard machine learning techniques is proposed. Expected results point to greater robustness in risk profile classification and a significant improvement in performance metrics such as the Area Under the Curve (AUC), demonstrating the efficacy of combining relational information with sequential learning strategies.

**Keywords:** Credit risk, Graph Neural Networks, Reinforcement Learning, Home Credit, Deep Learning.

\newpage





# 1. Introducción

La evaluación del riesgo crediticio constituye la piedra angular de la estabilidad financiera global y es el determinante principal para el funcionamiento y la rentabilidad de las entidades financieras[cite: 2]. Tradicionalmente, las instituciones financieras han utilizado modelos estadísticos clásicos, como la regresión logística, y más recientemente algoritmos de aprendizaje automático supervisado como *Random Forest* o *Gradient Boosting*.

Si bien estas herramientas han demostrado eficacia en el manejo de datos tabulares estructurados, operan bajo una suposición fundamental que a menudo no refleja la realidad: la independencia entre las observaciones.En la práctica, los solicitantes de crédito no son entidades aisladas; interactúan en un entorno social y económico complejo, compartiendo empleadores, direcciones, dispositivos de contacto o historiales de comportamiento similares

El planteamiento del problema de esta investigación surge de la limitación de los modelos actuales para capturar esta "topología del riesgo"Al ignorar las relaciones implícitas entre los clientes, los modelos tradicionales pierden información valiosa que podría predecir el incumplimiento, especialmente en clientes con expedientes crediticios delgados (*thin-files*)La incapacidad de modelar estas estructuras relacionales complejas de comportamiento limita la precisión de las predicciones.

En este contexto, el auge del Aprendizaje Profundo (*Deep Learning*) ofrece nuevas perspectivas.Específicamente, las Redes Neuronales de Grafos (GNN) han emergido como una solución potente para procesar datos, permitiendo aprender representaciones de nodos (clientes) basándose no solo en sus características intrínsecas, sino también en las de sus vecinos en el grafo. 

Simultáneamente, el Aprendizaje por Refuerzo (RL) presenta un paradigma donde un agente aprende a tomar decisiones óptimas, aprobar o rechazar un crédito mediante la interacción con un entorno y la maximización de una recompensa acumulada.

El presente proyecto busca desarrollar y evaluar una arquitectura híbrida que integre GNN para la extracción de características relacionales y algoritmos de Aprendizaje por Refuerzo Profundo (GNN-RL) para la clasificación del riesgo. 

Utilizando el conjunto de datos público de *Home Credit Default Risk*, que destaca por su riqueza en tablas relacionales y datos complementarios, se busca demostrar que la integración de la estructura de red de los solicitantes mejora significativamente la capacidad predictiva frente a los enfoques convencionales.

Los alcances de este estudio se centran en la implementación técnica del modelo híbrido y su validación estadística. Se espera que los resultados no solo contribuyan al ámbito académico demostrando la viabilidad de estas técnicas avanzadas en finanzas, sino que también ofrezcan a la industria una metodología para reducir las tasas de incumplimiento sin sacrificar la inclusión financiera de segmentos poblacionales tradicionalmente difíciles de evaluar.

## Pregunta de Investigación

¿En qué medida la implementación de una arquitectura híbrida, que integre Redes Neuronales de Grafos (GNN) para la extracción de características topológicas y comportamentales aplicadas con un modelo de Aprendizaje por Refuerzo Profundo (RL) para la optimización de decisiones, mejora la capacidad predictiva y la detección de incumplimiento en comparación con los modelos de *scoring* y aprendizaje supervisado tradicionales?.

# 2. Marco teórico y revisión de literatura

La fundamentación teórica de este proyecto entrelaza tres dominios del conocimiento: la modelación del riesgo financiero, el aprendizaje geométrico profundo y la toma de decisiones secuencial.

## 2.1. Modelos de Riesgo de Crédito
Históricamente, la evaluación de riesgo se ha sustentado en modelos de *Credit Scoring*. Estos modelos buscan estimar la probabilidad de incumplimiento ($PD$) de un cliente. Los antecedentes muestran una evolución desde sistemas expertos hasta modelos estadísticos como la Regresión Logística y el Análisis Discriminante. Más recientemente, algoritmos de conjunto (*Ensemble methods*) como XGBoost han dominado el estado del arte en datos tabulares, aunque suelen ignorar la interdependencia entre las observaciones.

## 2.2. Redes Neuronales de Grafos (GNN)
Las GNN extienden el aprendizaje profundo a datos no euclidianos. A diferencia de las redes convolucionales tradicionales (CNN) que operan en grillas fijas (imágenes), las GNN operan en grafos irregulares $G=(V,E)$.
* **Paso de Mensajes (*Message Passing*):** Es el mecanismo central donde cada nodo agrega información de sus vecinos para actualizar su propia representación (*embedding*). Esto permite capturar la homofilia (tendencia de nodos similares a conectarse) y patrones estructurales de fraude o riesgo compartido.

## 2.3. Aprendizaje por Refuerzo Profundo (DRL)
El DRL combina redes neuronales con el marco de aprendizaje por refuerzo. Se formaliza mediante Procesos de Decisión de Markov (MDP).
* **Agente y Entorno:** En el contexto de crédito, el "agente" es el sistema de aprobación y el "entorno" es el flujo de solicitudes.
* **Función Q:** Algoritmos como DQN (*Deep Q-Network*) aproximan el valor de tomar una acción específica en un estado dado, permitiendo optimizar métricas de negocio complejas más allá de la simple precisión de clasificación.

# 3. Objetivos

## 3.1. Objetivo General
Evaluar el desempeño de una arquitectura híbrida compuesta por Redes Neuronales de Grafos (GNN) y Aprendizaje por Refuerzo Profundo (RL) en la detección de riesgo de incumplimiento crediticio, utilizando el conjunto de datos de *Home Credit* para determinar su eficacia frente a modelos de *scoring* tradicionales (Baseline).

## 3.2. Objetivos Específicos
* **Construir** una representación basada en grafos a partir de los datos tabulares y relacionales de *Home Credit*, definiendo nodos y aristas que capturen las interacciones clave entre los solicitantes y sus atributos históricos.
* **Diseñar** una arquitectura de Red Neuronal de Grafos (GNN) capaz de generar *embeddings* vectoriales que sinteticen la información comportamental y de características de cada cliente.
* **Implementar** un agente de Aprendizaje por Refuerzo que utilice las representaciones generadas por la GNN para optimizar la política de decisión de aprobación o rechazo de crédito.
* **Comparar** las métricas de desempeño (AUC, Precisión, *Recall*) del modelo híbrido propuesto frente a modelos base tradicionales (*Logistic Regression*, *XGBoost*) para validar la mejora en la capacidad predictiva.

# 4. Metodología

La presente investigación adopta un enfoque cuantitativo y experimental. Para cumplir con el criterio de replicabilidad y objetividad, a continuación se detalla el procedimiento estadístico y computacional estructurado en cuatro fases secuenciales.

## 4.1. Fase 1: Preprocesamiento y Definición del Espacio de Datos
Se utilizará el conjunto de datos público *Home Credit Default Risk*. El procedimiento de limpieza incluirá:
1.  **Tratamiento de valores faltantes:** Imputación mediante *Iterative Imputer* (MICE) para variables numéricas.
2.  **Codificación:** Transformación de variables categóricas mediante *Target Encoding* para alta cardinalidad.
3.  **Balanceo de clases:** Aplicación de técnicas de submuestreo (*undersampling*) en la clase mayoritaria durante el entrenamiento.

## 4.2. Fase 2: Construcción del Grafo (Topología)
Los datos tabulares se transformarán en una estructura de grafo $G = (V, E)$, donde:
* **Nodos ($V$):** Representan a los solicitantes de crédito.
* **Aristas ($E$):** Representan conexiones relacionales latentes. Se definirán aristas si dos nodos comparten atributos críticos verificables (ej. mismo empleador, dirección, o co-aplicaciones) para evitar la saturación de la red.

## 4.3. Fase 3: Arquitectura Híbrida

### 4.3.1. Extracción de Características con GNN
Se implementará una arquitectura tipo **GraphSAGE** para generar *embeddings* inductivos. La actualización del vector de características para un nodo $v$ en la capa $k$ se define como:

$$h_v^{(k)} = \sigma \left( W^{(k)} \cdot \text{CONCAT} \left( h_v^{(k-1)}, \text{AGG} \left( \{h_u^{(k-1)}, \forall u \in \mathcal{N}(v)\} \right) \right) \right)$$

Donde $h_v^{(k)}$ es la representación del nodo, $\mathcal{N}(v)$ son los vecinos y $W^{(k)}$ son los pesos entrenables.

### 4.3.2. Optimización con Deep Reinforcement Learning (DRL)
Los *embeddings* de la GNN servirán como "Estado" ($S$) para un agente **Deep Q-Network (DQN)**:
* **Acción ($A_t$):** $\{0: \text{Rechazar}, 1: \text{Aprobar}\}$.
* **Recompensa ($R_t$):** Función de costo asimétrica basada en la matriz de confusión financiera:
    $$R = \begin{cases} +1 & \text{si } A=1 \land Y=0 \text{ (Aprobado y Pagado)} \\ -10 & \text{si } A=1 \land Y=1 \text{ (Aprobado y Default)} \\ 0 & \text{si } A=0 \text{ (Rechazado)} \end{cases}$$

**Ajuste de Parámetros:** Los hiperparámetros (tasa de aprendizaje, factor de descuento $\gamma$) serán ajustados mediante búsqueda de rejilla (*Grid Search*) en el conjunto de validación.

## 4.4. Fase 4: Validación
El modelo se evaluará utilizando validación cruzada ($k=5$). Las métricas principales serán **ROC-AUC** y **Ganancia Esperada** (suma de recompensas en test).

# 5. Cronograma y actividades

| Actividad | Mes 1 | Mes 2 | Mes 3 | Mes 4 |
| :--- | :---: | :---: | :---: | :---: |
| Preprocesamiento y Grafo | X | | | |
| Diseño e Implementación GNN | | X | | |
| Entrenamiento Agente RL | | | X | |
| Validación y Escritura Final | | | | X |

# 6. Bibliografía

* Breiman, L. (2001). Random forests. Machine Learning, 45(1), 5–32. https://doi.org/10.1023/A:1010933404324

* Chen, T., & Guestrin, C. (2016). XGBoost: A scalable tree boosting system. En Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining (pp. 785–794). ACM. https://doi.org/10.1145/2939672.2939785

* Goodfellow, I., Bengio, Y., & Courville, A. (2016). Deep learning. MIT Press.

* Hamilton, W. L., Ying, R., & Leskovec, J. (2017). Inductive representation learning on large graphs. En Proceedings of the 31st International Conference on Neural Information Processing Systems (NIPS'17) (pp. 1025–1035). Curran Associates Inc.

* Home Credit Group. (2018). Home Credit Default Risk: Can you predict how capable each applicant is of repaying a loan? [Conjunto de datos]. Kaggle. https://www.kaggle.com/c/home-credit-default-risk

* Mnih, V., Kavukcuoglu, K., Silver, D., Rusu, A. A., Veness, J., Bellemare, M. G., Graves, A., Riedmiller, M., Fidjeland, A. K., Ostrovski, G., Petersen, S., Beattie, C., Sadik, A., Antonoglou, I., King, H., Kumaran, D., Wierstra, D., Legg, S., & Hassabis, D. (2015). Human-level control through deep reinforcement learning. Nature, 518(7540), 529–533. https://doi.org/10.1038/nature14236

* Scarselli, F., Gori, M., Tsoi, A. C., Hagenbuchner, M., & Monfardini, G. (2009). The graph neural network model. IEEE Transactions on Neural Networks, 20(1), 61–80. https://doi.org/10.1109/TNN.2008.2005605

* Sutton, R. S., & Barto, A. G. (2018). Reinforcement learning: An introduction (2ª ed.). MIT Press.

* Wu, Z., Pan, S., Chen, F., Long, G., Zhang, C., & Yu, P. S. (2021). A comprehensive survey on graph neural networks. IEEE Transactions on Neural Networks and Learning Systems, 32(1), 4–24. https://doi.org/10.1109/TNNLS.2020.2978386

* Zhang, M., & Chen, Y. (2018). Link prediction based on graph neural networks. En Proceedings of the 32nd International Conference on Neural Information Processing Systems (NIPS'18) (pp. 5171–5181). Curran Associates Inc.

<div id="refs"></div>
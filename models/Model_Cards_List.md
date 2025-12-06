# 📊 Métricas y Parámetros de Evaluación: Modelo GNN-RL

Este documento define los indicadores clave de desempeño (KPIs) y los parámetros de salida esperados para el modelo híbrido de Graph Neural Networks y Reinforcement Learning aplicado al riesgo de crédito (Home Credit).

---

## 1. Métricas de Negocio (Enfoque RL)
*Estas métricas evalúan la capacidad del "Agente" para maximizar la rentabilidad del portafolio.*

* **💰 Recompensa Acumulada (Cumulative Reward):**
    * **Definición:** Suma total de recompensas obtenidas por el agente durante un episodio (conjunto de clientes).
    * **Objetivo:** La curva debe ser ascendente y converger en un valor estable superior al de una política aleatoria.
    * **Uso:** Indicador principal de que el agente está "aprendiendo" la política óptima.

* **📈 Ganancia Neta Estimada (Net Profit):**
    * **Definición:** `(Total Intereses Ganados) - (Total Capital Perdido por Default)`.
    * **Objetivo:** Superar la ganancia neta generada por un modelo base (ej. XGBoost o Regresión Logística).
    * **Fórmula:** $\sum_{i \in Aprobados} [ (1 - y_i) \cdot \text{Interés} - y_i \cdot \text{MontoPrestamo} ]$

* **✅ Tasa de Aprobación (Approval Rate):**
    * **Definición:** Porcentaje de solicitudes que el agente decide aprobar (`Action = 1`).
    * **Objetivo:** Mantener una tasa saludable (ej. 70-80%) sin disparar el riesgo. Si aprueba 0%, el riesgo es nulo pero el negocio quiebra.

* **⚠️ Tasa de Default en Cartera (Bad Rate):**
    * **Definición:** De los créditos aprobados, porcentaje que resultó en impago (`TARGET = 1`).
    * **Objetivo:** Debe ser menor al promedio del dataset original.

---

## 2. Métricas Estadísticas (Validación de Riesgo)
*Estas métricas son estándar en la industria bancaria para validar la capacidad discriminatoria del modelo.*

* **🎯 AUC-ROC (Area Under the Curve):**
    * **Definición:** Capacidad del modelo para ordenar correctamente a los "buenos" y "malos" pagadores.
    * **Valor Esperado:** `> 0.74` (Competitivo en Home Credit).
    * **Valor Excelente:** `> 0.78`.

* **📊 Coeficiente de Gini:**
    * **Definición:** Medida de desigualdad usada en crédito.
    * **Fórmula:** $Gini = 2 \times AUC - 1$.
    * **Valor Esperado:** `> 0.50` (Aprox).

* **📉 Estadístico KS (Kolmogorov-Smirnov):**
    * **Definición:** Máxima separación entre la distribución acumulada de buenos y malos.
    * **Objetivo:** Maximizar la separación en los primeros deciles.

---

## 3. Parámetros de Salida (Outputs del Sistema)
*Estos son los datos que el modelo entregará por cada cliente evaluado.*

| Parámetro | Tipo de Dato | Descripción | Uso Práctico |
| :--- | :--- | :--- | :--- |
| **Acción Óptima ($a_t$)** | Binario `{0, 1}` | Decisión final del agente. | `0`: Rechazar / `1`: Aprobar. |
| **Score de Riesgo** | Flotante `[0, 1]` | Probabilidad de incumplimiento estimada (o Q-value). | Asignación de tasa de interés basada en riesgo. |
| **Embedding ($s_t$)** | Vector `Array[64]` | Representación latente del cliente generada por la GNN. | Input para clusterización o análisis de similitud. |
| **Recompensa ($r_t$)** | Flotante | Valor del feedback recibido del entorno. | Solo disponible en entrenamiento/validación. |

---

## 4. Criterios de Éxito del Proyecto
El modelo se considerará exitoso si cumple simultáneamente:
1.  **AUC-ROC** superior a 0.74 en el set de prueba.
2.  **Ganancia Neta** superior a la estrategia "Aprobar a todos" y superior a un modelo aleatorio.
3.  **Convergencia** de la función de pérdida (Loss) del Actor-Critic durante el entrenamiento.
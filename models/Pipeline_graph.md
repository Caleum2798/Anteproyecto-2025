# 🔄 Pipeline: GNN + Reinforcement Learning

```text
📂 FASE 1: DATOS (HOME CREDIT)
+-------------------------------------------------------+
|  [application_{train|test}.csv]  [bureau.csv]   ...   |
|               👇                                      |
|  PREPROCESAMIENTO: Limpieza y Joins                   |
+-------------------------------------------------------+
            |
            v
🏗️ FASE 2: CONSTRUCCIÓN DEL GRAFO
+-------------------------------------------------------+
|  NODOS:   Clientes (SK_ID_CURR)                       |
|  ARISTAS: Relaciones (Familia, Historial, Avales)     |
|  OUTPUT:  Grafo Heterogéneo (PyG Data Object)         |
+-------------------------------------------------------+
            |
            v
🧠 FASE 3: MODELO GNN (Percepción)
+-------------------------------------------------------+
|  ENTRADA: Estructura del Grafo                        |
|  CAPAS:   GCN / GAT / GraphSAGE                       |
|  SALIDA:  Vector de Estado (Embedding $s_t$)          |
+-------------------------------------------------------+
            |
            v
🤖 FASE 4: AGENTE RL (Decisión)
+-------------------------------------------------------+
|  ALGORITMO: PPO / DQN                                 |
|  ACCIÓN ($a_t$):                                      |
|     [ 0: DENEGAR ]  o  [ 1: APROBAR ]                 |
+-------------------------------------------------------+
            |
            v
🌍 FASE 5: ENTORNO & RECOMPENSA
+-------------------------------------------------------+
|  EVALUACIÓN (Ground Truth):                           |
|  - Si Acción=1 y TARGET=0 (Pagó) -> 💰 GANANCIA (+)   |
|  - Si Acción=1 y TARGET=1 (Fallo)-> 📉 PÉRDIDA (-)    |
|  - Si Acción=0                   -> 😐 NEUTRO (0)     |
+-------------------------------------------------------+
            |
            +-----< RETROALIMENTACIÓN (Backprop) <------+
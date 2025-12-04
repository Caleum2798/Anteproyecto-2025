# Modelo para la Optimización de Riesgo Crediticio con GNN y Aprendizaje por Refuerzo (GNN-RL)

**Juan Sebastias Gonzalez Marroquin**
* Maestría en Estadística Aplicada
* Universidad Santo Tomás (Sede Bogotá)
* Contacto: [ juan.marroquingon@usantotomas.edu.co ] 

**Proyecto de Grado - Maestría en Estadística Aplicada**
> Una aproximación novedosa para la evaluación de riesgo financiero utilizando la estructura relacional de los datos (Grafos) y la toma de decisiones secuencial (Reinforcement Learning).
---

## 📋 Tabla de Contenidos
1. [Descripción del Proyecto](#-descripción-del-proyecto)
2. [El Problema de Negocio](#-el-problema-de-negocio)
3. [Metodología Propuesta](#-metodología-propuesta)
4. [Estructura del Repositorio](#-estructura-del-repositorio)
5. [Dataset](#-dataset)
6. [Instalación y Uso](#-instalación-y-uso)
7. [Resultados Esperados](#-resultados-esperados)
8. [Autor](#-autor)

---

## 📖 Descripción del Proyecto

Este proyecto busca superar las limitaciones de los modelos tradicionales de scoring de crédito (como Regresión Logística o XGBoost) que tratan a los clientes como observaciones independientes (filas en una tabla).

Nuestra propuesta integra dos técnicas avanzadas de Deep Learning:
1.  **Graph Neural Networks (GNN):** Para modelar explícitamente las relaciones complejas entre solicitantes, historiales de crédito y burós como un grafo.
2.  **Reinforcement Learning (RL):** Para simular un agente de decisión que optimiza una política de concesión de créditos a largo plazo, maximizando el retorno financiero en lugar de solo minimizar el error de clasificación.

## 💼 El Problema de Negocio

Las instituciones financieras enfrentan el desafío de clasificar clientes con historial crediticio insuficiente o complejo. El objetivo es:
* Reducir la tasa de incumplimiento (*Default Rate*).
* Maximizar la rentabilidad de la cartera.
* Utilizar información relacional no explotada en modelos tabulares clásicos.

## 🔬 Metodología Propuesta

El flujo de trabajo estadístico y computacional se divide en dos fases:

### Fase 1: Representación con GNN
Transformación de las tablas relacionales en una estructura de grafo heterogéneo.
* **Nodos:** Clientes (`SK_ID_CURR`), Préstamos Previos, Buró.
* **Aristas:** Relaciones históricas y sociales.
* **Modelo:** GCN (Graph Convolutional Network) o GAT (Graph Attention Network) para generar *embeddings* de los clientes.

### Fase 2: Decisión con RL
Un agente aprende a tomar la decisión de aprobar/rechazar basándose en el estado del cliente.
* **Estado ($S_t$):** Embedding del cliente generado por la GNN.
* **Acción ($A_t$):** Aprobar / Rechazar / Ajustar Tasa.
* **Recompensa ($R_t$):** Beneficio neto (Intereses - Pérdida por Default).
* **Algoritmo:** PPO (Proximal Policy Optimization) o DQN.

## 📂 Estructura del Repositorio (Git-Hub)

```text
├── data/
│   ├── raw/                 # Datos originales de Home Credit (Kaggle)
│   ├── processed/           # Datos procesados y grafos construidos
│   └── external/            # Diccionarios de datos
├── notebooks/
│   ├── 01_EDA_Tabular.ipynb # Análisis Exploratorio de Datos tradicional
│   ├── 02_Graph_Construction.ipynb # Construcción de nodos y aristas
│   ├── 03_GNN_Training.ipynb       # Entrenamiento de la red de grafos
│   └── 04_RL_Agent.ipynb           # Entrenamiento del agente de RL
├── src/
│   ├── data/                # Scripts de carga y limpieza
│   ├── features/            # Ingeniería de características
│   ├── models/              # Arquitecturas GNN y RL (PyTorch)
│   └── visualization/       # Scripts para gráficas de resultados
├── models/                  # Modelos entrenados (.pth, .pkl)
├── reports/                 # Tesis en PDF y borradores
├── requirements.txt         # Dependencias del proyecto
└── README.md                # Este archivo
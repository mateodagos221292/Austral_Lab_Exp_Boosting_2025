# Austral_Lab_Exp_Boosting_2025
Repo del Experimento de metodos de Boosting

# Métodos de Boosting en LightGBM: GBDT vs Random Forest vs DART

Trabajo de laboratorio de la materia **Hyperparameter Tuning** (Problema #14), donde comparamos diferentes métodos de boosting implementados en **LightGBM**:  
- Gradient Boosting Decision Trees (**GBDT**)  
- **Random Forest** (RF)  
- **DART** (Dropouts meet Multiple Additive Regression Trees)  

---

## 🎯 Objetivo e hipótesis

El objetivo del experimento es **comparar el desempeño** de los tres métodos de boosting de LightGBM en términos de **ganancia económica** obtenida en el mes **2021-09**, utilizando los resultados publicados en el leaderboard público de la competencia. 

**Hipótesis inicial:**

- El método que generará **mayor ganancia** será **GBDT**, considerando la bibliografía y el hecho de ser el método por defecto en LightGBM.   

---

## 🔬 Diseño experimental

El diseño experimental se organizó en dos grupos de trabajo (Grupo A y Grupo B) y siguió estos lineamientos principales:

1. Partir del **notebook de laboratorio senior** provisto por la cátedra.
2. Crear **tres notebooks** independientes, uno por cada método de boosting:
   - `GBDT`
   - `RandomForest`
   - `DART`  
   Manteniendo constantes el resto de los hiperparámetros para asegurar comparabilidad.
3. Ejecutar los modelos en **máquinas virtuales** en paralelo, variando únicamente la **semilla aleatoria**.
4. Dividir el experimento en dos grupos:
   - **Grupo A:** corridas sobre un período temporal y mes futuro específicos, con **5 semillas**.
   - **Grupo B:** mismo enfoque con **10 semillas**, para reforzar la robustez estadística.
5. Obtener la **ganancia** de cada corrida desde:
   - Leaderboard de Kaggle (Grupo A).
   - Archivos generados en el bucket de resultados (Grupo B).
6. Calcular la **ganancia promedio por método** y aplicar **test de Wilcoxon** para comparar pares de métodos.
7. Comparar resultados entre **Grupo A y B** para evaluar la consistencia.
8. Extraer conclusiones y recomendaciones para el workflow por defecto.

---

## ⚠️ Limitaciones

Algunas limitaciones importantes del experimento:

- **Tiempo y recursos de cómputo** limitados, especialmente críticos para DART.
- **DART**:
  - Tiempos de corrida **muy elevados** (en modalidad conceptual llegó a ~36 horas).
  - Se usaron los **hiperparámetros por defecto** del baseline para los parámetros exclusivos de DART.
  - No fue posible explorar muchas combinaciones alternativas con optimización bayesiana.
- Problemas operativos:
  - Máquinas virtuales *spot* que se apagaban.
  - Notebooks que se tildaban durante la optimización.
- Diferencias de capacidad de cómputo entre integrantes (no todas las VMs eran equivalentes).

---

## 📊 Resultados principales

A partir de las ganancias obtenidas para cada semilla y método, se construyeron tablas y gráficos comparativos (por grupo y por método) y se aplicó el **test de Wilcoxon** para contrastar los métodos.

**Hallazgos clave:**

- **DART** y **GBDT** presentan **ganancias similares**, sin diferencias estadísticamente significativas según el test de Wilcoxon.
- **Random Forest** muestra una **ganancia inferior**, y con más semillas (Grupo B) la diferencia frente a GBDT y DART se vuelve claramente significativa (p-valor ≪ 0.05).
- **Tiempo de cómputo**:
  - DART demora aproximadamente **5 veces más** que GBDT.
  - GBDT logra ganancias equivalentes con un tiempo de ejecución muy inferior.
- Los resultados de **Grupo A (5 semillas)** y **Grupo B (10 semillas)** son **coherentes y consistentes**, reforzando las conclusiones. 

---

## ✅ Conclusiones

- Dentro de los métodos de boosting evaluados de LightGBM, **GBDT y DART** son los que obtienen las **mayores ganancias**.  
- Sin embargo, dado que **DART es mucho más costoso en tiempo de ejecución** y la diferencia de ganancia respecto a GBDT **no es estadísticamente significativa**, la opción **más conveniente para producción es GBDT**. 
- En entornos reales, donde el tiempo de cómputo se traduce en **costos de procesamiento**, GBDT se presenta como la mejor relación **costo–beneficio**.
- Estas conclusiones aplican a:
  - Datasets con **características similares** a los utilizados.
  - Configuraciones de **hiperparámetros optimizados** según el workflow de este experimento.

---

## 💡 Recomendaciones para el workflow

- Mantener **GBDT** como **método de boosting por defecto** en el workflow de LightGBM.  
- **No modificar** el código por defecto del workflow salvo que haya una justificación muy clara y recursos extra para experimentar. 
- Utilizar **Random Forest** solo como modelo rápido de referencia inicial, no como modelo final de producción.
- Considerar **DART** únicamente cuando:
  - Se disponga de **tiempo y cómputo suficientes**.
  - El problema requiera explorar configuraciones avanzadas para maximizar ganancia sin restricciones de tiempo.

---

## 🔭 Trabajo futuro

- Explorar **nuevas combinaciones de hiperparámetros** exclusivos de DART (no solo los valores por defecto).
- Probar el experimento en:
  - Otros períodos temporales.
  - Otros datasets con distinta estructura de features.
- Aumentar el número de **semillas** para robustecer aún más el análisis estadístico.

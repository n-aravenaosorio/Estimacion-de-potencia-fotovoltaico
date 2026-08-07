<!-- 
INSTRUCCIÓN OBLIGATORIA PARA CUALQUIER IA / ASISTENTE:
ESTE ARCHIVO REGISTRA EXCLUSIVAMENTE CONTENIDO ACADÉMICO, CONCEPTOS DE MATERIA Y OBSERVACIONES DE APRENDIZAJE.

REGLAS DE REGISTRO:
1. NO REGISTRAR preguntas logísticas o administrativas (ej. impresión de PDFs, formatos de archivos, comparaciones de versiones impresas).
2. REGISTRAR ÚNICAMENTE preguntas y explicaciones sobre la MATERIA de la tesis: física, ecuaciones, código, modelos, métricas y fundamentación teórica.
3. REGISTRAR OBSERVACIONES SOBRE EL APRENDIZAJE: Si descubres un enfoque o estilo de explicación que le funcione mejor a Nicolás para entender un concepto, anótalo. Si su preferencia cambia, actualiza las notas anteriores. El objetivo es MAXIMIZAR SU APRENDIZAJE.
4. Mantener este archivo actualizado en tiempo real durante cada sesión.
-->

# Historial de Contenido Académico y Estrategia de Estudio

**Estudiante:** Nicolás Antonio Aravena Osorio  
**Tesis:** *Evaluación de un Modelo Híbrido Físico–Informado para la Estimación de Potencia Fotovoltaica bajo Distintos Regímenes de Fracción Difusa*  
**Objetivo:** Preparación intensiva de MATERIA y CÓDIGO para la defensa de tesis.

---

## 🎯 Perfil y Estilo de Aprendizaje de Nicolás (Dinámico)
* **Metodología de estudio principal:** Lectura cruzada del informe (sección a sección) en paralelo con el código (`Codigos_Tesis_Completos.pdf`) línea por línea para entender la correspondencia exacta entre teoría y programación.
* **Preferencia de explicación:** Directa, clara y al grano. Primero el por qué (motivo físico/teórico), luego el cómo (código/sintaxis).
* **Observaciones de aprendizaje:** *(Se irá actualizando dinámicamente a medida que abordemos los contenidos de materia).*

---

## 📚 Bitácora de Conceptos y Dudas de Materia

*(En esta sección se registrarán únicamente las explicaciones conceptuales, dudas sobre el código, fórmulas y defensas teóricas abordadas durante las sesiones).*

### Resumen de Contexto Técnico de la Tesis
- **Modelo Físico Base:** Modelo NOCT (Nominal Operating Cell Temperature), estima temperatura de celda y potencia DC a partir de $POA$ y $T_a$, sin considerar enfriamiento por viento ni radiación difusa directa.
- **Variable de Diagnóstico:** Fracción difusa ($fd = DHI / GHI$), clasificada en 3 regímenes: Dominio Directo ($fd < 0.3$), Transición ($0.3 \le fd \le 0.7$), y Dominio Difuso ($fd > 0.7$).
- **Modelo Híbrido:** XGBoost enfocado en *residual learning* ($\epsilon = P_{ref} - P_{NOCT}$).
- **Resultados Clave:** El modelo sin $fd$ reduce el MAE de test de 13.30 W a 2.70 W. Al agregar $fd$, el MAE baja a 2.22 W (reducción adicional del 17.69%).
- **Causalidad vs. Correlación:** La mejora con $fd$ indica aporte predictivo respecto al modelo NOCT en PVWatts, pero no implica causalidad física pura. El viento (2da variable más importante) evidencia que parte del residuo se debe al balance térmico no considerado por NOCT.
- **Importancia de Variables (Gain vs. SHAP):** Para la tesis se usó la **Ganancia Normalizada (Gain)** propia de XGBoost, que mide cuánto disminuye el error del modelo cada vez que un árbol se divide usando una variable. *Ojo para la defensa:* Esto es matemáticamente diferente a **SHAP** (teoría de juegos), el cual es un algoritmo externo más pesado que calcula la contribución marginal exacta. Es crucial tener clara esta distinción teórica frente a preguntas de la comisión.

### Dudas y Explicaciones (Sesiones Actuales)

**Duda:** ¿A qué se refiere la cita "El modelo técnico de PVWatts ha sido documentado por Dobos"?
**Explicación Teórica:**
Se refiere a **Aron P. Dobos**, investigador del NREL (National Renewable Energy Laboratory), quien es el autor del documento técnico oficial (*PVWatts Version 5 Manual*, 2014) que explica exactamente qué matemáticas hay "por debajo" de PVWatts.
El "modelo técnico" involucra la cadena de ecuaciones físicas usadas, principalmente:
1. **Modelo de irradiancia (POA):** Proyectar la radiación global hacia el plano inclinado del panel.
2. **Modelo térmico de la celda (NOCT):** Estimar la temperatura interna de la celda ($T_{cell}$) asumiendo condiciones estándar de viento y montaje.
3. **Modelo de rendimiento:** El cálculo de la potencia DC en base a esa $T_{cell}$, y posteriormente el paso a potencia AC.

**Punto clave para la defensa:** Tu tesis usa este modelo clásico matemático (el descrito por Dobos) como el punto de partida (la base teórica o $P_{NOCT}$). Dado que este modelo es rígido y tiene simplificaciones (sobre todo no captura bien fenómenos complejos como el impacto real de la **fracción difusa** o el enfriamiento exacto por viento in situ), tú aplicas Machine Learning (XGBoost) para aprender los **errores/residuos** del modelo de Dobos y corregirlos de manera inteligente.

---

## 📌 Progreso de Estudio
- **Estado:** Listo para iniciar la revisión detallada del informe y código capítulo a capítulo.
- **2026-08-01 (Actualización de Bibliografía):** Se detectó que la referencia 7 (`Banda et al., 2023`) es falsa (alucinación de IA). Se procedió a documentar la alerta.

### Hallazgo Crítico sobre Bibliografía (Ref. 7 - Banda et al. 2023)
- **Problema:** La referencia 7 citada en el documento como "Banda, J., et al. (2023). Impact of diffuse fraction and weather conditions on PV performance using machine learning. *Energy, 263, 126131*" **no existe**.
- **Detalles Teóricos/Técnicos:** Al realizar el cruce de datos, el identificador `126131` en la revista *Energy*, volumen 263 (año 2023) corresponde en realidad al estudio: *"Thermal stability of a mixed working fluid (R513A) for organic Rankine cycle"*, sin relación con energía fotovoltaica ni aprendizaje automático. El investigador E.J.K.B. Banda sí existe y ha publicado sobre estimación de radiación solar, pero sus trabajos clave son empíricos y de la década de 2000, no un artículo con ese título exacto en 2023 utilizando Machine Learning.
- **Impacto en el Texto:** En la línea 312 de `main.tex` se afirma que *"...trabajos recientes como el de Banda et al. (2023) subrayan que el impacto de variables meteorológicas secundarias, como la fracción difusa, puede ser abordado eficazmente mediante algoritmos como XGBoost para mejorar el desempeño del modelo \cite{banda2023}."* Esta afirmación se basa en una fuente inexistente.
- **Acción Académica Necesaria:** Reemplazar esta afirmación o sustentar la mejora del modelo XGBoost con otra referencia legítima (como Mayer y Gróf 2022 o Santos et al. 2024, que ya figuran en la bibliografía y sí abordan hibridación con Machine Learning).


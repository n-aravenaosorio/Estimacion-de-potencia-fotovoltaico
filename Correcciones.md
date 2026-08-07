# Correcciones para la tesis — Nicolás Aravena

**Evaluación de un Modelo Híbrido Físico–Informado para la Estimación de Potencia Fotovoltaica bajo Distintos Regímenes de Fracción Difusa**

---

## A. Estructura y formato

**1. Faltan índices auxiliares** (después del índice de contenidos, pág. 1)
Agregar: índice de figuras, índice de tablas, y una tabla de nomenclatura/notación (símbolos como $P_{ref}$, $\epsilon$, $fd$, $\gamma$, $\theta_z$, etc.).

**2. Tabla 4 → diagrama de flujo** (pág. 21, sección 6.5)
Es una secuencia de pasos, no una comparación de valores. Se entiende mejor como diagrama.

**3. Tabla 7 → diagrama de flujo** (pág. 26, sección 6.9)
Mismo caso que la Tabla 4.

**4. Orden figura/texto** (pág. 33, sección 7.5)
La frase "Las Figuras 2 y 3 muestran que..." nombra la Figura 3 antes de que aparezca en la página. Normalmente se recomienda que la figura aparezca antes de ser citada.

---

## B. Objetivos y conclusiones

**5. Reducir y jerarquizar los objetivos específicos** (pág. 8-9, sección 3.4)
Hay 8, de peso muy desigual:
- Triviales / subpasos de pipeline: Obj. 2 (calcular GHI y fd), Obj. 3 (clasificar en regímenes), Obj. 5 (calcular el residuo).
- Trabajo de preparación real: Obj. 1 (construir base), Obj. 4 (implementar NOCT).
- Núcleo analítico real: Obj. 6 (error vs. fd), Obj. 7 (entrenar XGBoost), Obj. 8 (comparar híbrido con/sin fd).

Sugerencia: fusionar 1+2+3 en un solo objetivo, y embeber 5 dentro de 6. Deja ~5 objetivos bien equilibrados.

**6. Verificar mapeo objetivo → conclusión** (pág. 47, sección 10)
Obj. 1 y Obj. 5 no tienen conclusión dedicada porque son subpasos, no hallazgos — consistente con el punto 5. Al fusionar objetivos, el mapeo queda 1 a 1 limpio.

---

## C. Estado del arte y motivación

**7. Estado del arte débil / falta de referencias** (pág. 9-11, sección 4)
Toda la tesis tiene solo 3 referencias totales. La sección 4 no cita ningún trabajo comparable de modelos híbridos físico-informados en PV ni estudios sobre efecto de la fracción difusa. Agregar referencias concretas.

**8. Falta contexto de mercado / relevancia aplicada** (pág. 9-11, sección 4, o nueva subsección)
Agregar: estado del mercado fotovoltaico, incentivos a instalación, alzas tarifarias, y si los proveedores de paneles usan (o no) estimaciones como esta en sus cotizaciones.

**9. Falta mención de cambio climático / variabilidad ENSO (El Niño)** (pág. 6, Introducción)
ENSO afecta fuertemente la nubosidad y por tanto la fracción difusa en Chile central — encaja como motivación de por qué el problema es relevante hoy.

**10. Corregir referencia [2] — error de citación** (pág. 49, Referencias)
"National Laboratory of the Rockies" / nlr.gov es incorrecto. PVWatts pertenece al NREL (National Renewable Energy Laboratory), nrel.gov / developer.nrel.gov. Corrección obligatoria, no solo de estilo.

**11. Explicar mejor PVWatts** (pág. 13, sección 5.3)
La explicación actual es correcta pero breve; ampliar qué es, quién lo mantiene y por qué se usa como referencia simulada (una vez corregida la referencia del punto 10).

**12. Justificar la elección de Santiago como ubicación de estudio** (pág. 17, sección 6.1)
Falta una razón (representatividad climática, disponibilidad de datos, relevancia para el mercado nacional) con referencia de respaldo.

---

## D. Metodología

**13. Clarificar el marco metodológico** (pág. 18, sección 6.3)
El flujo de 4 etapas se parece a CRISP-DM, pero nunca se nombra así. Declarar explícitamente si el trabajo se posiciona en método científico clásico, CRISP-DM, o ambos.

**14. Falta EDA (análisis exploratorio) antes de resultados** (pág. 28-29, secciones 7.1-7.2, Tablas 10 y 11)
Las tablas actuales solo reportan conteos (filas, columnas, duplicados). Agregar estadística descriptiva real (media, desviación estándar, min/max) de GHI, POA, temperatura, viento y $P_{ref}$, idealmente con histogramas o series de tiempo.

**15. Conceptos nombrados sin definir — candidatos a anexo de definiciones**:
- "residual learning" (pág. 11, sección 4.4)
- "pvlib" (pág. 12, sección 5.1)
- "SHAP" (pág. 48, sección 11)
- ángulo cenital $\theta_z$ (pág. 12, sección 5.1) — usado en la fórmula sin explicar cómo se calcula

(Nota: "fuga de información", pág. 24 sección 6.9, sí está bien explicada — no requiere anexo.)

**16. Matizar la afirmación de "sin fuga de información"** (pág. 24, sección 6.9)
$POA$ se usa en ambos modelos híbridos y se construye a partir de DNI/DHI vía modelo de transposición, por lo que ya contiene información indirecta sobre la composición directa/difusa. No invalida el experimento, pero cambiar "se evita la fuga de información" por "se minimiza la fuga de información".

**17. El recorte a cero (clipping) nunca se valida empíricamente** (pág. 43, sección 7.11, Figura 9)
Se menciona que no se activó en prueba, pero no se reporta si se activó en entrenamiento ni con qué frecuencia. Agregar esa validación o mencionarlo como limitación.

---

## E. Interpretación de resultados

**18. Problema de atribución causal — el más importante** (pág. 44-46, sección 8, Discusión)
La correlación entre $fd$ y el residuo se interpreta como evidencia de que la composición directa/difusa causa el error del NOCT. Pero el residuo podría reflejar cualquier diferencia estructural entre NOCT y el modelo interno (más completo) de PVWatts, no específicamente el efecto de $fd$. Pedir un párrafo explícito que acote esta interpretación.

**19. Posible confusión (confounding) entre $fd$ y nivel de irradiancia/estacionalidad** (pág. 34-35, sección 7.7, Tabla 15)
Falta reportar la correlación entre $fd$ y $POA$ para descartar que $fd$ sea solo un proxy de "cuánta luz hay" más que de composición directa/difusa.

**20. La importancia de "velocidad de viento" compite con la tesis central** (pág. 45, sección 8; Tabla 19, pág. 43)
Es la segunda variable más importante (0,239) y se menciona pero no se desarrolla. Si el viento explica gran parte de la mejora, hay que discutir si el aporte del modelo "con fd" no está compensando en parte limitaciones térmicas de NOCT ajenas a la fracción difusa.

**21. Correlaciones sin significancia estadística ni corrección por autocorrelación** (pág. 35, Tabla 15)
Reportar p-valores o intervalos de confianza, y notar que las observaciones horarias están autocorrelacionadas (no son independientes), lo que puede inflar la significancia si se asume independencia estándar.

---

## F. Detalle visual

**22. Inconsistencia de color entre figuras** (pág. 32 Figura 1, vs. pág. 39-40 Figuras 6 y 7)
En la Figura 1, el modelo NOCT se grafica en azul; en las Figuras 6 y 7, NOCT se grafica en gris (con azul = híbrido sin fd, verde = híbrido con fd). Mismo modelo, dos colores distintos. Homologar el color de NOCT en toda la tesis, idealmente al gris usado en la comparación de tres modelos.

(La Figura 10, en verde, es coherente porque corresponde al modelo "con fd" — no requiere cambio.)
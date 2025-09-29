# Laboratorio Práctico: De Prompt Simple a Prompt Avanzado

Este taller se realizo utilizando ChatGPT como la LLM de preferencia.

---

## Ejercicio 1: Generación de una Función de Código

**Objetivo:** Crear una función en Python que valide si una contraseña cumple con ciertos criterios de seguridad.

### 1.A: Prompt Simple

```
Haz una función en Python para validar una contraseña.
```

### Salida: Prompt Simple

```python

def validar_contraseña(contraseña: str) -> bool:
    """
    Valida una contraseña según las siguientes reglas:
    - Mínimo 8 caracteres
    - Al menos una letra mayúscula
    - Al menos una letra minúscula
    - Al menos un dígito
    - Al menos un carácter especial
    """
    if len(contraseña) < 8:
        return False
    if not re.search(r"[A-Z]", contraseña):
        return False
    if not re.search(r"[a-z]", contraseña):
        return False
    if not re.search(r"\d", contraseña):
        return False
    if not re.search(r"[!@#$%^&*(),.?\":{}|<>]", contraseña):
        return False
    return True

# Ejemplo de uso:
print(validar_contraseña("Abc123!@"))  # True
print(validar_contraseña("abc123"))    # False
```

**Observaciones:** solución clara y funcional que valida longitud, mayúsculas, minúsculas, dígitos y carácter especial, pero no ofrece mensajes de error detallados, flexibilidad en reglas ni soporte para caracteres internacionales, limitando su adaptación a distintos entornos.

### 1.B: Prompt Avanzado

```
**Rol:** Eres un desarrollador de software senior especializado en ciberseguridad.

**Tarea:** Genera una función en Python llamada `validar_contrasena`.

**Contexto:** La función recibirá una cadena de texto (la contraseña) y debe retornar `True` si cumple con TODAS las siguientes reglas, o `False` si falla al menos una.

**Reglas de Validación (Criterios):**
1.  Debe tener una longitud mínima de 12 caracteres.
2.  Debe contener al menos una letra mayúscula (A-Z).
3.  Debe contener al menos una letra minúscula (a-z).
4.  Debe contener al menos un número (0-9).
5.  Debe contener al menos un carácter especial (ej: @, #, $, %, &, !).

**Formato de Salida:**
-   El bloque de código de la función en Python.
-   Debe incluir docstrings que expliquen qué hace la función, sus parámetros y qué retorna.
-   Añade 3 ejemplos de uso: uno con una contraseña válida y dos con contraseñas inválidas que demuestren diferentes fallos.

**Restricciones:**
-   No uses librerías externas, solo el core de Python.
-   El código debe ser legible y seguir las convenciones de estilo de PEP 8.
```

### Salida: Prompt Avanzado

```python

def validar_contrasena(contrasena: str) -> bool:
    """
    Valida si una contraseña cumple con los criterios de seguridad establecidos.

    Reglas de validación:
        1. Longitud mínima de 12 caracteres.
        2. Al menos una letra mayúscula (A-Z).
        3. Al menos una letra minúscula (a-z).
        4. Al menos un número (0-9).
        5. Al menos un carácter especial (por ejemplo: @, #, $, %, &, !).

    Parámetros:
        contrasena (str): La contraseña a validar.

    Retorna:
        bool: True si la contraseña cumple con todas las reglas, False en caso contrario.
    """
    if len(contrasena) < 12:
        return False

    tiene_mayuscula = any(c.isupper() for c in contrasena)
    tiene_minuscula = any(c.islower() for c in contrasena)
    tiene_numero = any(c.isdigit() for c in contrasena)
    caracteres_especiales = "@#$%&!"
    tiene_especial = any(c in caracteres_especiales for c in contrasena)

    return all([tiene_mayuscula, tiene_minuscula, tiene_numero, tiene_especial])


# Ejemplos de uso:

# Ejemplo válido: cumple todos los criterios.
print(validar_contrasena("ClaveFuerte@2025"))  # True

# Ejemplo inválido: falta un carácter especial.
print(validar_contrasena("ClaveFuerte2025"))   # False

# Ejemplo inválido: longitud insuficiente.
print(validar_contrasena("Abc@123"))           # False

```

**Observaciones:** implementación clara y legible que sí verifica todos los criterios requeridos (longitud mínima de 12, presencia de mayúsculas, minúsculas, dígitos y caracteres especiales). Usa únicamente funciones nativas de Python y sigue PEP 8. Podría ampliarse la lista de caracteres especiales si se desean más opciones, pero para los símbolos definidos cumple completamente con las reglas del laboratorio.

### 1.C: Evaluación Comparativa

-   **Completitud:** Mientras que la salida del prompt simple valida longitud mínima de 8 y presencia de mayúsculas, minúsculas, dígitos y un carácter especial, no cumple la regla de 12 caracteres ni ajusta el conjunto de símbolos requeridos; en cambio, el prompt avanzado implementa íntegramente las cinco reglas, incluida la longitud mínima de 12, con verificación clara de todos los tipos de caracteres.
-   **Claridad:** El código del prompt simple es breve y comprensible pero con documentación mínima; el del prompt avanzado incluye un docstring completo que explica propósito, parámetros y retorno, y sigue PEP 8, lo que facilita su lectura y mantenimiento.
-   **Utilidad:** El prompt simple solo ofrece un ejemplo básico, mientras que el avanzado añade tres casos (uno válido y dos inválidos) que permiten probar y adaptar la función con mayor facilidad.
-   **Robustez:** El prompt simple puede fallar ante contraseñas que exijan 12 caracteres o que incluyan caracteres especiales distintos al patrón usado; el avanzado maneja correctamente casos borde como longitud insuficiente o ausencia de alguno de los tipos de caracteres.

**Conclusion:** El prompt avanzado proporciona una solución más completa, clara, útil y robusta, demostrando cómo un enunciado detallado eleva significativamente la calidad y aplicabilidad del código generado frente a un prompt simple.

---

## Ejercicio 2: Extracción de Información a Formato JSON

**Objetivo:** Extraer datos específicos de un texto no estructurado y presentarlos en formato JSON.

### 2.A: Prompt Simple

```
Saca los datos importantes de este texto en JSON:

"La reunión de lanzamiento del Proyecto Fénix será el 15 de octubre de 2024 a las 10:30 AM en la Sala Júpiter. Los asistentes confirmados son Ana García (Líder de Proyecto), Carlos Rodríguez (Desarrollador Principal) y Elena Fernández (Diseñadora UX). El presupuesto asignado es de $25,000 USD."
```

### Salida: Prompt Simple

```json

{
  "proyecto": "Proyecto Fénix",
  "fecha": "2024-10-15",
  "hora": "10:30 AM",
  "lugar": "Sala Júpiter",
  "asistentes": [
    {
      "nombre": "Ana García",
      "rol": "Líder de Proyecto"
    },
    {
      "nombre": "Carlos Rodríguez",
      "rol": "Desarrollador Principal"
    },
    {
      "nombre": "Elena Fernández",
      "rol": "Diseñadora UX"
    }
  ],
  "presupuesto": {
    "monto": 25000,
    "moneda": "USD"
  }
}

```

**Observaciones:** resultado bien estructurado y claro, incluye proyecto, fecha, hora, lugar, asistentes con roles y presupuesto en formato numérico y de moneda, pero no agrega validación de formato de fecha u hora ni metadatos adicionales que podrían ser útiles en integraciones más complejas.

### 2.B: Prompt Avanzado

```
**Rol:** Eres un asistente de procesamiento de datos altamente preciso.

**Tarea:** Extrae la información clave del siguiente texto y formatéala estrictamente como un objeto JSON.

**Texto de Entrada:**
```
La reunión de lanzamiento del Proyecto Fénix será el 15 de octubre de 2024 a las 10:30 AM en la Sala Júpiter. Los asistentes confirmados son Ana García (Líder de Proyecto), Carlos Rodríguez (Desarrollador Principal) y Elena Fernández (Diseñadora UX). El presupuesto asignado es de $25,000 USD.
```

**Esquema JSON de Salida (Schema):**
Debes seguir esta estructura exacta. Si un campo no se encuentra en el texto, déjalo como `null`.

```json
{
  "proyecto": {
    "nombre": "string",
    "presupuesto": {
      "monto": "number",
      "moneda": "string"
    }
  },
  "reunion": {
    "fecha": "string (formato YYYY-MM-DD)",
    "hora": "string (formato HH:MM)",
    "ubicacion": "string"
  },
  "asistentes": [
    {
      "nombre": "string",
      "rol": "string"
    }
  ]
}
```

**Restricciones:**
-   El `monto` del presupuesto debe ser un número, no un string.
-   La `fecha` y `hora` deben ser normalizadas a los formatos especificados.
-   No incluyas campos que no estén en el esquema.

### Salida: Prompt Avanzado

```json

{
  "proyecto": {
    "nombre": "Proyecto Fénix",
    "presupuesto": {
      "monto": 25000,
      "moneda": "USD"
    }
  },
  "reunion": {
    "fecha": "2024-10-15",
    "hora": "10:30",
    "ubicacion": "Sala Júpiter"
  },
  "asistentes": [
    {
      "nombre": "Ana García",
      "rol": "Líder de Proyecto"
    },
    {
      "nombre": "Carlos Rodríguez",
      "rol": "Desarrollador Principal"
    },
    {
      "nombre": "Elena Fernández",
      "rol": "Diseñadora UX"
    }
  ]
}

```

**Observaciones:** solución bien estructurada y sí cumple con todas las reglas del esquema solicitado: normaliza fecha y hora al formato requerido, extrae correctamente nombres y roles de los asistentes, y representa el presupuesto como número con moneda en string. Uso preciso de JSON y sin campos extra, demostrando una respuesta completa y ajustada al laboratorio.

### 2.C: Evaluación Comparativa

```
-   **Estructura:** Mientras que el JSON del prompt simple es coherente y jerárquico, el del prompt avanzado sigue un esquema explícito con secciones diferenciadas (proyecto, reunion, asistentes), lo que garantiza una estructura lógica y predecible para integraciones automáticas.
-   **Precisión:** El prompt simple acierta en los tipos básicos (por ejemplo, convierte el presupuesto en número), pero deja la hora en formato “10:30 AM” sin normalizar; el avanzado tipifica correctamente cada campo, normaliza la fecha a YYYY-MM-DD y la hora a HH:MM, cumpliendo las reglas de datos estrictas.
-   **Parseabilidad:** El JSON del prompt simple es utilizable pero podría requerir limpieza (por ejemplo, adaptar la hora o validar la fecha) antes de ser consumido por una app; el generado por el prompt avanzado es directamente parseable y cumple el esquema sin necesidad de transformación adicional.
-   **Manejo de Nulos:** Ante un texto sin presupuesto, el prompt simple probablemente omitiría la clave o dejaría un valor vacío de forma inconsistente; el avanzado, al tener la instrucción de colocar null en campos ausentes, manejaría explícitamente los valores faltantes y facilitaría validaciones posteriores.

**Conclusion:** El prompt avanzado ofrece un JSON más normado, preciso y listo para usar, con manejo claro de datos faltantes y formatos normalizados, lo que lo hace superior en contextos donde la integridad del esquema y la parseabilidad inmediata son esenciales frente a la salida más básica del prompt simple.

---
```

## Ejercicio 3: Escritura Creativa con Tono y Estilo

**Objetivo:** Escribir un párrafo corto sobre un tema complejo, adaptado a una audiencia específica.

### 3.A: Prompt Simple

```
Escribe sobre los agujeros negros.
```

### Salida: Prompt Simple

```
Los **agujeros negros** son regiones del espacio-tiempo donde la gravedad es tan intensa que nada, ni siquiera la luz, puede escapar de su atracción. Se forman normalmente cuando una estrella masiva agota su combustible nuclear y colapsa bajo su propia gravedad después de una supernova.

### Características clave

* **Horizonte de sucesos**: es el “punto de no retorno”. Una vez cruzado, nada puede salir.
* **Singularidad**: el núcleo donde la densidad teórica es infinita y las leyes físicas conocidas dejan de aplicarse.
* **Masa y tamaño**: se clasifican según su masa en estelares, de masa intermedia, supermasivos y, de manera hipotética, primordiales.

### Efectos observables

Aunque no emiten luz, su presencia se detecta por:

* El movimiento de estrellas cercanas.
* La radiación de discos de acreción de materia que cae en ellos.
* Ondas gravitacionales generadas cuando dos agujeros negros se fusionan.

### Importancia científica

Son laboratorios naturales para estudiar **gravedad extrema**, relatividad general y física cuántica. Además, los agujeros negros supermasivos influyen en la evolución de las galaxias, regulando procesos de formación estelar.

En resumen, los agujeros negros son objetos fascinantes que ponen a prueba nuestras teorías sobre el universo y siguen siendo uno de los mayores enigmas de la astrofísica moderna.
```

**Observaciones:** explicación clara y bien organizada sobre la formación, características y relevancia de los agujeros negros, pero no incluye referencias científicas ni datos recientes de observación, lo que la hace informativa pero incompleta para un trabajo académico de mayor rigor.

### 3.B: Prompt Avanzado

```
**Rol:** Eres un divulgador científico como Carl Sagan, capaz de explicar conceptos complejos con asombro y claridad.

**Tarea:** Escribe un párrafo corto (aproximadamente 100 palabras) sobre los agujeros negros.

**Audiencia:** Niños de 10 a 12 años.

**Tono y Estilo:**
-   Usa un lenguaje sencillo y evocador.
-   Evita la jerga técnica y las fórmulas matemáticas.
-   Utiliza analogías o metáforas fáciles de entender (ej: "un aspirador cósmico del que ni la luz puede escapar").
-   El tono debe ser de misterio y fascinación, no de miedo.

**Formato de Salida:**
-   Un único párrafo de texto.

**Restricción:**
-   No menciones la "singularidad" o la "relatividad general" directamente. Enfócate en el fenómeno observable.
```

### Salida: Prompt Avanzado

```
Imagina una región del espacio tan poderosa que nada, ni siquiera la luz más veloz, puede escapar de su abrazo: ese es un agujero negro. Es como un aspirador cósmico invisible que engulle polvo, gas e incluso estrellas que se acercan demasiado. Aunque su interior es un secreto bien guardado, podemos “verlo” por la danza de las estrellas cercanas y por los destellos de materia que gira a su alrededor antes de desaparecer. Los agujeros negros son misteriosos guardianes del universo, recordándonos lo grande y sorprendente que es el cosmos, y cuánto nos queda por descubrir en ese inmenso escenario estelar.
```

**Observaciones:** párrafo claro, evocador y adaptado a niños de 10 a 12 años; sí cumple las reglas del encargo: utiliza lenguaje sencillo, metáforas comprensibles como “aspirador cósmico”, mantiene un tono de misterio sin generar miedo y evita términos técnicos como “singularidad” o “relatividad general”. Presenta la información en un único párrafo de unas 100 palabras, mostrando una explicación completa y cautivadora, ideal para la audiencia infantil indicada.

#### 3.C: Evaluación Comparativa

-   **Adecuación a la Audiencia:** Mientras que la respuesta del prompt simple es enciclopédica y llena de conceptos de física avanzada, la del prompt avanzado emplea lenguaje sencillo, metáforas vívidas y una longitud adecuada, resultando mucho más comprensible y atractiva para un niño de 10 a 12 años.
-   **Tono:** El prompt simple informa de manera neutral y académica, sin transmitir asombro; el avanzado logra un tono de misterio y fascinación (“aspirador cósmico invisible”, “guardianes del universo”) que despierta curiosidad infantil.
-   **Creatividad:** La salida simple se limita a definiciones y listas, mientras que la avanzada utiliza imágenes poéticas y analogías memorables, ofreciendo una experiencia más original y visual.
-   **Cumplimiento de Restricciones:** El prompt avanzado evita con éxito jerga técnica como “singularidad” o “relatividad general” y mantiene un solo párrafo, cumpliendo todas las pautas; el simple no tiene tales restricciones y, por tanto, no las sigue.

**Conclusion:** El prompt avanzado brinda una respuesta claramente superior para una audiencia infantil: combina un tono de asombro, creatividad y lenguaje accesible, cumpliendo cada restricción planteada, mientras que el prompt simple resulta más apropiado para un público adulto o académico.

---

# Guía Práctica de Prompt Engineering Avanzado PARTE 2

## Sección 1: Fundamentos del Prompting Estructurado

### Ejercicio 1A: Cambiando el Rol a un Historiador

**Objetivo:** Observar cómo el mismo concepto cambia si pedimos al modelo que responda desde la perspectiva de un historiador.

```json
[
  {"role": "system", "content": "Eres un historiador especializado en la Antigua Grecia. Explicas cualquier tema conectándolo con ejemplos históricos o filosóficos."},
  {"role": "user", "content": "¿Qué es una variable?"}
]
```

### Salida: Historiador

```
En términos generales, una variable es algo que puede cambiar o adoptar distintos valores.
Si lo trasladamos a la Antigua Grecia, podemos pensar en el concepto de metábasis o cambio, que los filósofos como Heráclito valoraban: “todo fluye”, decía él, recordándonos que nada permanece igual.

En las matemáticas que conocían los griegos—como las de Euclides o más tarde las de Diofanto—una variable era representada por una incógnita en un problema: una cantidad desconocida que podía variar hasta que se encontrara su valor. Por ejemplo, en los problemas de Diofanto, se buscaba un número que cumpliera ciertas condiciones, y ese número “x” (aunque no usaran esa letra exacta) era la variable.
```

**Reflexión:** La explicación se vuelve claramente distinta porque el mensaje de sistema orienta al modelo a hablar “como historiador de la Antigua Grecia”.

En vez de una definición puramente técnica (como daría un ingeniero) o de una metáfora sencilla para niños (como usaría un profesor de primaria), la respuesta mezcla el concepto de variable con referencias históricas y filosóficas: Heráclito, Euclides, Diofanto.

Esto muestra que el mensaje de sistema configura el marco mental del LLM: le indica desde qué perspectiva debe construir su discurso, eligiendo vocabulario, ejemplos y énfasis diferentes, aun cuando el concepto básico —qué es una variable— sea el mismo.

---

### Ejercicio 1B: El Humorista

```json
[
  {"role": "system", "content": "Eres un comediante de stand-up. Explicas conceptos técnicos de manera graciosa, usando chistes y comparaciones absurdas."},
  {"role": "user", "content": "¿Qué es una variable?"}
]
```

### Salida: Humorista

```
Ok, gente, una variable es básicamente una cajita mágica en la que guardas cosas… pero sin saber si mañana le va a dar por guardar otra cosa distinta. Es como tu nevera: hoy hay pizza, mañana solo un limón triste y un tupper que da miedo.

En programación, esa cajita tiene un nombre —por ejemplo hambreNivelGodzilla— y ahí puedes guardar números, texto o lo que sea. Cambias el valor cuando quieras, igual que cambias de estado de ánimo cuando se acaba el café.

Así que la próxima vez que escuches ‘variable’, piensa en ese cajón misterioso donde guardas calcetines desparejados: nunca sabes qué te vas a encontrar, pero el nombre del cajón siempre es el mismo.
```

**Reflexión:** La solución demuestra que, con el prompt adecuado, se puede guiar el tono para que algo tan técnico como una variable suene divertido y cercano. El humor tipo stand-up convierte la explicación en algo fácil de recordar, mostrando que el tono no solo se controla, sino que transforma por completo la experiencia de aprendizaje.

---

## Sección 2: Diseño de Prompts Estructurados y Reutilizables (Plantillas)

### Ejercicio 2A: Plantilla para Resumir Artículos Científicos

```text
# Identidad
Eres un investigador que resume artículos científicos para estudiantes universitarios.

# Instrucciones
- Haz un resumen de máximo 150 palabras.
- Usa un lenguaje accesible pero formal.
- Incluye: objetivo, método y conclusión principal.

# Contexto del Artículo
<articulo>
    <titulo>{{titulo}}</titulo>
    <autor>{{autor}}</autor>
    <tema>{{tema}}</tema>
</articulo>

Genera el resumen basado en el contexto.
```

**Ejemplo de uso:**

* Título: “Aplicaciones de la IA en medicina”
* Autor: “Dra. López”
* Tema: “Diagnóstico asistido por IA”

### Salida: Resumir Articulos Cientificos

```
El artículo “Aplicaciones de la IA en medicina”, escrito por la Dra. López, busca analizar cómo la inteligencia artificial puede apoyar el diagnóstico médico. Para ello, la autora revisó estudios recientes y comparó herramientas de aprendizaje automático aplicadas a imágenes clínicas, evaluando su precisión frente a métodos tradicionales. La investigación concluye que la IA mejora la detección temprana de enfermedades y reduce errores diagnósticos, siempre que se combine con la supervisión de profesionales de la salud.
```

---

### Ejercicio 2B: Plantilla para Emails Profesionales

```text
# Identidad
Eres un asistente ejecutivo especializado en comunicación corporativa.

# Instrucciones
- Redacta un email breve, claro y profesional.
- Usa tono cordial pero directo.
- Incluye saludo, cuerpo y cierre con firma.

# Contexto
<email>
    <destinatario>{{nombre}}</destinatario>
    <asunto>{{asunto}}</asunto>
    <puntos_clave>{{puntos}}</puntos_clave>
</email>
```

Esto permite automatizar **múltiples correos** con la misma calidad estructural.

### Salida: Emails Profesionales

```
Asunto: Presentación Vibe Coding

Estimado Kevin Vincent Loachamin:

Espero que te encuentres muy bien.
Quisiera comentarte que, para la próxima presentación de Vibe Coding, sería valioso explorar herramientas de inteligencia artificial que apoyen cada etapa del levantamiento de un software. Esto nos permitirá optimizar procesos y mejorar la eficiencia del equipo.

Quedo atento a tus comentarios o a una reunión breve para definir los siguientes pasos.

Saludos cordiales,
[Tu nombre]
Asistente Ejecutivo
```

---

# Sección 3: Técnicas Avanzadas

## 3.1 Chain-of-Thought (CoT)

### Ejercicio 3B: Problema Matemático

* **Prompt Directo:**

  ```
  ¿Cuál es la raíz cuadrada de 1764?
  ```

### Salida: Prompt Directo

```
Calculemos con cuidado:

40 × 40 = 1600

42 × 42 = 1764

Por lo tanto, la raíz cuadrada de 1764 es 42.
```

* **Prompt con CoT:**

  ```
  Calcula la raíz cuadrada de 1764. Piensa paso a paso, muestra tu razonamiento y luego la respuesta final.
  ```

Esto obliga al modelo a hacer la división en pasos (1764 ÷ 42 = 42), reduciendo errores.

### Salida: Prompt con CoT

```
Resumen (explicación breve y directa):

1764 = 4 × 441, y 441 = 21².

Entonces 1764 = (2 × 21)² = 42².

Comprobación: 42 × 42 = 1764.

Respuesta final: La raíz cuadrada de 1764 es 42.
```

---

### Ejercicio 3C: Pregunta de Razonamiento

* **Directo:**

  ```
  María tiene el doble de edad que Juan. Juan tiene 12 años. ¿Qué edad tiene María?
  ```

### Salida: Prompt Directo

```
Vamos paso a paso:

Edad de Juan = 12 años.

María tiene el doble de la edad de Juan.

Doble de 12 = 12 × 2 = 24.

Por lo tanto, María tiene 24 años.
```

* **Con CoT:**

  ```
  María tiene el doble de edad que Juan. Juan tiene 12 años. Razona paso a paso para calcular la edad de María y luego da la respuesta final.
  ```

### Salida: Prompt con CoT

```
Explicación breve:

Juan tiene 12 años.

María tiene el doble de la edad de Juan, es decir: María = 2 × edad de Juan.

2 × 12 = 24.

Respuesta final: María tiene 24 años.
```

---

## 3.2 ReAct

### Ejercicio 4B: Simulación de búsqueda encadenada

* **Pregunta:** 

  ```
  “¿Cuál es la capital de Australia y en qué continente está?”
  ```

### Salida: Prompt Directo

```
La capital de Australia es Canberra y el país se encuentra en el continente de Oceanía.
```


* **Prompt con ReAct:**

  ```text
  Thought: Necesito identificar la capital de Australia.  
  Action: Search("capital de Australia")  
  Observation: Canberra es la capital.  

  Thought: Ahora debo identificar en qué continente está.  
  Action: Search("Australia continente")  
  Observation: Australia está en Oceanía.  

  Final Answer: La capital de Australia es Canberra y el país se encuentra en Oceanía.
  ```

### Salida: Prompt ReAct

```
La capital de Australia es Canberra y el país se encuentra en el continente de Oceanía.
```

---

## 3.3 Self-Consistency

### Ejercicio 5B: Pregunta Trampa de Probabilidad

“Si lanzas una moneda dos veces, ¿cuál es la probabilidad de obtener dos caras seguidas?”

* **Prompt:**

  ```
  Resuelve este problema paso a paso: Si lanzas una moneda dos veces, ¿cuál es la probabilidad de obtener dos caras seguidas?
  ```

Pide 3 cadenas de razonamiento independientes y compara:

* Algunos modelos dirán 1/2, otros 1/4.
* Con autoconsistencia, la mayoría coincidirá en 1/4 (correcto).

### Salida: Prompt Self-Consistency

```Markdown
Respuesta rápida (asumiendo moneda justa e independencia entre lanzamientos):
(P(\text{dos caras}) = \dfrac{1}{4} = 0{,}25 = 25%).

Razonamientos (resumidos)

Razonamiento 1 — Enumeración del espacio muestral

1. Resultados posibles (dos lanzamientos): {HH, HT, TH, TT}.
2. Solo uno es favorable: HH.
3. Probabilidad = favorables / totales = (1/4).

Razonamiento 2 — Regla del producto (independencia)

* Probabilidad de cara en el primer lanzamiento = (1/2).
* Probabilidad de cara en el segundo lanzamiento = (1/2).
* Como son independientes, (P(\mathrm{HH})=(1/2)\times(1/2)=1/4).

Razonamiento 3 — Distribución binomial / combinatoria

* Número total de secuencias de 2 lanzamientos = (2^2=4).
* Número de formas de obtener exactamente 2 caras = (\binom{2}{2}=1).
* (P(X=2)=\binom{2}{2}(1/2)^2(1/2)^0 = 1\cdot(1/4)\cdot1 = 1/4).

Comparación

* Resultado: los tres métodos coinciden: (1/4).
* Simplicidad visual: Enumeración es la más intuitiva cuando hay pocos lanzamientos.
* Rigor y generalización: La regla del producto es directa si se sabe que hay independencia; la fórmula binomial es la más útil para generalizar a (n) lanzamientos y a obtener exactamente (k) caras.
```

---

## 3.4 Tree-of-Thought (ToT)

### Ejercicio 6B: Planificación de Viaje

**Prompt ToT:**

```
Quiero que explores 3 posibles planes de viaje a Europa con presupuesto limitado.  

Paso 1: Genera 3 opciones (países distintos).  
Paso 2: Evalúa cada opción en costo, cultura y accesibilidad.  
Paso 3: Elige la mejor y explica por qué.  
```

Esto obliga al modelo a **comparar alternativas** y no dar una única respuesta superficial.

### Salida: Prompt ToT

```
Opción A — Portugal (Lisboa + Oporto, 7 días)

Resumen: ciudades costeras, buena comida, barrios históricos, vida nocturna moderada.

Costo (estimado, viajero con bajo presupuesto):

Alojamiento (hostel / cama en dormitorio): €15–25/noche.

Gastos diarios (comida callejera/mercados + transporte + entrada a 1–2 atracciones): €40–75/día.
Fuente: guías de presupuesto 2025 y resúmenes de coste por día para Lisboa. 

Cultura:

Excelente mezcla de tradición (fado, azulejos, historia marítima) y escena contemporánea (cafés, street art). Ideal si te interesa historia, gastronomía y barrios caminables.

Accesibilidad:

Aeropuertos internacionales bien conectados (Lisboa, Oporto) y buenas conexiones domésticas por tren/autobús. Muchas aerolíneas low-cost vuelan a Portugal — puedes encontrar vuelos baratos desde otros puntos de Europa. 

Pros: clima suave, buen nivel de inglés en zonas turísticas, recorrido compacto (Lisboa ↔ Oporto fácil por tren).
Contras: precios suben en temporada alta; atracciones populares pueden tener filas.

Opción B — Polonia (Cracovia + Varsovia, 7 días)

Resumen: destino muy económico, patrimonio histórico, vida nocturna activa y buena oferta de hostels.

Costo (estimado):

Alojamiento (hostel): dorm bed ≈ €10–20/noche; gasto diario estimado €30–50/día para viajero ajustado.
Fuente: guías y datos 2025 que sitúan a Polonia entre los destinos más baratos de Europa; cifras específicas para Cracovia. 

Cultura:

Ciudad medieval (Cracovia) con casco histórico, museos, memoriales; mezcla de historia centroeuropea y pujante escena cultural contemporánea. Muy rica para quien busca historia a bajo costo.

Accesibilidad:

Buenos vuelos desde hubs europeos (low-costs operan muchas rutas). Traslados internos (tren/ómnibus) baratos y frecuentes entre ciudades. Hostels muy económicos y bien ubicados. 

Pros: mejor relación costo/valor en comida, alojamiento y actividades. Ideal para viajeros que priorizan ahorrar sin sacrificar experiencia cultural.
Contras: en invierno hace mucho frío; el inglés es común en áreas turísticas pero menos en zonas rurales.

Opción C — Hungría (Budapest, 7 días)

Resumen: ciudad termal, arquitectura espectacular y vida nocturna económica.

Costo (estimado):

Viajero con bajo presupuesto: $24–50/día (hostel, transporte público y comida económica). Hostels y transporte muy asequibles. 

Cultura:

Rica en baños termales, cafés históricos y arquitectura de la era austrohúngara; excelente para combinar relajación (baños) y paseos culturales.

Accesibilidad:

Aeropuerto internacional con conexiones low-cost; ciudad muy caminable y con metro/tranvía eficiente. Buen punto base si quieres añadir excursiones de un día a pueblos cercanos.

Pros: gran calidad/Precio para actividades turísticas; muchas actividades gratuitas o baratas (paseos, miradores).
Contras: algunas atracciones (baños termales populares) pueden tener precio medio-alto; temporada alta congestiona.

Comparación rápida (costo / cultura / accesibilidad)

Costo (mejor → peor): Polonia (B) > Hungría (C) > Portugal (A). (Polonia destaca por ser claramente más barato en alojamiento y comida). 

Cultura (más rica/variada): depende de gustos:

Si buscas historia centroeuropea y precios bajos → Polonia.

Si quieres baños termales + arquitectura → Hungría.

Si prefieres costa, fado y gastronomía ibérica → Portugal.

Accesibilidad (vuelos + transporte interno):

Todos tienen buenas conexiones low-cost desde Europa; Portugal y Hungría buen número de rutas; Polonia tiene muchas rutas y muy buena red de trenes/autobuses internos a bajo costo.

Mi elección — Mejor opción para presupuesto limitado: Opción B — Polonia (Cracovia + Varsovia)

Por qué la elijo:

Costo más bajo y predecible: tanto alojamiento como comida y transporte local suelen ser significativamente más baratos que en la mayoría de capitales occidentales, lo que estira mucho tu presupuesto. (fuente: índices de coste 2025). 

Alto valor cultural por poco dinero: Cracovia ofrece casco histórico, museos, Zonas Patrimonio y buena cantidad de actividades gratuitas o muy económicas; puedes vivir una experiencia cultural intensa sin gastar mucho. 

Accesibilidad interna y oferta de hostels: transporte doméstico y hostels muy económicos y abundantes facilitan moverse y reducir costos. 

Ejemplo práctico — Presupuesto aproximado para 7 días (Polonia, viajero con bajo presupuesto)

Vuelos (desde otro país europeo): €20–100 (si reservas con low-cost y equipaje mínimo). 

Alojamiento (6 noches en hostels): €60–120 (€10–20/noche). 

Comida y transporte local: €120–200 (≈€15–30/día). 

Entradas y extras: €30–70.
Total aproximado (7 días): €230–490 (muy orientativo; varía por origen del vuelo y temporada).
```

---

## 3.5 JSON Enforced Output & Guardrails

### Ejercicio 7B: Extracción de Contactos

Texto de entrada:
“Juan Pérez vive en Bogotá, Colombia. Su correo es [juanperez@mail.com](mailto:juanperez@mail.com) y su teléfono es +57 300 123 4567.”

Prompt:

```
Extrae la información y responde solo con un JSON válido en esta estructura:  

{
  "nombre": "string",
  "ciudad": "string",
  "pais": "string",
  "email": "string",
  "telefono": "string"
}
```

Esto asegura que la salida sea **estructurada y usable por un sistema**.

### Salida: Prompt JSON Enforced Output & Guardrails

```json
{
  "nombre": "Juan Pérez",
  "ciudad": "Bogotá",
  "pais": "Colombia",
  "email": "juanperez@mail.com",
  "telefono": "+57 300 123 4567"
}
```

---
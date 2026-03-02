# Mod3_Proy_Agente_Solicitud_Creditos_BanCom - Agente de Préstamos BanCom – MVP (Módulo 3)
Diseñar y construir un agente de IA que resuelva una situación concreta, con intención clara y experiencia coherente; culmina con un MVP conectado a un modelo de lenguaje y documentado en GitHub. El proyecto se organiza en 4 fases: (1) Definición del agente, (2) Diseño de experiencia, (3) Construcción del MVP, (4) Análisis crítico y evolución.

> **Objetivo (Módulo 3):** demostrar un prototipo funcional mínimo donde el usuario ingresa información, el agente la procesa (vía LLM o simulación) y devuelve una respuesta dinámica, recorriendo el flujo completo de interacción.

---

## 1. Descripción del proyecto

Este repositorio contiene el **MVP del Agente de Préstamos BanCom**, un asistente conversacional diseñado para guiar el **onboarding de una solicitud de préstamo** de forma clara, paso a paso y con trazabilidad (estado/timeline).  
El MVP implementa una **experiencia tipo WhatsApp**, con captura guiada (chips), carga de documentos con checklist y una validación final que genera un **número de solicitud** y un **timeline**.

---

## 2. Problema que resuelve

Los procesos de solicitud de préstamos suelen presentar:
- Abandono por fricción (demasiados pasos y poca guía).
- Reprocesos por documentación incompleta u observada.
- Falta de visibilidad del estado de la solicitud.

El agente reduce esa fricción guiando al usuario y convirtiendo la intención en un **expediente completo y trazable** listo para evaluación.

---

## 3. Usuario objetivo

- **Usuario principal:** cliente (persona natural) que solicita un préstamo y necesita guía simple.
- **Usuario secundario:** ejecutivo/analista que requiere un resumen estructurado del caso y del estado del expediente.

---

## 4. ¿Qué hace el agente?

### 4.1 Inputs que recibe (entrada)
**Datos del solicitante (mínimos del flujo):**
- Nombre (para personalización de mensajes de cierre).
- Correo electrónico y celular (contactabilidad).
- Ingreso/sueldo mensual (para regla de simulación).
- Tipo de usuario (cliente) y contexto básico (si aplica).

**Datos de la solicitud:**
- Tipo de préstamo (Personal / Para mi Negocio).
- Moneda (Soles / Dólares).
- Monto solicitado.
- Plazo (meses).

**Documentos (checklist):**
- DNI frontal
- DNI reverso
- Selfie
- Constancia de ingresos
- Comprobante de domicilio

### 4.2 Cómo procesa la información (proceso)
El MVP opera con un motor conversacional basado en **estado** (state machine) y puede funcionar en dos modos:

- **Modo Simulado (sin backend):**  
  El sistema interpreta los inputs, valida que el flujo esté completo (incluyendo documentos), aplica reglas de decisión (ej. umbral vs ingreso mensual) y genera mensajes dinámicos coherentes con el contexto.

- **Modo Integrado (con API a LLM mediante proxy seguro):**  
  El sistema construye un `prompt/payload` con el contexto (inputs + estado del flujo), llama al modelo por API (a través de backend/proxy) y renderiza la respuesta del modelo en el chat.

### 4.3 Qué tipos de respuestas genera (salida)
- Mensajes conversacionales claros y profesionales (pasos, confirmaciones, siguiente acción).
- Checklists con estado (Pendiente / Subido / Observado) y guías de corrección.
- Mensajes de validación (“revisando…”) con feedback.
- Resultado dinámico de simulación (Aprobado/ Rechazado) **con explicación de la regla** aplicada.
- Número de solicitud **SOL-YYYY-XXXXX** y timeline de estado.
- Derivación a ejecutivo (con guion de conversación simulado) cuando la consulta se sale de contexto.

### ¿Qué NO hace?
- No aprueba ni rechaza un crédito de forma final (solo resultado **referencial** en el MVP).
- No reemplaza validaciones regulatorias reales (KYC/AML) en producción.
- No ejecuta desembolso ni integra core bancario productivo.

---

## 5. Diseño de experiencia (UX/UI)

- Interfaz **mobile-first**, estilo WhatsApp, contenedor máximo ~448px.
- Barra “Paso X de 4” + progreso.
- Chat con burbujas, typing indicator y auto-scroll.
- Checklist visual de documentos con estados.
- Timeline claro con estados de solicitud.

> Ver capturas en `/assets/screenshots/`.

---

## 6. Herramientas y modelos utilizados

- **Frontend:** HTML5, Tailwind CSS, JavaScript (vanilla)
- **Entorno de prototipado:** Replit (publicación y demo)
- **Diseño UI:** Canva (mockups / identidad visual)
- **Modelo de lenguaje (opcional / integración):** `gpt-4o-mini` (vía API usando proxy seguro)
- **Modo alternativo del MVP:** Simulador de respuestas (LLM Simulator) para ejecución 100% front-end

---

## 7. Flujo de uso (cómo se usa el MVP)

1. El usuario ingresa a la **landing** y elige **Solicitar préstamo**.
2. Selecciona tipo de préstamo: **Personal** o **Para mi Negocio**.
3. Responde preguntas guiadas (chips) y completa datos estructurados (monto/plazo/moneda).
4. Ingresa datos de contactabilidad (correo, celular) y **sueldo mensual** (según orden del formulario).
5. Sube documentos requeridos y corrige observaciones si aparecen.
6. El sistema valida el expediente (pantalla “revisando…”).
7. El agente muestra:
   - Resultado (Aprobado/Rechazado) **y la regla que lo determinó**.
   - Número de solicitud y timeline.
8. Cierre del flujo:
   - Si fue **Aprobada**, el agente se despide con el nombre del solicitante e informa contacto y siguiente etapa (desembolso) y **retorna al inicio** de la landing. :contentReference[oaicite:0]{index=0}
   - Si fue **Rechazada**, el agente informa que un ejecutivo se contactará con alternativas y luego retorna al inicio (con confirmación). :contentReference[oaicite:1]{index=1}

**Reglas de experiencia (importantes para demo):**
- Al cargar/ejecutar la landing, el MVP debe **iniciar siempre desde el inicio**, no donde se quedó. :contentReference[oaicite:2]{index=2}
- Evitar un botón “Nueva solicitud” forzado; el cierre debe **volver naturalmente** al inicio tras despedida. :contentReference[oaicite:3]{index=3}

---

## 8. Implementación del MVP

### 8.1 Stack / tecnologías
- HTML5 + Tailwind CSS
- JavaScript (vanilla)
- (Opcional) Backend mínimo (proxy seguro) para integrar API del modelo

### 8.2 Estructura del repositorio (sugerida)
```txt
/
├─ index.html
├─ /js
│  ├─ app.js            # lógica principal (estado + UI + flujo)
│  ├─ llm.js            # callLLM() / simulador + reglas de decisión
│  └─ constants.js      # textos, prompts, configuración
├─ /assets
│  ├─ screenshots/      # capturas del MVP
│  └─ brand/            # logo/íconos
└─ README.md

---

## 9. Posibles mejoras o evoluciones (basado en “Proyecto_Mejora del prompt en el Replit.docx”)

- Reset correcto de la demo
Al ejecutar la landing debe iniciar siempre “desde cero” (evitar retomar estado anterior).

- Cierre natural del flujo (sin botón forzado “Nueva solicitud”)
  Eliminar botón de “nueva solicitud” si se percibe forzado.
  Usar despedida personal (con nombre) + mensaje de contacto + retorno automático a landing.

### Flujo de conversación con Ejecutivo
Al elegir “Hablar con ejecutivo”, simular conversación sobre su solicitud.
Si el usuario pregunta fuera de contexto, derivar a oficina principal:
Av. Canaval y Moreyra 452, San Isidro para asesoría especializada.

### Correcciones de contenido
Corregir “Suelas” por Soles en todos los mensajes.

### Orden correcto de captura de datos
Corregir el orden de campos para evitar saltos ilógicos (ej. correo → celular → ingreso mensual).

### Inactividad
Después de 2 minutos sin actividad, preguntar al usuario y luego despedirse; retornar a la landing.

### Mensajería de resultado más completa
Si rechaza, informar que un ejecutivo contactará para alternativas financieras.
Si aprueba, indicar siguiente paso (correo con día/hora de desembolso; cuenta ahorro creada; listo para app) y solicitar confirmación antes de volver a la landing.

### Regla de decisión mejorada (simulador)
Aprobar si el monto solicitado es ≤ 30% del sueldo mensual.
Rechazar si es > 30% del sueldo mensual.
Mostrar mensaje explicando la regla aplicada.

### Cierre inteligente post-aprobación
Si ya fue aprobada y el usuario solo agradece/se despide, cerrar el flujo, solicitar confirmación y volver a landing.
i fue rechazada, mantener conversación y derivar a ejecutivo si se sale de contexto.

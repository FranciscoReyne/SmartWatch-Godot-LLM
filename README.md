# SmartWatch Godot LLM

LLMs Agent based Games made in Godot for SmartWatch.

---
**ATENCIÓN !!!: ESTE PROYECTO ESTÁ EN CONSTRUCCIÓN**.-
---
Configurar Ollama en un smartwatch de forma local plantea desafíos significativos, ya que los smartwatches tienen recursos muy limitados para ejecutar modelos de lenguaje (LLMs). A continuación te explico sus limitaciones:

## Limitaciones de los smartwatches como hardware para correr LLMs de forma local.

- **RAM insuficiente**: Con ~1GB de RAM, es prácticamente imposible ejecutar Ollama directamente en el dispositivo
- **Procesador**: Los procesadores de smartwatches no tienen capacidad para inferencia de LLMs
- **Almacenamiento**: Los modelos más pequeños ocupan varios GB, excediendo el almacenamiento típico
- **Térmicos**: Los LLMs generarían sobrecalentamiento severo
- **Batería**: Agotaría la batería en minutos

----
En resumen, mi recomendación es que no sea local el uso de LLMs en el SmartWatch. 

Los modelos LLM actuales, incluso los más optimizados como DeepSeek-Lite o Phi-2, simplemente no pueden ejecutarse directamente en un smartwatch debido a las severas limitaciones de hardware. Un smartwatch típico de $100 no tiene:

1. Suficiente RAM (los modelos más pequeños requieren mínimo 2-4GB)
2. Potencia de procesamiento adecuada
3. Capacidad térmica para manejar la carga computacional
4. Suficiente batería para sostener el procesamiento intensivo

La mejor aproximación es usar una arquitectura cliente-servidor donde:

- El smartwatch actúa solo como cliente/interfaz
- El procesamiento del LLM ocurre en un servidor, PC o dispositivo más potente
- La comunicación se realiza mediante API REST o WebSockets

Esta estructura nos permitirá crear un juego en Godot para smartwatch que aparentemente utiliza IA avanzada, pero en realidad está delegando el trabajo pesado a un sistema externo más capaz.

----


## Alternativa viable

En lugar de ejecutar Ollama en el smartwatch, lo recomendable sería:

1. **Arquitectura cliente-servidor**:
   - Ejecuta Ollama en un servidor o PC
   - Usa el smartwatch solo como cliente que envía peticiones y recibe respuestas
   - Comunícate mediante API REST o WebSockets

2. **Modelos ultralivianos para el servidor**:
   - DeepSeek-Lite (1.3B) - uno de los más eficientes en relación tamaño/rendimiento
   - Phi-2 (2.7B) - muy eficiente para su tamaño
   - Gemma-2B - optimizado para dispositivos de menor potencia

3. **Cuantización agresiva**:
   - Usa modelos cuantizados a 4-bit para reducir requisitos de memoria

4. **Proxy API optimizado**:
   - Diseña un servicio intermediario que precompute respuestas comunes
   - Implementa un sistema de caché para minimizar llamadas al modelo

En cuanto al juego en Godot, debemos implementar un cliente simple para comunicarte con el servidor externo donde se ejecutaría Ollama con el modelo DeepSeek más ligero posible.

---

---

# Recursos para implementar LLM DeepSeek en Redmi 13C conectado a SmartWatch

Usaremos como ejemplo el uso de un smartphone Xiaomi Redmi 13C, que tiene especificaciones modestas (MediaTek Helio G85, 4 + 8GB RAM), lo que presenta desafíos para ejecutar LLMs. A continuación presento los recursos necesarios para hacer factible este proyecto:

## Software y herramientas base

1. **MLC-LLM** - Framework más liviano que Ollama para smartphones
   - GitHub: https://github.com/mlc-ai/mlc-llm
   - Optimizado para dispositivos móviles con recursos limitados

2. **Termux** (para Android)
   - Permite ejecutar entornos Linux en Android
   - Necesario para instalar dependencias de Python

3. **Python** (dentro de Termux)
   - Versión 3.9+ recomendada

4. **Godot 4.4**
   - Para desarrollo del juego en el smartwatch


## Modelos recomendados para un smartphone promedio (usando como referencia un Redmi 13C, ).

1. **DeepSeek-Lite-1.3B-Q4** (cuantizado a 4-bit)
   - Tamaño en disco: ~700MB
   - Requisitos RAM: ~2-3GB durante ejecución

2. **DeepSeek-Coder-Lite-1.3B-Q4** (alternativa para tareas de código)
   - Similar en requisitos al anterior

## Librerías y dependencias

1. **TensorFlow Lite** o **ONNX Runtime**
   - Para ejecutar el modelo cuantizado

2. **FastAPI** o **Flask**
   - Para crear el servidor REST local en el smartphone

3. **PyTorch (versión lite)**
   - Posiblemente necesario como dependencia

4. **Godot HTTP Plugin**
   - Para comunicación desde el smartwatch

## Comunicación smartphone-smartwatch

1. **Bluetooth LE API**
   - Para comunicación eficiente entre dispositivos

2. **WebSockets** (alternativa si ambos están en la misma red WiFi)
   - Más rápido pero consume más batería

## Optimizaciones necesarias

1. **Cache de respuestas**
   - Implementar almacenamiento local de respuestas comunes

2. **Prompt trimming**
   - Reducir longitud de prompts al mínimo necesario

3. **Cuantización dinámica**
   - Ajustar precisión según disponibilidad de memoria

## Alternativa a Ollama !!

Para evitar Ollama, puedes usar MLC-LLM que es más eficiente o implementar directamente:

```
1. llama.cpp + Python bindings
2. DeepSeek-Lite-1.3B modelo precompilado para ARM
3. Servidor REST simple con Flask
```

Esta configuración consumirá menos recursos que Ollama mientras mantiene la capacidad de ejecutar DeepSeek en formato cuantizado.


----

----


## SmartWatch recomendados.
Para este proyecto específico, recomendaría el **Amazfit GTS 2e o el Amazfit GTR 2e**. Estos smartwatches ofrecen un buen equilibrio entre precio y características que beneficiarían al proyecto:

**Ventajas para el proyecto:**
- Precio accesible (alrededor de $80-100 USD)
- Pantalla AMOLED de buena calidad y tamaño razonable
- Buena duración de batería
- Conectividad Bluetooth estable
- Sistema operativo Zepp OS que permite algunas aplicaciones personalizadas
- Sensores básicos que podrías integrar en tu juego

Otra opción económica pero funcional sería el **Xiaomi Mi Band 7 Pro** o el **Xiaomi Smart Band 8 Pro** (~100 U$D), que aunque técnicamente son smart bands, tienen **pantallas suficientemente grandes** para un juego simple y se integrarían bien con el Redmi 13C, manteniendo todo dentro del ecosistema Xiaomi para mejor compatibilidad.

Si puedes estirarte un poco más en presupuesto, el **Xiaomi Redmi Watch 3 Active** (~120 U$D) sería ideal ya que tendras mejor compatibilidad con un smartphone como el **Redmi 13C** y podría facilitar la comunicación entre dispositivos.

----

### Comparación rápida de carácteristicas para guiar lo que estés buscando en un smartwatch o smartband. :  

| **Característica**          | **Xiaomi Smart Band 7 Pro** | **Xiaomi Smart Band 8** | **Xiaomi Redmi Watch 3 Active** |
|----------------------------|----------------------------|-------------------------|---------------------------------|
| **Pantalla**               | AMOLED 1.64”               | AMOLED 1.62”            | TFT LCD 1.83”                  |
| **Resolución**             | 280 × 456 px               | 192 × 490 px            | 240 × 280 px                   |
| **GPS Integrado**         | ✅ Sí                       | ❌ No                    | ✅ Sí                           |
| **Modos deportivos**       | 117+                        | 150+                     | 100+                            |
| **Resistencia al agua**    | 5 ATM                      | 5 ATM                    | 5 ATM                           |
| **Duración de batería**    | 12 días aprox.             | 16 días aprox.           | 12 días aprox.                  |
| **Monitorización de salud**| FC, SpO2, Sueño, Estrés    | FC, SpO2, Sueño, Estrés  | FC, SpO2, Sueño, Estrés         |
| **Llamadas Bluetooth**     | ❌ No                      | ❌ No                    | ✅ Sí                           |
| **Diseño**                 | Rectangular premium        | Banda delgada            | Reloj más tradicional          |

### **¿Cuál elegir?**

Recomendaciones:
- **Si quieres GPS integrado y pantalla premium:** *Xiaomi Smart Band 7 Pro*  
- **Si buscas la mejor smartband para deporte y batería:** *Xiaomi Smart Band 8*  
- **Si prefieres un smartwatch con llamadas Bluetooth y diseño más de reloj:** *Redmi Watch 3 Active*  

**Para deporte y funcionalidad avanzada, el Smart Band 7 Pro es la mejor opción.** Pero si buscas algo más económico y con diseño de reloj, el *Redmi Watch 3 Active* es una buena alternativa (es el que elegí yo).


----


El **Xiaomi Smart Band 7 Pro** es mejor que el **Redmi Watch 3 Active** en varios aspectos clave, especialmente si priorizas **pantalla, GPS y sensores de salud**. Aquí te explico por qué:  

### **1️⃣ Pantalla AMOLED vs. TFT LCD**
- **Band 7 Pro** tiene una pantalla **AMOLED de 1.64”** con mejor calidad de imagen, colores más vivos y mejor visibilidad en exteriores.  
- **Watch 3 Active** usa una pantalla **TFT LCD de 1.83”**, que es más grande, pero de menor calidad en contraste y brillo.  

### **2️⃣ GPS Integrado**
- **Band 7 Pro** tiene **GPS integrado**, lo que significa que puedes salir a correr sin llevar tu teléfono y seguirás teniendo un registro preciso de tu ruta.  
- **Watch 3 Active** **también tiene GPS**, pero no es tan preciso y confiable como el del Band 7 Pro.  

### **3️⃣ Sensores de salud y deporte**
- **Band 7 Pro** tiene **más modos deportivos** y mejores algoritmos para el seguimiento de actividades físicas.  
- Ambos tienen monitoreo de **frecuencia cardíaca, oxígeno en sangre (SpO2), sueño y estrés**, pero el Band 7 Pro suele ser más preciso en sus mediciones.  

### **4️⃣ Diseño y Materiales**
- **Band 7 Pro** tiene un diseño más **premium y compacto**, con un marco metálico que lo hace ver más elegante.  
- **Watch 3 Active** tiene un diseño más de **reloj clásico**, pero con un acabado más básico y una sensación más plástica.  

### **📌 Entonces, ¿cuál elegir?**
✔️ **Si quieres una pantalla de mejor calidad, GPS confiable y más precisión en salud/deporte → Xiaomi Smart Band 7 Pro.**  
✔️ **Si prefieres un diseño más de smartwatch, con pantalla más grande y llamadas Bluetooth → Redmi Watch 3 Active.**  

💡 **El Band 7 Pro es mejor en funcionalidad y sensores, pero el Watch 3 Active es más útil si necesitas recibir llamadas desde la muñeca.**

---

Suerte !!

Te Saluda,
Francisco Reyne

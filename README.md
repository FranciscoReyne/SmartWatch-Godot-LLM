# SmartWatch Godot LLM
LLMs Games made in Godot for SmartWatch.
---
**ATENCIÓN: ESTE PROYECTO ESTÁ EN CONSTRUCCIÓN**.-
---
Configurar Ollama en un smartwatch de forma local plantea desafíos significativos, ya que los smartwatches tienen recursos muy limitados para ejecutar modelos de lenguaje (LLMs). A continuación te explico sus limitaciones:

## Limitaciones de los smartwatches como hardware para correr LLMs de forma local.

- **RAM insuficiente**: Con ~1GB de RAM, es prácticamente imposible ejecutar Ollama directamente en el dispositivo
- **Procesador**: Los procesadores de smartwatches no tienen capacidad para inferencia de LLMs
- **Almacenamiento**: Los modelos más pequeños ocupan varios GB, excediendo el almacenamiento típico
- **Térmicos**: Los LLMs generarían sobrecalentamiento severo
- **Batería**: Agotaría la batería en minutos

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

# Recursos para implementar LLM DeepSeek en Redmi 13C conectado a smartwatch

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
Para este proyecto específico, recomendaría el Amazfit GTS 2e o el Amazfit GTR 2e. Estos smartwatches ofrecen un buen equilibrio entre precio y características que beneficiarían al proyecto:

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
----

Suerte !!

Te Saluda,
Francisco Reyne

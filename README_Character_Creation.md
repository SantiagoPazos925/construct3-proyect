# Sistema de Creación de Personaje - Construct 3

## Descripción General

Este sistema permite a los jugadores crear un personaje personalizado con:
- **Nombre personalizable**
- **Selección de género** (Masculino/Femenino)
- **5 atributos principales** con sistema de puntos
- **Validación** para asegurar que todos los puntos sean distribuidos
- **Integración directa** con el sistema de combate usando variables de instancia

## Arquitectura del Sistema

### Variables Globales Permanentes

#### Información del Personaje
- `playerName` (string): Nombre del personaje
- `playerGender` (string): Género (Masculino/Femenino)

#### Sistema de Puntos
- `availablePoints` (number): Puntos disponibles para distribuir
- `minAttributeValue` (number): Valor mínimo para cualquier atributo (1)
- `maxAttributeValue` (number): Valor máximo para cualquier atributo (10)

### Variables Temporales para Transferencia
- `tempStrength` (number): Valor temporal de fuerza para transferir al combate
- `tempVitality` (number): Valor temporal de vitalidad para transferir al combate
- `tempAgility` (number): Valor temporal de agilidad para transferir al combate
- `tempSpeed` (number): Valor temporal de velocidad para transferir al combate
- `tempDefense` (number): Valor temporal de defensa para transferir al combate

### Variables de Instancia (PlayerCombat)
- `Strength` (number): Fuerza (afecta el daño de ataque)
- `Vitality` (number): Vitalidad (afecta la vida máxima)
- `Agility` (number): Agilidad (afecta la probabilidad de esquiva)
- `Speed` (number): Velocidad (afecta la velocidad de movimiento)
- `Defense` (number): Defensa (reduce el daño recibido)

## Implementación en el Layout

### 1. Elementos de UI Necesarios

#### Campo de Nombre
- **Text Input**: `PlayerNameInput`
- **Label**: "Nombre del Personaje:"

#### Selector de Género
- **Button**: `GenderToggleButton` (con texto "Cambiar Género")
- **Text**: `GenderDisplayText` (muestra el género actual)

#### Contador de Puntos
- **Text**: `AvailablePointsText` (muestra "Puntos disponibles: X")

#### Atributos (para cada uno):
- **Label**: Nombre del atributo
- **Text**: `[Atributo]ValueText` (muestra el valor actual)
- **Button**: `[Atributo]PlusButton` (botón "+")
- **Button**: `[Atributo]MinusButton` (botón "-")

#### Botones de Control
- **Button**: `ResetButton` (texto "Reiniciar")
- **Button**: `ConfirmButton` (texto "Confirmar Personaje")

### 2. Posicionamiento Recomendado

```
┌─────────────────────────────────────────────────────────┐
│                    CREACIÓN DE PERSONAJE                 │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  Nombre del Personaje: [________________]              │
│                                                         │
│  Género: [Masculino] [Cambiar Género]                 │
│                                                         │
│  Puntos disponibles: 20                                │
│                                                         │
│  ┌─────────────┬─────────┬─────────┬─────────┐         │
│  │   Fuerza   │    5    │   [+]   │   [-]   │         │
│  ├─────────────┼─────────┼─────────┼─────────┤         │
│  │  Vitalidad │    5    │   [+]   │   [-]   │         │
│  ├─────────────┼─────────┼─────────┼─────────┤         │
│  │  Agilidad  │    5    │   [+]   │   [-]   │         │
│  ├─────────────┼─────────┼─────────┼─────────┤         │
│  │  Velocidad │    5    │   [+]   │   [-]   │         │
│  ├─────────────┼─────────┼─────────┼─────────┤         │
│  │  Defensa   │    5    │   [+]   │   [-]   │         │
│  └─────────────┴─────────┴─────────┴─────────┘         │
│                                                         │
│  [Reiniciar]                    [Confirmar Personaje]  │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 3. Configuración de Eventos

El archivo `CharacterCreationEvents.json` contiene todos los eventos necesarios:

- **Inicialización**: Configura valores por defecto al cargar el layout
- **Entrada de nombre**: Actualiza la variable `playerName`
- **Cambio de género**: Alterna entre Masculino y Femenino
- **Aumentar/Disminuir atributos**: Con validaciones de puntos disponibles
- **Reiniciar**: Restaura todos los valores por defecto
- **Confirmar**: Valida que el nombre esté establecido y todos los puntos distribuidos

### 4. Validaciones del Sistema

- **Puntos mínimos**: Cada atributo debe tener al menos 1 punto
- **Puntos máximos**: Cada atributo puede tener máximo 10 puntos
- **Distribución completa**: Deben usarse todos los 20 puntos disponibles
- **Nombre requerido**: El personaje debe tener un nombre antes de confirmar

### 5. Flujo de Creación

1. **Inicio**: Se cargan valores por defecto (5 en cada atributo, 20 puntos disponibles)
2. **Personalización**: El jugador modifica nombre, género y distribuye puntos
3. **Validación**: El sistema verifica que se cumplan todas las reglas
4. **Confirmación**: Al presionar "Confirmar Personaje":
   - Se guardan los valores de `PlayerCombat` en las variables temporales globales
   - Se navega al layout de combate

## Integración con el Sistema de Combate

### Transferencia de Atributos

El sistema utiliza un mecanismo de puente para transferir atributos entre layouts:

1. **En CreateCharacter**: Los atributos se modifican directamente en `PlayerCombat.Strength`, `PlayerCombat.Vitality`, etc.
2. **Al confirmar**: Los valores se copian a las variables temporales globales (`tempStrength`, `tempVitality`, etc.)
3. **En Combat**: Al inicio del layout, las variables temporales se leen y se asignan a `PlayerCombat.Strength`, `PlayerCombat.Vitality`, etc.

### Efectos en Combate

Los atributos del personaje afectan directamente las estadísticas de combate:

- **Fuerza**: Aumenta el daño de ataque (`playerAttack`)
- **Vitalidad**: Aumenta la vida máxima (`currentHP`)
- **Agilidad**: Mejora la probabilidad de esquiva
- **Velocidad**: Afecta la velocidad de movimiento
- **Defensa**: Reduce el daño recibido

## Importación de Elementos UI

### Opción A: Elementos Individuales (Recomendado)

Cada elemento UI está disponible como un archivo JSON separado que puede importarse directamente en Construct 3:

1. Ve a **Project → Project Settings → Object Types**
2. Haz clic en **Import** y selecciona el archivo JSON correspondiente
3. Los archivos disponibles incluyen:
   - `AvailablePointsText.json` - Texto para mostrar puntos disponibles
   - `TitleText.json` - Título del layout
   - `NameLabelText.json` - Etiqueta del nombre
   - `GenderLabelText.json` - Etiqueta del género
   - `StrengthLabelText.json` - Etiqueta de fuerza
   - `StrengthValueText.json` - Valor de fuerza
   - `StrengthPlusButton.json` - Botón + de fuerza
   - `StrengthMinusButton.json` - Botón - de fuerza
   - `VitalityLabelText.json` - Etiqueta de vitalidad
   - `VitalityValueText.json` - Valor de vitalidad
   - `VitalityPlusButton.json` - Botón + de vitalidad
   - `VitalityMinusButton.json` - Botón - de vitalidad
   - `AgilityLabelText.json` - Etiqueta de agilidad
   - `AgilityValueText.json` - Valor de agilidad
   - `AgilityPlusButton.json` - Botón + de agilidad
   - `AgilityMinusButton.json` - Botón - de agilidad
   - `SpeedLabelText.json` - Etiqueta de velocidad
   - `SpeedValueText.json` - Valor de velocidad
   - `SpeedPlusButton.json` - Botón + de velocidad
   - `SpeedMinusButton.json` - Botón - de velocidad
   - `DefenseLabelText.json` - Etiqueta de defensa
   - `DefenseValueText.json` - Valor de defensa
   - `DefensePlusButton.json` - Botón + de defensa
   - `DefenseMinusButton.json` - Botón - de defensa
   - `ResetButton.json` - Botón de reiniciar
   - `ConfirmButton.json` - Botón de confirmar

### Opción B: Creación Manual

Si prefieres crear los elementos manualmente, sigue el patrón tradicional descrito en la sección de implementación.

## Personalización Adicional

### Colores y Estilos
- Usar colores consistentes con el tema del juego
- Botones con estados visuales (normal, hover, presionado)
- Texto legible con buen contraste

### Animaciones
- Transiciones suaves al cambiar valores
- Efectos visuales al confirmar el personaje
- Feedback visual al presionar botones

### Sonidos
- Efectos de sonido al cambiar atributos
- Sonido de confirmación al crear el personaje
- Música de fondo apropiada

## Solución de Problemas

### Puntos no se actualizan
- Verificar que los eventos estén correctamente configurados
- Comprobar que los nombres de los objetos coincidan
- Asegurar que haya una instancia de `PlayerCombat` en el layout

### No puede confirmar el personaje
- Asegurar que se haya ingresado un nombre
- Verificar que todos los puntos estén distribuidos (`availablePoints = 0`)

### Valores de atributos fuera de rango
- Los atributos deben estar entre 1 y 10
- El sistema automáticamente aplica estos límites

### Los atributos no se transfieren al combate
- Verificar que las variables temporales globales se estén actualizando en `CharacterCreationEvents.json`
- Asegurar que `CombatEvents.json` esté leyendo las variables temporales al inicio del layout
- Verificar que no haya errores en la consola durante la transición

## Ventajas de la Nueva Arquitectura

1. **Eficiencia**: Las variables de instancia son más eficientes que las variables globales
2. **Consistencia**: Un solo lugar para los datos del personaje
3. **Mantenibilidad**: Más fácil de mantener y debuggear
4. **Escalabilidad**: Preparado para múltiples personajes
5. **Integración**: Conexión directa con el sistema de combate
6. **Transferencia**: Sistema robusto de transferencia de atributos entre layouts

## Notas de Implementación

- Este sistema está diseñado para ser modular y fácil de expandir
- Se pueden agregar más atributos modificando las variables de instancia y eventos
- El sistema de puntos es flexible y se puede ajustar la cantidad inicial
- Todos los eventos usan la sintaxis estándar de Construct 3
- **IMPORTANTE**: Debe haber una instancia de `PlayerCombat` en el layout de creación de personaje
- El sistema de transferencia de atributos es robusto y maneja correctamente la comunicación entre layouts

# Implementación Paso a Paso - Sistema de Creación de Personaje

## ⚠️ IMPORTANTE: Cambio en la Arquitectura

**Se ha corregido un error de diseño importante:** En lugar de usar variables globales para los atributos del personaje, ahora se usan directamente las **variables de instancia de `PlayerCombat`**.

### ¿Por qué este cambio?

1. **Eficiencia**: Las variables de instancia son más eficientes que las variables globales
2. **Consistencia**: El sistema de combate ya usa `PlayerCombat.Strength`, `PlayerCombat.Vitality`, etc.
3. **Mantenibilidad**: Un solo lugar para los datos del personaje
4. **Escalabilidad**: Más fácil agregar múltiples personajes en el futuro

### Variables que se mantienen como globales:
- `playerName`: Nombre del personaje
- `playerGender`: Género del personaje  
- `availablePoints`: Puntos disponibles para distribuir
- `minAttributeValue` y `maxAttributeValue`: Límites de atributos

### Variables temporales para transferencia entre layouts:
- `tempStrength`, `tempVitality`, `tempAgility`, `tempSpeed`, `tempDefense`: Actúan como puente para transferir atributos del layout `CreateCharacter` al layout `Combat`

### Variables que ahora son de instancia (PlayerCombat):
- `Strength`, `Vitality`, `Agility`, `Speed`, `Defense`

## Paso 1: Importar las Variables Globales

1. Abre tu proyecto en Construct 3
2. Ve a **Project → Project Settings → Variables**
3. Haz clic en **Import** y selecciona el archivo `eventSheets/GlobalVariables.json`
4. Verifica que se hayan agregado las variables globales necesarias:
   - Variables del personaje: `playerName`, `playerGender`
   - Variables del sistema: `availablePoints`, `minAttributeValue`, `maxAttributeValue`
   - Variables temporales: `tempStrength`, `tempVitality`, `tempAgility`, `tempSpeed`, `tempDefense`

## Paso 2: Crear el Layout de Creación de Personaje

### 2.1 Crear el Layout
1. Haz clic derecho en **Layouts** en el panel de Project
2. Selecciona **Add Layout**
3. Nombra el layout como `CreateCharacter`

### 2.2 Agregar Elementos de UI

**IMPORTANTE**: Ahora puedes importar cada elemento UI como un tipo de objeto individual desde los archivos JSON proporcionados.

#### Opción A: Importar Elementos Individuales (Recomendado)
1. Para cada elemento UI, ve a **Project → Project Settings → Object Types**
2. Haz clic en **Import** y selecciona el archivo JSON correspondiente:
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

#### Opción B: Crear Manualmente (Alternativa)
Si prefieres crear los elementos manualmente, sigue el patrón tradicional:

##### Campo de Nombre
1. Arrastra un **Text Input** al layout
2. Nómbralo como `PlayerNameInput`
3. Posiciónalo en la parte superior (ej: X: 400, Y: 100)
4. Configura el texto inicial como "Ingresa tu nombre"
5. Ajusta el tamaño a aproximadamente 200x30

##### Selector de Género
1. Arrastra un **Text** al layout
2. Nómbralo como `GenderDisplayText`
3. Posiciónalo debajo del nombre (ej: X: 400, Y: 150)
4. Configura el texto inicial como "Masculino"
5. Arrastra un **Button** al layout
6. Nómbralo como `GenderToggleButton`
7. Posiciónalo al lado del texto de género (ej: X: 500, Y: 150)
8. Configura el texto del botón como "Cambiar Género"

##### Contador de Puntos
1. Arrastra un **Text** al layout
2. Nómbralo como `AvailablePointsText`
3. Posiciónalo debajo del género (ej: X: 400, Y: 200)
4. Configura el texto inicial como "Puntos disponibles: 20"

### 2.3 Crear la Sección de Atributos

Para cada atributo (Fuerza, Vitalidad, Agilidad, Velocidad, Defensa), sigue este patrón:

#### Fuerza (Strength)
1. **Label**: Arrastra un **Text** y nómbralo como `StrengthLabel`
   - Texto: "Fuerza"
   - Posición: X: 300, Y: 250
2. **Valor**: Arrastra un **Text** y nómbralo como `StrengthValueText`
   - Texto: "5"
   - Posición: X: 400, Y: 250
3. **Botón +**: Arrastra un **Button** y nómbralo como `StrengthPlusButton`
   - Texto: "+"
   - Posición: X: 450, Y: 250
   - Tamaño: 30x30
4. **Botón -**: Arrastra un **Button** y nómbralo como `StrengthMinusButton`
   - Texto: "-"
   - Posición: X: 500, Y: 250
   - Tamaño: 30x30

#### Repite para los otros atributos:
- **Vitalidad**: Y: 290, nombres: `VitalityLabel`, `VitalityValueText`, `VitalityPlusButton`, `VitalityMinusButton`
- **Agilidad**: Y: 330, nombres: `AgilityLabel`, `AgilityValueText`, `AgilityPlusButton`, `AgilityMinusButton`
- **Velocidad**: Y: 370, nombres: `SpeedLabel`, `SpeedValueText`, `SpeedPlusButton`, `SpeedMinusButton`
- **Defensa**: Y: 410, nombres: `DefenseLabel`, `DefenseValueText`, `DefensePlusButton`, `DefenseMinusButton`

### 2.4 Botones de Control
1. **Botón Reiniciar**: Arrastra un **Button** y nómbralo como `ResetButton`
   - Texto: "Reiniciar"
   - Posición: X: 300, Y: 500
   - Tamaño: 100x40
2. **Botón Confirmar**: Arrastra un **Button** y nómbralo como `ConfirmButton`
   - Texto: "Confirmar Personaje"
   - Posición: X: 500, Y: 500
   - Tamaño: 150x40

### 2.5 Agregar el Objeto PlayerCombat
**IMPORTANTE**: Necesitas agregar una instancia de `PlayerCombat` al layout de creación de personaje para que las variables de instancia estén disponibles.

1. Arrastra un **PlayerCombat** al layout
2. Posiciónalo fuera de la pantalla (ej: X: -100, Y: -100) para que no sea visible
3. **NO** lo elimines - es necesario para que funcionen las variables de instancia

## Paso 3: Importar los Eventos

### 3.1 Eventos de Creación de Personaje
1. En el layout `CreateCharacter`, ve a **Events**
2. Haz clic en **Import** y selecciona `eventSheets/CharacterCreationEvents.json`
3. Verifica que se hayan importado todos los eventos

### 3.2 Eventos de Combate
1. En el layout `Combat`, ve a **Events**
2. Haz clic en **Import** y selecciona `eventSheets/CombatEvents.json`
3. Verifica que se hayan importado los eventos de combate

## Paso 4: Configurar la Navegación

### 4.1 Desde el Menú Principal
1. En tu layout principal o menú, agrega un botón "Crear Personaje"
2. Configura la acción: **System → Go to layout → CreateCharacter**

### 4.2 Desde la Creación de Personaje
1. El botón "Confirmar Personaje" ya está configurado para:
   - Guardar los valores de `PlayerCombat` en las variables temporales globales
   - Ir al layout `Combat`
2. Verifica que el nombre del layout sea correcto en tu proyecto

## Paso 5: Personalizar la Apariencia

### 5.1 Colores y Estilos
1. Selecciona todos los botones y aplica un estilo consistente
2. Usa colores que coincidan con tu tema de juego
3. Asegúrate de que el texto sea legible

### 5.2 Fuentes y Tamaños
1. Selecciona todos los textos y aplica una fuente consistente
2. Ajusta los tamaños para mejor legibilidad
3. Usa diferentes tamaños para títulos y valores

### 5.3 Posicionamiento Final
1. Ajusta las posiciones X e Y de todos los elementos
2. Asegúrate de que todo esté centrado y alineado
3. Prueba diferentes resoluciones para verificar que se vea bien

## Paso 6: Probar el Sistema

### 6.1 Funcionalidad Básica
1. Ejecuta el proyecto
2. Ve al layout de creación de personaje
3. Verifica que:
   - Se muestren los valores iniciales correctos
   - Los botones + y - funcionen para cada atributo
   - El contador de puntos se actualice
   - El botón de género cambie entre Masculino y Femenino

### 6.2 Validaciones
1. Intenta confirmar sin nombre → Debe fallar
2. Intenta confirmar con puntos sin distribuir → Debe fallar
3. Distribuye todos los puntos y confirma → Debe funcionar

### 6.3 Integración con Combate
1. Crea un personaje y confírmalo
2. Verifica que las variables temporales globales se hayan actualizado:
   - `tempStrength`, `tempVitality`, `tempAgility`, `tempSpeed`, `tempDefense`
3. Ve al layout de combate
4. Verifica que las estadísticas se hayan aplicado correctamente
5. **IMPORTANTE**: Verifica que `PlayerCombat.Strength`, `PlayerCombat.Vitality`, etc. tengan los valores correctos

## Paso 7: Ajustes y Optimizaciones

### 7.1 Balance de Juego
1. Ajusta las fórmulas de atributos en `CombatEvents.json`
2. Prueba diferentes distribuciones de puntos
3. Asegúrate de que ninguna combinación sea demasiado poderosa

### 7.2 Feedback Visual
1. Agrega efectos de sonido al cambiar atributos
2. Agrega animaciones al confirmar el personaje
3. Agrega indicadores visuales cuando no se puede confirmar

### 7.3 Guardado de Datos
1. Considera agregar persistencia de datos
2. Implementa un sistema de múltiples personajes
3. Agrega opciones de importar/exportar personajes

## Solución de Problemas Comunes

### Los botones no funcionan
- Verifica que los nombres de los objetos coincidan exactamente con los eventos
- Asegúrate de que los eventos estén importados correctamente
- Verifica que no haya conflictos con otros eventos
- **IMPORTANTE**: Verifica que haya una instancia de `PlayerCombat` en el layout

### Los valores no se actualizan
- Verifica que las variables globales estén configuradas correctamente
- Asegúrate de que los eventos se ejecuten en el orden correcto
- Verifica que no haya errores en la consola
- **IMPORTANTE**: Verifica que las variables de instancia de `PlayerCombat` se estén modificando

### No puede confirmar el personaje
- Verifica que se haya ingresado un nombre
- Asegúrate de que todos los puntos estén distribuidos
- Verifica que las condiciones de los eventos sean correctas

### Error: "PlayerCombat.Strength is not defined"
- **SOLUCIÓN**: Asegúrate de que haya una instancia de `PlayerCombat` en el layout de creación de personaje
- Verifica que el objeto `PlayerCombat` tenga las variables de instancia configuradas correctamente

### Los atributos no se transfieren al combate
- Verifica que las variables temporales globales se estén actualizando en `CharacterCreationEvents.json`
- Asegúrate de que `CombatEvents.json` esté leyendo las variables temporales al inicio del layout
- Verifica que no haya errores en la consola durante la transición

## Notas Importantes

- **Nombres de objetos**: Deben coincidir exactamente con los especificados en los eventos
- **Variables**: Solo las variables globales mencionadas arriba son necesarias
- **Layouts**: Verifica que los nombres de los layouts coincidan con los eventos
- **Orden de importación**: Importa primero las variables, luego los eventos
- **PlayerCombat**: **DEBE** estar presente en el layout de creación de personaje
- **Transferencia de atributos**: El sistema usa variables temporales globales como puente entre layouts

## Ventajas de la Nueva Arquitectura

1. **Eficiencia**: Menos variables globales permanentes = mejor rendimiento
2. **Consistencia**: Un solo lugar para los datos del personaje
3. **Mantenibilidad**: Más fácil de mantener y debuggear
4. **Escalabilidad**: Preparado para múltiples personajes
5. **Integración**: Conexión directa con el sistema de combate
6. **Transferencia**: Sistema robusto de transferencia de atributos entre layouts

## Próximos Pasos

Una vez que el sistema básico esté funcionando, puedes considerar:

1. **Agregar más atributos** (Inteligencia, Carisma, etc.)
2. **Sistema de clases** (Guerrero, Mago, Arquero)
3. **Equipamiento** que modifique los atributos
4. **Progresión** de personaje con experiencia
5. **Múltiples personajes** guardados
6. **Importar/Exportar** personajes

¡Con estos pasos tendrás un sistema completo de creación de personaje funcionando correctamente en tu proyecto de Construct 3!

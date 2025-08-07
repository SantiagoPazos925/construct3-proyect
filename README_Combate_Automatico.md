# Sistema de Combate Automático - Construct 3

## 📋 Variables Globales Requeridas

### **Variables Principales del Combate:**

| Variable | Tipo | Valor Inicial | Descripción |
|----------|------|---------------|-------------|
| `combatState` | String | "playerTurn" | Estado del combate |
| `playerHP` | Number | 100 | Vida del jugador |
| `enemyHP` | Number | 100 | Vida del enemigo |
| `playerAttack` | Number | 25 | Daño del jugador |
| `enemyAttack` | Number | 20 | Daño del enemigo |
| `isAttacking` | Boolean | false | Control de ataque |
| `attackCooldown` | Number | 0 | Cooldown entre ataques |

### **Variables de Posición:**

| Variable | Tipo | Valor Inicial | Descripción |
|----------|------|---------------|-------------|
| `targetX` | Number | 0 | Posición X objetivo |
| `targetY` | Number | 0 | Posición Y objetivo |
| `originalPlayerX` | Number | 100 | Posición X inicial jugador |
| `originalPlayerY` | Number | 300 | Posición Y inicial jugador |
| `originalEnemyX` | Number | 700 | Posición X inicial enemigo |
| `originalEnemyY` | Number | 300 | Posición Y inicial enemigo |

### **Variables de Configuración:**

| Variable | Tipo | Valor Inicial | Descripción |
|----------|------|---------------|-------------|
| `attackDistance` | Number | 50 | Distancia para atacar |
| `returnDistance` | Number | 10 | Distancia para regresar |
| `movementSpeed` | Number | 0.1 | Velocidad de movimiento |
| `cooldownDuration` | Number | 120 | Duración del cooldown |

## 🛠️ Configuración en Construct 3

### **Paso 1: Crear Variables Globales**

**Opción A: Copiar y Pegar (Recomendado)**
1. Abre tu proyecto en Construct 3
2. Ve a **Project > Project Properties**
3. Haz clic en la pestaña **"Variables"**
4. Copia el contenido del archivo `construct3_variables.json`
5. Pega directamente en la sección de variables globales
6. Guarda el proyecto

**Opción B: Crear Manualmente**
1. Abre tu proyecto en Construct 3
2. Ve a **Project > Project Properties**
3. Haz clic en la pestaña **"Variables"**
4. Agrega cada variable de la lista anterior:
   - Haz clic en **"Add Variable"**
   - Establece el **nombre** exacto
   - Selecciona el **tipo** correcto (number, boolean, string)
   - Establece el **valor inicial**
   - Marca la casilla **"Global"**
5. Guarda el proyecto

### **Paso 2: Configurar Sprites**

#### **PlayerCombat:**
- **Posición inicial:** X=100, Y=300
- **Behaviors recomendados:**
  - Bound (para mantener en pantalla)
  - Solid (opcional, para colisiones)

#### **EnemyCombat:**
- **Posición inicial:** X=700, Y=300
- **Behaviors recomendados:**
  - Bound (para mantener en pantalla)
  - Solid (opcional, para colisiones)

### **Paso 3: Importar Eventos**

1. Ve a **Event Sheets**
2. Selecciona **"CombatEvents"**
3. Importa el contenido del archivo `CombatEvents.json`
4. Verifica que todos los eventos se hayan importado correctamente

## 🎮 Estados del Combate

### **Estados Principales:**
- `playerTurn` - Turno del jugador
- `enemyTurn` - Turno del enemigo
- `playerAttacking` - Jugador atacando
- `enemyAttacking` - Enemigo atacando
- `playerReturning` - Jugador regresando
- `enemyReturning` - Enemigo regresando
- `victory` - Victoria del jugador
- `defeat` - Derrota del jugador

## 🔧 Personalización

### **Ajustar Velocidad de Combate:**
- Modifica `cooldownDuration` (frames entre ataques)
- Modifica `movementSpeed` (velocidad de movimiento)

### **Ajustar Balance:**
- Modifica `playerAttack` y `enemyAttack` (daño)
- Modifica `playerHP` y `enemyHP` (vida inicial)

### **Ajustar Distancias:**
- Modifica `attackDistance` (distancia para atacar)
- Modifica `returnDistance` (distancia para regresar)

## 🎨 Efectos Visuales Opcionales

### **Para Mejorar la Experiencia Visual:**

#### **Efectos de Ataque:**
- **Tint:** Cambiar color durante el ataque
- **Glow:** Efecto de brillo al atacar
- **Shake:** Vibración al recibir daño

#### **UI de Combate:**
- **Textos para mostrar:**
  - `playerHP` - Vida del jugador
  - `enemyHP` - Vida del enemigo
  - `combatState` - Estado actual

## 🚀 Funcionamiento

### **Flujo del Combate:**
1. **Inicio:** Se establecen variables iniciales
2. **Cooldown:** Sistema de espera entre ataques
3. **Turnos:** Alternancia automática entre jugador y enemigo
4. **Movimiento:** Los sprites se mueven hacia el objetivo
5. **Ataque:** Cuando están cerca, se aplica el daño
6. **Retorno:** Los sprites regresan a su posición original
7. **Muerte:** Si la vida llega a 0, el sprite se destruye

### **Características:**
✅ **Combate automático** - No requiere input del usuario
✅ **Movimiento suave** - Usa interpolación para movimiento natural
✅ **Sistema de turnos** - Alternancia automática
✅ **Cooldown de ataques** - Evita spam de ataques
✅ **Detección de muerte** - Termina el combate cuando alguien muere
✅ **Posiciones fijas** - Los combatientes regresan a su posición inicial
✅ **Sistema de esquiva** - El jugador puede esquivar ataques basándose en su agilidad y velocidad

## 🛡️ Sistema de Esquiva

### **Descripción:**
El sistema de esquiva permite que el jugador evite recibir daño de los ataques del enemigo basándose en su atributo `DodgeChance`.

### **Cálculo de Esquiva:**
```
DodgeChance = Agility * 1.5 + Speed * 0.8
```

### **Funcionamiento:**
1. **Cuando el enemigo ataca:** Se genera un valor aleatorio entre 0 y 100
2. **Comparación:** Si el valor aleatorio es menor o igual al `DodgeChance` del jugador, el ataque se esquiva
3. **Resultado:** 
   - **Esquiva exitosa:** Muestra "ESQUIVADO!" y no recibe daño
   - **Esquiva fallida:** Recibe el daño normal del enemigo

### **Variables Relacionadas:**
- `DodgeChance` - Probabilidad de esquiva (calculada automáticamente)
- `randomDodgeValue` - Valor aleatorio generado para el cálculo
- `playerDodged` - Indica si el jugador esquivó exitosamente

### **Personalización:**
- **Ajustar fórmula:** Modifica la fórmula en la inicialización del sistema
- **Ajustar atributos:** Cambia los valores de `Agility` y `Speed` del PlayerCombat
- **Ajustar multiplicadores:** Modifica los multiplicadores (1.5 y 0.8) en la fórmula

### **Ejemplo de Configuración:**
- **Agility = 10, Speed = 5:** DodgeChance = 10 * 1.5 + 5 * 0.8 = 19%
- **Agility = 20, Speed = 15:** DodgeChance = 20 * 1.5 + 15 * 0.8 = 42%

## 📝 Notas Importantes

- **FPS:** El sistema está configurado para 60 FPS
- **Posiciones:** Ajusta las posiciones iniciales según tu layout
- **Sprites:** Asegúrate de que los sprites tengan los nombres exactos: `PlayerCombat` y `EnemyCombat`
- **Variables:** Todas las variables deben ser globales para funcionar correctamente

## 🔍 Solución de Problemas

### **Si el combate no inicia:**
- Verifica que todas las variables globales estén creadas
- Asegúrate de que los sprites tengan los nombres correctos
- Verifica que los eventos estén importados correctamente

### **Si el movimiento no funciona:**
- Verifica las posiciones iniciales de los sprites
- Ajusta `movementSpeed` si es necesario
- Verifica que no haya colisiones bloqueando el movimiento

### **Si el daño no se aplica:**
- Verifica que `attackDistance` sea apropiado
- Asegúrate de que las variables de daño tengan valores mayores a 0
- Verifica que las variables de vida se estén actualizando correctamente 
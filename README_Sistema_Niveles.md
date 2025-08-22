# 🎮 Sistema de Niveles - Construct 3

## 📋 **Descripción General**

Sistema completo de experiencia y niveles para juegos RPG en Construct 3. Permite a los jugadores ganar experiencia al derrotar enemigos, subir de nivel y distribuir puntos de estadística de manera controlada.

## 🚀 **Características Principales**

- ✅ **Sistema de experiencia progresivo** con escalado automático
- ✅ **Distribución de puntos de estadística** con protección de valores confirmados
- ✅ **Barra de experiencia visual** con animaciones suaves
- ✅ **Sistema de validación** que previene modificaciones no autorizadas
- ✅ **Integración completa** con el sistema de combate
- ✅ **UI de notificaciones** para subidas de nivel

## 📁 **Archivos del Sistema**

### **Core Files:**
- `eventSheets/LevelSystemEvents.json` - Lógica principal del sistema de niveles
- `eventSheets/GlobalVariables.json` - Variables globales del sistema
- `eventSheets/CombatEvents.json` - Integración con el sistema de combate
- `eventSheets/Town.json` - Sistema de distribución de estadísticas

### **Variables del Sistema:**
- `playerLevel` - Nivel actual del jugador
- `playerExperience` - Experiencia acumulada
- `experienceToNextLevel` - Experiencia necesaria para el siguiente nivel
- `availablePoints` - Puntos disponibles para distribuir
- `enemyExperienceReward` - Experiencia otorgada por enemigo
- `showLevelUpNotification` - Estado de notificación de subida
- `levelUpNotificationTimer` - Timer para notificaciones

## 🎯 **Funcionamiento del Sistema**

### **1. Ganancia de Experiencia**
```javascript
// Al derrotar un enemigo
playerExperience = playerExperience + enemyExperienceReward
```

**Ejemplo:**
- **Experiencia actual**: 0 XP
- **Enemigo derrotado**: +100 XP
- **Nueva experiencia**: 100 XP
- **Experiencia necesaria**: 100 XP
- **Resultado**: ¡Subida de nivel!

### **2. Escalado de Experiencia**
```javascript
// Después de cada subida de nivel
experienceToNextLevel = experienceToNextLevel * 1.5
```

**Progresión:**
- **Nivel 1 → 2**: 100 XP
- **Nivel 2 → 3**: 150 XP
- **Nivel 3 → 4**: 225 XP
- **Nivel 4 → 5**: 337.5 XP

### **3. Distribución de Puntos**
- **Al subir de nivel**: +5 puntos disponibles
- **Puntos acumulables**: Sí, se pueden guardar para usar después
- **Máximo por estadística**: Configurable en `maxAttributeValue`

## 🛡️ **Sistema de Protección de Estadísticas**

### **Variables Base (Valores Confirmados):**
- `tempStrength` - Fuerza confirmada
- `tempVitality` - Vitalidad confirmada
- `tempAgility` - Agilidad confirmada
- `tempSpeed` - Velocidad confirmada
- `tempDefense` - Defensa confirmada

### **Validaciones Implementadas:**
```json
// Ejemplo para StrengthMinusButton
{
    "conditions": [
        "Strength > tempStrength",  // Solo si es mayor al valor confirmado
        "attributesLocked == 0"     // Solo si no están bloqueados
    ]
}
```

**Resultado:**
- ✅ **Puede incrementar** estadísticas con puntos nuevos
- ✅ **Puede reasignar** puntos nuevos entre estadísticas
- ❌ **NO puede modificar** estadísticas ya confirmadas previamente

## 📊 **Barra de Experiencia**

### **Características:**
- **Ancho máximo**: 570 píxeles
- **Animación**: 0.5 segundos con `easeoutsine`
- **Límite de overflow**: `min(570, cálculo)` previene salirse del marco

### **Fórmula de Cálculo:**
```javascript
end-value: "min(570, 570 * (playerExperience / experienceToNextLevel))"
```

**Ejemplos:**
- **0/100 XP**: 0 píxeles (barra vacía)
- **50/100 XP**: 285 píxeles (barra a la mitad)
- **100/100 XP**: 570 píxeles (barra llena)
- **150/100 XP**: 570 píxeles (barra llena, sin overflow)

### **Actualizaciones Automáticas:**
1. **Al ganar experiencia** → Se actualiza desde `enemyDefeated`
2. **Al subir de nivel** → Se actualiza desde `levelUp`
3. **Al mostrar notificación** → Se resetea desde `showLevelUpMessage`

## 🔧 **Implementación Técnica**

### **Función `enemyDefeated`:**
```json
{
    "actions": [
        "Otorgar experiencia",
        "Actualizar barra visual",
        "Verificar subida de nivel"
    ]
}
```

### **Función `checkLevelUp`:**
```json
{
    "conditions": [
        "playerExperience >= experienceToNextLevel"
    ],
    "actions": [
        "callFunction levelUp"
    ]
}
```

### **Función `levelUp`:**
```json
{
    "actions": [
        "Incrementar nivel",
        "Restar experiencia gastada",
        "Otorgar 5 puntos",
        "Actualizar experiencia necesaria",
        "Actualizar barra visual",
        "Mostrar notificación"
    ]
}
```

## 🎮 **Flujo de Juego Completo**

### **1. Inicio del Juego:**
- Variables se inicializan automáticamente
- `playerLevel = 1`, `playerExperience = 0`, `experienceToNextLevel = 100`

### **2. Durante el Combate:**
- Al derrotar enemigo → Se ejecuta `enemyDefeated`
- Se otorga experiencia y se actualiza la barra
- Se verifica si puede subir de nivel

### **3. Subida de Nivel:**
- Si `playerExperience >= experienceToNextLevel` → Se ejecuta `levelUp`
- Se incrementa nivel y se otorgan 5 puntos
- Se actualiza experiencia necesaria para siguiente nivel
- Se muestra notificación de subida

### **4. Distribución de Puntos:**
- En layout `Town` → Jugador puede distribuir puntos disponibles
- Sistema protege estadísticas ya confirmadas
- Solo permite modificar puntos nuevos obtenidos

## 🚨 **Solución de Problemas**

### **Problema: Bucle Infinito de Subida de Nivel**
**Causa:** Bloque de eventos de nivel superior ejecutándose continuamente
**Solución:** Integrar lógica de subida directamente en `enemyDefeated`

### **Problema: Barra de Experiencia se Sale del Marco**
**Causa:** Cálculo sin límite máximo cuando se excede `experienceToNextLevel`
**Solución:** Usar `min(570, cálculo)` para limitar el ancho máximo

### **Problema: Jugador Modifica Estadísticas Confirmadas**
**Causa:** Falta de validación en botones de decremento
**Solución:** Comparar contra variables `temp*` (valores base confirmados)

## 📈 **Personalización del Sistema**

### **Cambiar Experiencia por Enemigo:**
```json
"enemyExperienceReward": "100"  // Cambiar valor según enemigo
```

### **Cambiar Escalado de Experiencia:**
```json
"experienceToNextLevel * 1.5"   // Cambiar multiplicador
```

### **Cambiar Puntos por Nivel:**
```json
"availablePoints + 5"           // Cambiar cantidad de puntos
```

### **Cambiar Ancho de Barra:**
```json
"min(570, 570 * ...)"          // Cambiar 570 por nuevo ancho
```

## 🔍 **Debugging y Logs**

### **Console Logs Implementados:**
- 🎮 **Inicialización**: Variables del sistema
- ⚔️ **Combate**: Enemigo derrotado, experiencia otorgada
- 🔍 **Verificación**: Estado de subida de nivel
- 🚀 **Subida**: Proceso de incremento de nivel
- 📊 **UI**: Estado de notificaciones y barras

### **Variables a Monitorear:**
- `playerExperience` vs `experienceToNextLevel`
- `availablePoints` para distribución
- Estado de `attributesLocked`
- Valores de variables `temp*`

## ✅ **Checklist de Implementación**

- [ ] **Incluir `LevelSystemEvents.json`** en el layout principal
- [ ] **Incluir `GlobalVariables.json`** para variables del sistema
- [ ] **Configurar `enemyExperienceReward`** según balance del juego
- [ ] **Verificar integración** con sistema de combate
- [ ] **Probar sistema de protección** de estadísticas
- [ ] **Verificar animaciones** de barra de experiencia
- [ ] **Testear escalado** de experiencia por nivel

## 🎯 **Próximas Mejoras Sugeridas**

1. **Sistema de logros** basado en niveles
2. **Bonificaciones especiales** por subir múltiples niveles
3. **Sistema de experiencia retroactiva** para enemigos de bajo nivel
4. **Notificaciones personalizables** para diferentes tipos de subida
5. **Sistema de prestigio** para niveles muy altos

---

**Versión**: 2.0  
**Última actualización**: Sistema de protección implementado  
**Estado**: ✅ Completamente funcional

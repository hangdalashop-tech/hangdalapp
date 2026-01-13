# Fase 2-3: Arquitectura UX, Wireframes y Especificaciones de Diseño

## Aura Handpan - Prototipo Interactivo

---

## 1. Mapa de Sitio (Sitemap)

```
Aura Handpan
├── 🏠 Inicio (Home)
│   ├── Dashboard personalizado
│   ├── Últimas lecciones
│   ├── Retos creativos semanales
│   └── Acceso rápido a Mi Viaje (diario)
│
├── 📚 Aprender (Learning)
│   ├── El Dojo (Academia)
│   │   ├── Nivel 1 - Principiante
│   │   ├── Nivel 2 - Intermedio
│   │   └── Nivel 3 - Avanzado
│   ├── Biblioteca de Escalas
│   │   ├── Explorador de escalas
│   │   ├── Reproductor de escala
│   │   └── Comparador de escalas
│   ├── Entrenador Rítmico
│   │   ├── Metrónomo visual
│   │   └── Patrones de práctica
│   └── Glosario de Técnicas
│       └── Vídeos de técnicas
│
├── 🎨 Crear (Create)
│   ├── Grabadora de Ideas
│   │   ├── Multipista
│   │   └── Mis grabaciones
│   ├── Soundscape Generativo
│   │   ├── Selector de escala
│   │   └── Controles de ambiente
│   ├── Generador de Progresiones
│   │   ├── Sugerencias armónicas
│   │   └── Presets
│   └── Mi Colección de Handpans
│       ├── Agregar instrumento
│       └── Detalles del instrumento
│
├── 🌍 Comunidad (Community)
│   ├── Mapa Mundial
│   │   ├── Intérpretes cercanos
│   │   └── Eventos locales
│   ├── Echo Circles (Jam Virtual)
│   │   ├── Salas activas
│   │   └── Crear nueva sesión
│   ├── Perfiles de Músico
│   │   ├── Mi perfil
│   │   └── Explorar perfiles
│   ├── Tablón de Eventos
│   │   ├── Festivales
│   │   ├── Talleres
│   │   └── Retiros
│   └── Chispas Semanales (Retos)
│       ├── Reto actual
│       └── Mis respuestas
│
└── 🔧 Herramientas (Tools)
    ├── Aura Tune (Afinador)
    ├── Metrónomo
    ├── Mi Viaje (Diario de Práctica)
    ├── Mi Flow (Seguimiento de Progreso)
    ├── Catálogo de Composiciones
    ├── Modo Offline
    └── Configuración
        ├── Perfil
        ├── Preferencias
        └── Acerca de

```

---

## 2. Flujos de Usuario Clave

### Flujo 1: Principiante Descubre y Aprende

```
Inicio → Explora "Aprender" → Selecciona Nivel 1 
→ Ve lección introductoria → Practica con Aura Tune 
→ Registra progreso en "Mi Viaje" → Accede a Biblioteca de Escalas 
→ Toca virtualmente una escala → Guarda favorita
```

### Flujo 2: Artista Crea y Comparte

```
Inicio → Accede a "Crear" → Abre Grabadora de Ideas 
→ Crea base rítmica → Añade melodía → Genera progresiones 
→ Guarda en Catálogo → Comparte en Comunidad 
→ Participa en Chispas Semanales
```

### Flujo 3: Terapeuta Personaliza Sesión

```
Inicio → "Crear" → Soundscape Generativo 
→ Selecciona escala del handpan → Configura ambiente 
→ Inicia reproducción → Abre "Mi Viaje" para anotar 
→ Registra impacto emocional → Guarda sesión
```

### Flujo 4: Comunidad Conecta

```
Inicio → "Comunidad" → Mapa Mundial 
→ Localiza músicos cercanos → Accede a Echo Circles 
→ Se une a sesión de jam en vivo → Interactúa con otros intérpretes
```

---

## 3. Especificaciones de Componentes UI

### Navegación Principal (Bottom Tab Bar)

**Ubicación**: Fijo en la base de la pantalla (mobile-first)
**Elementos**: 5 tabs con iconos + etiquetas
**Comportamiento**: 
- Tab activo: color primario (#A67C52), fondo sutil
- Tab inactivo: color muted-foreground
- Transición suave (400ms)

### Card de Funcionalidad

**Estructura**:
- Icono en esquina superior izquierda (fondo primario/10)
- Título (h3, Cormorant)
- Descripción (body, Inter)
- Badge de categoría (esquina superior derecha)
- Hover state: elevación + cambio de sombra

**Sombra**: `0 20px 60px -15px rgba(166, 124, 82, 0.15)`

### Botones

**Variantes**:
- Primary: bg-primary, text-primary-foreground, shadow-organic-lg
- Secondary: bg-secondary, text-secondary-foreground
- Outline: border-border, bg-transparent
- Ghost: bg-transparent, text-foreground

**Transición**: 400ms ease-in-out

### Modal/Dialog

**Estilo**: Fondo con backdrop-blur, border sutil, shadow-organic-lg
**Animación entrada**: fade-in + scale (0.95 → 1.0)
**Cierre**: Click fuera o botón X

---

## 4. Paleta de Colores Aplicada

| Elemento | Color | Código | Uso |
|----------|-------|--------|-----|
| Primario | Bronce Oxidado | #A67C52 | CTAs, acentos, hover |
| Secundario | Verde Salvia | #8B9D83 | Estados activos, alternancia |
| Fondo | Crema | #F5F1E8 | Background principal |
| Texto | Carbón Suave | #3A3A3A | Texto principal |
| Muted | Beige Arena | #E8DCC4 | Backgrounds secundarios |
| Metal | Gris Plateado | #B8B8B8 | Bordes, divisores |
| Destructivo | Rojo Terracota | #C85A54 | Errores, acciones peligrosas |

---

## 5. Tipografía

| Elemento | Fuente | Peso | Tamaño | Line-height |
|----------|--------|------|--------|------------|
| H1 | Cormorant Garamond | 600 | 3.5rem (56px) | 1.2 |
| H2 | Cormorant Garamond | 600 | 2.5rem (40px) | 1.2 |
| H3 | Inter | 600 | 1.75rem (28px) | 1.3 |
| Body | Inter | 400 | 1.125rem (18px) | 1.7 |
| Caption | Space Mono | 400 | 0.875rem (14px) | 1.5 |
| Button | Inter | 500 | 1rem (16px) | 1 |

---

## 6. Patrones de Interacción

### Scroll Reveal
- Elementos aparecen con fade-in + slight scale (0.95 → 1.0)
- Delay escalonado: 100ms entre elementos
- Trigger: Cuando elemento entra en viewport

### Hover States
- Cards: Elevación suave + cambio de sombra + -translate-y-1
- Botones: Cambio de color + sombra más pronunciada
- Links: Underline suave + color primario

### Transiciones
- Duración estándar: 400ms
- Timing: cubic-bezier(0.4, 0, 0.2, 1)
- Excepción: Animaciones de carga (pulse suave)

### Estados de Carga
- Pulse orgánico en lugar de spinner mecánico
- Skeleton screens con gradiente sutil

---

## 7. Responsive Design

### Breakpoints
- Mobile: < 640px
- Tablet: 640px - 1024px
- Desktop: > 1024px

### Ajustes por Pantalla
- Mobile: Bottom tab navigation, single column, full-width cards
- Tablet: 2-column grid, sidebar colapsable
- Desktop: 3-column grid, sidebar persistente

---

## 8. Especificaciones de Pantallas Principales

### Pantalla 1: Inicio (Home)
- Hero con imagen de fondo (parallax sutil)
- 3 secciones: Últimas lecciones, Mi Viaje rápido, Reto semanal
- Acceso rápido a herramientas (Aura Tune, Soundscape)
- Motivación personalizada basada en progreso

### Pantalla 2: El Dojo (Academia)
- Selector de nivel (1, 2, 3)
- Grid de lecciones con progreso visual
- Reproductor de vídeo con controles
- Botón "Practicar" que abre Aura Tune

### Pantalla 3: Biblioteca de Escalas
- Buscador de escalas
- Reproductor de escala (botón play)
- Visualización de notas
- Botón "Tocar virtualmente" (teclado interactivo)

### Pantalla 4: Grabadora de Ideas
- Pistas multipista (base, melodía, percusión)
- Controles de grabación (rec, play, stop)
- Volumen por pista
- Guardado automático

### Pantalla 5: Mapa Mundial
- Mapa interactivo con marcadores
- Filtros: Intérpretes, Profesores, Eventos
- Perfil emergente al hacer clic
- Botón "Conectar"

### Pantalla 6: Mi Viaje (Diario)
- Entrada de hoy destacada
- Historial de entradas (timeline)
- Campos: Duración, Escala practicada, Notas, Emoción
- Gráfico de consistencia

### Pantalla 7: Mi Flow (Progreso)
- Gráfico de horas de práctica (últimos 30 días)
- Hitos alcanzados (badges)
- Racha actual
- Comparativa con objetivos

---

## 9. Consideraciones de Accesibilidad

- Contraste mínimo WCAG AA (4.5:1 para texto)
- Navegación por teclado completa
- Labels explícitos en formularios
- Iconos con texto alternativo
- Colores no como único indicador (usar patrones además)
- Focus rings visibles en todos los elementos interactivos

---

## 10. Guía de Animaciones

### Entrada de Página
```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
duration: 600ms
delay: staggered 100ms
```

### Hover en Cards
```css
@keyframes elevate {
  from { transform: translateY(0); box-shadow: 0 20px 60px -15px rgba(..., 0.15); }
  to { transform: translateY(-4px); box-shadow: 0 30px 80px -20px rgba(..., 0.2); }
}
duration: 400ms
```

### Pulse de Carga
```css
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}
duration: 2s
```

---

## 11. Próximas Consideraciones para Desarrollo

1. **Integración de Audio**: Implementar Web Audio API para Aura Tune y Soundscape
2. **Grabación**: Usar MediaRecorder API para Grabadora de Ideas
3. **Mapas**: Google Maps API para Mapa Mundial
4. **Tiempo Real**: WebSockets para Echo Circles (jam virtual)
5. **Almacenamiento**: IndexedDB para modo offline
6. **Notificaciones**: Push notifications para retos semanales
7. **Sincronización**: Cloud sync para datos del usuario

---

**Fin de Arquitectura UX y Especificaciones**

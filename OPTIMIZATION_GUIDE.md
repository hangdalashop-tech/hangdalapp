# Guía de Optimización y Performance - Aura Handpan

## Resumen Ejecutivo

Esta guía documenta las estrategias de optimización implementadas y recomendadas para Aura Handpan, enfocándose en rendimiento web, PWA y preparación para mobile.

## 1. Optimización Web

### 1.1 Code Splitting

**Implementación actual:**
- Vite realiza code splitting automático
- Componentes de páginas se cargan bajo demanda

**Mejoras recomendadas:**
```typescript
// Lazy loading de componentes
import { lazy, Suspense } from 'react';

const Learning = lazy(() => import('./pages/Learning'));
const Create = lazy(() => import('./pages/Create'));
const Community = lazy(() => import('./pages/Community'));
const Tools = lazy(() => import('./pages/Tools'));

// En Router:
<Suspense fallback={<LoadingSpinner />}>
  <Route path="/learning" component={Learning} />
</Suspense>
```

### 1.2 Compresión de Assets

**Recomendaciones:**
```bash
# Instalar plugin de compresión
npm install -D vite-plugin-compression

# En vite.config.ts:
import compression from 'vite-plugin-compression';

export default {
  plugins: [
    compression({
      algorithm: 'brotli',
      ext: '.br',
    }),
  ],
};
```

### 1.3 Image Optimization

**Estrategia actual:**
- Imágenes generadas con alta calidad
- Almacenadas en `/public/images/`

**Mejoras:**
```typescript
// Usar WebP con fallback
<picture>
  <source srcSet="/images/hero.webp" type="image/webp" />
  <img src="/images/hero.png" alt="Hero" />
</picture>

// O usar componente optimizado
import Image from 'next-image-export-optimizer';
```

## 2. PWA Optimization

### 2.1 Service Worker Caching Strategy

**Implementado:**
- Cache-first para assets estáticos
- Network-first para API calls
- Fallback offline

**Métricas esperadas:**
- Tiempo de carga inicial: 2-3s
- Tiempo de carga en caché: <500ms
- Tamaño de caché: ~5-10MB

### 2.2 Web Vitals

**Monitoreo recomendado:**
```typescript
// Instalar web-vitals
npm install web-vitals

// En main.tsx
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals';

getCLS(console.log);
getFID(console.log);
getFCP(console.log);
getLCP(console.log);
getTTFB(console.log);
```

**Objetivos:**
- LCP (Largest Contentful Paint): < 2.5s
- FID (First Input Delay): < 100ms
- CLS (Cumulative Layout Shift): < 0.1

### 2.3 Manifest Optimization

**Implementado:**
- manifest.json con iconos múltiples
- Shortcuts para acceso rápido
- Share target API

**Validación:**
```bash
# Usar Lighthouse
npm install -g lighthouse
lighthouse https://aura-handpan.app --view
```

## 3. Audio Processing Optimization

### 3.1 Web Audio API Performance

**Optimizaciones implementadas:**
```typescript
// Usar OfflineAudioContext para procesamiento
const offlineContext = new OfflineAudioContext(
  2,
  44100 * 10, // 10 segundos
  44100
);

// Renderizar offline
const renderedBuffer = await offlineContext.startRendering();
```

### 3.2 Grabación Multipista

**Optimizaciones:**
```typescript
// Usar MediaRecorder con opciones
const options = {
  audioBitsPerSecond: 128000, // 128kbps
  mimeType: 'audio/webm;codecs=opus',
};

const mediaRecorder = new MediaRecorder(stream, options);
```

### 3.3 Soundscape Generativo

**Mejoras de rendimiento:**
```typescript
// Usar Web Workers para síntesis
const worker = new Worker('audio-worker.js');

worker.postMessage({
  type: 'generate-soundscape',
  scale: selectedScale,
  duration: 30,
});

worker.onmessage = (event) => {
  const audioBuffer = event.data;
  // Reproducir buffer
};
```

## 4. Database Optimization

### 4.1 IndexedDB

**Optimizaciones:**
```typescript
// Crear índices para búsquedas rápidas
const recordingStore = database.createObjectStore('recordings', { keyPath: 'id' });
recordingStore.createIndex('date', 'date', { unique: false });
recordingStore.createIndex('scale', 'scale', { unique: false });

// Usar cursor para iteración eficiente
const index = recordingStore.index('date');
const range = IDBKeyRange.bound(startDate, endDate);
const cursor = index.openCursor(range);
```

### 4.2 Limpieza de Datos

**Política de retención:**
```typescript
export async function cleanupOldData() {
  const db = await initDB();
  const thirtyDaysAgo = new Date(Date.now() - 30 * 24 * 60 * 60 * 1000);

  const tx = db.transaction(['recordings', 'diaryEntries'], 'readwrite');
  
  // Eliminar grabaciones antiguas
  const recordingIndex = tx.objectStore('recordings').index('date');
  const range = IDBKeyRange.upperBound(thirtyDaysAgo);
  recordingIndex.openCursor(range).onsuccess = (event) => {
    const cursor = event.target.result;
    if (cursor) {
      cursor.delete();
      cursor.continue();
    }
  };
}
```

## 5. React Performance

### 5.1 Memoization

**Implementación:**
```typescript
import { memo, useMemo, useCallback } from 'react';

// Memoizar componentes costosos
const TunerVisualizer = memo(function TunerVisualizer() {
  // ...
});

// Memoizar cálculos
const filteredEvents = useMemo(() => {
  return events.filter(e => e.type === selectedType);
}, [events, selectedType]);

// Memoizar callbacks
const handleClick = useCallback(() => {
  // ...
}, [dependencies]);
```

### 5.2 Virtual Scrolling

**Para listas largas:**
```typescript
import { FixedSizeList } from 'react-window';

export function EventsList({ events }) {
  return (
    <FixedSizeList
      height={600}
      itemCount={events.length}
      itemSize={100}
      width="100%"
    >
      {({ index, style }) => (
        <div style={style}>
          <EventCard event={events[index]} />
        </div>
      )}
    </FixedSizeList>
  );
}
```

## 6. Network Optimization

### 6.1 Lazy Loading

**Implementación:**
```typescript
// Lazy load imágenes
import { lazy } from 'react';

const EventImage = lazy(() => import('./EventImage'));

// Usar Intersection Observer
const useIntersectionObserver = (ref) => {
  const [isVisible, setIsVisible] = useState(false);

  useEffect(() => {
    const observer = new IntersectionObserver(([entry]) => {
      if (entry.isIntersecting) {
        setIsVisible(true);
        observer.unobserve(entry.target);
      }
    });

    if (ref.current) observer.observe(ref.current);
    return () => observer.disconnect();
  }, [ref]);

  return isVisible;
};
```

### 6.2 Request Batching

**Para múltiples API calls:**
```typescript
export async function batchRequests(requests) {
  return Promise.all(
    requests.map(req => fetch(req).then(r => r.json()))
  );
}

// Usar con debounce
const debouncedSearch = debounce((query) => {
  batchRequests([
    `/api/events?q=${query}`,
    `/api/classes?q=${query}`,
  ]);
}, 300);
```

## 7. Monitoreo y Métricas

### 7.1 Analytics

```typescript
// Trackear eventos importantes
export function trackEvent(eventName, eventData) {
  if (window.umami) {
    window.umami.track(eventName, eventData);
  }
}

// Ejemplos de uso
trackEvent('recording_started', { scale: 'D Minor' });
trackEvent('soundscape_generated', { duration: 30 });
trackEvent('event_shared', { eventId: '123' });
```

### 7.2 Error Tracking

```typescript
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: process.env.VITE_SENTRY_DSN,
  environment: process.env.NODE_ENV,
});

// Capturar errores automáticamente
export const ErrorBoundary = Sentry.withErrorBoundary(
  YourComponent,
  { fallback: <ErrorFallback /> }
);
```

## 8. Checklist de Optimización

- [ ] Code splitting implementado
- [ ] Lazy loading de componentes
- [ ] Imágenes optimizadas (WebP)
- [ ] Service Worker activo
- [ ] Manifest.json validado
- [ ] Web Vitals monitoreados
- [ ] IndexedDB con índices
- [ ] Memoization en componentes costosos
- [ ] Virtual scrolling para listas largas
- [ ] Analytics configurado
- [ ] Error tracking activo
- [ ] Lighthouse score > 90

## 9. Herramientas de Medición

```bash
# Lighthouse
npm install -g lighthouse
lighthouse https://aura-handpan.app --view

# WebPageTest
# https://www.webpagetest.org/

# Chrome DevTools
# DevTools → Performance → Record

# Bundlesize
npm install --save-dev bundlesize
```

## 10. Roadmap de Optimización

**Corto plazo (1-2 semanas):**
- Implementar lazy loading de componentes
- Optimizar imágenes a WebP
- Configurar compresión Brotli

**Mediano plazo (1 mes):**
- Implementar virtual scrolling
- Agregar Web Workers para audio
- Optimizar IndexedDB queries

**Largo plazo (2-3 meses):**
- Implementar edge caching
- Agregar analytics avanzado
- Optimizar para mobile (React Native)

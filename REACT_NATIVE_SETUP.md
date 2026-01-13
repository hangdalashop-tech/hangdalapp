# Guía de Conversión a React Native - Aura Handpan

## Descripción General

Este documento proporciona una hoja de ruta para convertir Aura Handpan de una aplicación web (PWA) a una aplicación nativa para iOS y Android usando React Native.

## Arquitectura Compartida

La aplicación está diseñada para compartir lógica entre web y mobile:

```
├── client/src/
│   ├── components/          # Componentes reutilizables
│   ├── pages/              # Páginas web (convertir a screens)
│   ├── hooks/              # Custom hooks (reutilizables)
│   ├── lib/                # Lógica de negocio (reutilizable)
│   └── contexts/           # Context API (reutilizable)
├── shared/                 # Código compartido (tipos, constantes)
└── mobile/                 # Nuevo: Código específico de React Native
    ├── src/
    │   ├── screens/        # Pantallas de la app
    │   ├── components/     # Componentes nativos
    │   ├── navigation/     # Navegación nativa
    │   └── hooks/          # Hooks específicos de mobile
    └── app.json           # Configuración de Expo
```

## Fases de Implementación

### Fase 1: Preparación (1-2 semanas)

1. **Extraer lógica compartida**
   - Mover `lib/db.ts` a `shared/`
   - Crear `shared/types.ts` con interfaces
   - Crear `shared/constants.ts` con datos

2. **Crear estructura de React Native**
   ```bash
   npx create-expo-app aura-handpan-mobile
   cd aura-handpan-mobile
   npm install react-native-paper expo-av expo-audio expo-permissions
   ```

3. **Configurar Expo**
   - Crear `app.json` con configuración
   - Configurar iconos y splash screens
   - Configurar permisos (micrófono, almacenamiento)

### Fase 2: Componentes Base (2-3 semanas)

1. **Convertir componentes web a React Native**
   - `TunerVisualizer` → `TunerScreen`
   - `MetronomeVisualizer` → `MetronomeScreen`
   - `SoundscapeGenerator` → `SoundscapeScreen`
   - `MultitrackRecorder` → `RecorderScreen`

2. **Implementar navegación**
   ```typescript
   import { NavigationContainer } from '@react-navigation/native';
   import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
   
   const Tab = createBottomTabNavigator();
   
   export default function Navigation() {
     return (
       <NavigationContainer>
         <Tab.Navigator>
           <Tab.Screen name="Home" component={HomeScreen} />
           <Tab.Screen name="Learn" component={LearningScreen} />
           <Tab.Screen name="Create" component={CreateScreen} />
           <Tab.Screen name="Community" component={CommunityScreen} />
           <Tab.Screen name="Tools" component={ToolsScreen} />
         </Tab.Navigator>
       </NavigationContainer>
     );
   }
   ```

3. **Adaptar Web Audio API**
   - Usar `expo-av` para reproducción de audio
   - Usar `expo-audio` para grabación
   - Implementar alternativas para Web Audio API

### Fase 3: Características Específicas (2-3 semanas)

1. **Audio en Mobile**
   ```typescript
   import * as Audio from 'expo-audio';
   
   export async function startRecording() {
     await Audio.setAudioModeAsync({
       allowsRecordingIOS: true,
       playsInSilentModeIOS: true,
     });
     
     const { recording } = await Audio.Recording.createAsync(
       Audio.RecordingOptionsPresets.HIGH_QUALITY
     );
     return recording;
   }
   ```

2. **Almacenamiento Local**
   - Usar `expo-file-system` en lugar de IndexedDB
   - Adaptar `lib/db.ts` para AsyncStorage + FileSystem

3. **Permisos**
   ```typescript
   import * as Permissions from 'expo-permissions';
   
   export async function requestMicrophonePermission() {
     const { status } = await Permissions.askAsync(
       Permissions.AUDIO_RECORDING
     );
     return status === 'granted';
   }
   ```

### Fase 4: Optimización y Testing (2 semanas)

1. **Performance**
   - Optimizar re-renders con `useMemo` y `useCallback`
   - Implementar lazy loading de pantallas
   - Usar `react-native-fast-image` para imágenes

2. **Testing**
   ```bash
   npm install --save-dev jest @testing-library/react-native
   ```

3. **Build y Distribución**
   - Configurar EAS Build para compilación en la nube
   - Generar certificados de firma
   - Preparar para App Store y Google Play

## Dependencias Recomendadas

```json
{
  "dependencies": {
    "react-native": "latest",
    "expo": "latest",
    "react-navigation": "^6.x",
    "@react-navigation/bottom-tabs": "^6.x",
    "@react-navigation/native": "^6.x",
    "react-native-paper": "^5.x",
    "expo-av": "latest",
    "expo-audio": "latest",
    "expo-file-system": "latest",
    "expo-permissions": "latest",
    "react-native-fast-image": "^8.x",
    "zustand": "^4.x",
    "zod": "^4.x"
  },
  "devDependencies": {
    "@testing-library/react-native": "latest",
    "jest": "latest",
    "typescript": "latest"
  }
}
```

## Mapeo de Componentes

| Web | Mobile | Notas |
|-----|--------|-------|
| `Card` | `Surface` | React Native Paper |
| `Button` | `Button` | React Native Paper |
| `Slider` | `Slider` | react-native-slider |
| `Badge` | `Chip` | React Native Paper |
| `Tabs` | `BottomTabNavigator` | React Navigation |
| Web Audio API | expo-av/expo-audio | Alternativa nativa |
| IndexedDB | AsyncStorage + FileSystem | Almacenamiento mobile |

## Estructura de Carpetas Recomendada

```
aura-handpan-mobile/
├── src/
│   ├── screens/
│   │   ├── HomeScreen.tsx
│   │   ├── LearningScreen.tsx
│   │   ├── CreateScreen.tsx
│   │   ├── CommunityScreen.tsx
│   │   └── ToolsScreen.tsx
│   ├── components/
│   │   ├── TunerVisualizer.tsx
│   │   ├── MetronomeVisualizer.tsx
│   │   ├── SoundscapeGenerator.tsx
│   │   └── MultitrackRecorder.tsx
│   ├── navigation/
│   │   └── Navigation.tsx
│   ├── hooks/
│   │   ├── useAudio.ts
│   │   └── useStorage.ts
│   ├── services/
│   │   └── audioService.ts
│   ├── contexts/
│   │   └── AudioContext.tsx
│   └── App.tsx
├── app.json
├── app.config.ts
└── package.json
```

## Configuración de app.json

```json
{
  "expo": {
    "name": "Aura Handpan",
    "slug": "aura-handpan",
    "version": "1.0.0",
    "assetBundlePatterns": ["**/*"],
    "ios": {
      "supportsTabletMode": true,
      "bundleIdentifier": "com.aurahandpan.app"
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#ffffff"
      },
      "package": "com.aurahandpan.app"
    },
    "plugins": [
      [
        "expo-av",
        {
          "microphonePermission": "Allow Aura Handpan to access your microphone for recording and tuning."
        }
      ]
    ]
  }
}
```

## Próximos Pasos

1. Crear rama `mobile` en el repositorio
2. Inicializar proyecto de Expo
3. Comenzar con Fase 1 de preparación
4. Implementar pantalla de inicio como prueba de concepto
5. Iterar con feedback de usuarios

## Recursos

- [React Native Docs](https://reactnative.dev/)
- [Expo Documentation](https://docs.expo.dev/)
- [React Navigation](https://reactnavigation.org/)
- [React Native Paper](https://callstack.github.io/react-native-paper/)
- [Expo AV (Audio/Video)](https://docs.expo.dev/versions/latest/sdk/av/)

## Consideraciones Importantes

- **Permisos**: Solicitar permisos de micrófono y almacenamiento
- **Rendimiento**: Optimizar para dispositivos de gama baja
- **Offline**: Asegurar que la app funcione sin conexión
- **Sincronización**: Implementar sincronización con backend cuando esté disponible
- **Testing**: Probar en dispositivos reales, no solo simuladores

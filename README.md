# 🚗 Control de Flota Semanal BETA

> Aplicación web progresiva (PWA) y APK Android para la gestión integral de flotas vehiculares — bitácoras semanales, historial de mantenimiento, reportes financieros y alertas inteligentes por kilometraje.

![Stack](https://img.shields.io/badge/React-19-61DAFB?style=flat&logo=react)
![TypeScript](https://img.shields.io/badge/TypeScript-5.8-3178C6?style=flat&logo=typescript)
![Tailwind](https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?style=flat&logo=tailwindcss)
![Supabase](https://img.shields.io/badge/Supabase-Backend-3ECF8E?style=flat&logo=supabase)
![Capacitor](https://img.shields.io/badge/Capacitor-Android_APK-119EFF?style=flat&logo=capacitor)
![Gemini](https://img.shields.io/badge/Google_Gemini-AI-4285F4?style=flat&logo=google)

---

## 📋 Descripción

**Control de Flota Semanal** es una solución full-stack para propietarios y operadores de flotas de vehículos (transporte, renta, logística). Permite registrar bitácoras semanales, controlar costos de mantenimiento, gestionar siniestros y visualizar rentabilidad por unidad — todo sincronizado en la nube con soporte offline.

La app se despliega como **APK nativo para Android** usando Capacitor, con backend serverless en Supabase y autenticación por Row Level Security (RLS).

---

## ✨ Funcionalidades principales

- **Dashboard general** — vista de estado de toda la flota con semáforo de salud por vehículo (excelente / atención / crítico)
- **Bitácora semanal** — registro de kilometraje, ganancias y evidencia fotográfica por semana
- **Historial de mantenimiento** — preventivo, predictivo, correctivo y estético con costos de refacciones y mano de obra
- **Mantenimientos pendientes** — programación por fecha o kilometraje con alertas automáticas
- **Gestión de siniestros** — registro de incidentes con fotos antes/después, costos estimados vs reales y fechas de cierre
- **Reportes financieros** — comparativa de ingresos vs gastos de los últimos 6 meses por unidad
- **Alertas por km** — configuración de intervalos personalizados para balatas, llantas, aceite y anticongelante
- **Cierre mensual en PDF** — descarga de reporte mensual generado con jsPDF
- **Sincronización cloud** — modo offline con sincronización a Supabase al conectarse
- **Push notifications** — notificaciones nativas Android via Capacitor para alertas de mantenimiento
- **Autenticación** — login/registro con Supabase Auth + políticas RLS por usuario

---

## 🛠️ Stack tecnológico

| Capa | Tecnología |
|---|---|
| Frontend | React 19, TypeScript 5.8, Tailwind CSS 4 |
| Backend / BaaS | Supabase (PostgreSQL, Auth, RLS) |
| Mobile | Capacitor (Android APK) |
| Server | Express + TSX (Node.js) |
| AI | Google Gemini API (`@google/genai`) |
| PDF | jsPDF |
| Animaciones | Motion (Framer Motion) |
| Iconos | Lucide React |
| Build | Vite 6 |

---

## 🗄️ Arquitectura de base de datos

```
vehicles
  ├── alert_settings      (1:1)
  ├── weekly_logs         (1:N)
  ├── maintenance_history (1:N)
  ├── pending_maintenance (1:N)
  └── siniestros          (1:N)
```

Toda la seguridad se maneja con **Row Level Security (RLS)** en Supabase — cada usuario solo accede a sus propios vehículos y registros. El schema SQL completo está disponible en [`src/lib/supabase.ts`](src/lib/supabase.ts).

---

## 🚀 Instalación y ejecución local

### Prerequisitos
- Node.js 20+
- Cuenta en [Supabase](https://supabase.com) (gratuita)
- API Key de [Google Gemini](https://ai.google.dev)

### 1. Clonar el repositorio
```bash
git clone https://github.com/adan-loera/control-de-flota-semanal.git
cd control-de-flota-semanal
```

### 2. Instalar dependencias
```bash
npm install
```

### 3. Configurar variables de entorno
```bash
cp .env.example .env.local
```
Edita `.env.local` con tus credenciales:
```env
VITE_SUPABASE_URL=https://tu-proyecto.supabase.co
VITE_SUPABASE_ANON_KEY=tu_anon_key
GEMINI_API_KEY=tu_gemini_api_key
```

### 4. Crear tablas en Supabase
En el **SQL Editor** de tu proyecto Supabase, ejecuta el schema incluido en `src/lib/supabase.ts` (busca la constante `SUPABASE_SQL_SCHEMA`).

### 5. Ejecutar en desarrollo
```bash
npm run dev
```

La app estará disponible en `http://localhost:5173`

---

## 📱 Compilar APK para Android

```bash
# Build de producción
npm run build

# Sincronizar con Capacitor
npx cap sync android

# Abrir en Android Studio
npx cap open android
```

En Android Studio: **Build → Generate Signed APK** o usar el emulador directamente.

> Consulta [`android_studio_apk_troubleshooting.md`](android_studio_apk_troubleshooting.md) para solución de problemas comunes.

---

## 📂 Estructura del proyecto

```
control-de-flota-semanal/
├── src/
│   ├── components/
│   │   ├── Dashboard.tsx              # Vista principal de flota
│   │   ├── VehicleDetail.tsx          # Detalle por vehículo
│   │   ├── FinancialReports.tsx       # Reportes de ingresos/gastos
│   │   ├── PendingMaintenanceList.tsx # Lista de mantenimientos pendientes
│   │   ├── CustomChart.tsx            # Gráficas personalizadas
│   │   ├── AuthScreen.tsx             # Login / Registro
│   │   └── Logo.tsx
│   ├── lib/
│   │   ├── supabase.ts                # Cliente Supabase + schema SQL + sync functions
│   │   └── api.ts                     # Utilidades de API
│   ├── types.ts                       # Interfaces TypeScript (Vehicle, WeeklyLog, etc.)
│   ├── mockData.ts                    # Datos de prueba
│   ├── App.tsx                        # Componente raíz + estado global
│   └── main.tsx
├── server.ts                          # Express server (secrets, config API)
├── index.html
├── vite.config.ts
├── tsconfig.json
├── .env.example
├── android_studio_apk_troubleshooting.md
└── android_native_notifications_guide.md
```

---

## 🔐 Seguridad

- Las credenciales de Supabase se inyectan en runtime a través del backend Express (no expuestas en el bundle del cliente)
- Soporte para override via `localStorage` en builds locales para dispositivos físicos
- Todas las tablas usan RLS: un usuario autenticado solo puede leer/escribir sus propios registros
- Secrets sensibles nunca se hardcodean en el frontend

---

## 📸 Screenshots


<img width="1220" height="683" alt="image" src="https://github.com/user-attachments/assets/51758b44-0b1e-4bf0-b192-92742bacc00e" />
<img width="462" height="666" alt="image" src="https://github.com/user-attachments/assets/9253bf18-22d0-4ce7-9af2-d913bc296b5e" />
<img width="432" height="418" alt="image" src="https://github.com/user-attachments/assets/ae3960d8-fe6e-4d33-b516-347eb42bdf0b" />
<img width="405" height="517" alt="image" src="https://github.com/user-attachments/assets/f5515c56-da22-49af-af33-4b969e054216" />
<img width="456" height="662" alt="image" src="https://github.com/user-attachments/assets/727ce142-39ca-4ada-8e0f-ff60e9a1ad60" />
<img width="442" height="346" alt="image" src="https://github.com/user-attachments/assets/31f02fcc-20e5-43f7-bd3b-1574ec28fdc8" />
<img width="428" height="402" alt="image" src="https://github.com/user-attachments/assets/2d26f9b2-700d-4367-925b-45f79a06e055" />
<img width="427" height="605" alt="image" src="https://github.com/user-attachments/assets/aa472e26-6010-4ec5-9f7e-6dfc896ef66e" />
<img width="443" height="360" alt="image" src="https://github.com/user-attachments/assets/3ae7f223-2359-4e95-a2c1-577f35010875" />
<img width="430" height="472" alt="image" src="https://github.com/user-attachments/assets/abdddc06-d9c1-4ee7-aeab-3c3c2f8f06f5" />
<img width="418" height="553" alt="image" src="https://github.com/user-attachments/assets/61967064-708b-4688-922d-f7cb5dfb535a" />


---

## 🗺️ Roadmap

- [ ] Notificaciones push nativas en iOS (Capacitor)
- [ ] Exportación de reportes a Excel
- [ ] Modo multi-usuario / compartir flota con empleados
- [ ] Integración con Google Maps para rutas
- [ ] Análisis predictivo de mantenimiento con Gemini AI

---

## 👤 Autor

**Adán Loera Sánchez**
Full Stack Developer · Estado de México

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Adán_Loera-0A66C2?style=flat&logo=linkedin)](https://linkedin.com/in/adan-loera-sanchez-721ab629b)
[![GitHub](https://img.shields.io/badge/GitHub-adan--loera-181717?style=flat&logo=github)](https://github.com/adan-loera)

---

## 📄 Licencia

Copyright (c) 2026 Adán Loera Sánchez and Grupo Sallo S.A.S. de C.V. All rights reserved.
This code is not open source. Viewing is permitted for portfolio purposes only.
Copying, modifying, or distributing this code is strictly prohibited.

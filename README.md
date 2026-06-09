<div align="center">

<a href="https://fixoria.com.co">
  <img src="https://raw.githubusercontent.com/joanbeno/joanbeno/main/banner.svg" alt="Nicolas Benavides — Fixoria" width="100%" />
</a>

### Administrador de Empresas · Constructor de soluciones digitales · Co-fundador de Fixoria

Construyo sistemas reales para empresas reales: desde diagnósticos de IA hasta ERPs industriales.<br/>
No solo estrategia: código que funciona en producción.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-nicolasbenavides-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/nicolasbenavides)
[![Fixoria](https://img.shields.io/badge/Fixoria-fixoria.com.co-172554?style=flat-square&logo=vercel&logoColor=white)](https://fixoria.com.co)
[![Email](https://img.shields.io/badge/Email-joanbeno@unicauca.edu.co-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:joanbeno@unicauca.edu.co)

</div>

---

## Proyectos en producción

### COTA: ERP Industrial para Talleres de Mecanizado

[![Demo en vivo](https://img.shields.io/badge/Demo_en_vivo-%E2%86%92_cota--jcb.vercel.app%2Fdemo-172554?style=for-the-badge&logo=vercel&logoColor=white)](https://cota-jcb.vercel.app/demo)

<img src="https://raw.githubusercontent.com/joanbeno/joanbeno/main/assets/cota-preview.jpg" alt="COTA — ERP Industrial para Talleres" width="100%" />

El **Taller Industrial JCB** (Popayán, Cauca) operaba con Word para cotizar, Excel para la contabilidad y WhatsApp para coordinar producción. COTA reemplazó todo eso: un sistema web mobile-first donde cada acción alimenta automáticamente al módulo siguiente, sin doble entrada de datos.

**Flujo completo integrado:**

```
Cotización → cliente aprueba por link público → OT generada automáticamente
→ Kanban de producción → Gantt por máquina → nómina → factura DIAN → cobro
```

**10 módulos en producción:**

| Módulo | Qué resuelve |
|--------|-------------|
| **Cotizaciones** | PDF con logo y membrete, envío automático por email, link de aprobación para el cliente. Estados: Borrador → Enviada → Aprobada → En producción → Facturada → Pagada |
| **Producción / OTs** | Kanban visual por estado, Gantt tipo job-shop organizado por máquina (Torno CNC · Fresadora · Soldadora…), asignación de operarios y tiempos reales |
| **Nómina** | Integrada a las horas registradas en cada orden de trabajo |
| **Cuentas por cobrar** | Semáforo de vencimiento con filtros. Vencidas · Pendiente · Parcial · Pagadas |
| **Contabilidad** | Flujo de caja mensual, balance general, P&L, exportación XLS lista para el contador |
| **Inventario** | Stock mínimo con alertas automáticas |
| **Proveedores / OC** | Órdenes de compra vinculadas al sistema |
| **Facturación DIAN** | Integración Factus API v2, rangos de numeración, consecutivo controlado, sandbox y producción |
| **Préstamos** | Deuda activa por entidad, cuotas pagadas vs. pendientes, barra de progreso |
| **Presupuestos** | Planificación mensual comparada contra ejecución real |

**Arquitectura — dispatcher centralizado:**

Toda la lógica de datos fluye por un único punto: `dispatchDb()`, un `switch` con ~200 operaciones nombradas (`'cotizaciones.crear'`, `'produccion.actualizarEstado'`, `'cxc.marcarPagada'`…). Los server components lo invocan directamente; los client components hacen POST a `/api/sheets`, que delega al mismo dispatcher. Un único lugar donde vive toda la lógica de negocio: auditable, extensible, sin endpoints dispersos.

```
Server Components  →  callSheets()      →  dispatchDb()  →  Supabase
Client Components  →  POST /api/sheets  →  dispatchDb()  →  Supabase
```

**Stack:** Next.js 16 · React 19 · TypeScript · Supabase (PostgreSQL) · NextAuth v5 · Tailwind v4 · Framer Motion · Recharts · ExcelJS · @react-pdf/renderer

---

### Agente IA de Ventas para WhatsApp Business

> Agente conversacional en producción para empresa de confección en Popayán. Funcionando 24/7 desde noviembre 2025, desplegado en VPS propio con integración directa a la API de Meta.

El cliente tenía un equipo comercial desbordado, inventario desactualizado y perdía clientes por respuesta lenta. El agente reemplazó esa carga operativa completamente.

**Lo que hace:**

- Vende: responde consultas, cotiza y cierra pedidos por WhatsApp sin intervención humana
- Entiende voz: transcribe notas de voz con Whisper y las procesa igual que texto
- Identifica productos: reconoce referencias, tipos de prenda y variantes desde texto, imagen o audio
- Asesora tallas: recomienda talla según medidas, historial de compras o descripción del cliente
- OCR: lee fotos de catálogos, etiquetas y referencias físicas; extrae datos para procesarlos en la conversación
- RAG: consulta una base de conocimiento del negocio (productos, precios, políticas) para respuestas precisas sin alucinar
- Ve y edita inventario: consulta stock en tiempo real en Google Sheets y lo actualiza directamente desde la conversación
- Avisa: notifica proactivamente: confirmaciones, estados de pedido, seguimientos automáticos
- Dispara despacho: al cerrar una venta envía automáticamente un correo al equipo vía Resend para que preparen y despachen el pedido
- Pausa: el equipo puede tomar el hilo manualmente en cualquier momento y devolver el control al agente

**Arquitectura:**

```
WhatsApp (cliente)
  → Meta API  →  N8N (orquestador)
                  ├── Whisper          (transcripción de notas de voz)
                  ├── OCR              (lectura de imágenes y catálogos)
                  ├── RAG              (base de conocimiento del negocio)
                  ├── Google Sheets    (inventario y registro de ventas)
                  ├── Resend           (email de despacho al equipo al cerrar venta)
                  └── LLM              (razonamiento y respuesta final)
  → WhatsApp (respuesta)
```

Desplegado en VPS propio. En producción continua desde noviembre 2025.

**Stack:** N8N · Meta API (WhatsApp Business) · Whisper · OCR · RAG · Google Sheets · Resend · VPS

[![Ver caso de éxito](https://img.shields.io/badge/Caso_de_éxito-fixoria.com.co-C0392B?style=flat-square&logo=whatsapp&logoColor=white)](https://fixoria.com.co)

---

### Diagnóstico de Madurez en IA (ML Studio)

> Herramienta de diagnóstico estratégico para equipos de trabajo. 12 preguntas → perfil de madurez (Explorador / Operativo / Optimizador / Estratega) → plan de recomendaciones → guía interactiva personalizada.

**Stack:** HTML · JavaScript · Google Apps Script · Chart.js · Vercel Serverless

[![Ver herramienta](https://img.shields.io/badge/Ver_herramienta-diagnostico--fixoria.vercel.app-3535cc?style=flat-square&logo=vercel&logoColor=white)](https://diagnostico-fixoria.vercel.app)

<img src="https://raw.githubusercontent.com/joanbeno/Diagnostico-IA/main/assets/facilitador-screenshot.png" alt="Dashboard Facilitador — Diagnóstico IA" width="100%" style="border-radius:8px;margin-top:8px;" />

---

### SAGEST: Diagnóstico de aprendizaje organizacional

> Sistema de evaluación de gestión del conocimiento para equipos de restaurante. 5 dimensiones · scoring server-side · radar de brechas · guía interactiva.

**Stack:** HTML · JavaScript · Google Apps Script · Chart.js · Vercel

[![Ver herramienta](https://img.shields.io/badge/Ver_herramienta-sagest.vercel.app-1E72E4?style=flat-square&logo=vercel&logoColor=white)](https://sagest.vercel.app)
[![Anexo técnico](https://img.shields.io/badge/Anexo_técnico-arquitectura_y_flujo-D4873A?style=flat-square)](https://sagest.vercel.app/arquitectura)

<img src="https://raw.githubusercontent.com/joanbeno/SAGEST/main/assets/preview.svg" alt="Flujo SAGEST" width="100%" />

---

## Proyectos académicos, Universidad del Cauca

### Cronograma de Práctica Académica

> Dashboard en tiempo real para seguimiento de práctica doctoral en IA para proyectos públicos. Conectado a Google Sheets, muestra progreso ponderado por fase, semana actual, diferencia vs. cronograma base y estado general.

**Stack:** HTML · JavaScript · Google Apps Script · Google Sheets API

<img src="https://raw.githubusercontent.com/joanbeno/joanbeno/main/assets/seguimiento-preview.jpg" alt="Cronograma de Práctica Académica — Dashboard en tiempo real" width="100%" style="border-radius:8px;margin-top:8px;" />

---

### Sistema de Seguimiento Académico (Prototipo)

> Propuesta de arquitectura para un sistema de seguimiento académico en la Unicauca. Google OAuth + Sheet Maestro + Apps Script como backend central. Incluye diagrama del sistema, hub del profesor y guía del estudiante. Costo: $0.

**Stack:** HTML · Google OAuth · Apps Script · Google Sheets

[![Ver prototipo](https://img.shields.io/badge/Ver_prototipo-sistema_de_seguimiento-1E72E4?style=flat-square&logo=googlechrome&logoColor=white)](https://joanbeno.github.io/prototipo-seguimiento-/)

<img src="https://raw.githubusercontent.com/joanbeno/joanbeno/main/assets/prototipo-preview.jpg" alt="Sistema de Seguimiento Académico — Prototipo" width="100%" style="border-radius:8px;margin-top:8px;" />

---

## Stack principal

![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=flat-square&logo=supabase&logoColor=white)
![PocketBase](https://img.shields.io/badge/PocketBase-B8DBE4?style=flat-square&logo=pocketbase&logoColor=black)
![Appwrite](https://img.shields.io/badge/Appwrite-FD366E?style=flat-square&logo=appwrite&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white)
![Netlify](https://img.shields.io/badge/Netlify-00C7B7?style=flat-square&logo=netlify&logoColor=white)
![Coolify](https://img.shields.io/badge/Coolify-6C47FF?style=flat-square&logo=coolify&logoColor=white)
![Google Apps Script](https://img.shields.io/badge/Google_Apps_Script-4285F4?style=flat-square&logo=google&logoColor=white)

---

<div align="center">

**[Fixoria](https://fixoria.com.co)** · Consultoría en IA y desarrollo de software · Popayán, Colombia

</div>

# Ayrton Cela — Portafolio de ingeniería y automatización

> Engineering manager y builder con más de 8 años en telecomunicaciones, APIs, infraestructura y automatización con IA.
> Construyo sistemas en producción que convierten conversaciones reales en datos limpios, estructurados y fáciles de buscar.

🌐 **Portafolio:** [ayrtoncela.cloud](https://ayrtoncela.cloud)
🎥 **Video demo:** [Bot de WhatsApp + backend](https://www.youtube.com/watch?v=_C-984BwlBQ)
🇺🇸 **English version:** [portafolio-ai](https://github.com/ayrtoncela/portafolio-ai)

> El código de clientes vive en repositorios privados. Con gusto muestro la arquitectura, los modelos de datos o una demo en vivo en una llamada.

---

## Proyectos

### 1. Atico Film Lab — Gestión de pedidos por Instagram DM · `En producción`

> Laboratorio de revelado analógico en CDMX. Cada pedido vivía en la bandeja de Instagram del equipo: sin seguimiento, sin historial.

**Resultados:** más de $50K MXN procesados en los primeros 2 meses · más de 150 rollos con seguimiento de punta a punta

- Bot que cotiza con el catálogo real, confirma el pedido y guía el pago (ES/EN, detección automática)
- Cada conversación se convierte en un pedido estructurado: ID (`ORD-YYYYMM-XXXX`), formato, cantidad, precio, sucursal, estado
- Comprobantes de pago recibidos por DM y validados desde el dashboard
- Dashboard de operación: pipeline tipo kanban, KPIs financieros por semana/mes, búsqueda global (Cmd+K), toma de control humana
- RAG sobre conversaciones pasadas (`text-embedding-3-small` → pgvector) para que el equipo pregunte en lenguaje natural

**Stack:** `Node.js` `Express` `OpenAI` `pgvector` `Instagram Graph API` `Supabase` `PostgreSQL` `Stripe` `Railway` `Sentry`

---

### 2. Mikaela Montenegro — CRM conversacional y atribución de leads · `En producción`

> Artista plástica y escuela de arte en Ecuador. Más de 880 conversaciones de Instagram sin responder, alumnos llevados en notas y sin saber qué campaña trajo a cada alumno.

**Resultados:** más de 880 conversaciones gestionadas · más de 100 DMs atendidos en 24 h sin intervención · leads sin atribuir de 370 a 4

- Bot que responde con base en su catálogo real (RAG)
- Dashboard de alumnos, talleres, comisiones, exposiciones y campañas
- Conciliación de los reportes de anuncios de Meta contra los leads para atribuir cada lead a su campaña
- Bot de campañas: cada reel promocionado tiene su propio contexto (oferta, precio, horario, cupo); los DMs se asocian solos al anuncio y la campaña se pausa sola al llenarse el último lugar
- Sitio web bilingüe con exposiciones, visor de obra y tienda de originales y prints giclée
- Calendario de contenido sincronizado 1:1 con el documento fuente (202 piezas)

**Stack:** `Node.js` `Express` `OpenAI` `RAG` `Supabase` `Stripe` `Resend` `Railway`

---

### 3. AyrTok — Plataforma de agendamiento conversacional (SaaS multi-cliente) · `En producción`

> Producto propio. Clínicas y negocios pequeños agendan a mano por WhatsApp e Instagram.

- Un solo backend para muchos negocios; ruteo de webhooks por cliente
- Las conversaciones se convierten en citas y expedientes validados
- Sincronización con Google Calendar y cobros por Stripe Connect (cada negocio recibe su dinero directo)
- Dashboard por negocio: agenda, expedientes, conversaciones, pagos

**Stack:** `Node.js` `Express` `Supabase` `WhatsApp Cloud API` `Instagram Graph API` `Google Calendar API` `Stripe Connect` `OpenAI` `Railway`

🔗 [ayrtok.com](https://ayrtok.com)

---

### 4. Laboratorio clínico — Bot de agendamiento por WhatsApp · `Demo en vivo`

- Agendamiento paso a paso: tipo de estudio → sucursal → día → hora → datos del paciente
- 3 sucursales con horarios e instrucciones de preparación por estudio
- Cada lead se registra en Google Sheets con notificación por email
- El mismo backend atiende también un chat web

**Stack:** `Node.js` `OpenAI` `Meta Cloud API (WhatsApp)` `Google Apps Script` `Google Sheets` `Railway`

🔗 [Demo en vivo](https://web-page-saa-s.vercel.app) (widget de chat en la esquina inferior derecha)

---

### 5. Plataforma de finanzas personales — Pipeline de datos local

- Un parser por banco o tarjeta: extracción de texto de PDF más OCR para estados escaneados
- Transacciones normalizadas en una sola base SQLite con categorización e indicadores mensuales
- Una auditoría de calidad de datos encontró y corrigió 29 transacciones duplicadas y 4 mal categorizadas que inflaban un mes en cerca de 25%

**Stack:** `Python` `SQLite` `pdfplumber` `Tesseract OCR`

---

## Arquitectura común de los bots

```
┌──────────────────┐     ┌─────────────────────┐     ┌──────────────────┐
│  Instagram DMs   │────▶│                     │────▶│   OpenAI API     │
│  WhatsApp        │     │  Node.js + Express  │     │  (prompt + RAG   │
│  Chat web        │────▶│      backend        │     │   por cliente)   │
└──────────────────┘     │                     │     └──────────────────┘
                         │  • Webhooks         │
                         │  • Máquina estados  │────▶┌──────────────────┐
                         │  • Deduplicación    │     │    Supabase      │
                         │  • Sesión + idioma  │     │  (PostgreSQL)    │
                         │  • Toma de control  │     └──────────────────┘
                         │  • Alertas email    │
                         └──────────┬──────────┘
                                    │
                         ┌──────────┴──────────┐
                         │  Dashboard de       │
                         │  operación          │
                         └─────────────────────┘
```

- Máquina de estados por cliente para que los flujos estructurados no se salgan de orden; respaldo con LLM para mensajes libres
- Deduplicación de mensajes (`processed_messages`), timeout de sesión configurable, detección ES/EN
- Restricciones reales de Instagram/WhatsApp resueltas: ventana de 24 h, tipos de mensaje, reintentos con backoff
- Toma de control humana: se pausa el bot y se responde a mano desde el dashboard

---

## Trayectoria profesional

- **Consulting Engineering Manager** en una plataforma UCaaS con sede en EE. UU. (KAZOO): lidero 6 ingenieros en distintos países; contact center de ~10,000 posiciones, la implementación más grande en la historia de la compañía; más de 30 proyectos empresariales en Europa, Sudáfrica, EE. UU. y LATAM
- **Senior Engineer** en MCM Telecom (México): más de 40 proyectos de telecomunicaciones en México y LATAM
- Kamailio · FreeSWITCH · Kazoo · BroadWorks · MetaSwitch · SIP/RTP · Ansible · Python · Linux
- Ingeniería en Electrónica y Telecomunicaciones · MBA

---

## Stack

| Área | Herramientas |
|---|---|
| Datos y backend | PostgreSQL · Supabase · SQLite · Node.js · Express · Python · REST APIs · Webhooks |
| Automatización e IA | OpenAI API · RAG / pgvector · Google Apps Script · Google Sheets · n8n · Claude Code |
| Mensajería y pagos | WhatsApp Cloud API · Instagram Graph API · Stripe / Stripe Connect · Google Calendar API |
| Infraestructura | Linux · Docker · Ansible · Bash · Git / GitHub · Railway · Vercel · Cloudflare · Sentry |

---

## Contacto

📧 [ayrton@ayrtoncela.cloud](mailto:ayrton@ayrtoncela.cloud)
📱 [+52 55 4462 1764](https://wa.me/525544621764) (WhatsApp)
💼 [LinkedIn](https://linkedin.com/in/ayrton-c-66361a203)
🌐 [ayrtoncela.cloud](https://ayrtoncela.cloud)

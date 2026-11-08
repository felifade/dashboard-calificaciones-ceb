# 📋 Regla de Sincronización de Agenda CEB 5/4 e iCloud

Esta regla y flujo de trabajo establece cómo se procesan, distribuyen y sincronizan las actividades escolares del **CEB 5/4** entre el repositorio institucional de GitHub y los calendarios/recordatorios personales de **iCloud (Apple)**.

---

## 🎯 Objetivo
Mantener una única fuente de verdad institucional en la web (`dashboard-calificaciones-ceb`) y, simultáneamente, automatizar la agenda personal del docente en sus dispositivos Apple (iPhone, iPad, Mac) clasificando eventos y fechas límite.

---

## 🔄 Protocolo de Flujo de Trabajo en 2 Pasos

### Paso 1: Registro en Agenda Institucional (GitHub)
Cada vez que el usuario proporcione actividades, comunicados o fechas:
1. **Actualizar [`agenda.html`](file:///home/antigravity/felifade/dashboard-calificaciones-ceb/agenda.html)**:
   - Registrar la actividad en `TABLE_23` (si es actividad oficial SEMS/DGB) o en el bloque correspondiente de `WEEKS_LIST` (Semanas S00 a S20).
   - Indicar responsables, turnos (Matutino / Vespertino / Ambos) y estatus (`done`, `active`, `next`).
2. **Actualizar [`agenda.ics`](file:///home/antigravity/felifade/dashboard-calificaciones-ceb/agenda.ics)**:
   - Crear el bloque estándar `BEGIN:VEVENT ... END:VEVENT` con UID único, fecha de inicio (`DTSTART`), fecha fin (`DTEND`) y descripción clara.
3. **Guardar y Sincronizar en Git**:
   - `git add agenda.html agenda.ics`
   - `git commit -m "docs(agenda): actualizar calendario institucional"`
   - `git push origin main`

---

### Paso 2: Filtro y Sincronización en iCloud Personal
Analizar las actividades ingresadas y determinar cuáles le corresponden al usuario (como docente de Cultura Digital/Informática, tutor o miembro de academia):

1. **Eventos y Reuniones (Calendario iCloud)**:
   - Se inyectan en el calendario correspondiente:
     - `CEB Phil`: Clases, tutorías y seguimiento personal directo.
     - `CEB Actividades`: Reuniones generales, academias, eventos cívicos o institucionales del plantel.
2. **Fechas Límite y Entregas (Recordatorios Apple Reminders)**:
   - Las fechas críticas (ej. entrega de calificaciones al PACE, boletas a tutores, firma de actas) se registran en la lista de recordatorios con fecha/hora de vencimiento y alarma.

---

## ⚙️ Herramientas y Scripts Locales

* **Entorno virtual CalDAV:**
  - Ubicación: `/home/antigravity/.caldav-venv`
  - Librería: `caldav` (v3.7.0) + `icalendar`
* **Script de sincronización:**
  - [`/home/antigravity/icloud_manager.py`](file:///home/antigravity/icloud_manager.py)
* **Credenciales configuradas (App-Specific):**
  - Apple ID: `felifade@icloud.com`
  - Protocolo: CalDAV vía `https://caldav.icloud.com/`

---

## 📌 Guía de Clasificación Rápida para el Asistente

| Tipo de Actividad | Destino 1 (GitHub) | Destino 2 (iCloud) | Formato en iCloud |
| :--- | :--- | :--- | :--- |
| **Evaluación Parcial (Exámenes)** | `agenda.html` + `agenda.ics` | Calendario `CEB Phil` | Evento con horario |
| **Entrega de Calificaciones (PACE)** | `agenda.html` + `agenda.ics` | Lista `Reminders` | Recordatorio con alerta (Due date) |
| **Reunión de Academia / General** | `agenda.html` + `agenda.ics` | Calendario `CEB Actividades` | Evento |
| **Entrega de Boletas a Padres** | `agenda.html` + `agenda.ics` | Calendario `CEB Phil` + Recordatorio | Evento + Recordatorio preparatorio |
| **Suspensión oficial / Consejo** | `agenda.html` + `agenda.ics` | Calendario `CEB Actividades` | Evento de día completo |

# ComandaSmart SaaS - Sistema de Gestión de Pedidos por QR
sexo anal

Proyecto de plataforma **Multi-tenant** para la gestión automatizada de comandas, pagos y menús en tiempo real para múltiples restaurantes.

---

## Arquitectura del Sistema

El sistema utiliza un modelo de software como servicio (SaaS), permitiendo que múltiples negocios operen de forma aislada sobre la misma infraestructura.

### Componentes Principales:
1.  **App Cliente (PWA):** Interfaz ligera accesible mediante escaneo de QR (Mesa/Restaurante).
2.  **Dashboard Restaurante:** Panel de control para cocina y camareros con actualizaciones en tiempo real.
3.  **SuperAdmin Panel:** Gestión de suscripciones, altas de locales y analíticas globales.

---

## Stack Tecnológico

### Frontend (Web & Mobile Friendly)
* **Framework:** `Next.js` (React) - Para SEO en menús y velocidad de carga.
* **Estilos:** `Tailwind CSS` - Temas dinámicos según la identidad visual de cada local.
* **Gestión de Estado:** `TanStack Query` (React Query) - Sincronización de datos.

### Backend (Escalabilidad)
* **Entorno:** `Node.js` con `TypeScript`.
* **API:** `REST` o `GraphQL`.
* **Real-time:** `Socket.io` - Notificación inmediata de nuevas comandas a cocina.

### Base de Datos y Almacenamiento
* **Principal:** `PostgreSQL` - Relaciones complejas (Restaurante -> Categoría -> Plato).
* **Caché/Sesiones:** `Redis` - Para gestionar carritos de compra activos.
* **Imágenes:** `Cloudinary` o `AWS S3` - Almacenamiento de fotos de los platos.

---

## Esquema de Base de Datos (Propuesta Inicial)

```sql
-- Estructura simplificada para Multi-tenancy
Table Restaurantes {
  id uuid [primary key]
  nombre varchar
  slug_url varchar [unique] -- ej: "pizzeria-luigi"
  config_estetica jsonb -- colores, logo, fuentes
}

Table Mesas {
  id uuid [primary key]
  restaurante_id uuid [ref: > Restaurantes.id]
  numero integer
  qr_code_url text
}

Table Productos {
  id uuid [primary key]
  restaurante_id uuid [ref: > Restaurantes.id]
  nombre varchar
  precio decimal
  disponible boolean
}

Table Comandas {
  id uuid [primary key]
  mesa_id uuid [ref: > Mesas.id]
  estado varchar -- 'pendiente', 'preparando', 'servido', 'pagado'
  total decimal
  created_at timestamp
}
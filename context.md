# Contexto — RentaCar (demo)

## Usuario objetivo
Persona que quiere alquilar un auto desde el celular, rápido y sin registrarse.

## Stack y restricciones
- Next.js 16 (App Router) + TypeScript + Tailwind
- Mobile-first: 375px primero, escritorio desde 768px
- Sin librerías de componentes (ni shadcn, MUI, Chakra)
- Navegación interna solo con <Link>, nunca <a href>
- Solo componentes funcionales (const)

## Páginas
| Ruta | Propósito | Componentes |
|------|-----------|-------------|
| `/` | Explorar autos | Navbar, SearchBar, CategoryFilter, CarGrid, CarCard |
| `/fleet` | Ver disponibles y ordenar | ResultsHeader, CarCard, MapPlaceholder |
| `/cars/[id]` | Detalle y reserva | PhotoGallery, CarHeader, SpecsGrid, BookingCard |

## Datos
// types/car.ts
interface Car {
  id: string;
  model: string;
  year: number;
  category: "Económico" | "SUV" | "Eléctrico" | "Lujo";
  pricePerDay: number;
  rating: number;
  reviews: number;
  seats: number;
  transmission: "Manual" | "Automática";
  fuel: string;
  doors: number;
  photos: string[];
}

## Comportamiento
- SearchBar filtra las tarjetas en tiempo real
- CategoryFilter resalta el chip activo
- CarGrid muestra estado de carga mientras "llegan" los datos
- BookingCard: contador de días entre 1 y 30
- Click en CarCard → /cars/[id]; en detalle, botón volver → /fleet

## Carpetas
/app (rutas) · /components (UI reutilizable) · /types (interfaces)

## Fuera de alcance
Login, pagos, mapa real, backend.
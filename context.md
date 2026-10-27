# Contexto — RentaCar (demo)

## Usuario objetivo
Persona que quiere alquilar un auto desde el celular, rápido y sin registrarse.

## Stack y restricciones
- Next.js 16 (App Router) + TypeScript + Tailwind
- Inicializar con `npx create-next-app@16`, seleccionando TypeScript, Tailwind y App Router, sin directorio `src`.
- Mobile-first: 375px primero, escritorio desde 768px
- Sin librerías de componentes (ni shadcn, MUI, Chakra)
- Navegación interna solo con <Link>, nunca <a href>
- Solo componentes funcionales (const)
- Los componentes que usen estado, efectos o eventos deben estar dentro de una frontera de cliente definida con `"use client"`.

## Páginas
| Ruta | Propósito | Componentes |
|------|-----------|-------------|
| `/` | Explorar autos | Navbar, SearchBar, CategoryFilter, CarGrid, CarCard |
| `/fleet` | Ver disponibles y ordenar | ResultsHeader, CarCard, MapPlaceholder |
| `/cars/[id]` | Detalle y reserva | PhotoGallery, CarHeader, SpecsGrid, BookingCard |

## Datos
Definir y exportar la interfaz `Car` en `/types/car.ts`. Usar un conjunto de datos locales compartido por las tres rutas, con identificadores únicos y vehículos de las cuatro categorías. No se necesita una API.

```ts
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
```

## Comportamiento

### Inicio (`/`)
- `Navbar`: logo, campo de búsqueda e icono de usuario. Reutilizar `SearchBar` para el campo de búsqueda, sin duplicar controles. El icono de usuario es visual y no inicia un flujo de login.
- `SearchBar`: input controlado con `useState`; filtra por modelo en tiempo real, sin distinguir mayúsculas y omitiendo espacios al principio y al final.
- `CategoryFilter`: chips Económico, SUV, Eléctrico y Lujo, con categoría activa en `useState` y resaltado visual. Añadir "Todos", activo inicialmente, para quitar el filtro.
- La búsqueda y la categoría se combinan: una tarjeta debe cumplir ambos filtros para mostrarse.
- `CarGrid`: simular la carga de datos con `useEffect` y `setTimeout`, limpiar el temporizador al desmontar y mostrar un estado de carga mientras llegan los datos.
- La cuadrícula tiene una columna a 375px y varias desde 768px. Los chips permiten desplazamiento horizontal sin desbordar la página.
- `CarCard`: foto o placeholder si no está disponible, modelo, precio por día y valoración con estrellas. Cada tarjeta navega con `<Link>` a `/cars/[id]`.
- Si ningún coche coincide, mostrar "No hay coches que coincidan con tu búsqueda".

### Flota (`/fleet`)
- `ResultsHeader`: mostrar "X coches disponibles", calculado a partir de los resultados visibles.
- Control de orden con `useState`: precio ascendente por defecto y opción descendente. El control debe reordenar las tarjetas, sin modificar el conjunto de datos original.
- Reutilizar el mismo componente `CarCard` que en el inicio.
- `MapPlaceholder`: caja gris con texto "Mapa", debajo de los resultados en móvil y a su derecha desde 768px. No tiene funcionalidad de mapa real.

### Detalle (`/cars/[id]`)
- Cargar el coche correspondiente al identificador con `useEffect`, usando los datos locales y mostrando un estado de carga hasta resolverlo.
- Si el identificador no existe, mostrar "Coche no encontrado" y un enlace para volver a `/fleet`.
- `PhotoGallery`: índice de foto en `useState`, botones Anterior/Siguiente y navegación circular entre las fotos. Con una sola foto, deshabilitar ambos botones; sin fotos, mostrar un placeholder y deshabilitarlos.
- `CarHeader`: modelo, año, valoración y número de reseñas.
- `SpecsGrid`: plazas, transmisión, combustible y puertas.
- `BookingCard`: precio por día y contador de días en `useState`, inicialmente 1 y limitado a 1–30. Deshabilitar decrementar en 1 e incrementar en 30.
- CTA "Reservar": mostrar una confirmación de demostración con el modelo y los días seleccionados, indicando que no se ha creado una reserva real. No solicitar registro ni pago.
- El botón volver navega siempre a `/fleet` mediante `<Link>`.

## Carpetas
/app (rutas) · /components (UI reutilizable) · /types (interfaces)

## Referencia visual — pendiente antes de implementar
- Capturar una web real de alquiler de coches, por ejemplo Hertz o Enterprise, con un viewport de 375px.
- Adjuntar la captura al agente y pedir una especificación de componentes.
- Documentar aquí la especificación generada antes de codificar la vista correspondiente: distribución, componentes, tipografía, colores, espaciado y adaptación a escritorio.
- Referencia elegida, URL y captura: pendientes.
- Especificación derivada de la captura: pendiente. No considerar completado este requisito hasta incorporar la evidencia.

## Criterios de aceptación
- [ ] Proyecto configurado con Next.js 16, TypeScript, Tailwind y App Router, con las carpetas indicadas.
- [ ] Captura real a 375px y especificación de componentes documentadas antes de implementar la vista.
- [ ] Las tres rutas funcionan y usan el mismo conjunto de datos y la interfaz `Car`.
- [ ] Inicio: búsqueda y categoría se combinan, el chip activo se resalta y "Todos" permite quitar el filtro.
- [ ] `CarGrid` simula la carga con `useEffect` y muestra estados de carga y resultados vacíos.
- [ ] Flota: recuento correcto, orden ascendente/descendente por precio y reutilización de `CarCard`.
- [ ] El placeholder del mapa aparece abajo en móvil y a la derecha en escritorio.
- [ ] Detalle: datos cargados con `useEffect`, galería navegable, cabecera y cuatro especificaciones visibles.
- [ ] Un identificador inexistente muestra un estado claro y permite volver a la flota.
- [ ] El contador no puede salir de 1–30 y el CTA muestra únicamente una confirmación de demostración.
- [ ] Tarjetas y botón volver utilizan `<Link>`; no hay `<a href>` para navegación interna.
- [ ] Todos los componentes se declaran con `const`; no se usan clases ni librerías de componentes prefabricadas.
- [ ] La interfaz funciona a 375px y desde 768px sin solapamientos ni desbordamiento horizontal de la página.

## Origen y decisiones de producto
Referencia: [Ejemplo de interfaz de alquiler de coches](https://github.com/4GeeksAcademy/ai-engineering-syllabus/blob/main/content/projects/nextjs-airbnb-ui-clone/.learn/example/README.es.md).

El documento enlazado es un ejemplo para instructores y omite requisitos secundarios del proyecto oficial. Este contexto cubre ese ejemplo; no garantiza cubrir la rúbrica completa del proyecto del estudiante.

Son decisiones adicionales de RentaCar: datos locales compartidos, búsqueda por modelo combinada con categoría, chip "Todos", orden ascendente inicial, estados vacíos y de identificador inexistente, galería circular con alternativas cuando faltan fotos, botones deshabilitados en los límites y confirmación de reserva de demostración. Estas decisiones concretan comportamientos que el ejemplo no especifica.

## Fuera de alcance
Login, pagos, mapa real, backend.
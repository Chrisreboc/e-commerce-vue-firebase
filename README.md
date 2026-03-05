# CRUD DE PRODUCTOS
El equipo de desarrollo del área de e-commerce necesita una aplicación SPA (Single Page Application) que sirva como catálogo interactivo de productos. Se busca un sistema moderno, dinámico y responsive que permita a los usuarios visualizar información de productos, filtrar por categorías y ver detalles individuales. Actualmente, el equipo utiliza una API interna para exponer los productos, pero no cuentan con una interfaz robusta que consuma esos datos ni gestione el estado de forma centralizada. Además, buscan asegurar la calidad de la solución con pruebas automatizadas y dejar abierta la posibilidad de escalar la aplicación a móvil o escritorio.

## REPOSITORIO Y DEPLOY DEL PROYECTO

Repositorio de Github:
Deploy del Proyecto:

Cabe recordar que para poder hacer uso de la plataforma debes ingresar tus keys y datos a firebaseConfigExample.js.
## DETALLES DEL PROYECTO

El producto se ha adaptado a un sistema de Vue que funciona como SPA, la página no requiere de recargas para funcionar o redireccionar pues tiene elemetnos y vistas dinámicos. además como fuente de almacenamiento se ha usado firebase como database, ideal para almacenar imágenes y variables globales que pueden ser utilizadas a lo largo del proyecto y que posteriormente un cliente puede acceder a ellas.

## ESTRUCTURA

### Vistas Disponibles

Se han generado 4 vistas accesibles como cliente y 1 vista que requiere administrador, las vistas de cliente son:
- HomeView: Posee todo lo principal de la página, los productos divididos por categorías al igual que un slideshow para destacar productos que puedan tener destacados.
- LoginView: Una vista para que un usuario que ya forma parte de la clientela pueda acceder a la página, sus datos de registro siendo guardados en firebese.
- ProductsView: Una vista que que contiene todos los productos y permite filtrar los elementos por nombre o categoría.
Las vistas de administrador son:
- ProductsCrudView: la cual posee un crud de productos, permitiendo añadir nuevos, editar existentes o borrarlos de la base de datos, ideal para manejar los productos en exposición.

### Componentes

Se han creado 5 componentes para evitar el duplicado de código y permitir la reutilización de estos de ser necesario, entre ellos tenemos:
- HeaderComp: este componente posee un título dinámico que permite aplicar el nombre que se desee a la página sin necesidad de repetir su estructura.
- FooterComp: este componente está a pie de página, se evita el duplicado de su código.
- ListProdcuts: este componente contiene cartas agrupadas de productos, se puede utilizar para llamar la lista de productos en diferentes vistas como HomeView y ProductView.
- ProductCard: Este contiene una carta en dónde se hace el display de los productos, imágenes, nombres, descripciones y precios.
- SlideshowComp: Este componente contiene un slideshow, es útilizado una vez en home, pero de ser necesitado se puede utilizar en otras vistas.

### Uso de Pinea

Pinea es utilizado en este proyecto para trabajar funciones y variables globales así estas pueden ser llamadas en todas las vistas que se requiera sin necesidad de hacer excesivo uso de emits y props, esto reduce el código considerablemente y nos permite utilizarlo (en este caso) en 3 vistas, ProductsCrudView, ProductsView, HomeView.

## LIBRERÍAS

Las librerías se implementaron para ahorrar tiempo de desarrollo, entre ellas están Bostrap (mediante CND) y Vuetify, utiliza componentes de ambos, que de por sí ya poseen una estructura modularizada y orientadas a ser componentes, en especial Vuetify.

## CONCLUSIONES

El proyecto busca reflejar como el uso de un framework para proyectos complejos puede optimizar el código, evitar repeticiones y modularizarse para ser mantenible en el tiempo y escalable para crecer según las necesidades que surjan. Además de reducir los tiempos de carga mediante sistemas v-if que permiten el display de vistas o componentes según se requiere sin recargar la página, entregando una mejor experiencia de usuario.
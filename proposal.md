# Propuesta TP DSW

## Grupo
### Integrantes
* 52285 - Gregoret, Agustín
* 53742 - Bolzico, Nicolás
* 53952 - Cabrera, Martín Leonel

---

## Link Monorepo
* https://github.com/TinchoLC/tpDSW-Campaign-Manager-Dev

---

## Tema

### Descripción
Aplicación web para la gestión de campañas de juegos de rol de mesa (TTRPG), permitiendo crear campañas, administrar personajes y gestionar mapas con fichas sobre una grilla bidimensional.  
Facilita la interacción entre jugadores y directores de juego, así como el seguimiento del estado y progreso de los personajes.

---

### Modelo
# Modelo de Dominio / Diagrama de Clases

```mermaid
classDiagram
    direction TB

    class Campana {
        id
        nombre
        descripcion
        imagenUrl
        estado
    }

    class Mapa {
        id
        nombre
        descripcion
        imagenUrl
        estado
    }

    class FichaMapa {
        id
        posicionX
        posicionY
        estado
        imagenUrl
    }

    class CatObjeto {
        id
        nombre
    }

    class Objeto {
        id
        nombre
        descripcion
        imagenUrl
        visibleAJugador
        valor
    }

    class Usuario {
        id
        nombre
        contrasena
        tipo
        imagenUrl
    }

    class Personaje {
        id
        nombre
        descripcion
        imagenUrl
        especie
        armadura
        iniciativa
        velocidad
        vidaMax
        fuerza
        destreza
        constitucion
        inteligencia
        carisma
        sabiduria
    }

    class Jugable {
        nivel
        manaMax
        clase
        experiencia
        oro
    }

    class NoJugable {
        estadoAgresion
        vidaActual
    }

    class Rasgo {
        id
        nombre
        visibleAJugador
        descripcion
        nivelMinimo
    }

    class Accion {
        id
        nombre
        visibleAJugador
        descripcion
        costoMana
    }

    class PersonajeCampana {
        id
        nombre
        armadura
        iniciativa
        velocidad
        vidaMax
        vidaActual
        nivel
        oro
        experiencia
        clase
        manaMax
        manaActual
        fuerza
        destreza
        constitucion
        sabiduria
        carisma
        inteligencia
        estado
    }

    %% Relaciones
    Campana "1" *-- "0..*" Mapa : contiene >
    Mapa "1" -- "0..*" FichaMapa : tiene >

    FichaMapa "0..*" -- "0..1" NoJugable : representa >
    FichaMapa "0..*" -- "0..1" PersonajeCampana : representa >

    CatObjeto "1" o-- "0..*" Objeto
    Objeto "0..*" -- "0..*" Personaje : tiene (Cantidad) >

    Personaje <|-- Jugable
    Personaje <|-- NoJugable

    Personaje "0..*" -- "0..*" Rasgo : tiene >
    Personaje "0..*" -- "0..*" Accion : tiene >

    Campana "1" -- "0..*" NoJugable : contiene >

    Usuario "1" -- "0..*" Campana : crea >
    Usuario "1" -- "0..*" Jugable : crea >

    Campana "1" -- "0..*" PersonajeCampana : tiene >
    Usuario "1" -- "0..*" PersonajeCampana : controla >
    Jugable "1" -- "0..*" PersonajeCampana : instancia >

    PersonajeCampana "0..*" -- "0..*" Objeto : tiene >
    PersonajeCampana "0..*" -- "0..*" Rasgo : tiene >
    PersonajeCampana "0..*" -- "0..*" Accion : tiene >
```

## Alcance Funcional 

### Alcance Mínimo

Regularidad:

|Req|Detalle|
|:-|:-|
|CRUD simple|1. CRUD Usuario<br>2. CRUD Campaña<br>3. CRUD Rasgo|
|CRUD dependiente|1. CRUD Personaje {depende de} CRUD Usuario<br>2. CRUD Participación (PersonajeCampana) {depende de} CRUD Usuario, CRUD Jugable y CRUD Campaña|
|Listado<br>+<br>detalle|1. Listado de campañas filtrado por estado, muestra nombre, master e imagen => detalle permite unirse y entrar a la sesión<br>2. Listado de NPC filtrado por estadoAgresión, muestra nombre y descripción => detalle con todos los atributos|
|CUU/Epic|1. CUU Crear mapa para una campaña asignando imagen y dimensiones de grilla<br>2. CUU Posicionar y mover fichas de personajes o NPCs en la grilla bidimensional del mapa|

---

### Adicionales para Aprobación

|Req|Detalle|
|:-|:-|
|CRUD |1. CRUD Usuario<br>2. CRUD Campaña<br>3. CRUD Participación (PersonajeCampana)<br>4. CRUD Personaje<br>5. CRUD Jugable<br>6. CRUD NoJugable<br>7. CRUD Objeto<br>8. CRUD CatObjeto<br>9. CRUD Rasgo<br>10. CRUD Acción<br>11. CRUD Mapa|
|CUU/Epic|1. Crear un personaje con atributos, habilidades, estadísticas y objetos<br>2. Sistema de chat entre jugadores en partida.<br>3. Consultar estado y posición de fichas de personajes en el mapa activo<br>4. Modificar atributos y gestionar objetos durante la campaña|

---

## Alcance Adicional Voluntario

|Req|Detalle|
|:-|:-|
|Listados |1. Listado de objetos de un personaje filtrado por categoría, muestra nombre y valor => detalle con descripción e imagen|
|CUU/Epic|1. Invitar usuario a campaña privada y confirmar ingreso<br>2. Asignar personaje a campaña|
|Otros|1. Envío de invitación o notificación por email<br>2. Sistema de dados que puede utilizarse en campaña y escribe resultado por el chat|

#  Dragonlandia
## Introducción

Un juego muy simplón de dragones y hechizos.

## Analisis
El análsis de dragolandia

## Diseño
### Diagrama de Entidad relación
```mermaid
erDiagram
    MAGOS {
        int id PK
        string nombre
        int vida
        int nivel_magia
    }
    HECHIZOS {
        int id PK
        string descripcion
        TipoHechizo nombre
    }
    MONSTROS {
        int id PK
        string nombre
        int fuerza
        int vida
        TipoMonstro tipo
    }
    BOSQUES {
        int id PK
        string nombre
        int monstroJefe_id FK
    }
    DRAGONES {
        string nombre PK
        int intensidad_fuego
        int resistencia
    }
    MAGOS_HECHIZOS {
        int mago_id FK
        int hechizo_id FK
    }
    BOSQUES_MONSTROS {
        int bosque_id FK
        int monstro_id FK
    }

    MAGOS ||--o{ MAGOS_HECHIZOS : ""
    HECHIZOS ||--o{ MAGOS_HECHIZOS : ""
    BOSQUES ||--o{ BOSQUES_MONSTROS : ""
    MONSTROS ||--o{ BOSQUES_MONSTROS : ""
    BOSQUES }o--|| MONSTROS : "monstroJefe"
```
### Diagrama de clase

```mermaid
classDiagram
    class Mago {
        int id
        String nombre
        int vida
        int nivelMagia
        List~Hechizo~ conjuros
        lanzarHechizo(Hechizo)
        lanzarHechizo(Hechizo, Monstro)
    }
    class Hechizo {
        int id
        TipoHechizo nombre
        String descripcion
        efecto(Monstro)
    }
    class Monstro {
        int id
        int fuerza
        String nombre
        TipoMonstro tipo
        int vida
    }
    class Bosque {
        int id
        String nombre
        Monstro monstroJefe
        List~Monstro~ monstros
    }
    class Dragon {
        String nombre
        int intensidadFuego
        int resistencia
    }
    class TipoHechizo {
        <<enum>>
        BOLA_DE_FUEGO
        RAYO_CONGELANTE
        TORMENTA_ELECTRICA
    }
    class TipoMonstro {
        <<enum>>
        OGRO
        TROLL
        ESPECTRO
    }

    Bosque "1" -- "0..1" Monstro : monstroJefe
    Bosque "many" -- "many" Monstro : monstros
    Hechizo o-- TipoHechizo
    Monstro o-- TipoMonstro
```
## Ampliaciones y Mejoras.

Inicialmente para ampliar el juego y mejorarlo se debe de replantear enteramente el flujo del programa, teniendo en cuenta que un ORM no es la herramienta mas adecuada para la gestión de un juego. 

### Patrones a implementar

Uno de los primeros patrones a implementar es el [patrón de diseño de estado](https://refactoring.guru/design-patterns/state). Un patrón de diseño que permitirá manejar el estado del juego. Un sencillo diagrama explicativo:

```mermaid
flowchart LR
    A(Inicio)--> B{Seleccion de turno}
    B --> C(Turno A)
    C --> D(Turno B)
    D --> C
    D --> E(Fin del juego)
```

De esta manera se puede controlar bien el estado del juego y se evita comportamiento indefinido. Además de que habilita la posiblidad de expansión a otros tipos de estados. 

### Jerarquia y abstracción

Deberia de abstraerse las clases `Monstro`, `Dragon` y `Mago` a una superclase con atributos comunes e interfaces requeridas.
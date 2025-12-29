

# Tutorial ejercicio individual ArrayList con Git

Vamos a **desarrollar un ejercicio en Java paso a paso** mientras **aprendemos Git por consola**.  
Por esta vez evitaremos utilizar las herramientas de integración con Git que tiene IntelliJ.
Recuerda inicializar Git en tu sesión de usuario. Si no lo hiciste ya, revisa lo dispuesto en [[1 Configuración básica de Git]].

## FASE 1 – Crear repositorio local y primera versión

### 1.1 Crear proyecto Java en IntelliJ

- New Project → Java → Nombre: `GestorDeudas`.

Si IntelliJ detecta que tienes Git instalado en tu ordenador, te aparecerá un check bajo el nombre del proyecto. No lo pulsaremos para hacer el comando equivalente por consola.

![[Pasted image 20251208121344.png]]

Una vez generado el proyecto Java observa el fichero .gitignore con el siguiente contenido. Este fichero marca los archivos que no se subirán al repositorio.

```txt
### IntelliJ IDEA ###  
out/  
!**/src/main/**/out/  
!**/src/test/**/out/  
  
### Eclipse ###  
.apt_generated  
.classpath  
.factorypath  
.project  
.settings  
.springBeans  
.sts4-cache  
bin/  
!**/src/main/**/bin/  
!**/src/test/**/bin/  
  
### NetBeans ###  
/nbproject/private/  
/nbbuild/  
/dist/  
/nbdist/  
/.nb-gradle/  
  
### VS Code ###  
.vscode/  
  
### Mac OS ###  
.DS_Store
```

Así, no guardaremos en el Git todos los archivos que IntelliJ genera para la gestión del proyecto, y para sus compilados.

**Como ya sabemos IntelliJ genera un fichero Main.java a efectos demostrativos. Como no lo necesitamos en este proyecto procedemos a borrarlo desde el IDE.**
### 1.2 Inicializar Git (por consola)
```bash
# Moverse a la carpeta raíz del proyecto
cd ruta/al/GestorDeudas

# Iniciar repo
git init
```

Con esto hemos generado el Entorno de Trabajo - Zona 1 (Working Directory) con el contenido actual de nuestro Proyecto.

### 1.3 Primer commit – estructura mínima
```bash
# Ver estado actual
git status
```

![[Pasted image 20251208122217.png]]

```bash 
git add .
git status
```

![[Pasted image 20251208122416.png]]

Ahora todos estos archivos se encuentran en la Zona 2, o Zona de Preparación (Staging Area)

```bash
git commit -m "Añade esqueleto del proyecto"
```

![[Pasted image 20251208122512.png]]

Ahora, todos esos archivos se encuentran en el Repositorio (Zona 3).

---

## FASE 2 – Añadir menú y métodos vacíos

### 2.1 Crear clase `GestorDeudas.java`

Creamos la clase GestorDeudas. Si trabajamos con IntelliJ y la carpeta es un proyecto GIT, cada vez que creamos una clase nos saldrá una diálogo de este tipo:

![[Pasted image 20251208123844.png]]

De momento elegiremos la opción Cancel, y más tarde añadiremos el fichero por consola.

En esta primera versión de `GestorDeudas` solo vamos a programar el esqueleto del menú, y una serie de comentarios que indican el trabajo pendiente. Conforme resolvemos cada una de estas tareas, eliminaremos el comentario y actualizaremos nuestra versión.

```java
import java.util.ArrayList;
import java.util.Scanner;

public class GestorDeudas {

    public static void main(String[] args) {
        ArrayList<String> clientes = new ArrayList<>();
        ArrayList<Double> deudas   = new ArrayList<>();
        Scanner sc = new Scanner(System.in);

        char opc;
        do {
            System.out.println("\n--- MENÚ ---");
            System.out.println("A) Añadir cliente");
            System.out.println("E) Eliminar cliente");
            System.out.println("D) Consultar listado");
            System.out.println("C) Consultar saldo cliente");
            System.out.println("M) Modificar deuda");
            System.out.println("S) Salir");
            System.out.print("Elige: ");
            opc = sc.nextLine().toUpperCase().charAt(0);

            switch (opc) {
                case 'A': {
                    /* TODO: añadir cliente */
                    break;
                }
                case 'E': {
                    /* TODO: eliminar cliente */
                    break;
                }
                case 'D': {
                    /* TODO: listar deudas */
                    break;
                }
                case 'C': {
                    /* TODO: consultar cliente */
                    break;
                }
                case 'M': {
                    /* TODO: modificar deuda */
                    break;
                }
                case 'S':
                    System.out.println("¡Hasta luego!");
                    break;
                default:
                    System.out.println("Opción no válida.");
            }
        } while (opc != 'S');

    }
}
       
```

### 2.2 Segundo `commit`


```bash
git status          # Aparecera src/ como archivo sin seguimiento
git add src/
git status         # Aparecerá src/GestorDeudas.java como archivo nuevo
git commit -m "Esqueleto Completo: menú y bloques TODO en main"
```

---

## FASE 3 – Programación Opciones

### 3.1 Implementar cada opción en commits separados

#### Opción A – Método añadir cliente
```java
// Dentro de la clase, fuera de main
public static void anyadirCliente(ArrayList<String> clientes,
                                  ArrayList<Double> deudas,
                                  Scanner sc) {
    System.out.print("Nombre: ");
    String nombre = sc.nextLine();
    if (clientes.contains(nombre)) {
        System.out.println("Error: cliente ya existe.");
        return;
    }
    System.out.print("Deuda inicial: ");
    double deuda = Double.parseDouble(sc.nextLine());
    clientes.add(nombre);
    deudas.add(deuda);
    System.out.println("Cliente añadido.");
}
```

En `main`, dentro del `case 'A'`:

```java
case 'A':
    anyadirCliente(clientes, deudas, sc);
    break;
```

Commit:
```bash
git add src/
git commit -m "Implementa opción A: añadir cliente"
```

El procedimiento se va a repetir para cada una de las funciones de este problema


#### Opción D – Listar deudas
```java
public static void listarDeudas(ArrayList<String> clientes,
                                 ArrayList<Double> deudas) {
    if (clientes.isEmpty()) {
        System.out.println("No hay clientes.");
        return;
    }
    double total = 0;
    System.out.println("\n--- ESTADO DE DEUDAS ---");
    System.out.printf("%-15s %10s%n", "CLIENTE", "DEUDA");
    System.out.println("----------------------");
    for (int i = 0; i < clientes.size(); i++) {
        System.out.printf("%-15s %10.2f €%n", clientes.get(i), deudas.get(i));
        total += deudas.get(i);
    }
    System.out.println("----------------------");
    System.out.printf("TOTAL DEUDA: %10.2f €%n", total);
}
```

En `main`, dentro del `case 'D'`:

```java
case 'D':
    listarDeudas(clientes, deudas);
    break;
```

`Add` y `Commit` todo en uno:
```bash
git commit -am "Implementa opción D: listar deudas"
```

Dado que solo estamos modificando el archivo GestorDeudas.java, podemos usar `git commit -am` para ahorrar el paso intermedio de `git add`.


#### Opción C – Consultar saldo de un cliente
```java
public static void consultarCliente(ArrayList<String> clientes,
                                     ArrayList<Double> deudas,
                                     Scanner sc) {
    System.out.print("Nombre: ");
    String nombre = sc.nextLine();
    int idx = clientes.indexOf(nombre);
    if (idx == -1) {
        System.out.println("Cliente no encontrado.");
    } else {
        System.out.printf("%s debe %.2f €%n", nombre, deudas.get(idx));
    }
}
```

En `main`, dentro del `case 'C'`:

```java
case 'C':
    consultarCliente(clientes, deudas, sc);
    break;
```

```bash
git commit -am "Implementa opción C: consultar saldo cliente"
```

#### Opción E – Eliminar cliente
```java
public static void eliminarCliente(ArrayList<String> clientes,
                                    ArrayList<Double> deudas,
                                    Scanner sc) {
    System.out.print("Nombre: ");
    String nombre = sc.nextLine();
    int idx = clientes.indexOf(nombre);
    if (idx == -1) {
        System.out.println("Cliente no encontrado.");
    } else {
        clientes.remove(idx);
        deudas.remove(idx);
        System.out.println("Cliente eliminado.");
    }
}
```

En `main`, dentro del `case 'E'`:

```java
case 'E':
    eliminarCliente(clientes, deudas, sc);
    break;
```

```bash
git commit -am "Implementa opción E: eliminar cliente"
```

#### Opción M – Modificar deuda
```java
public static void modificarDeuda(ArrayList<String> clientes,
                                   ArrayList<Double> deudas,
                                   Scanner sc) {
    System.out.print("Nombre: ");
    String nombre = sc.nextLine();
    int idx = clientes.indexOf(nombre);
    if (idx == -1) {
        System.out.println("Cliente no encontrado.");
        return;
    }
    System.out.print("Nueva deuda: ");
    double nueva = Double.parseDouble(sc.nextLine());
    deudas.set(idx, nueva);
    System.out.println("Deuda actualizada.");
}
```

En `main`, dentro del `case 'M'`:

```java
case 'M':
    modificarDeuda(clientes, deudas, sc);
    break;
```

```bash
git commit -am "Implementa opción M: modificar deuda"
```

#### Historial de commits

Los commits han de quedar de la siguiente manera:

```bash
git log --oneline
5f6g7h8 Implementa opción M: modificar deuda
4e5f6g7 Implementa opción E: eliminar cliente
3d4e5f6 Implementa opción C: consultar saldo cliente
2c3d4e5 Implementa opción D: listar deudas
1b2c3d4 Implementa opción A: añadir cliente
0a1b2c3 Esqueleto completo: menú y bloques TODO en main
be1c2d4 Añade esqueleto del proyecto
```
---

## FASE 4 – Deshacer un cambio “accidental”

### 4.1 Simular error
Un alumno borra sin querer el método `listarDeudas()` y guarda en IntelliJ

### 4.2 Ver diferencias
```bash
git diff          # Muestra las línesa borradas en rojo
```

### 4.3 Recuperar versión del repositorio
```bash
git restore src/GestorDeudas.java
```


---

## FASE 5 – Subir el proyecto a GitHub

### 5.1 Crear repositorio en GitHub

1. Ir a [https://github.com](https://github.com)
2. Hacer clic en **"New repository"**
3. Rellenar:
   - **Repository name**: `GestorDeudas`
   - **Description**: (opcional) *Gestor de deudas en Java con Git por consola*
   - **Public** o **Private** (según prefieras)
   - **NO** marcar "Initialize this repository with a README" (ya tenemos nuestro propio historial)

4. Hacer clic en **"Create repository"**

GitHub te mostrará una pantalla con instrucciones. Vamos a usar la opción **"…or push an existing repository from the command line"**.

---

### 5.2 Conectar repositorio local con GitHub

Desde la terminal, dentro de la carpeta del proyecto:

```bash
# Añadir el remoto (sustituye TU_USUARIO por tu nombre de usuario)
git remote add origin https://github.com/TU_USUARIO/GestorDeudas.git

# Verificar que se ha añadido correctamente
git remote -v
```

---

### 5.3 Subir los commits al remoto

```bash
# Subir la rama master (o main, según tu versión de Git).
git branch ## Consulta como se llama la rama main o  master
git push -u origin master
```

> Si es la primera vez que usas GitHub desde la terminal, puede pedirte autenticación.  
> Git ahora usa **token de acceso personal (PAT)** en lugar de contraseña.  
> Puedes generar uno en:  
> [https://github.com/settings/tokens](https://github.com/settings/tokens)

Los tokens de grano fino nos van a permir generar un token para un repositorio concreto. En nuestro caso, como queremos más adelante hacer commits desde nuestro PC, hay que acordarse de elegir el repositorio correcto y añadir los permisos sobre Contenido, tanto en modo lectura como en modo escritura.
Los tokens clasicos nos van a servir para autenticar en cualquiera de nuestros repositorios. Tenemos que elegir, por los menos los permisos del bloque repo.

Asi, cuando hagamos por primera vez un `git push` nos pedirá nuestro nombre de usuario y nuestra contraseña, en este caso no es la contraseña de nuestra cuenta de github sino el token generado.

Los tokens tienen caducidad y se pueden regenerar, cuando queremos incorporar el repositorio en otro equipo de desarrollo. 

---

### 5.4 Verificar en GitHub

Recarga la página del repositorio en GitHub. Deberías ver:
- Todos los archivos del proyecto
- El historial de commits (`git log`)
- El `.gitignore` correctamente aplicado

---

### 5.5 Simular un cambio desde otro entorno

Cuando varias personas editan nuestro proyecto de Github llegará el momento que necesitemos llevar los cambios de los demás participantes a nuestro entorno de desarrollo. Vamos a simular un cambio de otro usuario, mediante una modificación desde GitHub.

#### 5.5.1 Hacer un cambio pequeño desde GitHub
1. En GitHub, editar el archivo `GestorDeudas.java`
2. Añadir un comentario al inicio:
   ```java
   // Proyecto: GestorDeudas - v1.0
   ```
3. Hacer `commit` con el mensaje: `"Añade comentario de versión"`

#### 5.5.2 Traer cambios al local
```bash
git pull origin master
```

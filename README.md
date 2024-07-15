# SQL
Dominando SQL: Fundamentos para el éxito - Caso de Uso

**Problemática**: Una empresa de **paquetería** desea desarrollar un sistema de gestión de envíos. La base de datos debe manejar información sobre clientes, paquetes, envíos, rutas y el estado de los envíos. A continuación, se describen los requisitos de la base de datos y las tareas a realizar:

**Requisitos Base de Datos**

**Clientes:** Cada cliente tiene un identificador único, nombre, dirección, teléfono y correo electrónico.

**Paquetes:** Cada paquete tiene un identificador único, peso, dimensiones (largo, ancho, alto), y una descripción.

**Envíos:**  Cada envío tiene un identificador único, una fecha de envío, una fecha estimada de entrega, una dirección de destino, un estado (en tránsito, entregado, retrasado), y un cliente asociado.

**Rutas:** Cada ruta tiene un identificador único, un nombre, y una descripción.

**Detalles de Envío:** Cada detalle de envío registra la asociación de un paquete con un envío, incluyendo el identificador del paquete, el identificador del envío y la posición del paquete en el envío.

**Tareas a Realizar**

- [x]  Crear una nueva Base de Datos modificando el archivo “docker-compose.yml” para configurar el entorno de PostgreSQL.
    - *Cree la BS en “Visual Studio Code y revisé que se reflejara en Docker*
    
    ```sql
    version: "3"
    
    services:
      PaqueteriaDB:
        image: postgres:15.7
        container_name: mypaqueteria
        restart: always
        ports: 
          - 5437:5432
        environment:
          - POSTGRES_USER=IvanF
          - POSTGRES_PASSWORD=123123
          - POSTGRES_DB=PaqueteriaDB
        volumes:
          - ./postgresPaqueteriaDB:/var/lib/postgresql/data
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/56c30fbc-41f8-48b4-8ed1-fa82eb8b09ad/Untitled.png)
    
    - Se conectó exitosamente a TablePlus
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/8f1ea7c0-f2ac-47a4-807b-b9212a741581/Untitled.png)
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/f4444531-d41a-4578-ba01-5bf086b02648/Untitled.png)
    
    [Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/197f97bb-0f85-4b74-9c7c-84896cb1f5ef/Untitled.txt)
    
- [x]  Crear las tablas de datos de:
    - **Gits para la construcción de tablas**
        
        https://gist.github.com/CarlosDev3119/d1ae4e1b8a7c1d554e755da675059ea0 
        
        ```sql
        ##Ejemplo de tablas
        -- Tabla Clientes
        CREATE TABLE clientes (
            id_cliente SERIAL PRIMARY KEY,
            nombre VARCHAR(100) NOT NULL,
            direccion TEXT NOT NULL,
            telefono VARCHAR(20),
            email VARCHAR(100)
        );
        
        -- Tabla Paquetes
        CREATE TABLE paquetes (
            id_paquete SERIAL PRIMARY KEY,
            peso DECIMAL(10, 2) NOT NULL,
            dimensiones VARCHAR(50) NOT NULL,
            descripcion TEXT
        );
        
        -- Tabla Envios
        CREATE TABLE envios (
            id_envio SERIAL PRIMARY KEY,
            fecha_envio DATE NOT NULL,
            fecha_estimada_entrega DATE,
            direccion_destino TEXT NOT NULL,
            estado VARCHAR(50) NOT NULL,
            id_cliente INT REFERENCES clientes(id_cliente)
        );
        
        -- Tabla Rutas
        CREATE TABLE rutas (
            id_ruta SERIAL PRIMARY KEY,
            nombre VARCHAR(100) NOT NULL,
            descripcion TEXT
        );
        
        -- Tabla Detalles de Envío
        CREATE TABLE detalles_envio (
            id_detalle SERIAL PRIMARY KEY,
            id_envio INT REFERENCES envios(id_envio),
            id_paquete INT REFERENCES paquetes(id_paquete),
            posicion INT NOT NULL
        );
        
        -- Tabla de Auditoría
        CREATE TABLE registro_envios (
            id_registro SERIAL PRIMARY KEY,
            id_envio INT,
            fecha_registro TIMESTAMP
        );
        ```
        
    - [x]  Clientes.
    
    ```sql
    CREATE TABLE clientes (
        id_cliente SERIAL PRIMARY KEY,
        nombre VARCHAR(100) NOT NULL,
        direccion TEXT NOT NULL,
        telefono VARCHAR(20),
        email VARCHAR(100)
    );
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/2c2ad763-e3c5-491f-bd52-34888a9be610/Untitled.png)
    
    - [x]  Paquetes
        
        ```sql
        CREATE TABLE paquetes (
            id_paquete SERIAL PRIMARY KEY,
            peso DECIMAL(10, 2) NOT NULL,
            dimensiones VARCHAR(50) NOT NULL,
            descripcion TEXT
        );
        ```
        
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/28724887-1a0a-4047-8f95-182d7b7e289e/Untitled.png)
    
    - [x]  Envíos
    
    ```sql
    CREATE TABLE envios (
        id_envio SERIAL PRIMARY KEY,
        fecha_envio DATE NOT NULL,
        fecha_estimada_entrega DATE,
        direccion_destino TEXT NOT NULL,
        estado VARCHAR(50) NOT NULL,
        id_cliente INT REFERENCES clientes(id_cliente)
    );
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/6cfd0450-8e60-4f75-a26e-ce8bb92b9350/Untitled.png)
    
    - [x]  Rutas
        
        ```sql
        CREATE TABLE rutas (
            id_ruta SERIAL PRIMARY KEY,
            nombre VARCHAR(100) NOT NULL,
            descripcion TEXT
        );
        ```
        
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/f19ad915-5f39-42e5-bd34-78e4a590a824/Untitled.png)
    
    - [x]  Detalle Envío
    
    ```sql
    CREATE TABLE detalles_envio (
        id_detalle SERIAL PRIMARY KEY,
        id_envio INT REFERENCES envios(id_envio),
        id_paquete INT REFERENCES paquetes(id_paquete),
        posicion INT NOT NULL
    );
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/ab5db711-f935-4224-aa59-52b945009285/Untitled.png)
    
    - [x]  Auditoria
    
    ```sql
    CREATE TABLE registro_envios (
        id_registro SERIAL PRIMARY KEY,
        id_envio INT,
        fecha_registro TIMESTAMP
    );
    ```
    

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/a25b3566-493b-44cb-afa5-7e26231a0093/Untitled.png)

- [x]  Definir las llaves primarias y foráneas para mantener la integridad referencial.

**Modelo relacional**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/c539d5c2-8dbb-409e-bb3f-1b97178bf4a6/Untitled.png)

### Descripción de las Relaciones

- **clientes** a **envíos**: Un cliente puede tener muchos envíos (`id_cliente` en `envios` es una clave foránea que referencia a `clientes`). → Uno a muchos.
- **envios** a **detalles_envio**: Un envío puede contener muchos detalles de envíos (`id_envio` en `detalles_envio` es una clave foránea que referencia a `envios`). → Uno a muchos.
- **paquetes** a **detalles_envio**: Un paquete puede estar en muchos detalles de envíos (`id_paquete` en `detalles_envio` es una clave foránea que referencia a `paquetes`). → Uno a muchos.
- **envios** a **registro_envios**: Un envío puede tener muchos registros de envíos (`id_envio` en `registro_envios` es una clave foránea que referencia a `envios`).→ Uno a muchos.

**Definición de laves primarias y foráneas.** 

**Clave foránea:**  olumna o un grupo de columnas en una tabla que establece un vínculo entre los datos en dos tablas.

```sql
-- AGREGAR CLAVES FOREANEAS
-- Agregar llave foránea a la tabla envios
ALTER TABLE
    envios
ADD
    CONSTRAINT fk_cliente FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente);

-- Agregar llaves foráneas a la tabla detalles_envio
ALTER TABLE
    detalles_envio
ADD
    CONSTRAINT fk_envio FOREIGN KEY (id_envio) REFERENCES envios(id_envio);

ALTER TABLE
    detalles_envio
ADD
    CONSTRAINT fk_paquete FOREIGN KEY (id_paquete) REFERENCES paquetes(id_paquete);

-- Agregar llave foránea a la tabla registro_envios
ALTER TABLE
    registro_envios
ADD
    CONSTRAINT fk_registro_envio FOREIGN KEY (id_envio) REFERENCES envios(id_envio);
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/04d69dbd-debc-472e-a11c-c8f5459f0437/Untitled.png)

- [x]  Normaliza las tablas según las tres primeras formas normales.
    
    <aside>
    📖 **Formas Normales:**
    
    - **Primera Forma Normal (1FN)**:
        - Asegúrate de que cada columna contenga solo valores atómicos.
        - Elimina cualquier fila duplicada.
    - **Segunda Forma Normal (2FN)**:
        - La tabla debe estar en 1FN.
        - Todos los atributos no clave deben depender completamente de la clave principal.
    - **Tercera Forma Normal (3FN)**:
        - La tabla debe estar en 2FN.
        - No debe haber dependencias transitivas, es decir, ningún atributo no clave debe depender de otro atributo no clave.
    </aside>
    
- [x]  **Triggers:** Crea un trigger que registre la inserción de nuevos envíos en una tabla de auditoría registro_envios.
    
    <aside>
    📖 *Un "trigger" (o disparador) en el contexto de bases de datos es un tipo de objeto que permite ejecutar automáticamente una serie de acciones (como una sentencia SQL) en respuesta a ciertos eventos que ocurren en una tabla o vista. Estos eventos pueden incluir operaciones de inserción, actualización o eliminación de datos.*
    
    </aside>
    
    **Creación de la función**
    
    ```sql
    -- Triggers
    -- Paso 1: Crear la función del disparador
    CREATE
    OR REPLACE FUNCTION registrar_insercion_envio() RETURNS TRIGGER AS $$ 
    BEGIN
    INSERT INTO
        registro_envios (id_envio, fecha_registro)
    VALUES
        (NEW.id_envio, NOW());
    RETURN NEW;
    END;
    $ $ LANGUAGE plpgsql;
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/79b1ea0e-3ff2-4c6c-a20f-50a1badb617e/Untitled.png)
    
    **Creación del Tigger**
    
    ```sql
    -- Paso 2: Crear el trigger
    CREATE TRIGGER trigger_registrar_insercion_envio
    AFTER INSERT ON envios
    FOR EACH ROW
    EXECUTE FUNCTION registrar_insercion_envio();
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/7634c673-0111-4e88-8228-3e43e2012e6f/Untitled.png)
    

- [x]  **Stored Procedures:** Crea un stored procedure que, dado un id_cliente, retorne todos los envíos asociados a ese cliente.
    
    **Stored Procedures:** (Procedimiento Almacenado) es un conjunto de instrucciones SQL que se almacenan y se ejecutan en la base de datos. 
    
    - Para crear un procedimiento almacenado en SQL, debes:
        1. Definir el nombre del procedimiento y sus parámetros.
        2. Especificar el tipo de valor que devolverá.
        3. Delimitar el bloque de código de la función.
        4. Escribir la lógica del procedimiento dentro del bloque `BEGIN ... END`.
        5. Especificar el lenguaje de procedimiento.
    
    **Crear Función**
    
    ```sql
    -- Stored Procedures
    -- Crear función
    CREATE OR REPLACE FUNCTION obtener_envios_por_cliente(p_id_cliente INT)
    RETURNS TABLE(
        id_envio INT,
        fecha_envio DATE,
        fecha_estimada_entrega DATE,
        direccion_destino TEXT,
        estado VARCHAR(50)
    ) AS $$
    BEGIN
        RETURN QUERY
        SELECT
            envios.id_envio,
            envios.fecha_envio,
            envios.fecha_estimada_entrega,
            envios.direccion_destino,
            envios.estado
        FROM
            envios
        WHERE
            envios.id_cliente = p_id_cliente;
    END;
    $$ LANGUAGE plpgsql;
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/286b0507-9719-4afa-8441-9acf1dc63e68/Untitled.png)
    
    **Función**
    
    ```sql
    -- Uso de la función
    SELECT * FROM obtener_envios_por_cliente(1);
    
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/fb01375e-bacd-4298-be6a-7baab8bcf8db/Untitled.png)
    

- [x]  **Functions:** Crea una función que calcule el tiempo total de envío de un paquete, dado un id_envio.
    
    **Functions:** son bloques de código que realizan una tarea específica y devuelven un valor. Son similares a los procedimientos almacenados (stored procedures) pero se diferencian en que las funciones siempre devuelven un valor (o un conjunto de valores) y se pueden usar directamente en las consultas SQL como si fueran columnas.
    
    ```sql
    -- Functions
    -- Crear función para calcular el tiempo total de envío de un paquete
    CREATE OR REPLACE FUNCTION calcular_tiempo_envio(p_id_envio INT)
    RETURNS INTERVAL AS $$
    DECLARE
        fecha_envio DATE;
        fecha_estimada_entrega DATE;
        tiempo_envio INTERVAL;
    BEGIN
    
        -- Obtener las fechas de envío y de entrega estimada
        SELECT e.fecha_envio, e.fecha_estimada_entrega
        INTO fecha_envio, fecha_estimada_entrega
        FROM envios e
        WHERE e.id_envio = p_id_envio;
    
        -- Calcular el intervalo de tiempo entre las dos fechas
        IF fecha_estimada_entrega IS NOT NULL THEN
            tiempo_envio := fecha_estimada_entrega - fecha_envio;
        ELSE
            tiempo_envio := NULL;
        END IF;
    
        RETURN tiempo_envio;
    END;
    $$ LANGUAGE plpgsql;
    
    -- Uso de la función
    SELECT calcular_tiempo_envio(1);
    
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/36a34c77-851a-4ad4-847d-c515569280a7/Untitled.png)
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/0fc6d841-9ef4-4437-9ec8-6824616fb9be/Untitled.png)
    

- [x]  **Consultas Avanzadas:**
    
    **Consultas avanzadas**: consultas SQL que van más allá de las operaciones básicas de recuperación de datos simples. Estas consultas implican el uso de funciones más complejas, operaciones de agrupamiento y filtrado sofisticadas, combinación de múltiples tablas, uso de subconsultas, vistas, y en algunos casos, la utilización de funciones analíticas y operaciones con conjuntos.
    
    - [x]  Escribe una consulta que muestre los clientes que han realizado más de 5 envíos en el último mes.
    
    ```sql
    --CONSULTAS AVANZADAS
    -- Consulta para mostrar los clientes que han realizado más de 5 envíos en el último mes
    CREATE OR REPLACE VIEW clientes_mas_de_5_envios_ultimo_mes AS
    SELECT 
        c.id_cliente,
        c.nombre,
        c.direccion,
        c.telefono,
        c.email,
        COUNT(e.id_envio) AS cantidad_envios
    FROM 
        clientes c
    JOIN 
        envios e ON c.id_cliente = e.id_cliente
    WHERE 
        e.fecha_envio >= CURRENT_DATE - INTERVAL '1 month'
    GROUP BY 
        c.id_cliente, c.nombre, c.direccion, c.telefono, c.email
    HAVING 
        COUNT(e.id_envio) > 5;
    
    -- Consulta más de 5 envíos
    SELECT * FROM clientes_mas_de_5_envios_ultimo_mes;
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/2ebfa8df-a17f-4889-aa10-27bc7786436c/Untitled.png)
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/6d381bba-d3d6-459c-ae79-ec51e48c0ad7/Untitled.png)
    
    - [x]  Escribe una consulta que muestre el promedio de tiempo de entrega de los envíos completados.
        
        ```sql
        -- Consulta para calcular el promedio de tiempo de entrega de los envíos completados
        CREATE OR REPLACE VIEW promedio_tiempo_entrega_completados AS
        SELECT 
            AVG(fecha_estimada_entrega - fecha_envio) AS promedio_tiempo_entrega
        FROM 
            envios
        WHERE 
            estado = 'completado' AND
            fecha_estimada_entrega IS NOT NULL;
        
        -- Consultar promedio de entrega
        SELECT * FROM promedio_tiempo_entrega_completados;
        ```
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/b7622379-91cd-4e65-b9b1-72b866cd8d87/Untitled.png)
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/26b65b22-4f6b-4f1a-aa21-8774f90da936/Untitled.png)
        
    
- [x]  **Vistas:** Las vistas en SQL son tablas virtuales que muestran datos seleccionados de una o más tablas existentes en la base de datos. No almacenan datos físicamente como lo hacen las tablas regulares, sino que son consultas guardadas que se ejecutan dinámicamente cuando se accede a ellas.
    - [x]  Crea una vista que muestre todos los envíos junto con la información del cliente y el estado del envío.
        
        ```sql
        -- VISTAS
        -- Crear la vista que muestra todos los envíos junto con la información del cliente y el estado del envío
        CREATE OR REPLACE VIEW envios_con_informacion_cliente AS
        SELECT 
            e.id_envio,
            e.fecha_envio,
            e.fecha_estimada_entrega,
            e.direccion_destino,
            e.estado,
            c.id_cliente,
            c.nombre AS nombre_cliente,
            c.direccion AS direccion_cliente,
            c.telefono,
            c.email
        FROM 
            envios e
        JOIN 
            clientes c ON e.id_cliente = c.id_cliente;
        
        -- Consiltar vista de todos los envíos e información de un clientes
        SELECT * FROM envios_con_informacion_cliente;
        ```
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/b7067ae2-7865-4285-8723-3d351279b748/Untitled.png)
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/ac7705b4-e2cf-4785-aef7-f311e47a4c5e/Untitled.png)
        
    - [x]  Crea una vista que muestre los envíos entregados con su tiempo total de envío.
        
        ```sql
        -- Crear la vista que muestra los envíos entregados con su tiempo total de envío
        CREATE OR REPLACE VIEW envios_entregados_con_tiempo_total AS
        SELECT 
            e.id_envio,
            e.fecha_envio,
            e.fecha_estimada_entrega,
            e.direccion_destino,
            e.estado,
            (e.fecha_estimada_entrega - e.fecha_envio) AS tiempo_total_envio
        FROM 
            envios e
        WHERE 
            e.estado = 'entregado' AND
            e.fecha_estimada_entrega IS NOT NULL;
        
        -- Consultar la vista que muestra los envíos entregados 
        SELECT * FROM envios_entregados_con_tiempo_total;
        ```
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/79a9a781-facc-482f-9b46-7d688cf413a1/Untitled.png)
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/2f024ae7-08ad-4a05-b39c-468f8e91a910/02fd6166-6993-42bd-b86a-a2a1049addc0/Untitled.png)
        

### Código completo

### Documentación para Conectar PowerBI a BBDD en Amazon via SSH Tunneling

#### 1. Descargar e Instalar el Conector de MariaDB para Windows

-   **Acceso al Conector:** Visitar [MariaDB Connector](https://mariadb.com/downloads/connectors/) para descargar el conector que permite a PowerBI comunicarse con la base de datos MariaDB.
-   **Selección y Descarga:** Seleccionar el conector adecuado para Windows.
-   **Instalación:** Seguir las instrucciones del sitio para una correcta instalación en el sistema operativo Windows.

#### 2. Configuración de WSL (Subsistema de Windows para Linux)

-   **Instalación de WSL:** Abrir PowerShell como administrador y ejecutar:
    
    
    `wsl --install` 
    
    Esto instalará la última versión disponible de Ubuntu por defecto en Windows.
-   **Inicio de WSL:** Iniciar Ubuntu desde el menú de inicio de Windows tras la instalación.

#### 3. Configuración de Claves SSH en WSL

-   **Generación de Claves SSH:** En la terminal de Ubuntu en WSL, generar un nuevo par de claves SSH con:
    
    `ssh-keygen -t rsa -b 4096` 
    
-   **Distribución de la Clave Pública:** Copiar la clave pública al servidor bastion que alberga la base de datos con:
    
    `ssh-copy-id -i ~/.ssh/id_rsa.pub usuario@direccion-bastion` 
    
    Reemplazar "usuario" y "direccion-bastion" con los datos reales del servidor bastion.

#### 4. Establecimiento del SSH Tunneling a través de una Máquina Bastion

-   **Creación del Túnel con Jump Host:** En WSL, establecer el túnel SSH con el comando:
    
    `ssh -J usuario@direccion-bastion usuario@direccion-final -L 3306:127.0.0.1:3306 -N -f -i ~/.ssh/id_rsa` 
    
    Esto configura un túnel que redirige el puerto local 3306 al puerto 3306 en el servidor final a través de la máquina bastion.
-   **Notas Importantes:** Las direcciones IP podrían cambiar al reiniciar las máquinas, lo cual debe ser considerado al conectarse.

#### 5. Conectar PowerBI a la Base de Datos

-   **Configuración en PowerBI:** Abrir PowerBI, seleccionar "Obtener datos" y luego "Base de datos MySQL".
-   **Detalles de la Conexión:**
    -   **Servidor:** Escribir `127.0.0.1`
    -   **Puerto:** Usar el puerto `3306` o el que se haya configurado en el túnel SSH.
-   **Credenciales de la Base de Datos:** Introducir las credenciales de usuario y contraseña para la base de datos.
-   **Finalización:** Conectar y comenzar a analizar los datos.

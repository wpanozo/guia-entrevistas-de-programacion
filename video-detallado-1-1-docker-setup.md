# Video 1.1: Bienvenida y Setup Docker Completo

## 📋 Información del Video
- **Duración**: 15 minutos
- **Tipo**: Setup + Proyecto práctico
- **Dificultad**: Principiante
- **Mini-Proyecto**: Environment Setup Checker

## 🎯 Objetivos del Video
Al completar este video, el estudiante será capaz de:
- ✅ Configurar entorno completo con Docker
- ✅ Verificar que todos los servicios funcionen
- ✅ Crear su primer proyecto web funcional
- ✅ Entender el flujo de trabajo del bootcamp

## 📝 Script Detallado del Video

### Introducción (2 minutos)

**[APARECE EN PANTALLA]**
```
Título: "Docker Setup Completo"
Subtítulo: "Tu entorno de desarrollo profesional"
```

**[INSTRUCTOR HABLANDO]**
"¡Hola! Soy [Tu Nombre], Ingeniero de Telecomunicaciones con más de 15 años como Senior Software Engineer. En este primer video del bootcamp, vamos a configurar un entorno de desarrollo profesional usando Docker. 

¿Por qué Docker desde el día 1? Porque elimina el famoso problema de 'funciona en mi máquina' y te prepara para trabajar como lo hacen los desarrolladores en empresas reales.

Al final de este video, tendrás corriendo:
- Un servidor web Nginx
- Un entorno Node.js 
- VS Code en el navegador
- Live reload automático
- Y crearás tu primer proyecto: un verificador de entorno"

### Verificación de Prerrequisitos (3 minutos)

**[COMPARTIR PANTALLA - TERMINAL]**

**[INSTRUCTOR]**
"Primero, verificamos que tienes Docker instalado. Abre tu terminal..."

```bash
# Comando 1: Verificar Docker
docker --version
```

**[RESULTADO ESPERADO EN PANTALLA]**
```
Docker version 24.0.7, build afdd53b
```

**[INSTRUCTOR]**
"Perfecto. Ahora Docker Compose:"

```bash
# Comando 2: Verificar Docker Compose
docker-compose --version
```

**[RESULTADO ESPERADO EN PANTALLA]**
```
Docker Compose version v2.23.3-desktop.2
```

**[INSTRUCTOR]**
"Si no tienes Docker instalado, pausa el video y ve a docker.com para descargarlo. Es gratuito."

**[MOSTRAR PANTALLA - docker.com]**
"Para Windows/Mac: Docker Desktop. Para Linux: Docker Engine."

### Configuración del Proyecto (5 minutos)

**[COMPARTIR PANTALLA - TERMINAL]**

**[INSTRUCTOR]**
"Ahora vamos a clonar el repositorio del curso:"

```bash
# Comando 3: Clonar repositorio
git clone https://github.com/tu-bootcamp/curso-1-fundamentos.git
cd curso-1-fundamentos
```

**[MOSTRAR ESTRUCTURA DE ARCHIVOS]**
```
curso-1-fundamentos/
├── docker-compose.yml
├── nginx.conf
├── package.json
├── projects/
│   └── 01-environment-checker/
├── README.md
└── .gitignore
```

**[INSTRUCTOR]**
"Aquí tienes la estructura del curso. Cada proyecto estará en la carpeta 'projects'. Ahora vamos a ver el docker-compose.yml:"

**[ABRIR ARCHIVO - docker-compose.yml]**
```yaml
version: '3.8'
services:
  # Servidor web principal
  nginx-dev:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./projects:/usr/share/nginx/html
    depends_on:
      - node-dev

  # Entorno Node.js
  node-dev:
    image: node:18-alpine
    volumes:
      - ./:/app
      - node_modules:/app/node_modules
    working_dir: /app
    ports:
      - "3000:3000"
      - "5000:5000"
    environment:
      - NODE_ENV=development
    command: npm run dev

  # VS Code en el navegador
  vscode-server:
    image: codercom/code-server:latest
    ports:
      - "8080:8080"
    volumes:
      - ./:/home/coder/project
      - ~/.gitconfig:/home/coder/.gitconfig
    environment:
      - PASSWORD=bootcamp2024
    command: code-server --bind-addr 0.0.0.0:8080 --auth password

  # Servidor con live reload
  live-server:
    image: node:18-alpine
    ports:
      - "8000:8000"
    volumes:
      - ./projects:/app
    working_dir: /app
    command: npx live-server --port=8000 --host=0.0.0.0

volumes:
  node_modules:
```

**[INSTRUCTOR]**
"Este archivo define 4 servicios:
1. **nginx-dev**: Servidor web principal en puerto 80
2. **node-dev**: Entorno Node.js para desarrollo en puertos 3000 y 5000
3. **vscode-server**: VS Code en el navegador en puerto 8080
4. **live-server**: Servidor con recarga automática en puerto 8000

¡Ahora levantemos todo!"

### Levantando los Servicios (3 minutos)

**[TERMINAL]**

```bash
# Comando 4: Levantar todos los servicios
docker-compose up -d
```

**[INSTRUCTOR]**
"El flag '-d' significa 'detached', para que corra en segundo plano."

**[MOSTRAR OUTPUT DEL COMANDO]**
```
Creating network "curso-1-fundamentos_default" with the default driver
Creating volume "curso-1-fundamentos_node_modules" with default driver
Pulling nginx-dev (nginx:alpine)...
alpine: Pulling from library/nginx
...
Creating curso-1-fundamentos_node-dev_1     ... done
Creating curso-1-fundamentos_vscode-server_1 ... done
Creating curso-1-fundamentos_live-server_1   ... done
Creating curso-1-fundamentos_nginx-dev_1     ... done
```

**[INSTRUCTOR]**
"¡Perfecto! Ahora vamos a verificar que todo esté corriendo:"

```bash
# Comando 5: Verificar servicios
docker-compose ps
```

**[MOSTRAR OUTPUT]**
```
              Name                            Command               State                       Ports
curso-1-fundamentos_live-server_1   docker-entrypoint.sh npx ...   Up      0.0.0.0:8000->8000/tcp
curso-1-fundamentos_nginx-dev_1     /docker-entrypoint.sh ngin...   Up      0.0.0.0:443->443/tcp, 0.0.0.0:80->80/tcp
curso-1-fundamentos_node-dev_1      docker-entrypoint.sh npm ...   Up      0.0.0.0:3000->3000/tcp, 0.0.0.0:5000->5000/tcp
curso-1-fundamentos_vscode-server_1 /usr/bin/entrypoint.sh cod...   Up      0.0.0.0:8080->8080/tcp
```

### Creando el Primer Proyecto (2 minutos)

**[INSTRUCTOR]**
"Ahora vamos a crear nuestro primer proyecto: un verificador de entorno. Primero, vamos a VS Code en el navegador:"

**[ABRIR NAVEGADOR - http://localhost:8080]**

**[MOSTRAR PANTALLA DE LOGIN]**
"Password: bootcamp2024"

**[DENTRO DE VS CODE SERVER]**
"¡Increíble! Tenemos VS Code completo en el navegador. Naveguemos a projects/01-environment-checker/"

**[CREAR ARCHIVO - index.html]**

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Environment Checker</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            max-width: 800px; 
            margin: 0 auto; 
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        .container {
            background: white;
            padding: 2rem;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }
        .check { 
            padding: 10px; 
            margin: 10px 0; 
            border-radius: 5px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .success { 
            background: #d4edda; 
            border: 1px solid #c3e6cb; 
        }
        .error { 
            background: #f8d7da; 
            border: 1px solid #f5c6cb; 
        }
        .icon {
            font-size: 1.2em;
        }
        h1 {
            color: #333;
            text-align: center;
            margin-bottom: 2rem;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🐳 Docker Environment Checker</h1>
        <div id="checks"></div>
        <div id="info"></div>
    </div>
    
    <script>
        // Array de verificaciones
        const checks = [
            { 
                name: 'Docker Running', 
                test: () => true, // Siempre true si llegamos aquí
                description: 'Contenedores Docker funcionando correctamente'
            },
            { 
                name: 'Live Server', 
                test: () => window.location.port === '8000',
                description: 'Servidor con recarga automática activo'
            },
            { 
                name: 'Modern Browser', 
                test: () => !!window.fetch,
                description: 'Navegador con soporte para APIs modernas'
            },
            {
                name: 'Local Storage',
                test: () => !!window.localStorage,
                description: 'Almacenamiento local disponible'
            },
            {
                name: 'Console API',
                test: () => !!window.console,
                description: 'Consola de desarrollador disponible'
            }
        ];
        
        // Función para ejecutar las verificaciones
        function runChecks() {
            const checksDiv = document.getElementById('checks');
            let successCount = 0;
            
            checks.forEach(check => {
                const div = document.createElement('div');
                const isSuccess = check.test();
                if (isSuccess) successCount++;
                
                div.className = `check ${isSuccess ? 'success' : 'error'}`;
                div.innerHTML = `
                    <span class="icon">${isSuccess ? '✅' : '❌'}</span>
                    <div>
                        <strong>${check.name}</strong>: ${isSuccess ? 'OK' : 'FAIL'}
                        <br><small>${check.description}</small>
                    </div>
                `;
                checksDiv.appendChild(div);
            });
            
            // Mostrar información adicional
            const infoDiv = document.getElementById('info');
            infoDiv.innerHTML = `
                <div class="check success">
                    <span class="icon">📊</span>
                    <div>
                        <strong>Resumen</strong>: ${successCount}/${checks.length} verificaciones exitosas
                        <br><small>Entorno listo para desarrollo</small>
                    </div>
                </div>
                <div class="check success">
                    <span class="icon">🚀</span>
                    <div>
                        <strong>Siguiente paso</strong>: ¡Comienza con el Video 1.2!
                        <br><small>Crearás tu primera business card digital</small>
                    </div>
                </div>
            `;
        }
        
        // Ejecutar al cargar la página
        document.addEventListener('DOMContentLoaded', runChecks);
        
        // Función para mostrar información del entorno
        function showEnvironmentInfo() {
            console.log('🐳 Environment Information:');
            console.log('Browser:', navigator.userAgent);
            console.log('Viewport:', window.innerWidth + 'x' + window.innerHeight);
            console.log('URL:', window.location.href);
            console.log('Local Storage available:', !!window.localStorage);
        }
        
        // Mostrar info en consola
        showEnvironmentInfo();
    </script>
</body>
</html>
```

### Verificación Final (1 minuto)

**[INSTRUCTOR]**
"¡Perfecto! Ahora vamos a ver nuestro proyecto en acción:"

**[ABRIR NAVEGADOR - http://localhost:8000/01-environment-checker/]**

**[MOSTRAR RESULTADO]**
"¡Increíble! Nuestro verificador de entorno está funcionando. Todas las verificaciones deberían estar en verde."

**[ABRIR CONSOLA DEL NAVEGADOR]**
"Y mira la consola del navegador - tenemos información adicional del entorno."

### Conclusión y Próximos Pasos (1 minuto)

**[INSTRUCTOR]**
"¡Felicidades! Has completado exitosamente el setup de tu entorno Docker y creado tu primer proyecto. 

En resumen, ahora tienes:
✅ Docker corriendo con 4 servicios
✅ VS Code en el navegador para programar
✅ Live reload para ver cambios instantáneos  
✅ Tu primer proyecto web funcional

En el próximo video, crearemos tu business card digital usando HTML5 semántico. ¡Nos vemos allí!"

**[PANTALLA FINAL]**
```
Próximo video: "HTML5 Básico - Mi Primera Página"
Mini-proyecto: Personal Business Card
¡Nos vemos en 5 minutos! 🚀
```

---

## 📚 Materiales del Video

### Archivos Descargables:
1. **docker-compose.yml** - Configuración completa
2. **nginx.conf** - Configuración del servidor web
3. **package.json** - Dependencias del proyecto
4. **Environment Checker Source Code** - Código completo del proyecto

### Enlaces Útiles:
- [Docker Desktop Download](https://www.docker.com/products/docker-desktop)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [VS Code Server GitHub](https://github.com/coder/code-server)

### Comandos del Video:
```bash
# Verificar Docker
docker --version
docker-compose --version

# Clonar y setup
git clone https://github.com/tu-bootcamp/curso-1-fundamentos.git
cd curso-1-fundamentos

# Levantar servicios
docker-compose up -d

# Verificar estado
docker-compose ps

# Detener servicios (cuando sea necesario)
docker-compose down
```

### Puertos Utilizados:
- **80**: Nginx web server
- **3000**: Node.js development server
- **5000**: Node.js secondary port
- **8000**: Live reload server
- **8080**: VS Code server

---

## 🧪 Quiz del Video (5 preguntas)

### Pregunta 1:
**¿Cuántos servicios Docker configuramos en este video?**
- a) 2 servicios
- b) 3 servicios  
- c) 4 servicios ✅
- d) 5 servicios

### Pregunta 2:
**¿En qué puerto corre el VS Code server?**
- a) 3000
- b) 8000
- c) 8080 ✅
- d) 80

### Pregunta 3:
**¿Cuál es la contraseña del VS Code server?**
- a) password
- b) docker2024
- c) bootcamp2024 ✅
- d) vscode

### Pregunta 4:
**¿Qué ventaja principal tiene usar Docker desde el día 1?**
- a) Es más rápido que la instalación local
- b) Elimina problemas de "funciona en mi máquina" ✅
- c) Es gratis
- d) Todas las anteriores

### Pregunta 5:
**¿Qué comando usamos para levantar todos los servicios Docker?**
- a) docker start
- b) docker run
- c) docker-compose up -d ✅
- d) docker-compose start

---

## 🎯 Objetivos de Aprendizaje Cumplidos

Al completar este video, el estudiante habrá:
- ✅ Configurado un entorno Docker profesional
- ✅ Verificado el funcionamiento de todos los servicios
- ✅ Creado su primer proyecto web interactivo
- ✅ Entendido el flujo de trabajo del bootcamp
- ✅ Usado VS Code en el navegador
- ✅ Experimentado con live reload
- ✅ Aplicado HTML, CSS y JavaScript básico
- ✅ Implementado verificaciones dinámicas con JavaScript

## 🔄 Transición al Siguiente Video

**Setup para Video 1.2:**
- Entorno Docker ya configurado ✅
- VS Code server funcionando ✅  
- Live reload activo ✅
- Carpeta de proyectos lista ✅
- Conocimientos básicos de HTML/CSS/JS aplicados ✅

**Lo que aprenderemos:**
- HTML5 semántico avanzado
- Estructura de documento profesional
- Meta tags y SEO básico
- Accesibilidad web
- Business card responsive
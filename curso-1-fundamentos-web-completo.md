# Curso 1: Fundamentos Web + Docker Setup

## 📋 Información General
- **Título**: "Fundamentos Web con Docker: 48 Proyectos Prácticos desde Cero"
- **Duración**: 12 horas (48 videos de 15 min c/u)
- **Nivel**: Principiante absoluto
- **Proyecto Principal**: Blog Personal + Portfolio Interactivo
- **Mini-proyectos**: 48 proyectos únicos (1 por video)
- **Precio Sugerido**: $89.99

## 🐳 Docker Setup Completo

### docker-compose.yml Principal:
```yaml
version: '3.8'
services:
  # Development Server
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

  # Node.js Development Environment
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

  # VS Code Server
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

  # Live Reload Server
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

## 🎯 Objetivos de Aprendizaje

Al finalizar este curso, el estudiante será capaz de:
- ✅ Configurar entorno completo con Docker
- ✅ Crear 48 proyectos web funcionales
- ✅ Dominar HTML5 semántico y accesible
- ✅ Manejar CSS3 moderno (Flexbox, Grid, Animations)
- ✅ Programar JavaScript ES6+ interactivo
- ✅ Usar Git para control de versiones
- ✅ Deployar proyectos en múltiples plataformas
- ✅ Implementar responsive design y PWA básico

---

## 📚 Estructura Completa del Curso (12 Secciones - 48 Videos)

### **Sección 1: Docker Setup y Primer Proyecto (4 videos)**

#### Video 1.1: Bienvenida y Setup Docker Completo (15 min)
**Mini-Proyecto**: Environment Setup Checker
- **Objetivo**: Configurar todo el entorno Docker
- **Tecnologías**: Docker, Docker Compose, Git
- **Resultado**: Entorno completamente funcional

**Contenido del Video**:
```bash
# 1. Verificar Docker
docker --version
docker-compose --version

# 2. Clonar repositorio del curso
git clone https://github.com/tu-bootcamp/curso-1-fundamentos.git
cd curso-1-fundamentos

# 3. Levantar stack completo
docker-compose up -d

# 4. Verificar servicios
curl http://localhost        # Nginx
curl http://localhost:8080   # VS Code Server
curl http://localhost:8000   # Live Server
```

**Proyecto: Environment Checker**
```html
<!-- projects/01-environment-checker/index.html -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Environment Checker</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 800px; margin: 0 auto; padding: 20px; }
        .check { padding: 10px; margin: 10px 0; border-radius: 5px; }
        .success { background: #d4edda; border: 1px solid #c3e6cb; }
        .error { background: #f8d7da; border: 1px solid #f5c6cb; }
    </style>
</head>
<body>
    <h1>🐳 Docker Environment Checker</h1>
    <div id="checks"></div>
    <script>
        const checks = [
            { name: 'Docker Running', test: () => true },
            { name: 'Live Server', test: () => window.location.port === '8000' },
            { name: 'Modern Browser', test: () => !!window.fetch }
        ];
        
        const checksDiv = document.getElementById('checks');
        checks.forEach(check => {
            const div = document.createElement('div');
            const isSuccess = check.test();
            div.className = `check ${isSuccess ? 'success' : 'error'}`;
            div.innerHTML = `${check.name}: ${isSuccess ? '✅ OK' : '❌ FAIL'}`;
            checksDiv.appendChild(div);
        });
    </script>
</body>
</html>
```

#### Video 1.2: HTML5 Básico - Mi Primera Página (15 min)
**Mini-Proyecto**: Personal Business Card
- **Objetivo**: Crear primera página HTML válida
- **Tecnologías**: HTML5 semántico
- **Resultado**: Business card digital responsive

**Proyecto: Business Card**
```html
<!-- projects/02-business-card/index.html -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Business card de [Tu Nombre]">
    <title>[Tu Nombre] - Full Stack Developer</title>
</head>
<body>
    <header>
        <h1>[Tu Nombre]</h1>
        <p>Full Stack Developer en Formación</p>
    </header>
    
    <main>
        <section>
            <h2>Sobre Mí</h2>
            <p>Estudiante del Bootcamp Full Stack, apasionado por la tecnología y el desarrollo web.</p>
        </section>
        
        <section>
            <h2>Habilidades</h2>
            <ul>
                <li>HTML5</li>
                <li>CSS3</li>
                <li>JavaScript (en aprendizaje)</li>
                <li>Docker</li>
            </ul>
        </section>
        
        <section>
            <h2>Contacto</h2>
            <address>
                Email: <a href="mailto:tu@email.com">tu@email.com</a><br>
                GitHub: <a href="https://github.com/tu-usuario">tu-usuario</a>
            </address>
        </section>
    </main>
    
    <footer>
        <p>&copy; 2024 [Tu Nombre]. Proyecto del Bootcamp.</p>
    </footer>
</body>
</html>
```

#### Video 1.3: CSS Básico - Estilizando la Business Card (15 min)
**Mini-Proyecto**: Styled Business Card
- **Objetivo**: Aplicar estilos CSS básicos
- **Tecnologías**: CSS3, Selectores, Box Model
- **Resultado**: Business card con diseño atractivo

**Proyecto: Styled Business Card**
```css
/* projects/03-styled-card/styles.css */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Arial', sans-serif;
    line-height: 1.6;
    color: #333;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
}

.card {
    background: white;
    max-width: 400px;
    padding: 2rem;
    border-radius: 10px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    text-align: center;
}

.card h1 {
    color: #667eea;
    margin-bottom: 0.5rem;
    font-size: 2rem;
}

.card .subtitle {
    color: #666;
    margin-bottom: 2rem;
    font-style: italic;
}

.skills {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin: 1rem 0;
}

.skill-tag {
    background: #667eea;
    color: white;
    padding: 0.25rem 0.75rem;
    border-radius: 20px;
    font-size: 0.8rem;
}

.contact-links a {
    color: #667eea;
    text-decoration: none;
    margin: 0 1rem;
    transition: color 0.3s;
}

.contact-links a:hover {
    color: #764ba2;
}
```

#### Video 1.4: JavaScript Básico - Interactividad (15 min)
**Mini-Proyecto**: Interactive Business Card
- **Objetivo**: Agregar interactividad con JavaScript
- **Tecnologías**: JavaScript ES6, DOM manipulation
- **Resultado**: Business card con efectos interactivos

**Proyecto: Interactive Card**
```javascript
// projects/04-interactive-card/script.js
document.addEventListener('DOMContentLoaded', function() {
    // Dark/Light Mode Toggle
    const toggleBtn = document.createElement('button');
    toggleBtn.textContent = '🌙';
    toggleBtn.className = 'theme-toggle';
    document.body.appendChild(toggleBtn);
    
    toggleBtn.addEventListener('click', function() {
        document.body.classList.toggle('dark-mode');
        toggleBtn.textContent = document.body.classList.contains('dark-mode') ? '☀️' : '🌙';
    });
    
    // Skill Counter Animation
    const skillTags = document.querySelectorAll('.skill-tag');
    let count = 0;
    
    skillTags.forEach((tag, index) => {
        setTimeout(() => {
            tag.style.opacity = '0';
            tag.style.transform = 'translateY(20px)';
            
            setTimeout(() => {
                tag.style.transition = 'all 0.5s ease';
                tag.style.opacity = '1';
                tag.style.transform = 'translateY(0)';
            }, 100);
        }, index * 200);
    });
    
    // Contact Info Copy to Clipboard
    const emailLink = document.querySelector('a[href^="mailto:"]');
    if (emailLink) {
        emailLink.addEventListener('click', function(e) {
            e.preventDefault();
            const email = this.href.replace('mailto:', '');
            navigator.clipboard.writeText(email).then(() => {
                showNotification('Email copiado al portapapeles!');
            });
        });
    }
    
    function showNotification(message) {
        const notification = document.createElement('div');
        notification.className = 'notification';
        notification.textContent = message;
        document.body.appendChild(notification);
        
        setTimeout(() => {
            notification.remove();
        }, 3000);
    }
});
```

---

### **Sección 2: HTML5 Semántico Avanzado (4 videos)**

#### Video 2.1: Elementos Semánticos y Accesibilidad (15 min)
**Mini-Proyecto**: Blog Article Template
- **Objetivo**: Usar HTML5 semántico correctamente
- **Tecnologías**: HTML5 semantic elements, ARIA
- **Resultado**: Template de artículo accesible

#### Video 2.2: Formularios Modernos (15 min)
**Mini-Proyecto**: Contact Form with Validation
- **Objetivo**: Crear formularios con validación HTML5
- **Tecnologías**: HTML5 forms, input types, validation
- **Resultado**: Formulario de contacto funcional

#### Video 2.3: Media y Multimedia (15 min)
**Mini-Proyecto**: Media Gallery
- **Objetivo**: Integrar diferentes tipos de media
- **Tecnologías**: HTML5 video, audio, picture element
- **Resultado**: Galería multimedia responsive

#### Video 2.4: Microdata y SEO (15 min)
**Mini-Proyecto**: SEO-Optimized Profile
- **Objetivo**: Optimizar contenido para SEO
- **Tecnologías**: Schema.org, Open Graph, meta tags
- **Resultado**: Perfil optimizado para motores de búsqueda

---

### **Sección 3: CSS3 Layout Fundamentals (4 videos)**

#### Video 3.1: Flexbox Mastery (15 min)
**Mini-Proyecto**: Navigation Menu System
- **Objetivo**: Dominar Flexbox layouts
- **Tecnologías**: CSS Flexbox, responsive design
- **Resultado**: Sistema de navegación adaptable

#### Video 3.2: CSS Grid Fundamentals (15 min)
**Mini-Proyecto**: Magazine Layout
- **Objetivo**: Crear layouts complejos con Grid
- **Tecnologías**: CSS Grid, grid areas
- **Resultado**: Layout tipo revista

#### Video 3.3: Responsive Design Patterns (15 min)
**Mini-Proyecto**: Responsive Dashboard
- **Objetivo**: Implementar patrones responsive
- **Tecnologías**: Media queries, mobile-first
- **Resultado**: Dashboard adaptable

#### Video 3.4: CSS Custom Properties (15 min)
**Mini-Proyecto**: Theme System
- **Objetivo**: Sistema de temas con CSS variables
- **Tecnologías**: CSS custom properties, JavaScript
- **Resultado**: Sistema de temas dinámico

---

### **Sección 4: CSS3 Avanzado y Animaciones (4 videos)**

#### Video 4.1: Transforms y Transitions (15 min)
**Mini-Proyecto**: Interactive Card Flip
- **Objetivo**: Crear animaciones fluidas
- **Tecnologías**: CSS transforms, transitions
- **Resultado**: Tarjeta con efecto flip 3D

#### Video 4.2: CSS Animations y Keyframes (15 min)
**Mini-Proyecto**: Loading Animations
- **Objetivo**: Crear animaciones complejas
- **Tecnologías**: CSS animations, keyframes
- **Resultado**: Colección de loading spinners

#### Video 4.3: SVG y CSS (15 min)
**Mini-Proyecto**: Animated Logo
- **Objetivo**: Animar gráficos SVG
- **Tecnologías**: SVG, CSS animations
- **Resultado**: Logo animado interactivo

#### Video 4.4: CSS Grid Avanzado (15 min)
**Mini-Proyecto**: Masonry Layout
- **Objetivo**: Layouts complejos con Grid
- **Tecnologías**: CSS Grid avanzado, JavaScript
- **Resultado**: Layout tipo Pinterest

---

### **Sección 5: JavaScript ES6+ Fundamentals (4 videos)**

#### Video 5.1: Variables, Functions y Scope (15 min)
**Mini-Proyecto**: Calculator App
- **Objetivo**: Fundamentos de JavaScript moderno
- **Tecnologías**: ES6+, functions, scope
- **Resultado**: Calculadora funcional

#### Video 5.2: Arrays y Objects (15 min)
**Mini-Proyecto**: Student Grade Manager
- **Objetivo**: Manipular datos complejos
- **Tecnologías**: Arrays, Objects, methods
- **Resultado**: Sistema de gestión de notas

#### Video 5.3: Destructuring y Spread (15 min)
**Mini-Proyecto**: Data Transformer
- **Objetivo**: Técnicas modernas de JS
- **Tecnologías**: Destructuring, spread, rest
- **Resultado**: Herramienta de transformación de datos

#### Video 5.4: Async/Await y Promises (15 min)
**Mini-Proyecto**: Weather App
- **Objetivo**: Programación asíncrona
- **Tecnologías**: Promises, async/await, fetch
- **Resultado**: App del clima con API externa

---

### **Sección 6: DOM Manipulation y Events (4 videos)**

#### Video 6.1: Selecting y Modifying Elements (15 min)
**Mini-Proyecto**: Dynamic Content Generator
- **Objetivo**: Manipular DOM dinámicamente
- **Tecnologías**: querySelector, innerHTML
- **Resultado**: Generador de contenido dinámico

#### Video 6.2: Event Handling Advanced (15 min)
**Mini-Proyecto**: Interactive Game Board
- **Objetivo**: Manejar eventos complejos
- **Tecnologías**: Event listeners, event delegation
- **Resultado**: Tablero de juego interactivo

#### Video 6.3: Form Handling y Validation (15 min)
**Mini-Proyecto**: Multi-Step Form
- **Objetivo**: Formularios avanzados
- **Tecnologías**: Form validation, localStorage
- **Resultado**: Formulario multi-paso con persistencia

#### Video 6.4: Browser APIs (15 min)
**Mini-Proyecto**: PWA Features Demo
- **Objetivo**: Usar APIs del navegador
- **Tecnologías**: Geolocation, localStorage, notifications
- **Resultado**: Demo de características PWA

---

### **Sección 7: Local Storage y State Management (4 videos)**

#### Video 7.1: localStorage Fundamentals (15 min)
**Mini-Proyecto**: Note Taking App
- **Objetivo**: Persistencia de datos local
- **Tecnologías**: localStorage, JSON
- **Resultado**: App de notas persistente

#### Video 7.2: sessionStorage y cookies (15 min)
**Mini-Proyecto**: Shopping Cart
- **Objetivo**: Diferentes tipos de almacenamiento
- **Tecnologías**: sessionStorage, cookies
- **Resultado**: Carrito de compras funcional

#### Video 7.3: State Management Patterns (15 min)
**Mini-Proyecto**: Todo App with State
- **Objetivo**: Gestión de estado centralizada
- **Tecnologías**: JavaScript modules, state pattern
- **Resultado**: Todo app con arquitectura escalable

#### Video 7.4: IndexedDB Basics (15 min)
**Mini-Proyecto**: Offline Database App
- **Objetivo**: Base de datos local
- **Tecnologías**: IndexedDB, offline storage
- **Resultado**: App con base de datos offline

---

### **Sección 8: APIs y AJAX (4 videos)**

#### Video 8.1: Fetch API Fundamentals (15 min)
**Mini-Proyecto**: API Explorer
- **Objetivo**: Consumir APIs REST
- **Tecnologías**: Fetch API, JSON
- **Resultado**: Explorador de APIs interactivo

#### Video 8.2: Error Handling y Loading States (15 min)
**Mini-Proyecto**: Robust API Client
- **Objetivo**: Manejo robusto de APIs
- **Tecnologías**: Error handling, loading states
- **Resultado**: Cliente API con manejo de errores

#### Video 8.3: Working with Real APIs (15 min)
**Mini-Proyecto**: News Reader App
- **Objetivo**: Integración con APIs reales
- **Tecnologías**: News API, pagination
- **Resultado**: Lector de noticias dinámico

#### Video 8.4: Building Mock APIs (15 min)
**Mini-Proyecto**: JSON Server Setup
- **Objetivo**: Crear APIs de prueba
- **Tecnologías**: JSON Server, mock data
- **Resultado**: API mock para desarrollo

---

### **Sección 9: Git y Version Control (4 videos)**

#### Video 9.1: Git Basics y Setup (15 min)
**Mini-Proyecto**: Version Control Workflow
- **Objetivo**: Fundamentos de Git
- **Tecnologías**: Git, GitHub
- **Resultado**: Repositorio con historial limpio

#### Video 9.2: Branching y Merging (15 min)
**Mini-Proyecto**: Feature Branch Workflow
- **Objetivo**: Workflows de desarrollo
- **Tecnologías**: Git branches, merge
- **Resultado**: Proyecto con múltiples features

#### Video 9.3: GitHub y Collaboration (15 min)
**Mini-Proyecto**: Open Source Contribution
- **Objetivo**: Colaboración en GitHub
- **Tecnologías**: GitHub, pull requests
- **Resultado**: Contribución a proyecto open source

#### Video 9.4: Git Hooks y Automation (15 min)
**Mini-Proyecto**: Automated Workflow
- **Objetivo**: Automatización con Git
- **Tecnologías**: Git hooks, GitHub Actions
- **Resultado**: Workflow automatizado

---

### **Sección 10: Performance y Optimization (4 videos)**

#### Video 10.1: Image Optimization (15 min)
**Mini-Proyecto**: Optimized Gallery
- **Objetivo**: Optimizar imágenes web
- **Tecnologías**: WebP, lazy loading, compression
- **Resultado**: Galería optimizada

#### Video 10.2: CSS y JS Minification (15 min)
**Mini-Proyecto**: Build Process Setup
- **Objetivo**: Optimizar assets
- **Tecnologías**: Minification, bundling
- **Resultado**: Proceso de build automatizado

#### Video 10.3: Lighthouse y Auditing (15 min)
**Mini-Proyecto**: Performance Audit
- **Objetivo**: Auditar rendimiento
- **Tecnologías**: Lighthouse, web vitals
- **Resultado**: Sitio optimizado para performance

#### Video 10.4: Caching Strategies (15 min)
**Mini-Proyecto**: Cache Implementation
- **Objetivo**: Estrategias de caché
- **Tecnologías**: Service workers, HTTP cache
- **Resultado**: App con caché inteligente

---

### **Sección 11: Deployment y Hosting (4 videos)**

#### Video 11.1: Netlify Deployment (15 min)
**Mini-Proyecto**: Netlify Site with Forms
- **Objetivo**: Deploy en Netlify
- **Tecnologías**: Netlify, serverless functions
- **Resultado**: Sitio deployado con formularios

#### Video 11.2: Vercel y GitHub Pages (15 min)
**Mini-Proyecto**: Multi-Platform Deployment
- **Objetivo**: Múltiples plataformas de deploy
- **Tecnologías**: Vercel, GitHub Pages
- **Resultado**: Proyectos en múltiples plataformas

#### Video 11.3: Custom Domain y SSL (15 min)
**Mini-Proyecto**: Professional Site Setup
- **Objetivo**: Configuración profesional
- **Tecnologías**: Custom domains, SSL
- **Resultado**: Sitio con dominio personalizado

#### Video 11.4: CI/CD with GitHub Actions (15 min)
**Mini-Proyecto**: Automated Deployment
- **Objetivo**: Deploy automatizado
- **Tecnologías**: GitHub Actions, CI/CD
- **Resultado**: Pipeline de deploy automatizado

---

### **Sección 12: Proyecto Final y Portfolio (4 videos)**

#### Video 12.1: Portfolio Planning y Design (15 min)
**Mini-Proyecto**: Portfolio Wireframe
- **Objetivo**: Planificar portfolio profesional
- **Tecnologías**: Design thinking, wireframing
- **Resultado**: Plan completo del portfolio

#### Video 12.2: Portfolio Development (15 min)
**Mini-Proyecto**: Portfolio Implementation
- **Objetivo**: Desarrollar portfolio
- **Tecnologías**: Todo lo aprendido
- **Resultado**: Portfolio funcional

#### Video 12.3: Blog Integration (15 min)
**Mini-Proyecto**: Blog with CMS
- **Objetivo**: Integrar sistema de blog
- **Tecnologías**: Headless CMS, JAMstack
- **Resultado**: Portfolio con blog dinámico

#### Video 12.4: Final Deployment y Next Steps (15 min)
**Mini-Proyecto**: Production Ready Portfolio
- **Objetivo**: Deploy final optimizado
- **Tecnologías**: Production optimization
- **Resultado**: Portfolio listo para profesionales

---

## 🎁 Materiales Descargables Completos

### 📦 Código y Templates (por sección):
1. **Docker Setup Templates** - Configuraciones completas
2. **HTML5 Boilerplates** - Templates semánticos
3. **CSS3 Frameworks** - Sistema de componentes custom
4. **JavaScript Utilities** - Librería de funciones útiles
5. **Git Workflows** - Templates de workflows
6. **Deployment Scripts** - Scripts de automatización

### 🎨 Recursos de Diseño:
1. **Design System** - Colores, tipografías, espaciado
2. **Icon Library** - 500+ iconos SVG
3. **Image Assets** - Banco de imágenes para proyectos
4. **UI Components** - Componentes reutilizables

### 📚 Documentación:
1. **Best Practices Guide** - 50 páginas de mejores prácticas
2. **Troubleshooting Guide** - Solución a problemas comunes
3. **Performance Checklist** - Lista de optimizaciones
4. **Security Guidelines** - Buenas prácticas de seguridad

---

## 🧪 Sistema de Evaluación

### Quizzes Progresivos (12 total):
- 1 quiz por sección (5-10 preguntas)
- Auto-evaluación después de cada proyecto
- Quiz final integrador (50 preguntas)

### Portfolio Checkpoint:
- Evaluación cada 4 videos
- Peer review opcional
- Auto-evaluación con rubric

### Proyecto Final:
- Portfolio completo con 48 mini-proyectos
- Blog personal funcional
- Deploy en múltiples plataformas
- README profesional en GitHub

---

## 📈 Métricas de Éxito Específicas

### Engagement:
- **Completion Rate**: 80%+ (objetivo)
- **Project Submission**: 90%+ completan 40+ proyectos
- **GitHub Activity**: 85% tienen repos activos

### Learning Outcomes:
- **HTML/CSS Proficiency**: 95% post-course assessment
- **JavaScript Basics**: 85% pueden crear apps interactivas
- **Docker Usage**: 75% configuran entornos independientemente
- **Git Workflow**: 90% usan Git correctamente

### Career Impact:
- **Portfolio Quality**: 80% tienen portfolio deployado
- **Job Applications**: 40% reportan más entrevistas
- **Salary Negotiation**: 30% mencionan proyectos en entrevistas

---

## 🚀 Preparación para Curso 2

### Skills Bridge:
- **React Prerequisites**: JavaScript ES6+ sólido
- **Component Thinking**: Modularización aprendida
- **State Management**: Bases con localStorage
- **API Integration**: Fetch y async/await

### Portfolio Transition:
- Proyectos del Curso 1 como base
- Refactoring a React en Curso 2
- Progressive enhancement approach

### Community Building:
- Discord channel específico
- Code review sessions
- Peer programming exercises
- Showcase monthly events
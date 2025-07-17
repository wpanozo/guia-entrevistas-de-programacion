# Video 1.2: HTML5 Básico - Mi Primera Página

## 📋 Información del Video
- **Duración**: 15 minutos
- **Tipo**: HTML5 Semántico + Proyecto práctico
- **Dificultad**: Principiante
- **Mini-Proyecto**: Personal Business Card

## 🎯 Objetivos del Video
Al completar este video, el estudiante será capaz de:
- ✅ Crear documentos HTML5 semánticamente correctos
- ✅ Usar elementos semánticos apropiadamente
- ✅ Implementar meta tags para SEO básico
- ✅ Estructurar contenido de forma profesional
- ✅ Aplicar principios de accesibilidad web

## 📝 Script Detallado del Video

### Introducción (2 minutos)

**[APARECE EN PANTALLA]**
```
Título: "HTML5 Semántico"
Subtítulo: "Business Card Digital Profesional"
```

**[INSTRUCTOR HABLANDO]**
"¡Bienvenidos de vuelta! En el video anterior configuramos nuestro entorno Docker. Ahora vamos a crear tu primera página web profesional: una business card digital.

Pero no cualquier business card. Vamos a usar HTML5 semántico, que es cómo escriben código los desarrolladores profesionales. Al final de este video, tendrás una tarjeta digital que:
- Está optimizada para motores de búsqueda
- Es accesible para personas con discapacidades
- Sigue las mejores prácticas de la industria
- Y se ve profesional"

**[MOSTRAR PREVIEW DEL RESULTADO FINAL]**

### Conceptos de HTML5 Semántico (3 minutos)

**[COMPARTIR PANTALLA - VS CODE]**

**[INSTRUCTOR]**
"Primero, entendamos qué significa 'semántico'. En HTML5, no solo estructuramos contenido, sino que le damos SIGNIFICADO. Vamos a crear la carpeta para nuestro nuevo proyecto:"

**[NAVEGADOR EN VS CODE SERVER - http://localhost:8080]**

```
projects/
├── 01-environment-checker/ ✅
└── 02-business-card/ ← Nuevo proyecto
```

**[CREAR CARPETA Y ARCHIVO]**
"Creamos: `projects/02-business-card/index.html`"

**[INSTRUCTOR]**
"Mira la diferencia entre HTML antiguo y HTML5 moderno:"

**[MOSTRAR CÓDIGO EN PANTALLA]**

```html
<!-- ❌ HTML Antiguo (No hagas esto) -->
<div class="header">
  <div class="title">Mi Nombre</div>
</div>
<div class="content">
  <div class="section">Sobre mí</div>
</div>

<!-- ✅ HTML5 Semántico (Así lo hacemos) -->
<header>
  <h1>Mi Nombre</h1>
</header>
<main>
  <section>
    <h2>Sobre Mí</h2>
  </section>
</main>
```

**[INSTRUCTOR]**
"¿Ves la diferencia? El HTML5 semántico DICE qué es cada cosa, no solo cómo se ve. Esto ayuda a:
- Motores de búsqueda (Google entiende mejor tu contenido)
- Lectores de pantalla (accesibilidad)
- Otros desarrolladores (código más limpio)
- Tú mismo en el futuro"

### Creando la Estructura Base (4 minutos)

**[ESCRIBIR CÓDIGO EN VIVO]**

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- SEO Meta Tags -->
    <meta name="description" content="Business card digital de [Tu Nombre], Full Stack Developer en formación especializado en tecnologías web modernas.">
    <meta name="keywords" content="desarrollador, full stack, javascript, react, node.js, programador">
    <meta name="author" content="[Tu Nombre]">
    
    <!-- Open Graph para redes sociales -->
    <meta property="og:title" content="[Tu Nombre] - Full Stack Developer">
    <meta property="og:description" content="Conoce mi perfil profesional y proyectos de desarrollo web.">
    <meta property="og:type" content="profile">
    
    <title>[Tu Nombre] - Full Stack Developer | Portfolio</title>
</head>
<body>
    <!-- Aquí irá nuestro contenido -->
</body>
</html>
```

**[INSTRUCTOR]**
"Vamos línea por línea:

1. **`<!DOCTYPE html>`**: Le dice al navegador que usamos HTML5
2. **`lang='es'`**: Importante para accesibilidad y SEO
3. **`charset='UTF-8'`**: Para caracteres especiales (ñ, acentos, etc.)
4. **`viewport`**: CRÍTICO para responsive design
5. **Meta description**: Aparece en Google cuando buscan tu nombre
6. **Open Graph**: Cuando compartes en redes sociales

Estos meta tags son lo que diferencia una página amateur de una profesional."

### Estructura Semántica del Contenido (4 minutos)

**[CONTINUAR ESCRIBIENDO CÓDIGO]**

```html
<body>
    <!-- Header: Información principal -->
    <header>
        <h1>[Tu Nombre Completo]</h1>
        <p class="subtitle">Full Stack Developer en Formación</p>
        <p class="tagline">Transformando ideas en código, un proyecto a la vez</p>
    </header>
    
    <!-- Navegación (para futuras secciones) -->
    <nav aria-label="Navegación principal">
        <ul>
            <li><a href="#about">Sobre Mí</a></li>
            <li><a href="#skills">Habilidades</a></li>
            <li><a href="#projects">Proyectos</a></li>
            <li><a href="#contact">Contacto</a></li>
        </ul>
    </nav>
    
    <!-- Contenido principal -->
    <main>
        <!-- Sección: Sobre Mí -->
        <section id="about">
            <h2>Sobre Mí</h2>
            <p>Soy estudiante del <strong>Bootcamp Full Stack Developer</strong>, apasionado por crear soluciones web innovadoras. Con base en telecomunicaciones, estoy expandiendo mis habilidades hacia el desarrollo completo de aplicaciones.</p>
            
            <p>Mi objetivo es dominar el stack <abbr title="MongoDB, Express, React, Node.js">MERN</abbr> y contribuir a proyectos que impacten positivamente a los usuarios.</p>
        </section>
        
        <!-- Sección: Habilidades Técnicas -->
        <section id="skills">
            <h2>Habilidades Técnicas</h2>
            
            <h3>Frontend</h3>
            <ul>
                <li>HTML5 Semántico</li>
                <li>CSS3 Moderno (Flexbox, Grid)</li>
                <li>JavaScript ES6+</li>
                <li>React.js <em>(próximamente)</em></li>
            </ul>
            
            <h3>Backend</h3>
            <ul>
                <li>Node.js <em>(próximamente)</em></li>
                <li>Express.js <em>(próximamente)</em></li>
                <li>MongoDB <em>(próximamente)</em></li>
            </ul>
            
            <h3>Herramientas</h3>
            <ul>
                <li>Docker 🐳</li>
                <li>Git & GitHub</li>
                <li>VS Code</li>
                <li>Linux/Terminal</li>
            </ul>
        </section>
        
        <!-- Sección: Proyectos -->
        <section id="projects">
            <h2>Proyectos Recientes</h2>
            
            <article>
                <h3>Environment Checker</h3>
                <p>Primer proyecto del bootcamp: verificador de entorno Docker con JavaScript interactivo.</p>
                <p><strong>Tecnologías:</strong> HTML5, CSS3, JavaScript, Docker</p>
                <a href="../01-environment-checker/" target="_blank" rel="noopener">Ver Proyecto</a>
            </article>
            
            <article>
                <h3>Business Card Digital</h3>
                <p>Tarjeta de presentación web con HTML5 semántico y SEO optimizado.</p>
                <p><strong>Tecnologías:</strong> HTML5 Semántico, Meta Tags, Accesibilidad</p>
                <p><em>Proyecto actual - En desarrollo...</em></p>
            </article>
        </section>
        
        <!-- Sección: Contacto -->
        <section id="contact">
            <h2>Contacto</h2>
            
            <address>
                <p>📧 Email: <a href="mailto:tu@email.com">tu@email.com</a></p>
                <p>🐙 GitHub: <a href="https://github.com/tu-usuario" target="_blank" rel="noopener">tu-usuario</a></p>
                <p>💼 LinkedIn: <a href="https://linkedin.com/in/tu-perfil" target="_blank" rel="noopener">tu-perfil</a></p>
                <p>📍 Ubicación: Tu Ciudad, Tu País</p>
            </address>
        </section>
    </main>
    
    <!-- Footer -->
    <footer>
        <p>&copy; 2024 <strong>[Tu Nombre]</strong>. Desarrollado durante el Bootcamp Full Stack.</p>
        <p>
            <small>
                Última actualización: <time datetime="2024-01-15">15 de enero, 2024</time>
            </small>
        </p>
    </footer>
</body>
```

**[INSTRUCTOR]**
"Fíjate en estos elementos semánticos importantes:

- **`<header>`**: Información principal/encabezado
- **`<nav>`**: Navegación (con aria-label para accesibilidad)
- **`<main>`**: Contenido principal de la página
- **`<section>`**: Secciones temáticas con ID para navegación
- **`<article>`**: Contenido independiente (cada proyecto)
- **`<address>`**: Información de contacto
- **`<footer>`**: Pie de página
- **`<time>`**: Fechas semánticamente marcadas
- **`<abbr>`**: Abreviaciones con explicación
- **`<strong>`** y **`<em>`**: Énfasis semántico (no solo visual)"

### Verificación en Navegador (1 minuto)

**[ABRIR NAVEGADOR - http://localhost:8000/02-business-card/]**

**[INSTRUCTOR]**
"¡Perfecto! Nuestra business card está funcionando. Se ve básica ahora, pero tiene una estructura HTML5 perfecta. En el próximo video le daremos estilo con CSS."

**[ABRIR HERRAMIENTAS DE DESARROLLADOR]**
"Mira en las herramientas de desarrollador cómo el HTML está perfectamente estructurado. Los lectores de pantalla pueden navegar fácilmente por nuestras secciones."

### Validación y Mejores Prácticas (1 minuto)

**[ABRIR NUEVA PESTAÑA - validator.w3.org]**

**[INSTRUCTOR]**
"Siempre valida tu HTML. Ve a validator.w3.org y pega la URL de tu proyecto o el código directamente."

**[MOSTRAR VALIDACIÓN]**
"HTML válido significa:
- Mejor SEO
- Compatibilidad entre navegadores
- Menos bugs
- Código profesional"

### Conclusión y Próximos Pasos (1 minuto)

**[INSTRUCTOR]**
"¡Excelente trabajo! Has creado tu primera página web con HTML5 semántico profesional. 

Lo que lograste:
✅ Estructura semántica correcta
✅ Meta tags para SEO
✅ Accesibilidad básica
✅ Contenido bien organizado
✅ Validación W3C

En el próximo video, transformaremos esta business card con CSS3 moderno. Le daremos:
- Diseño atractivo
- Efectos visuales
- Layout responsive
- Animaciones suaves

¡Nos vemos en el Video 1.3!"

**[PANTALLA FINAL]**
```
Próximo video: "CSS Básico - Estilizando la Business Card"
Mini-proyecto: Styled Business Card
¡Nos vemos en 5 minutos! 🎨
```

---

## 📚 Materiales del Video

### Código Completo:
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- SEO Meta Tags -->
    <meta name="description" content="Business card digital de [Tu Nombre], Full Stack Developer en formación especializado en tecnologías web modernas.">
    <meta name="keywords" content="desarrollador, full stack, javascript, react, node.js, programador">
    <meta name="author" content="[Tu Nombre]">
    
    <!-- Open Graph para redes sociales -->
    <meta property="og:title" content="[Tu Nombre] - Full Stack Developer">
    <meta property="og:description" content="Conoce mi perfil profesional y proyectos de desarrollo web.">
    <meta property="og:type" content="profile">
    
    <title>[Tu Nombre] - Full Stack Developer | Portfolio</title>
</head>
<body>
    <header>
        <h1>[Tu Nombre Completo]</h1>
        <p class="subtitle">Full Stack Developer en Formación</p>
        <p class="tagline">Transformando ideas en código, un proyecto a la vez</p>
    </header>
    
    <nav aria-label="Navegación principal">
        <ul>
            <li><a href="#about">Sobre Mí</a></li>
            <li><a href="#skills">Habilidades</a></li>
            <li><a href="#projects">Proyectos</a></li>
            <li><a href="#contact">Contacto</a></li>
        </ul>
    </nav>
    
    <main>
        <section id="about">
            <h2>Sobre Mí</h2>
            <p>Soy estudiante del <strong>Bootcamp Full Stack Developer</strong>, apasionado por crear soluciones web innovadoras. Con base en telecomunicaciones, estoy expandiendo mis habilidades hacia el desarrollo completo de aplicaciones.</p>
            <p>Mi objetivo es dominar el stack <abbr title="MongoDB, Express, React, Node.js">MERN</abbr> y contribuir a proyectos que impacten positivamente a los usuarios.</p>
        </section>
        
        <section id="skills">
            <h2>Habilidades Técnicas</h2>
            
            <h3>Frontend</h3>
            <ul>
                <li>HTML5 Semántico</li>
                <li>CSS3 Moderno (Flexbox, Grid)</li>
                <li>JavaScript ES6+</li>
                <li>React.js <em>(próximamente)</em></li>
            </ul>
            
            <h3>Backend</h3>
            <ul>
                <li>Node.js <em>(próximamente)</em></li>
                <li>Express.js <em>(próximamente)</em></li>
                <li>MongoDB <em>(próximamente)</em></li>
            </ul>
            
            <h3>Herramientas</h3>
            <ul>
                <li>Docker 🐳</li>
                <li>Git & GitHub</li>
                <li>VS Code</li>
                <li>Linux/Terminal</li>
            </ul>
        </section>
        
        <section id="projects">
            <h2>Proyectos Recientes</h2>
            
            <article>
                <h3>Environment Checker</h3>
                <p>Primer proyecto del bootcamp: verificador de entorno Docker con JavaScript interactivo.</p>
                <p><strong>Tecnologías:</strong> HTML5, CSS3, JavaScript, Docker</p>
                <a href="../01-environment-checker/" target="_blank" rel="noopener">Ver Proyecto</a>
            </article>
            
            <article>
                <h3>Business Card Digital</h3>
                <p>Tarjeta de presentación web con HTML5 semántico y SEO optimizado.</p>
                <p><strong>Tecnologías:</strong> HTML5 Semántico, Meta Tags, Accesibilidad</p>
                <p><em>Proyecto actual - En desarrollo...</em></p>
            </article>
        </section>
        
        <section id="contact">
            <h2>Contacto</h2>
            
            <address>
                <p>📧 Email: <a href="mailto:tu@email.com">tu@email.com</a></p>
                <p>🐙 GitHub: <a href="https://github.com/tu-usuario" target="_blank" rel="noopener">tu-usuario</a></p>
                <p>💼 LinkedIn: <a href="https://linkedin.com/in/tu-perfil" target="_blank" rel="noopener">tu-perfil</a></p>
                <p>📍 Ubicación: Tu Ciudad, Tu País</p>
            </address>
        </section>
    </main>
    
    <footer>
        <p>&copy; 2024 <strong>[Tu Nombre]</strong>. Desarrollado durante el Bootcamp Full Stack.</p>
        <p>
            <small>
                Última actualización: <time datetime="2024-01-15">15 de enero, 2024</time>
            </small>
        </p>
    </footer>
</body>
</html>
```

### Enlaces Útiles:
- [HTML5 Semantic Elements - MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)
- [W3C HTML Validator](https://validator.w3.org/)
- [Schema.org Structured Data](https://schema.org/)
- [Open Graph Protocol](https://ogp.me/)
- [ARIA Labels Guide](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)

### Elementos HTML5 Usados:
- `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`
- `<h1>-<h6>`, `<p>`, `<ul>`, `<li>`, `<a>`
- `<strong>`, `<em>`, `<abbr>`, `<time>`, `<address>`
- `<meta>` tags para SEO
- `aria-label` para accesibilidad

---

## 🧪 Quiz del Video (5 preguntas)

### Pregunta 1:
**¿Cuál es la principal ventaja del HTML5 semántico?**
- a) Se ve mejor visualmente
- b) Da significado al contenido para navegadores y herramientas ✅
- c) Es más corto de escribir
- d) Solo funciona en navegadores modernos

### Pregunta 2:
**¿Qué elemento HTML5 usamos para información de contacto?**
- a) `<contact>`
- b) `<info>`
- c) `<address>` ✅
- d) `<details>`

### Pregunta 3:
**¿Para qué sirve el atributo `aria-label`?**
- a) Para agregar estilos CSS
- b) Para accesibilidad y lectores de pantalla ✅
- c) Para validación HTML
- d) Para SEO únicamente

### Pregunta 4:
**¿Cuál es el propósito del meta tag "description"?**
- a) Describe el código para otros desarrolladores
- b) Aparece en resultados de búsqueda de Google ✅
- c) Define el título de la página
- d) Establece el idioma del contenido

### Pregunta 5:
**¿Qué diferencia hay entre `<strong>` y `<b>`?**
- a) No hay diferencia
- b) `<strong>` es semántico (importancia), `<b>` es solo visual ✅
- c) `<b>` es más moderno
- d) `<strong>` no funciona en navegadores antiguos

---

## 🎯 Objetivos de Aprendizaje Cumplidos

Al completar este video, el estudiante habrá:
- ✅ Creado su primera página HTML5 semánticamente correcta
- ✅ Implementado meta tags para SEO básico
- ✅ Usado elementos semánticos apropiadamente
- ✅ Aplicado principios básicos de accesibilidad
- ✅ Estructurado contenido de forma profesional
- ✅ Validado HTML usando herramientas estándar
- ✅ Entendido la diferencia entre semántica y presentación
- ✅ Implementado navegación interna con anclas

## 🔄 Transición al Siguiente Video

**Setup para Video 1.3:**
- HTML5 estructura completa ✅
- Contenido semánticamente marcado ✅
- Meta tags implementados ✅
- Navegación funcional ✅
- Validación W3C pasada ✅

**Lo que aprenderemos:**
- CSS3 selectores y propiedades
- Box model y layout básico
- Tipografía y colores
- Efectos visuales
- Responsive design básico

**Resultado esperado:**
Business card visualmente atractiva manteniendo la estructura semántica.
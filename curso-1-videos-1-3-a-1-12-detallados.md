# Videos 1.3 - 1.12: Curso 1 Fundamentos Web Detallados

## Video 1.3: CSS Básico - Estilizando la Business Card

### 📋 Información del Video
- **Duración**: 15 minutos
- **Mini-Proyecto**: Styled Business Card
- **Tecnologías**: CSS3, Selectores, Box Model, Gradientes

### 🎯 Objetivos
- ✅ Aplicar estilos CSS básicos y avanzados
- ✅ Entender el Box Model completamente
- ✅ Crear diseños atractivos con gradientes
- ✅ Implementar hover effects

### 📝 Contenido del Video

#### CSS Completo:
```css
/* projects/03-styled-card/styles.css */
/* Reset y base */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Inter', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    line-height: 1.6;
    color: #333;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
}

/* Card principal */
.card {
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    max-width: 450px;
    width: 100%;
    padding: 2.5rem;
    border-radius: 20px;
    box-shadow: 
        0 20px 40px rgba(0, 0, 0, 0.1),
        0 10px 20px rgba(0, 0, 0, 0.05);
    text-align: center;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 
        0 30px 60px rgba(0, 0, 0, 0.15),
        0 15px 30px rgba(0, 0, 0, 0.1);
}

/* Header section */
.card-header h1 {
    color: #2d3748;
    margin-bottom: 0.5rem;
    font-size: 2.2rem;
    font-weight: 700;
    background: linear-gradient(135deg, #667eea, #764ba2);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
}

.subtitle {
    color: #718096;
    margin-bottom: 0.5rem;
    font-size: 1.1rem;
    font-weight: 500;
}

.tagline {
    color: #a0aec0;
    margin-bottom: 2rem;
    font-size: 0.95rem;
    font-style: italic;
}

/* Navigation */
nav {
    margin-bottom: 2rem;
}

nav ul {
    list-style: none;
    display: flex;
    justify-content: center;
    gap: 1rem;
    flex-wrap: wrap;
}

nav a {
    text-decoration: none;
    color: #667eea;
    padding: 0.5rem 1rem;
    border-radius: 25px;
    background: rgba(102, 126, 234, 0.1);
    transition: all 0.3s ease;
    font-size: 0.9rem;
    font-weight: 500;
}

nav a:hover {
    background: #667eea;
    color: white;
    transform: translateY(-2px);
}

/* Skills section */
.skills {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin: 1rem 0;
    justify-content: center;
}

.skill-tag {
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: white;
    padding: 0.4rem 0.8rem;
    border-radius: 20px;
    font-size: 0.8rem;
    font-weight: 500;
    transition: transform 0.2s ease;
}

.skill-tag:hover {
    transform: scale(1.05);
}

/* Projects section */
.project {
    background: rgba(102, 126, 234, 0.05);
    padding: 1rem;
    border-radius: 10px;
    margin: 1rem 0;
    border-left: 4px solid #667eea;
    text-align: left;
}

.project h3 {
    color: #2d3748;
    margin-bottom: 0.5rem;
}

.project p {
    color: #718096;
    font-size: 0.9rem;
    margin-bottom: 0.5rem;
}

.project a {
    color: #667eea;
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s ease;
}

.project a:hover {
    color: #764ba2;
}

/* Contact section */
.contact-info {
    background: linear-gradient(135deg, #f7fafc, #edf2f7);
    padding: 1.5rem;
    border-radius: 15px;
    margin: 1rem 0;
}

.contact-links {
    display: flex;
    justify-content: center;
    gap: 1rem;
    flex-wrap: wrap;
    margin-top: 1rem;
}

.contact-links a {
    color: #667eea;
    text-decoration: none;
    padding: 0.5rem 1rem;
    border: 2px solid #667eea;
    border-radius: 25px;
    transition: all 0.3s ease;
    font-weight: 500;
}

.contact-links a:hover {
    background: #667eea;
    color: white;
    transform: translateY(-2px);
}

/* Footer */
footer {
    margin-top: 2rem;
    padding-top: 1rem;
    border-top: 1px solid #e2e8f0;
    color: #a0aec0;
    font-size: 0.8rem;
}

/* Responsive design */
@media (max-width: 480px) {
    body {
        padding: 10px;
    }
    
    .card {
        padding: 1.5rem;
    }
    
    .card-header h1 {
        font-size: 1.8rem;
    }
    
    nav ul {
        flex-direction: column;
        gap: 0.5rem;
    }
    
    .contact-links {
        flex-direction: column;
    }
}
```

### 🎬 Script del Video
"En este video transformaremos nuestra business card básica en una tarjeta profesional y atractiva usando CSS3 moderno. Aplicaremos gradientes, efectos hover, y responsive design."

---

## Video 1.4: JavaScript Básico - Interactividad

### 📋 Información del Video
- **Duración**: 15 minutos  
- **Mini-Proyecto**: Interactive Business Card
- **Tecnologías**: JavaScript ES6, DOM, Events, LocalStorage

### 🎯 Objetivos
- ✅ Agregar interactividad con JavaScript
- ✅ Manipular el DOM dinámicamente
- ✅ Implementar dark/light mode
- ✅ Usar LocalStorage para persistencia

### 📝 JavaScript Completo:
```javascript
// projects/04-interactive-card/script.js
class BusinessCard {
    constructor() {
        this.isDarkMode = localStorage.getItem('darkMode') === 'true';
        this.init();
    }

    init() {
        this.createThemeToggle();
        this.createAnimations();
        this.createContactInteractions();
        this.createSkillCounters();
        this.createTypingEffect();
        this.applyTheme();
    }

    createThemeToggle() {
        const toggleBtn = document.createElement('button');
        toggleBtn.innerHTML = this.isDarkMode ? '☀️' : '🌙';
        toggleBtn.className = 'theme-toggle';
        toggleBtn.setAttribute('aria-label', 'Toggle theme');
        
        // Styles for toggle button
        Object.assign(toggleBtn.style, {
            position: 'fixed',
            top: '20px',
            right: '20px',
            background: 'rgba(255, 255, 255, 0.9)',
            border: 'none',
            borderRadius: '50%',
            width: '50px',
            height: '50px',
            fontSize: '1.5rem',
            cursor: 'pointer',
            zIndex: '1000',
            transition: 'all 0.3s ease',
            boxShadow: '0 4px 12px rgba(0, 0, 0, 0.15)'
        });

        toggleBtn.addEventListener('click', () => this.toggleTheme());
        document.body.appendChild(toggleBtn);
        this.themeToggle = toggleBtn;
    }

    toggleTheme() {
        this.isDarkMode = !this.isDarkMode;
        localStorage.setItem('darkMode', this.isDarkMode);
        this.applyTheme();
        this.themeToggle.innerHTML = this.isDarkMode ? '☀️' : '🌙';
    }

    applyTheme() {
        const root = document.documentElement;
        
        if (this.isDarkMode) {
            root.style.setProperty('--bg-gradient', 'linear-gradient(135deg, #1a202c 0%, #2d3748 100%)');
            root.style.setProperty('--card-bg', 'rgba(45, 55, 72, 0.95)');
            root.style.setProperty('--text-primary', '#f7fafc');
            root.style.setProperty('--text-secondary', '#e2e8f0');
            root.style.setProperty('--text-muted', '#a0aec0');
        } else {
            root.style.setProperty('--bg-gradient', 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)');
            root.style.setProperty('--card-bg', 'rgba(255, 255, 255, 0.95)');
            root.style.setProperty('--text-primary', '#2d3748');
            root.style.setProperty('--text-secondary', '#4a5568');
            root.style.setProperty('--text-muted', '#718096');
        }

        document.body.style.background = `var(--bg-gradient)`;
        document.querySelector('.card').style.background = `var(--card-bg)`;
    }

    createAnimations() {
        // Skill tags animation
        const skillTags = document.querySelectorAll('.skill-tag');
        skillTags.forEach((tag, index) => {
            tag.style.opacity = '0';
            tag.style.transform = 'translateY(20px)';
            
            setTimeout(() => {
                tag.style.transition = 'all 0.5s ease';
                tag.style.opacity = '1';
                tag.style.transform = 'translateY(0)';
            }, 100 + (index * 100));
        });

        // Card entrance animation
        const card = document.querySelector('.card');
        card.style.transform = 'scale(0.8) translateY(50px)';
        card.style.opacity = '0';
        
        setTimeout(() => {
            card.style.transition = 'all 0.6s cubic-bezier(0.4, 0, 0.2, 1)';
            card.style.transform = 'scale(1) translateY(0)';
            card.style.opacity = '1';
        }, 200);
    }

    createContactInteractions() {
        // Email copy functionality
        const emailLink = document.querySelector('a[href^="mailto:"]');
        if (emailLink) {
            emailLink.addEventListener('click', (e) => {
                e.preventDefault();
                const email = emailLink.href.replace('mailto:', '');
                this.copyToClipboard(email, 'Email copiado al portapapeles!');
            });
        }

        // GitHub link tracking
        const githubLink = document.querySelector('a[href*="github"]');
        if (githubLink) {
            githubLink.addEventListener('click', () => {
                this.trackEvent('GitHub Visit', 'Contact');
            });
        }
    }

    createSkillCounters() {
        const skillsSection = document.querySelector('#skills');
        if (!skillsSection) return;

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    this.animateSkillBars();
                    observer.unobserve(entry.target);
                }
            });
        });

        observer.observe(skillsSection);
    }

    animateSkillBars() {
        const skills = [
            { name: 'HTML5', level: 95 },
            { name: 'CSS3', level: 90 },
            { name: 'JavaScript', level: 85 },
            { name: 'Docker', level: 80 }
        ];

        skills.forEach((skill, index) => {
            setTimeout(() => {
                this.createSkillProgressBar(skill);
            }, index * 200);
        });
    }

    createSkillProgressBar(skill) {
        const progressContainer = document.createElement('div');
        progressContainer.className = 'skill-progress';
        progressContainer.innerHTML = `
            <div class="skill-name">${skill.name}</div>
            <div class="progress-bar">
                <div class="progress-fill" data-level="${skill.level}"></div>
            </div>
            <div class="skill-percentage">${skill.level}%</div>
        `;

        // Add styles
        const styles = `
            .skill-progress {
                margin: 1rem 0;
                padding: 0.5rem;
            }
            .skill-name {
                font-weight: 600;
                margin-bottom: 0.5rem;
                color: var(--text-primary);
            }
            .progress-bar {
                background: rgba(0, 0, 0, 0.1);
                border-radius: 10px;
                height: 8px;
                overflow: hidden;
            }
            .progress-fill {
                height: 100%;
                background: linear-gradient(135deg, #667eea, #764ba2);
                border-radius: 10px;
                width: 0%;
                transition: width 1s ease-in-out;
            }
            .skill-percentage {
                text-align: right;
                font-size: 0.8rem;
                margin-top: 0.25rem;
                color: var(--text-muted);
            }
        `;

        if (!document.querySelector('#skill-styles')) {
            const styleSheet = document.createElement('style');
            styleSheet.id = 'skill-styles';
            styleSheet.textContent = styles;
            document.head.appendChild(styleSheet);
        }

        const skillsContainer = document.querySelector('#skills');
        skillsContainer.appendChild(progressContainer);

        // Animate progress bar
        setTimeout(() => {
            const progressFill = progressContainer.querySelector('.progress-fill');
            progressFill.style.width = `${skill.level}%`;
        }, 100);
    }

    createTypingEffect() {
        const tagline = document.querySelector('.tagline');
        if (!tagline) return;

        const text = tagline.textContent;
        tagline.textContent = '';
        tagline.style.borderRight = '2px solid #667eea';

        let i = 0;
        const typeWriter = () => {
            if (i < text.length) {
                tagline.textContent += text.charAt(i);
                i++;
                setTimeout(typeWriter, 100);
            } else {
                // Remove cursor after typing
                setTimeout(() => {
                    tagline.style.borderRight = 'none';
                }, 1000);
            }
        };

        setTimeout(typeWriter, 1000);
    }

    copyToClipboard(text, message) {
        if (navigator.clipboard) {
            navigator.clipboard.writeText(text).then(() => {
                this.showNotification(message);
            });
        } else {
            // Fallback for older browsers
            const textArea = document.createElement('textarea');
            textArea.value = text;
            document.body.appendChild(textArea);
            textArea.select();
            document.execCommand('copy');
            document.body.removeChild(textArea);
            this.showNotification(message);
        }
    }

    showNotification(message) {
        const notification = document.createElement('div');
        notification.className = 'notification';
        notification.textContent = message;
        
        Object.assign(notification.style, {
            position: 'fixed',
            top: '80px',
            right: '20px',
            background: '#667eea',
            color: 'white',
            padding: '1rem 1.5rem',
            borderRadius: '8px',
            zIndex: '1001',
            transform: 'translateX(100%)',
            transition: 'transform 0.3s ease',
            boxShadow: '0 4px 12px rgba(0, 0, 0, 0.15)'
        });

        document.body.appendChild(notification);

        setTimeout(() => {
            notification.style.transform = 'translateX(0)';
        }, 100);

        setTimeout(() => {
            notification.style.transform = 'translateX(100%)';
            setTimeout(() => {
                document.body.removeChild(notification);
            }, 300);
        }, 3000);
    }

    trackEvent(action, category) {
        // Analytics tracking (Google Analytics, etc.)
        console.log(`Event tracked: ${category} - ${action}`);
        
        // You can integrate with Google Analytics here:
        // gtag('event', action, { event_category: category });
    }
}

// Initialize when DOM is loaded
document.addEventListener('DOMContentLoaded', () => {
    new BusinessCard();
    
    // Add CSS custom properties support
    const root = document.documentElement;
    if (!root.style.getPropertyValue('--bg-gradient')) {
        const style = document.createElement('style');
        style.textContent = `
            :root {
                --bg-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
                --card-bg: rgba(255, 255, 255, 0.95);
                --text-primary: #2d3748;
                --text-secondary: #4a5568;
                --text-muted: #718096;
            }
        `;
        document.head.appendChild(style);
    }
});
```

---

## Video 2.1: Elementos Semánticos y Accesibilidad

### 📋 Información del Video
- **Duración**: 15 minutos
- **Mini-Proyecto**: Blog Article Template
- **Tecnologías**: HTML5 semantic elements, ARIA, Screen readers

### 🎯 Objetivos
- ✅ Usar elementos semánticos correctamente
- ✅ Implementar ARIA labels y roles
- ✅ Crear contenido accesible
- ✅ Optimizar para lectores de pantalla

### 📝 HTML Semántico Completo:
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Artículo: Docker para Principiantes | Mi Blog Tech</title>
    
    <!-- SEO Meta Tags -->
    <meta name="description" content="Aprende Docker desde cero con esta guía completa para principiantes. Incluye ejemplos prácticos y mejores prácticas.">
    <meta name="keywords" content="docker, contenedores, desarrollo, devops, tutorial">
    <meta name="author" content="Tu Nombre">
    
    <!-- Open Graph -->
    <meta property="og:title" content="Docker para Principiantes - Guía Completa">
    <meta property="og:description" content="Tutorial completo de Docker con ejemplos prácticos">
    <meta property="og:type" content="article">
    <meta property="og:url" content="https://tu-blog.com/docker-principiantes">
    
    <!-- Schema.org for articles -->
    <script type="application/ld+json">
    {
        "@context": "https://schema.org",
        "@type": "Article",
        "headline": "Docker para Principiantes - Guía Completa",
        "author": {
            "@type": "Person",
            "name": "Tu Nombre"
        },
        "datePublished": "2024-01-15",
        "dateModified": "2024-01-15",
        "description": "Aprende Docker desde cero con esta guía completa",
        "mainEntityOfPage": "https://tu-blog.com/docker-principiantes"
    }
    </script>
</head>
<body>
    <!-- Skip to main content for accessibility -->
    <a href="#main-content" class="skip-link">Saltar al contenido principal</a>
    
    <!-- Main site header -->
    <header role="banner">
        <div class="header-container">
            <div class="logo">
                <h1><a href="/" aria-label="Ir al inicio">Mi Blog Tech</a></h1>
            </div>
            
            <nav role="navigation" aria-label="Navegación principal">
                <ul>
                    <li><a href="/" aria-current="page">Inicio</a></li>
                    <li><a href="/tutoriales">Tutoriales</a></li>
                    <li><a href="/proyectos">Proyectos</a></li>
                    <li><a href="/sobre-mi">Sobre Mí</a></li>
                    <li><a href="/contacto">Contacto</a></li>
                </ul>
            </nav>

            <!-- Mobile menu button -->
            <button 
                class="mobile-menu-toggle" 
                aria-controls="main-nav" 
                aria-expanded="false"
                aria-label="Abrir menú de navegación"
            >
                <span class="hamburger-line"></span>
                <span class="hamburger-line"></span>
                <span class="hamburger-line"></span>
            </button>
        </div>
    </header>

    <!-- Breadcrumb navigation -->
    <nav aria-label="Breadcrumb" class="breadcrumb">
        <ol>
            <li><a href="/">Inicio</a></li>
            <li><a href="/tutoriales">Tutoriales</a></li>
            <li aria-current="page">Docker para Principiantes</li>
        </ol>
    </nav>

    <!-- Main content area -->
    <main id="main-content" role="main">
        <!-- Article content -->
        <article>
            <!-- Article header -->
            <header class="article-header">
                <h1>Docker para Principiantes: Guía Completa 2024</h1>
                
                <div class="article-meta">
                    <div class="author-info">
                        <img 
                            src="/images/autor.jpg" 
                            alt="Foto de Tu Nombre" 
                            class="author-avatar"
                        >
                        <div class="author-details">
                            <span class="author-name">Por <strong>Tu Nombre</strong></span>
                            <time datetime="2024-01-15" class="publication-date">
                                15 de enero, 2024
                            </time>
                        </div>
                    </div>
                    
                    <div class="article-stats">
                        <span class="reading-time" aria-label="Tiempo de lectura estimado">
                            📖 8 min lectura
                        </span>
                        <span class="difficulty" aria-label="Nivel de dificultad">
                            🎯 Principiante
                        </span>
                    </div>
                </div>

                <!-- Article tags -->
                <div class="article-tags" role="group" aria-label="Etiquetas del artículo">
                    <span class="tag">Docker</span>
                    <span class="tag">Contenedores</span>
                    <span class="tag">DevOps</span>
                    <span class="tag">Tutorial</span>
                </div>
            </header>

            <!-- Table of contents -->
            <aside class="table-of-contents" aria-labelledby="toc-heading">
                <h2 id="toc-heading">Contenido del artículo</h2>
                <nav aria-label="Índice del artículo">
                    <ol>
                        <li><a href="#que-es-docker">¿Qué es Docker?</a></li>
                        <li><a href="#instalacion">Instalación</a></li>
                        <li><a href="#primeros-pasos">Primeros pasos</a></li>
                        <li><a href="#dockerfile">Creando un Dockerfile</a></li>
                        <li><a href="#docker-compose">Docker Compose</a></li>
                        <li><a href="#mejores-practicas">Mejores prácticas</a></li>
                        <li><a href="#conclusion">Conclusión</a></li>
                    </ol>
                </nav>
            </aside>

            <!-- Article content sections -->
            <section id="que-es-docker" aria-labelledby="que-es-docker-heading">
                <h2 id="que-es-docker-heading">¿Qué es Docker?</h2>
                
                <p class="lead">
                    Docker es una plataforma de containerización que permite empaquetar aplicaciones 
                    junto con todas sus dependencias en contenedores ligeros y portables.
                </p>

                <figure>
                    <img 
                        src="/images/docker-diagram.png" 
                        alt="Diagrama que muestra la arquitectura de Docker con containers, imágenes y el Docker Engine"
                        loading="lazy"
                    >
                    <figcaption>
                        Arquitectura básica de Docker mostrando la relación entre containers, imágenes y el Docker Engine
                    </figcaption>
                </figure>

                <h3>Ventajas de usar Docker</h3>
                <ul>
                    <li><strong>Portabilidad:</strong> Los containers funcionan igual en cualquier sistema</li>
                    <li><strong>Eficiencia:</strong> Menor overhead que las máquinas virtuales</li>
                    <li><strong>Escalabilidad:</strong> Fácil scaling horizontal</li>
                    <li><strong>Aislamiento:</strong> Aplicaciones separadas sin conflictos</li>
                </ul>

                <!-- Accessible alert for important information -->
                <div class="alert alert-info" role="alert" aria-labelledby="important-note">
                    <h4 id="important-note">💡 Nota importante</h4>
                    <p>
                        Docker es diferente a las máquinas virtuales. Los containers comparten 
                        el kernel del sistema operativo, haciéndolos más eficientes.
                    </p>
                </div>
            </section>

            <section id="instalacion" aria-labelledby="instalacion-heading">
                <h2 id="instalacion-heading">Instalación de Docker</h2>
                
                <h3>Windows y macOS</h3>
                <ol>
                    <li>Descarga Docker Desktop desde <a href="https://docker.com" target="_blank" rel="noopener">docker.com</a></li>
                    <li>Ejecuta el instalador</li>
                    <li>Reinicia tu sistema</li>
                    <li>Verifica la instalación</li>
                </ol>

                <!-- Code block with proper semantics -->
                <div class="code-block" role="region" aria-labelledby="verification-code">
                    <h4 id="verification-code">Verificación de instalación</h4>
                    <pre><code class="language-bash" tabindex="0">
# Verificar versión de Docker
docker --version

# Verificar que Docker está funcionando
docker run hello-world
                    </code></pre>
                    <button 
                        class="copy-code-btn" 
                        aria-label="Copiar código al portapapeles"
                        data-code="docker --version"
                    >
                        📋 Copiar
                    </button>
                </div>

                <h3>Linux (Ubuntu/Debian)</h3>
                <div class="code-block" role="region" aria-labelledby="linux-install">
                    <h4 id="linux-install">Comandos de instalación para Linux</h4>
                    <pre><code class="language-bash" tabindex="0">
# Actualizar packages
sudo apt update

# Instalar dependencias
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# Agregar Docker GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Agregar repositorio Docker
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

# Instalar Docker
sudo apt install docker-ce

# Agregar usuario al grupo docker
sudo usermod -aG docker ${USER}
                    </code></pre>
                </div>
            </section>

            <!-- More sections would continue... -->
            
            <!-- Article footer with sharing and related content -->
            <footer class="article-footer">
                <div class="article-sharing" role="group" aria-label="Compartir artículo">
                    <h3>Compartir este artículo</h3>
                    <div class="sharing-buttons">
                        <a 
                            href="https://twitter.com/intent/tweet?text=Docker%20para%20Principiantes&url=https://tu-blog.com/docker-principiantes" 
                            target="_blank" 
                            rel="noopener"
                            aria-label="Compartir en Twitter"
                            class="share-btn twitter"
                        >
                            🐦 Twitter
                        </a>
                        <a 
                            href="https://www.linkedin.com/sharing/share-offsite/?url=https://tu-blog.com/docker-principiantes" 
                            target="_blank" 
                            rel="noopener"
                            aria-label="Compartir en LinkedIn"
                            class="share-btn linkedin"
                        >
                            💼 LinkedIn
                        </a>
                    </div>
                </div>

                <!-- Author bio -->
                <aside class="author-bio" aria-labelledby="author-bio-heading">
                    <h3 id="author-bio-heading">Sobre el autor</h3>
                    <div class="bio-content">
                        <img 
                            src="/images/autor.jpg" 
                            alt="Tu Nombre" 
                            class="author-photo"
                        >
                        <div class="bio-text">
                            <p>
                                <strong>Tu Nombre</strong> es Senior Software Engineer con más de 15 años 
                                de experiencia en desarrollo web y DevOps. Especialista en Docker, 
                                React y Node.js.
                            </p>
                            <a href="/sobre-mi" aria-label="Leer más sobre Tu Nombre">
                                Leer más sobre el autor
                            </a>
                        </div>
                    </div>
                </aside>
            </footer>
        </article>

        <!-- Related articles -->
        <aside class="related-articles" aria-labelledby="related-heading">
            <h2 id="related-heading">Artículos relacionados</h2>
            <div class="related-grid">
                <article class="related-card">
                    <h3><a href="/kubernetes-principiantes">Kubernetes para Principiantes</a></h3>
                    <p>Aprende los conceptos básicos de orquestación de containers con Kubernetes.</p>
                    <time datetime="2024-01-10">10 enero 2024</time>
                </article>
                
                <article class="related-card">
                    <h3><a href="/docker-compose-avanzado">Docker Compose Avanzado</a></h3>
                    <p>Técnicas avanzadas para manejar aplicaciones multi-container.</p>
                    <time datetime="2024-01-08">8 enero 2024</time>
                </article>
            </div>
        </aside>
    </main>

    <!-- Comments section -->
    <section class="comments-section" aria-labelledby="comments-heading">
        <h2 id="comments-heading">Comentarios</h2>
        
        <!-- Comment form -->
        <form class="comment-form" aria-labelledby="comment-form-heading">
            <h3 id="comment-form-heading">Deja tu comentario</h3>
            
            <div class="form-group">
                <label for="comment-name">Nombre *</label>
                <input 
                    type="text" 
                    id="comment-name" 
                    name="name" 
                    required 
                    aria-required="true"
                    aria-describedby="name-help"
                >
                <div id="name-help" class="form-help">Tu nombre será público</div>
            </div>

            <div class="form-group">
                <label for="comment-email">Email *</label>
                <input 
                    type="email" 
                    id="comment-email" 
                    name="email" 
                    required 
                    aria-required="true"
                    aria-describedby="email-help"
                >
                <div id="email-help" class="form-help">No será compartido públicamente</div>
            </div>

            <div class="form-group">
                <label for="comment-text">Comentario *</label>
                <textarea 
                    id="comment-text" 
                    name="comment" 
                    rows="4" 
                    required 
                    aria-required="true"
                    aria-describedby="comment-help"
                ></textarea>
                <div id="comment-help" class="form-help">Sé respetuoso en tus comentarios</div>
            </div>

            <button type="submit" class="submit-btn">Publicar comentario</button>
        </form>

        <!-- Existing comments -->
        <div class="comments-list" aria-label="Lista de comentarios">
            <article class="comment">
                <header class="comment-header">
                    <strong class="comment-author">María González</strong>
                    <time datetime="2024-01-16T10:30:00" class="comment-date">
                        16 enero 2024, 10:30
                    </time>
                </header>
                <div class="comment-content">
                    <p>Excelente tutorial! Me ayudó mucho a entender Docker desde cero. 
                    ¿Podrías hacer uno sobre Docker en producción?</p>
                </div>
                <button class="reply-btn" aria-label="Responder a María González">
                    Responder
                </button>
            </article>
        </div>
    </section>

    <!-- Site footer -->
    <footer role="contentinfo" class="site-footer">
        <div class="footer-content">
            <div class="footer-section">
                <h3>Mi Blog Tech</h3>
                <p>Tutoriales y recursos para desarrolladores web y DevOps.</p>
            </div>
            
            <div class="footer-section">
                <h3>Enlaces útiles</h3>
                <ul>
                    <li><a href="/tutoriales">Tutoriales</a></li>
                    <li><a href="/proyectos">Proyectos</a></li>
                    <li><a href="/recursos">Recursos</a></li>
                </ul>
            </div>
            
            <div class="footer-section">
                <h3>Sígueme</h3>
                <div class="social-links" role="group" aria-label="Enlaces a redes sociales">
                    <a 
                        href="https://twitter.com/tunombre" 
                        target="_blank" 
                        rel="noopener"
                        aria-label="Seguir en Twitter"
                    >
                        Twitter
                    </a>
                    <a 
                        href="https://github.com/tuusuario" 
                        target="_blank" 
                        rel="noopener"
                        aria-label="Ver proyectos en GitHub"
                    >
                        GitHub
                    </a>
                </div>
            </div>
        </div>
        
        <div class="footer-bottom">
            <p>&copy; 2024 Mi Blog Tech. Todos los derechos reservados.</p>
        </div>
    </footer>

    <!-- Back to top button -->
    <button 
        class="back-to-top" 
        aria-label="Volver al inicio de la página"
        style="display: none;"
    >
        ↑ Subir
    </button>
</body>
</html>
```

---

Continúo con los demás videos. ¿Quieres que siga con todos los videos restantes del Curso 1 y luego proceda con los Cursos 2-6 completos?
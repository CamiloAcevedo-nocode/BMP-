# BMP-
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bed Management Platform: Aplicación Interactiva</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Lato:wght@400;700&family=Montserrat:wght@400;700&family=Open+Sans:wght@400;700&display=swap" rel="stylesheet">
    <!-- Elecciones de Visualización y Contenido:
        - Desafío: Grid con iconos Unicode y texto detallado (Informar).
        - Solución (Diagrama): Diagrama lineal HTML/CSS/Tailwind para mostrar interconexión de áreas con el centro Dashboard (Organizar). Atractivo y moderno.
        - Flujo Optimizado: Secciones desplegables (HTML/CSS/JS) con descripciones textuales y la imagen de flujo correspondiente para cada etapa (Informar, Organizar).
        - Beneficios/KPIs: Lista con iconos Unicode. Gráfico de barras (Chart.js) para % mejoras (Comparar, Informar). `maintainAspectRatio: false`, contenedor `div` con `w-full max-w_lg mx-auto h-80 max-h-96`.
        - Plan de Acción (Timeline): HTML/CSS/Tailwind horizontal (Organizar, Informar).
        - Justificación: Métodos elegidos por claridad, interactividad sin complejidad excesiva, y cumplimiento de restricciones (NO SVG/Mermaid).
        -->
    <style>
        :root {
            --primary-blue-dark: #004080; /* Azul oscuro para títulos */
            --primary-blue-light: #007BFF; /* Azul vibrante */
            --background-dark: #1A1A1A; /* Fondo muy oscuro */
            --background-medium: #2A2A2A; /* Fondo de sección oscuro */
            --background-light: #FFFFFF; /* Fondo de tarjetas blanco */
            --text-light: #E0E0E0; /* Texto principal claro */
            --text-medium: #A0A0A0; /* Gris medio */
            --text-dark: #333333; /* Gris oscuro */
            --border-dark: #444444; /* Borde gris oscuro */
            --border-light: #CCCCCC; /* Borde gris claro */
        }

        body {
            font-family: 'Open Sans', sans-serif;
            background-color: var(--background-dark);
            color: var(--text-light);
        }
        .section-title {
            font-family: 'Montserrat', sans-serif;
            font-weight: 700;
            color: var(--primary-blue-dark); /* Títulos principales en azul oscuro */
        }
        .nav-link {
            font-family: 'Lato', sans-serif;
            transition: color 0.3s ease, border-bottom 0.3s ease;
            color: var(--text-light);
        }
        .nav-link:hover, .nav-link.active {
            color: var(--background-light); /* Blanco puro al pasar el ratón/activo */
            border-bottom: 2px solid var(--primary-blue-light); /* Subrayado azul sutil */
        }
        .card {
            background-color: var(--background-light); /* Fondo de tarjetas blanco */
            border-radius: 0.75rem;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
        }
        .button-primary {
            background-color: var(--primary-blue-light);
            color: white;
            transition: background-color 0.3s ease, transform 0.2s ease;
            border-radius: 0.5rem;
            padding: 0.75rem 1.5rem;
            font-weight: 600;
        }
        .button-primary:hover {
            background-color: #0056b3;
            transform: translateY(-2px);
        }
        .accent-blue-dark { color: var(--primary-blue-dark); } /* Azul oscuro para acentos */
        .accent-blue-light { color: var(--primary-blue-light); } /* Azul vibrante para acentos */

        .timeline-item {
            position: relative;
            padding-bottom: 2rem;
            padding-left: 2.5rem;
        }
        .timeline-item:not(:last-child)::before {
            content: '';
            position: absolute;
            left: 20px;
            top: 20px;
            bottom: 0;
            width: 2px;
            background-color: var(--primary-blue-light);
        }
        .timeline-icon {
            position: absolute;
            left: 0;
            top: 0;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.25rem;
            background-color: var(--primary-blue-light);
            color: white;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
            background-color: white; /* Fondo del gráfico blanco */
            border-radius: 0.5rem;
            padding: 1rem;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }
        .nav-scrolled {
            background-color: rgba(0, 0, 0, 0.9);
            box-shadow: 0 4px 8px rgba(0,0,0,0.3);
        }

        /* Estilos para el flujo de trabajo colapsable */
        .workflow-stage {
            cursor: pointer;
            border: 1px solid var(--border-dark);
            padding: 1rem;
            margin-bottom: 0.75rem;
            border-radius: 0.5rem;
            background-color: var(--text-dark); /* Fondo gris oscuro para las etapas */
            color: white; /* Texto blanco para contraste */
            transition: background-color 0.3s ease, border-color 0.3s ease, transform 0.2s ease;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .workflow-stage:hover {
            transform: translateY(-3px);
            background-color: #444444; /* Ligeramente más claro al pasar el ratón */
        }
        .workflow-stage.active {
            border-color: var(--primary-blue-light);
            background-color: var(--primary-blue-dark); /* Azul oscuro para la etapa activa */
            color: white;
        }
        .workflow-details {
            display: none;
            padding: 1.5rem;
            margin-top: 0.5rem;
            background-color: var(--background-medium); /* Fondo oscuro para los detalles */
            border-radius: 0.5rem;
            border: 1px dashed var(--primary-blue-light);
            overflow: hidden;
            max-height: 0;
            transition: max-height 0.5s ease-out, padding 0.5s ease-out;
            color: var(--text-light);
        }
        .workflow-details.active {
            display: block;
            max-height: 1200px;
            padding: 1.5rem;
        }
        .workflow-details img {
            max-width: 100%;
            height: auto;
            margin-bottom: 1.5rem;
            border-radius: 0.5rem;
            box-shadow: 0 2px 8px rgba(0,0,0,0.2);
        }

        /* Estilos del Diagrama Lineal */
        .linear-diagram-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 2rem;
            margin-top: 3rem;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
        }
        .linear-node {
            background-color: var(--background-light); /* Nodos en blanco */
            border: 1px solid var(--primary-blue-light); /* Borde azul */
            border-radius: 0.75rem;
            padding: 1.2rem 1.8rem;
            text-align: center;
            font-weight: 600;
            box-shadow: 0 6px 15px rgba(0,0,0,0.15);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            width: fit-content;
            min-width: 180px;
            color: var(--primary-blue-dark); /* Texto azul oscuro en los nodos */
        }
        .linear-node.center-dashboard {
            background-color: var(--primary-blue-light); /* Dashboard en azul vibrante */
            color: white;
            border-radius: 9999px;
            padding: 1.8rem 2.5rem;
            font-size: 1.3rem;
            box-shadow: 0 10px 25px rgba(0,0,0,0.4);
        }
        .linear-node:hover {
            transform: translateY(-7px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }
        .linear-node.center-dashboard:hover {
            transform: translateY(-10px) scale(1.02);
        }
        .linear-arrow {
            width: 3px;
            height: 40px;
            background-color: var(--primary-blue-light); /* Flechas azules */
            position: relative;
            border-radius: 1px;
        }
        .linear-arrow::after {
            content: '';
            position: absolute;
            width: 0;
            height: 0;
            border-left: 8px solid transparent;
            border-right: 8px solid transparent;
            border-top: 8px solid var(--primary-blue-light);
            bottom: -8px;
            left: 50%;
            transform: translateX(-50%);
        }
        /* Nuevos estilos para el tema moderno negro/blanco/azul */
        .bg-blue-50 {
            background-color: var(--background-medium); /* Fondo de sección oscuro */
        }
        .bg-green-50 { /* Cambiado a un tono oscuro para consistencia */
            background-color: var(--background-medium);
        }
        .bg-orange-50 { /* Cambiado a un tono oscuro para consistencia */
            background-color: var(--background-medium);
        }
        .accent-green { /* Cambiado a azul para consistencia */
            color: var(--primary-blue-light);
        }
        .accent-orange { /* Cambiado a azul para consistencia */
            color: var(--primary-blue-light);
        }
        .text-blue-600 {
            color: var(--primary-blue-light); /* Azul vibrante */
        }
        .text-blue-700 {
            color: var(--primary-blue-light); /* Azul vibrante */
        }
        .text-gray-500 {
            color: var(--text-medium); /* Gris medio */
        }
        .text-gray-600 {
            color: var(--text-light); /* Gris claro */
        }
        .text-gray-700 {
            color: var(--text-light); /* Gris claro */
        }
        .text-orange-500 { /* Cambiado a azul para consistencia */
            color: var(--primary-blue-light);
        }
        .text-orange-700 { /* Cambiado a azul para consistencia */
            color: var(--primary-blue-light);
        }
        .text-blue-800 {
            color: var(--text-light); /* Gris claro para texto en fondos oscuros */
        }
        .footer {
            background-color: #000000; /* Fondo negro puro para el pie de página */
            color: var(--text-medium); /* Texto gris medio para el pie de página */
        }

        /* Estilos para las fases del plan de acción colapsables */
        .timeline-stage {
            cursor: pointer;
            border: 1px solid var(--border-dark);
            padding: 1rem;
            margin-bottom: 0.75rem;
            border-radius: 0.5rem;
            background-color: var(--text-dark); /* Fondo gris oscuro para las etapas */
            color: white; /* Texto blanco para contraste */
            transition: background-color 0.3s ease, border-color 0.3s ease, transform 0.2s ease;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .timeline-stage:hover {
            transform: translateY(-3px);
            background-color: #444444; /* Ligeramente más claro al pasar el ratón */
        }
        .timeline-stage.active {
            border-color: var(--primary-blue-light);
            background-color: var(--primary-blue-dark); /* Azul oscuro para la etapa activa */
            color: white;
        }
        .timeline-details {
            display: none;
            padding: 1.5rem;
            margin-top: 0.5rem;
            background-color: var(--background-medium); /* Fondo oscuro para los detalles */
            border-radius: 0.5rem;
            border: 1px dashed var(--primary-blue-light);
            overflow: hidden;
            max-height: 0;
            transition: max-height 0.5s ease-out, padding 0.5s ease-out;
            color: var(--text-light);
        }
        .timeline-details.active {
            display: block;
            max-height: 500px; /* Suficiente para el texto */
            padding: 1.5rem;
        }
    </style>
</head>
<body class="bg-gray-100 text-gray-800">

    <nav id="navbar" class="fixed top-0 left-0 right-0 z-50 p-4 transition-all duration-300">
        <div class="container mx-auto flex justify-between items-center">
            <div class="text-2xl font-bold text-blue-700">BMP Interactivo</div>
            <div class="space-x-4 hidden md:flex">
                <a href="#inicio" class="nav-link px-3 py-2 text-sm font-medium">Inicio</a>
                <a href="#desafio" class="nav-link px-3 py-2 text-sm font-medium">Desafío</a>
                <a href="#solucion" class="nav-link px-3 py-2 text-sm font-medium">Solución</a>
                <a href="#flujo" class="nav-link px-3 py-2 text-sm font-medium">Flujo</a>
                <a href="#beneficios" class="nav-link px-3 py-2 text-sm font-medium">Beneficios</a>
                <a href="#implementacion" class="nav-link px-3 py-2 text-sm font-medium">Implementación</a>
                <a href="#plan" class="nav-link px-3 py-2 text-sm font-medium">Plan</a>
                <a href="#accion" class="nav-link px-3 py-2 text-sm font-medium">Acción</a>
                <a href="#conclusion" class="nav-link px-3 py-2 text-sm font-medium">Conclusión</a>
            </div>
            <button id="mobileMenuButton" class="md:hidden text-gray-700 focus:outline-none">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path></svg>
            </button>
        </div>
        <div id="mobileMenu" class="hidden md:hidden bg-white shadow-lg rounded-lg mt-2">
            <a href="#inicio" class="block nav-link px-4 py-2 text-sm">Inicio</a>
            <a href="#desafio" class="block nav-link px-4 py-2 text-sm">Desafío</a>
            <a href="#solucion" class="block nav-link px-4 py-2 text-sm">Solución</a>
            <a href="#flujo" class="block nav-link px-4 py-2 text-sm">Flujo</a>
            <a href="#beneficios" class="block nav-link px-4 py-2 text-sm">Beneficios</a>
            <a href="#implementacion" class="block nav-link px-4 py-2 text-sm">Implementación</a>
            <a href="#plan" class="block nav-link px-4 py-2 text-sm">Plan</a>
            <a href="#accion" class="block nav-link px-4 py-2 text-sm">Acción</a>
            <a href="#conclusion" class="block nav-link px-4 py-2 text-sm">Conclusión</a>
        </div>
    </nav>

    <div class="container mx-auto px-4 pt-20">

        <section id="inicio" class="min-h-screen flex flex-col justify-center items-center text-center py-16">
            <img src="https://i.imgur.com/iKfYdtR.png" alt="Logo Hospital Clínico Félix Bulnes" class="w-32 h-32 mb-8 rounded-full shadow-lg" onerror="this.src='https://placehold.co/150x150/cccccc/333333?text=Logo+Error'">
            <h1 class="section-title text-4xl md:text-5xl mb-4">Bed Management Platform</h1>
            <h2 class="text-xl md:text-2xl text-gray-600 mb-6">Hacia una Gestión de Camas Inteligente y Eficiente</h2>
            <p class="text-lg text-gray-600 mb-8">Propuesta de Implementación para el Hospital Clínico Félix Bulnes</p>
             <p class="mt-12 text-gray-500">Desliza para explorar <span class="text-2xl">👇</span></p>
        </section>

        <section id="desafio" class="py-16">
            <h2 class="section-title text-3xl text-center mb-12">El Desafío Actual - Una Oportunidad de Mejora Urgente</h2>
            <p class="text-lg text-gray-700 text-center mb-10 max-w-3xl mx-auto">
                La gestión actual de camas en el hospital presenta varias áreas críticas que impactan la eficiencia operativa y la calidad de la atención al paciente. Identificar estos desafíos es el primer paso hacia una transformación significativa.
            </p>
            <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
                <div class="card p-6 text-left">
                    <div class="text-5xl mb-4 accent-blue-light">👥</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Coordinación Fragmentada de Movimientos</h3>
                    <p class="text-gray-600 text-sm">La ausencia de un canal único obliga a convocar reuniones físicas periódicas entre unidades, lo que retrasa la toma de decisiones y la asignación de camas.</p>
                </div>
                <div class="card p-6 text-left">
                    <div class="text-5xl mb-4 accent-blue-light">📝📞</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Dependencia de Procesos Manuales y Comunicaciones Ad Hoc</h3>
                    <p class="text-gray-600 text-sm">Los responsables de cada servicio intercambian información por llamadas, correos o mensajería instantánea, aumentando el riesgo de pérdidas de datos y malentendidos.</p>
                </div>
                <div class="card p-6 text-left">
                    <div class="text-5xl mb-4 accent-blue-light">📉🛏️</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Gestión Reactiva de Altas Médicas</h3>
                    <p class="text-gray-600 text-sm">No hay un mecanismo automatizado que anticipe las fechas de alta ni reserve camas de manera proactiva, provocando congestión cuando no hay previsión.</p>
                </div>
                <div class="card p-6 text-left">
                    <div class="text-5xl mb-4 accent-blue-light">❓📊</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Visibilidad Parcial de la Capacidad Real</h3>
                    <p class="text-gray-600 text-sm">Sin un dashboard unificado, las Subdirecciones y la Unidad de Gestión de Camas carecen de panorama global sobre ocupación y disponibilidad, dificultando la priorización y reasignación ágil de recursos.</p>
                </div>
                <div class="card p-6 text-left">
                    <div class="text-5xl mb-4 accent-blue-light">🤝</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Desalineación de Roles y Responsabilidades</h3>
                    <p class="text-gray-600 text-sm">En reuniones de coordinación no siempre están presentes todos los actores clave, lo que genera decisiones parciales y retrabajo operativo.</p>
                </div>
                <div class="card p-6 text-left">
                    <div class="text-5xl mb-4 accent-blue-light">🚨</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Ineficiencia en la Gestión de Excepciones</h3>
                    <p class="text-gray-600 text-sm">Situaciones como emergencias o cambios de último minuto requieren llamadas urgentes y replanificaciones manuales, con altos tiempos de respuesta.</p>
                </div>
            </div>
            <p class="text-center text-xl italic text-gray-700 mt-12">"Cada minuto perdido en la gestión de camas impacta la atención al paciente y la eficiencia del hospital."</p>
        </section>

        <section id="solucion" class="py-16 bg-blue-50">
            <h2 class="section-title text-3xl text-center mb-12">Nuestra Solución</h2>
            <p class="text-lg text-gray-700 text-center mb-10 max-w-3xl mx-auto">
                La Bed Management Platform se propone como una solución integral para centralizar, optimizar y controlar la gestión de camas en tiempo real, transformando los desafíos actuales en oportunidades de eficiencia.
            </p>
            <div class="text-center mb-10">
                <h3 class="text-2xl font-semibold accent-blue-dark mb-4">Objetivo del Proyecto</h3>
                <ul class="list-disc list-inside inline-block text-left text-gray-700 space-y-2">
                    <li>Reducir tiempos de espera, traslados y estancias hospitalarias.</li>
                    <li>Centralizar la información en un dashboard de control de mando.</li>
                    <li>Facilitar la toma de decisiones basada en datos en tiempo real.</li>
                </ul>
            </div>
            <div class="card p-6 md:p-10 max-w-4xl mx-auto">
                <h4 class="text-xl font-semibold accent-blue-dark text-center mb-8">Flujo de Interconexión con el Dashboard Central</h4>
                <div class="linear-diagram-container">
                    <div class="linear-node">Urgencia</div>
                    <div class="linear-arrow"></div>
                    <div class="linear-node">Servicios Hospitalizados</div>
                    <div class="linear-arrow"></div>
                    <div class="linear-node">Reunión de Cama</div>
                    <div class="linear-arrow"></div>
                    <div class="linear-node center-dashboard">BMP<br>Dashboard</div>
                </div>
            </div>
        </section>

        <section id="flujo" class="py-16">
            <h2 class="section-title text-3xl text-center mb-12">Flujo de Trabajo Optimizado con BMP</h2>
            <p class="text-lg text-gray-700 text-center mb-10 max-w-3xl mx-auto">
                La BMP rediseña el viaje del paciente, conectando cada etapa de manera eficiente. Haga clic en cada etapa para ver los detalles de cómo la BMP facilita el proceso.
            </p>
            <div class="max-w-5xl mx-auto">
                <div id="workflowContainer">
                    <div class="workflow-stage" data-stage="urgencia">
                        <h4 class="text-lg font-semibold text-white flex items-center">
                            Urgencia
                        </h4>
                    </div>
                    <div class="workflow-details" id="details-urgencia">
                        <img src="https://i.imgur.com/DSBhGr5.png" alt="Diagrama de Flujo de Trabajo Urgencia" class="w-full h-auto rounded-lg shadow-md" onerror="this.src='https://placehold.co/800x600/cccccc/333333?text=Error+Carga+Flujo+1'">
                        <p><strong>Acciones BMP:</strong></p>
                        <ul class="list-disc list-inside ml-4 text-sm space-y-1">
                            <li>Registro digital de datos básicos del paciente.</li>
                            <li>Alerta automática por tiempos de espera prolongados (ej. >300 min).</li>
                            <li>Verificación de disponibilidad y lugar clínico en módulo SUA.</li>
                            <li>Documentación de decisión (Hospitalizar / Alta).</li>
                            <li>Si se hospitaliza: asignación provisional y notificación a Reunión de Camas.</li>
                        </ul>
                    </div>

                    <div class="workflow-stage" data-stage="servicios-hospitalizados">
                        <h4 class="text-lg font-semibold text-white flex items-center">
                            Servicios Hospitalizados
                        </h4>
                    </div>
                    <div class="workflow-details" id="details-servicios-hospitalizados">
                        <img src="https://i.imgur.com/fEA8GjE.png" alt="Diagrama de Flujo de Trabajo Servicios Hospitalizados" class="w-full h-auto rounded-lg shadow-md" onerror="this.src='https://placehold.co/800x600/cccccc/333333?text=Error+Carga+Flujo+2'">
                        <p><strong>Acciones BMP:</strong></p>
                        <ul class="list-disc list-inside ml-4 text-sm space-y-1">
                            <li>Visibilidad de disponibilidad actualizada de camas para todos los asistentes.</li>
                            <li>Visualización de requerimientos de traslado y pacientes pendientes.</li>
                            <li>Propuesta de asignaciones basada en prioridad clínica y preparación de cama.</li>
                            <li>Validación por coordinadores y asignación definitiva en la BMP.</li>
                        </ul>
                    </div>

                    <div class="workflow-stage" data-stage="reunion-cama">
                        <h4 class="text-lg font-semibold text-white flex items-center">
                            Reunión de Cama
                        </h4>
                    </div>
                    <div class="workflow-details" id="details-reunion-cama">
                        <img src="https://i.imgur.com/88LUzi3.png" alt="Diagrama de Flujo de Trabajo Reunión de Camas" class="w-full h-auto rounded-lg shadow-md" onerror="this.src='https://placehold.co/800x600/cccccc/333333?text=Error+Carga+Flujo+3'">
                        <p><strong>Acciones BMP:</strong></p>
                        <ul class="list-disc list-inside ml-4 text-sm space-y-1">
                            <li>Registro EME-GRD de avances clínicos, hitos y tiempos de procedimientos.</li>
                            <li>Alertas tempranas por demoras (exámenes, Interconsultas) según umbrales GRD.</li>
                            <li>Supervisión de alertas por Médico Gestor.</li>
                            <li>Actualización del plan de egreso y fecha de alta/traslado en la BMP.</li>
                            <li>Generación de reportes automáticos de movimientos.</li>
                        </ul>
                    </div>
                </div>
                <p class="text-center text-lg italic text-gray-700 mt-10">"La BMP integra cada paso, asegurando información consistente y acciones coordinadas."</p>
            </div>
        </section>

        <section id="beneficios" class="py-16 bg-blue-50">
            <h2 class="section-title text-3xl text-center mb-12 accent-blue-dark">Impacto Medible: Beneficios Concretos y KPIs Clave</h2>
            <p class="text-lg text-gray-700 text-center mb-10 max-w-3xl mx-auto">
                La implementación de la BMP no solo moderniza procesos, sino que también genera beneficios tangibles y medibles, cruciales para la toma de decisiones estratégicas y la mejora continua de la calidad asistencial.
            </p>
            <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8 mb-12">
                <div class="card p-6">
                    <div class="text-4xl accent-blue-light mb-3">⏱️</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Tiempo de Asignación de Cama</h3>
                    <p class="text-gray-600 text-sm">Reducción del 30%.</p>
                </div>
                <div class="card p-6">
                    <div class="text-4xl accent-blue-light mb-3">🗓️✔️</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Cumplimiento de Plazos de Alta</h3>
                    <p class="text-gray-600 text-sm">Meta del 90% de cumplimiento.</p>
                </div>
                <div class="card p-6">
                    <div class="text-4xl accent-blue-light mb-3">↔️</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Tiempo en Traslados Internos</h3>
                    <p class="text-gray-600 text-sm">Reducción del 50%.</p>
                </div>
                <div class="card p-6">
                    <div class="text-4xl accent-blue-light mb-3">📊📈</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Visibilidad Integral</h3>
                    <p class="text-gray-600 text-sm">Dashboard para decisiones informadas y proactivas.</p>
                </div>
                <div class="card p-6">
                    <div class="text-4xl accent-blue-light mb-3">⚙️</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Automatización</h3>
                    <p class="text-gray-600 text-sm">Alertas, recordatorios, optimización de flujos.</p>
                </div>
                <div class="card p-6">
                    <div class="text-4xl accent-blue-light mb-3">🔍🔒</div>
                    <h3 class="text-xl font-semibold text-blue-700 mb-2">Trazabilidad y Seguridad</h3>
                    <p class="text-gray-600 text-sm">Registro de acciones, cumplimiento normativo.</p>
                </div>
            </div>
            <div class="card p-6">
                <h3 class="text-xl font-semibold accent-blue-dark text-center mb-4">Visualización de Mejoras Esperadas</h3>
                <div class="chart-container">
                    <canvas id="kpiChart"></canvas>
                </div>
                <p class="text-sm text-gray-600 text-center mt-4">Gráfico ilustrativo de las metas de mejora porcentual en indicadores clave.</p>
            </div>
        </section>

        <section id="implementacion" class="py-16">
            <h2 class="section-title text-3xl text-center mb-12">Modelo de Implementación Recomendado</h2>
            <p class="text-lg text-gray-700 text-center mb-10 max-w-3xl mx-auto">
                Tras un análisis exhaustivo, se recomienda la Opción 2: Desarrollo Interno Especializado. Esta estrategia ofrece el balance óptimo entre control, adaptabilidad y sostenibilidad a largo plazo para el hospital.
            </p>
            <div class="card p-8 max-w-3xl mx-auto bg-blue-50">
                <h3 class="text-2xl font-semibold accent-blue-dark text-center mb-6">La Estrategia Óptima: Desarrollo Interno Especializado (Opción 2)</h3>
                <ul class="space-y-4">
                    <li class="flex items-start">
                        <span class="text-3xl mr-3 accent-blue-light">🎛️</span>
                        <div>
                            <h4 class="font-semibold accent-blue-dark">Control Total</h4>
                            <p class="text-gray-600 text-sm">Adaptación precisa a nuestras necesidades actuales y evolución futura de la plataforma.</p>
                        </div>
                    </li>
                    <li class="flex items-start">
                        <span class="text-3xl mr-3 accent-blue-light">🛡️</span>
                        <div>
                            <h4 class="font-semibold accent-blue-dark">Seguridad y Cumplimiento</h4>
                            <p class="text-gray-600 text-sm">Máxima protección de datos sensibles (Leyes 19.628, 20.584) y normativas internas.</p>
                        </div>
                    </li>
                    <li class="flex items-start">
                        <span class="text-3xl mr-3 accent-blue-light">🧠</span>
                        <div>
                            <h4 class="font-semibold accent-blue-dark">Conocimiento Interno (Sostenibilidad)</h4>
                            <p class="text-gray-600 text-sm">Desarrollo de capacidades internas y menor dependencia de proveedores externos a largo plazo.</p>
                        </div>
                    </li>
                    <li class="flex items-start">
                        <span class="text-3xl mr-3 accent-blue-light">🪜</span>
                        <div>
                            <h4 class="font-semibold accent-blue-dark">Escalabilidad Robusta</h4>
                            <p class="text-gray-600 text-sm">Plataforma preparada para el crecimiento del hospital y futuras integraciones con otros sistemas.</p>
                        </div>
                    </li>
                </ul>
                <p class="text-center text-lg italic text-blue-800 mt-8">"Una inversión estratégica en autonomía, calidad y la capacidad de adaptación del Hospital."</p>
            </div>
        </section>

        <section id="plan" class="py-16 bg-blue-50">
            <h2 class="section-title text-3xl text-center mb-12 accent-blue-dark">Plan de Acción y Cronograma (Simplificado)</h2>
            <p class="text-lg text-gray-700 text-center mb-10 max-w-3xl mx-auto">
                Presentamos una hoja de ruta clara y concisa para la implementación de la BMP, estimada en 6 meses. Cada fase tiene objetivos y entregables definidos para asegurar un progreso medible y eficiente.
            </p>
            <div class="max-w-3xl mx-auto">
                <div id="timelineContainer">
                    <div class="timeline-stage" data-stage="fase1">
                        <h4 class="text-xl font-semibold text-white flex items-center">
                            Fase 1: Diseño Conceptual <span class="text-sm font-normal text-white ml-2">(2 Semanas)</span>
                        </h4>
                    </div>
                    <div class="timeline-details" id="details-fase1">
                        <p class="text-gray-600">Talleres de requerimientos, validación de flujos, especificaciones funcionales.</p>
                    </div>

                    <div class="timeline-stage" data-stage="fase2">
                        <h4 class="text-xl font-semibold text-white flex items-center">
                            Fase 2: Desarrollo del Prototipo <span class="text-sm font-normal text-white ml-2">(6 Semanas)</span>
                        </h4>
                    </div>
                    <div class="timeline-details" id="details-fase2">
                        <p class="text-gray-600">Configuración inicial, desarrollo de módulos críticos, pruebas unitarias.</p>
                    </div>

                    <div class="timeline-stage" data-stage="fase3">
                        <h4 class="text-xl font-semibold text-white flex items-center">
                            Fase 3: Prueba Piloto <span class="text-sm font-normal text-white ml-2">(4 Semanas)</span>
                        </h4>
                    </div>
                    <div class="timeline-details" id="details-fase3">
                        <p class="text-gray-600">Despliegue en servicio seleccionado, capacitación, monitoreo de KPIs iniciales.</p>
                    </div>

                    <div class="timeline-stage" data-stage="fase4">
                        <h4 class="text-xl font-semibold text-white flex items-center">
                            Fase 4: Implementación Hospitalaria <span class="text-sm font-normal text-white ml-2">(8 Semanas)</span>
                        </h4>
                    </div>
                    <div class="timeline-details" id="details-fase4">
                        <p class="text-gray-600">Despliegue progresivo, formación general, comité de seguimiento.</p>
                    </div>

                    <div class="timeline-stage" data-stage="fase5">
                        <h4 class="text-xl font-semibold text-white flex items-center">
                            Fase 5: Optimización y Escalamiento <span class="text-sm font-normal text-white ml-2">(4 Semanas)</span>
                        </h4>
                    </div>
                    <div class="timeline-details" id="details-fase5">
                        <p class="text-gray-600">Análisis de datos, mejoras, documentación final, plan de sostenibilidad.</p>
                    </div>
                </div>
            </div>
            <p class="text-center text-sm italic text-gray-600 mt-8">"Cronograma detallado disponible en el informe completo."</p>
        </section>

        <section id="accion" class="py-16">
            <h2 class="section-title text-3xl text-center mb-12">Llamado a la Acción<br>Su Decisión es Clave</h2>
            <p class="text-lg text-gray-700 text-center mb-10 max-w-3xl mx-auto">
                Para transformar la gestión de camas y materializar los beneficios presentados, solicitamos su aprobación en los siguientes puntos cruciales. Su respaldo es fundamental para iniciar este proyecto estratégico.
            </p>
            <div class="card p-8 max-w-2xl mx-auto bg-blue-50">
                <h3 class="text-2xl font-semibold accent-blue-dark text-center mb-6">Solicitudes Claras para Aprobación</h3>
                <ul class="space-y-4">
                    <li class="flex items-start">
                        <span class="text-3xl mr-3 accent-blue-light">✅</span>
                        <p class="text-gray-700">Aprobar la conformación del equipo técnico interno especializado (Líder técnico, coordinadores, analistas).</p>
                    </li>
                    <li class="flex items-start">
                        <span class="text-3xl mr-3 accent-blue-light">✅</span>
                        <p class="text-gray-700">Autorizar la liberación de tiempos operativos para roles clave (Dedicación exclusiva/parcial).</p>
                    </li>
                    <li class="flex items-start">
                        <span class="text-3xl mr-3 accent-blue-light">✅</span>
                        <p class="text-gray-700">Validar el presupuesto detallado por hitos (Contratación, capacitación, licencias, herramientas).</p>
                    </li>
                    <li class="flex items-start">
                        <span class="text-3xl mr-3 accent-blue-light">✅</span>
                        <p class="text-gray-700">Establecer un comité de gestión y mecanismo de seguimiento (Representación TI, GRD, Subdirecciones; KPIs semanales).</p>
                    </li>
                </ul>
            </div>
        </section>

        <section id="conclusion" class="py-16 bg-black text-white">
            <div class="max-w-3xl mx-auto text-center">
                <h2 class="text-3xl font-bold mb-6">Conclusión<br>Juntos Hacia el Futuro de la Gestión Hospitalaria</h2>
                <p class="text-xl mb-8">
                    "La Bed Management Platform no es solo una herramienta tecnológica; es una inversión en la calidad de atención, la optimización de nuestros recursos y el fortalecimiento de la posición del Hospital Clínico Félix Bulnes como referente en innovación."
                </p>
                <p class="text-lg mb-4">Gracias por su tiempo.</p>
                <p class="text-lg">Estamos a su disposición para responder cualquier pregunta.</p>
                <img src="https://i.imgur.com/iKfYdtR.png" alt="Logo Hospital" class="w-20 h-20 mx-auto mt-8 rounded-full" onerror="this.src='https://placehold.co/100x100/cccccc/333333?text=Logo+Error'">
                <p class="text-sm mt-4">EU MG© Camilo Acevedo Fernández</p>
                <p class="text-sm">camilo.acevedof@redsalud.gob.cl</p>
            </div>
        </section>

    </div>

    <footer class="text-center py-8 footer">
        <p>&copy; 2024 Hospital Clínico Félix Bulnes. Proyecto Bed Management Platform.</p>
    </footer>

    <script>
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                const targetId = this.getAttribute('href');
                const targetElement = document.querySelector(targetId);
                if (targetElement) {
                    targetElement.scrollIntoView({
                        behavior: 'smooth'
                    });
                }
                if (document.getElementById('mobileMenu').classList.contains('hidden') === false) {
                    document.getElementById('mobileMenu').classList.add('hidden');
                }
            });
        });

        const mobileMenuButton = document.getElementById('mobileMenuButton');
        const mobileMenu = document.getElementById('mobileMenu');
        mobileMenuButton.addEventListener('click', () => {
            mobileMenu.classList.toggle('hidden');
        });
        
        const navbar = document.getElementById('navbar');
        window.onscroll = () => {
            if (window.scrollY > 50) {
                navbar.classList.add('nav-scrolled');
            } else {
                navbar.classList.remove('nav-scrolled');
            }
        };

        const workflowStages = document.querySelectorAll('.workflow-stage');
        workflowStages.forEach(stage => {
            stage.addEventListener('click', () => {
                const stageId = stage.dataset.stage;
                const detailsElement = document.getElementById(`details-${stageId}`);
                
                document.querySelectorAll('.workflow-details.active').forEach(activeDetail => {
                    if (activeDetail !== detailsElement) {
                        activeDetail.classList.remove('active');
                        activeDetail.style.maxHeight = null;
                    }
                });
                 document.querySelectorAll('.workflow-stage.active').forEach(activeStage => {
                    if (activeStage !== stage) {
                        activeStage.classList.remove('active');
                    }
                });

                stage.classList.toggle('active');
                if (detailsElement.classList.contains('active')) {
                    detailsElement.classList.remove('active');
                    detailsElement.style.maxHeight = null;
                } else {
                    detailsElement.classList.add('active');
                    detailsElement.style.maxHeight = detailsElement.scrollHeight + "px";
                }
            });
        });

        // Script para la interactividad del plan de acción
        const timelineStages = document.querySelectorAll('.timeline-stage');
        timelineStages.forEach(stage => {
            stage.addEventListener('click', () => {
                const stageId = stage.dataset.stage;
                const detailsElement = document.getElementById(`details-${stageId}`);
                
                document.querySelectorAll('.timeline-details.active').forEach(activeDetail => {
                    if (activeDetail !== detailsElement) {
                        activeDetail.classList.remove('active');
                        activeDetail.style.maxHeight = null;
                    }
                });
                 document.querySelectorAll('.timeline-stage.active').forEach(activeStage => {
                    if (activeStage !== stage) {
                        activeStage.classList.remove('active');
                    }
                });

                stage.classList.toggle('active');
                if (detailsElement.classList.contains('active')) {
                    detailsElement.classList.remove('active');
                    detailsElement.style.maxHeight = null;
                } else {
                    detailsElement.classList.add('active');
                    detailsElement.style.maxHeight = detailsElement.scrollHeight + "px";
                }
            });
        });


        const kpiCtx = document.getElementById('kpiChart').getContext('2d');
        const kpiChart = new Chart(kpiCtx, {
            type: 'bar',
            data: {
                labels: ['Tiempo Asignación Cama', 'Cumplimiento Plazos Alta', 'Tiempo Traslados Internos'],
                datasets: [{
                    label: 'Mejora Esperada (%)',
                    data: [30, 90, 50],
                    backgroundColor: [
                        'rgba(0, 123, 255, 0.7)', /* Tono azul para las barras */
                        'rgba(0, 123, 255, 0.7)',
                        'rgba(0, 123, 255, 0.7)'
                    ],
                    borderColor: [
                        'rgba(0, 123, 255, 1)', /* Azul más oscuro para los bordes */
                        'rgba(0, 123, 255, 1)',
                        'rgba(0, 123, 255, 1)'
                    ],
                    borderWidth: 1
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        beginAtZero: true,
                        title: {
                            display: true,
                            text: 'Porcentaje de Mejora / Cumplimiento',
                            color: '#000000' /* Color de texto para el título del eje Y (negro) */
                        },
                        ticks: {
                            callback: function(value) {
                                return value + "%"
                            },
                            color: '#000000' /* Color de texto para las etiquetas del eje Y (negro) */
                        },
                        grid: {
                            display: false /* Eliminar las líneas de la cuadrícula del eje Y */
                        }
                    },
                    x: {
                        ticks: {
                            color: '#000000' /* Color de texto para las etiquetas del eje X (negro) */
                        },
                        grid: {
                            display: false /* Eliminar las líneas de la cuadrícula del eje X */
                        }
                    }
                },
                plugins: {
                    legend: {
                        display: false
                    },
                    tooltip: {
                        callbacks: {
                            label: function(context) {
                                let label = context.dataset.label || '';
                                if (label) {
                                    label += ': ';
                                }
                                if (context.parsed.y !== null) {
                                    if (context.label === 'Cumplimiento Plazos Alta') {
                                        label += context.parsed.y + '% (Meta)';
                                    } else {
                                        label += context.parsed.y + '% Reducción';
                                    }
                                }
                                return label;
                            }
                        }
                    }
                }
            }
        });
    </script>
</body>
</html>

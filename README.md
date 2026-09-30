
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rastreo 360° | Galletas Ducales - Cadena de Suministro</title>
    
    <!-- Fuente tipográfica moderna -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    
    <!-- CSS de Leaflet para el Mapa Interactivo -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />

    <style>
        :root {
            --primary: #b80000;
            --primary-dark: #7a0000;
            --gold: #d4af37;
            --gold-light: #fff8e7;
            --dark: #1e1e1e;
            --light-bg: #fdfbf7;
            --white: #ffffff;
            --gray-text: #555555;
            --shadow: 0 10px 25px rgba(0,0,0,0.08);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Poppins', sans-serif;
            background-color: var(--light-bg);
            /* Ilustración de fondo con Trigos y Galletas en patrón estático */
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="180" height="180" viewBox="0 0 180 180"><g opacity="0.12" fill="%23d4af37" stroke="%237a0000" stroke-width="1.5"><rect x="20" y="25" width="34" height="22" rx="3"/><circle cx="28" cy="32" r="1.2" fill="%237a0000"/><circle cx="37" cy="32" r="1.2" fill="%237a0000"/><circle cx="46" cy="32" r="1.2" fill="%237a0000"/><circle cx="28" cy="40" r="1.2" fill="%237a0000"/><circle cx="37" cy="40" r="1.2" fill="%237a0000"/><circle cx="46" cy="40" r="1.2" fill="%237a0000"/></g><g opacity="0.14" stroke="%23d4af37" stroke-width="2.5" fill="none" stroke-linecap="round"><path d="M125 90 L125 155 M112 103 L125 116 L138 103 M108 120 L125 133 L142 120"/></g><g opacity="0.12" fill="%23d4af37" stroke="%237a0000" stroke-width="1.5"><rect x="35" y="110" width="34" height="22" rx="3"/><circle cx="43" cy="117" r="1.2" fill="%237a0000"/><circle cx="52" cy="117" r="1.2" fill="%237a0000"/><circle cx="61" cy="117" r="1.2" fill="%237a0000"/><circle cx="43" cy="125" r="1.2" fill="%237a0000"/><circle cx="52" cy="125" r="1.2" fill="%237a0000"/><circle cx="61" cy="125" r="1.2" fill="%237a0000"/></g><g opacity="0.14" stroke="%23b80000" stroke-width="2.5" fill="none" stroke-linecap="round"><path d="M135 15 L135 75 M122 28 L135 40 L148 28 M118 45 L135 58 L152 45"/></g></svg>');
            background-repeat: repeat;
            background-attachment: fixed; /* Mantiene el fondo estático al desplazar la pantalla */
            background-size: 180px 180px;
            color: var(--dark);
            line-height: 1.7;
        }

        /* HEADER / HERO SECTION CON ILUSTRACIÓN DE FONDO */
        header {
            background: linear-gradient(135deg, rgba(122,0,0,0.94), rgba(184,0,0,0.9)), 
                        url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100"><path d="M30 20 L50 40 L70 20" stroke="%23d4af37" stroke-width="2" fill="none" opacity="0.15"/></svg>');
            color: var(--white);
            padding: 90px 20px;
            text-align: center;
            position: relative;
            box-shadow: 0 4px 20px rgba(0,0,0,0.2);
        }

        .hero-illustration {
            max-width: 120px;
            margin: 0 auto 20px auto;
            display: block;
        }

        .badge {
            background-color: var(--gold);
            color: var(--dark);
            padding: 6px 18px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 1px;
            display: inline-block;
            margin-bottom: 15px;
        }

        header h1 {
            font-size: 2.8rem;
            font-weight: 700;
            margin-bottom: 15px;
            color: #fff3c4;
        }

        header p {
            font-size: 1.15rem;
            max-width: 850px;
            margin: 0 auto 30px auto;
            opacity: 0.95;
        }

        /* NAVEGACIÓN STICKY */
        nav {
            background-color: var(--dark);
            padding: 15px 0;
            position: sticky;
            top: 0;
            z-index: 1000;
            box-shadow: 0 4px 10px rgba(0,0,0,0.15);
        }

        .nav-container {
            max-width: 1200px;
            margin: 0 auto;
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 20px;
        }

        nav a {
            color: var(--white);
            text-decoration: none;
            font-size: 0.95rem;
            font-weight: 600;
            transition: color 0.3s;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        nav a:hover {
            color: var(--gold);
        }

        /* CONTENEDOR PRINCIPAL */
        .container {
            max-width: 1150px;
            margin: 50px auto;
            padding: 0 20px;
        }

        /* TÍTULOS DE SECCIÓN */
        .section-header {
            text-align: center;
            margin-bottom: 35px;
            position: relative;
        }

        .section-header h2 {
            font-size: 2.2rem;
            color: var(--primary);
            display: inline-block;
            position: relative;
            background: rgba(253, 251, 247, 0.85);
            padding: 0 15px;
            border-radius: 8px;
        }

        .section-header h2::after {
            content: '';
            width: 60%;
            height: 4px;
            background-color: var(--gold);
            display: block;
            margin: 8px auto 0 auto;
            border-radius: 2px;
        }

        .section-header p {
            color: var(--gray-text);
            margin-top: 10px;
        }

        /* MAPA INTERACTIVO ESTILO CON ILUSTRACIÓN DE BORDE */
        #map-container {
            background: var(--white);
            padding: 20px;
            border-radius: 20px;
            box-shadow: var(--shadow);
            margin-bottom: 60px;
            border: 2px solid var(--gold);
            position: relative;
        }

        #map {
            height: 480px;
            width: 100%;
            border-radius: 12px;
            z-index: 1;
        }

        /* GRID Y TARJETAS ILUSTRADAS */
        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
            gap: 25px;
            margin-bottom: 60px;
        }

        .card {
            background: var(--white);
            border-radius: 16px;
            padding: 30px;
            box-shadow: var(--shadow);
            border-top: 6px solid var(--primary);
            position: relative;
            overflow: hidden;
            transition: transform 0.3s ease;
        }

        .card:hover {
            transform: translateY(-5px);
        }

        /* Marcador de agua / Ilustración decorativa de fondo en las tarjetas */
        .card::before {
            content: '';
            position: absolute;
            right: -20px;
            bottom: -20px;
            width: 100px;
            height: 100px;
            background-repeat: no-repeat;
            background-size: contain;
            opacity: 0.06;
            pointer-events: none;
        }

        .card-wheat::before {
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%23b80000"><path d="M12 2L10 6H14L12 2ZM12 7L9 11H15L12 7ZM12 12L8 16H16L12 12ZM11 17H13V22H11V17Z"/></svg>');
        }

        .card-factory::before {
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%23b80000"><path d="M2 22V10L7 13V8 L12 11V3L22 8V22H2Z"/></svg>');
        }

        .card-truck::before {
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%23b80000"><path d="M20 8h-3V4H1v13h2a3 3 0 0 0 6 0h6a3 3 0 0 0 6 0h1v-5l-2-4z"/></svg>');
        }

        .card-header-icon {
            width: 45px;
            height: 45px;
            background-color: var(--gold-light);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 15px;
        }

        .card h3 {
            color: var(--primary);
            font-size: 1.3rem;
            margin-bottom: 12px;
        }

        .card-tag {
            background-color: var(--gold-light);
            color: var(--primary-dark);
            font-size: 0.75rem;
            font-weight: 700;
            padding: 4px 12px;
            border-radius: 12px;
            display: inline-block;
            margin-bottom: 15px;
            border: 1px solid var(--gold);
        }

        /* TIMELINE / PASOS DE MANUFACTURA */
        .timeline {
            position: relative;
            margin: 40px 0;
            padding-left: 20px;
        }

        .timeline-step {
            position: relative;
            padding-left: 55px;
            margin-bottom: 35px;
        }

        .timeline-step::before {
            content: attr(data-number);
            position: absolute;
            left: 0;
            top: 0;
            width: 42px;
            height: 42px;
            background-color: var(--primary);
            color: var(--white);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            font-size: 1.1rem;
            box-shadow: 0 4px 10px rgba(184, 0, 0, 0.3);
        }

        .timeline-card {
            background: var(--white);
            padding: 22px;
            border-radius: 12px;
            box-shadow: var(--shadow);
            border-left: 4px solid var(--gold);
        }

        /* TABLAS ESTILIZADAS */
        .table-responsive {
            overflow-x: auto;
            margin-bottom: 60px;
            border-radius: 14px;
            box-shadow: var(--shadow);
            background: var(--white);
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
        }

        th {
            background-color: var(--primary);
            color: var(--white);
            padding: 18px 20px;
        }

        td {
            padding: 16px 20px;
            border-bottom: 1px solid #eeeeee;
        }

        /* CAJA RECETA / ILUSTRACIÓN FINAL */
        .recipe-box {
            background: linear-gradient(135deg, #fff8e7, #ffe899), 
                        url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="60" height="60" viewBox="0 0 24 24" fill="none" stroke="%23d4af37" stroke-width="1"><circle cx="12" cy="12" r="9"/></svg>');
            border: 2px dashed var(--gold);
            border-radius: 20px;
            padding: 35px;
            margin-top: 30px;
            box-shadow: var(--shadow);
        }

        footer {
            background-color: var(--dark);
            color: var(--white);
            text-align: center;
            padding: 40px 20px;
            margin-top: 80px;
            border-top: 5px solid var(--gold);
        }
    </style>
</head>
<body>

    <!-- HERO SECTION ILUSTRADO -->
    <header>
        <!-- SVG Ilustración de Espiga de Trigo / Galleta -->
        <svg class="hero-illustration" viewBox="0 0 100 100" width="80" height="80">
            <circle cx="50" cy="50" r="42" fill="#d4af37" opacity="0.3"/>
            <path d="M50 15 L50 85 M35 30 L50 45 L65 30 M30 50 L50 65 L70 50" stroke="#fff3c4" stroke-width="6" stroke-linecap="round" stroke-linejoin="round" fill="none"/>
        </svg>

        <span class="badge">Estudio de Rastreo Agroindustrial</span>
        <h1>Galletas Ducales: Del Grano a la Mesa</h1>
        <p>Rastreo interactivo de la cadena de suministro, origen de materias primas, logística internacional, manufactura y distribución de la marca de Compañía Galletas Noel (Grupo Nutresa).</p>
    </header>

    <!-- BARRA DE NAVEGACIÓN -->
    <nav>
        <div class="nav-container">
            <a href="#mapa-sec">📍 Mapa Interactivo</a>
            <a href="#origen">🌾 Materias Primas</a>
            <a href="#empaques">📦 Empaques</a>
            <a href="#manufactura">🏭 Planta Noel</a>
            <a href="#logistica">🚚 Red Logística</a>
            <a href="#consumidor">🛒 Consumidor Final</a>
        </div>
    </nav>

    <div class="container">

        <!-- SECCIÓN MAPA INTERACTIVO -->
        <section id="mapa-sec">
            <div class="section-header">
                <h2>📍 Mapa Interactivo de Rastreo Global y Nacional</h2>
                <p>Haz clic en los marcadores del mapa para explorar el origen de los insumos, puertos e instalaciones de producción.</p>
            </div>

            <div id="map-container">
                <div id="map"></div>
            </div>
        </section>

        <!-- MÓDULO 1: ORIGEN DE MATERIAS PRIMAS -->
        <section id="origen">
            <div class="section-header">
                <h2>1. Origen de Materias Primas e Insumos</h2>
                <p>Rastreo detallado de procedencia global y nacional de los ingredientes.</p>
            </div>

            <div class="grid">
                <div class="card card-wheat">
                    <div class="card-header-icon">
                        <svg width="24" height="24" viewBox="0 0 24 24" fill="#b80000"><path d="M12 2L10 6H14L12 2ZM12 7L9 11H15L12 7ZM12 12L8 16H16L12 12ZM11 17H13V22H11V17Z"/></svg>
                    </div>
                    <span class="card-tag">Insumo Importado (Global)</span>
                    <h3>🌾 Harina de Trigo</h3>
                    <p><strong>Origen Internacional:</strong> Colombia importa más del <strong>99%</strong> del trigo que consume. El trigo para Ducales proviene de las praderas de <strong>Canadá</strong> (52%) y <strong>Estados Unidos</strong> (33%).</p>
                    <p><strong>Ingreso al País:</strong> Llega en buques graneleros a los puertos de <strong>Buenaventura</strong> y <strong>Barranquilla</strong>, y se traslada a molinos nacionales para su procesamiento y fortificación.</p>
                </div>

                <div class="card card-wheat">
                    <div class="card-header-icon">
                        <svg width="24" height="24" viewBox="0 0 24 24" fill="#b80000"><path d="M12 2a10 10 0 1 0 10 10A10 10 0 0 0 12 2zm1 15h-2v-2h2zm0-4h-2V7h2z"/></svg>
                    </div>
                    <span class="card-tag">Insumo Nacional</span>
                    <h3>🌴 Grasas Vegetales y Azúcar</h3>
                    <p><strong>Aceite de Palma:</strong> Cosechado en plantaciones del <strong>Meta</strong>, <strong>Cesar y Magdalena</strong>. La manteca se refina para otorgar la textura hojaldrada.</p>
                    <p><strong>Azúcar de Caña:</strong> Proviene del cultivo de caña en los ingenios azucareros del <strong>Valle del Cauca</strong> (Incauca, Manuelita, Mayagüez).</p>
                </div>

                <div class="card card-wheat">
                    <div class="card-header-icon">
                        <svg width="24" height="24" viewBox="0 0 24 24" fill="#b80000"><path d="M12 3L2 12h3v8h14v-8h3L12 3z"/></svg>
                    </div>
                    <span class="card-tag">Insumo Nacional</span>
                    <h3>🧂 Sal, Lácteos y Malta</h3>
                    <p><strong>Sal Refinada:</strong> Extraída en <strong>Manaure (La Guajira)</strong> y las minas de <strong>Zipaquirá</strong>.</p>
                    <p><strong>Lácteos y Malta:</strong> Suero proveniente del <strong>Norte de Antioquia</strong> y extracto de malta para lograr el tono dorado y el toque dulce-salado.</p>
                </div>
            </div>
        </section>

        <!-- MÓDULO 2: EMPAQUES -->
        <section id="empaques">
            <div class="section-header">
                <h2>2. Rastreo de Materiales de Empaque</h2>
                <p>Ingeniería de materiales para preservar el sabor y la textura.</p>
            </div>

            <div class="table-responsive">
                <table>
                    <thead>
                        <tr>
                            <th>Componente de Empaque</th>
                            <th>Material Técnico</th>
                            <th>Origen / Proveedor</th>
                            <th>Función Logística</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td><strong>Empaque Primario</strong></td>
                            <td>Polipropileno Biorientado Metalizado (BOPP)</td>
                            <td>Alico S.A. / Carvajal Empaques (Colombia)</td>
                            <td>Barrera total contra humedad, luz UV y oxígeno.</td>
                        </tr>
                        <tr>
                            <td><strong>Caja Máster</strong></td>
                            <td>Cartón corrugado reciclable</td>
                            <td>Smurfit Kappa (Planta Yumbo, Valle)</td>
                            <td>Protección estructural para transporte masivo y apilamiento.</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </section>

        <!-- MÓDULO 3: MANUFACTURA Y PLANTA -->
        <section id="manufactura">
            <div class="section-header">
                <h2>3. Transformación e Industria (Planta Noel)</h2>
                <p>Fabricación en la Planta Noel de <strong>Medellín, Antioquia</strong>.</p>
            </div>

            <div class="timeline">
                <div class="timeline-step" data-number="1">
                    <h4>Almacenamiento y Dosificación</h4>
                    <div class="timeline-card">La harina se descarga neumáticamente en silos verticales. Computadoras dosifican con precisión harina, grasa, azúcares y agua.</div>
                </div>

                <div class="timeline-step" data-number="2">
                    <h4>Amasado y "Toque Secreto"</h4>
                    <div class="timeline-card">Se mezcla en amasadoras industriales donde se incorpora la fórmula patentada que equilibra lo dulce y lo salado.</div>
                </div>

                <div class="timeline-step" data-number="3">
                    <h4>Laminado, Troquelado y Perforado</h4>
                    <div class="timeline-card">Rodillos reducen la masa a láminas delgadas, un troquel le da la forma rectangular ondulada y perfora los orificios para liberar vapor.</div>
                </div>

                <div class="timeline-step" data-number="4">
                    <h4>Horneado Continuo en Horno Túnel</h4>
                    <div class="timeline-card">Las galletas recorren un horno túnel de más de 100 metros a temperaturas entre 180°C y 250°C.</div>
                </div>

                <div class="timeline-step" data-number="5">
                    <h4>Enfriamiento y Empaque Robótico</h4>
                    <div class="timeline-card">Se enfrían en espirales de aire, se cuentan automáticamente y se sellan herméticamente en su envoltura.</div>
                </div>
            </div>
        </section>

        <!-- MÓDULO 4: RED LOGÍSTICA -->
        <section id="logistica">
            <div class="section-header">
                <h2>4. Transporte, Logística y Distribución</h2>
                <p>Red operada por <strong>Comercial Nutresa</strong>.</p>
            </div>

            <div class="grid">
                <div class="card card-truck">
                    <h3>🚛 Transporte Primario</h3>
                    <p>Tractomulas llevan el producto desde Medellín hacia Centros de Distribución (CEDI) en Bogotá, Cali, Barranquilla y Bucaramanga.</p>
                </div>
                <div class="card card-truck">
                    <h3>🏪 Última Milla (Canales)</h3>
                    <p>Distribución diaria hacia más de 300.000 tiendas de barrio (TAT) y grandes cadenas de supermercados en todo el país.</p>
                </div>
                <div class="card card-truck">
                    <h3>🚢 Exportación</h3>
                    <p>Contenedores marítimos despachados desde Cartagena y Buenaventura a más de 20 países (EE.UU., Ecuador, España, etc.).</p>
                </div>
            </div>
        </section>

        <!-- MÓDULO 5: CONSUMIDOR -->
        <section id="consumidor">
            <div class="section-header">
                <h2>5. Consumidor Final y Uso Cultural</h2>
                <p>El producto en los hogares colombianos.</p>
            </div>

            <div class="card card-factory">
                <h3>🇨🇴 Impacto Cultural</h3>
                <p>Presente en hogares de todos los estratos socioeconómicos. Se consume como pasabocas diario con café o chocolate.</p>
            </div>

            <div class="recipe-box">
                <h3>🍰 Receta Tradicional: Postre de Limón con Ducales</h3>
                <p>Combinación cultural colombiana usando tacos de Galletas Ducales como base crocante, leche condensada, crema de leche y jugo de limón fresco.</p>
            </div>
        </section>

    </div>

    <footer>
        <p><strong>Proyecto Escolar de Rastreo Agroindustrial</strong></p>
        <p>Galletas Ducales | Compañía de Galletas Noel - Grupo Nutresa</p>
    </footer>

    <!-- LIBRERÍA DE JS PARA EL MAPA INTERACTIVO (LEAFLET.JS) -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        // Inicializar el mapa centrado entre América del Norte y Colombia
        var map = L.map('map').setView([15.0, -70.0], 3);

        // Capa de mapas OpenStreetMap
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            maxZoom: 18,
            attribution: '© OpenStreetMap'
        }).addTo(map);

        // Puntos de la Cadena de Suministro con información de Rastreo
        var locations = [
            {
                coords: [56.1304, -106.3468],
                title: "🌾 Canadá (Origen del Trigo)",
                desc: "Exporta el 52% del trigo consumido para la elaboración de la harina."
            },
            {
                coords: [37.0902, -95.7129],
                title: "🌾 EE.UU. (Origen del Trigo)",
                desc: "Exporta el 33% del trigo para la mezcla de harina galletera."
            },
            {
                coords: [3.8801, -77.0312],
                title: "🚢 Puerto de Buenaventura",
                desc: "Puerto de entrada del trigo importado en buques graneleros."
            },
            {
                coords: [10.9685, -74.7813],
                title: "🚢 Puerto de Barranquilla",
                desc: "Ingreso marítimo del trigo y puerto de salida para exportaciones."
            },
            {
                coords: [6.2442, -75.5812],
                title: "🏭 Planta Noel (Medellín, Antioquia)",
                desc: "Fábrica principal donde se hornean y empaquetan las Galletas Ducales."
            },
            {
                coords: [3.8903, -76.3040],
                title: "🌱 Valle del Cauca (Ingenios Azucareros)",
                desc: "Producción y suministro del azúcar de caña para la mezcla dulce-salada."
            },
            {
                coords: [11.7753, -72.4442],
                title: "🧂 Salinas de Manaure (La Guajira)",
                desc: "Origen de la sal marina refinada añadida a la galleta."
            },
            {
                coords: [4.8156, -74.0322],
                title: "🚚 CEDI Siberia (Bogotá)",
                desc: "Centro de Distribución principal para abastecer el centro del país."
            }
        ];

        // Agregar los marcadores al mapa
        locations.forEach(function(loc) {
            var marker = L.marker(loc.coords).addTo(map);
            marker.bindPopup("<b>" + loc.title + "</b><br>" + loc.desc);
        });
    </script>
</body>
</html>

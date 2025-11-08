<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Explorador Interactivo del PIB</title>
    <!-- Carga de Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Configuración de Tailwind para usar la fuente Inter -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        'primary-blue': '#1D4ED8',
                        'secondary-light': '#F3F4F6',
                        'accent-orange': '#F59E0B',
                    }
                }
            }
        }
    </script>
    <style>
        /* Estilos generales para asegurar la fuente Inter */
        body { font-family: 'Inter', sans-serif; background-color: #E5E7EB; }
        /* Estilo para los botones de pestañas */
        .tab-button.active {
            border-bottom: 3px solid #F59E0B;
            color: #1D4ED8;
            font-weight: 600;
        }
        /* Estilo para el contenedor principal de la visualización */
        .card-visual {
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease;
        }
    </style>
</head>
<body class="p-4 sm:p-8 min-h-screen">

    <div id="app" class="max-w-6xl mx-auto bg-white rounded-xl shadow-2xl overflow-hidden">
        
        <!-- Encabezado Principal -->
        <header class="p-6 sm:p-10 bg-primary-blue text-white">
            <h1 class="text-3xl sm:text-4xl font-extrabold tracking-tight">Explorador del PIB: Conceptos y Datos de Guatemala</h1>
            <p class="mt-2 text-primary-light italic">Página interactiva basada en el informe técnico de Cuentas Nacionales.</p>
        </header>

        <!-- Navegación por Pestañas -->
        <div class="border-b border-gray-200 bg-secondary-light">
            <nav class="-mb-px flex space-x-4 sm:space-x-8 px-6 sm:px-10" id="tab-nav">
                <button class="tab-button py-4 px-1 text-sm sm:text-base font-medium text-gray-500 hover:text-primary-blue transition duration-150 active" data-tab="va-explorer">Valor Agregado (VA)</button>
                <button class="tab-button py-4 px-1 text-sm sm:text-base font-medium text-gray-500 hover:text-primary-blue transition duration-150" data-tab="pib-pnb">PIB vs PNB</button>
                <button class="tab-button py-4 px-1 text-sm sm:text-base font-medium text-gray-500 hover:text-primary-blue transition duration-150" data-tab="real-vs-nominal">Real vs. Nominal</button>
                <button class="tab-button py-4 px-1 text-sm sm:text-base font-medium text-gray-500 hover:text-primary-blue transition duration-150" data-tab="data-guatemala">Estructura del Gasto</button>
            </nav>
        </div>

        <!-- Contenido de las Pestañas -->
        <main class="p-6 sm:p-10">
            <!-- 1. Valor Agregado Explorer (Default) -->
            <section id="va-explorer" class="tab-content">
                <h2 class="text-2xl font-bold text-gray-800 mb-6">1. Valor Agregado (VA): Eliminando la Doble Contabilización</h2>
                
                <div class="bg-blue-50 p-6 rounded-lg mb-8 border-l-4 border-primary-blue">
                    <p class="font-semibold text-primary-blue">Concepto Clave:</p>
                    <p class="mt-1 text-gray-700">El PIB solo cuenta el **valor nuevo creado** por el trabajo humano en cada etapa, eliminando el valor de los insumos (compras intermedias) para evitar contar la misma producción varias veces.</p>
                </div>

                <!-- Simulación del Ejemplo del Pan -->
                <div class="flex flex-col space-y-4 md:space-y-0 md:flex-row md:justify-between md:items-center text-center mb-8">
                    <!-- Finca -->
                    <div id="finca" class="card-visual bg-white p-4 rounded-lg flex-1 mx-2 border-2 border-green-500">
                        <p class="font-semibold text-lg text-green-700">Fase 1: Finca de Trigo</p>
                        <p class="mt-1 text-sm text-gray-600">Ventas: Q3,000.00 | Compras: Q0.00</p>
                        <p class="mt-2 text-xl font-bold text-green-800 va-value">VA: Q3,000.00</p>
                    </div>
                    <i class="fas fa-arrow-right text-3xl text-gray-400 hidden md:block"></i>
                    <!-- Molino -->
                    <div id="molino" class="card-visual bg-white p-4 rounded-lg flex-1 mx-2 border-2 border-yellow-500">
                        <p class="font-semibold text-lg text-yellow-700">Fase 2: Molino de Trigo</p>
                        <p class="mt-1 text-sm text-gray-600">Ventas: Q3,400.00 | Compras: Q3,000.00</p>
                        <p class="mt-2 text-xl font-bold text-yellow-800 va-value">VA: Q400.00</p>
                    </div>
                    <i class="fas fa-arrow-right text-3xl text-gray-400 hidden md:block"></i>
                    <!-- Panadería -->
                    <div id="panaderia" class="card-visual bg-white p-4 rounded-lg flex-1 mx-2 border-2 border-red-500">
                        <p class="font-semibold text-lg text-red-700">Fase 3: Panadería</p>
                        <p class="mt-1 text-sm text-gray-600">Ventas: Q4,000.00 | Compras: Q3,400.00</p>
                        <p class="mt-2 text-xl font-bold text-red-800 va-value">VA: Q600.00</p>
                    </div>
                </div>

                <!-- Resumen y Alternar Cálculo -->
                <div class="text-center mt-10">
                    <div id="result-box" class="p-6 rounded-xl inline-block shadow-lg border-t-4 border-accent-orange">
                        <p class="text-xl font-semibold mb-2" id="calculation-mode-label">Modo: Cálculo de Valor Bruto (VBP)</p>
                        <p class="text-4xl font-extrabold text-red-600" id="pib-result">Q10,400.00 (¡Incorrecto por doble contabilización!)</p>
                    </div>
                    
                    <button id="toggle-pib-mode" class="mt-6 bg-accent-orange hover:bg-yellow-600 text-white font-bold py-3 px-8 rounded-full transition duration-300 shadow-md hover:shadow-xl">
                        Ver el Cálculo Correcto (PIB por Valor Agregado)
                    </button>
                </div>
            </section>

            <!-- 2. PIB vs PNB -->
            <section id="pib-pnb" class="tab-content hidden">
                <h2 class="text-2xl font-bold text-gray-800 mb-6">2. PIB (Interno) vs PNB (Nacional)</h2>
                
                <div class="grid md:grid-cols-2 gap-8">
                    <!-- PIB Card -->
                    <div class="bg-gray-50 p-6 rounded-xl border-l-4 border-primary-blue shadow-md">
                        <h3 class="text-xl font-semibold text-primary-blue flex items-center mb-3">
                            <i class="fas fa-globe-americas mr-3"></i> Producto Interno Bruto (PIB)
                        </h3>
                        <p class="text-lg text-gray-800 font-bold mb-2">Perspectiva: Geográfica (Dentro de las fronteras)</p>
                        <p class="text-gray-600">Mide la producción generada **dentro de las fronteras** del país, sin importar la nacionalidad de la empresa. Incluye las ganancias de empresas extranjeras que operan en el país.</p>
                        <p class="mt-3 text-sm text-red-500 italic">No incluye la producción de empresas nacionales en el extranjero.</p>
                    </div>
                    
                    <!-- PNB Card -->
                    <div class="bg-gray-50 p-6 rounded-xl border-l-4 border-accent-orange shadow-md">
                        <h3 class="text-xl font-semibold text-accent-orange flex items-center mb-3">
                            <i class="fas fa-flag mr-3"></i> Producto Nacional Bruto (PNB)
                        </h3>
                        <p class="text-lg text-gray-800 font-bold mb-2">Perspectiva: Nacionalidad (Factores de Producción)</p>
                        <p class="text-gray-600">Mide la producción generada por los **nacionales** del país, sin importar dónde estén ubicados geográficamente.</p>
                        <p class="mt-3 text-sm text-red-500 italic">En países menos desarrollados, PNB suele ser **menor** que el PIB debido a la remisión de utilidades al extranjero.</p>
                    </div>
                </div>

                <div class="mt-10 p-6 bg-yellow-50 rounded-lg shadow-inner">
                    <p class="text-lg font-bold text-gray-800">Fórmula de Relación:</p>
                    <p class="mt-2 text-xl font-mono bg-yellow-100 p-2 rounded inline-block text-red-600">
                        PIB + Ingresos Netos del Exterior por Factores de Producción = PNB
                    </p>
                </div>
            </section>

            <!-- 3. Real vs Nominal -->
            <section id="real-vs-nominal" class="tab-content hidden">
                <h2 class="text-2xl font-bold text-gray-800 mb-6">3. PIB Real vs. PIB Nominal: El Impacto de la Inflación</h2>
                
                <div class="grid md:grid-cols-2 gap-8">
                    <!-- Nominal Card -->
                    <div class="bg-gray-50 p-6 rounded-xl border-l-4 border-red-500 shadow-md">
                        <h3 class="text-xl font-semibold text-red-600 flex items-center mb-3">
                            <i class="fas fa-money-bill-wave mr-3"></i> PIB Nominal (Precios Corrientes)
                        </h3>
                        <p class="text-gray-600">Utiliza los **precios vigentes** de cada año para valorar la producción. </p>
                        <p class="mt-3 text-lg font-bold text-red-700">Riesgo:</p>
                        <p class="text-gray-600">Un aumento puede deberse solo al aumento de precios (**inflación**) y no a un crecimiento real de la producción.</p>
                    </div>
                    
                    <!-- Real Card -->
                    <div class="bg-gray-50 p-6 rounded-xl border-l-4 border-green-500 shadow-md">
                        <h3 class="text-xl font-semibold text-green-700 flex items-center mb-3">
                            <i class="fas fa-chart-line mr-3"></i> PIB Real (Precios Constantes)
                        </h3>
                        <p class="text-gray-600">Utiliza los **precios de un año base** (ej. 2013 en Guatemala) para valorar la producción.</p>
                        <p class="mt-3 text-lg font-bold text-green-700">Objetivo:</p>
                        <p class="text-gray-600">**Eliminar el efecto de los precios** para reflejar el comportamiento **real** del volumen de bienes y servicios producidos.</p>
                    </div>
                </div>

                <div class="mt-10 p-6 bg-blue-100 rounded-lg shadow-inner text-center">
                    <p class="text-lg font-bold text-gray-800">Conclusión del Informe (2022):</p>
                    <p class="mt-2 text-2xl font-extrabold text-primary-blue">
                        PIB Nominal fue 29.9% más alto que el PIB Real,
                    </p>
                    <p class="text-lg text-gray-600">Demostrando la fuerte distorsión del aumento de precios desde el año base 2013.</p>
                </div>
            </section>

            <!-- 4. Estructura del Gasto (Data Guatemala) -->
            <section id="data-guatemala" class="tab-content hidden">
                <h2 class="text-2xl font-bold text-gray-800 mb-6">4. Estructura del Gasto y Producción (Guatemala 2023)</h2>
                
                <div class="grid md:grid-cols-2 gap-8">
                    <!-- Gasto (Demanda) -->
                    <div>
                        <h3 class="text-xl font-semibold text-primary-blue mb-4 border-b pb-2">Enfoque del Gasto (Demanda)</h3>
                        <p class="text-gray-700 mb-4">El motor principal de la economía es el consumo, haciendo que la demanda interna supere la capacidad productiva del país.</p>
                        <div class="space-y-3">
                            <div class="p-3 bg-blue-50 rounded-lg">
                                <p class="font-bold">Gasto de consumo final</p>
                                <p class="text-sm text-gray-600">Q593,162.3M (Representa el 100.6% del PIB total)</p>
                            </div>
                            <div class="p-3 bg-blue-50 rounded-lg">
                                <p class="font-bold">Formación Bruta de Capital Fijo (Inversión)</p>
                                <p class="text-sm text-gray-600">Q96,141.0M</p>
                            </div>
                            <div class="p-3 bg-red-50 rounded-lg">
                                <p class="font-bold">Exportaciones Netas (X - M)</p>
                                <p class="text-sm text-gray-600">Déficit comercial significativo: El valor de las importaciones supera a las exportaciones.</p>
                            </div>
                        </div>
                    </div>

                    <!-- Producción (Oferta) -->
                    <div>
                        <h3 class="text-xl font-semibold text-accent-orange mb-4 border-b pb-2">Top 3 Enfoque de la Producción (Oferta)</h3>
                        <p class="text-gray-700 mb-4">Estos tres sectores representan el 41.5% del PIB total de Guatemala en 2023.</p>
                        
                        <div id="chart-container" class="space-y-4">
                            <!-- Chart components rendered by JS -->
                        </div>
                    </div>
                </div>

            </section>
        </main>

    </div>

    <script>
        const app = document.getElementById('app');
        const tabNav = document.getElementById('tab-nav');
        const tabContents = document.querySelectorAll('.tab-content');
        const toggleButton = document.getElementById('toggle-pib-mode');
        const pibResult = document.getElementById('pib-result');
        const calcModeLabel = document.getElementById('calculation-mode-label');

        let isVBPMode = true;

        // --- Manejo de Pestañas ---
        function switchTab(targetId) {
            // Desactivar todas las pestañas y ocultar contenido
            document.querySelectorAll('.tab-button').forEach(btn => btn.classList.remove('active'));
            tabContents.forEach(content => content.classList.add('hidden'));

            // Activar la pestaña y mostrar el contenido
            document.querySelector(`[data-tab="${targetId}"]`).classList.add('active');
            document.getElementById(targetId).classList.remove('hidden');

            // Reiniciar el explorador VA cada vez que se cambia de pestaña
            if (targetId !== 'va-explorer') {
                resetVAExplorer();
            }
        }

        tabNav.addEventListener('click', (e) => {
            if (e.target.classList.contains('tab-button')) {
                const targetId = e.target.dataset.tab;
                switchTab(targetId);
            }
        });

        // --- Explorador de Valor Agregado (VA) ---
        
        const VBP_TOTAL = 10400.00; // 3000 + 3400 + 400
        const PIB_TOTAL = 4000.00; // 3000 + 400 + 600

        function updateVAExplorer() {
            if (isVBPMode) {
                // Modo VBP (Incorrecto)
                calcModeLabel.textContent = 'Modo: Cálculo de Valor Bruto (VBP)';
                pibResult.textContent = `Q${VBP_TOTAL.toLocaleString('es-GT', { minimumFractionDigits: 2 })} (¡Incorrecto por doble contabilización!)`;
                pibResult.classList.remove('text-green-600');
                pibResult.classList.add('text-red-600');
                toggleButton.textContent = 'Ver el Cálculo Correcto (PIB por Valor Agregado)';
            } else {
                // Modo Valor Agregado (Correcto)
                calcModeLabel.textContent = 'Modo: Producto Interno Bruto (PIB) por Valor Agregado';
                pibResult.textContent = `Q${PIB_TOTAL.toLocaleString('es-GT', { minimumFractionDigits: 2 })} (¡Correcto! Valor del producto final)`;
                pibResult.classList.remove('text-red-600');
                pibResult.classList.add('text-green-600');
                toggleButton.textContent = 'Ver el Cálculo Bruto (VBP - Incorrecto)';
            }
        }

        function resetVAExplorer() {
             isVBPMode = true;
             updateVAExplorer();
        }

        toggleButton.addEventListener('click', () => {
            isVBPMode = !isVBPMode; // Alternar el modo
            updateVAExplorer();
        });

        // --- Visualización de Datos de Producción (Guatemala) ---
        const productionData = [
            { activity: 'Comercio y reparación de vehículos', value: 107885.6, color: 'bg-primary-blue' },
            { activity: 'Industrias manufactureras', value: 82734.7, color: 'bg-indigo-500' },
            { activity: 'Agricultura, ganadería, silvicultura y pesca', value: 54295.5, color: 'bg-green-500' },
            { activity: 'Actividades inmobiliarias', value: 54032.6, color: 'bg-purple-500' },
            { activity: 'Construcción', value: 27810.4, color: 'bg-red-500' },
        ];

        function renderChart() {
            const chartContainer = document.getElementById('chart-container');
            const total = productionData[0].value; // Usar el más alto como 100% para la escala

            const chartHTML = productionData.map(item => {
                const width = (item.value / total) * 100;
                return `
                    <div class="flex items-center space-x-4">
                        <div class="flex-shrink-0 w-2/5 text-right font-medium text-sm text-gray-700">${item.activity}</div>
                        <div class="flex-1 h-8 ${item.color} rounded-full" style="width: ${width.toFixed(1)}%;">
                            <div class="h-full flex items-center justify-end pr-2 text-white text-xs font-semibold">
                                Q${(item.value / 1000).toFixed(1)} Mil Millones
                            </div>
                        </div>
                    </div>
                `;
            }).join('');

            chartContainer.innerHTML = chartHTML;
        }

        // --- Inicialización ---
        window.onload = () => {
            updateVAExplorer(); // Inicializar en modo VBP
            renderChart();     // Renderizar el gráfico de barras
            switchTab('va-explorer'); // Asegurar que la primera pestaña esté activa
        };
    </script>
    <script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>
</body>
</html>

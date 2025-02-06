# permfumeri-y-mas
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Gestión de Pagos y Gastos</title>
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet" />
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #f0f0f0;
      margin: 0;
      padding: 0;
    }
    header {
      background-color: #ff6347; /* Color llamativo */
      color: white;
      padding: 20px;
      text-align: center;
      font-size: 2em;
      animation: bounce 1s infinite alternate;
      border-bottom: 5px solid #e55347;
    }
    @keyframes bounce {
      0% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
      100% { transform: translateY(0); }
    }
    .container {
      padding: 20px;
    }
    .tabs {
      position: fixed;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      gap: 20px;
    }
    .tab-button {
      background-color: #ff6347;
      color: white;
      border-radius: 50%;
      width: 60px;
      height: 60px;
      font-size: 24px;
      display: flex;
      justify-content: center;
      align-items: center;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      cursor: pointer;
      transition: all 0.3s ease;
    }
    .tab-button:hover {
      background-color: #e55347;
      transform: translateY(-5px);
    }
    .content {
      margin-bottom: 100px; /* Espacio para los botones flotantes */
    }
    .table-container {
      margin-top: 20px;
      border-collapse: collapse;
      width: 100%;
    }
    .table-container th, .table-container td {
      border: 1px solid #ddd;
      padding: 12px;
      text-align: left;
    }
    .table-container th {
      background-color: #ff6347;
      color: white;
    }
    .button {
      background-color: #ff6347; /* Rojo vibrante */
      color: white;
      padding: 10px 20px;
      border: none;
      cursor: pointer;
      margin-top: 10px;
      border-radius: 5px;
      transition: background-color 0.3s ease;
    }
    .button:hover {
      background-color: #e55347;
    }
    .whatsapp-btn {
      background-color: #25d366;
      color: white;
      border: none;
      padding: 10px 20px;
      cursor: pointer;
      border-radius: 5px;
      transition: background-color 0.3s ease;
      display: inline-flex;
      align-items: center;
    }
    .whatsapp-btn:hover {
      background-color: #128c7e;
    }
    .whatsapp-btn img {
      width: 20px;
      height: 20px;
      margin-right: 10px;
    }
    .add-client-btn {
      background-color: #28a745;
      color: white;
      border-radius: 50%;
      width: 60px;
      height: 60px;
      font-size: 30px;
      text-align: center;
      line-height: 60px;
      position: fixed;
      bottom: 100px;
      right: 20px;
      cursor: pointer;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      transition: transform 0.3s ease;
    }
    .add-client-btn:hover {
      transform: translateY(-5px);
    }
    #map {
      height: 400px;
      width: 100%;
      margin-top: 20px;
    }
    .tabs .tab-button .material-icons {
      font-size: 28px;
    }
    .content h2 {
      text-align: center;
      font-size: 1.5em;
    }
    .login-container {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #f0f0f0;
    }
    .login-box {
      padding: 20px;
      background-color: white;
      border-radius: 5px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    .login-box input {
      margin: 10px 0;
      padding: 10px;
      width: 100%;
      border: 1px solid #ddd;
      border-radius: 5px;
    }
    .login-box button {
      background-color: #28a745;
      color: white;
      border: none;
      padding: 10px 20px;
      border-radius: 5px;
      width: 100%;
      cursor: pointer;
    }
  </style>
</head>
<body>

<!-- Login -->
<div id="login" class="login-container">
  <div class="login-box">
    <h2>Iniciar Sesión</h2>
    <input type="text" id="username" placeholder="Usuario" />
    <input type="password" id="password" placeholder="Contraseña" />
    <button onclick="login()">Ingresar</button>
    <p id="login-error" style="color: red; display: none;">Usuario o contraseña incorrectos.</p>
  </div>
</div>

<header style="display: none;" id="header">
  Gestión de Pagos
</header>

<div class="container" id="container" style="display: none;">
  <!-- Contenido -->
  <div id="clientes" class="content">
    <h2>Lista de Clientes</h2>
    <table class="table-container">
      <thead>
        <tr>
          <th>Fecha del Préstamo</th>
          <th>Nombre</th>
          <th>Dirección</th>
          <th>Valor de Cuota</th>
          <th>Cuotas Pendientes</th>
          <th>Registrar Pago</th>
          <th>Estado de Cuenta</th>
          <th>Monto del Préstamo</th>
          <th>Llamar</th>
          <th>WhatsApp</th>
          <th>Modificar</th>
        </tr>
      </thead>
      <tbody id="clientes-table">
        <!-- Los clientes y pagos se mostrarán aquí -->
      </tbody>
    </table>
    <div class="add-client-btn" onclick="agregarCliente()">+</div>
  </div>

  <div id="balance" class="content" style="display:none;">
    <h2>Balance Diario</h2>
    <canvas id="balanceChart" width="400" height="200"></canvas>
    <h2>Balance Semanal</h2>
    <canvas id="balanceChartSemana" width="400" height="200"></canvas>
    <h2>Balance Mensual</h2>
    <canvas id="balanceChartMes" width="400" height="200"></canvas>
    <div>
      <label for="gastos">Gastos del día:</label>
      <input type="number" id="gastos" />
    </div>
    <div>
      <label for="cobrado">Monto cobrado hoy:</label>
      <input type="number" id="cobrado" />
    </div>
    <div>
      <button onclick="calcularBalanceCaja()">Ver Balance en Caja</button>
    </div>
  </div>

  <div id="lista-negra" class="content" style="display:none;">
    <h2>Lista Negra - Clientes con Mora</h2>
    <table class="table-container">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Cuotas Pendientes</th>
          <th>Estado</th>
        </tr>
      </thead>
      <tbody id="lista-negra-table">
        <!-- Los clientes con mora se mostrarán aquí -->
      </tbody>
    </table>
  </div>
</div>

<!-- Pestañas flotantes -->
<div class="tabs" id="tabs" style="display: none;">
  <div class="tab-button" onclick="showSection('clientes')">
    <span class="material-icons">group</span>
  </div>
  <div class="tab-button" onclick="showSection('balance')">
    <span class="material-icons">show_chart</span>
  </div>
  <div class="tab-button" onclick="showSection('lista-negra')">
    <span class="material-icons">warning</span>
  </div>
</div>

<script>
// Datos actualizados de clientes con los nuevos valores de cuota y monto del préstamo
const clientes = [
  {
    id: 1,
    nombre: 'clara dos',
    direccion: 'Dirección clara dos',
    fechaPrestamo: '2025-01-12',
    cuota: 25,
    cuotasPendientes: 7,
    montoPrestamo: 500000,
    telefono: '600000001'
  },
  {
    id: 2,
    nombre: 'clara amiga',
    direccion: 'Dirección clara amiga',
    fechaPrestamo: '2025-01-17',
    cuota: 25,
    cuotasPendientes: 13,
    montoPrestamo: 50000,
    telefono: '600000002'
  },
  {
    id: 3,
    nombre: 'juan alonso',
    direccion: 'Dirección juan alonso',
    fechaPrestamo: '2025-01-18',
    cuota: 250,
    cuotasPendientes: 16,
    montoPrestamo: 5000000,
    telefono: '600000003'
  },
  {
    id: 4,
    nombre: 'buseta',
    direccion: 'Dirección buseta',
    fechaPrestamo: '2025-01-18',
    cuota: 25,
    cuotasPendientes: 17,
    montoPrestamo: 500000,
    telefono: '600000004'
  },
  {
    id: 5,
    nombre: 'heladero',
    direccion: 'Dirección heladero',
    fechaPrestamo: '2025-01-18',
    cuota: 25,
    cuotasPendientes: 18,
    montoPrestamo: 500000,
    telefono: '600000005'
  },
  {
    id: 6,
    nombre: 'navaja',
    direccion: 'Dirección navaja',
    fechaPrestamo: '2025-01-20',
    cuota: 25,
    cuotasPendientes: 20,
    montoPrestamo: 500000,
    telefono: '600000006'
  },
  {
    id: 7,
    nombre: 'clara luz',
    direccion: 'Dirección clara luz',
    fechaPrestamo: '2025-01-20',
    cuota: 100,
    cuotasPendientes: 15,
    montoPrestamo: 2000000,
    telefono: '600000007'
  },
  {
    id: 8,
    nombre: 'jeison',
    direccion: 'Dirección jeison',
    fechaPrestamo: '2025-01-21',
    cuota: 50,
    cuotasPendientes: 16,
    montoPrestamo: 1000000,
    telefono: '600000008'
  },
  {
    id: 9,
    nombre: 'chancero',
    direccion: 'Dirección chancero',
    fechaPrestamo: '2025-01-21',
    cuota: 25,
    cuotasPendientes: 19,
    montoPrestamo: 500000,
    telefono: '600000009'
  },
  {
    id: 10,
    nombre: 'carnicero',
    direccion: 'Dirección carnicero',
    fechaPrestamo: '2025-01-22',
    cuota: 25,
    cuotasPendientes: 19,
    montoPrestamo: 500000,
    telefono: '600000010'
  },
  {
    id: 11,
    nombre: 'dario',
    direccion: 'Dirección dario',
    fechaPrestamo: '2025-01-22',
    cuota: 50,
    cuotasPendientes: 19,
    montoPrestamo: 1000000,
    telefono: '600000011'
  },
  {
    id: 12,
    nombre: 'pieza heladero',
    direccion: 'Dirección pieza heladero',
    fechaPrestamo: '2025-01-23',
    cuota: 25,
    cuotasPendientes: 22,
    montoPrestamo: 500000,
    telefono: '600000012'
  },
  {
    id: 13,
    nombre: 'rolo',
    direccion: 'Dirección rolo',
    fechaPrestamo: '2025-01-24',
    cuota: 25,
    cuotasPendientes: 21,
    montoPrestamo: 500000,
    telefono: '600000013'
  },
  {
    id: 14,
    nombre: 'vecino',
    direccion: 'Dirección vecino',
    fechaPrestamo: '2025-01-25',
    cuota: 25,
    cuotasPendientes: 22,
    montoPrestamo: 500000,
    telefono: '600000014'
  },
  {
    id: 15,
    nombre: 'aurelia',
    direccion: 'Dirección aurelia',
    fechaPrestamo: '2025-01-25',
    cuota: 25,
    cuotasPendientes: 21,
    montoPrestamo: 500000,
    telefono: '600000015'
  },
  {
    id: 16,
    nombre: 'cesar',
    direccion: 'Dirección cesar',
    fechaPrestamo: '2025-01-28',
    cuota: 50,
    cuotasPendientes: 23,
    montoPrestamo: 1000000,
    telefono: '600000016'
  },
  {
    id: 17,
    nombre: 'chipiro',
    direccion: 'Dirección chipiro',
    fechaPrestamo: '2025-01-29',
    cuota: 50,
    cuotasPendientes: 0,
    montoPrestamo: 1000000,
    telefono: '600000017'
  }
];

// Datos de usuario
const adminUser = { username: 'admin', password: 'admin123' };
const limitedUser = { username: 'usuario', password: 'user123' };

// Variable para verificar el tipo de usuario
let currentUser = null;

function login() {
  const username = document.getElementById('username').value;
  const password = document.getElementById('password').value;
  
  if (username === adminUser.username && password === adminUser.password) {
    currentUser = 'admin';
    showApp();
  } else if (username === limitedUser.username && password === limitedUser.password) {
    currentUser = 'limited';
    showApp();
  } else {
    document.getElementById('login-error').style.display = 'block';
  }
}

// Función para mostrar la app después de iniciar sesión
function showApp() {
  document.getElementById('login').style.display = 'none';
  document.getElementById('header').style.display = 'block';
  document.getElementById('container').style.display = 'block';
  document.getElementById('tabs').style.display = 'block';

  if (currentUser === 'limited') {
    // Ocultar secciones que el usuario limitado no puede ver
    document.getElementById('balance').style.display = 'none';
    document.getElementById('lista-negra').style.display = 'none';
  }

  mostrarClientes();
}

// Mostrar los clientes en la tabla
function mostrarClientes() {
  const clientesTable = document.getElementById('clientes-table');
  clientesTable.innerHTML = '';

  clientes.forEach(cliente => {
    const row = document.createElement('tr');
    row.innerHTML = `
      <td>${cliente.fechaPrestamo}</td>
      <td>${cliente.nombre}</td>
      <td>${cliente.direccion}</td>
      <td>${cliente.cuota}</td>
      <td>${cliente.cuotasPendientes}</td>
      <td><button class="button" onclick="registrarPago(${cliente.id})">Registrar Pago</button></td>
      <td><span>${cliente.cuotasPendientes > 0 ? 'Pendiente' : 'Pagado'}</span></td>
      <td>${cliente.montoPrestamo}</td>
      <td><button class="button" onclick="llamarCliente('${cliente.telefono}')">Llamar</button></td>
      <td>
        <button class="whatsapp-btn" onclick="enviarWhatsApp('${cliente.telefono}')">
          <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/6b/WhatsApp.svg/120px-WhatsApp.svg.png" alt="WhatsApp">WhatsApp
        </button>
      </td>
      ${currentUser === 'admin' ? `<td><button class="button" onclick="editarCliente(${cliente.id})">Modificar</button></td>` : ''}
    `;
    clientesTable.appendChild(row);
  });
}

// Función para cambiar entre secciones
function showSection(sectionId) {
  const sections = document.querySelectorAll('.content');
  sections.forEach(section => {
    section.style.display = 'none';
  });
  document.getElementById(sectionId).style.display = 'block';
}

// Función para registrar un pago
function registrarPago(clienteId) {
  const cliente = clientes.find(c => c.id === clienteId);
  if (cliente.cuotasPendientes > 0) {
    cliente.cuotasPendientes -= 1;
    alert(`Pago registrado. Cuotas pendientes: ${cliente.cuotasPendientes}`);
    mostrarClientes();
  } else {
    alert("No hay cuotas pendientes.");
  }
}

// Función para llamar a un cliente
function llamarCliente(telefono) {
  window.location.href = `tel:${telefono}`;
}

// Función para enviar WhatsApp a un cliente
function enviarWhatsApp(telefono) {
  window.location.href = `https://wa.me/${telefono}`;
}

// Función para editar los datos de un cliente
function editarCliente(clienteId) {
  const cliente = clientes.find(c => c.id === clienteId);
  const nuevoNombre = prompt('Nuevo nombre', cliente.nombre);
  const nuevaDireccion = prompt('Nueva dirección', cliente.direccion);
  const nuevaCuota = prompt('Nuevo valor de cuota', cliente.cuota);
  const nuevoMontoPrestamo = prompt('Nuevo monto del préstamo', cliente.montoPrestamo);

  cliente.nombre = nuevoNombre;
  cliente.direccion = nuevaDireccion;
  cliente.cuota = parseFloat(nuevaCuota);
  cliente.montoPrestamo = parseFloat(nuevoMontoPrestamo);

  mostrarClientes();
}

// Función para calcular el balance futuro
function calcularBalanceFuturo() {
  const semanas = [500, 1200, 800, 1500];  // Ejemplo de valores
  const meses = [4500, 3500, 5000];  // Ejemplo de valores

  const promedioSemana = semanas.reduce((a, b) => a + b, 0) / semanas.length;
  const promedioMes = meses.reduce((a, b) => a + b, 0) / meses.length;

  const futuroSemana = promedioSemana * 4;  // Aproximación al futuro en semanas
  const futuroMes = promedioMes * 12;  // Aproximación al futuro en meses

  alert(`Balance futuro: Semana: ${futuroSemana}, Mes: ${futuroMes}`);
}

// Función para calcular el balance de caja
function calcularBalanceCaja() {
  const gastos = parseFloat(document.getElementById('gastos').value);
  const cobrado = parseFloat(document.getElementById('cobrado').value);

  const balanceCaja = cobrado - gastos;

  alert(`Balance en caja: ${balanceCaja}`);
}

// Función para agregar un nuevo cliente (implementación básica)
function agregarCliente() {
  alert('Función para agregar cliente aún no implementada.');
}
</script>
</body>
</html>


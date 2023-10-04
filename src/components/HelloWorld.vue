<template>
  <div>
    <form class="formulario" @submit.prevent="agregarEstudiante">
      <h2>Detalle</h2>
      <div class="campo">
        <label for="matricula">Matrícula:</label>
        <input id="matricula" v-model="detalle.matricula" type="text" />
      </div>
      <div class="campo">
        <label for="montoCredito">Monto del Crédito:</label>
        <input id="montoCredito" v-model="detalle.montoCredito" type="number" />
      </div>
      <div class="campo">
        <label for="fechaAprobacion">Fecha de Aprobación:</label>
        <input id="fechaAprobacion" v-model="detalle.fechaAprobacion" type="date" />
      </div>
      <div class="campo">
        <label for="fechaVencimiento">Fecha de Vencimiento:</label>
        <input id="fechaVencimiento" v-model="detalle.fechaVencimiento" type="date" />
      </div>

      <button type="submit">Agregar Estudiante</button>
    </form>

    <button @click="generarArchivoJSON">Generar Archivo JSON</button>
    <h2>Estudiantes</h2>
    <table class="tabla-estudiantes">
      <thead>
      <tr>
        <th>Matrícula</th>
        <th>Monto del Crédito</th>
        <th>Fecha de Aprobación</th>
        <th>Fecha de Vencimiento</th>
        <th>Acciones</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(estudiante, index) in estudiantes" :key="index">
        <td>{{ estudiante.matricula }}</td>
        <td>{{ estudiante.montoCredito }}</td>
        <td>{{ estudiante.fechaAprobacion }}</td>
        <td>{{ estudiante.fechaVencimiento }}</td>
        <td>
          <button @click="eliminarEstudiante(index)">Eliminar</button>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      detalle: {
        matricula: '',
        montoCredito: 0,
        fechaAprobacion: '',
        fechaVencimiento: '',
      },
      estudiantes: [],
    }
  },
  methods: {
    agregarEstudiante() {
      this.estudiantes.push({ ...this.detalle })

      this.detalle = {
        matricula: '',
        montoCredito: 0,
        fechaAprobacion: '',
        fechaVencimiento: '',
      }
    },
    eliminarEstudiante(index) {
      this.estudiantes.splice(index, 1);
    },
    generarArchivoJSON() {
      if (this.estudiantes.length === 0) {
        alert('No hay estudiantes para generar el archivo JSON.');
        return;
      }

      const contenidoArchivo = JSON.stringify(this.estudiantes, null, 2);
      const blob = new Blob([contenidoArchivo], { type: 'application/json' });

      const url = window.URL.createObjectURL(blob);

      const a = document.createElement('a');
      a.href = url;
      a.download = 'estudiantes.json';
      a.click();

      window.URL.revokeObjectURL(url);
    },
  },
}
</script>

<style>
.formulario {
  width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
}

.campo {
  margin-bottom: 10px;
}

.campo input {
  margin-left: 10px;
}

button[type="submit"] {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
}

.lista-estudiantes {
  list-style: none;
  padding: 0;
}

.lista-estudiantes li {
  margin-bottom: 5px;
}

.tabla-estudiantes {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.tabla-estudiantes th,
.tabla-estudiantes td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.tabla-estudiantes th {
  background-color: #f2f2f2;
}

.tabla-estudiantes tr:hover {
  background-color: #f5f5f5;
}

button {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
}
</style>

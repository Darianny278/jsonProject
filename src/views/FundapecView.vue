<template>
  <div>
    <h1>Tabla de Estudiantes desde Archivo JSON</h1>
    <input type="file" @change="cargarArchivo" accept=".json" />
    <table v-if="estudiantes.length > 0" class="tabla-estudiantes">
      <thead>
        <tr>
          <th>Matrícula</th>
          <th>Monto del Crédito</th>
          <th>Fecha de Aprobación</th>
          <th>Fecha de Vencimiento</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(estudiante, index) in estudiantes" :key="index">
          <td>{{ estudiante.matricula }}</td>
          <td>{{ estudiante.montoCredito }}</td>
          <td>{{ estudiante.fechaAprobacion }}</td>
          <td>{{ estudiante.fechaVencimiento }}</td>
        </tr>
      </tbody>

      <tfoot>
        <tr>
          <td colspan="3">Total de Registros:</td>
          <td>{{ totalRegistros }}</td>
        </tr>
        <tr>
          <td colspan="3">Monto Total de Créditos:</td>
          <td>{{ calcularMontoTotal() }}</td>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      estudiantes: [],
    };
  },
  computed: {
    totalRegistros() {
      return this.estudiantes.length;
    },
  },
  methods: {
    cargarArchivo(event) {
      const archivo = event.target.files[0];

      if (archivo) {
        const lector = new FileReader();

        lector.onload = (e) => {
          const contenido = e.target.result;
          try {
            const jsonData = JSON.parse(contenido);
            this.estudiantes = jsonData;
          } catch (error) {
            alert('Error al analizar el archivo JSON.');
            this.estudiantes = [];
          }
        };

        lector.readAsText(archivo);
      } else {
        this.estudiantes = [];
      }
    },
    calcularMontoTotal() {
      return this.estudiantes.reduce((total, estudiante) => {
        return total + parseFloat(estudiante.montoCredito);
      }, 0);
    },
  },
};
</script>

<style scoped>
.tabla-estudiantes {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.tabla-estudiantes th, .tabla-estudiantes td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.tabla-estudiantes th {
  background-color: #f2f2f2;
}

.tabla-estudiantes tr:nth-child(even) {
  background-color: #f2f2f2;
}

.tabla-estudiantes tr:hover {
  background-color: #ddd;
}
</style>

<script>
  import { navigate } from "svelte-routing";

  const crearUsuario = (e) => {
    e.preventDefault();

    const request = new XMLHttpRequest();

    const formData = new FormData();

    const formElements = document.getElementById("F2").elements;

    const data = {};

    for (let i = 0; i < formElements.length; i++) {
      const currentElement = formElements[i];
      if (!["submit", "file"].includes(currentElement.type)) {
        data[currentElement.name] = currentElement.value;
      } else if (currentElement.type === "file") {
        if (currentElement.files.length === 1) {
          const file = currentElement.files[0];
          formData.append(`files.${currentElement.name}`, file, file.name);
        } else {
          for (let i = 0; i < currentElement.files.length; i++) {
            const file = currentElement.files[i];

            formData.append(`files.${currentElement.name}`, file, file.name);
          }
        }
      }
    }

    formData.append("data", JSON.stringify(data));

    request.open("POST", `https://colegio-b.herokuapp.com/Usuarios`);

    request.send(formData);

    alert("Registro exitoso!");
    navigate("/", { replace: true });
  };
</script>

<div>
  <h1>Alta de Usuario</h1>

  <!-- <p align="right"><a href="./index.html">Usuarios registrados-></a></p>
  <p align="right"><a href="./login.html">Login-></a></p> -->

  <form
    action=""
    method="POST"
    id="F2"
    on:submit={crearUsuario}
    class="formulario"
  >
    <h3>Ingrese sus datos:</h3>
    <br />
    <div class="entrada">
      <span>Nombre:</span><input type="text" name="nombre" required /><br /><br
      />

      <span>Apellido:</span><input type="text" name="apellido" required /><br
      /><br />

      <span>Email:</span><input type="email" name="email" required /><br /><br
      />
      <span>Fecha de nacimiento:</span><input
        type="date"
        name="fechanac"
        required
      /><br /><br />

      <span>Sexo:</span><input type="text" name="sexo" required /><br /><br />
      <span>Nacionalidad:</span><input
        type="text"
        name="nacionalidad"
        required
      /><br /><br />

      <label for="dni">Ingrese la foto de su dni:</label>

      <input type="file" name="fotodni" id="dni" /><br /><br />
    </div>

    <input class="btn btn-primary enviar" type="submit" value="Enviar" />
  </form>
</div>

<style>
  /* h1 {
    position: absolute;
    top: 15%;
    left: 5%;
  } */
  .formulario {
    text-align: center;
    position: absolute;
    padding: 2rem 1rem;
    margin-bottom: 2rem;
    background-color: #e9ecef;
    border-radius: 0.3rem;
    top: 60%;
    left: 35%;
    transform: translate(-50%, -50%);
    width: 690px;
    padding: 10px;
    border: 1px solid #000;
  }
  input,
  input:focus-visible {
    width: 70%;
    margin-left: 5px;
    padding: 2px;
    font-size: 14px;
    border-width: 1px;
    border-color: #cccccc;
    color: #000000;
    border-style: solid;
    border-radius: 8px;
    box-shadow: 0px 0px 2px rgba(66, 66, 66, 0.75);
  }
  .enviar {
    width: 20%;
  }
  .entrada {
    width: 100%;
    margin-bottom: 5px;
    padding-right: 15px;
    padding-left: 5px;
    text-align: right;
  }
</style>

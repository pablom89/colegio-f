<script>
  // const formElement = document.getElementById("F2");
  // console.log(formElement);

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
  };
</script>

<div>
  <h1>Nuevo Usuario:</h1>

  <p align="right"><a href="./index.html">Usuarios registrados-></a></p>
  <p align="right"><a href="./login.html">Login-></a></p>
  <h3>Ingrese sus datos:</h3>

  <br />

  <form action="" method="POST" id="F2" on:submit={crearUsuario}>
    <span>Nombre:</span><input type="text" name="nombre" required /><br /><br />
    <span>Apellido:</span><input type="text" name="apellido" required /><br
    /><br />

    <span>Email:</span><input type="email" name="email" required /><br /><br />
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

    <label for="dni">Ingrese la foto de su dni:</label><br /><br />

    <input type="file" name="fotodni" id="dni" /><br /><br />

    <input type="submit" value="Enviar" />
  </form>
</div>

<style>
</style>

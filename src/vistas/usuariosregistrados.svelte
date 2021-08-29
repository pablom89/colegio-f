<script>
  import { onMount } from "svelte";

  const confirmar = async (id) => {
    //alert('http://localhost:1337/usuarios/'+id);
    if (confirm("Desea verificar este usuario?")) {
      const respuesta = await fetch(
        "https://colegio-b.herokuapp.com/usuarios/" + id,
        {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            verificado: true,
          }),
        }
      )
        .then((res) => res.json())
        .then((info) => {
          console.log(info);
          const characters =
            "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
          let token = "";
          for (let i = 0; i < 25; i++) {
            token += characters[Math.floor(Math.random() * characters.length)];
          }
          const verificar = fetch(
            "https://colegio-b.herokuapp.com/usuarios-verificados",
            {
              method: "POST",
              headers: {
                "Content-Type": "application/json",
              },
              body: JSON.stringify({
                nombre: info.nombre,
                apellido: info.apellido,
                email: info.email,
                fechanac: info.fechanac,
                sexo: info.sexo,
                nacionalidad: info.nacionalidad,
                fotodni: info.fotodni,
                verificado: true,
                tokenclave: token,
              }),
              //				}),
            }
          )
            .then((response) => response.json())
            .then((data) => console.log(data));
          datos();
        });
    } else {
    }
  };

  function tabla(info) {
    contenido.innerHTML = "";
    for (let valor of info) {
      var fecha = new Date(valor.fechanac);
      var fechalocale = fecha.toLocaleDateString();
      var urlfoto = valor.fotodni.url;
      console.log(urlfoto);
      contenido.innerHTML += `
                <tr style="border: 1px solid black; border-collapse: collapse;">
                    <td style="border: 1px solid black; border-collapse: collapse;">${valor.id}</td>
                    <td style="border: 1px solid black; border-collapse: collapse;">${valor.nombre}</td> 
                    <td style="border: 1px solid black; border-collapse: collapse;">${valor.apellido}</td> 
                    <td style="border: 1px solid black; border-collapse: collapse;">${valor.email}</td>
                    <td style="border: 1px solid black; border-collapse: collapse;">${fechalocale}</td>
                    <td style="border: 1px solid black; border-collapse: collapse;">${valor.sexo}</td>
                    <td style="border: 1px solid black; border-collapse: collapse;">${valor.nacionalidad}</td>
                    <td style="border: 1px solid black; border-collapse: collapse;"><img src=${urlfoto} style="width: 33px"></td>
                    <td style="border: 1px solid black; border-collapse: collapse;"><button on:click={confirmar(${valor.id})}>Confirmar</button></td>     
                </tr>
                `;
    }
  }

  onMount(async () => {
    const respuesta = await fetch(
      "https://colegio-b.herokuapp.com/Usuarios?verificado=false",
      {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
      }
    )
      .then((res) => res.json())
      .then((info) => {
        console.log(info);
        tabla(info);
      });
  });
</script>

<div>
  <h1>Usuarios Registrados</h1>
  <br />
  <table>
    <thead>
      <td>ID</td>
      <td>Nombre</td>
      <td>Apellido</td>
      <td>Email</td>
      <td>Fecha de Nacimiento</td>
      <td>Sexo</td>
      <td>Nacionalidad</td>
      <td>DNI</td>
      <td>Editar</td>
    </thead>
    <tbody id="contenido" />
  </table>
</div>

<style>
  table,
  td {
    border: 1px solid black;
    border-collapse: collapse;
    margin: 15px;
  }
</style>

<script>
  import { onMount } from "svelte";

  let registros = [];

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
                
                `;
    }
  }

  onMount(async () => {
     await cargar();
  });

  async function cargar (){
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
        //console.log(info);
        //tabla(info);
        registros = [...info]
      });
  }

</script>

<div>
  <h1>Usuarios Registrados</h1>
  <br />
  <table>
    <thead>
      <tr>
      <td>ID</td>
      <td>Nombre</td>
      <td>Apellido</td>
      <td>Email</td>
      <td>Fecha de Nacimiento</td>
      <td>Sexo</td>
      <td>Nacionalidad</td>
      <td>DNI</td>
      <td>Editar</td>
    </tr>
    </thead>
    <tbody>
      {#each registros as r }

      <tr style="border: 1px solid black; border-collapse: collapse;">
        <td style="border: 1px solid black; border-collapse: collapse;">{r.id}</td>
        <td style="border: 1px solid black; border-collapse: collapse;">{r.nombre}</td> 
        <td style="border: 1px solid black; border-collapse: collapse;">{r.apellido}</td> 
        <td style="border: 1px solid black; border-collapse: collapse;">{r.email}</td>
        <td style="border: 1px solid black; border-collapse: collapse;"></td>
        <td style="border: 1px solid black; border-collapse: collapse;">{r.sexo}</td>
        <td style="border: 1px solid black; border-collapse: collapse;">{r.nacionalidad}</td>
        <td style="border: 1px solid black; border-collapse: collapse;"></td>
        <td style="border: 1px solid black; border-collapse: collapse;"><button on:click={confirmar(r.id)}>Confirmar</button></td>     
    </tr>
      {/each}
      </tbody>
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
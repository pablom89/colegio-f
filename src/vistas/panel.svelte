<script>
  import { navigate } from "svelte-routing";
  var chek;
  $: vchek = (() => {
    if (chek) {
      alert(chek);
    }
  })();
  const s = localStorage.getItem("s");
  if (!s) {
    alert("Debe iniciar sesión!");
    //window.location.replace('./login.html');
    navigate("/", { replace: true });
  }

  async function datos() {
    const s = localStorage.getItem("s");
    if (s) {
      const respuesta = await fetch(
        "https://colegio-b.herokuapp.com/materias",
        {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
          },
          credentials: "include",
        }
      )
        .then((res) => res.json())
        .then((info) => {
          console.log(info);
          tabla(info);
        });
    } else {
      //alert("Debe iniciar sesión!");
      //window.location.replace("./login.html");
      navigate("/", { replace: true });
    }
  }

  function tabla(info) {
    contenido.innerHTML = "";
    for (let valor of info) {
      contenido.innerHTML += `
                <tr style='border: 1px solid black;border-collapse: collapse;'>
                    <td style='border: 1px solid black;border-collapse: collapse;'>${valor.nombre}</td>
                    <td style='border: 1px solid black;border-collapse: collapse;'>${valor.nombre_completo}</td> 
                    <td style='border: 1px solid black;border-collapse: collapse;'>${valor.vigente}</td> 
                    <td style='border: 1px solid black;border-collapse: collapse;'>${valor.anulado}</td>
                    <td style='border: 1px solid black;border-collapse: collapse;'>${valor.fecha_vigencia}</td>
                    <td style='border: 1px solid black;border-collapse: collapse;'>${valor.fecha_anulada}</td>
                    <td style='border: 1px solid black;border-collapse: collapse;'>${valor.id_usuario_vigencia}</td>
                    <td style='border: 1px solid black;border-collapse: collapse;'>${valor.id_usuario_anulado}</td>
                    <td style='border: 1px solid black;border-collapse: collapse;'>${valor.fecha_alta}</td>
                    <td style='border: 1px solid black;border-collapse: collapse;'>${valor.id_usuario_alta}</td>
                    <td style='border: 1px solid black;border-collapse: collapse; text-align:center;'><input type="checkbox" name="anular" id="${valor.id}"></td>
                </tr>`;
    }
  }

  const cerrar = async () => {
    localStorage.removeItem("s");
    const respuesta = await fetch("https://colegio-b.herokuapp.com/logout", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      credentials: "include",
      body: JSON.stringify(),
    })
      .then((res) => res.json())
      .then((info) => {
        console.log(info);
        //window.location.replace("./login.html");
        navigate("/", { replace: true });
      });
  };

  const agm = () => navigate("/agregarmateria", { replace: true });
  const edit = () => {
    chek = document.querySelector('input[type="checkbox"]:checked').id;
    //alert(chek);
    //window.location.replace(`./editarmateria.html?materia=${chek}`);
    //navigate("/editarmateria", { replace: true });
  };

  const elm = async () => {
    var chek = document.querySelector('input[type="checkbox"]:checked').id;

    const ELIM = await fetch(
      `https://colegio-b.herokuapp.com/materias/${chek}`,
      {
        method: "DELETE",
        headers: {
          "Content-Type": "application/json",
        },
        credentials: "include",
      }
    )
      .then((res) => res.json())
      .then((info) => {
        //if (info.user.idv) {
        console.log(info);
        datos();

        //}
      })
      .catch((error) => {
        alert("Error: Usuario o contraseña invalido"); // error.message
      });
  };

  const anul = async () => {
    var chek = document.querySelector('input[type="checkbox"]:checked').id;
    var hoy = new Date();

    const ANUL = await fetch(
      `https://colegio-b.herokuapp.com/materias/${chek}`,
      {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        credentials: "include",
        body: JSON.stringify({
          anulado: true,
          vigente: false,
          fecha_anulada: hoy,
          id_usuario_anulado: 45,
        }),
      }
    )
      .then((res) => res.json())
      .then((info) => {
        //if (info.user.idv) {
        console.log(info);
        datos();

        //}
      })
      .catch((error) => {
        alert("Error: Usuario o contraseña invalido"); // error.message
      });
  };
</script>

<div class="contenedor" on:load={datos()}>
  <h1>Panel de control</h1>
  <br />
  <button style="float: right" on:click={cerrar} class="btn btn-dark"
    >Cerrar sesión</button
  >
  <button on:click={agm}>Alta de Materia</button>
  <button on:click={edit}>Editar Materia</button>
  <button onclick="anul()">Anular Materia</button>
  <button onclick="elm()">Eliminar Materia</button>
  <br /><br />
  <table style="float: clear">
    <thead>
      <td>Nombre</td>
      <td>Nombre completo</td>
      <td>Vigencia</td>
      <td>Anulado</td>
      <td>Fecha vigencia</td>
      <td>Fecha anulada</td>
      <td>Id vigencia</td>
      <td>Id anulado</td>
      <td>Fecha alta</td>
      <td>Id alta</td>
      <td style="text-align:center"
        ><label for="anulart">Todos</label><input
          type="checkbox"
          id="anulart"
          name="anulart"
        /></td
      >
    </thead>
    <tbody id="contenido" />
  </table>
</div>

<style>
  table,
  td {
    border: 1px solid black;
    border-collapse: collapse;
    margin: 1rem;
  }
  thead {
    font-weight: bold;
    background-color: lightgray;
  }
  button {
    margin-left: 5px;
    margin-right: 5px;
    padding: 2px;
    font-size: 14px;
    border-width: 1px;
    border-style: solid;
    border-radius: 8px;
    box-shadow: 0px 0px 2px rgba(66, 66, 66, 0.75);
  }
</style>

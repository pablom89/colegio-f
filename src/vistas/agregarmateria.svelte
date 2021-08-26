<script>
  import { navigate } from "svelte-routing";
  const s = localStorage.getItem("s");
  if (!s) {
    alert("Debe iniciar sesión!");
    //window.location.replace('./login.html');
    navigate("/", { replace: true });
  }
  const agregarmat = async () => {
    const info = new FormData(document.getElementById("F3"));
    let nombre = info.get("nombre");
    let nombre_completo = info.get("nombrecompleto");
    let hoy = new Date();

    let user = await fetch(`https://colegio-b.herokuapp.com/users/me`, {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
      },
      credentials: "include",
    });

    let infor = await user.json();

    alert(infor.id);

    if (infor.id) {
      const ING = await fetch(`https://colegio-b.herokuapp.com/materias`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        credentials: "include",
        body: JSON.stringify({
          nombre,
          nombre_completo,
          fecha_vigencia: hoy,
          fecha_alta: hoy,
          id_usuario_alta: infor.id,
          id_usuario_vigencia: infor.id,
        }),
      })
        .then((res) => res.json())
        .then((info) => {
          //if (info.user.idv) {
          console.log(info);
          //alert("Bienvenido " + info.user.idv.nombre);
          //window.localStorage.setItem('s', true);
          //window.location.replace("./panelusuario.html");
          navigate("/panel", { replace: true });
          //}
        })
        .catch((error) => {
          alert("Error: Usuario o contraseña invalido"); // error.message
        });
    } else {
    }
  };
</script>

<div>
  <h1>Alta de Materia</h1>
  <form
    action=""
    method="POST"
    id="F3"
    on:submit|preventDefault={agregarmat}
    class="formulario"
  >
    <h3>Agregar Materia:</h3>
    <br />
    <div class="entrada">
      <label for="nombre">Nombre:</label><input
        type="text"
        id="nombre"
        name="nombre"
        required
      /><br /><br />
    </div>
    <div class="entrada nc">
      <label for="nombrecompleto">Nombre completo:</label>
      <textarea
        id="nombrecompleto"
        name="nombrecompleto"
        rows="4"
        cols="50"
      /><br /><br />
    </div>

    <input
      type="submit"
      value="Agregar materia"
      class="btn btn-primary boton"
    /><br /><br />
  </form>
  <!-- <a href="./panelusuario.html"><--Volver</a> -->
</div>

<style>
  .formulario {
    text-align: center;
    position: absolute;
    padding: 2rem 1rem;
    margin-bottom: 2rem;
    background-color: burlywood;
    border-radius: 0.3rem;
    top: 50%;
    left: 30%;
    transform: translate(-50%, -50%);
    width: 690px;
    padding: 10px;
  }
  input,
  input:focus-visible,
  textarea {
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
  textarea {
    margin-bottom: 2rem;
  }
  .entrada {
    width: 100%;
    margin-bottom: 5px;
    padding-right: 15px;
    text-align: center;
  }
  .nc {
    display: flex;
    align-items: flex-start;
  }
  .boton {
    text-align: center;
    /*position: absolute;*/
    padding: 2rem 1rem;
    margin-left: 50%;
    margin-top: 2rem;
    margin-bottom: 2rem;
    border-radius: 0.3rem;
    /*top: 90%;
    left: 50%;*/
    transform: translate(-50%, -50%);
    width: 50%;
    padding: 10px;
  }
</style>

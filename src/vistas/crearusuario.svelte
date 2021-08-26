<script>
  import { navigate } from "svelte-routing";

  function getParameterByName(name) {
    name = name.replace(/[\[]/, "\\[").replace(/[\]]/, "\\]");
    var regex = new RegExp("[\\?&]" + name + "=([^&#]*)"),
      results = regex.exec(location.search);
    return results === null
      ? ""
      : decodeURIComponent(results[1].replace(/\+/g, " "));
  }
  var token = getParameterByName("token");
  //alert(token);
  const crearusuario = async () => {
    const respuesta = await fetch(
      `https://colegio-b.herokuapp.com/usuarios-verificados?tokenclave=${token}`,
      {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
      }
    )
      .then((res) => res.json())
      .then(async (res) => {
        //console.log(res[0]);
        //console.log("///////HOLA MUNDO //////////");
        const form = document.querySelector("form");
        const infor = new FormData(form);
        var info = res[0];
        var usuario = infor.get("usuario");
        var contra = infor.get("contraseña");
        if (info.tokenclave) {
          //console.log(info);
          const actualizarVerificados = await fetch(
            `https://colegio-b.herokuapp.com/usuarios-verificados/${info.id}`,
            {
              method: "PUT",
              headers: {
                "Content-Type": "application/json",
              },
              body: JSON.stringify({
                usuario,
                contra,
                idv: info.id,
              }),
            }
          )
            .then((res) => res.json())
            .then((info) => {
              console.log(info);
            });

          ////////////////////// Conectar con la tabla User ////////////////////////////

          const tablauser = await fetch(
            `https://colegio-b.herokuapp.com/users`,
            {
              method: "POST",
              headers: {
                "Content-Type": "application/json",
              },
              body: JSON.stringify({
                username: usuario,
                email: info.email,
                password: contra,
                confirmed: true,
                idv: info.id,
              }),
            }
          )
            .then((res) => res.json())
            .then((info) => {
              console.log(info);
            });

          ////////////////////// Conectar con la tabla User ////////////////////////////
        } else {
        }
      });

    alert("Usuario y contraseña creado con exito!");
    //window.location.href="./login.html";
    navigate("/", { replace: true });
  };
</script>

<div>
  <h1>Alta de usuario</h1>
  <br /><br />
  <form
    action=""
    method="POST"
    id="F1"
    on:submit|preventDefault={crearusuario}
    class="formulario"
  >
    <h1>Ingrese su Usuario y Contraseña:</h1>
    <br /><br />
    <div class="entrada">
      <span>Usuario:</span><input type="text" name="usuario" required /><br
      /><br />
    </div>
    <div class="entrada">
      <span>Contraseña:</span><input
        type="password"
        name="contraseña"
        required
      /><br /><br />
    </div>

    <input type="submit" value="Enviar" class="btn btn-primary boton" />
  </form>
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
  .entrada {
    width: 80%;
    margin-bottom: 5px;
    padding-right: 15px;
    text-align: right;
  }
</style>

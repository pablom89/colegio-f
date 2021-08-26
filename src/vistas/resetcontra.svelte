<script>
  import { Router, Link, Route, navigate } from "svelte-routing";
  const resetcontra = async () => {
    const info = new FormData(document.getElementById("f1"));

    var email = info.get("email");

    const RESET = await fetch(
      `https://colegio-b.herokuapp.com/auth/forgot-password`,
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          email,
          url: "https://colegio-b.herokuapp.com/admin/plugins/users-permissions/auth/reset-password",
        }),
      }
    )
      .then((response) => {
        // Handle success.
        alert("Se ha enviado un email a su casilla de correo!");
        //window.location.replace('./login.html');
        navigate("/", { replace: true });
      })
      .catch((error) => {
        // Handle error.
        console.log("An error occurred:", error.response);
      });
  };
</script>

<div>
  <h1>Blanqueo de clave</h1>
  <form
    action=""
    id="f1"
    class="formulario"
    on:submit|preventDefault={resetcontra}
  >
    <h3>Blanqueo de contraseña:</h3>
    <br /><br />
    <div class="entrada">
      <span>Email:</span><input
        type="email"
        name="email"
        placeholder="Ingrese su email"
      /><br /><br />
      <input type="submit" value="Enviar" class="btn btn-primary boton" /><br
      /><br />
    </div>
    <!-- <a href="./login.html"><--Volver</a> -->

    <Link to="/"><button class="btn btn-sm btn-dark">Volver</button></Link>
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
    width: 50%;
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
  .entrada {
    width: 100%;
    margin-bottom: 5px;
    padding-right: 15px;
    text-align: center;
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

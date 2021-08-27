<script>
  function getParameterByName(name) {
    name = name.replace(/[\[]/, "\\[").replace(/[\]]/, "\\]");
    var regex = new RegExp("[\\?&]" + name + "=([^&#]*)"),
      results = regex.exec(location.search);
    return results === null
      ? ""
      : decodeURIComponent(results[1].replace(/\+/g, " "));
  }
  var code = getParameterByName("code");

  const nuevacontra = async () => {
    const info = new FormData(document.getElementById("f1"));

    var contra = info.get("contra");

    const NVACONTRA = await fetch(
      `https://colegio-b.herokuapp.com/auth/reset-password`,
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          code,
          password: contra,
          passwordConfirmation: contra,
        }),
      }
    )
      .then((response) => {
        // Handle success.
        response.json();
      })
      .then((response) => {

        alert("Contraseña cambiada con exito!");
        console.log(response);

        navigate("/", { replace: true });
      })
      .catch((error) => {

        console.log("An error occurred:", error.response);
      });
  };
</script>

<div>
  <h1>Blanqueo de clave</h1>
  <br /><br />
  <form
    action=""
    id="f1"
    class="formulario"
    on:submit|preventDefault={nuevacontra}
  >
    <h3>Blanqueo de contraseña:</h3>
    <br /><br />

    <div class="entrada">
      <span>Nueva contraseña:</span><input
        type="password"
        name="contra"
        placeholder="Ingrese la nueva contraseña"
      /><br /><br />
    </div>
    <div class="entrada">
      <span>Repita su nueva contraseña:</span><input
        type="password"
        name="contrarep"
        placeholder="Ingrese la nueva contraseña"
      /><br /><br />
    </div>

    <input type="submit" value="Enviar" class="btn btn-success boton" />
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
    text-align: right;
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

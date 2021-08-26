<script>
  import { navigate } from "svelte-routing";
  const s = localStorage.getItem("s");
  if (!s) {
    alert("Debe iniciar sesión!");
    //window.location.replace('./login.html');
    navigate("/", { replace: true });
  }

  const editarmat = async () => {
    //alert(m);
  };
</script>

<div>
  <h1>Editar Materia</h1>
  <br /><br />
  <form
    action=""
    method="POST"
    id="F4"
    on:submit|preventDefault={editarmat}
    class="formulario"
  >
    <h3>Editar Materia:</h3>
    <div class="entrada">
      <label for="nombre">Nombre:</label><input
        type="text"
        id="nombre"
        name="nombre"
        required
      /><br /><br />
    </div>
    <div class="entrada nc">
      <p><label for="nombrecompleto">Nombre completo:</label></p>
      <textarea
        id="nombrecompleto"
        name="nombrecompleto"
        rows="4"
        cols="50"
      /><br /><br />
    </div>

    <input type="submit" value="Modificar" class="btn btn-primary boton" /><br
    /><br />
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

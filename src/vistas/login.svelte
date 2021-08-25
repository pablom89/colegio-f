<script>
  let email = "",
    password = "";

  const procesarFormulario = () => {
    if (!email.trim() || !password.trim()) {
      console.log("campos vacios");
      return;
    }
  };
</script>

<div class="contenedor">
  <br />
  <form on:submit|preventDefault={procesarFormulario} class="formulario">
    <h4>Acceder al sistema</h4>
    <div class="entrada">
      <label for="usuario" class="form-label">Usuario:</label>
      <input
        type="email"
        placeholder="Ingrese su email o usuario"
        id="usuario"
        bind:value={email}
      />
    </div>
    <div class="entrada">
      <label for="contra" class="form-label">Contraseña:</label>
      <input
        type="password"
        placeholder="Ingrese su contraseña"
        id="contra"
        bind:value={password}
      />
    </div>
    <button type="submit" class="btn btn-sm btn-primary">
      <i class="bi bi-arrow-right-circle" />
      <span>Continuar</span>
    </button>
  </form>
</div>

<style>
  div {
    margin: auto;
    width: 50%;
  }
  h4 {
    text-align: center;
    padding-bottom: 10px;
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
  button {
    border-radius: 28px;
    margin-top: 15px;
  }
  .entrada {
    width: 100%;
    margin-bottom: 5px;
    padding-right: 15px;
    text-align: right;
  }
  .formulario {
    text-align: center;
    position: absolute;
    padding: 2rem 1rem;
    margin-bottom: 2rem;
    background-color: #e9ecef;
    border-radius: 0.3rem;
    top: 40%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 350px;
    padding: 10px;
    border: 1px solid #000;
  }
</style>

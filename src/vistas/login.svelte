<script>
  import { Router, Link, Route, navigate } from "svelte-routing";

  let email = "",
    password = "";

  const procesarFormulario = async () => {
    /*if (!email.trim() || !password.trim()) {
      alert("Debe ingresar sus datos!");
      return;
    }
*/
    //alert(password);
    const info = new FormData(document.getElementById("F1"));
    console.log(info);
    var email = info.get("email");
    var contra = info.get("contra");

    if (!email.trim() || !contra.trim()) {
      alert("Debe ingresar sus datos!");
      return;
    } else {
      const ING = await fetch(`https://colegio-b.herokuapp.com/auth/local`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        credentials: "include",
        body: JSON.stringify({
          identifier: email,
          password: contra,
        }),
      })
        .then((res) => res.json())
        .then((info) => {
          if (info.user.idv) {
            console.log(info);
            alert("Bienvenido " + info.user.idv.nombre);
            window.localStorage.setItem("s", true);
            //window.location.replace("./panelusuario.html");
            navigate("/panel", { replace: true });
          }
        })
        .catch((error) => {
          alert("Error: Usuario o contraseña invalido"); // error.message
        });
    }
  };
</script>

<div class="contenedor">
  <br />
  <form
    on:submit|preventDefault={procesarFormulario}
    class="formulario"
    id="F1"
  >
    <h4>Acceder al sistema</h4>
    <div class="entrada">
      <label for="email" class="form-label">Email:</label>
      <input
        type="email"
        placeholder="Ingrese su email o usuario"
        id="email"
        name="email"
        bind:value={email}
      />
    </div>
    <div class="entrada">
      <label for="contra" class="form-label">Contraseña:</label>
      <input
        type="password"
        placeholder="Ingrese su contraseña"
        id="contra"
        name="contra"
        bind:value={password}
      />
    </div>
    <button type="submit" class="btn btn-sm btn-primary">
      <i class="bi bi-arrow-right-circle" />
      <span>Continuar</span>
    </button><br /><br />
    <Link to="/resetcontra">¿Olvidaste tu contraseña?</Link>
  </form>

  <Router>
    <nav>
      <Link to="/registro">
        <button class="btn btn-dark registrarse"
          >Registrarse por primera vez</button
        >
      </Link>
    </nav>
  </Router>
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
  .registrarse {
    text-align: center;
    position: absolute;
    padding: 2rem 1rem;
    margin-bottom: 2rem;
    border-radius: 0.3rem;
    top: 70%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 350px;
    padding: 10px;
  }
</style>

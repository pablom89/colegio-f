<script>
  import { Router, Link, Route } from "svelte-routing";
  import Login from "../vistas/login.svelte";
  import Panel from "../vistas/panel.svelte";
  import Reg from "../vistas/reg.svelte";
</script>

<div>
  <img src="../../colegiojulietadel.png" alt="colegiojulietadelfino" />
  <h3>Colegio Julieta Delfino</h3>

  <Router>
    <nav>
      <Link to="/">Login</Link>
      <Link to="/panel">Panel</Link>
      <Link to="/registro">Registrarse por primera vez</Link>
    </nav>

    <Route path="/">
      <Login />
    </Route>
    <Route path="/panel">
      <Panel />
    </Route>
    <Route path="/registro">
      <Reg />
    </Route>
  </Router>
</div>

<style>
  img {
    max-width: 15%;
    height: 100px;
    float: left;
  }
  h3 {
    float: left;
    padding: 25px;
  }
</style>

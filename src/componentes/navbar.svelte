<!-- <div>
  <h1>Hola desde un componente</h1>
</div> -->
<script>
  import { Router, Link, Route } from "svelte-routing";
  import Login from "../vistas/login.svelte";
  import Panel from "../vistas/panel.svelte";
</script>

<div>
  <img src="../../colegiojulietadel.png" alt="colegiojulietadelfino" />
  <h3>Colegio Julieta Delfino</h3>
  <Router>
    <nav>
      <Link to="/">Login</Link>
      <Link to="/panel">Panel con</Link>
    </nav>

    <Route path="/">
      <Login />
    </Route>
    <Route path="/panel">
      <Panel />
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

<script>
  import { Router, Link, Route, navigate } from "svelte-routing";
  import Login from "../vistas/login.svelte";
  import Panel from "../vistas/panel.svelte";
  import Reg from "../vistas/reg.svelte";
  import AgMat from "../vistas/agregarmateria.svelte";
  import EditMat from "../vistas/editarmateria.svelte";
  import CrearU from "../vistas/crearusuario.svelte";
</script>

<div>
  <img src="../../colegiojulietadel.png" alt="colegiojulietadelfino" />
  <h3>Colegio Julieta Delfino</h3>

  <Router>
    <nav>
      <Link to="/">Acceder</Link>
      <Link to="/panel">Mi panel</Link>
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
    <Route path="/agregarmateria">
      <AgMat />
    </Route>
    <Route path="/editarmateria">
      <EditMat />
    </Route>
    <Route path="/crearusuario">
      <CrearU />
    </Route>
  </Router>
</div>

<style>
  div {
    background-color: beige;
    height: 90px;
  }
  img {
    max-width: 15%;
    height: 90px;
    float: left;
  }
  h3 {
    float: left;
    padding: 25px;
  }
</style>

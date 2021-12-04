<template>
  <!-- Footer -->
  <div class="footer">
    <footer>
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item>
            &copy;  {{ new Date().getFullYear() }} Copyright:   Helados Malvarrosa
              </b-nav-item>        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-button squared variant="primary" size="lg" router-link to="/menu" nuxt
            >Volver al Menú Principal</b-button
          >
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    </footer>
  </div>

  <!-- Footer -->
</template>
<style>
.footer {
align-self: end;
  bottom: 0;
  width: 100%;
  flex-direction: column;
}
</style>

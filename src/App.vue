
<template>
  <div class="theme-navy">
    <nav class="navbar bg-dark navbar-dark">
      <div class="container-fluid justify-content-center">
        <div class="navbar-nav flex-row gap-3">
          <RouterLink to="/" class="nav-link px-2">Cálculo</RouterLink>
          <RouterLink to="/sobre" class="nav-link px-2">Sobre</RouterLink>
        </div>
      </div>
    </nav>


    <main>
      <RouterView />
    </main>
    
    <footer class="footer-minimal bg-dark text-light py-3 mt-5">
    <div class="container text-center small">
      <span>Facilitando a vida de professores e alunos.</span>
    </div>
    </footer>

  </div>
</template>


<script>
  export default { name: 'LayoutNovo' };
</script>

<style scoped>
  .footer-minimal {
    border-top: 1px solid rgba(255, 255, 255, 0.12);
    letter-spacing: 0.2px;
  }
  
  .theme-navy {
    --brand-navy: #0b1f3a;
    --brand-black: #0a0a0a;
    --brand-gray: #6c757d;

    --bs-primary: var(--brand-navy);
    --bs-primary-rgb: 11,31,58;
    --bs-dark: var(--brand-black);
    --bs-dark-rgb: 10,10,10;
    --bs-secondary: var(--brand-gray);
    --bs-secondary-rgb: 108,117,125;
  }

  .navbar .nav-link.router-link-active,
  .navbar .nav-link.router-link-exact-active {
    color: #fff;
    font-weight: 600;
    border-bottom: 2px solid var(--bs-primary);
  }

</style>
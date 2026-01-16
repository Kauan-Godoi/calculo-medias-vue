
<template>
  <div class="theme-navy">
    <div class="container py-5">
      <div class="row justify-content-center">
        <div class="col-12 col-lg-8">
          <div class="card shadow-sm border-0">
            <div class="card-header bg-primary text-white">
              <h2 class="h4 mb-0">
                <i class="bi bi-info-circle"></i> Sobre o Sistema
              </h2>
            </div>

            <div class="card-body">
              <p class="lead">
                Nosso objetivo é <strong>facilitar a vida de professores e alunos</strong> através de uma plataforma simples e eficiente.
              </p>
              <p>Com este sistema, você pode:</p>

              <ul class="list-group list-group-flush mb-3">
                <li class="list-group-item">
                  <i class="bi bi-check-circle text-primary"></i> Cadastrar alunos e suas notas de forma rápida.
                </li>
                <li class="list-group-item">
                  <i class="bi bi-check-circle text-primary"></i> Calcular médias automaticamente, evitando erros manuais.
                </li>
                <li class="list-group-item">
                  <i class="bi bi-check-circle text-primary"></i> Editar e excluir registros com facilidade.
                </li>
                <li class="list-group-item">
                  <i class="bi bi-check-circle text-primary"></i> Visualizar resultados de forma clara e organizada.
                </li>
              </ul>

              <p>
                Tudo isso com uma interface intuitiva e responsiva, pensada para otimizar o tempo e reduzir a complexidade das tarefas escolares.
              </p>

              <router-link to="/" class="btn btn-outline-dark mt-3">
                <i class="bi bi-arrow-left"></i> Voltar para o calculo
              </router-link>
            </div>

            <div class="card-footer bg-body-tertiary text-muted small">
              Desenvolvido para tornar a gestão de notas mais simples e eficiente.
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default { name: 'AboutView' };
</script>

<style>
.theme-navy {
  --brand-navy: #0b1f3a;
  --brand-black: #0a0a0a;
  --brand-gray: #6c757d;

  --bs-primary: var(--brand-navy);
  --bs-primary-rgb: 11, 31, 58;

  --bs-dark: var(--brand-black);
  --bs-dark-rgb: 10, 10, 10;

  --bs-secondary: var(--brand-gray);
  --bs-secondary-rgb: 108, 117, 125;

  --navy-subtle-bg: rgba(var(--bs-primary-rgb), 0.12);
  --navy-subtle-text: var(--bs-primary);
  --dark-subtle-bg: rgba(var(--bs-dark-rgb), 0.12);
  --dark-subtle-text: var(--bs-dark);

  --bs-link-color: var(--bs-primary);
  --bs-link-hover-color: color-mix(in oklab, var(--bs-primary) 85%, black);
}

.card-header .badge { font-weight: 600; }
.list-group-item { transition: background-color .15s ease-in-out; }
.list-group-item:hover { background-color: rgba(0,0,0,0.03); }
</style>

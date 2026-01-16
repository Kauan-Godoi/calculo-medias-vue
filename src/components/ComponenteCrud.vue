
<template>
  <div class="theme-navy">
    <div class="container py-4">
      <div class="row justify-content-center">
        <div class="col-12 col-lg-8">
          <div class="card shadow-sm border-0">
            <div class="card-header bg-primary text-white d-flex align-items-center justify-content-between">
              <h2 class="h5 mb-0 d-flex align-items-center gap-2">
                <i class="bi bi-calculator"></i>
                Cálculo de médias
              </h2>
              <span class="badge bg-light text-primary border-0">
                {{ editando ? 'Editando' : 'Novo cadastro' }}
              </span>
            </div>

            <div class="card-body">
              <form @submit.prevent="salvar" novalidate>
                <div class="row g-3">
                  <div class="col-12">
                    <div class="form-floating">
                      <input
                        id="nome"
                        type="text"
                        class="form-control"
                        v-model.trim="form.nome"
                        placeholder="Digite o nome do aluno"
                        autocomplete="off"
                        maxlength="80"
                        required
                      />
                      <label for="nome">Nome do aluno</label>
                    </div>
                  </div>

                  <div class="col-12 col-md-4">
                    <div class="form-floating">
                      <input
                        id="p1"
                        type="number"
                        class="form-control"
                        v-model.number="form.P1"
                        placeholder="Prova 1"
                        min="0"
                        max="10"
                        step="0.01"
                        required
                      />
                      <label for="p1">Prova 1</label>
                    </div>
                  </div>

                  <div class="col-12 col-md-4">
                    <div class="form-floating">
                      <input
                        id="p2"
                        type="number"
                        class="form-control"
                        v-model.number="form.P2"
                        placeholder="Prova 2"
                        min="0"
                        max="10"
                        step="0.01"
                        required
                      />
                      <label for="p2">Prova 2</label>
                    </div>
                  </div>

                  <div class="col-12 col-md-4">
                    <div class="form-floating">
                      <input
                        id="p3"
                        type="number"
                        class="form-control"
                        v-model.number="form.P3"
                        placeholder="Prova 3"
                        min="0"
                        max="10"
                        step="0.01"
                        required
                      />
                      <label for="p3">Prova 3</label>
                    </div>
                  </div>

                  <div class="col-12 d-flex gap-2">
                    <button type="submit" class="btn btn-primary">
                      <i class="bi" :class="editando ? 'bi-pencil-square' : 'bi-save'"></i>
                      <span class="ms-1">{{ editando ? 'Editar' : 'Salvar' }}</span>
                    </button>

                    <button
                      type="button"
                      class="btn btn-outline-secondary"
                      @click="limparFormulario"
                    >
                      <i class="bi bi-x-circle"></i>
                      <span class="ms-1">Cancelar</span>
                    </button>
                  </div>
                </div>
              </form>

              <hr class="my-4" />

              <div>
                <div class="d-flex align-items-center justify-content-between mb-2">
                  <h3 class="h6 mb-0">
                    <i class="bi bi-card-checklist me-1"></i>
                    Registros
                  </h3>
                  <span class="text-muted small">{{ lista.length }} item(s)</span>
                </div>

                <div
                  v-if="lista.length === 0"
                  class="alert alert-light border d-flex align-items-center"
                  role="alert"
                >
                  <i class="bi bi-info-circle me-2"></i>
                  Nenhum registro ainda. Preencha os campos e clique em
                  <strong>Salvar</strong>.
                </div>

                <ul v-else class="list-group">
                  <li
                    v-for="(iten, index) in lista"
                    :key="index"
                    class="list-group-item d-flex flex-column flex-md-row align-items-md-center justify-content-between gap-2"
                  >
                    <div class="d-flex align-items-center gap-3">
                      <span class="badge rounded-pill bg-secondary-subtle text-secondary-emphasis">
                        {{ index + 1 }}
                      </span>

                      <div class="d-flex flex-column">
                        <div class="fw-semibold text-dark">
                          {{ iten.nome }}
                        </div>
                        <div class="text-muted small">
                          P1: <span class="fw-medium">{{ iten.P1 }}</span> ·
                          P2: <span class="fw-medium">{{ iten.P2 }}</span> ·
                          P3: <span class="fw-medium">{{ iten.P3 }}</span>
                        </div>
                      </div>
                    </div>

                    <div class="d-flex align-items-center gap-3">
                      <!-- Verde >= 6 / Vermelho < 6 -->
                      <span
                        class="badge"
                        :class="mediaDoItem(iten) >= 6
                          ? 'bg-success-subtle text-success-emphasis'
                          : 'bg-danger-subtle text-danger-emphasis'"
                      >
                        Média: {{ mediaDoItem(iten).toFixed(2) }}
                      </span>

                      <div class="btn-group btn-group-sm" role="group">
                        <button
                          type="button"
                          class="btn btn-outline-primary"
                          @click="editar(index)"
                        >
                          <i class="bi bi-pencil"></i>
                          <span class="d-none d-sm-inline ms-1">Editar</span>
                        </button>
                        <button
                          type="button"
                          class="btn btn-outline-dark"
                          @click="deletar(index)"
                        >
                          <i class="bi bi-trash"></i>
                          <span class="d-none d-sm-inline ms-1">Deletar</span>
                        </button>
                      </div>
                    </div>
                  </li>
                </ul>
              </div>
            </div>

            <div class="card-footer bg-body-tertiary small text-muted">
              Regra de aprovação usada aqui: média <strong>&ge; 6.0</strong>.
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const indexEditado = ref(null);
    const editando = ref(false);
    const mediaFinal = ref(null);

    const form = ref({
      nome: '',
      P1: null,
      P2: null,
      P3: null,
    });

    const lista = ref([]);

    const calcMedia = ({ P1, P2, P3 }) => {
      const n1 = Number(P1);
      const n2 = Number(P2);
      const n3 = Number(P3);
      if ([n1, n2, n3].some(n => Number.isNaN(n))) return 0;
      return (n1 + n2 + n3) / 3;
    };

    const salvar = () => {
      if (!form.value.nome?.trim()) return;
      if (form.value.P1 == null || form.value.P2 == null || form.value.P3 == null) return;

      mediaFinal.value = calcMedia(form.value);

      if (editando.value && indexEditado.value !== null) {
        lista.value[indexEditado.value].nome = form.value.nome.trim();
        lista.value[indexEditado.value].P1 = Number(form.value.P1);
        lista.value[indexEditado.value].P2 = Number(form.value.P2);
        lista.value[indexEditado.value].P3 = Number(form.value.P3);

        editando.value = false;
        indexEditado.value = null;
      } else {
        lista.value.push({
          nome: form.value.nome.trim(),
          P1: Number(form.value.P1),
          P2: Number(form.value.P2),
          P3: Number(form.value.P3),
        });
      }

      limparFormulario();
    };

    const editar = (index) => {
      const item = lista.value[index];
      form.value.nome = item.nome;
      form.value.P1 = item.P1;
      form.value.P2 = item.P2;
      form.value.P3 = item.P3;
      editando.value = true;
      indexEditado.value = index;
      mediaFinal.value = calcMedia(form.value);
    };

    const deletar = (index) => {
      lista.value.splice(index, 1);
      if (indexEditado.value === index) {
        limparFormulario();
      }
    };

    const limparFormulario = () => {
      form.value = { nome: '', P1: null, P2: null, P3: null };
      indexEditado.value = null;
      editando.value = false;
      mediaFinal.value = null;
    };

    const mediaDoItem = (iten) => calcMedia(iten);

    return {
      form,
      lista,
      editando,
      indexEditado,
      mediaFinal,
      salvar,
      editar,
      deletar,
      limparFormulario,
      mediaDoItem,
    };
  },
};
</script>

<style>
.theme-navy {
  --brand-navy: #0b1f3a;   /* Azul-marinho */
  --brand-black: #0a0a0a;  /* Preto */
  --brand-gray: #6c757d;   /* Cinza */

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

.badge-navy-subtle {
  background-color: var(--navy-subtle-bg);
  color: var(--navy-subtle-text);
  border: 1px solid rgba(var(--bs-primary-rgb), 0.25);
}

.badge-dark-subtle {
  background-color: var(--dark-subtle-bg);
  color: var(--dark-subtle-text);
  border: 1px solid rgba(var(--bs-dark-rgb), 0.25);
}

.card-header .badge {
  font-weight: 600;
}

.list-group-item {
  transition: background-color 0.15s ease-in-out;
}

.list-group-item:hover {
  background-color: rgba(0, 0, 0, 0.03);
}
</style>

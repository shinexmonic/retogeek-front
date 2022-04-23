<template>
    <Tareas tipoTarea="completa" />
</template>

<script>
export default {
  name: "Completas",
  components: {
    Tareas: () => import("@/components/Tareas")
  }
};
</script>

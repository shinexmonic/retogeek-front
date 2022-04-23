<template>
    <Tareas tipoTarea="pendiente" />
</template>

<script>
export default {
  name: "Pendientes",
  components: {
    Tareas: () => import("@/components/Tareas")
  }
};
</script>

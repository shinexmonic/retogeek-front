<template>
    <Tareas tipoTarea="incompleta" />
</template>

<script>
export default {
  name: "Incompletas",
  components: {
    Tareas: () => import("@/components/Tareas")
  }
};
</script>

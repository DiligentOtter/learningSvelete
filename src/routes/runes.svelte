<script lang="ts">
    /* Existen varios tipos de runas
        Tenemos $state(), permite declarar una variable como reactiva
        La cual es solo reactiva a reasignaciones, sin embargo tambien es
        sensible a mutaciones, algo no muy recomendado, pero posible
        Tenemos $derived(), permite derivar una variable 
        reactiva a partir de otras, cambia cuando sus state componentes
        lo hacen y si solo ese cambio provoca una mutacion o reasignacion
        donde el valor nuevo sea diferente del viejo
        Tenemos $effect(), permite ejecutar un efecto secundario cuando
        una variable reactiva cambia, ya sea un state, derived o prop y solamente
        en reasignaciones, no mutaciones.
        Por ultimo tenemos los $props o properties, los cuales
        funcionan como los atributos de un componente/entradas
        por las cuales un componente padre pasa valores al hijo/dependencia
        Como si fuera una clase componente.
    */
    
</script>


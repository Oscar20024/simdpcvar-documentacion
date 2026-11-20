# ADR-W-014 — Las solicitudes de la app se aprueban como «Operador de campo», sin elegir rol

14/11/2026 · Estado: aceptada por Oscar · Plataforma v1.6.1 · App sin cambios (0.6.1) · Autor: Claude (asistente de
desarrollo)

**Contexto.** Las cuentas que se piden desde la app (`POST /api/v1/auth/register`) llegan a «Solicitudes pendientes»
de Usuarios. La web mostraba ahí las casillas de los cuatro roles, incluidos Especialista, Supervisor y Administrador,
que son solo para cuentas de la web (ADR-W-005). No tenía sentido: quien se registra en la app es un operador de campo,
y elegir un rol de la web para él solo abría la puerta a errores. Lo mismo pasaba en «Todas las cuentas» → «Roles»:
a una cuenta de la app se le podían ofrecer roles de la web.

**Decisión.**

1. **Aprobar** una solicitud no pide rol: la cuenta queda con `OPERADOR_CAMPO`. La vista ignora cualquier `roles`
   enviado (no se puede forzar a mano). Mensaje: «Cuenta aprobada como «Operador de campo»…».
2. **«Roles» en Todas las cuentas** solo aparece en cuentas de la web y solo ofrece Especialista, Supervisor y
   Administrador (`RolesWebForm`). A una cuenta de la app no se le cambian los roles (error `ROLES_CUENTA_APP`) y a una
   de la web no se le agrega «Operador de campo» (error `ROLES_SOLO_WEB`).
3. Las cuentas de la web se siguen creando solo con «Nueva cuenta» → «Plataforma web» (ADR-W-005).
4. Ayuda de la página (`web/ayudas.py`), manual de la plataforma web y documentación del sistema actualizados.

**Consecuencias.** Sin migraciones ni dependencias nuevas. `cuentas.services` no cambia (sigue validando la
combinación de roles). Pruebas: `test_aprobar_solicitud_de_la_app_queda_como_operador` y
`test_operador_no_se_combina_con_roles_de_la_web` (307 pruebas OK). Solicitud de cambio asociada: SC-08 (reparación
de defecto) en la documentación del sistema.

---
name: obsidian-source-of-truth
description: Use to search, read, update or create notes in an Obsidian vault treated as the single source of truth, including notes in other local folders, and to plan consolidating them into one vault.
---

# Obsidian como fuente de verdad

La fuente de verdad es el vault principal de Obsidian y las carpetas adicionales registradas. Antes de responder sobre un proyecto, cliente, proceso o decisión, consulta esas fuentes. Si la información no existe en ninguna, dilo explícitamente y no la inventes.

## Configuración

Guarda la configuración en `obsidian-sources.json` dentro del vault principal:

```json
{
  "vault": "C:\\Users\\USUARIO\\CEO\\MiVault",
  "extra_roots": [],
  "destino_unico": "C:\\Users\\USUARIO\\CEO\\MiVault"
}
```

1. Si el archivo no existe, pide la ruta del vault una sola vez, créalo y continúa.
2. `extra_roots` contiene las carpetas donde pueda haber notas fuera del vault. Si el usuario menciona una carpeta nueva, añádela.
3. Confirma la ruta del vault comprobando que existe la carpeta `.obsidian/`.

## Lectura: buscar en todas las fuentes

Cuando el usuario pida algo como "busca mi trabajo en LeadNicho":

1. Busca el término en `*.md` dentro del vault y de cada carpeta de `extra_roots`. Incluye alias, títulos y nombres de archivo.
2. Si no aparece nada, busca variantes: mayúsculas, tildes, guiones y el nombre sin espacios.
3. Lee completa la nota más relevante y, si hace falta, las demás.
4. Responde solo con lo leído. Cita cada dato con la ruta completa del archivo, o con `[[nota]]` si está dentro del vault.
5. Si hay notas contradictorias, muestra ambas con su fecha (`updated` o última línea del Historial). No elijas una en silencio.
6. Si no hay resultados en ninguna fuente, responde "No aparece en ninguna fuente registrada" y pregunta en qué carpeta podría estar.

## Escritura: actualizar sin duplicar

1. Antes de crear una nota, busca si ya existe en cualquier fuente registrada. Si existe, actualízala donde esté.
2. Edita en su sitio con ediciones exactas. No reescribas notas enteras salvo que se pida.
3. Conserva el frontmatter. Si cambias contenido, actualiza `updated: YYYY-MM-DD`.
4. Añade una línea en `## Historial` al final de la nota: `- YYYY-MM-DD: qué cambió y por qué`.
5. Usa enlaces `[[nota]]` dentro del vault. Una nota nueva debe tener al menos un enlace entrante.
6. Nunca borres notas ni secciones sin confirmación explícita. Para retirar contenido, márcalo `obsoleto: YYYY-MM-DD` y enlaza su sustituto.
7. Nunca escribas claves de API, contraseñas, tokens ni datos sensibles.

## Consolidación en una carpeta

Solo se activa cuando el usuario lo pida explícitamente ("consolida", "migra", "unifica").

1. Inventario: lista todas las notas de `extra_roots` con tamaño, fecha de modificación y si ya existe una nota con el mismo nombre en el destino.
2. Plan (sin mover nada): muestra una tabla `origen → destino` y los enlaces que habría que reescribir. Marca los conflictos de nombre.
3. Espera confirmación. Sin ella no copies ni muevas nada.
4. Ejecución: copia (no muevas) a `destino_unico` en la subcarpeta `_migracion/` del vault. Reescribe los enlaces afectados. Guarda un manifiesto `_migracion/manifest.md` con origen, destino, fecha y hash de cada archivo.
5. Verificación: compara hashes entre origen y destino y comprueba que no hay enlaces rotos.
6. Los originales no se borran. Ofrece borrarlos solo tras confirmación y cuando la verificación haya pasado.
7. Tras la migración, vacía `extra_roots` y deja un único vault en la configuración.

## Formato de respuesta

- Respuesta directa primero, después las fuentes.
- Fuentes al final: `Fuentes: <ruta o [[nota]]>`.
- Separa lo que dice el vault de tu inferencia.

## Portabilidad

No incluyas rutas ni notas personales en el repositorio. `obsidian-sources.json` se queda en el vault y se añade a `.gitignore`.

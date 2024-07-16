## Despliega JSON Server en Vercel

### Cómo usar (resumen)

1. Clona este repositorio.
2. Actualiza o usa el [`db.json`](./db.json) predeterminado en el repositorio.
3. Regístrate o inicia sesión en [Vercel](https://vercel.com).
4. Desde el panel de Vercel, haz clic en "**+ New Project**" y luego "**Import**" tu repositorio.
5. En la pantalla "**Configure Project**", deja todo como está por defecto y haz clic en "**Deploy**".
6. Espera hasta que el despliegue esté completo, ¡y tu propio servidor JSON estará listo para funcionar!

## Créalo por Ti Mismo

### Paso 1

Crea un nuevo repositorio, por ejemplo, **alurageek-API**. Luego clona ese repositorio vacío.

### Paso 2

Necesitas ejecutar el comando `npm init`:

```
npm init -y
```

Esto generará un **package.json**. Ahora, lo que necesitas hacer es cambiar estas líneas:

Cambia esta línea:

```
 "main": "index.js",
```

A esto:

```
  "main": "api/server.js",
```

Y esto:

```
"test": "echo \"Error: no test specified\" && exit 1"
```

A esto:

```
"start": "node api/server.js"
```

### Paso 3

Ahora es el momento de ejecutar el comando:

```
npm install json-server cors
```

![Alt text](image.png)

Verás que tanto **cors** como **_json-server_** se han agregado al package.json.

### Paso 4

Ejecuta el comando:

```
npm install json-server
```

Agrega el **_.gitignore_** y agrega **_node_modules_**.

### Paso 5

Crea un archivo **_db.json_** y agrega tus propios datos.

Además, necesitarás agregar una nueva [Carpeta llamada **_api_**](./api/) y, dentro de ella, este archivo [**server.js**](./api/server.js):

```javascript
// Ver https://github.com/typicode/json-server#module
const jsonServer = require("json-server");
const server = jsonServer.create();
const router = jsonServer.router("db.json");
const middlewares = jsonServer.defaults();

server.use(middlewares);
// Agrega esto antes de server.use(router)
server.use(
  jsonServer.rewriter({
    "/api/*": "/$1",
    "/producto/:recurso/:id/ver": "/:recurso/:id",
  })
);
server.use(router);
server.listen(3000, () => {
  console.log("El servidor JSON está funcionando");
});

// Exporta la API del Servidor
module.exports = server;
```

### Paso 6

Crea un archivo nuevo llamado [**_vercel.json_**](./vercel.json)

```json
{
  "functions": {
    "api/server.js": {
      "memory": 1024,
      "includeFiles": "db.json"
    }
  },
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "api/server.js"
    }
  ]
}
```

# No olvides hacer commit y push a tus cambios 🐣

Ve a tu cuenta de Vercel, conecta un nuevo proyecto con tu repositorio y despliega 💙

## Debes tener paciencia

Puede tomar varios minutos para que funcione correctamente. ⏰

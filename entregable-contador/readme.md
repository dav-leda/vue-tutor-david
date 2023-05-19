<base target="_blank">

# Curso Vue.js 💻️ 🛠️
## Tutor: David Leda

---

Hola 🙋‍♂️️ Aquí les paso algunas recomendaciones para el entregable 1: __Contador con Vue__.

__1.__ Recuerden instalar la extensión [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) para Visual Studio Code. Una vez instalada deben darle click derecho sobre el archivo .html que quieran abrir y seleccionan _Open with Live Server_. Ahí van a poder ver su proyecto corriendo en el browser (Chrome, Edge, o el que usen).

__2.__ Luego abran la consola del browser con la tecla F12 (o Shift Ctrl J) y chequeen que no les esté dando ningún error. Antes de hacer las entregas siempre revisen si el proyecto no les está tirando algún error por consola. Si no pueden solucionar el error, no hay problema, pueden entregar igual, pero en ese caso avisen al hacer la entrega: "Me está dando tal error por consola."

__3.__ Traten de hacer la entrega enviando __el link al repositorio en GitHub, no en un archivo .zip, ni un link de Dropbox o Google Drive.__ Sería bueno que se vayan acostumbrando a usar GitHub, porque más tarde lo van a tener que usar.

__4.__ Para el CSS pueden usar lo que quieran, __no es obligatorio usar Bootstrap__. Puede ser CSS puro, o SASS, o algún otro framework como Tailwind o Bulma. O también pueden entregar sin CSS. Mientras la parte de Vue funcione es suficiente ✔️

__5.__ No es necesario que el número del contador esté dentro de un tag del tipo _input readonly_, puede estar dentro de un `<span>` o un `<p>`. 

__6.__ Si ponen el `<script>` dentro del `<head>` no se olviden de incluir el atributo `defer`, para que el script sea ejecutado luego de que el html haya sido renderizado:

```html
<script src="app.js" defer></script> 
```

Acá les paso [un ejemplo](https://dav-leda.github.io/vue3-ts-counter/) de cómo debería quedar (no es necesario que se vea exactamente igual, sólo el funcionamiento).

Cualquier duda que tengan me pueden escribir por [Discord](https://discord.gg/nY6xWbDm) o por la plataforma de Coder House 💬️
# Configurar .env no react vite
[Tutorial](https://medium.com/@RajiDevMind/importing-dotenv-env-in-your-react-vite-latest-e4e9f9bc80c7)

Instale as seguintes dependências:

1. `npm install dotenv`

Crie o arquivo `.env` na raiz do projeto e configure as variáveis de ambiente no arquivo com prefixo `VITE_`.

Altere o arquivo `vite.config.js` da seguinte forma:

```javascript
import { defineConfig } from 'vite';
import { config } from 'dotenv';

// Load environment variables from .env file
config();

export default defineConfig({
  // Your Vite configuration
  define: {
    'process.env': process.env
  }
});
```

Acesse as variáveis via `import.meta.env.VITE_...`;
## Step 1
Select
<img width="196" height="128" alt="Screenshot 2026-03-11 at 6 11 37 PM" src="https://github.com/user-attachments/assets/3c4c219d-5c2b-4858-9914-68580d5ba08e" />

## Step 2
Select type `Multi File` and template `NodeJS` and click on `Setup initial files`
<img width="634" height="392" alt="Screenshot 2026-03-11 at 6 21 03 PM" src="https://github.com/user-attachments/assets/016234da-293e-40c0-9f85-4ea619baaed6" />

A new tab will open just the way question will be visible to the candidates.

## Step 3
Drag and drop folder/files to the vscode, it'll automatically be added to the vscode like this.
<img width="1275" height="757" alt="Screenshot 2026-03-11 at 6 23 42 PM" src="https://github.com/user-attachments/assets/12d2a97d-25f1-4169-9087-7e93b12499a4" />

as you can see I've dragged and dropped the folder `vitejs-vite-gvqtkjas` in the project.

Next to open the preview I'll create one folder `.vscode` with a single file `settings.json` with content:
```json
{
    "remote.portsAttributes": {
      "5173": {
        "onAutoForward": "openPreview"
      }
    }
  }
```

and then I'll modify the vite config file `vite.config.ts`.
```js
import { defineConfig, Plugin } from 'vite';
import react from '@vitejs/plugin-react-swc';
import path from 'path';

const PROXY_BASE = '/proxy/5173/';

// Reconciles code-server's prefix stripping with Vite's base routing
function codeServerProxy(): Plugin {
  return {
    name: 'code-server-proxy-fix',
    configureServer(server) {
      server.middlewares.use((req, _res, next) => {
        // code-server already stripped /proxy/5173, but Vite expects it
        if (req.url && !req.url.startsWith(PROXY_BASE)) {
          req.url = PROXY_BASE.slice(0, -1) + req.url;
        }
        next();
      });
    },
  };
}

export default defineConfig({
  base: PROXY_BASE,
  plugins: [codeServerProxy(), react()],
  server: {
    host: '0.0.0.0',
    hmr: {
      clientPort: 443,
    },
  },
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
});
```
above code is applicable only if code will run on port `5173` (default vite port) we can modify it accordingly.

> **Note**: We should 
`npm i` before we try to run the project

## Step 4
Now if we run the project using `npm run dev` and preview will automatically open in the right pane of vscode.
<img width="1279" height="760" alt="Screenshot 2026-03-11 at 6 28 52 PM" src="https://github.com/user-attachments/assets/e1ca70f4-874c-4687-bbb0-f36640064c7f" />

Candidate can modify the placement as per his/her convenience.

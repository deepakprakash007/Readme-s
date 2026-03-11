# Setting Up a Multi-File Node.js Project with Live Preview

## Step 1 — Create a New Question

Click the **New Question** button to get started.

![New Question button](https://github.com/user-attachments/assets/3c4c219d-5c2b-4858-9914-68580d5ba08e)

---

## Step 2 — Configure the Question Type

Set the type to **Multi File** and select **NodeJS** as the template, then click **Setup Initial Files**.

![Multi File NodeJS setup](https://github.com/user-attachments/assets/016234da-293e-40c0-9f85-4ea619baaed6)

> A new tab will open showing the candidate's view of the question.

---

## Step 3 — Add Project Files

Drag and drop your project folder (e.g. `vitejs-vite-gvqtkjas`) directly into the VS Code panel. The files will be automatically imported into the workspace.

![Files imported into VS Code](https://github.com/user-attachments/assets/12d2a97d-25f1-4169-9087-7e93b12499a4)

### Enable Live Preview

To enable the live preview pane, add the following two files to your project:

**`.vscode/settings.json`**
```json
{
  "remote.portsAttributes": {
    "5173": {
      "onAutoForward": "openPreview"
    }
  }
}
```

**`vite.config.ts`**
```ts
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

> **Note:** The `PROXY_BASE` port (`5173`) matches Vite's default dev server port. Update this value in both files if your project runs on a different port.

### Install Dependencies

Before running the project, install dependencies:

```bash
npm i
```

---

## Step 4 — Run the Project

Start the dev server:

```bash
npm run dev
```

The preview will automatically open in the right pane of VS Code. Candidates can rearrange the layout to their preference.

![Live preview in VS Code](https://github.com/user-attachments/assets/e1ca70f4-874c-4687-bbb0-f36640064c7f)

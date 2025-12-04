# Contributing to Awesome Motia Plugins

Thank you for your interest in contributing to the Awesome Motia Plugins list! We welcome contributions from the community, whether you're adding an existing plugin to the list or creating a brand new plugin to share with the ecosystem.

## Table of Contents

- [How to Add a Plugin to the List](#how-to-add-a-plugin-to-the-list)
- [Guidelines for Listed Plugins](#guidelines-for-listed-plugins)
- [Creating a New Plugin](#creating-a-new-plugin)
  - [Quick Start with CLI](#quick-start-with-cli)
  - [Manual Setup](#manual-setup)
  - [Plugin Architecture](#plugin-architecture)
  - [Testing Your Plugin](#testing-your-plugin)
- [Publishing to NPM](#publishing-to-npm)
- [Plugin Best Practices](#plugin-best-practices)
- [Advanced Features](#advanced-features)
- [Examples and References](#examples-and-references)

---

## How to Add a Plugin to the List

Already created a plugin? Here's how to add it to the awesome list:

1. **Search**: Ensure your plugin isn't already listed in the README.
2. **Fork**: Fork this repository.
3. **Add**: Add your plugin to the `README.md` file in the appropriate category.
   - Format: `- [Plugin Name](Link) - Description.`
   - Keep descriptions concise and clear (1-2 sentences).
4. **Pull Request**: Submit a Pull Request with a clear title (e.g., "Add plugin-analytics").

### Submission Format Example

```markdown
### @yourusername/motia-plugin-analytics

[![npm version](https://badge.fury.io/js/%40yourusername%2Fmotia-plugin-analytics.svg)](https://www.npmjs.com/package/@yourusername/motia-plugin-analytics)

Performance analytics and monitoring for Motia workbench.

- **Author:** Your Name
- **NPM:** [@yourusername/motia-plugin-analytics](https://www.npmjs.com/package/@yourusername/motia-plugin-analytics)
- **GitHub:** [yourusername/motia-plugin-analytics](https://github.com/yourusername/motia-plugin-analytics)

**Installation:**
```bash
pnpm add @yourusername/motia-plugin-analytics
```

**Usage:**
```typescript
import analyticsPlugin from '@yourusername/motia-plugin-analytics/plugin'

export default {
  plugins: [analyticsPlugin],
}
```
```

---

## Guidelines for Listed Plugins

To maintain quality and consistency, plugins listed here should meet the following criteria:

- ✅ **Useful and Relevant**: Plugins should provide meaningful functionality for the Motia ecosystem
- ✅ **Valid Links**: All links should be accessible and point to active resources
- ✅ **Documentation**: Include clear installation and usage instructions
- ✅ **Open Source Preferred**: If your plugin is open source, please provide a link to the repository
- ✅ **NPM Package Recommended**: Publishing to NPM makes installation easier (though not strictly required)
- ✅ **Working**: Plugin should be tested and functional with the latest Motia version
- ✅ **Maintained**: Plugin should be actively maintained or clearly marked as archived

---

## Creating a New Plugin

Want to create a new plugin for Motia? This guide will walk you through the entire process, from creation to publication.

### Overview

Motia plugins extend the workbench functionality with custom visualizations, integrations, and features. You can create plugins as **independent NPM packages** without modifying the core Motia repository.

**Benefits:**
- ✅ Maintain your plugin independently with your own versioning
- ✅ Share functionality across multiple projects
- ✅ Contribute to the community via this awesome-plugins list
- ✅ Publish to NPM for easy distribution

### Quick Start with CLI

The fastest way to create a plugin:

```bash
# Create a new plugin project
pnpm dlx motia create --plugin my-awesome-plugin

# Navigate to the plugin directory
cd my-awesome-plugin

# Install dependencies
pnpm install

# Build the plugin
pnpm run build
```

This generates a complete plugin project with:
- ✅ TypeScript and React configuration
- ✅ Build setup (tsdown)
- ✅ Example workbench UI component
- ✅ All required dependencies

### Manual Setup

If you want to understand or customize the plugin structure:

#### 1. Create Project Structure

```
my-awesome-plugin/
├── src/
│   ├── components/
│   │   └── my-feature-page.tsx    # Main UI component
│   ├── index.ts                    # Package entry point
│   ├── plugin.ts                   # Plugin definition
│   └── styles.css                  # Tailwind styles
├── package.json
├── tsconfig.json
├── tsdown.config.ts               # Build configuration
├── README.md
└── LICENSE
```

#### 2. Configure `package.json`

```json
{
  "name": "@yourusername/motia-plugin-yourfeature",
  "version": "1.0.0",
  "description": "Your awesome Motia plugin",
  "type": "module",
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "keywords": ["motia", "motia-plugin", "yourfeature"],
  "author": "Your Name",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/yourusername/motia-plugin-yourfeature"
  },
  "exports": {
    ".": {
      "development": "./src/index.ts",
      "default": "./dist/index.js"
    },
    "./plugin": {
      "development": "./src/plugin.ts",
      "default": "./dist/plugin.js"
    },
    "./package.json": "./package.json"
  },
  "files": ["dist"],
  "scripts": {
    "build": "tsdown",
    "dev": "tsdown --watch",
    "clean": "rm -rf dist"
  },
  "peerDependencies": {
    "@motiadev/core": "^0.14.0",
    "@motiadev/ui": "^0.14.0",
    "react": "^19.0.0"
  },
  "devDependencies": {
    "@bosh-code/tsdown-plugin-tailwindcss": "^1.0.1",
    "@rollup/plugin-babel": "^6.1.0",
    "@tailwindcss/postcss": "^4.1.17",
    "@types/node": "^24.10.1",
    "@types/react": "^19.2.7",
    "babel-plugin-react-compiler": "^1.0.0",
    "tailwindcss": "^4.1.17",
    "tsdown": "^0.16.8",
    "typescript": "^5.9.3"
  },
  "publishConfig": {
    "exports": {
      ".": "./dist/index.js",
      "./plugin": "./dist/plugin.js",
      "./package.json": "./package.json"
    }
  }
}
```

#### 3. Define Plugin Entry (`src/plugin.ts`)

```typescript
import type { MotiaPlugin, MotiaPluginContext } from '@motiadev/core'

export default function plugin(motia: MotiaPluginContext): MotiaPlugin {
  // Optional: Register custom API endpoints
  motia.registerApi(
    {
      method: 'GET',
      path: '/__motia/my-plugin/data',
    },
    async (req, ctx) => {
      return {
        status: 200,
        body: { message: 'Hello from my plugin!' },
      }
    }
  )

  return {
    workbench: [
      {
        packageName: '@yourusername/motia-plugin-yourfeature',
        cssImports: ['@yourusername/motia-plugin-yourfeature/dist/index.css'],
        label: 'Your Feature',
        position: 'bottom', // or 'top'
        componentName: 'MyFeaturePage',
        labelIcon: 'sparkles', // lucide-react icon name
      },
    ],
  }
}
```

**Plugin Context API:**
- `motia.printer` - Logging utilities
- `motia.state` - State management adapter
- `motia.lockedData` - Thread-safe data access
- `motia.tracerFactory` - Tracing functionality
- `motia.eventAdapter` - Event system adapter
- `motia.registerApi()` - Register custom API endpoints

#### 4. Create UI Component (`src/components/my-feature-page.tsx`)

```typescript
import { Badge, Button, Card } from '@motiadev/ui'
import { Sparkles } from 'lucide-react'
import type React from 'react'
import { useEffect, useState } from 'react'

export const MyFeaturePage: React.FC = () => {
  const [data, setData] = useState<any>(null)
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    // Fetch data from your custom API
    fetch('/__motia/my-plugin/data')
      .then(res => res.json())
      .then(data => {
        setData(data)
        setLoading(false)
      })
      .catch(err => {
        console.error('Failed to fetch data:', err)
        setLoading(false)
      })
  }, [])

  return (
    <div className="h-full w-full p-6 overflow-auto">
      <div className="max-w-4xl mx-auto space-y-6">
        <div className="flex items-center gap-3">
          <Sparkles className="w-8 h-8 text-primary" />
          <h1 className="text-3xl font-bold">Your Feature</h1>
          <Badge variant="info">v1.0.0</Badge>
        </div>

        <Card className="p-6">
          <h2 className="text-xl font-semibold mb-4">Welcome!</h2>
          {loading ? (
            <p className="text-muted-foreground">Loading...</p>
          ) : (
            <p className="text-muted-foreground">
              {data?.message || 'No data available'}
            </p>
          )}
        </Card>
      </div>
    </div>
  )
}
```

**Styling Guidelines:**
- ✅ Use Tailwind utility classes only
- ✅ Use Motia's UI component library (`@motiadev/ui`)
- ✅ Avoid arbitrary values - use design system scales
- ✅ Keep components responsive
- ✅ Follow Motia's design patterns

#### 5. Export Components (`src/index.ts`)

```typescript
export { MyFeaturePage } from './components/my-feature-page'
```

#### 6. Add Styles (`src/styles.css`)

```css
@import 'tailwindcss';
```

#### 7. Configure Build (`tsdown.config.ts`)

```typescript
import { tailwindPlugin } from '@bosh-code/tsdown-plugin-tailwindcss'
import pluginBabel from '@rollup/plugin-babel'
import { defineConfig } from 'tsdown'

export default defineConfig([
  // Main JavaScript/TypeScript build
  {
    entry: {
      index: './src/index.ts',
      plugin: './src/plugin.ts',
    },
    format: 'esm',
    platform: 'browser',
    external: [/^react($|\/)/, 'react/jsx-runtime'],
    dts: {
      build: true,
    },
    exports: {
      devExports: 'development',
    },
    clean: true,
    outDir: 'dist',
    plugins: [
      pluginBabel({
        babelHelpers: 'bundled',
        parserOpts: {
          sourceType: 'module',
          plugins: ['jsx', 'typescript'],
        },
        plugins: ['babel-plugin-react-compiler'],
        extensions: ['.js', '.jsx', '.ts', '.tsx'],
      }),
    ],
  },
  // Separate CSS build
  {
    entry: {
      index: './src/styles.css',
    },
    format: 'esm',
    platform: 'browser',
    outDir: 'dist',
    clean: false,
    plugins: [
      tailwindPlugin({
        minify: process.env.NODE_ENV === 'prod',
      }),
    ],
  },
])
```

#### 8. TypeScript Configuration (`tsconfig.json`)

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "jsx": "react-jsx",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "outDir": "./dist",
    "moduleResolution": "bundler",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

#### 9. Build Your Plugin

```bash
# Install dependencies
pnpm install

# Build
pnpm run build

# Watch mode for development
pnpm run dev
```

### Plugin Architecture

#### Core Types

**MotiaPlugin**
```typescript
type MotiaPlugin = {
  workbench: WorkbenchPlugin[]  // Array of workbench tab configurations
  dirname?: string              // Optional plugin directory
  steps?: string[]              // Optional custom steps
}
```

**WorkbenchPlugin**
```typescript
type WorkbenchPlugin = {
  packageName: string           // Package registry name
  componentName?: string        // React component name to render
  label?: string                // Tab label text
  labelIcon?: string            // Icon name from lucide-react
  position?: 'bottom' | 'top'   // Tab position in workbench
  cssImports?: string[]         // CSS files to import
  props?: Record<string, any>   // Props passed to component
}
```

**MotiaPluginContext**
```typescript
type MotiaPluginContext = {
  printer: Printer              // Logging utilities
  state: StateAdapter           // State management
  lockedData: LockedData        // Thread-safe data access
  tracerFactory: TracerFactory  // Tracing functionality
  eventAdapter: EventAdapter    // Event system
  registerApi: (...)            // Register custom API endpoints
}
```

### Testing Your Plugin

Before publishing, test your plugin in a Motia project:

#### Method 1: NPM Link (Recommended)

```bash
# In your plugin directory
pnpm link --global

# In your Motia project
pnpm link --global @yourusername/motia-plugin-yourfeature
```

#### Method 2: Local Path

In your Motia project's `package.json`:

```json
{
  "dependencies": {
    "@yourusername/motia-plugin-yourfeature": "file:../path/to/your/plugin"
  }
}
```

#### Use in `motia.config.ts`

```typescript
import yourPlugin from '@yourusername/motia-plugin-yourfeature/plugin'

export default {
  plugins: [yourPlugin],
}
```

#### Start Motia and Test

```bash
# In your Motia project
pnpm run dev

# Open workbench and verify your plugin appears
# Test all functionality
# Check browser console for errors
```

---

## Publishing to NPM

Once your plugin is tested and ready, publish it to NPM for easy distribution.

### Prerequisites

1. **Create NPM account:** [npmjs.com/signup](https://www.npmjs.com/signup)
2. **Login to NPM CLI:**
   ```bash
   npm login
   ```

### Publishing Steps

1. **Build your plugin:**
   ```bash
   pnpm run build
   ```

2. **Verify package contents:**
   ```bash
   npm pack --dry-run
   ```
   
   This shows what will be published. Ensure only `dist/` and necessary files are included.

3. **Publish to NPM:**
   ```bash
   npm publish --access public
   ```

   > **Note:** For scoped packages (`@yourusername/...`), you need `--access public` for the first publish

4. **Verify publication:**
   ```bash
   npm view @yourusername/motia-plugin-yourfeature
   ```

### Version Updates

When releasing updates:

```bash
# Patch release (1.0.0 → 1.0.1) - Bug fixes
npm version patch

# Minor release (1.0.1 → 1.1.0) - New features, backward compatible
npm version minor

# Major release (1.1.0 → 2.0.0) - Breaking changes
npm version major

# Rebuild and publish
pnpm run build
npm publish
```

### Publishing Checklist

Before publishing, ensure:

- ✅ README.md is comprehensive
- ✅ package.json metadata is complete (description, keywords, author, repository)
- ✅ LICENSE file is included
- ✅ Plugin builds without errors
- ✅ Plugin has been tested in a real Motia project
- ✅ Version number follows semantic versioning
- ✅ CHANGELOG.md documents changes (recommended)

---

## Plugin Best Practices

### Naming Convention

- ✅ Use `@yourusername/motia-plugin-featurename` (scoped, recommended)
- ✅ Or `motia-plugin-featurename` (unscoped)
- ✅ Include "motia" and "plugin" for discoverability
- ✅ Use lowercase and hyphens, no spaces

**Good Examples:**
- `@acme/motia-plugin-analytics`
- `@mycompany/motia-plugin-performance`
- `motia-plugin-custom-charts`

**Bad Examples:**
- `my-plugin` (not discoverable)
- `MotiaPluginAnalytics` (should be lowercase)
- `@acme/analytics` (missing motia/plugin context)

### Documentation

Your plugin repository should include:

- ✅ **Comprehensive README.md** with:
  - Clear description of what the plugin does
  - Installation instructions
  - Usage examples with code snippets
  - Configuration options
  - Screenshots or GIFs showing the plugin in action
  - API endpoint documentation (if applicable)
  - Environment variables (if any)
  - Troubleshooting section
  
- ✅ **LICENSE file** (MIT, Apache 2.0, etc.)
- ✅ **CHANGELOG.md** documenting version changes
- ✅ **Contributing guidelines** (if accepting contributions)

### Code Quality

- ✅ Use TypeScript for type safety
- ✅ Follow Motia's design system
- ✅ Keep components simple and performant
- ✅ Add proper error handling
- ✅ Use meaningful variable and function names
- ✅ Add comments for complex logic
- ✅ Handle loading and error states in UI

### Versioning

- ✅ Follow [Semantic Versioning](https://semver.org/)
  - MAJOR: Breaking changes
  - MINOR: New features, backward compatible
  - PATCH: Bug fixes
- ✅ Test with latest Motia version before releasing
- ✅ Document breaking changes clearly

### Dependencies

- ✅ Use peer dependencies for Motia packages
- ✅ Minimize additional dependencies
- ✅ Pin versions or use caret ranges (`^`)
- ✅ Document required peer dependencies in README
- ✅ Keep dependencies up to date

### Performance

- ✅ Lazy load heavy components when possible
- ✅ Debounce API calls and user inputs
- ✅ Use React.memo for expensive components
- ✅ Optimize images and assets
- ✅ Avoid unnecessary re-renders

---

## Advanced Features

### Including Custom Steps

Plugins can include their own steps (API routes, event handlers, cron jobs):

```typescript
import path from 'node:path'
import type { MotiaPlugin, MotiaPluginContext } from '@motiadev/core'

export default function plugin(motia: MotiaPluginContext): MotiaPlugin {
  return {
    dirname: path.join(__dirname, 'steps'),
    steps: ['**/*.step.ts', '**/*_step.py'],
    workbench: [
      {
        packageName: '@yourusername/motia-plugin-yourfeature',
        // ... other config
      },
    ],
  }
}
```

Plugin steps are discovered recursively following the same rules as regular steps. This allows you to:
- Add custom API endpoints as steps
- Include background event handlers
- Add scheduled cron jobs
- Provide example workflows

### Using State Management

```typescript
export default function plugin(motia: MotiaPluginContext): MotiaPlugin {
  motia.registerApi(
    { method: 'GET', path: '/__motia/my-plugin/state' },
    async (req, ctx) => {
      const value = await motia.state.get('my-key')
      return { status: 200, body: { value } }
    }
  )
  
  motia.registerApi(
    { method: 'POST', path: '/__motia/my-plugin/state' },
    async (req, ctx) => {
      await motia.state.set('my-key', req.body.value)
      return { status: 200, body: { success: true } }
    }
  )
  
  return { workbench: [/* ... */] }
}
```

### Accessing Event System

```typescript
export default function plugin(motia: MotiaPluginContext): MotiaPlugin {
  // Emit events
  motia.registerApi(
    { method: 'POST', path: '/__motia/my-plugin/notify' },
    async (req, ctx) => {
      await motia.eventAdapter.emit('my-plugin-event', req.body)
      return { status: 200, body: { sent: true } }
    }
  )
  
  return { workbench: [/* ... */] }
}
```

### Environment Configuration

If your plugin needs configuration, use environment variables:

```typescript
export default function plugin(motia: MotiaPluginContext): MotiaPlugin {
  const apiKey = process.env.MY_PLUGIN_API_KEY
  const endpoint = process.env.MY_PLUGIN_ENDPOINT || 'https://default.api'
  
  if (!apiKey) {
    motia.printer.printPluginWarn('MY_PLUGIN_API_KEY not set')
  }
  
  // ... use config
}
```

Document required environment variables in your README:

```markdown
## Configuration

Set the following environment variables:

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `MY_PLUGIN_API_KEY` | Yes | - | API key for service |
| `MY_PLUGIN_ENDPOINT` | No | `https://default.api` | API endpoint URL |
```

### Adding Custom Dependencies

Add any dependencies your plugin needs:

```bash
pnpm add bullmq ioredis date-fns @tanstack/react-query
```

Update `package.json`:

```json
{
  "dependencies": {
    "bullmq": "^5.63.0",
    "ioredis": "^5.8.2",
    "date-fns": "^3.6.0",
    "@tanstack/react-query": "^5.62.0"
  }
}
```

---

## Examples and References

### Official Plugins Reference

Study these official plugins for inspiration:

| Plugin | Purpose | Key Features | Source |
|--------|---------|--------------|--------|
| **[plugin-example](https://github.com/motiadev/motia/tree/main/plugins/plugin-example)** | Minimal template | Basic structure, simple UI | [Code](https://github.com/motiadev/motia/tree/main/plugins/plugin-example) |
| **[plugin-logs](https://github.com/motiadev/motia/tree/main/plugins/plugin-logs)** | Log viewer | Real-time streaming, filtering, search | [Code](https://github.com/motiadev/motia/tree/main/plugins/plugin-logs) |
| **[plugin-bullmq](https://github.com/motiadev/motia/tree/main/plugins/plugin-bullmq)** | Queue management | Custom APIs, state management, complex UI | [Code](https://github.com/motiadev/motia/tree/main/plugins/plugin-bullmq) |
| **[plugin-endpoint](https://github.com/motiadev/motia/tree/main/plugins/plugin-endpoint)** | API testing | Request handling, forms, response visualization | [Code](https://github.com/motiadev/motia/tree/main/plugins/plugin-endpoint) |
| **[plugin-observability](https://github.com/motiadev/motia/tree/main/plugins/plugin-observability)** | Performance tracing | Trace visualization, metrics, distributed tracing | [Code](https://github.com/motiadev/motia/tree/main/plugins/plugin-observability) |
| **[plugin-states](https://github.com/motiadev/motia/tree/main/plugins/plugin-states)** | State inspector | State management, history tracking, JSON viewer | [Code](https://github.com/motiadev/motia/tree/main/plugins/plugin-states) |

### Official Resources

- **[Motia Plugin Development Guide](https://github.com/MotiaDev/motia/blob/main/packages/docs/content/docs/development-guide/plugins.mdx)** - Official comprehensive documentation
- **[Motia Documentation](https://motia.dev/docs)** - Full platform documentation
- **[Motia Repository](https://github.com/MotiaDev/motia)** - Main repository with examples
- **[NPM Publishing Guide](https://docs.npmjs.com/packages-and-modules/contributing-packages-to-the-registry)** - Official NPM documentation

### Community

- **[Motia Discord](https://discord.gg/motia)** - Join for community support
- **[GitHub Discussions](https://github.com/MotiaDev/motia/discussions)** - Ask questions and share ideas
- **[awesome-plugins Repository](https://github.com/MotiaDev/awesome-plugins)** - This repository!

---

## Troubleshooting

### Plugin Not Showing in Workbench

**Symptoms:** Plugin doesn't appear in workbench tabs after installation

**Solutions:**
- ✅ Check plugin is imported in `motia.config.ts`
- ✅ Verify `componentName` matches your exported component exactly
- ✅ Ensure plugin is built (`pnpm run build`)
- ✅ Check browser console for errors
- ✅ Restart Motia dev server after adding plugin
- ✅ Clear browser cache and reload

### Styles Not Loading

**Symptoms:** Plugin appears but styling is broken or missing

**Solutions:**
- ✅ Verify CSS is imported in `src/index.ts`
- ✅ Check `cssImports` path in `src/plugin.ts` points to correct file
- ✅ Ensure TailwindCSS is properly configured in `tsdown.config.ts`
- ✅ Rebuild plugin: `pnpm run build`
- ✅ Check that `dist/index.css` exists after build
- ✅ Verify PostCSS configuration

### Type Errors

**Symptoms:** TypeScript errors when importing or using plugin

**Solutions:**
- ✅ List `@motiadev/core` and `@motiadev/ui` in `peerDependencies`, not `dependencies`
- ✅ Run `pnpm install` to resolve peer dependencies
- ✅ Set `declaration: true` in `tsconfig.json`
- ✅ Check TypeScript version compatibility (use ^5.9.0)
- ✅ Rebuild with `pnpm run build` to regenerate type definitions
- ✅ Ensure `dist/` contains `.d.ts` files

### Build Errors

**Symptoms:** `pnpm run build` fails

**Solutions:**
- ✅ Check all imports are correct
- ✅ Ensure all dependencies are installed
- ✅ Verify `tsdown.config.ts` configuration
- ✅ Check for TypeScript errors: `tsc --noEmit`
- ✅ Clear build cache: `pnpm run clean && pnpm run build`
- ✅ Update dependencies to compatible versions

### Plugin Works Locally But Not After Publishing

**Symptoms:** Plugin works with `pnpm link` but fails when installed from NPM

**Solutions:**
- ✅ Check `files` field in `package.json` includes `dist/`
- ✅ Verify `exports` in `publishConfig` are correct
- ✅ Test with `npm pack` and inspect the tarball before publishing
- ✅ Ensure peer dependencies match consumer's versions
- ✅ Check external dependencies are correctly configured in build
- ✅ Rebuild before publishing: `pnpm run clean && pnpm run build`

---

## Need Help?

If you're stuck or have questions:

1. **Check Documentation:** Review the [official plugin guide](https://github.com/MotiaDev/motia/blob/main/packages/docs/content/docs/development-guide/plugins.mdx)
2. **Review Examples:** Look at [official plugin implementations](https://github.com/motiadev/motia/tree/main/plugins)
3. **Search Issues:** Check if someone already asked in [GitHub Issues](https://github.com/MotiaDev/motia/issues)
4. **Ask Community:** Join [Motia Discord](https://discord.gg/motia) for real-time help
5. **Open Discussion:** Start a [GitHub Discussion](https://github.com/MotiaDev/motia/discussions)

---

## Summary

Creating and contributing a Motia plugin:

1. ✅ **Create:** `pnpm dlx motia create --plugin my-plugin`
2. ✅ **Develop:** Build your plugin with React, TypeScript, and Motia UI
3. ✅ **Test:** Link locally and test thoroughly in a Motia project
4. ✅ **Document:** Write comprehensive README with examples and screenshots
5. ✅ **Publish:** `npm publish --access public`
6. ✅ **Share:** Submit PR to [awesome-plugins](https://github.com/MotiaDev/awesome-plugins)

**Let's build an amazing ecosystem together! 🚀**

---

## License

This repository and its contents are licensed under the [MIT License](LICENSE). See the LICENSE file for details.
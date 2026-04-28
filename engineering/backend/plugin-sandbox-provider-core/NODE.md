---
title: "Plugin-Backed Sandbox Provider Core"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
soft_links: [adapters/capability-flags/NODE.md, engineering/backend/NODE.md, engineering/frontend/NODE.md, plugins/runtime/NODE.md, plugins/sdk/NODE.md]
---

# Plugin-Backed Sandbox Provider Core

The server's sandbox provider machinery (`server/src/services/sandbox-provider-runtime.ts`, `environment-config.ts`, `environment-runtime.ts`, `plugin-environment-driver.ts`) is generalized so that sandbox providers can be supplied entirely by plugins, with no built-in server-side provider registration. The only built-in `SandboxEnvironmentProvider` key is `"fake"`, and it is now reported as `unsupported` in adapter capability listings — real providers come from installed plugins that declare `environmentDrivers` of `kind: "sandbox_provider"` in their manifest.

## Plugin-declared config schemas drive the UI and validation

Each plugin sandbox provider declares a JSON Schema (`configSchema`) on its driver declaration. The schema flows through `EnvironmentProviderCapability.configSchema` into the `/environments` route response and is consumed by `AgentConfigForm` / `CompanySettings` to render a `JsonSchemaForm` instead of hard-coded fields. `validatePluginSandboxProviderConfig` normalizes and validates incoming config against the plugin schema before it is persisted, and `resolvePluginSandboxProviderDriverByKey` looks up the owning plugin (optionally without requiring the worker to be running, e.g. for probe/lease-metadata flows).

## Generic stored shape and envelope stripping

Persisted sandbox configs use a generic plugin-backed shape: `{ provider, ...schemaFields, reuseLease }`. `normalizeEnvironmentConfig` and `parseEnvironmentDriverConfig` accept arbitrary schema-driven fields (e.g. `template`, `apiKey`, `timeoutMs`) for any plugin provider. Before invoking the plugin worker, `stripSandboxProviderEnvelope` removes the `provider` key so the worker receives only the schema-defined config it expects.

## Secret-ref handling via shared JSON-Schema walker

Secret-ref logic is extracted into `server/src/services/json-schema-secret-refs.ts` (`collectSecretRefPaths`, `isUuidSecretRef`, `readConfigValueAtPath`, `writeConfigValueAtPath`). It walks `properties` plus `allOf`/`anyOf`/`oneOf` composition keywords to find every `format: "secret-ref"` field. Both `plugin-secrets-handler` and the sandbox runtime reuse it: lease metadata is sanitized via `stripSecretRefValuesFromPluginLeaseMetadata` so persisted UUID secret references are never echoed back to plugin workers as raw secret values.

## Lease reuse for plugin providers

`findReusableSandboxProviderLeaseId` no longer requires only `reuseLease === true` for unknown (plugin-backed) providers. It now matches reusable leases by deep-equal of all persisted config fields (excluding `provider` and `reuseLease`) against lease metadata, so plugin providers with multiple templates or distinct API keys correctly select the matching prior lease instead of the first reusable one.

## Catching Errors Before They Run: A Guide to Validating GraphQL with ESLint

In a modern full-stack environment, your [GraphQL schema](https://graphql.org/learn/schema/) is the "source of truth." However, as projects grow, it’s easy for frontend queries to drift away from that truth. You might rename a field in the backend, only to find your frontend crashing because a UI component is still requesting the old name.

This is where **[ESLint](https://eslint.org/)** comes in. By integrating GraphQL validation into your linting workflow, you can catch syntax errors, deprecated fields, and schema mismatches directly in your code editor—long before you hit "Save" or trigger a [CI/CD pipeline](https://www.ibm.com/think/topics/ci-cd-pipeline).
<img width="1024" height="1024" alt="Whisk_b0e1863efd7948a86c24bbbbb18ffff4dr" src="https://github.com/user-attachments/assets/c387472c-6e90-4bab-8cf5-368506c098c2" />


---

### Why Lint Your GraphQL Queries?

While [TypeScript](https://www.typescriptlang.org/) provides excellent type safety, it often relies on generated hooks. Linting provides an immediate feedback loop:
1.  **[Syntax Errors](https://en.wikipedia.org/wiki/Syntax_error):** Catch missing brackets or commas instantly.
2.  **[Schema Validation](https://validator.schema.org/):** Ensure you aren't requesting fields that don't exist.
3.  **[Deprecation Warnings](https://craftcms.com/glossary/deprecation-warning):** Get notified when you’re using a field the backend team has marked as `@deprecated`.
4.  **Consistency:** Enforce naming conventions (e.g., PascalCase for fragments).

---

### Step 1: Install the Essentials

The industry standard for this task is `@graphql-eslint/eslint-plugin`. It treats GraphQL as a first-class citizen within the ESLint ecosystem.

Run the following command in your terminal:

```bash
npm install --save-dev eslint @graphql-eslint/eslint-plugin graphql
```

---

### Step 2: Configure ESLint

You need to tell ESLint how to handle `.graphql` files and how to find your schema. Create or update your `.eslintrc.json` (or the flat config equivalent):

```json
{
  "overrides": [
    {
      "files": ["*.graphql"],
      "parser": "@graphql-eslint/eslint-plugin",
      "plugins": ["@graphql-eslint"],
      "rules": {
        "@graphql-eslint/no-anonymous-operations": "error",
        "@graphql-eslint/no-duplicate-fields": "error",
        "@graphql-eslint/fields-on-correct-type": "error"
      },
      "parserOptions": {
        "schema": "./path/to/schema.graphql" 
      }
    },
    {
      "files": ["*.ts", "*.tsx"],
      "processor": "@graphql-eslint/graphql",
      "rules": {
        "@graphql-eslint/known-type-names": "error"
      }
    }
  ]
}
```

> **Pro Tip:** If your schema is on a remote server or [node.js hosting](https://www.vpsmalaysia.com.my/nodejs-hosting/), you can point the `schema` option to a URL, though a local `.json` or `.graphql` file is faster and more reliable for local development.

---

### Step 3: Validating Template Literals

Most developers write GraphQL inside JavaScript or TypeScript files using the `gql` tag. To lint these, the plugin uses a **processor**. It "plucks" the GraphQL strings out of your `.ts` files, validates them against your schema, and reports errors back to the original file.



---

### Step 4: Advanced Rules to Consider

Once the basics are running, you can enforce stricter standards:

* **`@graphql-eslint/no-deprecated`**: Highlights fields marked with the `@deprecated` directive in your schema.
* **`@graphql-eslint/naming-convention`**: Ensures all your Operations are named correctly (e.g., `GetUserInfo` instead of `user_info`).
* **`@graphql-eslint/executable-definitions`**: Ensures your frontend files only contain queries/mutations, not type definitions that belong in the backend.

---

### Summary

Validating GraphQL with ESLint turns your editor into a powerful guardrail. It closes the gap between the frontend and backend, ensuring that your queries are always in sync with your API's capabilities.

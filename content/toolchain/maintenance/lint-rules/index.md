---
title: "Custom Detekt Rules"
params:
  menuPre: "<i class=\"fa-solid fa-spell-check\"></i> "
weight: 3
---

This guide explains how to maintain, test, and expand the custom Detekt static analysis rules bundled with the MineKot toolchain.

## Static analysis architecture

The static analysis module `:plugin:lint-rules` compiles into a standalone rule set provider. This provider registers with the Detekt compiler plugin to scan Kotlin abstract syntax trees (AST) during compile-time checks.

The default configuration file is located in `plugin/toolchain/src/main/resources/config/detekt/minekot.yml`. During project initialization via `./gradlew mineKotInitializeProject`, the plugin copies this file to the consumer project's `config/detekt/minekot.yml` directory.

## Bundled rules

We define several custom lint rules inside `:plugin:lint-rules` to enforce MineKot standards:

- **String template braces** – Analyzes string expressions and triggers warnings for variable interpolations that do not use enclosing braces (for example, `$name` is flagged; `${name}` is allowed).
- **Try-catch rejection** - Walks AST expressions and flags any traditional `try-catch` blocks. The rule prompts the developer to use functional Alternatives like `runCatching` or specific parsing `Result` wrappers.
- **Legacy color rejection** – Inspects string literals to ensure they do not contain legacy Bukkit color format characters (like `§` or `&` prefix symbols), enforcing MiniMessage compliance.
- **Coroutine preference** – Detects usage of standard Java thread operations, executors, or timers, recommending coroutines (`kotlinx-coroutines`) instead.
- **Public KDoc validation** – Ensures that all public classes, functions, and parameters include complete, descriptive KDoc comments.

## Creating a new lint rule

To add a new static analysis rule, follow this workflow:

1. Create a new Kotlin class in the `:plugin:lint-rules` module under the appropriate package directory.
2. Extend `io.gitlab.arturbosch.detekt.api.Rule` and override `issue` properties:
   ```kotlin
   import io.gitlab.arturbosch.detekt.api.CodeSmell
   import io.gitlab.arturbosch.detekt.api.Config
   import io.gitlab.arturbosch.detekt.api.Debt
   import io.gitlab.arturbosch.detekt.api.Entity
   import io.gitlab.arturbosch.detekt.api.Issue
   import io.gitlab.arturbosch.detekt.api.Rule
   import io.gitlab.arturbosch.detekt.api.Severity
   import org.jetbrains.kotlin.psi.KtTryExpression

   class RejectTryCatchRule(config: Config) : Rule(config) {
       override val issue = Issue(
           id = "RejectTryCatchRule",
           severity = Severity.Style,
           description = "Traditional try-catch blocks are forbidden. Use runCatching.",
           debt = Debt.FIVE_MINS
       )
   }
   ```
3. Implement visitor methods to inspect targeted AST components (for example, override `visitTryExpression` or `visitNamedFunction`):
   ```kotlin
   override fun visitTryExpression(expression: KtTryExpression) {
       super.visitTryExpression(expression)
       report(CodeSmell(issue, Entity.from(expression), "Try-catch blocks are not allowed."))
   }
   ```
4. Register the rule class inside your rule set provider class (which implements `RuleSetProvider`):
   ```kotlin
   import io.gitlab.arturbosch.detekt.api.RuleSet
   import io.gitlab.arturbosch.detekt.api.RuleSetProvider

   class MineKotRuleSetProvider : RuleSetProvider {
       override val ruleSetId: String = "minekot"
       override fun instance(config: Config): RuleSet = RuleSet(
           ruleSetId,
           listOf(
               RejectTryCatchRule(config)
           )
       )
   }
   ```
5. Register the rule set provider by adding its fully qualified name to the `resources/META-INF/services/io.gitlab.arturbosch.detekt.api.RuleSetProvider` file.

## Testing custom rules

You must write unit tests for every new lint rule. The Detekt API includes a testing library that makes it easy to parse Kotlin code snippets programmatically and assert on code smells:

```kotlin
import io.gitlab.arturbosch.detekt.test.compileAndLint
import org.junit.jupiter.api.Assertions.assertEquals
import org.junit.jupiter.api.Test

class RejectTryCatchRuleTest {

    @Test
    fun testTryCatchIsFlagged() {
        val rule = RejectTryCatchRule(Config.empty)
        val code = """
            fun test() {
                try {
                    doSomething()
                } catch (e: Exception) {
                    handle(e)
                }
            }
        """.trimIndent()
        val findings = rule.compileAndLint(code)
        assertEquals(1, findings.size)
    }
}
```

Run `./gradlew :plugin:lint-rules:test` to verify your rules pass validation checks.

## Updating configurations

After implementing a new rule, you must add its configuration keys to the base template file:

1. Edit `plugin/toolchain/src/main/resources/config/detekt/minekot.yml`.
2. Add your rule configurations inside the `minekot` rule set block:
   ```yaml
   minekot:
     active: true
     RejectTryCatchRule:
       active: true
       autoCorrect: false
   ```
3. Run `./gradlew writeMineKotCodestyle` to output the updated rules config to local active configurations.
